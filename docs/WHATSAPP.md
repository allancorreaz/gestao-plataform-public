# WhatsApp dos pedidos

O sistema avisa o cliente em cada etapa do pedido e responde quem manda mensagem com o
link de pedidos. A conexão usa a **Evolution API** (código aberto, gratuita), que conecta
o número da loja por QR Code, como o WhatsApp Web.

> **Risco que você precisa saber.** É uma conexão **não oficial**: contraria os termos do
> WhatsApp, e a Meta pode bloquear o número. Para reduzir o risco: use um número só do
> negócio, não mande mensagem em massa nem para quem nunca falou com a loja, e mantenha os
> textos curtos. O sistema já espaça os envios (1,2 s) e só responde automaticamente uma vez
> a cada 12 h por número. Quando houver clientes pagando, a migração para a API oficial
> (WhatsApp Cloud API) troca só a função de envio.

## Como funciona

```
Pedido muda de etapa ──► gatilho no banco põe a mensagem em whatsapp_outbox
                                   │
pg_cron (a cada minuto) ──► Edge Function whatsapp-hook?mode=cron ──► Evolution API ──► WhatsApp do cliente

Cliente manda mensagem ──► Evolution API ──► whatsapp-hook?token=… ──► responde link / status
```

- **Um servidor da Evolution atende todas as empresas.** Cada empresa tem a sua instância
  (`gp-<slug>`), criada sozinha quando o dono clica em **Configurações → WhatsApp → Conectar**.
- Textos, etapas avisadas e resposta automática são configurados por empresa na mesma tela.

## 1. Subir a Evolution API de graça (Oracle Cloud Free)

Você já tem conta na Oracle. Use uma VM **Always Free** (Ampere A1, Ubuntu). Na VM:

```bash
sudo apt update && sudo apt install -y docker.io docker-compose-plugin
mkdir evolution && cd evolution
```

Crie `docker-compose.yml` (confira a versão estável atual em
github.com/EvolutionAPI/evolution-api antes de subir):

```yaml
services:
  evolution:
    image: evoapicloud/evolution-api:latest
    restart: always
    ports: ['8080:8080']
    environment:
      SERVER_URL: https://zap.seudominio.com.br
      AUTHENTICATION_API_KEY: TROQUE-POR-UMA-CHAVE-LONGA
      DATABASE_PROVIDER: postgresql
      DATABASE_CONNECTION_URI: postgresql://evo:evo@postgres:5432/evolution
      CACHE_REDIS_ENABLED: 'true'
      CACHE_REDIS_URI: redis://redis:6379/1
    depends_on: [postgres, redis]
  postgres:
    image: postgres:16
    restart: always
    environment: { POSTGRES_USER: evo, POSTGRES_PASSWORD: evo, POSTGRES_DB: evolution }
    volumes: ['pg:/var/lib/postgresql/data']
  redis:
    image: redis:7
    restart: always
  caddy:
    image: caddy:2
    restart: always
    ports: ['80:80', '443:443']
    command: caddy reverse-proxy --from zap.seudominio.com.br --to evolution:8080
    volumes: ['caddy:/data']
volumes: { pg: {}, caddy: {} }
```

```bash
sudo docker compose up -d
```

- Aponte `zap.seudominio.com.br` (registro A) para o IP público da VM. O Caddy gera o HTTPS
  sozinho.
- Libere as portas 80 e 443 na **Security List** da VCN da Oracle e no firewall da VM
  (`sudo iptables -I INPUT -p tcp -m multiport --dports 80,443 -j ACCEPT`).

## 2. Informar o servidor no sistema

Em **/admin → Integrações → WhatsApp**: cole o endereço (`https://zap.seudominio.com.br`) e a
chave `AUTHENTICATION_API_KEY` do compose. O token do webhook e o código do envio automático já
foram gerados e são usados sozinhos.

## 3. Envio automático

Já está agendado: o banco chama a função `whatsapp-hook` a cada minuto
(`supabase/ops/whatsapp-cron.sql`). Enquanto o servidor não estiver informado, a função só
responde "não configurado" e nada é enviado.

## 4. Usar

1. Configurações → WhatsApp → **Conectar (QR Code)** → no celular: Aparelhos conectados →
   Conectar aparelho.
2. **Verificar conexão** → **Enviar teste**.
3. Ligue **Mensagens automáticas**, escolha as etapas e salve.

## Problemas comuns

| Sintoma                                      | Causa provável                                        |
| -------------------------------------------- | ----------------------------------------------------- |
| "Servidor do WhatsApp ainda não configurado" | faltam os segredos `EVOLUTION_*`                      |
| Mensagens ficam "Na fila"                    | cron não agendado, ou WhatsApp desconectado           |
| "Falhou" com erro 401                        | `EVOLUTION_API_KEY` diferente da chave do servidor    |
| Não responde quem manda mensagem             | webhook sem o token certo: clique em Conectar de novo |
