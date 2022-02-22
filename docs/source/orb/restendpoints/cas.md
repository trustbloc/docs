# CAS Endpoint

**Endpoint:** /cas/[id]

### GET

Returns content stored in the Content Addressable Storage (CAS). The ID is either an IPFS
[CID](https://docs.ipfs.io/concepts/content-addressing/) or the hash of the content.

**Example**

Request an [AnchorEvent](https://trustbloc.github.io/activityanchors/#anchorevent) using its hash as the ID:

```
GET /cas/uEiD6mDyC3YodyjfNfmIE3jYzGSDalDqj8Syj-wY4IAC8yA HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains the [AnchorEvent](https://trustbloc.github.io/activityanchors/#anchorevent):

```json
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://w3id.org/activityanchors/v1"
  ],
  "attachment": [
    {
      "content": "{\"properties\":{\"https://w3id.org/activityanchors#generator\":\"https://w3id.org/orb#v0\",\"https://w3id.org/activityanchors#resources\":[{\"id\":\"did:orb:uAAA:EiBfWqeAJfENeHLABsYIYmsIqtk-bsmvJmoR6IgISd6ZcA\"},{\"id\":\"did:orb:uAAA:EiAdTRM3nwkp6lsn2n17_T-t7j4xT4IJxSXgfBCB3exZ2g\"},{\"id\":\"did:orb:uAAA:EiDllWjlTcoNyL5roXFRzdnWvygPT5FHbISVjwDNAD98tA\"},{\"id\":\"did:orb:uAAA:EiC6Sjz56-iawx_CUl3xoIykVG1iKtvh78zRl93WHFkQdA\"},{\"id\":\"did:orb:uAAA:EiADvRR2cTIv8si57Tl2puoqpIWK41L9rb9aVF2gdrKhyg\"}]},\"subject\":\"hl:uEiCVVS-n0wx0OfeEXBM9jcGNOcMEArYYWPIxk5D_l96ySg:uoQ-BeEJpcGZzOi8vYmFma3JlaWV2a3V4MnB1eW1vcTQ3cGJjNGNtNnkzcW1uaGhicWlhdndkYm1wZW1tdHNkN3pweHZzamk\"}",
      "generator": "https://w3id.org/orb#v0",
      "mediaType": "application/json",
      "tag": [
        {
          "href": "hl:uEiDSkSE5Oxl6Z6Tr6wGMW1dnlC6HaZrcGURNo6dSeQ2jzA",
          "rel": [
            "witness"
          ],
          "type": "Link"
        }
      ],
      "type": "AnchorObject",
      "url": "hl:uEiD96Zjp10atu2DPV1VbSxZmMvk5r5iAvSlnXXAfpkpY9g"
    },
    {
      "content": "{\"@context\":[\"https://www.w3.org/2018/credentials/v1\"],\"credentialSubject\":\"hl:uEiD96Zjp10atu2DPV1VbSxZmMvk5r5iAvSlnXXAfpkpY9g\",\"id\":\"https://orb.domain2.com/vc/afeead95-8507-460b-a73c-cf9b4853b4a9\",\"issuanceDate\":\"2022-02-22T15:45:41.6601759Z\",\"issuer\":\"https://orb.domain2.com\",\"proof\":[{\"created\":\"2022-02-22T15:45:41.6613096Z\",\"domain\":\"https://orb.domain2.com\",\"jws\":\"eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..2Sp1wXzI69pontDt-x26KwEBFhEzif33io9lIEHEEBRHgiLAFr4D6XDDVyjMmSCXWWuYBMZUFWwwHJkpPi08Dg\",\"proofPurpose\":\"assertionMethod\",\"type\":\"Ed25519Signature2018\",\"verificationMethod\":\"did:web:orb.domain2.com#orb2key\"},{\"created\":\"2022-02-22T15:45:42.664Z\",\"domain\":\"http://orb.vct:8077/maple2020\",\"jws\":\"eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19.._vShdZ5z_YKjWfKPt5zgTJpUoYx1pMfjETiFVHHVsZrUlAM9CXLSjrRAzGicT4uqTVFqurrpsQstPAABpm_DDQ\",\"proofPurpose\":\"assertionMethod\",\"type\":\"Ed25519Signature2018\",\"verificationMethod\":\"did:web:orb.domain1.com#orb1key2\"}],\"type\":\"VerifiableCredential\"}",
      "generator": "https://w3id.org/orb#v0",
      "mediaType": "application/json",
      "type": "AnchorObject",
      "url": "hl:uEiDSkSE5Oxl6Z6Tr6wGMW1dnlC6HaZrcGURNo6dSeQ2jzA"
    }
  ],
  "attributedTo": "https://orb.domain2.com/services/orb",
  "index": "hl:uEiD96Zjp10atu2DPV1VbSxZmMvk5r5iAvSlnXXAfpkpY9g",
  "published": "2022-02-22T15:45:41.6437395Z",
  "type": "AnchorEvent"
}
```
