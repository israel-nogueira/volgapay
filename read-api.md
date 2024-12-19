# Documentação da API Volgapay

## Introdução

Esta documentação descreve os endpoints e funcionalidades da API de pagamento da Volgapay, permitindo integração para processamento de pagamentos via cartão de crédito e Pix.

## Autenticação

### Login

- **Método**: POST  
- **Endpoint**: `https://api.volgapay.com/auth/login`  
- **Descrição**: Obtém token de autenticação para utilizar os demais endpoints

#### Requisição
```json
{
    "client_id": "18784b0-a784-11ef-afda-f43909454c09",
    "client_secret": "9c11d08075924c9bee6f382dcf554070614afdde2a107ae52ad8af9c74424019"
}
```

#### Resposta de Sucesso
```json
{
    "status": "success",
    "message": "Login successful",
    "token": "TOKEN_JWT",
    "token_type": "Bearer",
    "expires_at": "2025-01-11T01:53:11.000Z"
}
```

### Tabela de Campos Requeridos

| Campo            | Tipo   | Descrição              | Obrigatório |
|------------------|--------|------------------------|--------------|
| `client_id`      | string | Identificador do cliente fornecido pela Volgapay | Sim          |
| `client_secret`  | string | Chave secreta do cliente para autenticação       | Sim          |

## 🚀 Exemplo de Uso

### Autenticação

**Python:**
```python
import requests

login_data = {
    "client_id": "seu_client_id",
    "client_secret": "seu_client_secret"
}
response = requests.post("https://api.volgapay.com/auth/login", json=login_data)
token = response.json()["token"]
```

**Node.js:**
```javascript
const axios = require('axios');

const loginData = {
    client_id: 'seu_client_id',
    client_secret: 'seu_client_secret'
};

axios.post('https://api.volgapay.com/auth/login', loginData)
    .then(response => {
        const token = response.data.token;
        console.log(token);
    })
    .catch(error => {
        console.error(error);
    });
```

**PHP:**
```php
<?php

$loginData = array(
    "client_id" => "seu_client_id",
    "client_secret" => "seu_client_secret"
);

$options = array(
    "http" => array(
        "header"  => "Content-type: application/json\r\n",
        "method"  => "POST",
        "content" => json_encode($loginData),
    ),
);

$context  = stream_context_create($options);
$result = file_get_contents("https://api.volgapay.com/auth/login", false, $context);
$response = json_decode($result, true);

echo $response['token'];
?>
```

# Adicionar Cartão de Crédito

## Descrição
Este endpoint permite vincular um cartão de crédito a um cliente cadastrado na plataforma Volgapay.

- **Método**: POST  
- **Endpoint**: `https://api.volgapay.com/card/add`  
- **Autenticação**: Bearer YOUR_ACCESS_TOKEN (TOKEN JWT recebido no login)

---

## Requisição

```json
{
    "customer_id": "2525",
    "first_name": "First Name",
    "last_name": "Last Name",
    "address": "Address",    
    "country": "US",
    "state": "NY",
    "city": "New York",
    "zip": "38564",
    "email": "juca@bala.com",
    "country_code": "+99",
    "phone": "8401286548",
    "card_no": "4242424242424242",
    "ccExpiryMonth": "02",
    "ccExpiryYear": "2026",
    "cvvNumber": "123"
}
```

---

## Tabela de Campos Requeridos

| Campo              | Tipo   | Descrição                           | Obrigatório |
|--------------------|--------|---------------------------------------|--------------|
| `customer_id`      | string | Identificador do cliente              | Sim          |
| `first_name`       | string | Nome do titular do cartão             | Sim          |
| `last_name`        | string | Sobrenome do titular do cartão        | Sim          |
| `address`          | string | Endereço do titular                   | Sim          |
| `country`          | string | País do titular                       | Sim          |
| `state`            | string | Estado do titular                     | Sim          |
| `city`             | string | Cidade do titular                     | Sim          |
| `zip`              | string | Código postal                         | Sim          |
| `email`            | string | Email do titular                      | Sim          |
| `country_code`     | string | Código do país                        | Sim          |
| `phone`            | string | Número de telefone                    | Sim          |
| `card_no`          | string | Número do cartão de crédito           | Sim          |
| `ccExpiryMonth`    | string | Mês de expiração do cartão            | Sim          |
| `ccExpiryYear`     | string | Ano de expiração do cartão            | Sim          |
| `cvvNumber`        | string | Código de segurança do cartão (CVV)   | Sim          |

