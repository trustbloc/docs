# VC Endpoints

**Endpoint:** /vc/[id]

## GET

Returns the verifiable credential for the given ID.

**Example**

```
GET /vc/ade8c667-e143-42a4-af6e-a102265299f5 HTTP/1.1
Host: orb.domain2.com
Accept: application/ld+json
Accept-Encoding: gzip, deflate
```

Response contains the verifiable credential:

```json
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://w3id.org/security/suites/jws-2020/v1",
    "https://w3id.org/security/suites/ed25519-2020/v1"
  ],
  "credentialSubject": "hl:uEiAlCGTSINVXO2RrUrPjHbN6Z9E1VEzZwEdLv48yBhIbiQ",
  "id": "https://orb.domain2.com/vc/ade8c667-e143-42a4-af6e-a102265299f5",
  "issuanceDate": "2022-06-14T19:54:13.2628467Z",
  "issuer": "https://orb.domain2.com",
  "proof": [
    {
      "created": "2022-06-14T19:54:13.2646991Z",
      "domain": "https://orb.domain2.com",
      "proofPurpose": "assertionMethod",
      "proofValue": "z5LTTbjgBNT74x8QHF9hWfYcLjFJiV25KEgJ5YySvhS7XNqZiDAwUEVN5P4tpDFQj15sqLuaue4zP9WminCU4bHPe",
      "type": "Ed25519Signature2020",
      "verificationMethod": "did:web:orb.domain2.com#ErsYVzrxx3ONUNH37PzYHXPRevXG-01BgXuRafrD2iU"
    },
    {
      "created": "2022-06-14T19:54:13.956Z",
      "domain": "http://orb.vct:8077/maple2020",
      "proofPurpose": "assertionMethod",
      "proofValue": "z3eFSQag3NeUJpR24WbiZjeF9NoQwMSBiBWmUTEQDhivaa9PwEhE85qHkBHbFa8iUGYKmmcsxHHAFQ8kNeK76yQ9f",
      "type": "Ed25519Signature2020",
      "verificationMethod": "did:web:orb.domain1.com#91aeXdgP2yu8SEzX3fPkSiwnRj_z6_ySIj4U2Ai7fPo"
    }
  ],
  "type": "VerifiableCredential"
}
```
