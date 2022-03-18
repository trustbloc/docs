# CAS Endpoint

**Endpoint:** /cas/[id]

### GET

Returns content stored in the Content Addressable Storage (CAS). The ID is either an IPFS
[CID](https://docs.ipfs.io/concepts/content-addressing/) or the hash of the content.

**Example**

Request an [Anchor Linkset](https://trustbloc.github.io/activityanchors/#anchorlinkset) using its hash as the ID:

```
GET /cas/uEiBJQHYkD30RGKVlJ205wghkNQ_RTaZPxJkeGr5DDyaLIQ HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains the Anchor Linkset:

```json
{
  "linkset": [
    {
      "anchor": "hl:uEiBY85v7gtFoJ_K0iIONZcxGas3Kg0FrKidJdvEIt8h4Pg",
      "author": "https://orb.domain2.com/services/orb",
      "original": [
        {
          "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiDkZV2rw2XKdqPoGKy6Vg0HEk36HfsxyWfnY3jp3Q2K-g%22%2C%22author%22%3A%22https%3A%2F%2Forb.domain2.com%2Fservices%2Forb%22%2C%22item%22%3A%5B%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiBTRfNkzKwW3ZVDl6WwXsYhre6HPE8jQ7e9l3m6pii-iw%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiCUt537plK2HuI0k2rQNP3MgDtq1T5Wj_LZ7yQOJY2gcA%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiDpy0--tFbSNGG-UNce9_yXfRBFQ9kkme8SREFLUiaK6g%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiAmF0h5uxCI14AEPUfkgNzGbodfJtUlQVf0V7IWzPrPGg%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiAeNcjw65SIWS9qPm7MlFF-rYu-lKhKq16obJ-6LqOpaQ%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiDlaV0jjvy8JlBJ-Ugge47QOqBA0KxnAhHghcWKwLvWwQ%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiDIo4o6dD0oGBPtdYITyXWoXqNJ3OC74dgw2oocdz7AOA%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiDZ5gEl8ZiENuuQ5dw0ozfmesntsUne37vHKMkO0e9qHw%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiDL3vGRVNQw98-TJjVWfJX9cAPZTB2YlYSE2VLTktkUbA%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiAVOBVLnwIgF6f83JfJUi82mhfAQ2oVKpsZCfkt5tX2aQ%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiASpBh5wHkd2avGkUvsGku0NfqUreNU6oJtaRjrF2B3Iw%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiCPO-Xfxe4PblukhJSDpN87v-fGKX74eiUmcDm_lj_7Qg%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiDd0lK5lHAn1JxHFQXbcYoQubWSNNHKDpSPg6jEpfkAhQ%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiB9sRB7EfxBBRY7JxGdRKnbkE_b2Q9UYlZ6fY5jk8_CUw%22%7D%5D%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%7D%5D%7D",
          "type": "application/linkset+json"
        }
      ],
      "profile": "https://w3id.org/orb#v0",
      "related": [
        {
          "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiBY85v7gtFoJ_K0iIONZcxGas3Kg0FrKidJdvEIt8h4Pg%22%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%2C%22via%22%3A%5B%7B%22href%22%3A%22hl%3AuEiDkZV2rw2XKdqPoGKy6Vg0HEk36HfsxyWfnY3jp3Q2K-g%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpRGtaVjJydzJYS2RxUG9HS3k2VmcwSEVrMzZIZnN4eVdmblkzanAzUTJLLWc%22%7D%5D%7D%5D%7D",
          "type": "application/linkset+json"
        }
      ],
      "replies": [
        {
          "href": "data:application/json,%7B%22%40context%22%3A%5B%22https%3A%2F%2Fwww.w3.org%2F2018%2Fcredentials%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fed25519-2020%2Fv1%22%5D%2C%22credentialSubject%22%3A%22hl%3AuEiBY85v7gtFoJ_K0iIONZcxGas3Kg0FrKidJdvEIt8h4Pg%22%2C%22id%22%3A%22https%3A%2F%2Forb.domain2.com%2Fvc%2Fc8355893-0630-47a9-9135-b120149b3f9f%22%2C%22issuanceDate%22%3A%222022-03-18T14%3A39%3A00.2770898Z%22%2C%22issuer%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proof%22%3A%5B%7B%22created%22%3A%222022-03-18T14%3A39%3A00.2776418Z%22%2C%22domain%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22rgdMdUcfEB58YUTbRrJJHbDTlisgbW6qCEI4nU-DKxswZthLlZuvSQO_NRScedk4bLVh3UTgyY3ZP2QOb4K0Cw%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain2.com%23HmSDhi2uFacLdbmBpzuns2vZ8fIUZ30qaz2GMRzyhHs%22%7D%2C%7B%22created%22%3A%222022-03-18T14%3A39%3A00.558Z%22%2C%22domain%22%3A%22http%3A%2F%2Forb.vct%3A8077%2Fmaple2020%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22UKcectK_atESBKdt1B7zH5E_j2CkIyd0g2MoKVPY91APZdh-73qjv8ySPtdomveKoD0njOf14C3e9-tBeBITBg%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain1.com%23_pmWt-9hg6jjE5E_Ph22D3nDcJMIX-i9okW0tYJ3cy4%22%7D%5D%2C%22type%22%3A%22VerifiableCredential%22%7D",
          "type": "application/ld+json"
        }
      ]
    }
  ]
}
```