---

## Resposta de Sucesso

```json
{
    "status": "success",
    "message": "Card successfully added/updated.",
    "data": {
        "card_hash": "9a7a9956c7a427ff502e373a9a5217d66f432f59826e7492ad88a71f19dbbe9c",
        "last4_digits": "4242",
        "card_brand": "Visa"
    }
}
```

---

## 🚀 Exemplo de Uso

**Python:**
```python
import requests

url = "https://api.volgapay.com/card/add"
headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
}
card_data = {
    "customer_id": "2525",
    "first_name": "First Name",
    "last_name": "Last Name",
    "address": "Address",
    "country": "US",
    "state": "NY",
    "city": "New York",
    "zip": "38564",
    "email": "juca@bala.com",
    "country_code": "+99",
    "phone": "8401286548",
    "card_no": "4242424242424242",
    "ccExpiryMonth": "02",
    "ccExpiryYear": "2026",
    "cvvNumber": "123"
}
response = requests.post(url, headers=headers, json=card_data)
print(response.json())
```

**Node.js:**
```javascript
const axios = require('axios');

const url = "https://api.volgapay.com/card/add";
const headers = {
    Authorization: "Bearer YOUR_ACCESS_TOKEN"
};
const cardData = {
    customer_id: "2525",
    first_name: "First Name",
    last_name: "Last Name",
    address: "Address",
    country: "US",
    state: "NY",
    city: "New York",
    zip: "38564",
    email: "juca@bala.com",
    country_code: "+99",
    phone: "8401286548",
    card_no: "4242424242424242",
    ccExpiryMonth: "02",
    ccExpiryYear: "2026",
    cvvNumber: "123"
};

axios.post(url, cardData, { headers }).then(response => {
    console.log(response.data);
}).catch(error => {
    console.error(error);
});
```

**PHP:**
```php
<?php

$url = "https://api.volgapay.com/card/add";
$cardData = array(
    "customer_id" => "2525",
    "first_name" => "First Name",
    "last_name" => "Last Name",
    "address" => "Address",
    "country" => "US",
    "state" => "NY",
    "city" => "New York",
    "zip" => "38564",
    "email" => "juca@bala.com",
    "country_code" => "+99",
    "phone" => "8401286548",
    "card_no" => "4242424242424242",
    "ccExpiryMonth" => "02",
    "ccExpiryYear" => "2026",
    "cvvNumber" => "123"
);

$options = array(
    "http" => array(
        "header"  => "Content-type: application/json\r\nAuthorization: Bearer YOUR_ACCESS_TOKEN\r\n",
        "method"  => "POST",
        "content" => json_encode($cardData),
    ),
);

$context  = stream_context_create($options);
$result = file_get_contents($url, false, $context);
$response = json_decode($result, true);

print_r($response);
?>
```


# Deletar Cartão de Crédito

## Descrição
Este endpoint permite remover um cartão de crédito vinculado a um cliente cadastrado na plataforma Volgapay.

- **Método**: POST  
- **Endpoint**: `https://api.volgapay.com/card/delete`  
- **Autenticação**: Bearer YOUR_ACCESS_TOKEN (TOKEN JWT recebido no login)

---

## Requisição

```json
{
    "customer_id": "78544646461235",
    "card_hash": "662944e7e5e12185e157ce34d447015dbc363348daa41a6cf409a543df026da1"
}
```

---

## Tabela de Campos Requeridos

| Campo              | Tipo   | Descrição                          | Obrigatório |
|--------------------|--------|--------------------------------------|--------------|
| `customer_id`      | string | Identificador do cliente             | Sim          |
| `card_hash`        | string | Hash único do cartão a ser removido   | Sim          |

---

## Resposta de Sucesso

```json
{
    "status": "success",
    "message": "Card successfully marked as deleted.",
    "data": {
        "card_hash": "662944e7e5e12185e157ce34d447015dbc363348daa41a6cf409a543df026da1",
        "updated_rows": 1
    }
}
```

