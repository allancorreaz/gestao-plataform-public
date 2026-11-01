<div align="center">

<img src="docs/img/logo.svg" width="80" alt="" />

# Gestão Platform

### O sistema de gestão que fala a língua do pequeno negócio.

ERP **multiempresa**, **modular** e **mobile-first** para lanchonetes, lojas, depósitos de material
de construção, prestadores de serviço e comércio em geral. Um único núcleo, com vocabulário,
menu e cores de cada segmento.

[![Ver o app](https://img.shields.io/badge/▶%20ver%20o%20app-gestao--plataform.pages.dev-ff6b35?style=for-the-badge)](https://gestao-plataform.pages.dev)

![Versão](https://img.shields.io/badge/versão-1.0.0-111827)
![Testes](https://img.shields.io/badge/testes-66%20passando-0e9f6e)
![React](https://img.shields.io/badge/React-19-149eca?logo=react&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-strict-3178c6?logo=typescript&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-RLS-4169e1?logo=postgresql&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-Edge%20Functions-3ecf8e?logo=supabase&logoColor=white)
![Cloudflare](https://img.shields.io/badge/Cloudflare-Pages-f38020?logo=cloudflare&logoColor=white)
![Custo](https://img.shields.io/badge/infra-R%24%200%2Fmês-ffd84d)

<img src="docs/img/painel.png" alt="Painel do sistema" width="100%" />

</div>

> **Repositório vitrine.** Aqui estão a apresentação, a documentação e as imagens do produto.
> O código-fonte é privado. Quer ver funcionando ou conversar sobre o projeto?
> Fale comigo pelo [LinkedIn](https://www.linkedin.com/in/allandemelocorrea-45875016a) ou abra uma
> [issue](https://github.com/allancorreaz/gestao-plataform-public/issues).

---

## Sumário

- [Por que existe](#por-que-existe)
- [Destaques](#destaques)
- [Telas](#telas)
- [Tudo o que o sistema faz](#tudo-o-que-o-sistema-faz)
- [Segmentos](#segmentos)
- [Arquitetura](#arquitetura)
- [Stack](#stack)
- [Segurança](#segurança)
- [Qualidade e testes](#qualidade-e-testes)
- [Infraestrutura a custo zero](#infraestrutura-a-custo-zero)
- [Ilustrações 3D](#ilustrações-3d)
- [Planos](#planos)
- [Roteiro](#roteiro)
- [Documentação](#documentação)
- [Autor](#autor)

---

## Por que existe

Pequenos negócios costumam escolher entre **planilha + caderno** ou um **ERP caro e complicado**,
feito para indústria. O Gestão Platform é o meio do caminho: entra funcionando no primeiro dia,
com dados de exemplo do próprio segmento, e cresce junto, liberando módulos quando o negócio
precisa.

**Uma venda registrada uma vez atualiza tudo:** estoque (pela ficha técnica), financeiro (com a
taxa e o prazo da maquininha), painel, DRE e auditoria.

## Destaques

|     | Recurso                         | Em uma frase                                                                                    |
| --- | ------------------------------- | ----------------------------------------------------------------------------------------------- |
| 🍔  | **Cardápio digital com QR**     | O cliente pede pelo celular, sem baixar app, e o pedido cai na cozinha em tempo real.            |
| 💰  | **Formação de preço**           | Custo + frete + impostos → markup ou margem desejada → preço sugerido e reprecificação em massa. |
| 📄  | **Orçamento → pedido → nota**   | Fluxo comercial completo com PDF, WhatsApp, limite de desconto e reserva de estoque.             |
| 📦  | **Estoque com WMS**             | Depósitos, endereços, transferências entre empresas, inventário e rastreio de cada movimento.    |
| 🧾  | **Fiscal pronto**               | NF-e, NFC-e e NFS-e validadas antes do envio, com numeração sem buracos.                         |
| 💳  | **12x por bandeira**            | Taxa, juros por parcela e prazo de recebimento configurados por bandeira de cartão.              |
| 📊  | **Painel + DRE**                | Comparação com o período anterior, horários de pico, canais e ponto de equilíbrio.               |
| 🎨  | **A marca do cliente**          | Cada empresa escolhe suas cores; o sistema inteiro e o cardápio se adaptam.                      |
| 🧭  | **Tour guiado**                 | Cada tela ensina a si mesma com destaque passo a passo.                                          |

## Telas

| Painel                                         | Produtos com ilustrações 3D                        |
| ---------------------------------------------- | -------------------------------------------------- |
| <img src="docs/img/painel.png" alt="Painel" /> | <img src="docs/img/produtos.png" alt="Produtos" /> |
| **Formação de preço**                          | **Orçamento (material de construção)**             |
| <img src="docs/img/precos.png" alt="Preços" /> | <img src="docs/img/orcamento.png" alt="Orçamento" /> |

<table>
<tr>
<td width="300"><img src="docs/img/cardapio.png" alt="Cardápio digital no celular" width="300" /></td>
<td>

### Cardápio digital

- Link e **QR Code por mesa**
- Pedido **sem cadastro**
- Horário de funcionamento automático
- **Pix copia e cola** com QR Code
- "Meus pedidos" e "Pedir de novo"
- Nas **cores da marca** do estabelecimento
- Limite antiabuso por telefone

</td>
</tr>
</table>

<p align="center"><img src="docs/img/landing.png" alt="Página inicial" width="100%" /></p>

> Todas as capturas usam **dados fictícios**, geradas automaticamente com Playwright e um banco
> simulado.

## Tudo o que o sistema faz

| Área                 | Recursos                                                                                                                                                                         |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Vendas**           | PDV com busca, adicionais e tabela de preço · pedidos em tempo real · cozinha (KDS) · devolução e troca com crédito · impressão 80 mm · WhatsApp com mensagem pronta              |
| **Cardápio digital** | QR por mesa · pedido sem conta · horário de funcionamento · Pix com QR · "Pedir de novo" · cores da marca                                                                       |
| **Comercial**        | Orçamento → pré-pedido → pedido de venda → faturamento · negociação de preço só pela gestão · limite de desconto por papel · PDF A4 · reserva de estoque                          |
| **Entregas**         | Ordem de entrega a partir da venda · romaneio com assinatura · etapas (carregando, em rota, entregue) · valor a cobrar · link do mapa                                            |
| **Preços**           | Composição de custo (compra, frete, IPI/ST, outros) · markup ou margem (markup divisor) · reprecificação em massa · tabelas com ajuste %, preço próprio, faixas e validade         |
| **Pagamentos**       | Formas com taxa e prazo · cartão por bandeira · até 12x com "sem juros até" e juros por parcela · crédito do cliente                                                             |
| **Estoque e WMS**    | Ficha técnica · custo médio · saldo, reservado e previsto · compras · depósitos e endereços · transferências entre depósitos, filiais e empresas · inventário · movimentações     |
| **Planilhas**        | Modelo .xlsx com instruções · importação de produtos, preços, custos, estoque e fiscal · exportação · erro por linha sem travar o resto                                           |
| **Financeiro**       | A pagar e a receber com valor líquido · baixas · fluxo de caixa · **DRE** com análise vertical, comparação e ponto de equilíbrio                                                 |
| **Fiscal**           | Emitente com consulta de CNPJ · NCM, CEST, CFOP, CST/CSOSN · NF-e, NFC-e e NFS-e validadas contra rejeições comuns · fila e cancelamento · integração com provedor (Focus NFe)  |
| **Cadastros**        | Clientes PF/PJ com IE e regime tributário · **CEP e CNPJ preenchem sozinhos** · fornecedores · equipe por convite com papéis                                                     |
| **Gestão**           | Auditoria de tudo com antes/depois · licenciamento por plano e módulo · superadmin com receita recorrente                                                                        |
| **Experiência**      | Tour guiado · central de ajuda · cores da marca · PWA instalável · dados de exemplo por segmento                                                                                |

## Segmentos

O mesmo banco e as mesmas telas, com a linguagem de cada negócio:

| Segmento                   | Chama a venda de | Ênfase                                                     |
| -------------------------- | ---------------- | ---------------------------------------------------------- |
| 🍔 Lanchonete / restaurante | Pedido           | Cardápio digital, cozinha, adicionais, ficha técnica       |
| 👕 Loja / varejo            | Venda            | PDV, grade de produtos, crediário, tabelas de preço        |
| 🧱 Material de construção   | Pedido           | Orçamento, entrega com romaneio, venda por quantidade, WMS |
| 🛠️ Prestador de serviço     | Atendimento      | Orçamento, NFS-e, agenda de atendimento                    |
| 🏪 Comércio em geral        | Venda            | O essencial de tudo                                        |

## Arquitetura

```mermaid
flowchart LR
  subgraph Navegador
    A[App React / PWA]
    M[Cardápio público]
  end
  subgraph Supabase
    B[(PostgreSQL<br/>+ RLS)]
    R[Realtime]
    S[Storage]
    F[Edge Function<br/>fiscal-emit]
  end
  A -->|supabase-js| B
  M -->|RPC anônima com limite| B
  B --- R --> A
  B --- S
  F --> B
  F -->|HTTPS| P[Provedor fiscal]
  G[GitHub Actions] -->|migrations| B
  G -->|build| C[Cloudflare Pages]
  C --> A
```

**Decisões que fazem a diferença:**

- **Regra de negócio no banco.** Preço, estoque, financeiro, status, numeração fiscal e limites
  de desconto vivem em funções PostgreSQL `SECURITY DEFINER`. O navegador nunca define um preço
  nem um saldo: mesmo que alguém altere o front, o banco recusa.
- **Tudo vira movimentação.** O estoque só muda por movimentos (com depósito e endereço) e o
  dinheiro só por lançamentos. Nada é sobrescrito: há histórico completo e auditável.
- **Modelo híbrido de segmentos.** Um arquivo de configuração define vocabulário, menu e cor por
  segmento; banco e rotas são compartilhados. Novo segmento = configuração, não reescrita.
- **Idempotência.** Pedidos aceitam chave de idempotência: clique duplo não duplica venda.
- **Módulos por empresa.** Cada módulo é ligado pelo plano e conferido no banco, não só no menu.

Mais detalhes em [docs/ARQUITETURA.md](docs/ARQUITETURA.md).

## Stack

| Camada             | Tecnologia                                                                  |
| ------------------ | --------------------------------------------------------------------------- |
| Interface          | **React 19** · **TypeScript strict** · Vite 8 · React Router 7               |
| Estilo             | **Tailwind CSS v4** · componentes no padrão shadcn/ui (Radix) · lucide-react |
| Estado de servidor | **TanStack Query**                                                          |
| Formulários        | React Hook Form + **Zod**                                                   |
| Gráficos           | Recharts                                                                    |
| Tour guiado        | driver.js                                                                   |
| Planilhas          | read-excel-file · write-excel-file                                          |
| Banco              | **PostgreSQL** (Supabase) com Row Level Security e PL/pgSQL                 |
| Auth / Realtime    | Supabase Auth · Supabase Realtime                                           |
| Funções            | Supabase Edge Functions (Deno)                                              |
| Hospedagem         | Cloudflare Pages (CDN global)                                               |
| CI/CD              | GitHub Actions: lint, tipos, testes, build, migrations, backup              |
| Testes             | Vitest · **PGlite** (Postgres em memória) · Playwright (capturas)           |
| APIs públicas      | ViaCEP · BrasilAPI (CNPJ)                                                   |

## Segurança

- **Isolamento total entre empresas** por Row Level Security em todas as tabelas.
- **Seis papéis** (proprietário, administrador, gerente, caixa, cozinha, atendente); cada função
  do banco confere o papel antes de agir.
- **Colunas protegidas:** plano, status, cores da marca e numeração fiscal não aceitam alteração
  direta.
- **Credencial fiscal só de escrita:** o token do provedor é gravado, mas ninguém o lê de volta.
- **Cardápio público** limitado a funções específicas, com antiabuso.
- **Auditoria** de inserções, alterações e exclusões com antes, depois e autor.
- **Licença operacional:** fora do período pago (+ tolerância), o sistema fica somente leitura.

## Qualidade e testes

**66 testes automatizados** rodam em segundos, sem servidor, inclusive os do banco: as migrations
são aplicadas num **PostgreSQL em memória (PGlite)** e as regras são testadas de verdade.

- Isolamento entre empresas e permissões por papel
- Preço calculado pelo servidor e idempotência
- Estoque e financeiro na conclusão da venda, devoluções, compras
- Tabelas de preço, parcelamento por bandeira, importação de planilha
- Transferência entre empresas, inventário, orçamento → venda, entregas
- Validações fiscais e numeração
- Dados de exemplo dos 5 segmentos
- Cálculo do DRE, formação de preço, CPF/CNPJ

Toda alteração passa por **lint + formatação + tipos + testes + build** no CI, e as migrations
são aplicadas automaticamente ao entrar na `main`.

## Infraestrutura a custo zero

| Serviço          | Plano | Uso                                               |
| ---------------- | ----- | ------------------------------------------------- |
| Supabase         | Free  | Banco, autenticação, realtime, storage            |
| Cloudflare Pages | Free  | Frontend com CDN e preview por branch             |
| GitHub Actions   | Free  | CI, migrations, backup semanal, keep-alive diário |

## Ilustrações 3D

Os dados de exemplo de cada segmento vêm com **ilustrações 3D geradas por código** (SVG com
gradientes, luz e sombra), leves e sem direitos de terceiros:

|   |   |   |   |   |   |
|:-:|:-:|:-:|:-:|:-:|:-:|
| <img src="docs/ilustracoes/acai.svg" width="84" alt="acai" /><br/><sub>acai</sub> | <img src="docs/ilustracoes/areia.svg" width="84" alt="areia" /><br/><sub>areia</sub> | <img src="docs/ilustracoes/argamassa.svg" width="84" alt="argamassa" /><br/><sub>argamassa</sub> | <img src="docs/ilustracoes/batata-frita.svg" width="84" alt="batata-frita" /><br/><sub>batata frita</sub> | <img src="docs/ilustracoes/bone.svg" width="84" alt="bone" /><br/><sub>bone</sub> | <img src="docs/ilustracoes/brita.svg" width="84" alt="brita" /><br/><sub>brita</sub> |
| <img src="docs/ilustracoes/brownie.svg" width="84" alt="brownie" /><br/><sub>brownie</sub> | <img src="docs/ilustracoes/caderno.svg" width="84" alt="caderno" /><br/><sub>caderno</sub> | <img src="docs/ilustracoes/cafe.svg" width="84" alt="cafe" /><br/><sub>cafe</sub> | <img src="docs/ilustracoes/calca-jeans.svg" width="84" alt="calca-jeans" /><br/><sub>calca jeans</sub> | <img src="docs/ilustracoes/camiseta-azul.svg" width="84" alt="camiseta-azul" /><br/><sub>camiseta azul</sub> | <img src="docs/ilustracoes/camiseta-preta.svg" width="84" alt="camiseta-preta" /><br/><sub>camiseta preta</sub> |
| <img src="docs/ilustracoes/caneta.svg" width="84" alt="caneta" /><br/><sub>caneta</sub> | <img src="docs/ilustracoes/cano-pvc.svg" width="84" alt="cano-pvc" /><br/><sub>cano pvc</sub> | <img src="docs/ilustracoes/chave.svg" width="84" alt="chave" /><br/><sub>chave</sub> | <img src="docs/ilustracoes/cimento.svg" width="84" alt="cimento" /><br/><sub>cimento</sub> | <img src="docs/ilustracoes/fone.svg" width="84" alt="fone" /><br/><sub>fone</sub> | <img src="docs/ilustracoes/furadeira.svg" width="84" alt="furadeira" /><br/><sub>furadeira</sub> |
| <img src="docs/ilustracoes/garrafa.svg" width="84" alt="garrafa" /><br/><sub>garrafa</sub> | <img src="docs/ilustracoes/lampada.svg" width="84" alt="lampada" /><br/><sub>lampada</sub> | <img src="docs/ilustracoes/meia.svg" width="84" alt="meia" /><br/><sub>meia</sub> | <img src="docs/ilustracoes/mochila.svg" width="84" alt="mochila" /><br/><sub>mochila</sub> | <img src="docs/ilustracoes/prancheta.svg" width="84" alt="prancheta" /><br/><sub>prancheta</sub> | <img src="docs/ilustracoes/refrigerante.svg" width="84" alt="refrigerante" /><br/><sub>refrigerante</sub> |
| <img src="docs/ilustracoes/rolo-pintura.svg" width="84" alt="rolo-pintura" /><br/><sub>rolo pintura</sub> | <img src="docs/ilustracoes/telha.svg" width="84" alt="telha" /><br/><sub>telha</sub> | <img src="docs/ilustracoes/tesoura.svg" width="84" alt="tesoura" /><br/><sub>tesoura</sub> | <img src="docs/ilustracoes/tijolo.svg" width="84" alt="tijolo" /><br/><sub>tijolo</sub> | <img src="docs/ilustracoes/tinta.svg" width="84" alt="tinta" /><br/><sub>tinta</sub> | <img src="docs/ilustracoes/vergalhao.svg" width="84" alt="vergalhao" /><br/><sub>vergalhao</sub> |
| <img src="docs/ilustracoes/x-bacon.svg" width="84" alt="x-bacon" /><br/><sub>x bacon</sub> | <img src="docs/ilustracoes/x-burger.svg" width="84" alt="x-burger" /><br/><sub>x burger</sub> | <img src="docs/ilustracoes/x-salada.svg" width="84" alt="x-salada" /><br/><sub>x salada</sub> |  |  |  |

## Planos

Módulos são liberados por plano e podem ser ajustados por empresa. **Na avaliação, tudo fica
liberado.**

| Módulo                                                                  | Inicial | Profissional | Empresarial |
| ----------------------------------------------------------------------- | :-----: | :----------: | :---------: |
| Painel, pedidos/PDV, produtos, clientes, estoque, financeiro, auditoria |    ✓    |      ✓       |      ✓      |
| Cardápio digital e cozinha                                              |    ✓    |      ✓       |      ✓      |
| Fornecedores, relatórios, fidelidade                                    |         |      ✓       |      ✓      |
| Orçamentos, pedidos de venda e entregas                                 |         |      ✓       |      ✓      |
| Depósitos (WMS) e notas fiscais                                         |         |              |      ✓      |

## Roteiro

- [x] Núcleo multiempresa, PDV, cardápio, estoque, financeiro
- [x] Formação de preço, tabelas de preço e planilhas
- [x] Orçamentos, pedidos de venda, entregas, WMS e transferências
- [x] Fiscal pronto (NF-e, NFC-e, NFS-e) e DRE
- [ ] Ligar a emissão fiscal no primeiro cliente
- [ ] WhatsApp Cloud API com confirmação e status automáticos
- [ ] Integração com iFood e apps de entrega
- [ ] Conciliação bancária por OFX
- [ ] Lote e validade no estoque
- [ ] Ordem de serviço com checklist e assinatura
- [ ] App do entregador

## Documentação

| Documento                                        | Para quem                                       |
| ------------------------------------------------ | ----------------------------------------------- |
| [Guia do sistema](docs/GUIA-DO-SISTEMA.md)       | Quem usa: cada módulo explicado passo a passo   |
| [Arquitetura](docs/ARQUITETURA.md)               | Quem desenvolve: decisões, camadas e dados      |
| [Fiscal](docs/FISCAL.md)                         | Como a emissão de NF-e, NFC-e e NFS-e funciona  |
| [Histórico de versões](CHANGELOG.md)             | O que mudou em cada versão                      |

## Autor

<table>
<tr>
<td><img src="https://github.com/allancorreaz.png" width="88" alt="Allan Corrêa" /></td>
<td>

**Allan Corrêa** — desenvolvedor backend.<br/>
Projeto, arquitetura, banco de dados e produto.<br/>
[GitHub](https://github.com/allancorreaz) · [LinkedIn](https://www.linkedin.com/in/allandemelocorrea-45875016a) ·
[Portfólio](https://meu-portifolio-dun-two.vercel.app)

</td>
</tr>
</table>

---

<sub>© Allan Corrêa. Produto proprietário: este repositório reúne apresentação, documentação e
imagens; o código-fonte é privado. Ilustrações geradas por código para o próprio projeto.</sub>
