# Configuração dos Microserviços CPA no Railway

## Status Atual
- ✅ 4 Microserviços criados e deployados
- ✅ 4 Bancos PostgreSQL criados
- ⏳ Configuração de variáveis de ambiente (pendente devido instabilidade do Railway)

## Ordem de Configuração (CRÍTICA)
**IMPORTANTE**: Deve ser seguida esta ordem exata, pois há dependências entre os serviços.

### 1. Config Service (PRIMEIRO - Base para todos)
**Projeto**: `fature-config-service`
**Porta**: 3000
**Dependências**: Apenas banco próprio

**Variáveis de Ambiente:**
```
NODE_ENV=production
PORT=3000
DATABASE_URL=[URL do banco fature-config-db]
```

### 2. MLM Service V2 (SEGUNDO)
**Projeto**: `fature-mlm-service-v2`
**Porta**: 3001
**Dependências**: Config Service + banco próprio

**Variáveis de Ambiente:**
```
NODE_ENV=production
PORT=3001
DATABASE_URL=[URL do banco fature-mlm-db]
CONFIG_SERVICE_URL=[URL do Config Service deployado]
```

### 3. Commission Service (TERCEIRO)
**Projeto**: `fature-commission-service`
**Porta**: 3002
**Dependências**: Config Service + banco próprio

**Variáveis de Ambiente:**
```
NODE_ENV=production
PORT=3002
DATABASE_URL=[URL do banco fature-commission-db]
CONFIG_SERVICE_URL=[URL do Config Service deployado]
```

### 4. Data Service V2 (QUARTO)
**Projeto**: `fature-data-service-v2`
**Porta**: 3003
**Dependências**: Config Service + banco próprio

**Variáveis de Ambiente:**
```
NODE_ENV=production
PORT=3003
DATABASE_URL=[URL do banco fature-data-db]
CONFIG_SERVICE_URL=[URL do Config Service deployado]
```

## URLs dos Bancos PostgreSQL
Para obter as URLs de conexão dos bancos, acesse cada projeto de banco no Railway:

1. **fature-config-db**: Copiar DATABASE_URL das variáveis
2. **fature-mlm-db**: Copiar DATABASE_URL das variáveis  
3. **fature-commission-db**: Copiar DATABASE_URL das variáveis
4. **fature-data-db**: Copiar DATABASE_URL das variáveis

## URLs dos Serviços Deployados
Após o deploy, cada serviço terá uma URL pública no formato:
- Config Service: `https://fature-config-service-production.up.railway.app`
- MLM Service: `https://fature-mlm-service-v2-production.up.railway.app`
- Commission Service: `https://fature-commission-service-production.up.railway.app`
- Data Service: `https://fature-data-service-v2-production.up.railway.app`

## Configuração via CLI Railway (Alternativa)

Se a interface web estiver instável, use o CLI:

```bash
# Instalar Railway CLI
npm install -g @railway/cli

# Login
railway login

# Para cada serviço, navegar para o projeto e configurar:
railway variables set NODE_ENV=production
railway variables set PORT=3000
railway variables set DATABASE_URL="[URL_DO_BANCO]"
railway variables set CONFIG_SERVICE_URL="[URL_CONFIG_SERVICE]"
```

## Testes de Funcionamento

Após configuração, testar cada endpoint:

1. **Config Service**: `GET /health` - deve retornar status 200
2. **MLM Service**: `GET /health` - deve retornar status 200  
3. **Commission Service**: `GET /health` - deve retornar status 200
4. **Data Service**: `GET /health` - deve retornar status 200

## Integração Entre Serviços

- **Config Service**: Fornece configurações centralizadas
- **MLM Service**: Consome configs do Config Service
- **Commission Service**: Consome configs do Config Service  
- **Data Service**: Consome configs do Config Service

## Próximos Passos

1. Aguardar estabilização do Railway
2. Configurar variáveis de ambiente na ordem especificada
3. Testar conectividade entre serviços
4. Validar funcionamento completo do sistema CPA
5. Documentar URLs finais dos serviços

## Troubleshooting

**Problema**: Serviço não inicia
**Solução**: Verificar se DATABASE_URL está correto e banco está acessível

**Problema**: Erro de conexão entre serviços  
**Solução**: Verificar se CONFIG_SERVICE_URL está correto e Config Service está rodando

**Problema**: Deploy falha
**Solução**: Verificar logs do Railway e dependências do package.json

