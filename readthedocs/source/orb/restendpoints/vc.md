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
    "anchor": "hl:uEiBSN1ohlVzMayIU-ib1-TZtGnKht0t-K1GfJI4wx-WfWw",
    "href": "hl:uEiAirtU3PmCxBQI_obHgUP6FmJLNkoZ6PLHOKQIGBbQHyw",
    "profile": "https://w3id.org/orb#v0",
    "rel": "linkset",
    "type": [
      "AnchorLink"
    ]
  },
  "id": "https://orb.domain2.com/vc/57cf856f-e489-47dd-8f4e-65ea9524d6ea",
  "issuanceDate": "2022-09-19T14:14:02.112016149Z",
  "issuer": "https://orb.domain2.com",
  "proof": [
    {
      "created": "2022-09-19T14:14:02.112971919Z",
      "domain": "https://orb.domain2.com",
      "proofPurpose": "assertionMethod",
      "proofValue": "z4XcMUCpMULRSEYWZkTpvCyyraR6fJ3YonufK5Feg5pxFHgTKazTbaLXZkpSsJFj2PVWjnx9JPU2z8ASbrPmiTkDi",
      "type": "Ed25519Signature2020",
      "verificationMethod": "did:web:orb.domain2.com#VqRaibhCQnw06DZBffoiKPpNKTWYpVDsSk_PkQ9jPSk"
    },
    {
      "created": "2022-09-19T14:14:02.198Z",
      "domain": "http://orb.vct:8077/maple2020",
      "proofPurpose": "assertionMethod",
      "proofValue": "z2984NKGGSaSzzypMyA4DogiB7gz16nZvs3aHd1eEkg9SynNHWTt7ccFguwdPc6usKGxxhaXhdRvxwpPEo4arWRCq",
      "type": "Ed25519Signature2020",
      "verificationMethod": "did:web:orb.domain1.com#U8MGBdXnSkT35IBqx21m_ODAC0z3Z4sgLtwSL3JGRKQ"
    }
  ],
  "type": [
    "VerifiableCredential",
    "AnchorCredential"
  ]
}
```
