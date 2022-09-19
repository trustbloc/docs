# CAS Endpoint

**Endpoint:** /cas/[id]

## GET

Returns content stored in the Content Addressable Storage (CAS). The ID is either an IPFS
[CID](https://docs.ipfs.io/concepts/content-addressing/) or the hash of the content.

**Example**

Request an [Anchor Linkset](../model/anchorlinkset.html#anchor-linkset) using its hash as the ID:

```
GET /cas/uEiAFSmhbZSMOCK8YjpzoYrKTTrPZo5POkMGy3S6zOKnBgg HTTP/1.1
Host: orb.domain1.com
Accept-Encoding: gzip, deflate
```

Response contains the Anchor Linkset:

```json
{
  "linkset": [
    {
      "anchor": "hl:uEiBu8_7A4JQ1l9cRkxrInZ-2cxgeuoerMOueXmRI-mmfog",
      "author": [
        {
          "href": "did:web:orb.domain2.com:services:orb"
        }
      ],
      "original": [
        {
          "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiCdxlzbCM_pQopycBF8Q_KA58ioizEaWNJqlKxGrdlSjg%22%2C%22author%22%3A%5B%7B%22href%22%3A%22did%3Aweb%3Aorb.domain2.com%3Aservices%3Aorb%22%7D%5D%2C%22item%22%3A%5B%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiDO4M4BoSmK4wFL-sSmFBhOyjeWfb2IuDovY1hRov5Cgg%22%7D%5D%2C%22profile%22%3A%5B%7B%22href%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%7D%5D%7D%5D%7D",
          "type": "application/linkset+json"
        }
      ],
      "profile": [
        {
          "href": "https://w3id.org/orb#v0"
        }
      ],
      "related": [
        {
          "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiBu8_7A4JQ1l9cRkxrInZ-2cxgeuoerMOueXmRI-mmfog%22%2C%22profile%22%3A%5B%7B%22href%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%7D%5D%2C%22via%22%3A%5B%7B%22href%22%3A%22hl%3AuEiCdxlzbCM_pQopycBF8Q_KA58ioizEaWNJqlKxGrdlSjg%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ2R4bHpiQ01fcFFvcHljQkY4UV9LQTU4aW9pekVhV05KcWxLeEdyZGxTamc%22%7D%5D%7D%5D%7D",
          "type": "application/linkset+json"
        }
      ],
      "replies": [
        {
          "href": "data:application/json,%7B%22%40context%22%3A%5B%22https%3A%2F%2Fwww.w3.org%2F2018%2Fcredentials%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Factivityanchors%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fjws-2020%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fed25519-2020%2Fv1%22%5D%2C%22credentialSubject%22%3A%7B%22anchor%22%3A%22hl%3AuEiCdxlzbCM_pQopycBF8Q_KA58ioizEaWNJqlKxGrdlSjg%22%2C%22href%22%3A%22hl%3AuEiBu8_7A4JQ1l9cRkxrInZ-2cxgeuoerMOueXmRI-mmfog%22%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%2C%22rel%22%3A%22linkset%22%2C%22type%22%3A%5B%22AnchorLink%22%5D%7D%2C%22id%22%3A%22https%3A%2F%2Forb.domain2.com%2Fvc%2F65a306e4-1f86-480c-9fbd-c5e627363f0d%22%2C%22issuanceDate%22%3A%222022-08-26T15%3A26%3A20.114938493Z%22%2C%22issuer%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proof%22%3A%5B%7B%22created%22%3A%222022-08-26T15%3A26%3A20.116657916Z%22%2C%22domain%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22z63YhBwGe5h5y3ChrjTCeSXxFNf98krSDCP3FGQDRSFFCFbC7BryMpR5gaboGiaqtDRY4tNqUSZPHHHDr7jT3jSzd%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain2.com%23W51yCsfyP-3uyHi9BhTTd9qBzPM14YaQk2A0bCybRbU%22%7D%2C%7B%22created%22%3A%222022-08-26T15%3A26%3A20.243Z%22%2C%22domain%22%3A%22http%3A%2F%2Forb.vct%3A8077%2Fmaple2020%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22zr7L6vWBgVBLwPsbGnd59RWiv2t96wvwHCgRwjvLo3D69mvKpmQx6XE9qL2vR7c92LNE8xL58BAbGLWJqVb9WJBW%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain1.com%23c9lXUEfQVRceUV6FdwzzR19EMv4nZE-eopNnSmxum14%22%7D%5D%2C%22type%22%3A%5B%22VerifiableCredential%22%2C%22AnchorCredential%22%5D%7D",
          "type": "application/ld+json"
        }
      ]
    }
  ]
}
```
