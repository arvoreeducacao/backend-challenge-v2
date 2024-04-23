# Eventos de gamificação

Queremos implementar um sistema que processa eventos do sistema de gamificação da Árvore. Nele cada usuário tem uma "conta" com 3 "currencies" diferentes: water_drops, coins e points.

Eventos podem ser recebidos para adicionar novos valores (**amount_received**), usar valores existentes na conta (**amount_used**) e pra transferir valores entre usuários diferentes (**transfer_requested**).


Implemente um sistema que receba via stdin eventos no formato abaixo e que pra cada evento recebido retorne o resultado no stdout.

```
[
  {
    "event": "amount_received",
    "user_id": 123,
    "currency": "coins",
    "amount": 110,
    "created_at": "2024-01-01T00:00:00Z"
  },
  {
    "event": "amount_used",
    "user_id": 123,
    "currency": "coins",
    "amount": 10,
    "created_at": "2024-01-01T01Z00:01:00Z"
  },
  {
    "event": "transfer_requested",
    "origin_user_id": 123,
    "destination_user_id": 456,
    "currency": "coins",
    "amount": 99,
    "created_at": "2024-01-01T01Z00:02:00Z"
  }
]
```

Se a operação foi bem sucedida o resultado deve conter os dados da conta dos usuários envolvidos na transação:

```
{
  "user_id": 123,
  "balances": {
    "coins": 0,
    "water_drops": 100,
    "points": 50
  }
}
```

No caso da transferência envolve dois usuários, então o resultado seria assim:

```
{
  "user_balances": [
    {
      "user_id": 123,
      "balances": {
        "coins": 1,
        "water_drops": 100,
        "points": 50
      }
    },
    {
      "user_id": 456,
      "balances": {
        "coins": 99,
        "water_drops": 100,
        "points": 50
      }
    }
  ]
}
```

Se a operação não foi bem sucedida devemos retornar stdout um erro no formato abaixo contendo um código com a descrição do que aconteceu. Por exemplo:

```
{
  "error": "user_has_insufficient_funds"
}
```

## Regras de fraude

O sistema também precisa obedecer duas regras para evitar fraudes:

- Não podem haver mais de 3 ou mais eventos "amount_used" para um mesmo usuário num intervalo de 1 minuto
- Não podem haver mais de 3 ou mais transferências (evento "transfer_requested") de um mesmo usuário num intervalo de 10 minutos

Se uma operação disparar uma dessas regras de fraude o retorno da operação deve reportar um erro que identifique a fraude ocorrida.


## Requisitos

- O balanço/valor de uma conta nunca deve ser negativo.
- Os dados devem ser mantidos em memória. Não deve ser usado banco de dados. O estado do programa vai ser calculado a cada execução e perdido no final dela.
- Os eventos devem ser recebidos via stdin e não via um arquivo como parâmetro do programa/script.
- Escreva testes como se esse fosse um código real de produção, incluindo testes unitários e de integração.
- Apesar do código ser pequeno, tente separar os códigos de acordo com suas responsabilidades, evitando todo o código estar em um único módulo/namespace/classe.
- Valide os inputs e a operação e retorne erros de acordo com essas validações.
- Escreva um README comentando as principais decisões técnicas e explicando como rodar o programa