---

## 🚀 Exemplo de Uso

**Python:**
```python
import requests

url = "https://api.volgapay.com/card/delete"
headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
}
card_data = {
    "customer_id": "78544646461235",
    "card_hash": "662944e7e5e12185e157ce34d447015dbc363348daa41a6cf409a543df026da1"
}
response = requests.post(url, headers=headers, json=card_data)
print(response.json())
```

**Node.js:**
```javascript
const axios = require('axios');

const url = "https://api.volgapay.com/card/delete";
const headers = {
    Authorization: "Bearer YOUR_ACCESS_TOKEN"
};
const cardData = {
    customer_id: "78544646461235",
    card_hash: "662944e7e5e12185e157ce34d447015dbc363348daa41a6cf409a543df026da1"
};

axios.post(url, cardData, { headers }).then(response => {
    console.log(response.data);
}).catch(error => {
    console.error(error);
});
```

**PHP:**
```php
<?php

$url = "https://api.volgapay.com/card/delete";
$cardData = array(
    "customer_id" => "78544646461235",
    "card_hash" => "662944e7e5e12185e157ce34d447015dbc363348daa41a6cf409a543df026da1"
);

$options = array(
    "http" => array(
        "header"  => "Content-type: application/json\r\nAuthorization: Bearer YOUR_ACCESS_TOKEN\r\n",
        "method"  => "POST",
        "content" => json_encode($cardData),
    ),
);

$context  = stream_context_create($options);
$result = file_get_contents($url, false, $context);
$response = json_decode($result, true);

print_r($response);
?>
```

# Listar Cartões de Crédito

## Descrição
Este endpoint permite listar os cartões de crédito vinculados a um cliente cadastrado na plataforma Volgapay.

- **Método**: GET  
- **Endpoint**: `https://api.volgapay.com/card/list`  
- **Autenticação**: Bearer YOUR_ACCESS_TOKEN (TOKEN JWT recebido no login)

---

## Requisição

```json
{
    "customer_id": "2525"
}
```

---

## Tabela de Campos Requeridos

| Campo              | Tipo   | Descrição                          | Obrigatório |
|--------------------|--------|--------------------------------------|--------------|
| `customer_id`      | string | Identificador do cliente             | Sim          |

---

## Resposta de Sucesso

```json
{
    "status": "success",
    "customer_id": "2525",
    "data": [
        {
            "card_hash": "29495a7961aff16dc2446db35cb6b6f63bb52f81b84dace1f245806ac060f87a",
            "last4_digits": "3184",
            "card_brand": "Visa"
        }
    ]
}
```

---

## 🚀 Exemplo de Uso

**Python:**
```python
import requests

url = "https://api.volgapay.com/card/list"
headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
}
card_data = {
    "customer_id": "2525"
}
response = requests.get(url, headers=headers, json=card_data)
print(response.json())
```

**Node.js:**
```javascript
const axios = require('axios');

const url = "https://api.volgapay.com/card/list";
const headers = {
    Authorization: "Bearer YOUR_ACCESS_TOKEN"
};
const cardData = {
    customer_id: "2525"
};

axios.get(url, { headers, params: cardData }).then(response => {
    console.log(response.data);
}).catch(error => {
    console.error(error);
});
```

**PHP:**
```php
<?php

$url = "https://api.volgapay.com/card/list";
$cardData = array(
    "customer_id" => "2525"
);

$options = array(
    "http" => array(
        "header"  => "Content-type: application/json\r\nAuthorization: Bearer YOUR_ACCESS_TOKEN\r\n",
        "method"  => "GET",
        "content" => json_encode($cardData),
    ),
);

$context  = stream_context_create($options);
$result = file_get_contents($url, false, $context);
$response = json_decode($result, true);

print_r($response);
?>
```

# Enviar Pagamento de Cartão de Crédito

## Descrição
Este endpoint permite processar pagamentos com cartão de crédito, incluindo suporte para transações com ou sem redirecionamento 3D Secure.

- **Método**: POST  
- **Endpoint**: `https://api.volgapay.com/payment/credit-card`  
- **Autenticação**: Bearer YOUR_ACCESS_TOKEN (TOKEN JWT recebido no login)

