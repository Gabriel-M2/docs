# Documentação da API Finance

Esta documentação descreve todos os endpoints disponíveis na API Finance, organizados por funcionalidade.

## Visão Geral

A API Finance é uma aplicação Spring Boot que gerencia finanças pessoais, oferecendo funcionalidades para controle de contas, categorias e transações.

### Base URL
```
http://localhost:8080
```

### Autenticação
Todos os endpoints (exceto os de teste) requerem o parâmetro `userId` para identificar o usuário.

### Formato de Resposta
Todas as respostas seguem o padrão:
```json
{
  "success": true|false,
  "data": {...},
  "message": "Mensagem de sucesso ou erro",
  "error": "Código do erro (quando aplicável)"
}
```

## Endpoints por Categoria

### 📊 Contas (Accounts)
Gerenciamento de contas bancárias e financeiras.

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/accounts` | [Listar todas as contas](./accounts-get-all.mdx) |
| GET | `/accounts/{id}` | [Buscar conta por ID](./accounts-get-by-id.mdx) |
| POST | `/accounts` | [Criar nova conta](./accounts-create.mdx) |
| PUT | `/accounts/{id}` | [Atualizar conta](./accounts-update.mdx) |
| PUT | `/accounts/{id}/balance` | [Atualizar saldo da conta](./accounts-update-balance.mdx) |
| DELETE | `/accounts/{id}` | [Deletar conta](./accounts-delete.mdx) |
| GET | `/accounts/search` | [Buscar contas por nome](./accounts-search.mdx) |
| GET | `/accounts/total-balance` | [Saldo total das contas](./accounts-total-balance.mdx) |

### 🏷️ Categorias (Categories)
Gerenciamento de categorias para classificação de transações.

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/categories` | [Listar todas as categorias](./categories-get-all.mdx) |
| GET | `/categories/{id}` | [Buscar categoria por ID](./categories-get-by-id.mdx) |
| POST | `/categories` | [Criar nova categoria](./categories-create.mdx) |
| PUT | `/categories/{id}` | [Atualizar categoria](./categories-update.mdx) |
| DELETE | `/categories/{id}` | [Deletar categoria](./categories-delete.mdx) |
| GET | `/categories/search` | [Buscar categorias por nome](./categories-search.mdx) |

### 💰 Transações (Transactions)
Gerenciamento de transações financeiras.

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/transactions` | [Listar transações com filtros](./transactions-get-all.mdx) |
| GET | `/transactions/{id}` | [Buscar transação por ID](./transactions-get-by-id.mdx) |
| POST | `/transactions` | [Criar nova transação](./transactions-create.mdx) |
| PUT | `/transactions/{id}` | [Atualizar transação](./transactions-update.mdx) |
| DELETE | `/transactions/{id}` | [Deletar transação](./transactions-delete.mdx) |
| GET | `/transactions/recurring` | [Transações recorrentes](./transactions-recurring.mdx) |

### 🧪 Testes (Test)
Endpoints para verificação de saúde e diagnóstico.

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| GET | `/test` | [Teste de saúde da API](./test-health.mdx) |
| GET | `/test/health` | [Status detalhado da API](./test-health-detailed.mdx) |
| GET | `/test/database-check` | [Verificação do banco de dados](./test-database.mdx) |

## Tipos de Dados

### AccountDto
```json
{
  "id": "string",
  "name": "string",
  "type": "string",
  "balance": "BigDecimal",
  "currency": "string",
  "color": "string",
  "icon": "string"
}
```

### CategoryDto
```json
{
  "id": "string",
  "name": "string",
  "type": "string",
  "color": "string",
  "icon": "string",
  "budget": "BigDecimal",
  "spent": "BigDecimal"
}
```

### TransactionDto
```json
{
  "id": "string",
  "description": "string",
  "amount": "BigDecimal",
  "type": "string",
  "category": "string",
  "account": "string",
  "date": "LocalDate",
  "tags": ["string"],
  "recurring": "boolean",
  "recurringId": "string",
  "attachment": "string",
  "notes": "string",
  "createdAt": "LocalDateTime",
  "updatedAt": "LocalDateTime"
}
```

## Códigos de Status HTTP

| Código | Descrição |
|--------|-----------|
| 200 | OK - Sucesso |
| 201 | Created - Recurso criado |
| 400 | Bad Request - Dados inválidos |
| 404 | Not Found - Recurso não encontrado |
| 500 | Internal Server Error - Erro interno |

## Validações Comuns

### Campos Obrigatórios
- `userId` - ID do usuário (query parameter)
- `name` - Nome do recurso (máx 100 chars)
- `type` - Tipo do recurso
- `amount` - Valor (maior que zero)

### Tipos de Conta
- `CHECKING` - Conta Corrente
- `SAVINGS` - Poupança
- `CREDIT` - Cartão de Crédito
- `INVESTMENT` - Investimento
- `CASH` - Dinheiro em Espécie

### Tipos de Categoria/Transação
- `INCOME` - Receitas
- `EXPENSE` - Despesas

## Exemplos de Uso

### Criar uma Conta
```bash
curl -X POST "http://localhost:8080/accounts?userId=00000000-0000-0000-0000-000000000001" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Conta Corrente",
    "type": "CHECKING",
    "balance": 1500.00,
    "currency": "BRL",
    "color": "#3B82F6",
    "icon": "bank"
  }'
```

### Criar uma Transação
```bash
curl -X POST "http://localhost:8080/transactions?userId=00000000-0000-0000-0000-000000000001" \
  -H "Content-Type: application/json" \
  -d '{
    "description": "Supermercado",
    "amount": 150.50,
    "type": "EXPENSE",
    "category": "550e8400-e29b-41d4-a716-446655440001",
    "account": "550e8400-e29b-41d4-a716-446655440002",
    "date": "2024-01-15",
    "tags": ["alimentação", "supermercado"],
    "notes": "Compra do mês"
  }'
```

## Recursos Adicionais

- [Coleção Postman](./Finance-API-Postman-Collection.json)
- [Documentação Swagger](./swagger-ui.html)
- [Scripts de Teste](../test-*.ps1)

## Suporte

Para dúvidas ou problemas:
1. Verifique os endpoints de teste para diagnóstico
2. Consulte os logs da aplicação
3. Verifique a conectividade com o banco de dados
4. Confirme se todos os parâmetros obrigatórios estão sendo enviados 