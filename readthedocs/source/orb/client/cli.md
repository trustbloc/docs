# Command-Line Interface (CLI)

The Orb Command Line Interface (Orb CLI) is a unified tool that provides a consistent interface for interacting with all parts of Orb. 

## Create DID
This command used for creating DID.

### Usage
```
did create [flags]
```

### Flags
* `domain` _[string]_ - URL to the Orb domain.
* `sidetree-url` _[array|string]_ - Array of one or more Sidetree URLs.
* `sidetree-write-token` _[string]_ - The Sidetree write token.
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `publickey-file` _[string]_ - The file contains the DID public keys.
* `service-file` _[string]_ - The file contains the DID services.
* `recoverykey` _[string]_ - The public key PEM used for recovery of the document.
* `recoverykey-file` _[string]_ - The file that contains the public key PEM used for recovery of the document.
* `updatekey` _[string]_ - The public key PEM used for validating the signature of the next update of the document.
* `updatekey-file` _[string]_ - The file that contains the public key PEM used for validating the signature of the next update of the document.

### Example

#### create cmd
```
did create --domain https://orb1.com --publickey-file ./publickeys.json --service-file ./services.json 
--recoverykey-file ./keys/recover/public.pem --updatekey-file ./keys/update/public.pem
```

#### publickeys.json
```
[
 {
  "id": "key1",
  "type": "Ed25519VerificationKey2018",
  "purposes": ["authentication"],
  "jwkPath": "./key1_jwk.json"
 },
 {
  "id": "key2",
  "type": "JwsVerificationKey2020",
  "purposes": ["capabilityInvocation"],
  "jwkPath": "./key2_jwk.json"
 }
]
```

#### key1_jwk.json
```
{
  "kty":"OKP",
  "crv":"Ed25519",
  "x":"o1bG1U7G3CNbtALMafUiFOq8ODraTyVTmPtRDO1QUWg",
  "y":""
}
```

#### key2_jwk.json
```
{
  "kty":"EC",
  "crv":"P-256",
  "x":"bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
  "y":"PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
}
```

#### services.json
```
[
  {
    "id": "svc1",
    "type": "type1",
    "priority": 1,
    "routingKeys": ["key1"],
    "recipientKeys": ["key1"],
    "serviceEndpoint": "http://www.example.com"
  },
  {
    "id": "svc2",
    "type": "type2",
    "priority": 2,
    "routingKeys": ["key2"],
    "recipientKeys": ["key2"],
    "serviceEndpoint": "http://www.example.com"
  }
]
```

## Resolve
This command used for resolving DID.

### Usage
```
did resolve [flags]
```

### Flags
* `domain` _[string]_ - URL to the Orb domain.
* `sidetree-url-resolution` _[array|string]_ - Array of one or more Sidetree URLs resolution.
* `auth-token` _[string]_ - The Auth token.
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `did-uri` _[string]_ - DID URI.
* `verify-resolution-result-type` _[string]_ - Verify resolution result type. Values [all, none, unpublished].

### Difference between domain and sidetree-url-resolution
Domain will be used to discover Orb resolution endpoint if you have Orb DID without discovery information. sidetree-url-resolution 
will not discover any endpoint and hit the resolution directly.

### Example

#### resolve cmd
```
did resolve --domain https://orb1.com --did-uri did:orb:3XvwJ:EiDnJwbKHkHdaco4khFeBzvSL1hZ4eBGQq3q1Yjrpi5d4g  
--verify-resolution-result-type all
```

#### resolve cmd DID with discovery information
```
did resolve --did-uri did:orb:https:orb1.com:uAAA:EiAFqEsuKDpwbfFxHfqP-TLdFPjqSrFWwgj8_dU64GMcEQ  
--verify-resolution-result-type all
```

## Update
This command used for updating DID.

### Usage
```
did update [flags]
```

### Flags
* `domain` _[string]_ - URL to the Orb domain.
* `sidetree-url-operation` _[array|string]_ - Array of one or more Sidetree URLs operation.
* `sidetree-url-resolution` _[array|string]_ - Array of one or more Sidetree URLs resolution.
* `sidetree-write-token` _[string]_ - The Sidetree write token.
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `did-uri` _[string]_ - DID URI.
* `add-publickey-file` _[string]_ - The file contains the DID public keys to be added or updated.
* `add-service-file` _[string]_ - The file contains the DID services to be added or updated.
* `nextupdatekey` _[string]_ - The public key PEM used for validating the signature of the next update of the document.
* `nextupdatekey-file` _[string]_ - The file that contains the public key PEM used for validating the signature of the next update of the document.
* `signingkey` _[string]_ - The private key PEM used for signing the update of the document.
* `signingkey-file` _[string]_ -  The file that contains the private key PEM used for signing the update of the document.
* `signingkey-password` _[string]_ -  The Signing key PEM password.

