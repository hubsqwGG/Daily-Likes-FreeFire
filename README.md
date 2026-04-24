# Frifas API - Send Likes

Este repositório contém a documentação e exemplos de uso para a API de envio automatizado de likes no Free Fire, utilizando o serviço central de likes.

## Visão Geral

A Frifas API permite que desenvolvedores integrem a funcionalidade de envio de likes para contas do Free Fire em suas aplicações. A API é projetada para ser simples e direta, fornecendo respostas claras sobre o status das operações.

## Base URL

`https://hubsdev.com/api/frifas/sendlikes?key=&id=`

## Parâmetros da Requisição

| Parâmetro | Tipo   | Obrigatório | Descrição                                        |
| :-------- | :----- | :---------- | :----------------------------------------------- |
| `key`     | String | Sim         | Chave de API para autenticação.                  |
| `id`      | String | Sim         | ID (UID) da conta do Free Fire para enviar likes. |

## Resposta de Sucesso (200 OK)

```json
{
  "sucesso": true,
  "status": "SUCESSO_LIKES",
  "mensagem": "likes enviados com sucesso!",
  "statuscode": 200,
  "data": [
    {
      "conta": {
        "nome_conta": "VEM_X1_BUT11",
        "id_conta": "11190213202",
        "region": "BR"
      },
      "likes": {
        "antes": 26637,
        "enviadas": 210,
        "depois": 26847
      },
      "external": {
        "contabilizado": true,
        "generated_id": "XXXX",
        "endpoind_id": "https://d2.hubsdev.com/check?id=XXXX"
      }
    }
  ]
}
```

## Tabela de Erros

| HTTP Status | Código de Erro      | Descrição                                                              |
| :---------- | :------------------ | :--------------------------------------------------------------------- |
| `400`       | `ID_REQUIRED`       | O parâmetro `id` não foi informado.                                    |
| `400`       | `INVALID_REQUEST`   | O UID fornecido é inválido.                                            |
| `404`       | `PLAYER_NOT_FOUND`  | Jogador não encontrado ou UID inexistente.                             |
| `409`       | `AUTOLIKE_ACTIVE`   | A conta está registrada no auto-like e não aceita likes manuais.       |
| `429`       | `REQUEST_BLOCKED`   | O UID já recebeu likes nas últimas 24 horas.                           |
| `502`       | `SERVICE_UNAVAILABLE` | Falha na comunicação com o serviço central de likes.                   |
| `502`       | `SERVICE_ERROR`     | Resposta inválida do serviço de likes.                                 |
| `503`       | `SERVICE_UNAVAILABLE` | Nenhuma conta disponível para enviar likes.                            |
| `504`       | `TIMEOUT`           | O tempo limite de 20 minutos para a requisição foi excedido.           |
