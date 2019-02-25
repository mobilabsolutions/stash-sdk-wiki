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

- POST /transaction  
  Header: secret key  
  Charge:  
  {aliasId, paymentData: {amount, currency, reason}, purchaseId, customerId}  
  -> {id, amount, currency, status, action}

- POST /transaction/{id}/refund  
  Header: secret key  
  Refund:  
  { reason }  
  -> {id, amount, currency, status, action}

- POST /authorization  
  Header: secret key  
  Authorize (just cc):  
  {aliasId, paymentData: {amount, currency, reason}, purchaseId, customerId}  
  -> {id, amount, currency, status, action}

- POST /authorization/{id}/reverse  
  Header: secret key  
  Reverse:  
  {reason}  
  -> {id, amount, currency, status, action}

- POST /authorization/{id}/capture  
  Header: secret key  
  Capture:  
  {}  
  -> {id, amount, currency, status, action}

- DELETE /alias/{id}  
  Header: secret key
  Delete Alias:  
  null  
  -> { status }