---

## Requisição

```json
{
    "customer_order_id": "ORDER-006",
    "customer_id": "2525",
    "amount": "20",
    "currency": "USD",
    "card_hash": "9a7a9956c77427ff502e373a8a5217d66f432f59826e7492ad88a71f19dbbe9c",
    "cvvNumber": "107"
}
```

---

## Tabela de Campos Requeridos

| Campo                | Tipo   | Descrição                          | Obrigatório |
|----------------------|--------|--------------------------------------|--------------|
| `customer_order_id`  | string | Identificador exclusivo do pedido   | Sim          |
| `customer_id`        | string | Identificador do cliente            | Sim          |
| `amount`             | string | Valor da transação (formato decimal) | Sim          |
| `currency`           | string | Moeda do pagamento (ISO 4217)      | Sim          |
| `card_hash`          | string | Hash exclusivo do cartão de crédito  | Sim          |
| `cvvNumber`          | string | Código de segurança do cartão         | Sim          |

---

## Respostas de Sucesso

### Sem Redirecionamento 3D Secure

```json
{
    "status": "success",
    "message": "Your transaction has been processed successfully.",
    "redirect_url": null,
    "customer_order_id": "ORDER-008",
    "customer_id": "2525",
    "transaction_hash": "af1c2181d4529c6e054d279e1d610b4b404e54f5d0a81bff97963f60cd5cf740",
    "card_nr": "424242XXXXXX4242",
    "card_hash": "9a7a9956c77427ff502e373a8a5217d66f432f59826e7492ad88a71f19dbbe9c",
    "amount": "20.00",
    "currency": "USD",
    "brand": "Visa",
    "token_validation": "ZDFjNzg0YjAtYTc4NC0xMWVmLWFmZGEtZPQzOTA5NDU0YzA5OjljMDFkMDgwNzU5MjRjOWJlZTZ8MzgyZGNmNTU0MDcwNjE0YWZkZGUyYTEwN2FlNTJhZDhhZjljNzQ0MjQwMTk="
}
```

### Com Redirecionamento 3D Secure

```json
{
    "status": "3d_redirect",
    "message": "3DS link generated successfully, please redirect to 'redirect_3ds_url'.",
    "redirect_url": "https://dashboard.charge.money/payment/test-transaction/IMXH1733970358",
    "customer_order_id": "ORDER-5001",
    "customer_id": "2525",
    "transaction_hash": "f3e2ff059ccc603e401c75ddc4514e0f6c9b474a7b12063ca901cbfd268afb69",
    "card_nr": "400000XXXXXX3184",
    "card_hash": "19495a9961aff16dc2446db35cb6b6f63bb52f81b84dace1f245806ac060f87a",
    "amount": "15.00",
    "currency": "USD",
    "brand": "Visa",
    "token_validation": "ZDFjNzg0YjAtYTc4NC0xMWVmLWFmZGEtZPQzOTA5NDU0YzA5OjljMDFkMDgwNzU5MjRjOWJlZTZ8MzgyZGNmNTU0MDcwNjE0YWZkZGUyYTEwN2FlNTJhZDhhZjljNzQ0MjQwMTk="
}
```

---

## 🚀 Exemplo de Uso

**Python:**
```python
import requests

url = "https://api.volgapay.com/payment/credit-card"
headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
}
payment_data = {
    "customer_order_id": "ORDER-006",
    "customer_id": "2525",
    "amount": "20",
    "currency": "USD",
    "card_hash": "9a7a9956c77427ff502e373a8a5217d66f432f59826e7492ad88a71f19dbbe9c",
    "cvvNumber": "107"
}
response = requests.post(url, headers=headers, json=payment_data)
print(response.json())
```

**Node.js:**
```javascript
const axios = require('axios');

const url = "https://api.volgapay.com/payment/credit-card";
const headers = {
    Authorization: "Bearer YOUR_ACCESS_TOKEN"
};
const paymentData = {
    customer_order_id: "ORDER-006",
    customer_id: "2525",
    amount: "20",
    currency: "USD",
    card_hash: "9a7a9956c77427ff502e373a8a5217d66f432f59826e7492ad88a71f19dbbe9c",
    cvvNumber: "107"
};

axios.post(url, paymentData, { headers }).then(response => {
    console.log(response.data);
}).catch(error => {
    console.error(error);
});
```

