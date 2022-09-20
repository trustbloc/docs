# Verifiable Data Registry (VDR)

The VDR is a Go library that may be used by clients to manage DID operation.

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

### Options 
New options.

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
- ***All***: Will not trust server and verify provided resolution result from server against resolution result that is assembled
  from published (DID anchored) and unpublished (DID not anchored yet) operations.
- ***Unpublished***: Will not trust server and verify provided resolution result from server against resolution result that is assembled
  from unpublished operations (DID not anchored yet).
- ***None***: Will trust server and not verify document.

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

### Example

```
import (
	"crypto"
	"github.com/hyperledger/aries-framework-go-ext/component/vdr/orb"
)

type keyRetrieverImpl struct {
	nextRecoveryPublicKey crypto.PublicKey
	nextUpdatePublicKey   crypto.PublicKey
	updateKey             crypto.PrivateKey
	recoverKey            crypto.PrivateKey
}

func (k *keyRetrieverImpl) GetNextRecoveryPublicKey(didID string) (crypto.PublicKey, error) {
	return k.nextRecoveryPublicKey, nil
}

func (k *keyRetrieverImpl) GetNextUpdatePublicKey(didID string) (crypto.PublicKey, error) {
	return k.nextUpdatePublicKey, nil
}

func (k *keyRetrieverImpl) GetSigningKey(didID string, ot orb.OperationType) (crypto.PrivateKey, error) {
	if ot == orb.Update {
		return k.updateKey, nil
	}

	return k.recoverKey, nil
}


keyRetrieverImpl := &keyRetrieverImpl{}

vdr, err := orb.New(keyRetrieverImpl, orb.WithDomain("https://testnet.devel.trustbloc.dev"))
	if err != nil {
		return err
}
```

## Create
Create used to create new Orb DID.

``
func Create(did *docdid.Doc, opts ...vdrapi.DIDMethodOption) (*docdid.DocResolution, error)
``

### Options
Create options.

#### RecoveryPublicKeyOpt
This option is mandatory. Will be used to set recovery key private key for create.

#### UpdatePublicKeyOpt
This option is mandatory. Will be used to set update key private key for create.

#### OperationEndpointsOpt
This option is mandatory when domain not set. Will be used to set operation endpoint.

#### AnchorOriginOpt
This option is mandatory when domain not set. Will be used to set anchor origin for create request.

#### CheckDIDAnchored
This option is not mandatory. Will be used check if DID is anchored.

Value of CheckDIDAnchored option:

```
type ResolveDIDRetry struct {
	MaxNumber int
	SleepTime *time.Duration
}
```

### Example

```
import (
"crypto"
"crypto/ed25519"
"crypto/rand"
"fmt"

ariesdid "github.com/hyperledger/aries-framework-go/pkg/doc/did"
"github.com/hyperledger/aries-framework-go/pkg/doc/jose"
vdrapi "github.com/hyperledger/aries-framework-go/pkg/framework/aries/api/vdr"

"github.com/hyperledger/aries-framework-go-ext/component/vdr/orb"
)

recoveryKey, recoveryKeyPrivateKey, err := ed25519.GenerateKey(rand.Reader)
if err != nil {
	return err
}

updateKey, updateKeyPrivateKey, err := ed25519.GenerateKey(rand.Reader)
if err != nil {
	return err
}

didPublicKey, _, err := ed25519.GenerateKey(rand.Reader)
if err != nil {
	return err
}

jwk, err := jose.JWKFromKey(didPublicKey)
if err != nil {
	return err
}

vm,err:=ariesdid.NewVerificationMethodFromJWK("key1", "Ed25519VerificationKey2018", "", jwk)
if err != nil {
	return err
}

didDoc := &ariesdid.Doc{}

// add did keys
didDoc.Authentication = append(didDoc.Authentication, *ariesdid.NewReferencedVerification(vm,
		ariesdid.Authentication))

// add did services
didDoc.Service = []ariesdid.Service{{ID: "svc1", Type: "type", ServiceEndpoint: "http://www.example.com/"}}

// create did
createdDocResolution, err := vdr.Create(didDoc,
		vdrapi.WithOption(orb.RecoveryPublicKeyOpt, recoveryKey),
		vdrapi.WithOption(orb.UpdatePublicKeyOpt, updateKey),
		// No need to use this option because we already use domain
		// vdrapi.WithOption(orb.OperationEndpointsOpt, []string{"https://orb-1.devel.trustbloc.dev/sidetree/v1/operations"}),
		vdrapi.WithOption(orb.AnchorOriginOpt, "https://orb-2.devel.trustbloc.dev/services/orb"))
if err != nil {
	return err
}

fmt.Println(createdDocResolution.DIDDocument.ID)
```


