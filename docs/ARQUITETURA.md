# Arquitetura do Gestão Platform

## Visão

SaaS de gestão **multiempresa**, **modular** e **multissegmento** (alimentação, varejo, material de
construção e serviços). Um único núcleo de código atende todos os segmentos; o que muda por empresa
são os **módulos habilitados** (plano) e a **identidade** da interface (segmento).

## Decisões

| Decisão                    | Escolha                                                                      | Por quê                                                                                   |
| -------------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Estilo                     | Monólito modular                                                             | Um deploy, módulos com fronteiras claras. Microsserviços só se houver dor real.           |
| Identidade do produto      | **Modelo híbrido**                                                           | Mesmo núcleo; vocabulário, navegação e cor adaptados ao segmento (`src/app/segments.ts`). |
| Backend                    | Supabase (PostgreSQL + Auth + Realtime + Storage)                            | Tudo grátis no início; o banco é PostgreSQL padrão.                                       |
| Regras de negócio críticas | Funções SQL (`security definer`)                                             | Preço, estoque, financeiro e status não dependem do navegador.                            |
| Isolamento entre empresas  | RLS no PostgreSQL                                                            | Mesmo que alguém burle a interface, o banco recusa.                                       |
| Frontend                   | React 19 + TypeScript + Vite + Tailwind v4 + componentes no padrão shadcn/ui | Tipagem, build rápido, componentes acessíveis no próprio código.                          |
| Dados no frontend          | TanStack Query + camada `services/`                                          | Cache, revalidação e um único ponto de acesso ao Supabase por módulo.                     |
| Hospedagem                 | Cloudflare Pages                                                             | Grátis, CDN global, banda ilimitada para sites estáticos.                                 |

## Camadas

```
Navegador ─ React (páginas, componentes)            src/modules/*/pages, src/components
          ─ Serviços por módulo (acesso a dados)    src/modules/*/services
          ─ Cliente Supabase                        src/services/supabase
──────────── internet ────────────
Supabase  ─ PostgREST / RPC / Realtime / Storage
          ─ PostgreSQL: RLS + funções de negócio    supabase/migrations
```

## Multiempresa (tenancy)

- Toda tabela de negócio tem `company_id`.
- `company_members (company_id, user_id, role)` liga usuários a empresas. Um usuário pode estar em
  várias empresas com papéis diferentes.
- Funções de autorização (`can_read`, `can_write`, `has_role`, `has_module`,
  `company_is_operational`) são usadas por **todas** as políticas RLS.
- Regra padrão de escrita: membro ativo **e** papel permitido **e** licença operacional **e** módulo
  contratado.
- Triggers impedem referências cruzadas (ex.: produto de uma empresa com categoria de outra).

### Papéis

| Papel         | Pode                                                     |
| ------------- | -------------------------------------------------------- |
| owner / admin | Tudo na empresa, equipe, configurações, auditoria        |
| manager       | Cadastros, estoque, financeiro, cancelar venda concluída |
| cashier       | Vender, receber, conceder desconto, cadastrar clientes   |
| staff         | Registrar pedidos e clientes                             |
| kitchen       | Mover produção (preparo → pronto)                        |

## Licenciamento

`plans` define limites (usuários, filiais, produtos) e módulos. `companies.extra_modules` /
`disabled_modules` personalizam por cliente. Limites são aplicados por **trigger** no banco.
Plano, status e módulos só mudam por `admin_update_company_license` (superadmin). O tenant não tem
GRANT nessas colunas.

Ciclo: cadastro → `trial` (30 dias) → superadmin muda para `active` após o pagamento → `suspended`
em caso de inadimplência (dados ficam somente leitura; cardápio público sai do ar).

## Regras de negócio no banco

- **Pedido**: `create_order` / `place_public_order` recebem só IDs e quantidades. Preço, adicionais,
  taxa de entrega e total são calculados no servidor. `idempotency_key` impede pedido duplicado.
- **Status**: máquina de estados em `update_order_status`. Ao **concluir**: baixa de estoque pela
  ficha técnica, conta a receber e pontos de fidelidade. Ao **cancelar** um concluído: estorno de
  tudo.
- **Estoque**: saldo nunca é editado diretamente (sem GRANT na coluna). Só muda por
  `stock_movements`, que são imutáveis. Entradas atualizam o custo médio ponderado.
