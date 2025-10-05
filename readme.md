# 🚀 Evolution API v2 - WhatsApp API (Versão Corrigida)

**✅ Testado em Dezembro 2024 com WhatsApp Web 2.3000.1028000487**

Stack completa da Evolution API com PostgreSQL, Redis e correções para QR Code.

## 🎯 Correções Incluídas

- ✅ Versão atualizada do WhatsApp Web (2.3000.1028000487)
- ✅ Fix para geração de QR Code
- ✅ Variáveis sem duplicação
- ✅ Configurações otimizadas para estabilidade

## ⚡ Deploy Rápido via Portainer

### Passo 1: Criar Repositório no GitHub

1. Crie um novo repositório no GitHub
2. Adicione estes arquivos:
   - `docker-compose.yml` (use o arquivo fornecido)
   - `.env.example` (template de variáveis)
   - `README.md` (este arquivo)

### Passo 2: Gerar Chaves de Segurança

```bash
# Gerar API_KEY (GUARDE ISSO!)
openssl rand -hex 32

# Exemplo de saída (NÃO USE ESTA!):
# f3a8b9c2d4e6f8a1b3c5d7e9f0a2b4c6d8e0f2a4b6c8d0e2f4a6b8c0d2e4f6a8
```

### Passo 3: Deploy no Portainer

1. **Acesse Portainer** → **Stacks** → **Add Stack**
2. **Nome**: `evolution-api`
3. **Build method**: Repository
4. **Configurações do Git**:
   - **Repository URL**: `https://github.com/SEU-USUARIO/SEU-REPOSITORIO`
   - **Reference**: `main`
   - **Compose path**: `docker-compose.yml`

### Passo 4: Variáveis de Ambiente (OBRIGATÓRIAS!)

No Portainer, adicione estas variáveis:

```bash
# ESSENCIAIS - MUDE TODAS!
API_KEY=cole-sua-chave-de-32-caracteres-gerada
POSTGRES_PASSWORD=senha-super-forte-postgres-2024
SERVER_URL=http://SEU-IP:8080

# BÁSICAS
POSTGRES_DB=evolution
POSTGRES_USER=evolution
API_PORT=8080
```

### Passo 5: Deploy

1. Clique em **Deploy the stack**
2. Aguarde 2-3 minutos
3. Verifique se todos os containers estão "Running"

## 📱 Primeiro Acesso

1. **Acesse o Manager**: `http://SEU-IP:8080/manager`
2. **Login**: Use a API_KEY que você configurou
3. **Criar Instância**:
   - Nome: `whatsapp01`
   - Clique em "Create"
4. **Conectar WhatsApp**:
   - Clique na instância
   - Selecione "QR Code"
   - Escaneie com WhatsApp

## 🔗 Integração com n8n

### Webhook do Evolution para n8n

1. No Evolution Manager, configure o webhook:
   ```
   URL: http://n8n:5678/webhook/evolution
   Events: Selecione os desejados
   ```

2. No n8n, crie um Webhook node:
   ```
   HTTP Method: POST
   Path: evolution
   ```

### Enviar mensagem do n8n

Use HTTP Request node:
```json
Method: POST
URL: http://evolution-api:8080/message/sendText/whatsapp01
Headers:
  Content-Type: application/json
  apikey: SUA-API-KEY

Body:
{
  "number": "5511999999999",
  "text": "Mensagem do n8n!"
}
```

## 🐛 Troubleshooting

### QR Code não aparece?

O arquivo já contém o fix! Mas se ainda tiver problemas:

1. **Limpe o cache Redis**:
   ```bash
   docker exec evolution-redis redis-cli FLUSHALL
   ```

2. **Reinicie a API**:
   ```bash
   docker restart evolution-api
   ```

3. **Verifique os logs**:
   ```bash
   docker logs evolution-api --tail 50
   ```

### Container não inicia?

```bash
# Ver logs detalhados
docker logs evolution-api -f

# Verificar se PostgreSQL está OK
docker logs evolution-postgres
```

### Webhook não funciona?

Teste manualmente:
```bash
curl -X POST http://localhost:8080/webhook/test \
  -H "apikey: SUA-API-KEY"
```

## 📊 Endpoints Úteis

- **Manager**: `http://SEU-IP:8080/manager`
- **API Docs**: `http://SEU-IP:8080/docs`
- **Health Check**: `http://SEU-IP:8080/health`
- **Metrics**: `http://SEU-IP:8080/metrics`

## 🔧 Comandos Úteis

### Backup do Banco
```bash
docker exec evolution-postgres pg_dump -U evolution evolution > backup_$(date +%Y%m%d).sql
```

### Ver Status das Instâncias
```bash
curl http://localhost:8080/instance/list \
  -H "apikey: SUA-API-KEY" | jq
```

### Reconectar Instância
```bash
curl -X POST http://localhost:8080/instance/restart/whatsapp01 \
  -H "apikey: SUA-API-KEY"
```

## 🔒 Segurança

1. **SEMPRE** mude a API_KEY padrão
2. **Use HTTPS** em produção
3. **Firewall**: Libere apenas porta 8080
4. **Backup**: Configure backup diário do PostgreSQL

## 📚 Links Importantes

- [Evolution API Docs](https://doc.evolution-api.com)
- [GitHub Oficial](https://github.com/EvolutionAPI/evolution-api)
- [Discord](https://discord.gg/evolution-api)
- [Telegram BR](https://t.me/evolutionapibr)

## ⚠️ Notas da Versão

- **WhatsApp Web**: 2.3000.1028000487 (Dezembro 2024)
- **Evolution API**: latest
- **PostgreSQL**: 15-alpine
- **Redis**: 7-alpine

## ✅ Checklist de Sucesso

- [ ] API_KEY gerada e configurada
- [ ] PostgreSQL com senha forte
- [ ] Containers rodando
- [ ] QR Code aparecendo
- [ ] WhatsApp conectado
- [ ] Webhook testado
- [ ] Mensagem enviada com sucesso

---

**Problemas?** Abra uma issue no repositório ou entre na comunidade do Telegram!
