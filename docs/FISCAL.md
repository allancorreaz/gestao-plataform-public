# Fiscal: NF-e, NFC-e e NFS-e

O sistema já faz tudo o que depende dele. Falta só ligar um **provedor de emissão** quando o
primeiro cliente for emitir de verdade.

## O que já funciona

| Etapa        | Onde                                  | O que acontece                                                                       |
| ------------ | ------------------------------------- | ------------------------------------------------------------------------------------ |
| Emitente     | Configurações → Fiscal                | CNPJ preenche razão social, endereço, código IBGE e regime pela Receita (BrasilAPI). |
| Produtos     | Produtos → Dados fiscais, ou planilha | NCM, CEST, CFOP, origem e CST/CSOSN (lista muda conforme o regime).                  |
| Clientes     | Clientes                              | PF/PJ, IE, indicador de contribuinte, endereço por CEP com código IBGE.              |
| Rascunho     | Venda concluída → Nota fiscal         | Monta emitente, destinatário, itens, totais e pagamentos e roda as regras.           |
| Regras       | `public._fiscal_issues`               | Bloqueia o que a SEFAZ/prefeitura rejeitaria (lista abaixo).                         |
| Numeração    | `request_fiscal_emission`             | Número só é consumido quando a nota está válida: sem buracos, sem repetição.         |
| Emissão      | Edge Function `fiscal-emit`           | Transmite a fila ao provedor e grava chave, protocolo, XML e DANFE.                  |
| Cancelamento | Notas fiscais → Cancelar              | Justificativa ≥ 15 caracteres e prazo de 24 h para NF-e/NFC-e.                       |

### Regras que o sistema confere antes de emitir

- Emitente: CNPJ, CRT, IE (NF-e/NFC-e), IM (NFS-e), endereço completo com código IBGE, CSC da NFC-e.
- Destinatário NF-e: CPF/CNPJ, nome, endereço com IBGE, IE quando contribuinte; aviso para CNPJ "não contribuinte".
- NFC-e: só consumidor final, só dentro do estado, CPF/CNPJ obrigatório a partir de R$ 10.000.
- Itens: NCM de 8 dígitos, CFOP de saída, CSOSN no Simples/MEI e CST no regime normal,
  CEST quando há substituição tributária, CFOP 6xxx para outro estado (troca automática).
- NFS-e: código de serviço (LC 116) em cada item.

## Para ligar a emissão (quando houver cliente pagando)

1. **Certificado digital A1** do cliente (arquivo .pfx, ~R$ 150–250/ano em qualquer certificadora).
2. **Conta no provedor.** Já implementado: **Focus NFe**. Cadastre a empresa do cliente lá, envie o
   certificado e, para NFC-e, o CSC gerado no portal da SEFAZ.
3. No sistema, em **Configurações → Fiscal**: escolha o provedor, cole o **token** (ele é guardado
   só para escrita: ninguém lê pelo app) e ligue os tipos de nota.
4. Publique a função e agende a fila (uma vez para toda a plataforma):

   ```bash
   supabase functions deploy fiscal-emit --no-verify-jwt
   ```

   ```sql
   -- No SQL Editor (precisa das extensões pg_cron e pg_net habilitadas no Supabase)
   select cron.schedule('fiscal-emit', '* * * * *', $$
     select net.http_post(
       url := 'https://<PROJETO>.supabase.co/functions/v1/fiscal-emit',
       headers := jsonb_build_object('Authorization', 'Bearer <SERVICE_ROLE_KEY>')
     );
   $$);
   ```

5. Emita algumas notas em **homologação** e confira no painel do provedor. Só então mude para
   **produção**.

## Outros provedores

`supabase/functions/fiscal-emit/index.ts` tem a interface pronta (`send`, `status`, `cancel`).
PlugNotas, NFE.io, Webmania e o emissor nacional de NFS-e entram como novos adaptadores.

> Este módulo não substitui o contador: ele garante que a nota sai sem erro de preenchimento.
> A classificação fiscal (NCM, CST, alíquotas) deve ser confirmada pelo contador do cliente.
