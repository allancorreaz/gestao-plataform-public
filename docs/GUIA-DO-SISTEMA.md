# Guia do sistema

Como cada parte funciona, para quem vai usar, treinar a equipe ou apresentar a um cliente.
Dentro do app, o botão **?** no canto de cada tela mostra o mesmo conteúdo com um tour guiado.

## Sumário

1. [Primeiros passos](#1-primeiros-passos)
2. [Painel](#2-painel)
3. [Vendas (PDV) e pedidos](#3-vendas-pdv-e-pedidos)
4. [Cozinha](#4-cozinha)
5. [Cardápio digital e pedidos pelo WhatsApp](#5-cardápio-digital-e-pedidos-pelo-whatsapp)
6. [Orçamentos, pré-pedidos e pedidos de venda](#6-orçamentos-pré-pedidos-e-pedidos-de-venda)
7. [Entregas](#7-entregas)
8. [Produtos, preços e tabelas](#8-produtos-preços-e-tabelas)
9. [Importação por planilha](#9-importação-por-planilha)
10. [Estoque, compras e depósitos (WMS)](#10-estoque-compras-e-depósitos-wms)
11. [Clientes e fornecedores](#11-clientes-e-fornecedores)
12. [Financeiro e formas de pagamento](#12-financeiro-e-formas-de-pagamento)
13. [DRE e resultado](#13-dre-e-resultado)
14. [Notas fiscais](#14-notas-fiscais)
15. [Configurações, equipe e cores](#15-configurações-equipe-e-cores)
16. [Auditoria](#16-auditoria)
17. [Planos, licença e superadmin](#17-planos-licença-e-superadmin)
18. [Recomendações por segmento](#18-recomendações-por-segmento)

---

## 1. Primeiros passos

1. Crie a conta e a empresa escolhendo o **tipo de negócio**. O sistema adapta nomes ("Pedido",
   "Venda", "Atendimento"), menu e cores.
2. O **tour com destaque** abre sozinho: ele escurece a tela e explica cada parte. Em toda tela há
   um tour próprio, até você marcar "Não mostrar mais".
3. Em **Configurações → Ferramentas**, carregue os **dados de exemplo** para conhecer o sistema
   com produtos ilustrados, vendas de uma semana, estoque em dois depósitos, orçamento e entrega.
4. Siga o checklist do Painel: cadastre produtos (ou importe a planilha), formas de pagamento e
   convide a equipe.

## 2. Painel

- Faixa de boas-vindas com o que está em andamento e o horário de pico.
- Faturamento, vendas e ticket médio **comparados com o período anterior**.
- Vendas por dia, **horários de pico** e **dias da semana**: use para escalar a equipe e planejar
  compras.
- Por onde vendeu (balcão, cardápio digital), formas de pagamento e taxas pagas.
- Clientes novos e recorrentes, estoque baixo e situação do dinheiro.

Caixa e atendentes veem quantidades; valores ficam para a gestão.

## 3. Vendas (PDV) e pedidos

- **Nova venda:** busque por nome, código ou código de barras; itens com adicionais abrem as
  opções. Escolha tipo (mesa, retirada, entrega, balcão), cliente, **tabela de preço**, forma de
  pagamento, **bandeira e parcelas**.
- O preço final é sempre recalculado pelo servidor (o caixa não consegue mudar o preço no
  navegador). Clique duplo não duplica a venda.
- **Pedidos** em colunas, atualizando sozinho, com bip para pedidos novos do cardápio.
- No pedido: aceitar, avançar, cobrar (Pix com QR), imprimir comanda ou cupom, avisar no
  WhatsApp, gerar **ordem de entrega** e **nota fiscal**.
- **Concluir** baixa o estoque pela ficha técnica, lança a conta a receber (líquida da taxa, na
  data em que o banco paga) e dá pontos de fidelidade.
- **Devolução e troca** parcial: devolve em dinheiro ou como crédito do cliente, voltando (ou não)
  ao estoque.

## 4. Cozinha

Fila de produção em ordem de chegada. "Iniciar preparo" e "Pronto". Pedidos ficam amarelos após
15 minutos e vermelhos após 25. Ideal numa TV ou tablet.

## 5. Cardápio digital e pedidos pelo WhatsApp

- Link público e **QR Code por mesa**. O cliente pede sem baixar app e sem criar conta.
- Abre e fecha sozinho pelo horário de funcionamento; mostra taxa de entrega e Pix.
- O pedido cai na tela de Pedidos na hora. O cliente acompanha por um link e pode **confirmar o
  recebimento**.
- **WhatsApp grátis:** configure no WhatsApp Business a mensagem de saudação com o link do
  cardápio. Todo cliente que chamar recebe o link e pede por ele. Para respostas automáticas de
  status é preciso a API oficial da Meta (paga por mensagem), prevista no roteiro.

## 6. Orçamentos, pré-pedidos e pedidos de venda

```
Orçamento ──(aprovado)──► Pré-pedido ──(gestão aprova)──► Pedido de venda ──► Faturar = venda
     └───────────────────────────────────────────────────► Pedido de venda
```

- **Orçamento:** cliente, itens pelo preço da tabela, descontos por item e geral, frete, validade,
  condição de pagamento. Envie em **PDF** (com a cor da sua marca) ou pelo **WhatsApp**.
- Só a gestão negocia o **preço unitário**. Vendedores dão desconto até o limite definido em
  Configurações → Operação; acima disso, o servidor recusa.
- **Pedido de venda** reserva o estoque (aparece em "Reservado") e tem as etapas em aberto →
  em separação → pronto.
- **Faturar** gera a venda com os preços negociados, registra o pagamento (inclusive parcelado) e,
  ao concluir, baixa estoque e lança o financeiro.
- Recusa e cancelamento exigem motivo. Tudo fica na auditoria.

## 7. Entregas

- Crie a ordem a partir da venda ou do pedido de venda (os itens vêm sozinhos) ou avulsa.
- Quadro: agendada → carregando → em rota → entregue / não entregue.
- **Romaneio** impresso com checklist de itens e campos de assinatura.
- Valor a **cobrar na entrega**, motorista, veículo, período e link do mapa.
- "Entregue" pede o nome de quem recebeu; "não entregue" pede o motivo e permite reagendar.

## 8. Produtos, preços e tabelas

**Composição do custo** por unidade: compra + frete + impostos da compra (IPI/ST) + outros
(embalagem). O DRE e a análise de margem usam esse custo total.

**Formação do preço**, dois métodos:

| Método               | Fórmula                                                                 | Quando usar                                                  |
| -------------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------ |
| Markup sobre o custo | preço = custo × (1 + markup%)                                           | Referência rápida                                            |
| Margem desejada      | preço = custo ÷ (1 − impostos% − cartão% − comissão% − fixas% − lucro%) | Garante o lucro depois de pagar tudo o que sai em % da venda |

Exemplo: custo R$ 12,00 com 6% de impostos, 3% de cartão, 15% de despesas fixas e 15% de lucro
→ 12 ÷ (1 − 0,39) = **R$ 19,67** (arredondado para R$ 19,90). Um markup de 64% daria o mesmo
preço, mas só a margem desejada mostra que sobram 15% de verdade.

Em **Preços e tabelas**:

- Parâmetros padrão da empresa e lista de produtos **abaixo do preço sugerido**, com "Aplicar
  sugerido" em massa.
- **Tabelas de preço** (atacado, revenda, delivery, funcionários): ajuste % sobre o preço normal,
  preço próprio por produto, **preço por faixa de quantidade** e validade. No caixa e no
  orçamento, escolha a tabela e os preços mudam.

## 9. Importação por planilha

1. Baixe o **modelo .xlsx**: a aba "Instruções" explica cada coluna.
2. Preencha (ou exporte os produtos atuais, edite no Excel e importe de volta).
3. Envie .xlsx ou .csv, confira a prévia e importe.

- Produtos são reconhecidos pelo **código** (ou pelo nome); colunas vazias não apagam nada.
- "Estoque atual" é a contagem: o sistema lança só a diferença como ajuste.
- Classificação fiscal (NCM, CEST, CFOP, CST/CSOSN, origem) também entra pela planilha.
- Linhas com erro voltam numa lista (com a linha da planilha) e as demais são importadas.

## 10. Estoque, compras e depósitos (WMS)

- **Itens de estoque** (ingredientes ou mercadorias) com mínimo e custo médio ponderado.
- **Saldo, reservado, comprado, disponível e previsto** para cada item.
- **Compras:** registre o pedido ao fornecedor; ao receber, entra no estoque com o custo e gera a
  conta a pagar.
- **Depósitos:** loja, galpão, câmara fria, filial. Cada um com saldo próprio.
- **Endereços** (rua-prédio-nível) gerados em lote, por exemplo A-C × 1-10 × 1-4.
- **Transferências** entre depósitos, filiais ou **empresas do mesmo dono**: sai ao enviar, entra
  quando o destino confirma (registrando divergência), cancela devolvendo à origem.
- **Inventário:** congela o saldo esperado, a equipe conta e, ao aplicar, só a diferença vira
  ajuste.
- **Movimentações** com filtro por período, tipo, depósito e item, e exportação.

## 11. Clientes e fornecedores

- Cliente **pessoa física ou empresa**. Com CNPJ, o botão "Receita" preenche razão social,
  endereço e regime. O CEP completa rua, bairro, cidade, UF e **código IBGE**.
- Dados fiscais: contribuinte do ICMS (com IE, isento ou não contribuinte), IE, IM e regime.
- Histórico de pedidos, pontos e **extrato de crédito** (vale-troca).

## 12. Financeiro e formas de pagamento

- Cada maquininha com taxa, prazo e conta de destino. **Crédito em até 12x**, "sem juros até",
  e tabela por **bandeira × parcela** com taxa da máquina, juros ao cliente e prazo.
- A receber mostra taxa e **valor líquido**; a pagar com categorias.
- Baixas, fluxo de caixa por dia e exportação.

## 13. DRE e resultado

Receita bruta → deduções (descontos, devoluções, impostos) → **receita líquida** → CMV → **lucro
bruto** → despesas variáveis (taxas de cartão) → **margem de contribuição** → despesas
operacionais → **resultado**.

- Mês, trimestre ou ano, comparado com o período anterior.
- Coluna "% da receita" (análise vertical) e variação.
- **Ponto de equilíbrio**: quanto vender para empatar.
- Compras de fornecedor não são despesa: o custo entra no CMV quando o produto é vendido.

## 14. Notas fiscais

Resumo aqui; detalhes técnicos em [FISCAL.md](FISCAL.md).

1. **Configurações → Fiscal:** CNPJ (preenche pela Receita), regime, IE/IM, tipos de nota, séries
   e próximo número, padrões de CFOP e CST/CSOSN, emissor e token.
2. **Produtos:** NCM, CEST, CFOP, origem, CST/CSOSN, ou serviço com código LC 116.
3. **Venda concluída → Nota fiscal:** escolha NF-e, NFC-e ou NFS-e. O sistema monta o rascunho e
   aponta o que a SEFAZ/prefeitura rejeitaria.
4. **Emitir:** com emissor ligado, a nota vai para a fila e volta autorizada com DANFE e XML.

Qual nota usar:

| Situação                                                                    | Nota                                            |
| --------------------------------------------------------------------------- | ----------------------------------------------- |
| Venda no balcão para consumidor final no mesmo estado                       | NFC-e                                           |
| Venda para empresa, para outro estado ou com entrega e destaque de impostos | NF-e                                            |
| Prestação de serviço                                                        | NFS-e                                           |
| MEI vendendo para pessoa física                                             | Geralmente dispensado (confirme com o contador) |

## 15. Configurações, equipe e cores

- **Empresa:** dados e consulta de CNPJ.
- **Aparência:** 10 combinações prontas ou cor livre, com prévia. As cores valem para o sistema e
  o cardápio. Depois de salvar, a próxima troca fica liberada em 60 dias (o superadmin pode
  liberar antes, como serviço pago).
- **Operação:** aceite automático, conclusão de pedidos esquecidos, baixa de estoque e limite de
  desconto.
- **Equipe:** convites por link e papéis (proprietário, administrador, gerente, caixa, cozinha,
  atendente).
- **Pagamentos**, **Fiscal**, **Horário**, **Plano** e **Ferramentas** (dados de exemplo e
  tutoriais).

## 16. Auditoria

Tudo o que foi criado, alterado ou excluído, por quem e quando, com o **antes e depois de cada
campo**. Filtros por área, pessoa e período, e exportação.

## 17. Planos, licença e superadmin

- Planos com limites de usuários, filiais e produtos, e módulos incluídos.
- Avaliação libera todos os módulos. Licença ativa vale até "pago até" + 5 dias; depois, somente
  leitura (os dados nunca somem).
- O superadmin (painel **Plataforma**) vê receita recorrente e empresas que precisam de atenção,
  ajusta planos, módulos extras ou bloqueados, "pago até" e libera troca antecipada de cores.

## 18. Recomendações por segmento

| Segmento                | Comece com                                                     | Depois                                                                           |
| ----------------------- | -------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Lanchonete, restaurante | Painel, Pedidos, Cozinha, Cardápio digital, Produtos, Clientes | Financeiro e DRE; Estoque com ficha técnica dos 10 itens mais vendidos; Entregas |
| Loja (varejo)           | PDV, Produtos (importe a planilha), Estoque, Clientes          | Tabelas de preço, parcelamento por bandeira, NFC-e                               |
| Material de construção  | Orçamentos e pedidos de venda, Entregas, Estoque               | Depósitos e endereços, transferências, NF-e                                      |
| Serviços                | Atendimentos, Clientes, Financeiro                             | Orçamentos, NFS-e                                                                |

Libere aos poucos: sistema demais no primeiro dia assusta. Uma semana por etapa funciona bem.
