# VDR
VDR used to manage DID operation.

## New
New will return new instance of Orb VDR.

``
func New(keyRetriever KeyRetriever, opts ...Option) (*VDR, error)
``

### Key Retriever interface
Key Retriever is used to manage operation keys.

#### GetNextRecoveryPublicKey
GetNextRecoveryPublicKey is called in recover DID to get the recover next public key. This public key will be used to 
verify next recover request (the client need to use the private key to sign next recover request).

``
GetNextRecoveryPublicKey(didID, commitment string) (crypto.PublicKey, error)
``

#### GetNextUpdatePublicKey
GetNextUpdatePublicKey is called in update and recover DID to get the update next public key. This public key will be used to
verify next update request (the client need to use the private key to sign next update request).

``
GetNextUpdatePublicKey(didID, commitment string) (crypto.PublicKey, error)
``

#### GetSigningKey
GetSigningKey is called in update,recover and deactivate DID to get the private key. This private key will be used to
sign update,recover and deactivate request.

OperationType update need private key for update DID request.
OperationType recover need private key for recover or deactivate DID request.

``
GetSigningKey(didID string, ot OperationType, commitment string) (crypto.PrivateKey, error)
``

### New options
VDR New method options.

#### WithHTTPClient
WithHTTPClient option is for custom http client.

```
WithHTTPClient(httpClient *http.Client) Option
```

#### WithTLSConfig
WithTLSConfig option is for definition of secured HTTP transport using a tls.Config instance.

```
func WithTLSConfig(tlsConfig *tls.Config) Option 
```

#### WithUnanchoredMaxLifeTime
WithUnanchoredMaxLifeTime option is max time for unanchored to be trusted.

```
func WithUnanchoredMaxLifeTime(duration time.Duration) Option
```

#### WithVerifyResolutionResultType
WithVerifyResolutionResultType option is set verify resolution result type.

VerifyResolutionResultType Types:
* All: will not trust server and verify provided resolution result from server against resolution result that is assembled
  from published (DID anchored) and unpublished (DID not anchored yet) operations.
* Unpublished: will not trust server and verify provided resolution result from server against resolution result that is assembled
  from unpublished operations (DID not anchored yet).
* None: will trust server and not verify document.

```
func WithVerifyResolutionResultType(v VerifyResolutionResultType) Option 
```

#### WithAuthToken
WithAuthToken option add auth token.

```
func WithAuthToken(authToken string) Option
```

#### WithDomain
WithDomain option add Orb domains that vdr will them to communicate.

To add multiple domains you need to call this option once for each domain.

```
func WithDomain(domain string) Option
```

#### WithDocumentLoader
WithDocumentLoader option overrides the default JSONLD document loader used when processing JSONLD DID documents.

```
func WithDocumentLoader(l jsonld.DocumentLoader) Option
```

## Create
Create used to create new Orb DID.

``
func Create(did *docdid.Doc, opts ...vdrapi.DIDMethodOption) (*docdid.DocResolution, error)
``

## Read
Read used to resolve Orb DID.

``
func Read(did string, opts ...vdrapi.DIDMethodOption) (*docdid.DocResolution, error)
``

## Update
Update used to update or recover Orb DID.

``
func Update(didDoc *docdid.Doc, opts ...vdrapi.DIDMethodOption) error
``

## Deactivate
Deactivate used to deactivate Orb DID.

``
func Deactivate(didID string, opts ...vdrapi.DIDMethodOption) error
``