**PHP:**
```php
<?php

$url = "https://api.volgapay.com/payment/credit-card";
$paymentData = array(
    "customer_order_id" => "ORDER-006",
    "customer_id" => "2525",
    "amount" => "20",
    "currency" => "USD",
    "card_hash" => "9a7a9956c77427ff502e373a8a5217d66f432f59826e7492ad88a71f19dbbe9c",
    "cvvNumber" => "107"
);

$options = array(
    "http" => array(
        "header"  => "Content-type: application/json\r\nAuthorization: Bearer YOUR_ACCESS_TOKEN\r\n",
        "method"  => "POST",
        "content" => json_encode($paymentData),
    ),
);

$context  = stream_context_create($options);
$result = file_get_contents($url, false, $context);
$response = json_decode($result, true);

print_r($response);
?>
```

---

## Webhook de Retorno

O **webhook** será enviado ao endpoint configurado pelo cliente, conforme o status da transação.

### Exemplo de Webhook - Status: `pending`

```json
{
   "status": "pending",
   "message": "Payment pending approval.",
   "transaction_hash": "41ec3ba37e7b3f4d8b0c458e6d5360aa626522c5ca632ed2c819de0943d36f06",
   "customer_id": "2525",
   "customer_order_id": "009",
   "amount": "22.00",
   "currency": "BRL",
   "card_hash": "9a7a9956c7a427ff502e373a8a5217d66f432f59826e7492ad88a71f19dbbe9c",
   "token_validation": "YjViZjcxYjliMGZjYTE0NjlkNjVhZTI2MTE0NjhmNTg4NTk3NDg5ZTQyNTFhOTQ5NGZiNzNmZTIxMzI4Yzg4MDphNTIxZDVjMGJlMDEwZGI2MDllYzRkNTYwMTVhZDM4NDViZTZjZjE1ZDQyNjZkMjkzOTkyZTMzZDMwN2JkMzk3"
}
```

### Exemplo de Webhook - Status: `success`

```json
{
   "status": "success",
   "message": "Your transaction has been processed successfully.",
   "amount": "20.00",
   "currency": "USD",
   "email": "juca@bala.com",
   "transaction_hash": "bcfe4b5802019fd08b20bf8491bf33fed49dc231b05fe4e4977f87c2af61219c",
   "card_no": "424242XXXXXX4242",
   "card_hash": "9a7a9956c7a427ff502e373a8a5217d66f432f59826e7492ad88a71f19dbbe9c",
   "customer_order_id": "008",
   "customer_id": "2525",
   "token_validation": "ZDFjNzg0YjAtYTc4NC0xMWVmLWFmZGEtZjQzOTA5NDU0YzA5OjljMDFkMDgwNzU5MjRjOWJlZTZmMzgyZGNmNTU0MDcwNjE0YWZkZGUyYTEwN2FlNTJhZDhhZjljNzQ0MjQwMTk="
}
```


## Campos de Retorno do Webhook

Abaixo está a tabela com os campos retornados nos webhooks e suas descrições:

| **Campo**             | **Descrição**                                                                                           |
|-----------------------|---------------------------------------------------------------------------------------------------------|
| `status`              | O status da transação (ex: `pending`, `success`).                                                      |
| `message`             | Mensagem descrevendo o estado da transação (ex: "Payment pending approval", "Transaction processed successfully"). |
| `transaction_hash`    | Identificador único da transação.                                                                       |
| `customer_id`         | Identificador do cliente.                                                                               |
| `customer_order_id`   | Identificador do pedido do cliente.                                                                     |
| `amount`              | Valor da transação.                                                                                     |
| `currency`            | Moeda da transação (ex: `BRL`, `USD`).                                                                  |
| `card_hash`           | Hash do cartão de crédito utilizado.                                                                   |
| `token_validation`    | String codificada em Base64 contendo o `client_id` e `client_secret` concatenados.                     |
| `email`               | E-mail do cliente associado à transação (apenas em transações com status `success`).                    |
| `card_no`             | Número do cartão de crédito parcialmente mascarado (ex: `424242XXXXXX4242`).                           |

