---
title: SmartWallet API

language_tabs:
  - Json

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors
  - mcc

search: true
---

# Introdução

Bem vindo ao SmartWallet!
Você pode utilizar API para acessar os recusos do serviço, com os quais é possivel criar, atualizar e consultar informações sobre uma conta, além de executar split de transações.


# Autenticação

A API do SmartWallet usa chaves que permitem o acesso aos recursos do serviço. 
A SmartWalletKey é o seu identificador único no serviço, e a chave de acesso deve ser enviada no cabeçalho a cada requisição feita o serviço. 

Exemplo:

`SmartWalletKey: E485075C-A77D-404A-A966-72AD93E1CB7D`

<aside class="notice">
Você deve substituir `E485075C-A77D-404A-A966-72AD93E1CB7D` com a sua chave pessoal.
</aside>

# Contas

## Criação de Conta

> Requisição usada pra criar uma conta.

```json
{
    "AccountReference": "GenericReference",
    "Addresses": [
        {
            "AddressNumber": "42",
            "City": "Rio de Janeiro",
            "Complement": "",
            "Country": "Brasil",
            "District": "",
            "State": "RJ",
            "StreetAddress": "GenericStreet",
            "ZipCode": "123465"
        }
    ],
    "DbaName": "Arthur",
    "DocumentNumber": "123456789",
    "DocumentType": "CNPJ",
    "Email": "exemple@exemple.com",
    "FinancialDetails": [
        {
            "Bank": {
                "AccountNumber": "000",
                "AgencyNumber": "123456",
                "BankCode": "789"
            },
            "Destination": "sample",
            "Email": "exemple@exemple.com"
        }
    ],
    "LegalName": "Arthur Dent",
    "MCC": 0042,
    "PhoneNumber": "9999999999",
    "RequestKey": "4636d485-583e-4f7d-a776-bdc4d9654950",
    "SmartWalletKey": "309d448f-246b-4f4e-8e97-dbfb771844fb",
    "SocialUserName": ""
}
```

> A requisição acima retorna uma resposta como essa:

```json
{
    "AccountKey": "f489675e-6e9d-49e5-bea3-17f38163e079",
    "AccountReference": "GenericReference",
    "Acquirer": {
        "AcquireCode": "123456",
        "AcquirerAffiliationKey": "321657498765413267987",
        "AcquirerName": "Generic"
    },
    "DbaName": "Arthur",
    "DocumentNumber": "123456789",
    "DocumentType": CNPJ,
    "Errors": [
        {
            "DocumentationUrl": "sample string",
            "ErrorCode": 1,
            "ErrorDescription": "sample string",
            "ParameterName": "sample string"
        }
    ],
    "LegalName": "Arthur Dent",
    "RequestKey": "0fb908dd-d489-46f0-86b5-482206be3047"
}
```

Esse recurso é utilizado para criar uma nova conta que é ligada a sua carteira virtural, que é representada a sua SmartWalletKey.

### HTTP Request

`POST https://smartwallet.mundipaggone.com/Account/`

### Parâmetros

Parâmetros | Descrição
--------- | -----------
AccountReference | Identificador da conta definido pelo seu sistema.
LegalName | Razão social, ou nome caso seja pessoa fisica.
DbaName | Nome fantasia da empresa.
DocumentNumber | Numero de documento.
DocumentType | Tipo de documento (CPNJ / CPF).
Email | Endereço de email da conta.
PhoneNumber | Numero de telefone para a conta.
Addresses | Coleção de endereços.
FinancialDetails | Detalhes financeiro para a conta.
RequestKey | Chave que identifica a requisição enviada para a API (Caso não seja enviado será definido pelo serviço).
MCC | Codigo MCC. Tabela com os códigos MCC pode ser encontrada no fim do documento.


<aside class="warning">
Na resposta de seviço de criação de contas encontra-se a **AccountKey**. Essa chave será usada pra todas as futuras operações relacionadas a essa conta, como atualização de informações e adição de crédito.

É necessário guardar essa informação, pois sem ela não será possível realizar nenhuma operação relacionada a essa conta.

Ex:

**"AccountKey": "ba1c2524-bd81-4f9d-8f97-673781768c59"**
</aside>

## Atualização de Conta

`PATCH https://smartwallet.mundipaggone.com/Account/{AccountKey}`

Esse recurso é utilizado para atualizar os dados de uma conta já existente. O objeto enviado na requisição é o mesmo utilizado na criação de conta.
Todos os campos que forem enviados subistituirão os dados já existentes. 

Ex: Se o desejado é atualizar somente a Razão social e o Nome Fantasia da conta, é possível enviar somente esses dados;

````json
{
    "DbaName": "Arthur Dent Updated",    
    "LegalName": "Arthur Dent Updated"
}
```

## Consultar dados de uma conta.

> O objeto retornado para esse recurso:

```json
{
    "AccountKey": "f0b1e74c-0798-47e4-838d-fe02c9cecc71",
    "LegalName": "Arthur Dent",
    "DbaName": "Arthur",
    "AccountReference": "Arthur",
    "DocumentNumber": "123456789",
    "DocumentType": "CPF",
    "RequestKey": "8CE94ECB-7D8B-4D6C-A341-CA2C259A9C5C",
    "Errors": null
}
```

`GET https://smartwallet.mundipaggone.com/Account/{AccountKey}`

Esse recurso retorna as informações basicas para uma conta especifica.

### Exemplo:
HTTP's GET para esse recurso utilizando a AccountKey **F0B1E74C-0798-47E4-838D-FE02C9CECC71**

