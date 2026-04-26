# Frifas API - Documentação Técnica Completa

> Sistema centralizado para automação de likes e gerenciamento de ciclos de auto-like no Free Fire.

A Frifas API oferece uma infraestrutura robusta para desenvolvedores que buscam integrar funcionalidades de engajamento automatizado. Esta documentação descreve detalhadamente os endpoints, payloads de resposta e tratamentos de erro.

---

## Informações Gerais

*   **Base URL:** `https://hubsdev.com/api/frifas`
*   **Formato de Dados:** JSON
*   **Ciclo de Auto-Like:** Processamento diário às 13:00 BRT.

---

## 1. Send Likes
Envia likes manuais para um UID específico.

**Endpoint:** `GET /sendlikes?key={KEY}&id={UID}`

### Resposta de Sucesso (200 OK)
```json
{
  "sucesso": true,
  "status": "SUCESSO_LIKES",
  "mensagem": "likes enviados com sucesso!",
  "statuscode": 200,
  "data": [
    {
      "conta": {
        "nome_conta": "XXXXX",
        "id_conta": "XXXXX",
        "region": "XX"
      },
      "likes": {
        "antes": XXXXX,
        "enviadas": XXXXX,
        "depois": XXXXX
      },
      "external": {
        "contabilizado": true,
        "min_likes_required": 150,
        "generated_id": "XXXXX",
        "endpoind_id": "https://d2.hubsdev.com/check?id=XXXXX"
      }
    }
  ]
}
```

### Tabela de Erros
| HTTP | Código | Descrição |
| :--- | :--- | :--- |
| 400 | `ID_REQUIRED` | O parâmetro id não foi informado |
| 400 | `INVALID_REQUEST` | O UID fornecido é inválido |
| 401 | `INVALID_KEY` | Chave de API não fornecida ou inválida |
| 403 | `KEY_BANNED` | Chave banida por violar os termos de uso |
| 403 | `EXPIRED_KEY` | Chave expirada |
| 404 | `PLAYER_NOT_FOUND` | Jogador não encontrado ou UID inexistente |
| 409 | `AUTOLIKE_ACTIVE` | Conta registrada no auto-like |
| 429 | `REQUEST_BLOCKED` | O UID já recebeu likes nas últimas 24h |
| 429 | `DAILY_LIMIT_EXCEEDED` | Limite diário de requisições atingido |
| 502 | `SERVICE_UNAVAILABLE` | Falha na comunicação com o serviço central |
| 504 | `TIMEOUT` | Tempo limite de 20 minutos excedido |

---

## 2. List Open
Lista todas as contas registradas em um open de auto-like.

**Endpoint:** `GET /list-open?key={KEY}&access_id={ACCESS_ID}`

### Resposta de Sucesso (200 OK)
```json
{
  "sucesso": true,
  "status": "SUCESSO_LIST_OPEN",
  "mensagem": "list-open consultado com sucesso!",
  "statuscode": 200,
  "data": [
    {
      "conta": {
        "uid": "XXXXX",
        "player": "XXXXX",
        "region": "XX",
        "level": XX
      },
      "likes": {
        "inicial": XXXXX,
        "atual": XXXXX,
        "enviados": XXXXX
      },
      "progresso": {
        "dias_total": XX,
        "dias_usados": XX,
        "dias_restantes": XX,
        "porcentagem": XX,
        "concluido": false
      },
      "datas": {
        "registrado_em": "XXXX-XX-XX XX:XX:XX",
        "ultimo_envio_em": "XXXX-XX-XX XX:XX:XX",
        "proximo_envio_em": "XXXX-XX-XX XX:XX:XX"
      }
    }
  ]
}
```

### Tabela de Erros
| HTTP | Código | Descrição |
| :--- | :--- | :--- |
| 400 | `ACCESS_ID_REQUIRED` | O parâmetro access_id não foi informado |
| 400 | `INVALID_ACCESS_ID` | O access_id fornecido é inválido |
| 401 | `INVALID_KEY` | Chave de API não fornecida ou inválida |
| 404 | `ACCESS_ID_NOT_FOUND` | access_id não encontrado |
| 504 | `TIMEOUT` | Tempo limite de 30 segundos excedido |

