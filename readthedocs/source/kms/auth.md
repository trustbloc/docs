# Auth
KMS directly doesn't implement auth mechanism. It rely on [ORY Oathkeeper].

![mermaid-diagram-20220314150940](https://user-images.githubusercontent.com/62515092/158179173-1b4fddbc-912b-4d3a-9ae1-e84d3e04c0ab.png)

###ORY Oathkeeper
[ORY Oathkeeper] is an Identity & Access Proxy (IAP) and Access Control Decision API that authorizes HTTP requests based on sets of Access Rules.
[ORY Oathkeeper] is deployed in front of KMS service and is capable of authenticating and optionally authorizing access requests.

### Ory Hydra 
Ory Hydra is OpenID Certified OAuth 2.0 Server and OpenID Connect Provider.
Ory Hydra is not an identity provider (user sign up, user login, password reset flow), but connects to our existing identity provider through a login and consent app.

### Login and consent app

Login and consent app can be any OIDC identity provider

[ORY Oathkeeper]: https://github.com/ory/oathkeeper
[ORY Hydra]: https://github.com/ory/hydra

