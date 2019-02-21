# API

## alias (Customer SDK)

- POST /alias  
  Creates Alias:
  input -> Alias (JWT)

- PUT /alias/{id}  
  Exchange Alias: Alias -> Alias

## transaction (Merchant Backend / Dashboard)

- POST /transaction  
  Charge: Alias -> Transaction

- POST /transaction/{id}/refund  
  Refund: Id -> Refund

- POST /authorization  
  Authorize (just cc): Alias -> Authorization

- POST /authorization/{id}/reverse  
  Reverse: Authorization -> Reversal

- POST /authorization/{id}/capture  
  Capture: Authorization -> Transaction
