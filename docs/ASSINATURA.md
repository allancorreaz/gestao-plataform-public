# Assinatura e cobrança (Asaas)

O cliente contrata sozinho em **Configurações → Plano**. O pagamento confirmado renova a
licença automaticamente; sem pagamento, a empresa fica somente leitura no dia seguinte ao
"liberado até". Ninguém precisa marcar nada à mão.

## Por que o Asaas

- Sem mensalidade nem adesão: paga por cobrança recebida (Pix/boleto a partir de
  R$ 0,99–1,99; cartão com percentual).
- Assinatura recorrente pronta: ele gera a fatura todo mês, manda o e-mail/SMS de cobrança e
  avisa o sistema por webhook.
- Aceita conta de **pessoa física** (CPF) para começar; dá para migrar para CNPJ/MEI depois.
- Tem ambiente de **sandbox** para testar sem dinheiro de verdade.

## Fluxo

```
Dono escolhe plano ─► Edge Function billing ─► Asaas cria cliente + assinatura ─► abre a fatura
Asaas confirma pagamento ─► asaas-webhook ─► billing_apply_payment() ─► empresa ativa +1 mês
```

- O **valor** é calculado no banco (`billing_quote`): plano + empresas adicionais × preço da
  vaga. O navegador não define preço.
- Empresas adicionais contratadas viram vagas (`extra_company_slots`): sem pagar, a conta
  só cria 1 empresa.
- Avisos repetidos do Asaas são ignorados (`billing_events.id`), e o mesmo pagamento nunca
  renova duas vezes (`license_payments.provider_ref`).
- Em teste ou em dia, a 1ª cobrança vence quando o período atual termina; vencido, vence
  hoje.

## Configurar (uma vez)

Tudo pelo próprio sistema, em **/admin → Integrações**. Não precisa abrir o Supabase.

1. Crie a conta no Asaas. Para testar, use o **sandbox** (sandbox.asaas.com).
2. No Asaas: Integrações → **Chave de API** → copie.
3. No sistema: /admin → Integrações → **Ambiente: Teste** → cole a chave → Salvar.
4. No Asaas: Integrações → **Webhooks** → Adicionar. Cole a **URL do webhook** e o **Token de
   autenticação** que aparecem na tela de Integrações (o token já vem gerado). Marque os eventos
   de cobranças e de assinaturas.
5. Teste: em uma empresa, Configurações → Plano → Assinar. Pague a fatura de teste pelo painel do
   sandbox e veja a empresa ficar "Em dia" e o "liberado até" avançar 1 mês.
6. Para cobrar de verdade: troque o ambiente para **Produção**, cole a chave de produção e
   cadastre o mesmo webhook na conta de produção.

> As chaves ficam na tabela `platform_integrations`, que só as Edge Functions leem. Se preferir,
> os segredos `ASAAS_API_KEY`/`ASAAS_BASE_URL`/`ASAAS_WEBHOOK_TOKEN` nas Edge Functions têm
> prioridade sobre a tela.

## Preços

A migration `20261108000100` colocou preços **sugeridos** nos planos que estavam em zero:
Inicial R$ 89, Profissional R$ 149, Empresarial R$ 249 e empresa adicional R$ 49. Ajuste em
**/admin → Planos**. Assinaturas já criadas mantêm o valor até o cliente clicar em
"Atualizar assinatura".
