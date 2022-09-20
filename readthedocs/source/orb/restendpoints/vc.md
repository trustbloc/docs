# VC Endpoints

**Endpoint:** /vc/[id]

## GET

Returns the verifiable credential for the given ID.

**Example**

```
GET /vc/57cf856f-e489-47dd-8f4e-65ea9524d6ea HTTP/1.1
Host: orb.domain2.com
Accept: application/ld+json
Accept-Encoding: gzip, deflate
```

Response contains the verifiable credential:

```json
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://w3id.org/activityanchors/v1",
    "https://w3id.org/security/suites/ed25519-2020/v1"
  ],
  "credentialSubject": {
    "anchor": "hl:uEiCJYS5Jin-3ZSwBT_RT0c0zZ0Z6B3_o0ddDDCMZXlUxAQ",
    "href": "hl:uEiAIMnHwbdHbpWbL3OlruU1xtqW-Potpi0bW0ioXFCZ94w",
    "profile": "https://w3id.org/orb#v0",
    "rel": "linkset",
    "type": [
      "AnchorLink"
    ]
  },
  "id": "https://orb.domain5.com/vc/2a649ce3-3412-4ca7-953c-bf89e333c3db",
  "issuanceDate": "2022-09-20T20:57:11.776289881Z",
  "issuer": "did:web:orb.domain5.com:services:anchor",
  "proof": [
    {
      "created": "2022-09-20T20:57:11.782036216Z",
      "domain": "https://orb.domain5.com",
      "proofPurpose": "assertionMethod",
      "proofValue": "z3VUDMo8tksYYD14crKHQx7HZLtJPwi7V4g2az1puHDhRYxTXSFQ3a2Qch7Az8niyQ1TXdKKaWzjvXoYNFUbJJmr9",
      "type": "Ed25519Signature2020",
      "verificationMethod": "did:web:orb.domain5.com#H72rWxdnDTaf69z2OkDLUG0z7XtFkBY_WmG9__U060Y"
    },
    {
      "created": "2022-09-20T20:57:11.966Z",
      "domain": "http://orb.vct:8077/maple2020",
      "proofPurpose": "assertionMethod",
      "proofValue": "z54WJu6r64W6Fq4LfiVzojHYHkwo4aVEMQ1KA15XYUhqwZPFwByqeV9Dwi3UPFPfKUsUsVUh92edAD1nGsd6n2nf6",
      "type": "Ed25519Signature2020",
      "verificationMethod": "did:web:orb.domain1.com#K3CezR1_tXyZSNbgdFbikNxkPCypfZ1bcJFh9NcsJlk"
    }
  ],
  "type": [
    "VerifiableCredential",
    "AnchorCredential"
  ]
}
```
