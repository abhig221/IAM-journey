# Authentication vs Authorization

## Authentication
Authentication is the process of verifying that the entity — which 
could be a person, company, service account, API, etc. — is what it 
claims to be by making sure its information corresponds with a 
password, fingerprint, or hardware ID. Once authenticated, the server 
checks what level of access they have and limits or grants it accordingly.

## Identity claims
An identity claim is what or who the entity claims to be when trying 
to get authenticated.

## Tokens
Tokens, specifically JWT (JSON Web Token), are pieces of data that 
store the signature of who got authenticated along with identity 
claims and attributes. They are stored client-side.

## Sessions
Sessions are created once authentication has been completed. They 
allow the user to access what they need while the session is active. 
The server checks each action the user takes to verify they are who 
they say they are.

## Access decisions
Access decisions are the process of granting or limiting access based 
on what the entity is allowed to have, determined by their attributes 
and claims.

## Questions to revisit
- How exactly does the server use attributes inside a token to make 
  an access decision?
