# API

## alias (Customer SDK)

- POST /api/v1/alias  
  Creates Alias:
  input -> Alias  
  Header: publishable key

- PUT /api/v1/alias/{id}  
  Exchange Alias: Alias -> Alias  
  Header: publishable key

## transaction (Merchant Backend / Dashboard)

- PUT /api/v1/transaction  
  Header: secret key | Keycloak JWT Token, idempotentKey  
  Charge:  
  {aliasId, paymentData: {amount, currency, reason}, purchaseId, customerId}  
  -> {id, amount, currency, status, action}

- PUT /api/v1/transaction/{id}/refund  
  Header: secret key | Keycloak JWT Token, idempotentKey  
  Refund:  
  { reason, amount, currency }  
  -> {id, amount, currency, status, action}

- PUT /api/v1/authorization  
  Header: secret key | Keycloak JWT Token, idempotentKey  
  Authorize (just cc):  
  {aliasId, paymentData: {amount, currency, reason}, purchaseId, customerId}  
  -> {id, amount, currency, status, action}

- PUT /api/v1/authorization/{id}/reverse  
  Header: secret key | Keycloak JWT Token
  Reverse:  
  {reason}  
  -> {id, amount, currency, status, action}

- PUT /api/v1/authorization/{id}/capture  
  Header: secret key | Keycloak JWT Token
  Capture:  
  null  
  -> {id, amount, currency, status, action}

- DELETE /api/v1/alias/{id}  
  Header: secret key | Keycloak JWT Token
  Delete Alias:  
  null  
  -> { status }

Maybe Put the idempotentKey in the Url or Body, could make the usage easier, and calls can be POST again

## BaseData (Dashboard)

- GET /api/v1/merchant/{merchantId}  
  Header: Keycloak JWT Token  
  -> 200 { name, email, defaultCurrency } | 401 | 403 | 404

- PUT /api/v1/merchant/{merchantId}  
  Header: Keycloak JWT Token  
  { name, email, defaultCurrency }  
  -> 204 | 400 | 401 | 403 | 404

- PATCH /api/v1/merchant/{merchantId}  
  Header: Keycloak JWT Token  
  { name, email, defaultCurrency }  
  -> 204 | 400 | 401 | 403 | 404

- GET /api/v1/merchant/{merchantId}/api-key  
  Header: Keycloak JWT Token  
  -> 200 { data: [ { id, name, keyType }] } | 401 | 403 | 404

- POST /api/v1/merchant/{merchantId}/api-key  
  Header: Keycloak JWT Token  
  { name, keyType }  
  -> 201 { id, key } | 400 | 401 | 403

- GET /api/v1/merchant/{merchantId}/api-key/{apiKeyId}  
  Header: Keycloak JWT Token  
  -> 200 { id, name, keyType } | 401 | 403 | 404

- PATCH /api/v1/merchant/{merchantId}/api-key/{apiKeyId}  
  Header: Keycloak JWT Token  
  { name }  
  -> 204 | 400 | 401 | 403 | 404

- DELETE /api/v1/merchant/{merchantId}/api-key/{apiKeyId}  
  Header: Keycloak JWT Token  
  -> 204 | 401 | 403 | 404

- GET /api/v1/merchant/{merchantId}/psp  
  Header: Keycloak JWT Token  
  -> 200 { data: [ { config } ] } | 401 | 403

- POST /api/v1/merchant/{merchantId}/psp  
  Header: Keycloak JWT Token  
  { pspId, config }  
  -> 201 { pspId } | 400 | 401 | 403

- GET /api/v1/merchant/{merchantId}/psp/{pspId}  
  Header: Keycloak JWT Token  
  -> 200 { config } | 401 | 403 | 404

- PUT /api/v1/merchant/{merchantId}/psp/{pspId}  
  Header: Keycloak JWT Token  
  { config }  
  -> 204 | 401 | 403 | 404

## Transaction (Dashboard)

- GET /api/v1/transactions/{transactionId}  
  Header: Keycloak JWT Token  
  -> 200 { transactionId, pspId, paymentMethod, ccMask, ccExpiryDate, ccType, initialReason, currencyId, balance, merchantPurchaseId, merchantCustomerId, createdAt, refundedAt, chargebackAt, status, events: [ { action, status, amount, currency, reason, createdAt }] } | 401 | 403 | 404

- GET /api/v1/transactions  
  Header: Keycloak JWT Token  
  Query: { pspId, paymentMethod, ccMask, ccExpiryDate, ccExpiryDateStart, ccExpiryDateEnd, ccType, initialReason, currencyId, balanceStart, balanceEnd, merchantPurchaseId, merchantCustomerId, createdAtStart, createdAtEnd, status, refundedAtStart, refundedAtEnd, chargebackAtStart, chargebackAtEnd, hasEvent, hasNotEvent, offset, pageSize }  
  -> 200 { count, offset, pageSize, data: [ { ...TransactionDetail } ]}
