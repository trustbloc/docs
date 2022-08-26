# VC Endpoints

**Endpoint:** /vc/[id]

## GET

Returns the verifiable credential for the given ID.

**Example**

```
GET /vc/65a306e4-1f86-480c-9fbd-c5e627363f0d HTTP/1.1
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
    "https://w3id.org/security/suites/jws-2020/v1",
    "https://w3id.org/security/suites/ed25519-2020/v1"
  ],
  "credentialSubject": {
    "anchor": "hl:uEiCdxlzbCM_pQopycBF8Q_KA58ioizEaWNJqlKxGrdlSjg",
    "href": "hl:uEiBu8_7A4JQ1l9cRkxrInZ-2cxgeuoerMOueXmRI-mmfog",
    "profile": "https://w3id.org/orb#v0",
    "rel": "linkset",
    "type": [
      "AnchorLink"
    ]
  },
  "id": "https://orb.domain2.com/vc/65a306e4-1f86-480c-9fbd-c5e627363f0d",
  "issuanceDate": "2022-08-26T15:26:20.114938493Z",
  "issuer": "https://orb.domain2.com",
  "proof": [
    {
      "created": "2022-08-26T15:26:20.116657916Z",
      "domain": "https://orb.domain2.com",
      "proofPurpose": "assertionMethod",
      "proofValue": "z63YhBwGe5h5y3ChrjTCeSXxFNf98krSDCP3FGQDRSFFCFbC7BryMpR5gaboGiaqtDRY4tNqUSZPHHHDr7jT3jSzd",
      "type": "Ed25519Signature2020",
      "verificationMethod": "did:web:orb.domain2.com#W51yCsfyP-3uyHi9BhTTd9qBzPM14YaQk2A0bCybRbU"
    },
    {
      "created": "2022-08-26T15:26:20.243Z",
      "domain": "http://orb.vct:8077/maple2020",
      "proofPurpose": "assertionMethod",
      "proofValue": "zr7L6vWBgVBLwPsbGnd59RWiv2t96wvwHCgRwjvLo3D69mvKpmQx6XE9qL2vR7c92LNE8xL58BAbGLWJqVb9WJBW",
      "type": "Ed25519Signature2020",
      "verificationMethod": "did:web:orb.domain1.com#c9lXUEfQVRceUV6FdwzzR19EMv4nZE-eopNnSmxum14"
    }
  ],
  "type": [
    "VerifiableCredential",
    "AnchorCredential"
  ]
}
```
