# Setup — Evolution API + n8n

> Guia para subir o ambiente em VPS/máquina e configurar todas as credenciais.

---

## 1. Subir o ambiente

```bash
git clone https://github.com/VenturaVini/n8n.git
cd n8n
cp .env.example .env
# edite o .env com suas credenciais
docker compose up -d
```

---

## 2. Instalar o node da Evolution API no n8n

1. No n8n, vá em **Configurações → Community Nodes**
2. Instale: `n8n-nodes-evolution-api`

---

## 3. Configurar credenciais no n8n

### Evolution API
| Campo | Valor |
|---|---|
| Server URL | `http://evolution:8080` |
| API Key | valor de `EVOLUTION_API_KEY` no `.env` |

> A porta `8080` é interna do Docker. Use `evolution` como hostname (nome do serviço no compose).

### Google Gemini
| Campo | Valor |
|---|---|
| API Key | sua chave do Google AI Studio |

### Redis
| Campo | Valor |
|---|---|
| Host | `redis` |
| Port | `6379` |
| Password | valor de `REDIS_PASSWORD` no `.env` |

> Use `redis` como hostname (nome do serviço no compose).

---

## 4. Configurar Webhook no n8n

### URL de teste (acesso externo)
```
http://<IP_DO_SERVIDOR>:5678/webhook-test/<id-do-webhook>
```

### Se o webhook não responder externamente, use o hostname interno
```
http://n8n:5678/webhook-test/<id-do-webhook>
```

> Troque `<IP_DO_SERVIDOR>` pelo IP da sua VPS e `<id-do-webhook>` pelo ID gerado no nó Webhook do n8n.

---

## 5. Configurar instância no portal da Evolution API

1. Acesse `http://<IP_DO_SERVIDOR>:8080`
2. Adicione o número de telefone
3. Ative a **primeira opção** (ativo)
4. Em **Webhook**:
   - Ative **Webhook Base64**
   - Ative o evento **MESSAGES_UPSERT**
   - Salve

---

## 6. Campos da mensagem recebida no Webhook (nó "Padronizar mensagem")

Mapeamento dos campos que chegam via webhook da Evolution:

| Campo | Expressão n8n |
|---|---|
| `instancia` | `{{ $json.body.instance }}` |
| `chat_id` | `{{ $json.body.data.key.remoteJid }}` |
| `id` | `{{ $json.body.data.key.remoteJidAlt }}` |
| `envio_bot` | `{{ $json.body.data.key.fromMe }}` |
| `Tipo de mensagem` | `{{ $json.body.data.messageType }}` |
| `id_da_mensagem` | `{{ $json.body.data.key.id }}` |