# Transação

## Criando uma transação

> Objeto utilizado para criar uma transação

```json
{
    "CreditCardCollection": [
        {
            "Amount": 100,
            "CreditCardNumber": "123654889",
            "CreditCardType": "",
            "Cvv2": "528",
            "ExpirationDate": "2014-07-01",
            "HolderName": "Arthur Dent"
        }
    ],
    "CreditItemCollection": [
        {
            "AccountKey": "f0b1e74c-0798-47e4-838d-fe02c9cecc71",
            "Amount": 100,
            "CurrencyIsoCode": "BRL",
            "Description": "GenericItem",
            "FeeType": "Percent",
            "FeeValue": 10,
            "HoldTransaction": false,
            "ItemReference": "ItemReference123",
            "SplitTransaction": true,
            "LiabilityShift": "Client"
        }
    ],
    "Order": {
        "Amount": 100,
        "OrderDate": "2014-07-28",
        "OrderReference": "0123",
        "PaymentDate": "2014-07-28",
        "RefundDate": null
    },
    "RequestKey": "00000000-0000-0000-0000-000000000000"
}
```

> Exemplo de resposta para a requisição acima

```json
{
    "CreditFinancialMovementCollection": [
        {
            "ItemReference": "GenericItem",
            "AccountKey": "991aa261-9fe7-42c7-bbd7-8ef286c2d4df",
            "FinancialMovementKey": "ce5c421b-8690-411b-b984-7741abb5dc2c"
        }
    ],
    "RequestKey": "3797f66d-a465-4e84-ab09-c300b276a227",
    "Errors": null
}
```


### HTTP REQUEST
`POST https://smartwallet.mundipaggone.com/Transaction/`

### Parâmetros da requisição
Parametros | Descrição
--------- | -----------
CreditCardCollection | Coleção de cartões utilizados para pagar essa transação.
CreditItemCollection | Coleção de itens de crédito, cada item identifica uma transação para a conta referenciada.
Order (Optional) | Informações sobre o pedido.
RequestKey | Chave que identifica a requisição enviada para a API (Caso não seja enviado será definido pelo serviço).

### Parâmetros de cartão de crédito
Parametro | Descrição
--------- | -----------
Amount | Valor cobrado no cartão.
CrediCardNumber | Numero do cartão de credito.
CreditCardType | Banderia do cartão.
Cvv2 | Código de segurança do cartão de crédito.
ExpirationDate | Data de expiração do cartão.
HolderName | Nome do proprietário do cartão.

### Parâmetros de Créditos (Transações)
Parametro | Descrição
--------- | -----------
AccountKey | Chave que identifica a conta que será creditado o valor.
Amount | Valor que será creditado na conta.
CurrencyIsoCode | Código que representa a moeda que está sendo usada.
Description | Descrição do item.
FeeType | Define o tipo de cobrança. **Percent** or **Flat**
FeeValue | Valor da cobrança por essa transação.
HoldTransaction | Valor boleano que indica se a transação será bloqueada no momento da criação ou não.
ItemReference | Código de referencia do item.
SplitTransaction | Valor boleano que define se vai ser feito o Split da transação entre a conta recebendo o crédito e a conta dona da carteira.
LiabilityShift | Explicado na tabela abaixo *

### Parâmetros do pedido
Parametro | Descrição
--------- | -----------
Amount | Valor total do pedido.
OrderDate | Data do pedido.
OrderReference | Identificador do pedido (Ex: numero do pedido).
PaymentDate | Data que o pedido foi feito.
RefundDate | Data de estorno do pedido.


### LiabilityShift
Enviando o campo de LiabilityShift é possivel definir de que forma o estorno será feito.
Existem dois valores possíveis:

Parametro | Descrição
--------- | -----------
Client | Quando o estorno é feito pelo cliente tanto a MasterAccount quanto SubMerchantAccount tem os valores estornados de suas conta.
Partner | Quanto o pedido de estorno é feito por umas das contas da carteira, e por isso o cliente vai ser afetado, nesse será feito o estorno porem o SubMerchant continuará pagando os valores da comissão da venda.

O envio desse campo é opicional e caso não seja enviado o valor padrão é **Client**

<aside class="warning">
O objeto de resposta de criação de transações contém o **FinancialMovementKey**. Essa chave identifica a transção e será utilizada para as futuras opreções realacionadas a essa transação.

É necessário guardar essa informação, pois sem ela não será possível realizar nenhuma operação relacionada a essa transação.

**"FinancialMovementKey": "ba1c2524-bd81-4f9d-8f97-673781768c59"**
</aside>

## Estornar uma transação
### HTTP REQUEST

`POST https://smartwallet.mundipaggone.com/Transaction/{financialMovmentKey}`

Esse recurso é utilizado para estornoar uma transação.

## Atualizar (liberar) uma transação
### HTTP REQUEST
> Exemplo de atualização.

```json
{
    "RequestKey": "00000000-0000-0000-0000-000000000000", 
    "Status": "Used"
}
```
`PATCH https://smartwallet.mundipaggone.com/Transaction/{financialMovmentKey}`

Quanto uma transção é criada existe a opção de bloquear a transação e seus valores não serão contabilizados no totais de uma conta. Após isso é possivel atualizar a transação para dois possíveis valores:

Parâmetro | Descrição
----------|------------
Used | Quanto o status é definido para Usado o valores são liberados e somados os totais das contas.
Expired | Quanto o status é definido para expirado o valor total da transação é adicionado ao MasterAccount da carteira.
