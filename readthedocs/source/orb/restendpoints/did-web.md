# DID Web File Endpoint

The [DID Web](https://w3c-ccg.github.io/did-method-web/#read-resolve) file endpoint is used for resolution of did:web method documents by universal resolver and other did:web resolvers. This endpoint will serve DID document only not DID resolution result.

**Endpoint:** /scid/{id}/did.json

### GET

***Example***

Request:

```
GET /scid/EiCNuOisUYmxNfT2HugwwMYQAyzLr9FxI7tbT3eb5r3mFQ/did.json  HTTP/1.1
Host: orb.domain3.com
Accept: application/json
```

Response:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/jws-2020/v1",
    "https://w3id.org/security/suites/ed25519-2018/v1"
  ],
  "alsoKnownAs": [
    "https://myblog.example/",
    "did:orb:uEiDDV4Yn9wx0c5dlrYT90TsjB6fg2Gc6x91X1gG3O5oJIA:EiCNuOisUYmxNfT2HugwwMYQAyzLr9FxI7tbT3eb5r3mFQ",
    "did:orb:hl:uEiDDV4Yn9wx0c5dlrYT90TsjB6fg2Gc6x91X1gG3O5oJIA:uoQ-BeEtodHRwczovL29yYi5kb21haW4zLmNvbS9jYXMvdUVpRERWNFluOXd4MGM1ZGxyWVQ5MFRzakI2ZmcyR2M2eDkxWDFnRzNPNW9KSUE:EiCNuOisUYmxNfT2HugwwMYQAyzLr9FxI7tbT3eb5r3mFQ"
  ],
  "assertionMethod": [
    "did:web:orb.domain3.com:scid:EiCNuOisUYmxNfT2HugwwMYQAyzLr9FxI7tbT3eb5r3mFQ#auth"
  ],
  "authentication": [
    "did:web:orb.domain3.com:scid:EiCNuOisUYmxNfT2HugwwMYQAyzLr9FxI7tbT3eb5r3mFQ#createKey"
  ],
  "id": "did:web:orb.domain3.com:scid:EiCNuOisUYmxNfT2HugwwMYQAyzLr9FxI7tbT3eb5r3mFQ",
  "service": [
    {
      "id": "did:web:orb.domain3.com:scid:EiCNuOisUYmxNfT2HugwwMYQAyzLr9FxI7tbT3eb5r3mFQ#didcomm",
      "priority": 0,
      "recipientKeys": [
        "2FE8JwxEhd3apwqQQxpURmN4iiaM8H7cCAvU1K3D1Mmr"
      ],
      "serviceEndpoint": "https://hub.example.com/.identity/did:example:0123456789abcdef/",
      "type": "did-communication"
    }
  ],
  "verificationMethod": [
    {
      "controller": "did:web:orb.domain3.com:scid:EiCNuOisUYmxNfT2HugwwMYQAyzLr9FxI7tbT3eb5r3mFQ",
      "id": "did:web:orb.domain3.com:scid:EiCNuOisUYmxNfT2HugwwMYQAyzLr9FxI7tbT3eb5r3mFQ#createKey",
      "publicKeyJwk": {
        "crv": "P-256",
        "kty": "EC",
        "x": "0Di-FS6Y9v8QyNyswEPdHJ6HK_Yx2Ek-OLsfyEcKyLQ",
        "y": "MsQvvYkqcvRn4ndZCMU7JjTq1sUXpt3xpjaldNLiLxQ"
      },
      "type": "JsonWebKey2020"
    },
    {
      "controller": "did:web:orb.domain3.com:scid:EiCNuOisUYmxNfT2HugwwMYQAyzLr9FxI7tbT3eb5r3mFQ",
      "id": "did:web:orb.domain3.com:scid:EiCNuOisUYmxNfT2HugwwMYQAyzLr9FxI7tbT3eb5r3mFQ#auth",
      "publicKeyBase58": "XpPrnB4DAiQTXL2dDCoenje8aVtocY3FqMFcy8w6fBK",
      "type": "Ed25519VerificationKey2018"
    }
  ]
}
```

