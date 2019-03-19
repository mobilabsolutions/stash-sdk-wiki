# Adyen Integration

## Merchant Account

1. Adyen supports multiple [accounts](https://docs.adyen.com/developers/user-management/company-and-merchant-accounts#createmerchantaccount) (company and merchants), and there are test and live customer areas.
2. For merchant account configuration, we will probably need:
    * Merchant id
    * [API key](https://docs.adyen.com/developers/user-management/how-to-get-the-api-key) 
    * [Web service user password](https://docs.adyen.com/developers/user-management/how-to-get-the-web-service-ws-user-password)
    * [Origin key](https://docs.adyen.com/developers/user-management/how-to-get-an-origin-key)
    

## Payment Method Registration

- With Adyen it is possible to [create token](https://docs.adyen.com/developers/features/tokenization/creating-tokens) - save the payment method. Payment request should be sent to Adyen, but with an amount of 0, which means that the card information will only be validated, without a charge.
- We need to send shopperReference (customerId in our case), and paymentMethod.storeDetails set to true to Adyen.

## Making Transactions

[Payments](https://docs.adyen.com/developers/features/tokenization/making-payments-with-tokens) can be made with the saved token, and shopperReference (customerId).


## API Design

### Alias

- POST /api/v1/alias  
  Header: publishable key
  Creates Alias:  
  null
  -> 201 { aliasId }

- PUT /api/v1/alias/{id}  
  Header: publishable key  
  Add Data to Alias:  
  { pspAlias, email, extra: { customerId, ccMask, ccExpiry, ccType, IBAN... } }  
  -> 204 | 400 | 404
  
### Transaction

- PUT /api/v1/transaction  
  Header: secret key | JWT Token, idempotentKey  
  Charge:  
  {aliasId, paymentData: {amount, currency, reason}, purchaseId, customerId}  
  -> 200 | 201 {id, amount, currency, status, action} | 400 | 401 | 404