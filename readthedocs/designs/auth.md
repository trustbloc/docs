# TrustBloc Autorization

This is a WIP design document for TrustBloc Authorization.

## Overview

The TrustBloc plans to use [[IETF-DRAFT] Grant Negotiation and Authorization Protocol (GNAP)](https://www.ietf.org/archive/id/draft-ietf-gnap-core-protocol-09.html) to authorize interactions between different components.

## Sequence Diagram

```mermaid
sequenceDiagram
    autonumber
    actor user as User
    participant wallet_web as Wallet Web Browser <br>(Vue+WASM)
    participant auth as Hub Auth
    participant oidc as External OIDC Provider
    participant rs as KMS/EDV

    loop SignUp/SignIn
    user ->> wallet_web : go to wallet website
    wallet_web ->> wallet_web : create keypair
    wallet_web ->> auth : HTTP POST GNAP grant request
    auth ->> wallet_web : HTTP Response GNAP grant response with interact-redirect \n(to display list of OIDC providers)
    wallet_web ->> auth : HTTP Redirect
    auth ->> user : display list of OIDC providers
    user ->> auth : select oidc provider
    auth ->> oidc : HTTP Redirect oidc /auth
    oidc ->> user : show login prompt
    user ->> oidc : user enters login/password
    oidc ->> auth : HTTP Redirect odic /callback
    auth ->> oidc : HTTP POST oidc /token
    oidc ->> auth : HTTP Response id_token, accessToken
    auth ->> wallet_web : HTTP Redirect GNAP interact_ref
    wallet_web ->> auth : HTTP POST /continue with GNAP interact_ref
    auth ->> wallet_web : HTTP Response with access_token
    end

    loop KMS/EDV invocation
    wallet_web ->> wallet_web : Create HTTP req and compute httpsign
    wallet_web ->> rs : HTTP /kms/{keystoreID}/keys with HTTPSign and GNAP access_token
    rs ->> auth : HTTP POST /introspect
    auth ->> rs : HTTP Response clientKey and subID
    rs ->> rs : validate httpsig
    rs ->> rs : process user request
    rs ->> wallet_web : HTTP response
    end
```

## Wallet Auth Logic

```mermaid
flowchart TD
    A[User requests to access some wallet page] --> B{Does this page require the user to be authenticated?}
    B -->|No| C[Redirect the user to the requested path]
    C --> Z[Finish]
    B -->|Yes| D{Do we have username in vuex?}
    D -->|Yes| F{Do we have agent initialized?}
    D -->|No| E{Do we have username in local storage?}
    F -->|Yes| C
    F -->|No| R["Fetch bootstrap data"]

    E -->|Yes| F{Do we have agent initialized?}
    E -->|No| I["Request access with GNAP key pair"]
    I --> L{Does auth server return access token and subject id?}
    L -->|Yes| F
    L -->|No| M["Make a call to continue API"]
    M --> P["Receive access token and subject id"]
    P --> R
    R --> H[Initialize agent]
    H --> W["Fetch bootstrap data"]
    W --> X["Unlock wallet"]
    X --> C
```
