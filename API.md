# API

## alias (Customer SDK)

- POST /api/v1/alias  
  Header: publishable key
  Creates Alias:  
  null  
  -> 201 { aliasId }

- PUT /api/v1/alias/{id}  
  Header: publishable key  
  Add Data to Alias:  
  { pspAlias, extra: { ccMask, ccExpiry, ccType, ibanMask, ...} }  
  -> 204 | 400

## transaction (Merchant Backend / Dashboard)

- PUT /api/v1/transaction  
  Header: secret key | JWT Token, idempotentKey  
  Charge:  
  {aliasId, paymentData: {amount, currency, reason}, purchaseId, customerId}  
  -> 200 | 201 {id, amount, currency, status, action} | 400 | 401 | 404

- PUT /api/v1/transaction/{id}/refund  
  Header: secret key | JWT Token, idempotentKey  
  Refund:  
  { reason, amount, currency }  
  -> 200 | 201 {id, amount, currency, status, action} | 400 | 401 | 404

- PUT /api/v1/authorization  
  Header: secret key | JWT Token, idempotentKey  
  Authorize (just cc):  
  {aliasId, paymentData: {amount, currency, reason}, purchaseId, customerId}  
  -> 200 | 201 {id, amount, currency, status, action} | 400 | 401 | 404

- PUT /api/v1/authorization/{id}/reverse  
  Header: secret key | JWT Token  
  Reverse:  
  {reason}  
  -> 200 {id, amount, currency, status, action} | 400 | 401 | 404

- PUT /api/v1/authorization/{id}/capture  
  Header: secret key | JWT Token  
  Capture:  
  null  
  -> 200 {id, amount, currency, status, action} | 400 | 401 | 404

- DELETE /api/v1/alias/{id}  
  Header: secret key | JWT Token  
  Delete Alias:  
  null  
  -> 204 | 401 | 404

Maybe Put the idempotentKey in the Url or Body, could make the usage easier, and calls can be POST again

## BaseData (Dashboard)

- GET /api/v1/merchant/{merchantId}  
  Header: JWT Token  
  -> 200 { name, email, defaultCurrency } | 401 | 403 | 404

- PUT /api/v1/merchant/{merchantId}  
  Header: JWT Token  
  { name, email, defaultCurrency }  
  -> 204 | 400 | 401 | 403 | 404

- PATCH /api/v1/merchant/{merchantId}  
  Header: JWT Token  
  { name, email, defaultCurrency }  
  -> 204 | 400 | 401 | 403 | 404

- GET /api/v1/merchant/{merchantId}/api-key  
  Header: JWT Token  
  -> 200 { data: [ { id, name, keyType }] } | 401 | 403 | 404

- POST /api/v1/merchant/{merchantId}/api-key  
  Header: JWT Token  
  { name, keyType }  
  -> 201 { id, key } | 400 | 401 | 403

- GET /api/v1/merchant/{merchantId}/api-key/{apiKeyId}  
  Header: JWT Token  
  -> 200 { id, name, keyType } | 401 | 403 | 404

- PATCH /api/v1/merchant/{merchantId}/api-key/{apiKeyId}  
  Header: JWT Token  
  { name }  
  -> 204 | 400 | 401 | 403 | 404

- DELETE /api/v1/merchant/{merchantId}/api-key/{apiKeyId}  
  Header: JWT Token  
  -> 204 | 401 | 403 | 404

- GET /api/v1/merchant/{merchantId}/psp  
  Header: JWT Token  
  -> 200 { data: [ { config } ] } | 401 | 403

- POST /api/v1/merchant/{merchantId}/psp  
  Header: JWT Token  
  { pspId, config }  
  -> 201 { pspId } | 400 | 401 | 403

- GET /api/v1/merchant/{merchantId}/psp/{pspId}  
  Header: JWT Token  
  -> 200 { config } | 401 | 403 | 404

- PUT /api/v1/merchant/{merchantId}/psp/{pspId}  
  Header: JWT Token  
  { config }  
  -> 204 | 401 | 403 | 404

## Transaction (Dashboard)

- GET /api/v1/transactions/{transactionId}  
  Header: JWT Token  
  -> 200 { transactionId, pspId, paymentMethodId, ccMask, ccExpiryDate, ccType, ibanMask, initialReason, currencyId, balance, merchantPurchaseId, merchantCustomerId, createdAt, refundedAt, chargebackAt, status, events: [ { action, status, amount, currency, reason, createdAt }] } | 401 | 403 | 404

- GET /api/v1/transactions  
  Header: JWT Token  
  Query: { pspId, paymentMethodId, ccMask, ccExpiryDate, ccExpiryDateStart, ccExpiryDateEnd, ccType, ibanMask, initialReason, currencyId, balanceStart, balanceEnd, merchantPurchaseId, merchantCustomerId, createdAtStart, createdAtEnd, status, refundedAtStart, refundedAtEnd, chargebackAtStart, chargebackAtEnd, hasEvent, hasNotEvent, offset, pageSize }  
  -> 200 { count, offset, pageSize, data: [ { ...TransactionDetail } ]} | 401 | 403

## User Management

- POST /api/v1/token  
  { username, password }  
  -> 200 { accessToken: 5MinJWTToken, refreshToken: 24hJWTToken }  
  (MerchantId and UserId is in the Token, refreshToken can only be used for refresh)

- POST /api/v1/token/refresh  
  Header: JWT Token (Refresh)  
  null  
  -> 200 { accessToken: 5MinJWTToken, refreshToken: 24hJWTToken } | 401 | 403

- PUT /api/v1/user/{userId}  
  Header: JWT Token  
  { firstName, lastName }  
  -> 204 | 400 | 401 | 403 | 404

- PUT /api/v1/user/{userId}/password  
  Header: JWT Token  
  { oldPassword, newPassword }  
  -> 204 | 400 | 401 | 403 | 404

- POST /api/v1/user/{userName|email}/reset-password  
  { newPassword, changeToken }  
  -> 204 | 404?  
  (does not ha)

- POST /api/v1/user/{userName|email}/forgot-password  
  null  
  -> 204 | 404?  
  (does not ha)