### Example

#### update cmd
```
did update --domain https://orb1.com --did-uri did:orb:3XvwJ:EiDnJwbKHkHdaco4khFeBzvSL1hZ4eBGQq3q1Yjrpi5d4g  
--add-publickey-file ./publickeys.json --add-service-file ./services.json --signingkey-file ./keys/update/key_encrypted.pem --signingkey-password 123
--nextupdatekey-file ./keys/update2/public.pem
```

#### publickeys.json
```
[
 {
 "id": "key2",
 "type": "JwsVerificationKey2020",
 "purposes": ["capabilityInvocation"],
 "jwkPath": "./key2_jwk.json"
 },
 {
  "id": "key3",
  "type": "Ed25519VerificationKey2018",
  "purposes": ["authentication"],
  "jwkPath": "./key3_jwk.json"
 }
]
```

#### key2_jwk.json
```
{
  "kty":"EC",
  "crv":"P-256",
  "x":"bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
  "y":"PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
}
```

#### key3_jwk.json
```
{
  "kty":"OKP",
  "crv":"Ed25519",
  "x":"o1bG1U7G3CNbtALMafUiFOq8ODraTyVTmPtRDO1QUWg",
  "y":""
}
```

#### services.json
```
[
  {
    "id": "svc3",
    "type": "type3",
    "priority": 3,
    "routingKeys": ["key3"],
    "recipientKeys": ["key3"],
    "serviceEndpoint": "http://www.example.com"
  }
]
```

## Recover
This command used for recovering DID.

### Usage
```
did recover [flags]
```

### Flags
* `domain` _[string]_ - URL to the Orb domain.
* `sidetree-url-operation` _[array|string]_ - Array of one or more Sidetree URLs operation.
* `sidetree-url-resolution` _[array|string]_ - Array of one or more Sidetree URLs resolution.
* `sidetree-write-token` _[string]_ - The Sidetree write token.
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `did-uri` _[string]_ - DID URI.
* `publickey-file` _[string]_ - The file contains the DID public keys to be recovered.
* `service-file` _[string]_ - The file contains the DID services to be recovered.
* `nextupdatekey` _[string]_ - The public key PEM used for validating the signature of the next update of the document.
* `nextupdatekey-file` _[string]_ - The file that contains the public key PEM used for validating the signature of the next update of the document.
* `nextrecoverkey` _[string]_ - The public key PEM used for validating the signature of the next recovery of the document.
* `nextrecoverkey-file` _[string]_ - The file that contains the public key PEM used for validating the signature of the next recovery of the document.
* `signingkey` _[string]_ - The private key PEM used for signing the recovery of the document.
* `signingkey-file` _[string]_ -  The file that contains the private key PEM used for signing the recovery of the document.
* `signingkey-password` _[string]_ -  The Signing key PEM password.

### Example

#### recover cmd
```
did recover --domain https://orb1.com --did-uri did:orb:3XvwJ:EiDZTmh3BNBzhwSlOdh3FwAdjzu4BkWly2MoTVNHoNdJpw 
--publickey-file ./publickeys.json  --service-file ./services.json 
--nextrecoverkey-file ./keys/recover2/public.pem --nextupdatekey-file ./keys/update3/public.pem 
--signingkey-file ./keys/recover/key_encrypted.pem --signingkey-password 123
```

#### publickeys.json
```
[
 {
  "id": "key-recover-id",
  "type": "Ed25519VerificationKey2018",
  "purposes": ["authentication"],
  "jwkPath": "./fixtures/did-keys/recover/key1_jwk.json"
 }
]
```

#### key1_jwk.json
```
{
  "kty":"OKP",
  "crv":"Ed25519",
  "x":"o1bG1U7G3CNbtALMafUiFOq8ODraTyVTmPtRDO1QUWg",
  "y":""
}

```

#### services.json
```
[
  {
    "id": "svc-recover-id",
    "type": "type1",
    "priority": 1,
    "routingKeys": ["key1"],
    "recipientKeys": ["key1"],
    "serviceEndpoint": "http://www.example.com"
  }
]
```

