# CAS Endpoint

**Endpoint:** /cas/[id]

## GET

Returns content stored in the Content Addressable Storage (CAS). The ID is either an IPFS
[CID](https://docs.ipfs.io/concepts/content-addressing/) or the hash of the content.

**Example**

Request an [Anchor Linkset](https://trustbloc.github.io/activityanchors/#anchorlinkset) using its hash as the ID:

```
GET /cas/uEiBJQHYkD30RGKVlJ205wghkNQ_RTaZPxJkeGr5DDyaLIQ HTTP/1.1
Host: orb.domain1.com
Accept-Encoding: gzip, deflate
```

Response contains the Anchor Linkset:

```json
{
  "anchor": "hl:uEiDEHEbffPnDPPaElQs19c0PWxubBLPetTjiBAZ2ICUGYg",
  "profile": "https://w3id.org/orb#v0",
  "author": "https://orb.domain2.com/services/orb",
  "original": [
    {
      "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiDzQVPOTpOCMjT_N1WPB0PEh-oamUzrTIDQYmqWtG3XqQ%22%2C%22author%22%3A%22https%3A%2F%2Forb.domain2.com%2Fservices%2Forb%22%2C%22item%22%3A%5B%7B%22href%22%3A%22did%3Aorb%3AuEiBF2y_MJ8A5ak_6H1An5rkW2hLxUtpMCCzH1rwHKJ1-6Q%3AEiDxSI--8XryBw6wRhq6QiJWq4m9p3ckBFCT0fkdYDp6VA%22%2C%22previous%22%3A%5B%22hl%3AuEiBF2y_MJ8A5ak_6H1An5rkW2hLxUtpMCCzH1rwHKJ1-6Q%22%5D%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%3AEiBpK_86bE_LIeX4X9CMvdQ4kUol-SulAsCwjC69j_CsdQ%22%2C%22previous%22%3A%5B%22hl%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%22%5D%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiC6kYQL3Oaxssinyk5EFbbAUkUzfjh6E24dfPfqGB3CqQ%3AEiBNgkoeOaAbfhlAeCMN7qVLuQXpggIw7JfH7g3Jbc900A%22%2C%22previous%22%3A%5B%22hl%3AuEiC6kYQL3Oaxssinyk5EFbbAUkUzfjh6E24dfPfqGB3CqQ%22%5D%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiC6kYQL3Oaxssinyk5EFbbAUkUzfjh6E24dfPfqGB3CqQ%3AEiCey8gqQnAgArA5eX2EnnXXVaC8NaRLbCzt2r14DMNV4A%22%2C%22previous%22%3A%5B%22hl%3AuEiC6kYQL3Oaxssinyk5EFbbAUkUzfjh6E24dfPfqGB3CqQ%22%5D%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%3AEiA1MDpWS955wayduM4HrhUOTjj7kt9GP5yIR-s0NDW2xQ%22%2C%22previous%22%3A%5B%22hl%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%22%5D%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%3AEiB-Syt4ZfhRtlMjpwbjqEkkM36dR3ObM1W9u3ai9W3t1w%22%2C%22previous%22%3A%5B%22hl%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%22%5D%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%3AEiCk5zA2nurHM_1HmhAZq9QhWovFB0MoRPrkCgJItwIySw%22%2C%22previous%22%3A%5B%22hl%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%22%5D%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%3AEiBignYdbtZ_6zjKIpEM5OEZLVZB5x6bNnrB7uc_eugvIQ%22%2C%22previous%22%3A%5B%22hl%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%22%5D%7D%5D%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%7D%5D%7D",
      "type": "application/linkset+json"
    }
  ],
  "related": [
    {
      "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiDEHEbffPnDPPaElQs19c0PWxubBLPetTjiBAZ2ICUGYg%22%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%2C%22up%22%3A%5B%7B%22href%22%3A%22hl%3AuEiBF2y_MJ8A5ak_6H1An5rkW2hLxUtpMCCzH1rwHKJ1-6Q%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQkYyeV9NSjhBNWFrXzZIMUFuNXJrVzJoTHhVdHBNQ0N6SDFyd0hLSjEtNlE%22%7D%2C%7B%22href%22%3A%22hl%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQzBkVG1UYkg3X3RaaHlqMEc5cUtzVFg4NzF4QkdHVGg0MmNvWV9lcFd4Z0F4QmlwZnM6Ly9iYWZrcmVpZnVvdTR6ZzNkNjc2MnpxNHVwaWc2MnJreXRsN2hwbHJhcnF6aGI0bnRzcXk3eHZmbnJxYQ%22%7D%2C%7B%22href%22%3A%22hl%3AuEiC6kYQL3Oaxssinyk5EFbbAUkUzfjh6E24dfPfqGB3CqQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQzZrWVFMM09heHNzaW55azVFRmJiQVVrVXpmamg2RTI0ZGZQZnFHQjNDcVE%22%7D%5D%2C%22via%22%3A%5B%7B%22href%22%3A%22hl%3AuEiDzQVPOTpOCMjT_N1WPB0PEh-oamUzrTIDQYmqWtG3XqQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpRHpRVlBPVHBPQ01qVF9OMVdQQjBQRWgtb2FtVXpyVElEUVltcVd0RzNYcVE%22%7D%5D%7D%5D%7D",
      "type": "application/linkset+json"
    }
  ],
  "replies": [
    {
      "href": "data:application/json,%7B%22%40context%22%3A%5B%22https%3A%2F%2Fwww.w3.org%2F2018%2Fcredentials%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fjws-2020%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fed25519-2020%2Fv1%22%5D%2C%22credentialSubject%22%3A%22hl%3AuEiDEHEbffPnDPPaElQs19c0PWxubBLPetTjiBAZ2ICUGYg%22%2C%22id%22%3A%22https%3A%2F%2Forb.domain2.com%2Fvc%2Fcd75f717-d745-4280-8a5a-86061eefb023%22%2C%22issuanceDate%22%3A%222022-03-21T19%3A53%3A46.9207468Z%22%2C%22issuer%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proof%22%3A%7B%22created%22%3A%222022-03-21T19%3A53%3A46.9683797Z%22%2C%22domain%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22QlrxGFRczIkZyi1kuweqWfDgrq3squaAaZe0zLNih9QbaQH-pKy2fSszZjG3Yils3gTfedvzselIOKWqPOQaCQ%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain2.com%230lKiyMUrLLv_LEYbQeW1iZ6fGpxf7u2NWKxUX6ynsL8%22%7D%2C%22type%22%3A%22VerifiableCredential%22%7D",
      "type": "application/ld+json"
    }
  ]
}
```
