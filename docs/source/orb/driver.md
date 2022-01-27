# Universal Resolver Driver
Orb Driver is used for [Universal Resolver](https://github.com/decentralized-identity/universal-resolver)

## API
``
https://driver-url/1.0/identifiers/<your-did>
``

Driver will receive an `Accept` header with the value `application/ld+json`, and it should return either a valid [DID Document](https://w3c-ccg.github.io/did-resolution/#output-diddocument) or a [DID Resolution Result](https://w3c-ccg.github.io/did-resolution/#output-didresolutionresult) 
in the HTTP body.Driver should also return an appropriate value in the `Content-Type` header, such as `application/did+ld+json`.


## Startup Parameters

### ORB_DRIVER_HOST_URL

URL to run the orb-driver instance on. Format: HostName:Port.

### ORB_DRIVER_TLS_CACERTS

Comma-Separated list of ca certs path.

### ORB_DRIVER_TLS_CERTIFICATE

TLS certificate for ORB driver.

### ORB_DRIVER_TLS_KEY

TLS key for ORB driver.

### ORB_DRIVER_DOMAIN

Discovery endpoint domain.

### ORB_DRIVER_SIDETREE_TOKEN

Authorization token for driver REST endpoints.