---


### Campo `token_validation`

O campo **`token_validation`** é uma string codificada em **Base64** que contém os dados **`client_id`** e **`client_secret`** concatenados. Esta codificação é utilizada para garantir que as credenciais da API sejam recebidas de forma segura. O formato do `token_validation` é:

```
Base64(client_id:client_secret)
```

#### Exemplo:

- **`client_id`**: `your_client_id`
- **`client_secret`**: `your_client_secret`

A codificação em Base64 de **`your_client_id:your_client_secret`** seria:

```
eW91cl9jbGllZW50X2lkOnlvdXJfY2xpZW50X3NlY3JldA==
```

Portanto, a string **`token_validation`** para esses dados seria:

```json
"token_validation": "eW91cl9jbGllZW50X2lkOnlvdXJfY2xpZW50X3NlY3JldA=="
```



---

# Enviar Pagamento com Pix

## Descrição
Este endpoint permite processar pagamentos via Pix, fornecendo um código de pagamento e QR Code para a transação.

- **Método**: POST  
- **Endpoint**: `https://api.volgapay.com/payment/pix`  
- **Autenticação**: Bearer YOUR_ACCESS_TOKEN (TOKEN JWT recebido no login)

---

## Requisição

```json
{
    "customer_id": "1113",
    "amount": "21.0",
    "customer_order_id": "1514"
}
```

---

## Tabela de Campos Requeridos

| Campo               | Tipo   | Descrição                                    | Obrigatório |
|---------------------|--------|----------------------------------------------|-------------|
| `customer_id`       | string | Identificador do cliente                     | Sim         |
| `amount`            | string | Valor da transação (formato decimal)         | Sim         |
| `customer_order_id` | string | Identificador exclusivo do pedido            | Sim         |

---

## Respostas de Sucesso

```json
{
    "status": "pending",
    "message": "Transaction was pending state.",
    "customer_order_id": "1514",
    "customer_id": "1113",
    "transaction_hash": "29083fa157742a3edec8c9d40b7dcb42bd62c42ace6232e6922282d189a3b2b3",
    "txid": "Z6NH1J6JBYOPWD",
    "amount": "21.00",
    "copy_and_paste": "00020126840014br.gov.bcb.pix2562qrcode.transfeera.com/cob/d1d4a664-3869-4746-a4c1-3787a55e89a75204000053039865802BR5925DOBANK TECNOLOGIA EM PAGA6012SAO PAULO SP62070503***63044BE8",
    "qrcode": "iVBORw0KGgoAAAANSUhEUWNR7WWtd4WGtd42GtdY2HtdY1HtZa13hYa13j/wDPgtfaEoReCgAAAABJRU5ErkJggg=="
}
```

---

## 🚀 Exemplo de Uso

**Python:**
```python
import requests

url = "https://api.volgapay.com/payment/pix"
headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
}
payment_data = {
    "customer_id": "1113",
    "amount": "21.0",
    "customer_order_id": "1514"
}
response = requests.post(url, headers=headers, json=payment_data)
print(response.json())
```

**Node.js:**
```javascript
const axios = require('axios');

const url = "https://api.volgapay.com/payment/pix";
const headers = {
    Authorization: "Bearer YOUR_ACCESS_TOKEN"
};
const paymentData = {
    customer_id: "1113",
    amount: "21.0",
    customer_order_id: "1514"
};

axios.post(url, paymentData, { headers }).then(response => {
    console.log(response.data);
}).catch(error => {
    console.error(error);
});
```

**PHP:**
```php
<?php

$url = "https://api.volgapay.com/payment/pix";
$paymentData = array(
    "customer_id" => "1113",
    "amount" => "21.0",
    "customer_order_id" => "1514"
);

$options = array(
    "http" => array(
        "header"  => "Content-type: application/json\r\nAuthorization: Bearer YOUR_ACCESS_TOKEN\r\n",
        "method"  => "POST",
        "content" => json_encode($paymentData),
    ),
);

$context  = stream_context_create($options);
$result = file_get_contents($url, false, $context);
$response = json_decode($result, true);

print_r($response);
?>
```

