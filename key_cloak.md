# Keycloak

https://www.keycloak.org/

Keycloak can help us as a "Identity and Access Management" for the Dashboard

## Functionality it provides (and we need):

- Create Account
- Login
- Reset Password
- Logout
- Change Password
- Change Email

One Disadvantage is has, it is not really designed for having MULTI Merchant Support, and the UI they provide can not be fully integrated.

So we have to work around it, like dynamically create groups for each merchant.

## ToDos for integration:

- Build User Creation/Update flow in our Backend, that Creates/Update the Accounts on Keycloak site.
- Build a UI for it
- Style / change Emails Keycloak is sending
- Store all other User/Account data in our Database

What Keycloak is NOT providing is any kind of Private / Public Key authorization. This has to be developed by us.

## Other Option

would be to implement these functions our own, because we just need a very limited amount of the functions Keycloak is providing.

## Opinion

All other Dashboard functionalities are independent of if we use Keycloak or not.

I (Christoph) personally tend to the other Option, but it is only an opinion. We could change it later to the one or other direction. I guess the effort is about the same.
