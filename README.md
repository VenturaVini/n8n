# 🚀 Docker Compose - n8n + Evolution API + PostgreSQL + Redis

## 📦 Serviços Incluídos

| Serviço | Porta | Descrição |
|---------|-------|-----------|
| **n8n** | 5678 | Plataforma de automação de workflows |
| **Evolution API** | 8080 | API para WhatsApp Web |
| **PostgreSQL** | 5432 | Banco de dados |
| **Redis** | 6379 | Cache e filas de mensagens |

## 🎯 Pré-requisitos

- [Docker](https://www.docker.com/get-started) instalado
- [Docker Compose](https://docs.docker.com/compose/install/) instalado

## 1. Acesse os serviços

- **n8n**: http://localhost:5678
- **Evolution API Manager**: http://localhost:8080/manager
- **Evolution API Docs**: http://localhost:8080/docs

## 🔐 Credenciais Padrão

### PostgreSQL
```
Usuário: postgres
Senha: postgres123
Banco n8n: n8n
Banco Evolution: evolution
```

### Redis
```
Senha: redis123
```

### Evolution API
```
API Key: evolution_api_key_12345
```

> ⚠️ **IMPORTANTE**: Altere essas credenciais antes de usar em produção!

## 🔧 Personalizando Configurações

Edite o arquivo `docker-compose.yml` e procure pelos comentários:

```yaml
# Credenciais do banco (altere se necessário)
POSTGRES_PASSWORD: postgres123

# Senha do Redis (altere se necessário)
--requirepass redis123

# API Key (ALTERE AQUI - importante!)
AUTHENTICATION_API_KEY: evolution_api_key_12345
```

Se alterar as credenciais, lembre-se de ajustar em **todos os lugares** onde elas aparecem:
- PostgreSQL ➜ n8n e Evolution API
- Redis ➜ n8n e Evolution API

## 📚 Comandos Úteis

### Parar os serviços
```bash
docker-compose down
```

### Parar e remover volumes (⚠️ apaga dados!)
```bash
docker-compose down -v
```

### Reiniciar um serviço específico
```bash
docker-compose restart evolution
```

### Ver status dos containers
```bash
docker-compose ps
```

## 🔌 Testando as APIs

### Evolution API
```bash
# Verificar se está funcionando
curl http://localhost:8080

### n8n
```bash
# Verificar se está funcionando
curl http://localhost:5678
```

## 📖 Usando a Evolution API

### Criar uma instância do WhatsApp
1. Acesse http://localhost:8080/manager
2. Clique em "Create Instance"
3. Configure a instância
4. Escaneie o QR Code com seu WhatsApp

### Integrar com n8n
1. No n8n, crie um novo workflow
2. Adicione um node "Webhook"
3. Configure o webhook da Evolution API para apontar para o n8n
4. Use a URL: `http://host.docker.internal:5678/webhook/seu-webhook`

## 🌐 Usar com Domínio (Produção)

Se quiser usar um domínio próprio, altere no `docker-compose.yml`:

```yaml
# n8n
- N8N_HOST=seu-dominio.com
- N8N_PROTOCOL=https
- WEBHOOK_URL=https://seu-dominio.com/

# Evolution API
- SERVER_URL=https://api.seu-dominio.com
```

## 📁 Estrutura de Volumes

Os dados persistentes ficam salvos em:

```
docker volumes:
├── postgres_data       # Dados do PostgreSQL
├── redis_data          # Dados do Redis
├── n8n_data           # Workflows e credenciais do n8n
├── evolution_instances # Instâncias do WhatsApp
└── evolution_store    # Dados da Evolution API
```

## 🤝 Contribuindo

Sinta-se à vontade para abrir issues ou pull requests!

## 📝 Licença

Este projeto é para fins educacionais.

## ⚠️ Avisos Importantes

- Este setup é para **desenvolvimento/educação**
- Não use em produção sem ajustar as senhas
- A Evolution API é uma API não oficial do WhatsApp
- Respeite os Termos de Serviço do WhatsApp

**Feito com ❤️ para aprendizado**