## Read
Read used to resolve Orb DID.

``
func Read(did string, opts ...vdrapi.DIDMethodOption) (*docdid.DocResolution, error)
``

### Options
Read options.

#### ResolutionEndpointsOpt
This option is mandatory when domain not set. Will be used to set resolution endpoint.

### Example

```
docResolution, err := vdr.Read(didID)
if err != nil {
	return err
}

fmt.Println(docResolution.DIDDocument.ID)
```

## Update
Update used to update or recover Orb DID.

``
func Update(didDoc *docdid.Doc, opts ...vdrapi.DIDMethodOption) error
``

### Options
Update options.

#### RecoverOpt
This option is mandatory. Will be used to signal that it's recover request [true, false].

#### AnchorOriginOpt
This option is not mandatory. Will be used to set anchor origin for recover request.

#### OperationEndpointsOpt
This option is mandatory when domain not set. Will be used to set operation endpoint for recover request.

#### ResolutionEndpointsOpt
This option is mandatory when domain not set. Will be used to set resolution endpoint.

#### CheckDIDUpdated
This option is not mandatory. Will be used check if DID is updated.

Value of CheckDIDUpdated option:

```
type ResolveDIDRetry struct {
	MaxNumber int
	SleepTime *time.Duration
}
```

### Example Update

```
updateKey, updateKeyPrivateKey, err := ed25519.GenerateKey(rand.Reader)
if err != nil {
	return err
}

// this key will used for next update request
keyRetrieverImpl.nextUpdatePublicKey = updateKey

didPublicKey, _, err := ed25519.GenerateKey(rand.Reader)
if err != nil {
	return err
}

jwk, err := jose.JWKFromKey(didPublicKey)
if err != nil {
	return err
}

vm,err:=ariesdid.NewVerificationMethodFromJWK("key1", "Ed25519VerificationKey2018", "", jwk)
if err != nil {
	return err
}


didDoc := &ariesdid.Doc{ID: didID}

didDoc.Authentication = append(didDoc.Authentication, *ariesdid.NewReferencedVerification(vm,
		ariesdid.Authentication))

didDoc.CapabilityInvocation = append(didDoc.CapabilityInvocation, *ariesdid.NewReferencedVerification(vm,
		ariesdid.CapabilityInvocation))

didDoc.Service = []ariesdid.Service{
		{
			ID:              "svc1",
			Type:            "typeUpdated",
			ServiceEndpoint: "http://www.example.com/",
		},
		{
			ID:              "svc2",
			Type:            "type",
			ServiceEndpoint: "http://www.example.com/",
		},
}

if err := vdr.Update(didDoc); err != nil {
	return err
}

// update private key will be used to sign next update request
keyRetrieverImpl.updateKey = updateKeyPrivateKey
```

### Example Recover

```
recoveryKey, recoveryKeyPrivateKey, err := ed25519.GenerateKey(rand.Reader)
if err != nil {
	return err
}

// this key will used for next recover request
keyRetriever.nextRecoveryPublicKey = recoveryKey

didDoc := &ariesdid.Doc{ID: didID}

didPublicKey, _, err := ed25519.GenerateKey(rand.Reader)
if err != nil {
	return err
}

jwk, err := jose.JWKFromKey(didPublicKey)
if err != nil {
	return err
}

vm,err:=ariesdid.NewVerificationMethodFromJWK("key1", "Ed25519VerificationKey2018", "", jwk)
if err != nil {
	return err
}


didDoc.CapabilityInvocation = append(didDoc.CapabilityInvocation, *ariesdid.NewReferencedVerification(vm,
	ariesdid.CapabilityDelegation))

didDoc.Service = []ariesdid.Service{{ID: "svc1", Type: "type", ServiceEndpoint: "http://www.example.com/"}}

if err := e.vdr.Update(didDoc,
	vdrapi.WithOption(orb.RecoverOpt, true), 
	vdrapi.WithOption(orb.AnchorOriginOpt, "https://orb-2.devel.trustbloc.dev/services/orb")); err != nil {
	return err
}

// recover private key will be used to sign next recover request
keyRetrieverImpl.recoverKey = recoveryKeyPrivateKey
```

## Deactivate
Deactivate used to deactivate Orb DID.

``
func Deactivate(didID string, opts ...vdrapi.DIDMethodOption) error
``

### Options
Deactivate options.

#### OperationEndpointsOpt
This option is mandatory when domain not set. Will be used to set operation endpoint.


### Example

```
if err:=vdr.Deactivate(discoverableDID);err!=nil{
 return err
}
```