## Deactivate
This command used for Deactivating DID.

### Usage
```
did deactivate [flags]
```

### Flags
* `domain` _[string]_ - URL to the Orb domain.
* `sidetree-url-operation` _[array|string]_ - Array of one or more Sidetree URLs operation.
* `sidetree-url-resolution` _[array|string]_ - Array of one or more Sidetree URLs resolution.
* `sidetree-write-token` _[string]_ - The Sidetree write token.
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `did-uri` _[string]_ - DID URI.
* `signingkey` _[string]_ - The private key PEM used for signing deactivate of the document.
* `signingkey-file` _[string]_ -  The file that contains the private key PEM used for signing deactivate of the document.
* `signingkey-password` _[string]_ -  The Signing key PEM password.

### Example

#### deactivate cmd
```
deactivate-did --domain https://orb1.com --did-uri did:orb:3XvwJ:EiDnJwbKHkHdaco4khFeBzvSL1hZ4eBGQq3q1Yjrpi5d4g  
--signingkey-file ./keys/recover2/key_encrypted.pem --signingkey-password 123
```

## Follow
This command used for manage followers.

### Usage
```
follower [flags]
```

### Flags
* `outbox-url` _[string]_ - Outbox url.
* `actor` _[string]_ - Actor IRI.
* `to` _[string]_ - To IRI.
* `action` _[string]_ - Follower action (Follow, Undo).
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `auth-token` _[string]_ - Auth token.
* `follow-id` _[string]_ - Follow id required for undo action.



### Example

#### follow cmd (orb-2 server follows orb-1 server)
```
follower --outbox-url=https://orb-1.com/services/orb/outbox --actor=https://orb-2/services/orb
--to=https://orb-1/services/orb --action=Follow --auth-token=token123
```

#### undo cmd (orb-2 server unfollows orb-1 server)
```
follower --outbox-url=https://orb-1.com/services/orb/outbox --actor=https://orb-2/services/orb
--to=https://orb-1/services/orb --action=Undo --auth-token=token123 --follow-id=r232DD##
```

## Witness
This command used for manage witness.

### Usage
```
follower [flags]
```

### Flags
* `outbox-url` _[string]_ - Outbox url.
* `actor` _[string]_ - Actor IRI.
* `to` _[string]_ - To IRI.
* `action` _[string]_ - Follower action (Follow, Undo).
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.
* `auth-token` _[string]_ - Auth token.
* `follow-id` _[string]_ - Follow id required for undo action.


### Example

#### witness cmd (orb-1 invites orb-2 to be a witness)
```
witness --outbox-url=https://orb-1.com/services/orb/outbox --actor=https://orb-1.com/services/orb
--to=https://orb-2.com/services/orb --action=InviteWitness --auth-token=token123
```
#### undo cmd (orb-1 uninvite orb-2 to be a witness)
```
witness --outbox-url=https://orb-1.com/services/orb/outbox --actor=https://orb-1.com/services/orb
--to=https://orb-2.com/services/orb --action=InviteWitness --auth-token=token123 --invite-witness-id=r2WW##
```

## Accept List