- **Financeiro**: venda gera direito (conta a receber); recebimento é outro fato
  (`pay_financial_entry`). Lançamentos pagos/cancelados são imutáveis.
- **Auditoria**: triggers em empresas, membros, produtos, pedidos, financeiro e convites gravam
  quem, quando, antes e depois.

## Acesso público (cardápio)

Visitantes (`anon`) não têm política em nenhuma tabela. Só podem chamar `get_public_menu`,
`place_public_order` (com limite por telefone) e `get_public_order` (por token aleatório).

## Estrutura de pastas

```
src/
  app/            roteador, providers (auth, empresa), layouts, segments.ts (modelo híbrido)
  components/     ui (padrão shadcn), navigation, forms, feedback
  modules/
    auth, onboarding, dashboard, sales (pedidos/PDV), kitchen, products, menu,
    inventory, customers, suppliers, finance, settings (empresa/equipe/plano), admin
      └── pages/ components/ services/ (schemas/ hooks quando necessário)
  services/supabase   cliente e helpers
  lib/            utilitários, rótulos, erros, imagem
  types/          tipos do domínio
supabase/
  migrations/     esquema versionado (aplicar em ordem)
  tests/          testes do banco (PGlite)
```

Relatórios foram incorporados ao painel e ao fluxo de caixa; empresas e usuários vivem em
`settings` (tenant) e `admin` (plataforma).

## Domínios adicionados (v1.0)

| Domínio    | Tabelas                                                                              | Funções principais                                                                                        |
| ---------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- |
| Preços     | `price_lists`, `price_list_items`, colunas de custo em `products`                    | `_product_price`, `import_products`, `import_price_list`                                                  |
| Pagamentos | `payment_method_rates`, colunas em `orders`                                          | `_payment_terms`, `set_order_payment_method`                                                              |
| Comercial  | `sales_documents`, `sales_document_items`                                            | `save_sales_document`, `set_sales_document_status`, `convert_sales_document`, `invoice_sales_document`    |
| Entregas   | `deliveries`, `delivery_items`                                                       | `create_delivery`, `set_delivery_status`                                                                  |
| WMS        | `warehouses`, `stock_locations`, `stock_balances`, `stock_transfers`, `stock_counts` | `default_warehouse`, `save/send/receive/cancel_stock_transfer`, `start_stock_count`, `finish_stock_count` |
| Fiscal     | `fiscal_settings`, `fiscal_credentials`, `fiscal_documents`                          | `_fiscal_issues`, `prepare_fiscal_document`, `request_fiscal_emission`, `cancel_fiscal_document`          |
| Marca      | colunas `brand_*` em `companies`                                                     | `set_company_brand`, `admin_unlock_brand`                                                                 |
| Painel     | —                                                                                    | `dashboard_insights`                                                                                      |

### Saldo por depósito

`stock_items.quantity` continua sendo o saldo total. O mesmo gatilho que aplica a movimentação
(`apply_stock_movement`) agora também soma em `stock_balances` por depósito e endereço. Movimentos
sem depósito vão para o **depósito padrão**, criado na primeira vez (`default_warehouse`). Assim,
vendas e compras antigas continuam funcionando sem mudança.

### Transferência entre empresas

Só quem é gestor das duas empresas cria. O envio gera `transfer_out` na origem; o recebimento gera
`transfer_in` no destino com o custo da origem (entra no custo médio). Na outra empresa, o item é
localizado pelo nome ou criado. As duas pontas enxergam a transferência (RLS por `company_id` ou
`to_company_id`).

### Fiscal

O rascunho guarda uma **foto** de emitente, destinatário, itens e totais (`jsonb`), para a nota não
mudar se o cadastro mudar depois. A numeração é consumida apenas em `request_fiscal_emission`,
dentro de uma transação com `for update`, garantindo sequência sem buracos e sem repetição.

## Próximas evoluções (estrutura já preparada)

- Filiais operando separadamente (`branch_id` já existe em pedidos).
- WMS: endereçamento (depósito → ala → rua → módulo → nível → posição) como novas tabelas ligadas a
  `stock_items`, sem alterar as existentes.
- Fiscal: NCM/CFOP/CST nos produtos e importação de XML; emissão via provedor homologado depois.
- Ordens de serviço para o segmento de serviços (novo módulo `service_orders`).
