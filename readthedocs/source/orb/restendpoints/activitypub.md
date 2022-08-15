# ActivityPub Endpoints

The ActivityPub spec defines a number of REST endpoints that provide information about a [service](https://www.w3.org/TR/activitypub/#actor-objects).
The [ActivityAnchors](https://trustbloc.github.io/activityanchors/#actor-discovery) spec extends the ActivityPub spec to provide additional endpoints.

## Service

The REST endpoints for a service depend on the value of startup parameter [service-id](../parameters.html#service-id).
By default, the endpoint is */services/orb*, but if, for example, *service-id* is set to *did:web:orb.domain1.com:services:anchor*,
then the service REST endpoint will be */services/anchor*. 

**Endpoint:** /<service-path>

The returned data is a JSON document that contains REST endpoints that may be queried to return additional information.

### GET

**Example**

Assuming that the default value for service-id is used, then the Orb service is retrieved using the
*/services/orb* endpoint.

Request:

```
GET /services/orb HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains the [service](https://trustbloc.github.io/activityanchors/#actor-discovery):

```json
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://w3id.org/security/v1",
    "https://w3id.org/activityanchors/v1"
  ],
  "id": "https://orb.domain1.com/services/orb",
  "type": "Service",
  "followers": "https://orb.domain1.com/services/orb/followers",
  "following": "https://orb.domain1.com/services/orb/following",
  "inbox": "https://orb.domain1.com/services/orb/inbox",
  "liked": "https://orb.domain1.com/services/orb/liked",
  "likes": "https://orb.domain1.com/services/orb/likes",
  "outbox": "https://orb.domain1.com/services/orb/outbox",
  "publicKey": {
    "id": "https://orb.domain1.com/services/orb/keys/main-key",
    "owner": "https://orb.domain1.com/services/orb",
    "publicKeyPem": "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhki....."
  },
  "shares": "https://orb.domain1.com/services/orb/shares",
  "witnesses": "https://orb.domain1.com/services/orb/witnesses",
  "witnessing": "https://orb.domain1.com/services/orb/witnessing"
}
```

**Example**

Assuming that the value of the service-id parameter is set to *did:web:orb.domain1.com:services:anchor*, then the Orb
service is retrieved using the */services/anchor* endpoint.

Request:

```
GET /services/anchor HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains the [service](https://trustbloc.github.io/activityanchors/#actor-discovery):

```json
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://w3id.org/security/v1",
    "https://w3id.org/activityanchors/v1"
  ],
  "followers": "https://orb.domain1.com/services/anchor/followers",
  "following": "https://orb.domain1.com/services/anchor/following",
  "id": "did:web:orb.domain1.com:services:anchor",
  "publicKey": "did:web:orb.domain1.com:services:anchor#MrWUYMOS4ZGF7LCBVSr8njn98OvG70XaLV31EDFhBH0",
  "inbox": "https://orb.domain1.com/services/anchor/inbox",
  "liked": "https://orb.domain1.com/services/anchor/liked",
  "likes": "https://orb.domain1.com/services/anchor/likes",
  "outbox": "https://orb.domain1.com/services/anchor/outbox",
  "shares": "https://orb.domain1.com/services/anchor/shares",
  "type": "Service",
  "witnesses": "https://orb.domain1.com/services/anchor/witnesses",
  "witnessing": "https://orb.domain1.com/services/anchor/witnessing"
}
```

## Keys

**Endpoint:** /services/orb/keys/[id]

Note: This endpoint is unavailable if a DID is used as the public key, for example,

```json
{
  . . .
  "publicKey": "did:web:orb.domain2.com:services:anchor#MrWUYMOS4ZGF7LCBVSr8njn98OvG70XaLV31EDFhBH0",
  . . .
}
```

In this case, a DID resolver is required to resolve the public key.

### GET

The public key of an Orb service is retrieved using this endpoint.

**Example**

Request:

```
GET /services/orb/keys/main-key HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains the public key of the service:

```json
{
  "id": "https://orb.domain1.com/services/orb/keys/main-key",
  "owner": "https://orb.domain1.com/services/orb",
  "publicKeyPem": "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhki....."
}
```

## did.json

**Endpoint:** /services/orb/did.json

If the value of startup parameter [service-id](../parameters.html#service-id) is set to a DID, for example,
*did:web:orb.domain2.com:services:orb*, then the DID document for the service may be resolved at this endpoint.

### GET

**Example**

Request:

```
GET /services/orb/did.json HTTP/1.1
Host: orb.domain2.com
Accept: application/ld+json
Accept-Encoding: gzip, deflate
```

Response contains the DID document for the service, which includes the public key(s) for HTTP signatures
and the service endpoint:

```json
{
  "@context": [
    "https://w3id.org/did/v1",
    "https://identity.foundation/.well-known/did-configuration/v1"
  ],
  "id": "did:web:orb.domain2.com:services:orb",
  "verificationMethod": [
    {
      "controller": "did:web:orb.domain2.com:services:orb",
      "id": "did:web:orb.domain2.com:services:orb#MrWUYMOS4ZGF7LCBVSr8njn98OvG70XaLV31EDFhBH0",
      "publicKeyMultibase": "z7a7SJkF8LTqGACTxB8r86i2btE7y8qrPnGuCe3M9vk8f",
      "type": "Ed25519VerificationKey2020"
    }
  ],
  "service": [
    {
      "id": "did:web:orb.domain2.com:services:orb#activity-pub",
      "serviceEndpoint": "https://orb.domain2.com",
      "type": "LinkedDomains"
    }
  ]
}
```

## Followers

**Endpoint:** /services/orb/followers

### GET

The followers of this Orb service are returned via this endpoint. If no paging parameters are specified in the
URL then the response contains information about the collection, i.e. the links to the first and last page, as
well as the total number of items in the collection. A subsequent request may be made using parameters
that include a specified page number in order to retrieve the actual items.

**Example**

Request page information:

```
GET /services/orb/followers HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains page information:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/followers",
  "type": "Collection",
  "totalItems": 19,
  "first": "https://orb.domain1.com/services/orb/followers?page=true",
  "last": "https://orb.domain1.com/services/orb/followers?page=true&page-num=4"
}
```

Request first page:

```
GET /services/orb/followers?page=true&page-num=0 HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains items in the first page:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/followers?page=true&page-num=0",
  "type": "CollectionPage",
  "totalItems": 19,
  "next": "https://orb.domain1.com/services/orb/followers?page=true&page-num=1",
  "items": [
    "https://orb.domain2.com/services/orb",
    "https://orb.domain3.com/services/orb",
    "https://orb.domain4.com/services/orb"
  ]
}
```

## Following

**Endpoint:** /services/orb/following

### GET

The services following this Orb service are returned via this endpoint. If no paging parameters are specified
in the URL then the response contains information about the collection, i.e. the links to the first and last page, as
well as the total number of items in the collection. A subsequent request may be made using parameters
that include a specified page number in order to retrieve the actual items.

**Example**

Request:

```
GET /services/orb/following HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains page information:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/following",
  "type": "Collection",
  "totalItems": 19,
  "first": "https://orb.domain1.com/services/orb/following?page=true",
  "last": "https://orb.domain1.com/services/orb/following?page=true&page-num=4"
}
```

Request first page:

```
GET /services/orb/following?page=true&page-num=0 HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains items from the first page:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/following?page=true&page-num=0",
  "type": "CollectionPage",
  "totalItems": 19,
  "next": "https://orb.domain1.com/services/orb/following?page=true&page-num=1",
  "items": [
    "https://orb.domain2.com/services/orb",
    "https://orb.domain3.com/services/orb",
    "https://orb.domain4.com/services/orb"
  ]
}
```

## Outbox

**Endpoint:** /services/orb/outbox

### GET

A GET request to the outbox endpoint returns the activities that were posted to a service's Outbox.
This endpoint is restricted by authorization rules, i.e. the requester must have a valid
authorization bearer token or must be verified using HTTP signatures and also must be in the
_following_ or _witnesses_ collection. Although, any activity sent to a [public](https://www.w3.org/TR/activitypub/#public-addressing) URI,
is returned without authorization.

If no paging parameters are specified in the URL then the response contains information about the outbox collection, i.e. the links
to the first and last page, as well as the total number of items in the collection. A subsequent request may be made
using parameters that include a specified page number in order to retrieve the actual items.

**Example**

Request page information:

```
GET /services/orb/outbox HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains page information:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/outbox",
  "type": "OrderedCollection",
  "totalItems": 3212,
  "first": "https://orb.domain1.com/services/orb/outbox?page=true",
  "last": "https://orb.domain1.com/services/orb/outbox?page=true&page-num=76"
}
```

Request page 41:

```
GET /services/orb/outbox?page=true&page-num=41 HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains items from page 41:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/outbox?page=true&page-num=41",
  "type": "OrderedCollectionPage",
  "totalItems": 321,
  "next": "https://orb.domain1.com/services/orb/outbox?page=true&page-num=42",
  "orderedItems": [
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain1.com/services/orb",
      "id": "https://orb.domain1.com/services/orb/activities/72c49978-0812-4c07-83ac-a39c9faf1a0a",
      "object": "https://orb.domain2.com/services/orb",
      "to": "https://orb.domain2.com/services/orb",
      "type": "Follow"
    },
    {
      "@context": [
        "https://www.w3.org/ns/activitystreams",
        "https://w3id.org/activityanchors/v1"
      ],
      "actor": "https://orb.domain1.com/services/orb",
      "id": "https://orb.domain1.com/services/orb/activities/d5c75aa5-9d19-43ae-92e1-21ae7c6cf67e",
      "object": "https://w3id.org/activityanchors#AnchorWitness",
      "target": "https://orb.domain2.com/services/orb",
      "to": "https://orb.domain2.com/services/orb",
      "type": "Invite"
    },
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain1.com/services/orb",
      "id": "https://orb.domain1.com/services/orb/activities/db09985e-6d1b-4c9b-870e-bd4c4aa3fec5",
      "object": {
        "@context": "https://www.w3.org/ns/activitystreams",
        "actor": "https://orb.domain2.com/services/orb",
        "id": "https://orb.domain2.com/services/orb/activities/ba65fbd8-ef59-4f69-a3d0-0ba6a115c650",
        "object": "https://orb.domain1.com/services/orb",
        "to": "https://orb.domain1.com/services/orb",
        "type": "Follow"
      },
      "to": "https://orb.domain2.com/services/orb",
      "type": "Accept"
    },
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain1.com/services/orb",
      "id": "https://orb.domain1.com/services/orb/activities/5a2efe8a-ea7f-4b85-a58c-bdcba86cbb32",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "object": {
          "linkset": [
            {
              "anchor": "hl:uEiCbb4bC2VYhbQMtci_sE3OysFW9r-jbss5yM9F2mjYnVQ",
              "author": "https://orb.domain1.com/services/orb",
              "original": [
                {
                  "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiClKstiM9lzsB-XnDpSThBcpRc62ah4NoOsByxafcE-BQ%22%2C%22author%22%3A%22https%3A%2F%2Forb.domain2.com%2Fservices%2Forb%22%2C%22item%22%3A%5B%7B%22href%22%3A%22did%3Aorb%3AuEiAFbqLS7qyiDVPwsUgk1k5gXzill2codevVaNIio99FFA%3AEiDuEd9k8dT_8DD7MmN3ZDPVyynVp7dw0TE97zJO3ViotQ%22%2C%22previous%22%3A%5B%22hl%3AuEiAFbqLS7qyiDVPwsUgk1k5gXzill2codevVaNIio99FFA%22%5D%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiC6kYQL3Oaxssinyk5EFbbAUkUzfjh6E24dfPfqGB3CqQ%3AEiCwzIfeommz_je33aQc3egz9InPgLnqyVk78xqakyiroQ%22%2C%22previous%22%3A%5B%22hl%3AuEiC6kYQL3Oaxssinyk5EFbbAUkUzfjh6E24dfPfqGB3CqQ%22%5D%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiC6kYQL3Oaxssinyk5EFbbAUkUzfjh6E24dfPfqGB3CqQ%3AEiCugDLBgcTfR17JV61QF3QmiYbDrpgya1K3-nj9wcZAfw%22%2C%22previous%22%3A%5B%22hl%3AuEiC6kYQL3Oaxssinyk5EFbbAUkUzfjh6E24dfPfqGB3CqQ%22%5D%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%3AEiC5AR9oibSJfWpG0SjLFHBLUlElmWpLx3LbSUCLOMfdOg%22%2C%22previous%22%3A%5B%22hl%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%22%5D%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiDFQjpUbwsZgf3ER9TkfRulkEp_d4265BK5TBVW_TE7hA%3AEiAGoqmCKktOdP-gFWTDIyuGTI1v9g7BM2A3-GGzElpbiA%22%2C%22previous%22%3A%5B%22hl%3AuEiDFQjpUbwsZgf3ER9TkfRulkEp_d4265BK5TBVW_TE7hA%22%5D%7D%5D%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%7D%5D%7D",
                  "type": "application/linkset+json"
                }
              ],
              "profile": "https://w3id.org/orb#v0",
              "related": [
                {
                  "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiCbb4bC2VYhbQMtci_sE3OysFW9r-jbss5yM9F2mjYnVQ%22%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%2C%22up%22%3A%5B%7B%22href%22%3A%22hl%3AuEiAFbqLS7qyiDVPwsUgk1k5gXzill2codevVaNIio99FFA%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQUZicUxTN3F5aURWUHdzVWdrMWs1Z1h6aWxsMmNvZGV2VmFOSWlvOTlGRkF4QmlwZnM6Ly9iYWZrcmVpYWZuMnJuZjN2bXVpZ3ZoNGZyamFzbm10dGFsNDRrbGYzaGZiMjZ4dmxpMmlya2h4MmZjcQ%22%7D%2C%7B%22href%22%3A%22hl%3AuEiC6kYQL3Oaxssinyk5EFbbAUkUzfjh6E24dfPfqGB3CqQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQzZrWVFMM09heHNzaW55azVFRmJiQVVrVXpmamg2RTI0ZGZQZnFHQjNDcVE%22%7D%2C%7B%22href%22%3A%22hl%3AuEiC0dTmTbH7_tZhyj0G9qKsTX871xBGGTh42coY_epWxgA%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQzBkVG1UYkg3X3RaaHlqMEc5cUtzVFg4NzF4QkdHVGg0MmNvWV9lcFd4Z0F4QmlwZnM6Ly9iYWZrcmVpZnVvdTR6ZzNkNjc2MnpxNHVwaWc2MnJreXRsN2hwbHJhcnF6aGI0bnRzcXk3eHZmbnJxYQ%22%7D%2C%7B%22href%22%3A%22hl%3AuEiDFQjpUbwsZgf3ER9TkfRulkEp_d4265BK5TBVW_TE7hA%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpREZRanBVYndzWmdmM0VSOVRrZlJ1bGtFcF9kNDI2NUJLNVRCVldfVEU3aEE%22%7D%5D%2C%22via%22%3A%5B%7B%22href%22%3A%22hl%3AuEiClKstiM9lzsB-XnDpSThBcpRc62ah4NoOsByxafcE-BQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ2xLc3RpTTlsenNCLVhuRHBTVGhCY3BSYzYyYWg0Tm9Pc0J5eGFmY0UtQlE%22%7D%5D%7D%5D%7D",
                  "type": "application/linkset+json"
                }
              ],
              "replies": [
                {
                  "href": "data:application/json,%7B%22%40context%22%3A%5B%22https%3A%2F%2Fwww.w3.org%2F2018%2Fcredentials%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fjws-2020%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fed25519-2020%2Fv1%22%5D%2C%22credentialSubject%22%3A%22hl%3AuEiCbb4bC2VYhbQMtci_sE3OysFW9r-jbss5yM9F2mjYnVQ%22%2C%22id%22%3A%22https%3A%2F%2Forb.domain2.com%2Fvc%2F4bc3c6f6-47e0-4319-b9db-aabc02703504%22%2C%22issuanceDate%22%3A%222022-03-21T19%3A53%3A46.9426415Z%22%2C%22issuer%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proof%22%3A%5B%7B%22created%22%3A%222022-03-21T19%3A53%3A46.9433799Z%22%2C%22domain%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22jKU4JefCffpwrcMW6Z3Lg7Nq2zwZAAOUmTIsQ-M7y-87WJFcIJLu4mY1fe-FfzRzyH12WQSw6dk4H8J72AOCBQ%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain2.com%230lKiyMUrLLv_LEYbQeW1iZ6fGpxf7u2NWKxUX6ynsL8%22%7D%2C%7B%22created%22%3A%222022-03-21T19%3A53%3A47.48Z%22%2C%22domain%22%3A%22http%3A%2F%2Forb.vct%3A8077%2Fmaple2020%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22vFvEicqtNPtJlcIUnoDFnGSRQGJrYxdWkV3Zn3EXmbg75WQp1mL9-bw2-9EYkgaw2g_cgreq_lbEe8JvNiLhCg%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain1.com%23fFqABxOfNIE903WpSg6hjnZwhW4RWmyt1SrtFQ4xTP4%22%7D%5D%2C%22type%22%3A%22VerifiableCredential%22%7D",
                  "type": "application/ld+json"
                }
              ]
            }
          ]
        },
        "type": "AnchorEvent",
        "url": "hl:uEiAFnozcDJ94h_Cr--d_bpmmuCRWh8tC2HasXJmpyN5vOA:uoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQUZub3pjREo5NGhfQ3ItLWRfYnBtbXVDUldoOHRDMkhhc1hKbXB5TjV2T0E"
      },
      "published": "2022-03-21T19:53:48.0607948Z",
      "to": [
        "https://orb.domain2.com/services/orb/followers",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Create"
    }
  ]
}
```

### POST

A POST request to the outbox endpoint adds the activity contained in the request to the service's Outbox,
which will be processed by the ActivityPub [Outbox](../system/activitypub.html#outbox-inbox). This endpoint is restricted by
authorization rules, i.e. the requester must have a valid authorization bearer token,
which is usually an administrator token.

**Example**

Post a _Follow_ activity:

```
POST /services/orb/outbox HTTP/1.1
Host: orb.domain2.com
Content-Type: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate

{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/activities/91dbb2e2-1040-4fd5-bd2e-1bcd67bdda8a",
  "type": "Follow",
  "actor": "https://orb.domain1.com/services/orb",
  "to": "https://orb.domain2.com/services/orb",
  "object": "https://orb.domain2.com/services/orb"
}
```

## Inbox

**Endpoint:** /services/orb/inbox

### GET

The activities posted to the inbox of this service are returned via this endpoint. If no paging parameters are specified
in the URL then the response contains information about the inbox collection, i.e. the links to the first and last page, as
well as the total number of items in the inbox. A subsequent request may be made using parameters
that include a specified page number in order to retrieve the actual items.

**Example**

Request page information:

```
GET /services/orb/inbox HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains page information:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/inbox",
  "type": "OrderedCollection",
  "totalItems": 19,
  "first": "https://orb.domain1.com/services/orb/inbox?page=true",
  "last": "https://orb.domain1.com/services/orb/inbox?page=true&page-num=4"
}
```

Request first page:

```
GET /services/orb/inbox?page=true HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains items from the first page:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/inbox?page=true&page-num=0",
  "type": "OrderedCollectionPage",
  "totalItems": 19,
  "next": "https://orb.domain1.com/services/orb/inbox?page=true&page-num=1",
  "orderedItems": [
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain2.com/services/orb",
      "id": "https://orb.domain2.com/services/orb/activities/ba65fbd8-ef59-4f69-a3d0-0ba6a115c650",
      "object": "https://orb.domain1.com/services/orb",
      "to": "https://orb.domain1.com/services/orb",
      "type": "Follow"
    },
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain2.com/services/orb",
      "id": "https://orb.domain2.com/services/orb/activities/29cdbde2-d82a-4594-8fbc-26292316c0af",
      "object": {
        "@context": "https://www.w3.org/ns/activitystreams",
        "actor": "https://orb.domain1.com/services/orb",
        "id": "https://orb.domain1.com/services/orb/activities/72c49978-0812-4c07-83ac-a39c9faf1a0a",
        "object": "https://orb.domain2.com/services/orb",
        "to": "https://orb.domain2.com/services/orb",
        "type": "Follow"
      },
      "to": "https://orb.domain1.com/services/orb",
      "type": "Accept"
    },
    {
      "@context": [
        "https://www.w3.org/ns/activitystreams",
        "https://w3id.org/activityanchors/v1"
      ],
      "actor": "https://orb.domain2.com/services/orb",
      "id": "https://orb.domain2.com/services/orb/activities/30e184f6-f45f-43e2-9cf6-9e2b7c1ed230",
      "object": "https://w3id.org/activityanchors#AnchorWitness",
      "target": "https://orb.domain1.com/services/orb",
      "to": "https://orb.domain1.com/services/orb",
      "type": "Invite"
    },
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain2.com/services/orb",
      "id": "https://orb.domain2.com/services/orb/activities/635fe297-47d5-4374-8278-8befcdea2055",
      "object": {
        "@context": [
          "https://www.w3.org/ns/activitystreams",
          "https://w3id.org/activityanchors/v1"
        ],
        "actor": "https://orb.domain1.com/services/orb",
        "id": "https://orb.domain1.com/services/orb/activities/d5c75aa5-9d19-43ae-92e1-21ae7c6cf67e",
        "object": "https://w3id.org/activityanchors#AnchorWitness",
        "target": "https://orb.domain2.com/services/orb",
        "to": "https://orb.domain2.com/services/orb",
        "type": "Invite"
      },
      "to": "https://orb.domain1.com/services/orb",
      "type": "Accept"
    },
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain2.com/services/orb",
      "endTime": "2022-03-21T20:03:51.0383296Z",
      "id": "https://orb.domain2.com/services/orb/activities/cb55746b-3fdf-441b-8cce-ae6cf47bf676",
      "object": {
        "linkset": [
          {
            "anchor": "hl:uEiB9usl2nCTWvg2cqPKKkFctA-Msj0qtAYv4WzpAsxaErw",
            "author": "https://orb.domain2.com/services/orb",
            "original": [
              {
                "href": "data:application/gzip;base64,H4sIAAAAAAAA/8yYSa+i6haG/4t3WlYhPXtGK6CA9OhJhQAfPUgrCCf132/2SQ3uwBqQbHPP2Oj7+OZZKwv+3lX5vRzicffx19+74B5lTb/72GXVx4PPOTsfJesEiSsl5w87bdk2UNlaYoQMH3jCPzam0V7R3ge2vvu2Cx7j72+PYzt8/PjR9OF30NRBfj98j5r6xxD3Ux7Fw+cHu2+7fIzrf2KzPk52HzuQg4+mDz+TWQhYtRWKhD/esqWAjlR3GiyPJA5P5ni0MhSOmqsft+4zpT/4nI5waXSUp6no3cIRYEIRozbMSTrjE2fGnOtdb51EOhwB5t23XdvHU948ht3HX7//6Za83c9f314xc4JetHY4D7c0QXiDssrEeFQl3/oAhXGMOWEW47i+xRPZJzNzTc+sXeQNHpVSRUfkHCGnZ2LxVNPqe+x4reT1JsjziTy9Zt6S9ydmthBsABtY6oiKOPN2Ft7Fg4dVFdySa4rM8FE7iVzjsG2qf/A5K7eMxHXSfc4M+OTFsiowqdr7s16ZwjPRIvtizaF1XPXoDz1vyPsTMy2E3dkkuiXnnMs82Gl5KLHUW/OqgqMGxJMTqFLeUJQgfPbMweO14OqSBsx8HqEOcwRX9eJn3QexjiOpXTa3ha9KvyTol8xb8r6K2TzAFaRBOH+/kwoko/2dx4OHOrktZUiigLTEINH2NCNE+jZmRoAXX5FJGgtKHxcP9B3rSxfOzk97bBWWXcVDP4sn+bDH/3GjUuzc0ooh4+S5B23CLaXkgIsYyqiN0VYpwbOZDt79cX/NvCXvi3xmUpBDwyiRa1ZyFHIxTVvcQ9ysI4TLsNOzM3CLeSzKRXJeM3+Fzxt75gx9TlwjTEz0dqzZGtO4jjxz4hQ4K6MzCIAktVfDvZYkr33+ip437jp6GAZeVwvHzStQdJeyDO5RpYdY0eCR5p5Jgu3dqrnJbPK+XbeRmWMWbjQOnD6gnOoPrJ74TnbpITCdtL0yCofg4hZa5Cnpov9r9rOxMD5+54ACTU4lG70WjlzoTwyAkotaT2ekoc1DIjxvz/ft540+M3k0LRVMT046wUZCOPmoyQdqf59j7tbpaERbOsWUSgUf3ufzxp65hCupZ+iu8NywPDoB1TxSR8bo8gt1v3rduDZieYNm+Di+b29s3XUaE5uIlUhepWYUVVz2kB6YCMwIqagR0A08ijDbN1zHHF77/P9wg3mOXMfRFtPsXZopO/mc0W3+RCHG8zwH8erI7t0YP6zqa5+/xI1ttyizdx/ZgO/FKMBs5ACwRk4Rsb+d6sZ46OyxD32krrKCj+vXPn/FLbr13kCRtjrWgU8NLIotOBdZfjakhN5iqFogrEKnijP1SNsHr934intjY88c8GKJmp0ahRt4KFTgGex0hDMDDXrRTJVYqlxHNnq6Y/4wg1/Q89Ybabk+chB1UCmTVwjNLPMUhWC8QxCuttpwspzeoI8JySzL+24kGmTNHgfYJWGb/c0AjBKW1iV6WqM6zk8SFFaScGeP5Cf2k5lWNNg+WrwLrUMh3BgtO+355JzYtrr0uqj5ueufct8vcfr1DG7J+yI3aB/Cu4bF9hVbsDcC319upNE7khwjlRawafdIyYkuL77Svu95cOsMCkbdZIuMEF6GVhhBo3gDAJusw5AsM37zxWh/9VaAHNj3zeBWn8GICk8s00k/YWgD4FoCoMZkSx+l9PXRF4YdPXhSplXzfT5vZVbplfeCJunXESVYJrhGk6Fe4QS1ADzTT02baeTorY2Bvu55I/PPz99okryK/+fNyIzk4HvTpz+aPvzPBO1+/fz13wAAAP//1RF5K4oRAAA=",
                "type": "application/linkset+json"
              }
            ],
            "profile": "https://w3id.org/orb#v0",
            "related": [
              {
                "href": "data:application/gzip;base64,H4sIAAAAAAAA/5yUy26jSBiF38WzTSZQ2E7IzhgKjE053ApTo5YFlM2luBkwt1befZQeaRbd6ZEmu9qU/qPvfDrfF3lasvbSLV7/+r4IyiipmsXrIslf70oqifc2B+XW8foYRLe3/Z7BqNs8Gm3G3bqN3y+9ud60Y6A0w+JhUTfVNc0vH9+7rm5fn54GIaV/Vk38VDXhHz23eFjc6x+HkuZy/ffMlqNO4YTa87kjyZRxqnjbt87p5ZkfJVV1kiWIKv98qb0x3rzeK/Nxe1G6imrWEM1VfwDi5KcrFgI+CbzleChQH9pi5p+Mnrq4NmeJYZV3fRYLJ8EKAi2/GUq0itxuxjBeohkuTUY1rMacUaDew2IeQbokHFyaRT6Q0lgfJjH1PdJEBa5JiXvqWGsyI4ayCBhlPSIND4H38daby8lqEUiGUNOTqITrQN1xYWnN0YkJF40UYamPvrl4f/iJggzNrHbDoSXxVVAs0WFX654zpT7TJVivpP3KkbB3dpTn5B8K0n9QmH6mYCnECkoJ+yWdvYIWBoftI7Yakut8qHYwgiJD8g4gVz8gbG1xTq9YcYVAUX6Nus2gS4G1irFmaIPiJmGp8adVnoP6ZY6FAajHvSZXeFvH5v+PaoJaxZ45uRkuMUuQrVGIvSQNWTx6Mm5DWR+OWl0aQjRZnHiwFavHDA2E/yTqBoa3g/18m1IZvw2tGzOereLTnOY5iCp66XGAdmklihB+xS2XpJE7OkiAq8C1PFejM/ZoY3gtT/hkHXhj++EUUTHABTzaXt4fnVy12Odu+R65G6V+JxkC4QnXRCAVUskUFHAOC56jKmyRbDWh6s+BSlIjI0tajLVR5FMAkqVRkCz6xC0Jguls6C+bVcDOa43flKuGeSA5jG5XG9vtrPHNoO11/nH9lcKYP12wiOwskZAHm9NMdoYL7+ikN3jWK0dLMNUkZHJobctwolxysDOlQ/lnhdGkelzT1dt1Wz0Si0pGyJy3aHQ61A3jC82c61U+nF6Ufmt+oTDPqsKJA0TGJmGoP+DaJoqO/KJzXRUtsWYdqUaXR9W6YZUUljL6RwUDk/9NYQ4ejSKeqeNyVDY4UjCBgK4wCr0kszURQepDr+uNYiUglVYXB0/UMXhUWAXN6BiUUkrMxfu3h0WfBr9Oo+ym3c7Zc9os6undjettHaBtsZNgsm6V57Na2VbtL5szdb9Aw4K4pkru2rO0u2T5PjhhTARJjlR4NAGvm4xUqEQQ8aIW8siOIOOiXGQY/2YaBTyFap0S0E5+RgajqKdQQ7Nf1GkAPjRFnA9gHqo7PlQhd5EpCwC6EbXuQkAGlOHmB433b+9/BwAA///+JdsNmQYAAA==",
                "type": "application/linkset+json"
              }
            ],
            "replies": [
              {
                "href": "data:application/gzip;base64,H4sIAAAAAAAA/4yS226jOhSG38X7NgRjDgm+2qQlUk+TtGmLmtGoMrYJpgSobTBp1XcfkSbpaKRKc+1vff/vZb+D/2ldad5rgH+CXOtGYds2xoyNO67lxkbQmdpUcsYrLUip7M4Boy/QFWyPKU5bKfTOVq3QXNmFURaCCP4jzhnyfSc8jfwaga/MVZsWnGqAQV7iNhazsFUlqs7uk26D6Ovy6uplTnVk3agCvuroqfOStyZSPYmlASMg2DB5aFDLdMzqLRGVM6b11u6ojWDqEddjlu8GyPICl1lhQDIrmDqUTDzku5AMGqVaUlF+TjQHGCCIkAVdCzn3Toh9F/twPPFCz4fB+kBz+X0wGIFG1nUG8PtwU6I5+04a7oWfswfhwddRjadwMrG3pCn5sLujdtnKplZDT6IUl1rU1Q3Xec2OwCMp2+E4y9Vzay4vHxATM3VXrf1mkpNE5XPaBjHvEmPCxa5wnnZ3L+r8wnXmaLGOz5YmeL4oFpf9Leun54trR0TXVcNk6kdvD7MIjIDeNYM//nzYldhURLfyWLLjUmSCkj+KYcAEw4an+K9V/ZfNX6NZv8h+XMQhdJNmtQnyolqbPPHuku1OOyup57def7/0wMcp+XEfQdKSn51+Evj4HQAA///2m9v98AIAAA==",
                "type": "application/ld+json"
              }
            ]
          }
        ]
      },
      "startTime": "2022-03-21T19:53:51.0383296Z",
      "target": "https://w3id.org/activityanchors#AnchorWitness",
      "to": [
        "https://orb.domain1.com/services/orb",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Offer"
    },
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "did:web:orb.domain2.com:services:orb",
      "id": "https://orb.domain2.com/services/orb/activities/3dd6c5bf-dc7c-4743-87b2-12d303f0ecaf",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "object": {
          "linkset": [
            {
              "anchor": "hl:uEiAsmhY5seUU6LNsy30iJEjM-urZ4g_EPylBeXUTlzK33A",
              "author": "did:web:orb.domain2.com:services:orb",
              "original": [
                {
                  "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiDRfrx2UAy6wydaXceJL9hlpFQwTTuP_RIS5zm7J9sr2Q%22%2C%22author%22%3A%22did%3Aweb%3Aorb.domain2.com%3Aservices%3Aorb%22%2C%22item%22%3A%5B%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiDvfNx40DmnAE9tKw6eWue8OiWPh5668PvnyfcHQtr5sQ%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiCgK6K4eVoW2vYINhGDYyUsaFQh0Xq2GeQQ6No7sXrezg%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiAI60PiHNKAX7zGRe10UOPIfl5vabGYAkzOSF8KLgzmmg%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiCgNvbqlq5yxQGrM6ipv6iMDLhwYmlDBZO1f60mfTnLaA%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiBCHij5TOiT_D1COIdMGbBbIAJJ7PSt_aCWqv7BXFGpxQ%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiBkfnunulwm2oj9_PZXtrBxWFclC6Z64Ou2rX5sGM-D_A%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiBxxIu6yc1aFlCvV5rbjSbJ-qFWLLwxKPBstDyLMrhlNA%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiCUipZOgwqREWpmRtln44hWDQhENOOPzX5ByukeYV5eLQ%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiDk4OAYEyC3iQEqJcNxu6ljG7o5PayR-vugrtB27e_0KA%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiDY8i8w9v7yFb6VmcKvoxNZ7X_VjWfGuSN-QLH6crHnWg%22%7D%5D%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%7D%5D%7D",
                  "type": "application/linkset+json"
                }
              ],
              "profile": "https://w3id.org/orb#v0",
              "related": [
                {
                  "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiAsmhY5seUU6LNsy30iJEjM-urZ4g_EPylBeXUTlzK33A%22%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%2C%22via%22%3A%5B%7B%22href%22%3A%22hl%3AuEiDRfrx2UAy6wydaXceJL9hlpFQwTTuP_RIS5zm7J9sr2Q%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpRFJmcngyVUF5Nnd5ZGFYY2VKTDlobHBGUXdUVHVQX1JJUzV6bTdKOXNyMlE%22%7D%5D%7D%5D%7D",
                  "type": "application/linkset+json"
                }
              ],
              "replies": [
                {
                  "href": "data:application/json,%7B%22%40context%22%3A%5B%22https%3A%2F%2Fwww.w3.org%2F2018%2Fcredentials%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Factivityanchors%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fjws-2020%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fed25519-2020%2Fv1%22%5D%2C%22credentialSubject%22%3A%7B%22anchor%22%3A%22hl%3AuEiDRfrx2UAy6wydaXceJL9hlpFQwTTuP_RIS5zm7J9sr2Q%22%2C%22id%22%3A%22hl%3AuEiAsmhY5seUU6LNsy30iJEjM-urZ4g_EPylBeXUTlzK33A%22%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%7D%2C%22id%22%3A%22https%3A%2F%2Forb.domain2.com%2Fvc%2F63384454-28f5-4dd9-9833-b7f79d9087a9%22%2C%22issuanceDate%22%3A%222022-08-09T14%3A16%3A17.653656394Z%22%2C%22issuer%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proof%22%3A%5B%7B%22created%22%3A%222022-08-09T14%3A16%3A17.656966342Z%22%2C%22domain%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22z4H9r9tgoSDVczeC5WSig5Mp4ASojXuzQJ9JTqm78GXKKpKUx2ETQSf1TUXLYU84Uq36Y6SaPzon4AstwqxyYRFRv%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain2.com%23m0lkGe6apkR_uzrcBHsttLf-cMgCS-Z4_27Qt5kyRG0%22%7D%2C%7B%22created%22%3A%222022-08-09T14%3A16%3A17.824Z%22%2C%22domain%22%3A%22http%3A%2F%2Forb.vct%3A8077%2Fmaple2020%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22z5fBV9Dwac2c3rgg5adjpvWpWM57QuK5zh5FQXsn2Jr3bZyTd3G5ACwN23hiBUHFunJC73F28wg9AdYmXfzdHVjcJ%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain1.com%23ipyFbnxMIbWJME8K1uvIp-UdrdVpyuQ9QjCCgnHHjFg%22%7D%5D%2C%22type%22%3A%5B%22VerifiableCredential%22%2C%22AnchorCredential%22%5D%7D",
                  "type": "application/ld+json"
                }
              ]
            }
          ]
        },
        "type": "AnchorEvent",
        "url": "hl:uEiCP980v2AZbTgRmebFolD48LhE4u-yXzOoISRUJkUjToQ:uoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ1A5ODB2MkFaYlRnUm1lYkZvbEQ0OExoRTR1LXlYek9vSVNSVUprVWpUb1E"
      },
      "published": "2022-08-09T14:16:18.013015949Z",
      "to": [
        "https://orb.domain2.com/services/orb/followers",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Create"
    }
  ]
}
```

### POST

A POST request to the inbox endpoint adds the activity contained in the request to the service's Inbox,
which will be processed by the ActivityPub [Inbox](../system/activitypub.html#outbox-inbox). This endpoint is restricted by
authorization rules, i.e. the requester must sign the HTTP request. Some activities also have authorization rules
such that the actor must be in the destination server's [followers](#followers) and/or [witnessing](#witnessing)
collection.

**Example**

Post an _Invite_ activity:

```
POST /services/orb/inbox HTTP/1.1
Host: orb.domain2.com
Content-Type: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate

{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://w3id.org/activityanchors/v1"
  ],
  "actor": "https://orb.domain1.com/services/orb",
  "id": "https://orb.domain1.com/services/orb/activities/34e67f25-4832-4c62-a199-5614ec3e5582",
  "object": "https://w3id.org/activityanchors#AnchorWitness",
  "target": "https://orb.domain2.com/services/orb",
  "to": "https://orb.domain2.com/services/orb",
  "type": "Invite"
}
```

## Witnesses

**Endpoint:** /services/orb/witnesses

### GET

The witnesses of this service are returned via this endpoint. If no paging parameters are specified
in the URL then the response contains information about the _witnesses_ collection, i.e. the links to the first and last page, as
well as the total number of items in the collection. A subsequent request may be made using parameters
that include a specified page number in order to retrieve the actual items.

**Example**

Request page information:

```
GET /services/orb/witnesses HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains page information:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/witnesses",
  "type": "Collection",
  "totalItems": 19,
  "first": "https://orb.domain1.com/services/orb/witnesses?page=true",
  "last": "https://orb.domain1.com/services/orb/witnesses?page=true&page-num=4"
}
```

Request page 4:

```
GET /services/orb/witnesses??page=true&page-num=4 HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains items from page 4:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/witnesses?page=true&page-num=4",
  "type": "CollectionPage",
  "totalItems": 19,
  "items": [
    "https://orb.domain2.com/services/orb",
    "https://orb.domain3.com/services/orb",
    "https://orb.domain4.com/services/orb"
  ]
}
```

## Witnessing

**Endpoint:** /services/orb/witnessing

### GET

The services that are witnessing anchor events for this service are returned via this endpoint. If no paging parameters are specified
in the URL then the response contains information about the collection, i.e. the links to the first and last page, as
well as the total number of items in the collection. A subsequent request may be made using parameters
that include a specified page number in order to retrieve the actual items.

**Example**

Request page information:

```
GET /services/orb/witnessing HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains page information:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/witnessing",
  "type": "Collection",
  "totalItems": 19,
  "first": "https://orb.domain1.com/services/orb/witnessing?page=true",
  "last": "https://orb.domain1.com/services/orb/witnessing?page=true&page-num=4"
}
```

Request first page:

```
GET /services/orb/witnessing?page=true HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains items from the first page:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/witnessing?page=true&page-num=0",
  "type": "CollectionPage",
  "totalItems": 10,
  "next": "https://orb.domain1.com/services/orb/witnessing?page=true&page-num=1",
  "items": [
    "https://orb.domain2.com/services/orb",
    "https://orb.domain3.com/services/orb",
    "https://orb.domain4.com/services/orb"
  ]
}
```

## Liked

**Endpoint:** /services/orb/liked

### GET

The anchor events that are _liked_ are returned via this endpoint. (Liked means that the
anchors in the response were all added to the ledger.) If no paging parameters are specified in the URL then the response
contains information about the collection, i.e. the links to the first and last page, as well as the total number of
items in the collection. A subsequent request may be made using parameters that include a specified page number in order
to retrieve the actual items.

**Example**

Request page information:

```
GET /services/orb/liked HTTP/1.1
Host: orb.domain3.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains page information:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain3.com/services/orb/liked",
  "type": "OrderedCollection",
  "totalItems": 21,
  "first": "https://orb.domain3.com/services/orb/liked?page=true",
  "last": "https://orb.domain3.com/services/orb/liked?page=true&page-num=0"
}
```

Request first page:

```
GET /services/orb/liked?page=true HTTP/1.1
Host: orb.domain3.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains items from the first page:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain3.com/services/orb/liked?page=true&page-num=0",
  "type": "OrderedCollectionPage",
  "totalItems": 21,
  "next": "https://orb.domain3.com/services/orb/liked?page=true&page-num=1",
  "orderedItems": [
    "hl:uEiDt0x4uc87k2EHTYavT-SjSkZ6ZjIz1S4iylbrOu5ZDcQ:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpRHQweDR1Yzg3azJFSFRZYXZULVNqU2taNlpqSXoxUzRpeWxick91NVpEY1F4QmlwZnM6Ly9iYWZrcmVpaG4ybXBjNDQ2bzR0bWVkdTNidnBqN3NrZ3NzZ3BqdGRlbTZ2ZnlybXV2eGxobHhmc2RvZQ",
    "hl:uEiCLO_8LAn_dXKtkCWybunh5Fwhnhf8-zXFcqSvBgyKFnw:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ0xPXzhMQW5fZFhLdGtDV3lidW5oNUZ3aG5oZjgtelhGY3FTdkJneUtGbnd4QmlwZnM6Ly9iYWZrcmVpZWxocDdxd2F0NzN2b2t3emFqbnNuM3U2ZHpjNGVncGJwN2gzZ3hjeGZqZnBheWdpdWZ0NA",
    "hl:uEiCcU17xqBJOppmw4WQtoPpF1G-asBkNsHAU4HM5bCnd2w:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ2NVMTd4cUJKT3BwbXc0V1F0b1BwRjFHLWFzQmtOc0hBVTRITTViQ25kMnd4QmlwZnM6Ly9iYWZrcmVpZTRrbnBwZGthc2oydGp0bWhibXF3MmI2c2Yycnh6dm1hemJ3eWhhZmhhb200d3lrbzUzbQ",
    "hl:uEiDAdXUexB5Js-KpRfXV0C9uZq_vpk3nkykYlGB9f8-aYg:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpREFkWFVleEI1SnMtS3BSZlhWMEM5dVpxX3ZwazNua3lrWWxHQjlmOC1hWWd4QmlwZnM6Ly9iYWZrcmVpZ2FvdjJyNXJhNmpnejZma2tmNnhrNWFsM29tMng2N2pzbjQ2anNzZ2V1bWI2eDd0NDJtaQ"
  ]
}
```

## Likes

**Endpoint:** /services/orb/likes/[id]

### GET

This endpoint returns a collection of _Like_ activities for a given anchor. If no paging parameters are specified
in the URL then the response contains information about the collection, i.e. the links to the first and last page, as
well as the total number of items in the collection. A subsequent request may be made using parameters
that include a specified page number in order to retrieve the actual items.

**Example**

Request page information:

```
GET /services/orb/likes/hl%3AuEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains page information:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/likes/hl%3AuEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c",
  "type": "OrderedCollection",
  "totalItems": 1,
  "first": "https://orb.domain1.com/services/orb/likes/hl%3AuEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c?page=true",
  "last": "https://orb.domain1.com/services/orb/likes/hl%3AuEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c?page=true&page-num=0"
}
```

Request first page:

```
GET /services/orb/likes/hl%3AuEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c?page=true&page-num=0 HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains items from the first page:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/likes/hl%3AuEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c?page=true&page-num=0",
  "type": "OrderedCollectionPage",
  "totalItems": 1,
  "orderedItems": [
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain3.com/services/orb",
      "id": "https://orb.domain3.com/services/orb/activities/15d19d44-003d-4bf9-8952-bd69c107435f",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw:uoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c"
      },
      "published": "2022-03-22T13:26:23.1871399Z",
      "result": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw:uoQ-BeEtodHRwczovL29yYi5kb21haW4zLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c"
      },
      "to": [
        "https://orb.domain1.com/services/orb",
        "https://orb.domain2.com/services/orb",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Like"
    }
  ]
}
```

## Shares

**Endpoint:** /services/orb/shares/[id]

### GET

The _Create_ activities that were _Announced_ are returned via this endpoint. If no paging parameters are specified in the URL
then the response contains information about the collection, i.e. the links to the first and last page, as well as the total number of
items in the collection. A subsequent request may be made using parameters that include a specified page number in order
to retrieve the actual items.

**Example**

Request page information:

```
GET /services/orb/shares/hl%3AuEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c HTTP/1.1
Host: orb.domain3.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains page information:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain3.com/services/orb/shares/hl%3AuEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c",
  "type": "OrderedCollection",
  "totalItems": 1,
  "first": "https://orb.domain3.com/services/orb/shares/hl%3AuEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c?page=true",
  "last": "https://orb.domain3.com/services/orb/shares/hl%3AuEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c?page=true&page-num=0"
}
```

Request first page:

```
GET /services/orb/shares/hl%3AuEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c?page=true HTTP/1.1
Host: orb.domain3.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains items from the first page:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain3.com/services/orb/shares/hl%3AuEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c?page=true&page-num=0",
  "type": "OrderedCollectionPage",
  "totalItems": 1,
  "orderedItems": [
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain1.com/services/orb",
      "id": "https://orb.domain1.com/services/orb/activities/2f1706dc-723f-4d73-82c9-6cd23cd21541",
      "object": {
        "items": [
          {
            "@context": "https://w3id.org/activityanchors/v1",
            "object": {
              "linkset": [
                {
                  "anchor": "hl:uEiAKItnt8OwHFrvlE5pmMEUe9PJgJCY9Hy085IKjdOzAjQ",
                  "author": "https://orb.domain2.com/services/orb",
                  "original": [
                    {
                      "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiA8itLUWN1bmNXCUuksDuYhgMu4COs-SxhZIVc-FszrmA%22%2C%22author%22%3A%22https%3A%2F%2Forb.domain2.com%2Fservices%2Forb%22%2C%22item%22%3A%5B%7B%22href%22%3A%22did%3Aorb%3AuAAA%3AEiCAXMhVMMdJCUCzOGHYfrstd0jbEGvMD149hTJhXVJXJg%22%7D%5D%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%7D%5D%7D",
                      "type": "application/linkset+json"
                    }
                  ],
                  "profile": "https://w3id.org/orb#v0",
                  "related": [
                    {
                      "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiAKItnt8OwHFrvlE5pmMEUe9PJgJCY9Hy085IKjdOzAjQ%22%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%2C%22via%22%3A%5B%7B%22href%22%3A%22hl%3AuEiA8itLUWN1bmNXCUuksDuYhgMu4COs-SxhZIVc-FszrmA%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQThpdExVV04xYm1OWENVdWtzRHVZaGdNdTRDT3MtU3hoWklWYy1Gc3pybUE%22%7D%5D%7D%5D%7D",
                      "type": "application/linkset+json"
                    }
                  ],
                  "replies": [
                    {
                      "href": "data:application/json,%7B%22%40context%22%3A%5B%22https%3A%2F%2Fwww.w3.org%2F2018%2Fcredentials%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fjws-2020%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fed25519-2020%2Fv1%22%5D%2C%22credentialSubject%22%3A%22hl%3AuEiAKItnt8OwHFrvlE5pmMEUe9PJgJCY9Hy085IKjdOzAjQ%22%2C%22id%22%3A%22https%3A%2F%2Forb.domain2.com%2Fvc%2F0b4801ad-659f-4d61-8425-aeeff666c41d%22%2C%22issuanceDate%22%3A%222022-03-22T13%3A20%3A43.868323Z%22%2C%22issuer%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proof%22%3A%5B%7B%22created%22%3A%222022-03-22T13%3A20%3A43.8688649Z%22%2C%22domain%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22xQPOX5sCv6WAHBXNiJw6VKZbEshxRI53ZpN6kNQplVeckoju0sBFfrU3Kih4xxKfSm7TlC950phc2VuPuG-oDA%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain2.com%23YHyyBBDM0daarzg95h-91zCEh-Ud_Yjaa1YpQQZFscU%22%7D%2C%7B%22created%22%3A%222022-03-22T13%3A20%3A43.991Z%22%2C%22domain%22%3A%22http%3A%2F%2Forb.vct%3A8077%2Fmaple2020%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22VT8Nm5yxz2Ihx_dlFas59PpyXFKFFqeX8jDa-lVDRKlmCcQTgCjRBbma4gIiVq0VHwPHi_VK6yd8yR0K56AHAw%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain1.com%2343p30zaA40c5_pRaz774EAsA1y2MY0PwFB1lBfiCfhA%22%7D%5D%2C%22type%22%3A%22VerifiableCredential%22%7D",
                      "type": "application/ld+json"
                    }
                  ]
                }
              ]
            },
            "type": "AnchorEvent",
            "url": "hl:uEiCwSSUKoJxrWj_A80KxPE6rM5XA0fhHRVoPX09uVddgSw:uoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ3dTU1VLb0p4cldqX0E4MEt4UEU2ck01WEEwZmhIUlZvUFgwOXVWZGRnU3c"
          }
        ],
        "totalItems": 1,
        "type": "Collection"
      },
      "published": "2022-03-22T13:20:44.3037905Z",
      "to": [
        "https://orb.domain1.com/services/orb/followers",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Announce"
    }
  ]
}
```

## Activities

**Endpoint:** /services/orb/activities/[id]

### GET

This endpoint returns an activity for the specified ID.

**Example**

Request:

```
GET /services/orb/activities/91dbb2e2-1040-4fd5-bd2e-1bcd67bdda8a HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains the activity:

````json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/activities/91dbb2e2-1040-4fd5-bd2e-1bcd67bdda8a",
  "type": "Accept",
  "actor": "https://orb.domain1.com/services/orb",
  "to": "https://orb.domain5.com/services/orb",
  "object": {
    "@context": "https://www.w3.org/ns/activitystreams",
    "actor": "https://orb.domain5.com/services/orb",
    "id": "https://orb.domain5.com/services/orb/activities/b9f63dd7-b3da-414f-8c59-894d964787c4",
    "object": "https://orb.domain2.com/services/orb",
    "to": "https://orb.domain1.com/services/orb",
    "type": "Follow"
  }
}
````