---

## 3. Info Open
Retorna informações gerais do open de auto-like.

**Endpoint:** `GET /info-open?key={KEY}&access_id={ACCESS_ID}`

### Resposta de Sucesso (200 OK)
```json
{
  "sucesso": true,
  "status": "SUCESSO_INFO_OPEN",
  "mensagem": "info-open consultado com sucesso!",
  "statuscode": 200,
  "data": {
    "access_id": "XXXXX",
    "max_contas": XX,
    "max_dias": XX,
    "contas_registradas": XX,
    "contas_ativas": XX,
    "contas_concluidas": XX,
    "contas_restantes": XX,
    "created_at_brt": "XXXX-XX-XX XX:XX:XX",
    "checkpage_url": "https://d2.hubsdev.com/checkpage?access_id=XXXXX",
    "webhook": {
      "url": "https://XXXXX.com/webhook",
      "enabled": true
    }
  }
}
```

### Tabela de Erros
| HTTP | Código | Descrição |
| :--- | :--- | :--- |
| 400 | `ACCESS_ID_REQUIRED` | O parâmetro access_id não foi informado |
| 404 | `ACCESS_ID_NOT_FOUND` | access_id não encontrado |
| 502 | `SERVICE_UNAVAILABLE` | Falha na comunicação com o serviço |

---

## 4. Add Auto
Registra uma conta no sistema de auto-like.

**Endpoint:** `GET /add-auto?key={KEY}&id={UID}&dias={DIAS}&open={OPEN_ID}`

### Resposta de Sucesso (200 OK)
```json
{
  "sucesso": true,
  "status": "SUCESSO_ADD_AUTO",
  "mensagem": "conta registrada no auto-like com sucesso!",
  "statuscode": 200,
  "data": {
    "conta": {
      "uid": "XXXXX",
      "player": "XXXXX",
      "region": "XX",
      "level": XX
    },
    "likes": {
      "antes": XXXXX,
      "depois": XXXXX,
      "enviados": XXXXX,
      "inicial": XXXXX
    },
    "progresso": {
      "dias_total": XX,
      "dias_usados": XX,
      "dias_restantes": XX,
      "dia_consumido": true
    },
    "datas": {
      "proximo_envio_em": "XXXX-XX-XX XX:XX:XX"
    }
  }
}
```

### Tabela de Erros
| HTTP | Código | Descrição |
| :--- | :--- | :--- |
| 400 | `ID_REQUIRED` | O parâmetro id não foi informado |
| 400 | `DIAS_REQUIRED` | O parâmetro dias não foi informado |
| 400 | `OPEN_REQUIRED` | O parâmetro open não foi informado |
| 400 | `DIAS_EXCEEDS_OPEN_LIMIT` | Dias excede o limite do open |
| 404 | `OPEN_NOT_FOUND` | Open não encontrado |
| 409 | `ACCOUNT_ALREADY_IN_AUTOLIKE` | Conta já registrada em auto-like |
| 409 | `OPEN_FULL` | Open atingiu limite de contas |

---

## Webhooks
Notificações automáticas via POST (JSON) enviadas após o ciclo diário.

### Payload: Finalizado (`finished`)
```json
{
  "completed": true,
  "total_success": XX,
  "total_incomplete": XX,
  "total_accounts": XX,
  "status": "finished",
  "message": "Process completed successfully"
}
```

### Payload: Falha (`failed`)
```json
{
  "completed": false,
  "total_success": 0,
  "total_incomplete": XX,
  "status": "failed",
  "message": "No likes were sent to any account today"
}
```

---

## Regras de Negócio

*   **Contabilização:** Uma requisição só é debitada do limite diário se o sistema confirmar o envio de **150 ou mais likes**.
*   **Timeout:** O tempo limite para operações de envio manual é de **20 minutos**.
*   **Webhooks:** O servidor de destino deve responder em até **10 segundos**.

---
*Frifas API Service - Documentação Oficial*
