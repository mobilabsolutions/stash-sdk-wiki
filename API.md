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