---

# Obter Detalhes da Transação

## Descrição
Este endpoint permite obter os detalhes de uma transação específica, dado o seu identificador (`transaction_hash`).

- **Método**: POST  
- **Endpoint**: `https://api.volgapay.com/payment/transaction-details`  
- **Autenticação**: Bearer YOUR_ACCESS_TOKEN (TOKEN JWT recebido no login)

---

## Requisição

```json
{
    "transaction_hash": "41ec3ba37e7b3f4d8b0c458e6d5360aa626522c5ca632ed2c819de0943d36f06"
}
```

---

## Tabela de Campos Requeridos

| Campo              | Tipo   | Descrição                                            | Obrigatório |
|--------------------|--------|------------------------------------------------------|-------------|
| `transaction_hash` | string | Identificador único da transação.                    | Sim         |

---

## Resposta de Sucesso

```json
{
    "status": "success",
    "message": "Your transaction has been processed successfully.",
    "customer_order_id": "009",
    "customer_id": "2525",
    "transaction_hash": "41ec3ba37e7b3f4d8b0c458e6d5360aa626522c5ca632ed2c819de0943d36f06",
    "transaction_date": "2024-12-18T13:05:30.000Z",
    "ip_address": "200.140.255.102",
    "amount": "22.00",
    "currency": "BRL",
    "card_hash": "9a7a9956c7a427ff502e373a8a5217d66f432f59826e7492ad88a71f19dbbe9c",
    "card_xx": "424242XXXXXX4242",
    "redirect_url": null
}
```

---

## 🚀 Exemplo de Uso

**Python:**
```python
import requests

url = "https://api.volgapay.com/payment/transaction-details"
headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN"
}
transaction_data = {
    "transaction_hash": "41ec3ba37e7b3f4d8b0c458e6d5360aa626522c5ca632ed2c819de0943d36f06"
}
response = requests.post(url, headers=headers, json=transaction_data)
print(response.json())
```

**Node.js:**
```javascript
const axios = require('axios');

const url = "https://api.volgapay.com/payment/transaction-details";
const headers = {
    Authorization: "Bearer YOUR_ACCESS_TOKEN"
};
const transactionData = {
    transaction_hash: "41ec3ba37e7b3f4d8b0c458e6d5360aa626522c5ca632ed2c819de0943d36f06"
};

axios.post(url, transactionData, { headers }).then(response => {
    console.log(response.data);
}).catch(error => {
    console.error(error);
});
```

**PHP:**
```php
<?php

$url = "https://api.volgapay.com/payment/transaction-details";
$transactionData = array(
    "transaction_hash" => "41ec3ba37e7b3f4d8b0c458e6d5360aa626522c5ca632ed2c819de0943d36f06"
);

$options = array(
    "http" => array(
        "header"  => "Content-type: application/json\r\nAuthorization: Bearer YOUR_ACCESS_TOKEN\r\n",
        "method"  => "POST",
        "content" => json_encode($transactionData),
    ),
);

$context  = stream_context_create($options);
$result = file_get_contents($url, false, $context);
$response = json_decode($result, true);

print_r($response);
?>
```

---

## Explicação dos Campos de Resposta

| Campo              | Descrição                                                |
|--------------------|----------------------------------------------------------|
| `status`           | O status da transação (ex: `success`).                   |
| `message`          | Mensagem informando o resultado da requisição.           |
| `customer_order_id`| Identificador do pedido do cliente.                      |
| `customer_id`      | Identificador do cliente associado à transação.          |
| `transaction_hash` | Identificador único da transação.                        |
| `transaction_date` | Data e hora em que a transação foi realizada (ISO 8601). |
| `ip_address`       | O endereço IP do cliente que realizou a transação.       |
| `amount`           | O valor da transação.                                    |
| `currency`         | A moeda da transação (ex: `BRL`, `USD`).                |
| `card_hash`        | O hash do cartão de crédito utilizado.                   |
| `card_xx`          | O número do cartão de crédito, parcialmente mascarado (ex: `424242XXXXXX4242`). |
| `redirect_url`     | Caso haja algum redirecionamento, o URL para o qual o cliente será enviado. |

---
