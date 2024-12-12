
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

## Gestão de Cartões

### Adicionar Cartão de Crédito

- **Método**: POST
- **Endpoint**: `https://api.volgapay.com/card/add`
- **Autenticação**: Bearer Token
- **Descrição**: Vincula um cartão de crédito a um cliente

#### Requisição
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

#### Resposta de Sucesso
```json
{
    "status": "success",
    "message": "Card successfully added/updated.",
    "data": {
        "card_hash": "CARD_HASH",
        "last4_digits": "4242",
        "card_brand": "Visa"
    }
}
```

### Deletar Cartão de Crédito

- **Método**: POST
- **Endpoint**: `https://api.volgapay.com/card/delete`
- **Autenticação**: Bearer Token
- **Descrição**: Remove um cartão de crédito de um cliente

#### Requisição
```json
{
    "customer_id": "78544646461235",
    "card_hash": "CARD_HASH"
}
```

#### Resposta de Sucesso
```json
{
    "status": "success",
    "message": "Card successfully marked as deleted.",
    "data": {
        "card_hash": "CARD_HASH",
        "updated_rows": 1
    }
}
```

### Listar Cartões de Crédito

- **Método**: GET
- **Endpoint**: `https://api.volgapay.com/card/list`
- **Autenticação**: Bearer Token
- **Descrição**: Lista os cartões de crédito de um cliente

#### Requisição
```json
{
    "customer_id": "2525"
}
```

#### Resposta de Sucesso
```json
{
    "status": "success",
    "customer_id": "2525",
    "data": [
        {
            "card_hash": "CARD_HASH",
            "last4_digits": "3184",
            "card_brand": "Visa"
        }
    ]
}
```

## Processamento de Pagamentos

### Pagamento com Cartão de Crédito

- **Método**: POST
- **Endpoint**: `https://api.volgapay.com/payment/credit-card`
- **Autenticação**: Bearer Token
- **Descrição**: Processa pagamento via cartão de crédito

#### Requisição
```json
{
    "customer_order_id": "ORDER-006",
    "customer_id": "2525",
    "amount": "20",
    "currency": "USD",
    "card_hash": "CARD_HASH",
    "cvvNumber": "107" 
}
```

#### Resposta de Sucesso (Sem 3D Secure)
```json
{
    "status": "success",
    "message": "Your transaction has been processed successfully.",
    "customer_order_id": "ORDER-008",
    "transaction_hash": "TRANSACTION_HASH",
    "amount": "20.00",
    "currency": "USD"
}
```

#### Resposta de Sucesso (Com 3D Secure)
```json
{
    "status": "3d_redirect",
    "message": "3DS link generated successfully, please redirect to 'redirect_3ds_url'.",
    "redirect_url": "https://dashboard.charge.money/payment/test-transaction/IMXH1733970158",
    "customer_order_id": "ORDER-5001",
    "customer_id": "2525",
    "transaction_hash": "TRANSACTION_HASH",
    "card_nr": "400000XXXXXX3184",
    "card_hash": "CARD_HASH",
    "amount": "15.00",
    "currency": "USD",
    "brand": "Visa",
    "token_validation": "TOKEN_VALIDATION_HASH"
}

```

### Pagamento com Pix

- **Método**: POST
- **Endpoint**: `https://api.volgapay.com/payment/pix`
- **Autenticação**: Bearer Token
- **Descrição**: Gera um pagamento Pix

#### Requisição
```json
{
  "customer_id": "1113",
  "amount": 21.0,
  "customer_order_id": "1514"
}
```

#### Resposta de Sucesso
```json
{
    "status": "pending",
    "message": "Transaction was pending state.",
    "txid": "Z6NH1J6JBYOPWD",
    "amount": "21.00",
    "copy_and_paste": "PIX_COPY_PASTE_CODE",
    "qrcode": "BASE64_ENCODED_QRCODE"
}
```

## Considerações

- Sempre utilize o token JWT obtido no endpoint de login
- Verifique os códigos de status para tratamento de erros
- Mantenha o CVV do cartão confidencial
- Em transações com 3D Secure, redirecione o usuário para a URL fornecida

## Suporte

Para suporte adicional, entre em contato com a equipe da Volgapay através dos canais oficiais.

---
