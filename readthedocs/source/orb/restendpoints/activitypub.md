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
      "id": "https://orb.domain1.com/services/orb/activities/c3f51db8-fa65-487b-85f5-201f110201b6",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "object": {
          "linkset": [
            {
              "anchor": "hl:uEiCjjpFjLgWSPtDBJjZLk0oJyrLhcXw3K5SRcG9i5DTqbw",
              "author": [
                {
                  "href": "https://orb.domain1.com/services/orb"
                }
              ],
              "original": [
                {
                  "href": "data:application/gzip;base64,H4sIAAAAAAAA/2yOT0+DMBiHv8vrdY5NF429Vd38s3SBgRpdOBQo8I7S1lKYhPDdDTt58PzL8/yeASSqqhEOyGEArtJSWyBQStKukb6pkLH7sL3reRjIpKsjufu4CfdBxY8qN1fyk9ltf7k0/QlmwFt3pg8DlFbkk8Y50xDP0zaZZ7rmqJbzVNdeI2yHqWimAcZ4BuhE/RfMMCPaJqSllJI10m/DgqqLGH3m/pevdquX/bu/eqKLLRevUXHc/Dw+FJvbREt69hmrc5Tiv5bTNWZzbYvp+6JbwBiP8fgbAAD//9+kd3kHAQAA",
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
                  "href": "data:application/gzip;base64,H4sIAAAAAAAA/1TNTXOiMACA4f+SvbYrUHUGbrUtOhToIhAgOx4IoATyZeTTDv99Z731/s7zfgNKeHurOmD9/QY5L2qhgAVqavUf5K1ppN24lyT8073vnAa5rSacWbl1kY4vn5vwWOxNsnmPrngET0AqcSa0eki1qs7/na6TN2u1Gl9I+Vuoy0oo/GvQwHJ6AgPJf6SP5WvMQ8/bhb0552FA8cAi6ifb8Bi0ecPP0qCZpz7nZ13Oo9WL4Pmt+uhEeTiOxV0MrmHOGdm02NDrPFlPLvMHHJpNlnpDGUMZQNjHmu4H1Ne/UlrHun3LOOrgfvqCdxTF1FZZInvEX2dsU79ou40b2WOVluuA0RFxb+vOJskSpAoGZZZCrUydqUqcKz7UaxQ5V5RAozrsNBQddY8XOkp9ljHY51Gg+4a452yqKzYxnF7ueC+3OADLaTkt/wIAAP//F6rBvooBAAA=",
                  "type": "application/linkset+json"
                }
              ],
              "replies": [
                {
                  "href": "data:application/gzip;base64,H4sIAAAAAAAA/6xUW3OqSBj8L5NXFRgYQZ5W4m29ZI0YL5xKpYZh1FFgcBjgQCr/fYtoEmu3zm4ezit099dfNx+v4A/CY0l/SmD/AAcpk9RWlKIoWoXe4mKvQFWzFCJoQGPJcJgquQYaX0CdBe8wTCTLmSxxTA5c/AqVUpIJJkslzZikqXIs0iZUofpNOA0gQlrnk/LcAF/O3Mw/UiKB/QouJoANDqGd9Vn3KXZnM8fNOiV2H0M/j5bhw7rtLh5P+BjvEhhuZ2JSNrWkLGobgu4+uffHYzI4Tvdrdy57zvjoTU8qH5dieiCbQp8gd0GGHYZ6y7NfcxPBdyykNf2fy3Dh3+UqaABBQ2CDkMWnlErQALJMaB1+9931lMUn8PzWACy4UeHCh62AR5jFWovwSMmJQk11pxLTaiJVt5oG0nZNDH3UxFRHOlFVarQhaACWphmOCe1hWfuCKoRN1WpCtISarUNb11qm0TE1pOuGd8VT8R+zL2vyHbB/vNb5Y0mDXwmjTi15IV8lL4qtnEjbUk1TiXAS0rrRD915JhKe1l5xmlIhGY9nVB548AFY4TCrX1cIOVw8cNMa/1zDxdAbROsSppbra5o/qzgzkXxw2lNreI654XlIJvt+4jytnGyx3kTbwXFGplM0ScfQnUAu0Xq41LbeZP1ZCuhfvjiX7WMsM/HhM6eC7RjBN95sELDALqhv19vdxHXXFsKs5MYZ9kf4z7/kepCOo3JIpBef7zVDNS11kQ307nhVWeCt8X+RWpquIdQ20b+CvXZ1nQ1vq/p+pFAGOUvKxCn19ix116tTnlQjHldPT+UEz0fBvurDylhtxodJe3naeosFYcWod+4bu6QX6ZU3Hm2yop8/8p4h995w03nQArn9XZG+r3WXbNDLkYsRiduR4Rxepk29P5cbs9stHX18nryERjwnSz1PCvD2/HViq/ch2A/p/ed/AzSul3fz6Pnt7wAAAP//2OYItxgFAAA=",
                  "type": "application/ld+json"
                }
              ]
            }
          ]
        },
        "type": "AnchorEvent",
        "url": "hl:uEiCUKchKLxlnv5z9ovQ1_brtSIW4WUxrfalPfszpVPjUIg:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ1VLY2hLTHhsbnY1ejlvdlExX2JydFNJVzRXVXhyZmFsUGZzenBWUGpVSWd4QmlwZnM6Ly9iYWZrcmVpZXVmaGVldWx5em02N3p6N25jNnEyNzNveG5qY2MzcXdrbW5uNjJzdDM2enR1dmo2Z3VlaQ"
      },
      "published": "2022-08-25T21:32:31.941935474Z",
      "to": [
        "https://orb.domain1.com/services/orb/followers",
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
      "actor": "did:web:orb.domain2.com:services:orb",
      "id": "https://orb.domain2.com/services/orb/activities/ba65fbd8-ef59-4f69-a3d0-0ba6a115c650",
      "object": "https://orb.domain1.com/services/orb",
      "to": "https://orb.domain1.com/services/orb",
      "type": "Follow"
    },
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "did:web:orb.domain2.com:services:orb",
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
      "actor": "did:web:orb.domain2.com:services:orb",
      "id": "https://orb.domain2.com/services/orb/activities/30e184f6-f45f-43e2-9cf6-9e2b7c1ed230",
      "object": "https://w3id.org/activityanchors#AnchorWitness",
      "target": "https://orb.domain1.com/services/orb",
      "to": "https://orb.domain1.com/services/orb",
      "type": "Invite"
    },
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "did:web:orb.domain2.com:services:orb",
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
      "actor": "did:web:orb.domain2.com:services:orb",
      "endTime": "2022-08-26T15:36:20.174639514Z",
      "id": "https://orb.domain2.com/services/orb/activities/21646981-422c-46db-9bcb-66184aa2dd39",
      "object": {
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
                "href": "data:application/json,%7B%22%40context%22%3A%5B%22https%3A%2F%2Fwww.w3.org%2F2018%2Fcredentials%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Factivityanchors%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fjws-2020%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fed25519-2020%2Fv1%22%5D%2C%22credentialSubject%22%3A%7B%22anchor%22%3A%22hl%3AuEiCdxlzbCM_pQopycBF8Q_KA58ioizEaWNJqlKxGrdlSjg%22%2C%22href%22%3A%22hl%3AuEiBu8_7A4JQ1l9cRkxrInZ-2cxgeuoerMOueXmRI-mmfog%22%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%2C%22rel%22%3A%22linkset%22%2C%22type%22%3A%5B%22AnchorLink%22%5D%7D%2C%22id%22%3A%22https%3A%2F%2Forb.domain2.com%2Fvc%2F65a306e4-1f86-480c-9fbd-c5e627363f0d%22%2C%22issuanceDate%22%3A%222022-08-26T15%3A26%3A20.114938493Z%22%2C%22issuer%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proof%22%3A%7B%22created%22%3A%222022-08-26T15%3A26%3A20.116657916Z%22%2C%22domain%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22z63YhBwGe5h5y3ChrjTCeSXxFNf98krSDCP3FGQDRSFFCFbC7BryMpR5gaboGiaqtDRY4tNqUSZPHHHDr7jT3jSzd%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain2.com%23W51yCsfyP-3uyHi9BhTTd9qBzPM14YaQk2A0bCybRbU%22%7D%2C%22type%22%3A%5B%22VerifiableCredential%22%2C%22AnchorCredential%22%5D%7D",
                "type": "application/ld+json"
              }
            ]
          }
        ]
      },
      "startTime": "2022-08-26T15:26:20.174639514Z",
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
      "id": "https://orb.domain2.com/services/orb/activities/d5a83f39-f9e3-4833-b668-134e21f089d3",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "object": {
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
        },
        "type": "AnchorEvent",
        "url": "hl:uEiAFSmhbZSMOCK8YjpzoYrKTTrPZo5POkMGy3S6zOKnBgg:uoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQUZTbWhiWlNNT0NLOFlqcHpvWXJLVFRyUFpvNVBPa01HeTNTNnpPS25CZ2c"
      },
      "published": "2022-08-26T15:26:20.404295851Z",
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
GET /services/orb/likes/hl%3AuEiAFSmhbZSMOCK8YjpzoYrKTTrPZo5POkMGy3S6zOKnBgg%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQUZTbWhiWlNNT0NLOFlqcHpvWXJLVFRyUFpvNVBPa01HeTNTNnpPS25CZ2c?page=true&page-num=0 HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains items from the first page:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain2.com/services/orb/likes/hl%3AuEiAFSmhbZSMOCK8YjpzoYrKTTrPZo5POkMGy3S6zOKnBgg%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQUZTbWhiWlNNT0NLOFlqcHpvWXJLVFRyUFpvNVBPa01HeTNTNnpPS25CZ2c?page=true&page-num=0",
  "orderedItems": [
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain3.com/services/orb",
      "id": "https://orb.domain3.com/services/orb/activities/4880e2fc-972d-46db-bc19-c08dc8668da7",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiAFSmhbZSMOCK8YjpzoYrKTTrPZo5POkMGy3S6zOKnBgg:uoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQUZTbWhiWlNNT0NLOFlqcHpvWXJLVFRyUFpvNVBPa01HeTNTNnpPS25CZ2c"
      },
      "published": "2022-08-26T15:26:21.171433692Z",
      "result": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiAFSmhbZSMOCK8YjpzoYrKTTrPZo5POkMGy3S6zOKnBgg:uoQ-BeEtodHRwczovL29yYi5kb21haW4zLmNvbS9jYXMvdUVpQUZTbWhiWlNNT0NLOFlqcHpvWXJLVFRyUFpvNVBPa01HeTNTNnpPS25CZ2c"
      },
      "to": [
        "https://orb.domain1.com/services/orb",
        "https://orb.domain2.com/services/orb",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Like"
    },
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain1.com/services/orb",
      "id": "https://orb.domain1.com/services/orb/activities/5a053c65-eaed-4004-968b-2ead5f94a238",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiAFSmhbZSMOCK8YjpzoYrKTTrPZo5POkMGy3S6zOKnBgg:uoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQUZTbWhiWlNNT0NLOFlqcHpvWXJLVFRyUFpvNVBPa01HeTNTNnpPS25CZ2c"
      },
      "published": "2022-08-26T15:26:21.433750066Z",
      "result": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiAFSmhbZSMOCK8YjpzoYrKTTrPZo5POkMGy3S6zOKnBgg:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQUZTbWhiWlNNT0NLOFlqcHpvWXJLVFRyUFpvNVBPa01HeTNTNnpPS25CZ2d4QmlwZnM6Ly9iYWZrcmVpYWZqanVmd3pqZGJ5ZWs2Z2VvdHR1Z2ZtdXRqMno1dGk0dHoyaW1kbXc1ZjJ6dHJrb2JxaQ"
      },
      "to": [
        "https://orb.domain2.com/services/orb",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Like"
    }
  ],
  "totalItems": 2,
  "type": "OrderedCollectionPage"
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
GET /services/orb/shares/hl%3AuEiAFSmhbZSMOCK8YjpzoYrKTTrPZo5POkMGy3S6zOKnBgg%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQUZTbWhiWlNNT0NLOFlqcHpvWXJLVFRyUFpvNVBPa01HeTNTNnpPS25CZ2c?page=true HTTP/1.1
Host: orb.domain3.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains items from the first page:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain3.com/services/orb/shares/hl%3AuEiAFSmhbZSMOCK8YjpzoYrKTTrPZo5POkMGy3S6zOKnBgg%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQUZTbWhiWlNNT0NLOFlqcHpvWXJLVFRyUFpvNVBPa01HeTNTNnpPS25CZ2c?page=true&page-num=0",
  "orderedItems": [
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain1.com/services/orb",
      "id": "https://orb.domain1.com/services/orb/activities/12922991-3170-4912-95b9-f077b61eaaec",
      "object": {
        "items": [
          {
            "@context": "https://w3id.org/activityanchors/v1",
            "object": {
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
            },
            "type": "AnchorEvent",
            "url": "hl:uEiAFSmhbZSMOCK8YjpzoYrKTTrPZo5POkMGy3S6zOKnBgg:uoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQUZTbWhiWlNNT0NLOFlqcHpvWXJLVFRyUFpvNVBPa01HeTNTNnpPS25CZ2c"
          }
        ],
        "totalItems": 1,
        "type": "Collection"
      },
      "published": "2022-08-26T15:26:20.787897406Z",
      "to": [
        "https://orb.domain1.com/services/orb/followers",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Announce"
    }
  ],
  "totalItems": 1,
  "type": "OrderedCollectionPage"
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
