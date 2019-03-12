# PayPal Vault Integration

We would like to have PayPal Vault integration, and that's possible by using:

- The [Braintree SDK](https://developers.braintreepayments.com/start/hello-client/android/v2) for clients and backend
- [PayPal Vault](https://developers.braintreepayments.com/guides/paypal/vault/android/v2) for storing and reusing PayPal payment information

## PayPal Vault Flow

1. **Client**: Initializes the SDK if necessary - Request Braintree client token from the **Backend** or initializes Braintree with [tokenization key](https://developers.braintreepayments.com/guides/authorization/tokenization-key/android/v2) 
2. **Client**: Redirects to the PayPal login, and user enters PayPal credentials
3. **Client**: Creates a billing agreement, and sends the billing agreement id to the **Backend**
4. **Client**: Receives a payment method nonce from Braintree, and sends it to the **Backend**
5. **Client**: Collects device data, and sends it to the **Backend**
6. **Backend**: [Creates](https://developers.braintreepayments.com/guides/payment-methods/java#create) a payment method based on payment method nonce, and gets the payment token (alias) from Braintree

## PayPal Transaction Flow

1. **Backend**: [Creates](https://developers.braintreepayments.com/guides/paypal/server-side/java) a transaction at Braintree

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
  { pspAlias, email, extra: { billingAgreementId, paymentMethodNonce, deviceData } }  
  -> 204 | 400 | 404

### Transaction

- PUT /api/v1/transaction  
  Header: secret key | JWT Token, idempotentKey  
  Charge:  
  {aliasId, paymentData: {amount, currency, reason}, purchaseId, customerId}  
  -> 200 | 201 {id, amount, currency, status, action} | 400 | 401 | 404



