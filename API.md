# API

## alias (Customer SDK)

- POST /alias  
  Creates Alias:
  input -> Alias  
  Header: publishable key

- PUT /alias/{id}  
  Exchange Alias: Alias -> Alias  
  Header: publishable key

## transaction (Merchant Backend / Dashboard)

- PUT /transaction  
  Header: secret key, idempotentKey  
  Charge:  
  {aliasId, paymentData: {amount, currency, reason}, purchaseId, customerId}  
  -> {id, amount, currency, status, action}

- PUT /transaction/{id}/refund  
  Header: secret key, idempotentKey  
  Refund:  
  { reason, amount, currency }  
  -> {id, amount, currency, status, action}

- PUT /authorization  
  Header: secret key, idempotentKey  
  Authorize (just cc):  
  {aliasId, paymentData: {amount, currency, reason}, purchaseId, customerId}  
  -> {id, amount, currency, status, action}

- PUT /authorization/{id}/reverse  
  Header: secret key  
  Reverse:  
  {reason}  
  -> {id, amount, currency, status, action}

- PUT /authorization/{id}/capture  
  Header: secret key  
  Capture:  
  {}  
  -> {id, amount, currency, status, action}

- DELETE /alias/{id}  
  Header: secret key
  Delete Alias:  
  null  
  -> { status }

## BaseData (Dashboard)

- GET /merchant/{merchantId}  
  Header: Keycloak JWT Token  
  -> 200 { name, email, defaultCurrency } | 401 | 403 | 404

- PUT/PATCH /merchant/{merchantId}  
  Header: Keycloak JWT Token  
  { name, email, defaultCurrency }  
  -> 204 | 400 | 401 | 403 | 404

- GET /merchant/{merchantId}/api-key  
  Header: Keycloak JWT Token  
  -> 200 { count, data: [ { id, name, keyType }] } | 401 | 403 | 404

- POST /merchant/{merchantId}/api-key  
  Header: Keycloak JWT Token  
  { name, keyType }  
  -> 201 { id, key } | 400 | 401 | 403

- GET /merchant/{merchantId}/api-key/{apiKeyId}  
  Header: Keycloak JWT Token  
  -> 200 { id, name, keyType } | 401 | 403 | 404

- PATCH /merchant/{merchantId}/api-key/{apiKeyId}  
  Header: Keycloak JWT Token  
  { name }  
  -> 204 | 400 | 401 | 403 | 404

- DELETE /merchant/{merchantId}/api-key/{apiKeyId}  
  Header: Keycloak JWT Token  
  -> 204 | 401 | 403 | 404

- GET /merchant/{merchantId}/psp  
  Header: Keycloak JWT Token  
  -> 200 { count, data: [ { config } ] } | 401 | 403

- POST /merchant/{merchantId}/psp  
  Header: Keycloak JWT Token  
  { pspId, config }  
  -> 201 { pspId } | 400 | 401 | 403

- GET /merchant/{merchantId}/psp/{pspId}  
  Header: Keycloak JWT Token  
  -> 200 { config } | 401 | 403 | 404

- PUT /merchant/{merchantId}/psp/{pspId}  
  Header: Keycloak JWT Token  
  { config }  
  -> 204 | 401 | 403 | 404