The _acceptlist_ command adds and removes actors from the _follow_ and _witness_
[accept-lists](../system/activitypub.html#accept-list).

### Usage

```
acceptlist [command] [flags]
```

### Flags
 
* `url` _[string]_ - Accept-list [endpoint](../restendpoints/activitypub.html#accept-list).
* `actor` _[array|string]_ - Array of one or more actors to add to the accept-list.
* `type` _[string]_ - Accept-list type - either _follow_ or _witness_.

### Add Command

Adds one or more actors to an accept-list of a given type.

### Example

The orb domain, orb-1.com, adds orb-2.com and orb-3.com to the _follow_ accept list:

```
acceptlist add --url https://orb-1.com/services/orb/acceptlist --actor https://orb-2.com/services/orb --actor https://orb-3.com/services/orb --type follow
```

### Remove Command

Removes one or more actors from an accept-list of a given type:

### Example

The orb domain, orb-1.com, removes orb-2.com from the _witness_ accept list:

```
acceptlist remove --url https://orb-1.com/services/orb/acceptlist --actor https://orb-2.com/services/orb --type witness
```

### Get Command

Retrieves all accept-lists or an accept-list of the given type.

### Example

Retrieve orb-1.com's _witness_ accept-list:

```
acceptlist get --url https://orb-1.com/services/orb/acceptlist --type witness
```

Retrieve all of orb-1's accept-lists:

```
acceptlist get --url https://orb-1.com/services/orb/acceptlist
```

## Policy

The _policy_ command updates and retrieves the [witness policy](../system/witnesspolicy.html#witness-policy).

### Usage

```
policy [command] [flags]
```

#### Flags

* `url` _[string]_ - Witness policy [endpoint](../restendpoints/witness-policy.html#witness-policy-endpoint).
* `policy` _[string]_ - The policy. For example "MinPercent(100,batch) AND OutOf(1,system)".

### Update Command

The _update_ command updates a witness policy.

#### Example

```
policy update --url https://orb-1.com/policy --policy "MinPercent(100,batch) AND OutOf(1,system)"
```

### Get Command

The _get_ command retrieves the witness policy.

#### Example

```
policy get --url https://orb-1.com/policy
```
## Log

The _log_ command updates and retrieves the VCT log URL.

### Usage

```
log [command] [flags]
```

#### Flags

* `url` _[string]_ - Log [endpoint](../vct/restendpoints.html#log-configuration).
* `log` _[string]_ - The log URL. For example "https://vct.com/log".

### Update Command

The _update_ command updates a log URL.

#### Example

```
log update --url https://orb-1.com/log --log https://vct.com/log
```

### Get Command

The _get_ command retrieves the log URL.

#### Example

```
log get --url https://orb-1.com/log
```


## Log Monitor

The _logmonitor_ command activates and deactivates log from the list of logs that are observed by log monitoring service.
[log-monitor](../vct/log.html#log-monitoring).

### Usage

```
logmonitor [command] [flags]
```

### Flags

* `url` _[string]_ - Log monitor [endpoint](../vct/restendpoints.html#log-monitoring).
* `log` _[array|string]_ - Array of one or more logs to activate for log monitoring.

### Activate Command

Adds one or more logs to the list of logs that are monitored by log monitoring service.

### Example

```
logmonitor activate --url https://orb-1.com/log-monitor --url https://vct.com/log --url https://other-vct.com/some-log 
```

### Deactivate Command
 
Removes one or more logs from the list of logs that are monitored by log monitoring service.

### Example

```
logmonitor deactivate --url https://orb-1.com/log-monitor --url https://vct.com/log --url https://other-vct.com/some-log 
```

### Get Command

Retrieves all logs of the given status that are observed by monitoring service. Status can be either active or inactive. 
If not supplied it defaults to active.

### Example

Retrieve orb-1.com's inactive logs:

```
logmonitor get --url https://orb-1.com/log-monitor --status inactive
```

Retrieve all of orb-1's active logs:

```
logmonitor get --url https://orb-1.com/log-monitor --status active
```

## VCT

The _vct_ command interacts with the VCT server to verify that an anchor has been added to a log.

### Usage

```
vct [command] [flags]
```

#### Flags

* `anchor` _[string]_ - The hash of the anchor linkset.
* `cas-url` _[string]_ - The CAS URL.
* `vct-auth-token` _[string]_ - The authorization bearer token for the VCT server (optional).

### Verify Command

The _verify_ command verifies that the anchor linkset (given by the anchor hash) has been added to the VCT
log(s) that are specified in the proofs of the verifiable credential in the linkset.

#### Example

```
vct verify --anchor uEiDuIicNljP8PoHJk6_aA7w1d4U3FAvDMfF7Dsh7fkw3Wg --cas-url https://orb.domain1.com/cas
```

Response:

```json
[
  {
    "domain": "https://vct.domain1.com/maple2020",
    "proofValue": "JnP2nE4jLiHsmR65q1a6Tp-HUKZanjpCstm7qAljIE3Z6Y2mEcNarOESiu1gFlsonz1soQJe69piE7qZnTqrAw",
    "found": true,
    "leafIndex": 12,
    "auditPath": [
      "rgWgpjyv6227zFbcpyRv3YR1vbLbXaOuCeYoF7uDaY4=",
      "lg4iaTTfHNrH41C8CBuSlo+VIK+f+2dA+5i5N8NzsIM="
    ]
  },
  {
    "domain": "https://orb.domain2.com",
    "proofValue": "6H8dEtYndINR48g4mFJ9wAxXMVYu7FdOEzpwRap5AuOTPPpd9E7LYjME-pH2kvqiFLe9ZmKDJa2YzqOoLjHJAQ",
    "found": false,
    "leafIndex": 0,
    "auditPath": null,
    "error": "get STH: 404 page not found\n"
  }
]
```