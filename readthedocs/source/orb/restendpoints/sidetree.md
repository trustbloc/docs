# Sidetree Endpoints

Endpoints are defined to post Sidetree operations and to resolve DID documents.

## Operations

**Endpoint:** /sidetree/v1/operations

### POST

Post a Sidetree operation as per [Sidetree DID Operations](../system/sidetree.html#did-operations)

**Example**

Post a _create_ operation:

```json
POST /sidetree/v1/operations HTTP/1.1
Host: orb.domain1.com
Content-Type: application/ld+json
        
{
  "delta": {
    "patches": [
      {
        "action": "add-public-keys",
        "publicKeys": [
          {
            "id": "createKey",
            "publicKeyJwk": {
              "crv": "P-256",
              "kty": "EC",
              "x": "kTpW2qcc66DyPWNnTSmaomtcGC0fOB2XC-OavrtSmOQ",
              "y": "_-2MbdKMjYOTSny4zSHyHU-L2sT9MUoDyQfRr2R_avE"
            },
            "purposes": [
              "authentication"
            ],
            "type": "JsonWebKey2020"
          },
          {
            "id": "auth",
            "publicKeyJwk": {
              "crv": "Ed25519",
              "kty": "OKP",
              "x": "4BpSggpgRRtlQJKpPznhKfEon1OAlmr1MFjaN2sS4Ns",
              "y": ""
            },
            "purposes": [
              "assertionMethod"
            ],
            "type": "Ed25519VerificationKey2018"
          }
        ]
      },
      {
        "action": "add-services",
        "services": [
          {
            "id": "didcomm",
            "priority": 0,
            "recipientKeys": [
              "F1nHc1qea4QNfmcFjHvGxDNRogomARVhNpk8T812eWDD"
            ],
            "routingKeys": [
              "3xWoBwfwuyRH9ax82z6Lm24URNRJUoxg4PV6CcXBFvWr"
            ],
            "serviceEndpoint": "https://hub.example.com/.identity/did:example:0123456789abcdef/",
            "type": "did-communication"
          }
        ]
      }
    ],
    "updateCommitment": "EiB1FFyDuSMNoMVKvSqZjybZxs_NfqA8WpMd0RTivT351Q"
  },
  "suffixData": {
    "anchorOrigin": "https://orb.domain1.com",
    "deltaHash": "EiDt8PTc-7SRaEoGOkkTTAUTUb8E3fbr4AMAw0Oa8lDZfQ",
    "recoveryCommitment": "EiDEBk7ejSyjddtZWyNA37gA0SdMvkSAqw7BxkLUQkpbyw"
  },
  "type": "create"
}
```
 
Response from a _create_ operation:
```json
{
  "@context": "https://w3id.org/did-resolution/v1",
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/jws-2020/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1"
    ],
    "assertionMethod": [
      "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#auth"
    ],
    "authentication": [
      "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#createKey"
    ],
    "id": "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
    "service": [
      {
        "id": "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#didcomm",
        "priority": 0,
        "recipientKeys": [
          "F1nHc1qea4QNfmcFjHvGxDNRogomARVhNpk8T812eWDD"
        ],
        "routingKeys": [
          "3xWoBwfwuyRH9ax82z6Lm24URNRJUoxg4PV6CcXBFvWr"
        ],
        "serviceEndpoint": "https://hub.example.com/.identity/did:example:0123456789abcdef/",
        "type": "did-communication"
      }
    ],
    "verificationMethod": [
      {
        "controller": "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
        "id": "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#createKey",
        "publicKeyJwk": {
          "crv": "P-256",
          "kty": "EC",
          "x": "kTpW2qcc66DyPWNnTSmaomtcGC0fOB2XC-OavrtSmOQ",
          "y": "_-2MbdKMjYOTSny4zSHyHU-L2sT9MUoDyQfRr2R_avE"
        },
        "type": "JsonWebKey2020"
      },
      {
        "controller": "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
        "id": "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#auth",
        "publicKeyBase58": "G5oc2RXAWqCtF6w9K2qDTtPwPjWpSDk923MxWEKiVT6a",
        "type": "Ed25519VerificationKey2018"
      }
    ]
  },
  "didDocumentMetadata": {
    "equivalentId": [
      "did:orb:https:orb.domain1.com:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ"
    ],
    "method": {
      "anchorOrigin": "https://orb.domain1.com",
      "published": false,
      "recoveryCommitment": "EiDEBk7ejSyjddtZWyNA37gA0SdMvkSAqw7BxkLUQkpbyw",
      "updateCommitment": "EiB1FFyDuSMNoMVKvSqZjybZxs_NfqA8WpMd0RTivT351Q"
    }
  }
}
```

Create document response will contain only one entry in equivalentId list in document metadata. 
The client can use this equivalentId entry with https hint to resolve document from the domain that is specified in the https hint.

If Orb domain supports unpublished operation caching the client will be able to immediately resolve 
unpublished DID document. The server response for unpublished document equals the above-mentioned _create_ operation response.

If the client queries for DID document upon successful anchoring of create operation the document metadata will contain 
the following information:
- published flag is set to true
- canonicalId will be present
- multiple equivalent IDs will be present in equivalentId list
- versionId will be present

```json
{
  "@context": "https://w3id.org/did-resolution/v1",
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/jws-2020/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1"
    ],
    "assertionMethod": [
      "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#auth"
    ],
    "authentication": [
      "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#createKey"
    ],
    "id": "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
    "service": [
      {
        "id": "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#didcomm",
        "priority": 0,
        "recipientKeys": [
          "F1nHc1qea4QNfmcFjHvGxDNRogomARVhNpk8T812eWDD"
        ],
        "routingKeys": [
          "3xWoBwfwuyRH9ax82z6Lm24URNRJUoxg4PV6CcXBFvWr"
        ],
        "serviceEndpoint": "https://hub.example.com/.identity/did:example:0123456789abcdef/",
        "type": "did-communication"
      }
    ],
    "verificationMethod": [
      {
        "controller": "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
        "id": "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#createKey",
        "publicKeyJwk": {
          "crv": "P-256",
          "kty": "EC",
          "x": "kTpW2qcc66DyPWNnTSmaomtcGC0fOB2XC-OavrtSmOQ",
          "y": "_-2MbdKMjYOTSny4zSHyHU-L2sT9MUoDyQfRr2R_avE"
        },
        "type": "JsonWebKey2020"
      },
      {
        "controller": "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
        "id": "did:orb:uAAA:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#auth",
        "publicKeyBase58": "G5oc2RXAWqCtF6w9K2qDTtPwPjWpSDk923MxWEKiVT6a",
        "type": "Ed25519VerificationKey2018"
      }
    ]
  },
  "didDocumentMetadata": {
    "canonicalId": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
    "equivalentId": [
      "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
      "did:orb:hl:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpRGFKR05rRkpTTWxTZUtqRUZBSXZqQnhieWJyN1ZXakxweHBsaFhQeDhGQ3d4QmlwZnM6Ly9iYWZrcmVpZzJlcnJ3aWZldXJza3NwY3VtaWZhY2Y2Z2J5dzZqeGw1dmsyZ2x1NG5nbGJsdDZoeWZibQ:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
    ],
    "method": {
      "anchorOrigin": "https://orb.domain1.com",
      "published": true,
      "recoveryCommitment": "EiDEBk7ejSyjddtZWyNA37gA0SdMvkSAqw7BxkLUQkpbyw",
      "updateCommitment": "EiB1FFyDuSMNoMVKvSqZjybZxs_NfqA8WpMd0RTivT351Q"
    },
    "versionId": "uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw"
  }
}
```

The Orb server will include the anchor-hash segment within the canonicalId property of the returned DID document metadata. 
The anchor-hash is set to the multihash (with multibase prefix) of the latest known AnchorCredential that contains a Create or Recover operation for the DID.

For more details about canonicalId see [Canonical IDs](https://trustbloc.github.io/did-method-orb/#propagation-delay-and-canonical-ids)

The Orb server will also include canonicalId as the fist item in equivalent IDs list. The second item in the list 
will be discoverable [Cryptographic Hyperlink](https://w3c-ccg.github.io/hashlink/)

Once the client is able to obtain canonicalId, the client should discontinue using 'un-anchored' identifier (DID that includes uAAA) 
and use either canonicalId or cryptographic hyperlink from equivalentId list.

**Example**

Post an _update_ operation to add new service:

```json
POST /sidetree/v1/operations HTTP/1.1
Host: orb.domain1.com
Content-Type: application/ld+json
{
  "delta": {
    "patches": [
      {
        "action": "add-services",
        "services": [
          {
            "id": "newService",
            "serviceEndpoint": "http://hub.my-personal-server.com",
            "type": "SecureDataStore"
          }
        ]
      }
    ],
    "updateCommitment": "EiCosM6zJoafNLoFcvwMTxjqJymG7GRym56PwlP2Jz3Iqg"
  },
  "didSuffix": "EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
  "revealValue": "EiD4_J1zZ89P4uN8F2_R6a7kgge9s3posjMPMur4Jx0Z4A",
  "signedData": "eyJhbGciOiJFUzI1NiJ9.eyJhbmNob3JGcm9tIjoxNjQ2Njg0MDIzLCJhbmNob3JVbnRpbCI6MTY0NjY4NDMyMywiZGVsdGFIYXNoIjoiRWlDN1B5cGYxSE5xN09SQk53S2NtdWRyYTlScENyeXd4M1lYdGduUjF5azZ0ZyIsInVwZGF0ZUtleSI6eyJjcnYiOiJQLTI1NiIsImt0eSI6IkVDIiwieCI6IlNyb21qMWljRl91b0JVZldPUVotUDdVaXRWeFhqMUU3WXJvbm5BeTEyTUUiLCJ5IjoidEdOMEE4OTh5QnlNUkpmRG9qTHpQR0JYVEJaVHZHOWhZMVRhMmRlc01CYyJ9fQ.T9GaRytJSm1KsUoUoQcKX6Pdk3DJ9qX-IyuRI9iwCl9oGDPMteA9M3ngU9XiXhJpiVJ6MhDGGhH7bR0jZ5HcuQ",
  "type": "update"
}
```

Resolution response from Orb server upon successful anchoring of update operation:
sidetree/v1/identifiers/did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ
server

```json
{
  "@context": "https://w3id.org/did-resolution/v1",
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/jws-2020/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1"
    ],
    "assertionMethod": [
      "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#auth"
    ],
    "authentication": [
      "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#createKey"
    ],
    "id": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
    "service": [
      {
        "id": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#didcomm",
        "priority": 0,
        "recipientKeys": [
          "F1nHc1qea4QNfmcFjHvGxDNRogomARVhNpk8T812eWDD"
        ],
        "routingKeys": [
          "3xWoBwfwuyRH9ax82z6Lm24URNRJUoxg4PV6CcXBFvWr"
        ],
        "serviceEndpoint": "https://hub.example.com/.identity/did:example:0123456789abcdef/",
        "type": "did-communication"
      },
      {
        "id": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#newService",
        "serviceEndpoint": "http://hub.my-personal-server.com",
        "type": "SecureDataStore"
      }
    ],
    "verificationMethod": [
      {
        "controller": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
        "id": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#createKey",
        "publicKeyJwk": {
          "crv": "P-256",
          "kty": "EC",
          "x": "kTpW2qcc66DyPWNnTSmaomtcGC0fOB2XC-OavrtSmOQ",
          "y": "_-2MbdKMjYOTSny4zSHyHU-L2sT9MUoDyQfRr2R_avE"
        },
        "type": "JsonWebKey2020"
      },
      {
        "controller": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
        "id": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#auth",
        "publicKeyBase58": "G5oc2RXAWqCtF6w9K2qDTtPwPjWpSDk923MxWEKiVT6a",
        "type": "Ed25519VerificationKey2018"
      }
    ]
  },
  "didDocumentMetadata": {
    "canonicalId": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
    "equivalentId": [
      "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
      "did:orb:hl:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpRGFKR05rRkpTTWxTZUtqRUZBSXZqQnhieWJyN1ZXakxweHBsaFhQeDhGQ3d4QmlwZnM6Ly9iYWZrcmVpZzJlcnJ3aWZldXJza3NwY3VtaWZhY2Y2Z2J5dzZqeGw1dmsyZ2x1NG5nbGJsdDZoeWZibQ:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
   ],
    "method": {
      "anchorOrigin": "https://orb.domain1.com",
      "published": true,
      "recoveryCommitment": "EiDEBk7ejSyjddtZWyNA37gA0SdMvkSAqw7BxkLUQkpbyw",
      "updateCommitment": "EiCosM6zJoafNLoFcvwMTxjqJymG7GRym56PwlP2Jz3Iqg"
    },
    "versionId": "uEiB0lC69bPfaIYmRN0wBEYygbEiSFZpJEuIjbqFHnui0ig"
  }
}
```

Note that versionId specifies anchor-hash that contains update operation. You can use versionId to 
resolve document at specific version.

**Example**

Post a _recover_ operation:

```json
POST /sidetree/v1/operations HTTP/1.1
Host: orb.domain1.com
Content-Type: application/ld+json
{
  "delta": {
    "patches": [
      {
        "action": "add-public-keys",
        "publicKeys": [
          {
            "id": "recoveryKey",
            "publicKeyJwk": {
              "crv": "P-256",
              "kty": "EC",
              "x": "-IRdzsImjgsrto6r_MzU9LjecdeBg153ixNSyUYSiwI",
              "y": "q5g_WtYdCxjXyey4ASH9N7rg9CIwY9guVoD269mTq4k"
            },
            "purposes": [
              "authentication"
            ],
            "type": "JsonWebKey2020"
          },
          {
            "id": "auth",
            "publicKeyJwk": {
              "crv": "Ed25519",
              "kty": "OKP",
              "x": "6fEptoI2zIVlR9YQHar_xUyWlhnYH6WjJoI-tTIwopY",
              "y": ""
            },
            "purposes": [
              "assertionMethod"
            ],
            "type": "Ed25519VerificationKey2018"
          }
        ]
      },
      {
        "action": "add-services",
        "services": [
          {
            "id": "didcomm",
            "priority": 0,
            "recipientKeys": [
              "EvwVTTeLzVRNxX9vETVv8xRSsqfUGi2r3a6DzrhSMTGc"
            ],
            "routingKeys": [
              "EaMUBZMAYZU8CWcioQBxT6dLjhDvbRjDyVisttixAzpc"
            ],
            "serviceEndpoint": "https://hub.example.com/.identity/did:example:0123456789abcdef/",
            "type": "did-communication"
          }
        ]
      }
    ],
    "updateCommitment": "EiCZDsrREJOpB49QfIPCU-74VXM760gTJGlkhvQAgwdcMQ"
  },
  "didSuffix": "EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
  "revealValue": "EiAkLgBNtJVpSHGzuWceujPdw0A3p_tK3cXhDndQG3JjPQ",
  "signedData": "eyJhbGciOiJFUzI1NiJ9.eyJhbmNob3JGcm9tIjoxNjQ2Njg0MDI4LCJhbmNob3JPcmlnaW4iOiJodHRwczovL29yYi5kb21haW4xLmNvbSIsImFuY2hvclVudGlsIjoxNjQ2Njg0MzI4LCJkZWx0YUhhc2giOiJFaURJZnVuZXBNbEFSeGFNT2RkcEdxWXIxVkFDVm5VNV8yRkxhYm1FcEdqcXRnIiwicmVjb3ZlcnlDb21taXRtZW50IjoiRWlEY0x1eUNLN2pWVHQ0d0JWM09pSFhiX2E4WmNkMG5NMEtnTXlLTnJlWkZ3dyIsInJlY292ZXJ5S2V5Ijp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiQnF2anpEMDdWNnRwYjVfVHNVRzNDRWJENHVJSkh3TmlvTzFOc0VGWVpRUSIsInkiOiJmbGktSVJRcnhHXzhESGJLMlJtMVEtN1hKay14VEtHQjIwRk5WdFFTSTJZIn19.EBbqSNSe6vZHOJMsxXkv7tRk1g6k28qzOWwhqAP8XIB_4mpzJEhoVaVSKT14LIOTQBAaQXad-PV52WnwoyBWvg",
  "type": "recover"
}
```

Upon successful anchoring of recover operation the Orb server will return the following resolution response
for the following request:
sidetree/v1/identifiers/did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ

```json
{
  "@context": "https://w3id.org/did-resolution/v1",
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/jws-2020/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1"
    ],
    "assertionMethod": [
      "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#auth"
    ],
    "authentication": [
      "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#recoveryKey"
    ],
    "id": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
    "service": [
      {
        "id": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#didcomm",
        "priority": 0,
        "recipientKeys": [
          "EvwVTTeLzVRNxX9vETVv8xRSsqfUGi2r3a6DzrhSMTGc"
        ],
        "routingKeys": [
          "EaMUBZMAYZU8CWcioQBxT6dLjhDvbRjDyVisttixAzpc"
        ],
        "serviceEndpoint": "https://hub.example.com/.identity/did:example:0123456789abcdef/",
        "type": "did-communication"
      }
    ],
    "verificationMethod": [
      {
        "controller": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
        "id": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#recoveryKey",
        "publicKeyJwk": {
          "crv": "P-256",
          "kty": "EC",
          "x": "-IRdzsImjgsrto6r_MzU9LjecdeBg153ixNSyUYSiwI",
          "y": "q5g_WtYdCxjXyey4ASH9N7rg9CIwY9guVoD269mTq4k"
        },
        "type": "JsonWebKey2020"
      },
      {
        "controller": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
        "id": "did:orb:uEiDaJGNkFJSMlSeKjEFAIvjBxbybr7VWjLpxplhXPx8FCw:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ#auth",
        "publicKeyBase58": "GkDHCaxe2yyD5SHRZPWTd61rzsrx66aimzJAV1SNYPpH",
        "type": "Ed25519VerificationKey2018"
      }
    ]
  },
  "didDocumentMetadata": {
    "canonicalId": "did:orb:uEiDU52ebPORSjOWs2Lh5pTOBFVlioPNyVdOfsrL2feH5Pg:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
    "equivalentId": [
      "did:orb:uEiDU52ebPORSjOWs2Lh5pTOBFVlioPNyVdOfsrL2feH5Pg:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
      "did:orb:hl:uEiDU52ebPORSjOWs2Lh5pTOBFVlioPNyVdOfsrL2feH5Pg:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpRFU1MmViUE9SU2pPV3MyTGg1cFRPQkZWbGlvUE55VmRPZnNyTDJmZUg1UGd4QmlwZnM6Ly9iYWZrcmVpZ3U0NXR6d3BoZWtrZ29sbGd5eGI0MmttNGJjdm13ZmlodG9qazVoaDVzd2wzaDN5cHpoeQ:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
     ],
    "method": {
      "anchorOrigin": "https://orb.domain1.com",
      "published": true,
      "recoveryCommitment": "EiDcLuyCK7jVTt4wBV3OiHXb_a8Zcd0nM0KgMyKNreZFww",
      "updateCommitment": "EiCZDsrREJOpB49QfIPCU-74VXM760gTJGlkhvQAgwdcMQ"
    },
    "versionId": "uEiDU52ebPORSjOWs2Lh5pTOBFVlioPNyVdOfsrL2feH5Pg"
  }
}
```

Note that canonicalId has changed - it includes anchor-hash that contains recover operation. At this point the client 
should discontinue using old canonicalId and use new canonicalId.

For more details about canonicalId see [Canonical IDs](https://trustbloc.github.io/did-method-orb/#propagation-delay-and-canonical-ids)

**Example**

Post a _deactivate_ operation:

```json
POST /sidetree/v1/operations HTTP/1.1
Host: orb.domain1.com
Content-Type: application/ld+json
{
  "didSuffix": "EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
  "revealValue": "EiDdHkE3GeAfTcPQdrVi8ET2_4NbRl9emFigCKdx9hp1yQ",
  "signedData": "eyJhbGciOiJFUzI1NiJ9.eyJhbmNob3JGcm9tIjoxNjQ2Njg0MDM4LCJkaWRTdWZmaXgiOiJFaURJRnU4QmV2LW5Kc3E4dXcyeEdIS3RMS3I2YUxuY0ZNbk9sU1E5cGZQNFZRIiwicmVjb3ZlcnlLZXkiOnsiY3J2IjoiUC0yNTYiLCJrdHkiOiJFQyIsIngiOiJsWDYxUXlFWkVwaWh3N1dmMGt1bXlYMW1WX19heHRObkczM0VPNUZVRmhZIiwieSI6ImZUdWlURkJsZmo3OFIwUzVvbXRJNTZmamdOVlU5MFhMTzdkWEx0X21TbW8ifSwicmV2ZWFsVmFsdWUiOiIifQ.8lC5SPQO6FsvXCOCly5WNt33muD5SEAHC3iOLG1pGc0pWyct7R5MCOMD80xZwB_KnJManW6eMuVY4lZI6UmuTw",
  "type": "deactivate"
}
```

Upon successful anchoring of deactivate operation the Orb server will return the following resolution response
for the following request:
sidetree/v1/identifiers/did:orb:uEiDU52ebPORSjOWs2Lh5pTOBFVlioPNyVdOfsrL2feH5Pg:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ

```json
{
  "@context": "https://w3id.org/did-resolution/v1",
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1"
    ],
    "id": "did:orb:uEiDU52ebPORSjOWs2Lh5pTOBFVlioPNyVdOfsrL2feH5Pg:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ"
  },
  "didDocumentMetadata": {
    "canonicalId": "did:orb:uEiDU52ebPORSjOWs2Lh5pTOBFVlioPNyVdOfsrL2feH5Pg:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
    "deactivated": true,
    "equivalentId": [
      "did:orb:uEiDU52ebPORSjOWs2Lh5pTOBFVlioPNyVdOfsrL2feH5Pg:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
      "did:orb:hl:uEiDU52ebPORSjOWs2Lh5pTOBFVlioPNyVdOfsrL2feH5Pg:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpRFU1MmViUE9SU2pPV3MyTGg1cFRPQkZWbGlvUE55VmRPZnNyTDJmZUg1UGd4QmlwZnM6Ly9iYWZrcmVpZ3U0NXR6d3BoZWtrZ29sbGd5eGI0MmttNGJjdm13ZmlodG9qazVoaDVzd2wzaDN5cHpoeQ:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ",
      "did:orb:https:shared.domain.com:uEiDU52ebPORSjOWs2Lh5pTOBFVlioPNyVdOfsrL2feH5Pg:EiDIFu8Bev-nJsq8uw2xGHKtLKr6aLncFMnOlSQ9pfP4VQ"
    ],
    "method": {
      "anchorOrigin": "https://orb.domain1.com",
      "published": true
    },
    "versionId": "uEiB7tOd2S0lyQxQYaR0PcgbEQHhSS1PNplvlP5Qtfu-kjw"
  }
}
```

Note that deactivated flag in the document metadata has been set to true and that DID document is an empty document 
that contains only id. Once DID document has been deactivated it is no longer possible to recover or modify DID document. 

## Identifiers

**Endpoint:** /sidetree/v1/identifiers/[id]

### GET

Resolve a DID document as per [Sidetree DID Resolution](../system/sidetree.html#did-resolution)

**Example**

Resolve a DID document using a canonical DID:

```
GET /sidetree/v1/identifiers/did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q HTTP/1.1
Host: orb.domain2.com
Accept: application/ld+json
Accept-Encoding: gzip, deflate
```

Response contains the DID document:

```json
{
  "@context": "https://w3id.org/did-resolution/v1",
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/jws-2020/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1"
    ],
    "assertionMethod": [
      "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q#auth"
    ],
    "authentication": [
      "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q#recoveryKey"
    ],
    "id": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q",
    "service": [
      {
        "id": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q#didcomm",
        "priority": 0,
        "recipientKeys": [
          "2qN15Qwk1uEi19BYJ19zshrxLHkKa5QvvXkLjdn56AaX"
        ],
        "routingKeys": [
          "3kRwP6VuwcbNSzevReUpfDWwyVGRS4YtqxB69DAs3VFN"
        ],
        "serviceEndpoint": "https://hub.example.com/.identity/did:example:0123456789abcdef/",
        "type": "did-communication"
      }
    ],
    "verificationMethod": [
      {
        "controller": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q",
        "id": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q#recoveryKey",
        "publicKeyJwk": {
          "crv": "P-256",
          "kty": "EC",
          "x": "MhxmsoUl3ACv0aZSJejEp6kxEO7QzSHedcYKN-3o0xw",
          "y": "7-TWvrUbnBkFFrqZ_nqVwFTgQyJE_mg11e3ckUEThvQ"
        },
        "type": "JsonWebKey2020"
      },
      {
        "controller": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q",
        "id": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q#auth",
        "publicKeyBase58": "4bkaQc1vU79nTbzUwyBSYtiv2pbb9mTYxHP1A7hGbyU3",
        "type": "Ed25519VerificationKey2018"
      }
    ]
  },
  "didDocumentMetadata": {
    "canonicalId": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q",
    "equivalentId": [
      "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q",
      "did:orb:hl:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:uoQ-BeEJpcGZzOi8vYmFma3JlaWRxbWtkZHlyMmxrNmd4NWJ4enJneGdraHRqczNzcHJ5bmtkaHd3bGkzN2l4eXhzbHB5bnU:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q"
    ],
    "method": {
      "anchorOrigin": "https://orb.domain1.com",
      "published": true,
      "publishedOperations": [
        {
          "anchorOrigin": "https://orb.domain1.com",
          "canonicalReference": "uEiDb2k2i5gYlgTg8GA8gN7apTp_JOkLgUCDY9A5eLOTXsQ",
          "equivalentReferences": [
            "hl:uEiDb2k2i5gYlgTg8GA8gN7apTp_JOkLgUCDY9A5eLOTXsQ:uoQ-BeEJpcGZzOi8vYmFma3JlaWczM2pnMmZ6cWdld2F0cXBheWI0cWRwbnZqajJwNHNvc2M0YmljYndodWJ6cGN6emd4d2U"
          ],
          "operationRequest": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImNyZWF0ZUtleSIsInB1YmxpY0tleUp3ayI6eyJjcnYiOiJQLTI1NiIsImt0eSI6IkVDIiwieCI6IjFwLU1sU0psTHowM05JemRuWWVTUnhMQ3dUNXhLQUpSQTVlcWd1TUIxOFEiLCJ5IjoiUFhXdXRsV3hRM3hDLTYwbExaRkc1REpZR2VNcVFUSDNUQ21jMDg1WFlHSSJ9LCJwdXJwb3NlcyI6WyJhdXRoZW50aWNhdGlvbiJdLCJ0eXBlIjoiSnNvbldlYktleTIwMjAifSx7ImlkIjoiYXV0aCIsInB1YmxpY0tleUp3ayI6eyJjcnYiOiJFZDI1NTE5Iiwia3R5IjoiT0tQIiwieCI6IkZfMFZfTG1Fel9qZVBFbFJmYXBEdzlvbHphbEo1SjJ2MW44MVpkVWVkWU0iLCJ5IjoiIn0sInB1cnBvc2VzIjpbImFzc2VydGlvbk1ldGhvZCJdLCJ0eXBlIjoiRWQyNTUxOVZlcmlmaWNhdGlvbktleTIwMTgifV19LHsiYWN0aW9uIjoiYWRkLXNlcnZpY2VzIiwic2VydmljZXMiOlt7ImlkIjoiZGlkY29tbSIsInByaW9yaXR5IjowLCJyZWNpcGllbnRLZXlzIjpbIkFrc3hOcnVrcUpBUkZQVENrUVFOZHprWjZpaFhLbXNQUHg3Z1M5cXBKZ3pDIl0sInJvdXRpbmdLZXlzIjpbIkRpRloxNG00M3FCa1VTVHNaWFR2U2oxQjVYQmR2UkptR2dINkZQVmM0bXdNIl0sInNlcnZpY2VFbmRwb2ludCI6Imh0dHBzOi8vaHViLmV4YW1wbGUuY29tLy5pZGVudGl0eS9kaWQ6ZXhhbXBsZTowMTIzNDU2Nzg5YWJjZGVmLyIsInR5cGUiOiJkaWQtY29tbXVuaWNhdGlvbiJ9XX1dLCJ1cGRhdGVDb21taXRtZW50IjoiRWlEWjhRMXMwb0ZhbkhXWmxrOHBKTWdEc29EeTVVX1ZoeHUzMG9BaEUyVVQ3dyJ9LCJzdWZmaXhEYXRhIjp7ImFuY2hvck9yaWdpbiI6Imh0dHBzOi8vb3JiLmRvbWFpbjEuY29tIiwiZGVsdGFIYXNoIjoiRWlBNmlYcVJ1TGNramFNbjk3M09wTWxsQm1RNzZjWnY1VDFlMkZQMWVrc19idyIsInJlY292ZXJ5Q29tbWl0bWVudCI6IkVpQjQ3NzVNd0NERnRiSDFGU1dpU3UybkpjcGVVblZ6a1U4MW8wMHUxMEZRUVEifSwidHlwZSI6ImNyZWF0ZSJ9",
          "protocolVersion": 0,
          "transactionNumber": 0,
          "transactionTime": 1645800397,
          "type": "create"
        },
        {
          "anchorOrigin": "https://orb.domain1.com",
          "canonicalReference": "uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ",
          "equivalentReferences": [
            "hl:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:uoQ-BeEJpcGZzOi8vYmFma3JlaWRxbWtkZHlyMmxrNmd4NWJ4enJneGdraHRqczNzcHJ5bmtkaHd3bGkzN2l4eXhzbHB5bnU"
          ],
          "operationRequest": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6InJlY292ZXJ5S2V5IiwicHVibGljS2V5SndrIjp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiTWh4bXNvVWwzQUN2MGFaU0plakVwNmt4RU83UXpTSGVkY1lLTi0zbzB4dyIsInkiOiI3LVRXdnJVYm5Ca0ZGcnFaX25xVndGVGdReUpFX21nMTFlM2NrVUVUaHZRIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJKc29uV2ViS2V5MjAyMCJ9LHsiaWQiOiJhdXRoIiwicHVibGljS2V5SndrIjp7ImNydiI6IkVkMjU1MTkiLCJrdHkiOiJPS1AiLCJ4IjoiTlhvVWJrWFJnbWpJcnIzNzZxbTVxaDJKUFpXREhxRkVNV3dwdzE1LURFQSIsInkiOiIifSwicHVycG9zZXMiOlsiYXNzZXJ0aW9uTWV0aG9kIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJkaWRjb21tIiwicHJpb3JpdHkiOjAsInJlY2lwaWVudEtleXMiOlsiMnFOMTVRd2sxdUVpMTlCWUoxOXpzaHJ4TEhrS2E1UXZ2WGtMamRuNTZBYVgiXSwicm91dGluZ0tleXMiOlsiM2tSd1A2VnV3Y2JOU3pldlJlVXBmRFd3eVZHUlM0WXRxeEI2OURBczNWRk4iXSwic2VydmljZUVuZHBvaW50IjoiaHR0cHM6Ly9odWIuZXhhbXBsZS5jb20vLmlkZW50aXR5L2RpZDpleGFtcGxlOjAxMjM0NTY3ODlhYmNkZWYvIiwidHlwZSI6ImRpZC1jb21tdW5pY2F0aW9uIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaUFsczR5V2xGRWIwWHBkYlViVjVpODY0TE9VX05WMGxFb3pyYmFoN2syMWxnIn0sImRpZFN1ZmZpeCI6IkVpQXpYTF9SYlp4M1JDTTRhVGhaMC1RZVAwTDl4N2VvWXdKQksybl9hMFVtNVEiLCJyZXZlYWxWYWx1ZSI6IkVpQzdPcDJTcUlJR1FEX2xRWEhIa0xKOWo3ODJfVTNGTVExeGNQVDAxNzQyQXciLCJzaWduZWREYXRhIjoiZXlKaGJHY2lPaUpGVXpJMU5pSjkuZXlKaGJtTm9iM0pHY205dElqb3hOalExT0RBd016azRMQ0poYm1Ob2IzSlBjbWxuYVc0aU9pSm9kSFJ3Y3pvdkwyOXlZaTVrYjIxaGFXNHhMbU52YlNJc0ltRnVZMmh2Y2xWdWRHbHNJam94TmpRMU9EQXdOams0TENKa1pXeDBZVWhoYzJnaU9pSkZhVVJrUkRWR1VtRjJNMDFhYnpGWGFHNU5WRmhVV201NVJIbGpXVXR3UjNSWFVWYzBSVU5zY2poUWVsUkJJaXdpY21WamIzWmxjbmxEYjIxdGFYUnRaVzUwSWpvaVJXbENUbXh5ZEhJeVJucFFkM1pCVUZFeGMyaERPR3hGVG1GT2EyMXpTR1Z3UXpOR1JHUjJiek5EVERoWlFTSXNJbkpsWTI5MlpYSjVTMlY1SWpwN0ltTnlkaUk2SWxBdE1qVTJJaXdpYTNSNUlqb2lSVU1pTENKNElqb2lSMk5LVW10TFREQk5aVnBLYnpWTVZHRndjVTl1YmxGamNFMWZWR3d4U0RGamVteFpWMDVUYkdoMU9DSXNJbmtpT2lJeWVpMXRhRlJyT0VGcFVISmZlWEJTUmpodkxVOTFkelpKZFRaUlEwUnZhSEZMYURGeGJsVlBhelF3SW4xOS5vcDVCR3Q5cHF6UEdJQml2R1RiYzBJMkdtSzN0dmpBRzE3NERac2VkWTRMT0NHdkk3RDI3MlVGZFNZMWRIVlYxdkI4dm5vNzdUWkNadDdyY3FMVno4USIsInR5cGUiOiJyZWNvdmVyIn0=",
          "protocolVersion": 0,
          "transactionNumber": 0,
          "transactionTime": 1645800399,
          "type": "recover"
        }
      ],
      "recoveryCommitment": "EiBNlrtr2FzPwvAPQ1shC8lENaNkmsHepC3FDdvo3CL8YA",
      "updateCommitment": "EiAls4yWlFEb0XpdbUbV5i864LOU_NV0lEozrbah7k21lg"
    }
  }
}
```

### DID Parameters

Orb supports the following DID query parameters:

#### versionId	
Identifies a specific version of a DID document to be resolved. 
In orb the version ID specifies anchor hash that includes relevant DID operation. 
If present, the associated value MUST be an ASCII string.

```
GET /sidetree/v1/identifiers/did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q?versionId=uEiCRI0FqR6cFVQ8rOPOsD-muNo8k_m7mYU_RPTYLPxdBvg HTTP/1.1
Host: orb.domain2.com
Accept: application/ld+json
Accept-Encoding: gzip, deflate
```

#### versionTime	
Identifies a certain version timestamp of a DID document to be resolved. 
That is, the DID document that was valid for a DID at a certain time. 
If present, the associated value MUST be an ASCII string which is a valid XML datetime value. 
This datetime value MUST be normalized to UTC 00:00:00 and without sub-second decimal precision. 
For example: 2020-12-20T19:17:47Z. 

```
GET /sidetree/v1/identifiers/did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q?versionTime=2022-03-04T17:40:59Z HTTP/1.1
Host: orb.domain2.com
Accept: application/ld+json
Accept-Encoding: gzip, deflate
```

For more details about versionId and versionTime queries see [DID Parameters](https://www.w3.org/TR/did-core/#did-parameters)