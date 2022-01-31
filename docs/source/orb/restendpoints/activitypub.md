# ActivityPub Endpoints

The ActivityPub spec defines a number of REST endpoints that provide information about a [service](https://www.w3.org/TR/activitypub/#actor-objects).
The [ActivityAnchors](https://trustbloc.github.io/activityanchors/#actor-discovery) spec extends the ActivityPub spec to provide additional endpoints.

## Service

**Endpoint:** /services/orb

The Orb service is retrieved using the */services/orb* endpoint. The returned data is a JSON document
that contains REST endpoints that may be queried to return additional information.

**Example**

```
GET /services/orb HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

```json
{
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    "https://w3id.org/security/v1",
    "https://w3id.org/activityanchors/v1"
  ],
  "followers": "https://orb.domain1.com/services/orb/followers",
  "following": "https://orb.domain1.com/services/orb/following",
  "id": "https://orb.domain1.com/services/orb",
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
  "type": "Service",
  "witnesses": "https://orb.domain1.com/services/orb/witnesses",
  "witnessing": "https://orb.domain1.com/services/orb/witnessing"
}
```

## Keys

**Endpoint:** /services/orb/keys/[id]

The public key of an Orb service is retrieved using this endpoint.

**Example**

```
GET /services/orb/keys/main-key HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

```json
{
  "id": "https://orb.domain1.com/services/orb/keys/main-key",
  "owner": "https://orb.domain1.com/services/orb",
  "publicKeyPem": "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhki....."
}
```

## Followers

**Endpoint:** /services/orb/followers

The followers of this Orb service are returned via this endpoint. If no paging parameters are specified in the
URL then the response contains information about the collection, i.e. the links to the first and last page, as
well as the total number of items in the collection. A subsequent request may be made using parameters
that include a specified page number in order to retrieve the actual items.

**Example**

```
GET /services/orb/followers HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

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

```
GET /services/orb/followers?page=true&page-num=0 HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

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

The services following this Orb service are returned via this endpoint. If no paging parameters are specified
in the URL then the response contains information about the collection, i.e. the links to the first and last page, as
well as the total number of items in the collection. A subsequent request may be made using parameters
that include a specified page number in order to retrieve the actual items.

**Example**

```
GET /services/orb/following HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

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

```
GET /services/orb/following?page=true&page-num=0 HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

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
authentication bearer token or must be verified using HTTP signatures and also must be in the
_following_ or _witnesses_ collection. Although, any activity sent to a [public](https://www.w3.org/TR/activitypub/#public-addressing) URI,
is returned without authorization.

If no paging parameters are specified in the URL then the response contains information about the outbox collection, i.e. the links
to the first and last page, as well as the total number of items in the collection. A subsequent request may be made
using parameters that include a specified page number in order to retrieve the actual items.

**Example**

```
GET /services/orb/outbox HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

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

```
GET /services/orb/outbox?page=true&page-num=41 HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/outbox?page=true&page-num=41",
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
      "@context": [
        "https://www.w3.org/ns/activitystreams",
        "https://w3id.org/activityanchors/v1"
      ],
      "actor": "https://orb.domain1.com/services/orb",
      "id": "https://orb.domain1.com/services/orb/activities/637ccf91-f7ae-413a-942a-04b07046da59",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "attachment": [
          {
            "contentObject": {
              "properties": {
                "https://w3id.org/activityanchors#generator": "https://w3id.org/orb#v0",
                "https://w3id.org/activityanchors#resources": [
                  {
                    "id": "did:orb:uAAA:EiDlnLQMBvVSoMx8OKgDN1N5TcfVA8utoNUjqMFMXXEeYw"
                  },
                  {
                    "id": "did:orb:uAAA:EiCCMC3RgwyhRNNd_mJ0AwWVX4bWkMi7CpjkQ2FkrLiXsg"
                  },
                  {
                    "id": "did:orb:uAAA:EiBXtRyaJu32AMIHPFoS6arrbDDWGs9KNE8XS4L-RIaC8Q"
                  },
                  {
                    "id": "did:orb:uAAA:EiAcQ0aIQUWID6Tvk76ibkDPWVpbQfK1XlkhQjuQTn-X8A"
                  },
                  {
                    "id": "did:orb:uAAA:EiAS-LE3LJLRlDjkLBvzqpVdlJmqoDSYfjBp0rFuxJhTqg"
                  },
                  {
                    "id": "did:orb:uAAA:EiCtz5gAz3KCGlXKAtcows6ci7uM2uX1dpGDXyzFO3K6Uw"
                  },
                  {
                    "id": "did:orb:uAAA:EiDRDMn2OgRLDJPIKAsHBAV7qadhNe1B5pLHS8PF3oS_Ig"
                  },
                  {
                    "id": "did:orb:uAAA:EiCms2W6kn1WAKqV4hka_mUhbyyX1a6XNfuVsTQRwaY5XQ"
                  }
                ]
              },
              "subject": "hl:uEiDOclra4zeucLxB0ziTL5QrixAoLhK-05z3ZEl7o1XSJg:uoQ-BeEJpcGZzOi8vYmFma3JlaWdvb2pubnZ5enh2enlseXFvdGhjanM3ZmJscm1pY3FscXN4M2p6ejUzZWpmNTJndm9zZXk"
            },
            "generator": "https://w3id.org/orb#v0",
            "tag": [
              {
                "href": "hl:uEiAkFg0pAVYpDvFnYVwk_bGTqvnuNLY0X0gX-ympRqL6og",
                "rel": [
                  "witness"
                ],
                "type": "Link"
              }
            ],
            "type": "AnchorObject",
            "url": "hl:uEiCp7ykqVgPs05jrV09Zll0nNzScwjt5zvf6hB6HctCsmw"
          },
          {
            "contentObject": {
              "@context": [
                "https://www.w3.org/2018/credentials/v1",
                "https://w3id.org/security/suites/jws-2020/v1"
              ],
              "credentialSubject": "hl:uEiCp7ykqVgPs05jrV09Zll0nNzScwjt5zvf6hB6HctCsmw",
              "id": "https://orb2.domain1.com/vc/d6959588-8bbb-4afe-885d-23e625516456",
              "issuanceDate": "2022-01-27T18:57:24.8765814Z",
              "issuer": "https://orb2.domain1.com",
              "proof": [
                {
                  "created": "2022-01-27T18:57:24.902Z",
                  "domain": "http://orb.vct:8077/maple2020",
                  "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..MynMVMqmcRoKEEaEyByMjmhIvDlyGwwrsXGCFPUguQAp8yhWGKzRf6HX4Y-cfhKEvPdqNul4Rea1Rh8O79hSBw",
                  "proofPurpose": "assertionMethod",
                  "type": "Ed25519Signature2018",
                  "verificationMethod": "did:web:orb.domain1.com#orb1key2"
                },
                {
                  "created": "2022-01-27T18:57:25.229246Z",
                  "domain": "https://orb.domain2.com",
                  "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..7AWGtArWfWGRvc9jeoyuk8cvVT_RxF6Bd2e6sq8KMc06Ywz3dldqseUXVSeLlg_UeX6ZX73aEmYKlHbTDUY3Dw",
                  "proofPurpose": "assertionMethod",
                  "type": "Ed25519Signature2018",
                  "verificationMethod": "did:web:orb.domain2.com#orb2key"
                }
              ],
              "type": "VerifiableCredential"
            },
            "generator": "https://w3id.org/orb#v0",
            "type": "AnchorObject",
            "url": "hl:uEiAkFg0pAVYpDvFnYVwk_bGTqvnuNLY0X0gX-ympRqL6og"
          }
        ],
        "attributedTo": "https://orb.domain1.com/services/orb",
        "index": "hl:uEiCp7ykqVgPs05jrV09Zll0nNzScwjt5zvf6hB6HctCsmw",
        "published": "2022-01-27T18:57:24.8542932Z",
        "type": "AnchorEvent",
        "url": "hl:uEiD2dUBkkx8XQaVPlK05iMgHZHrSthy8lpE5cahfClJ4Qw:uoQ-BeEJpcGZzOi8vYmFma3JlaWh3b3ZhZ2pleTdjNWEya3Q0dXZ1NHlyc2FobXI1bmZucTR4c2xqY29scnZicHF1dXR5aW0"
      },
      "published": "2022-01-27T18:57:25.6558989Z",
      "to": [
        "https://orb.domain1.com/services/orb/followers",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Create"
    }
  ],
  "totalItems": 3212,
  "type": "OrderedCollectionPage"
}
```

### POST

A POST request to the outbox endpoint adds the activity contained in the request to the service's Outbox,
which will be processed by the ActivityPub [Outbox](../activitypub.html#outbox-inbox). This endpoint is restricted by
authorization rules, i.e. the requester must have a valid authentication bearer token,
which is usually an administrator token.

**Example**

```
POST /services/orb/outbox HTTP/1.1
Host: orb.domain1.com
Content-Type: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate

{
  "@context": "https://www.w3.org/ns/activitystreams",
  "actor": "https://orb.domain1.com/services/orb",
  "id": "https://orb.domain1.com/services/orb/activities/91dbb2e2-1040-4fd5-bd2e-1bcd67bdda8a",
  "to": "https://orb.domain2.com/services/orb",
  "object": "https://orb.domain2.com/services/orb",
  "type": "Follow"
}
```

## Inbox

**Endpoint:** /services/orb/inbox

The activities posted to the inbox of this service are returned via this endpoint. If no paging parameters are specified
in the URL then the response contains information about the inbox collection, i.e. the links to the first and last page, as
well as the total number of items in the inbox. A subsequent request may be made using parameters
that include a specified page number in order to retrieve the actual items.

**Example**

```
GET /services/orb/inbox HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

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

```
GET /services/orb/inbox?page=true HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/inbox?page=true&page-num=0",
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
      "id": "https://orb.domain2.com/services/orb/activities/204d4e3e-f3e5-4395-9a4b-74865d43ded7",
      "object": {
        "@context": "https://www.w3.org/ns/activitystreams",
        "actor": "https://orb.domain1.com/services/orb",
        "id": "https://orb.domain1.com/services/orb/activities/61181815-f857-4710-b239-c5949b70d2f3",
        "object": "hl:uEiDKqLL1x6qmJ65ClfE0EUyGIYlg0_lKH0JlBvunln9A1Q",
        "target": "https://w3id.org/activityanchors#AnchorWitness",
        "to": [
          "https://orb.domain2.com/services/orb",
          "https://www.w3.org/ns/activitystreams#Public"
        ],
        "type": "Offer"
      }
    }
  ],
  "totalItems": 19,
  "type": "OrderedCollectionPage"
}
```

## Witnesses

**Endpoint:** /services/orb/witnesses

The witnesses of this service are returned via this endpoint. If no paging parameters are specified
in the URL then the response contains information about the _witnesses_ collection, i.e. the links to the first and last page, as
well as the total number of items in the collection. A subsequent request may be made using parameters
that include a specified page number in order to retrieve the actual items.

**Example**

```
GET /services/orb/witnesses HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

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

```
GET /services/orb/witnesses??page=true&page-num=4 HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

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

The services that are witnessing anchor events for this service are returned via this endpoint. If no paging parameters are specified
in the URL then the response contains information about the collection, i.e. the links to the first and last page, as
well as the total number of items in the collection. A subsequent request may be made using parameters
that include a specified page number in order to retrieve the actual items.

**Example**

```
GET /services/orb/witnessing HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

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

```
GET /services/orb/witnessing?page=true HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

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

The anchor events that are _liked_ are returned via this endpoint. (Liked means that the
anchors in the response were all added to the ledger.) If no paging parameters are specified in the URL then the response
contains information about the collection, i.e. the links to the first and last page, as well as the total number of
items in the collection. A subsequent request may be made using parameters that include a specified page number in order
to retrieve the actual items.

**Example**

```
GET /services/orb/liked HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "first": "https://orb.domain1.com/services/orb/liked?page=true",
  "id": "https://orb.domain1.com/services/orb/liked",
  "last": "https://orb.domain1.com/services/orb/liked?page=true&page-num=3",
  "totalItems": 232,
  "type": "OrderedCollection"
}
```

```
GET /services/orb/liked?page=true HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/liked?page=true&page-num=0",
  "next": "https://orb.domain1.com/services/orb/liked?page=true&page-num=1",
  "orderedItems": [
    "hl:uEiCsFp-ft8tI1DFGbXs78tw-HS561mMPa3Z6GsGAHElrNQ:uoQ-CeE1odHRwczovL3NhbGx5LmV4YW1wbGUuY29tL2Nhcy91RWlDc0ZwLWZ0OHRJMURGR2JYczc4dHctSFM1NjFtTVBhM1o2R3NHQUhFbHJOUXhCaXBmczovL2JhZmtyZWlmbWMycHo3bjZsamRrZGNydG5wbTU3ZnhiNmR1eGh2dnRkYjV2eG02cTJ5Z2FieXNsbGd1",
    "hl:uEiCsFp-ft8tI1DFGbXs78tw-HS561mMPa3Z6GsGAHElrNQ:uoQ-CeE1odHRwczovL3NhbGx5LmV4YW1wbGUuY29tL2Nhcy91RWlDc0ZwLWZ0OHRJMURGR2JYczc4dHctSFM1NjFtTVBhM1o2R3NHQUhFbHJOUXhCaXBmczovL2JhZmtyZWlmbWMycHo3bjZsamRrZGNydG5wbTU3ZnhiNmR1eGh2dnRkYjV2eG02cTJ5Z2FieXNsbGd1",
    "hl:uEiCsFp-ft8tI1DFGbXs78tw-HS561mMPa3Z6GsGAHElrNQ:uoQ-CeE1odHRwczovL3NhbGx5LmV4YW1wbGUuY29tL2Nhcy91RWlDc0ZwLWZ0OHRJMURGR2JYczc4dHctSFM1NjFtTVBhM1o2R3NHQUhFbHJOUXhCaXBmczovL2JhZmtyZWlmbWMycHo3bjZsamRrZGNydG5wbTU3ZnhiNmR1eGh2dnRkYjV2eG02cTJ5Z2FieXNsbGd1",
    "hl:uEiCsFp-ft8tI1DFGbXs78tw-HS561mMPa3Z6GsGAHElrNQ:uoQ-CeE1odHRwczovL3NhbGx5LmV4YW1wbGUuY29tL2Nhcy91RWlDc0ZwLWZ0OHRJMURGR2JYczc4dHctSFM1NjFtTVBhM1o2R3NHQUhFbHJOUXhCaXBmczovL2JhZmtyZWlmbWMycHo3bjZsamRrZGNydG5wbTU3ZnhiNmR1eGh2dnRkYjV2eG02cTJ5Z2FieXNsbGd1",
    "hl:uEiCsFp-ft8tI1DFGbXs78tw-HS561mMPa3Z6GsGAHElrNQ:uoQ-CeE1odHRwczovL3NhbGx5LmV4YW1wbGUuY29tL2Nhcy91RWlDc0ZwLWZ0OHRJMURGR2JYczc4dHctSFM1NjFtTVBhM1o2R3NHQUhFbHJOUXhCaXBmczovL2JhZmtyZWlmbWMycHo3bjZsamRrZGNydG5wbTU3ZnhiNmR1eGh2dnRkYjV2eG02cTJ5Z2FieXNsbGd1"
  ],
  "totalItems": 2322,
  "type": "OrderedCollectionPage"
}
```

## Likes

**Endpoint:** /services/orb/likes?id=[id]

This endpoint returns a collection of _Like_ activities for a given anchor. If no paging parameters are specified
in the URL then the response contains information about the collection, i.e. the links to the first and last page, as
well as the total number of items in the collection. A subsequent request may be made using parameters
that include a specified page number in order to retrieve the actual items.

**Example**

```
GET /services/orb/likes?id=hl%3AuEiCVELh0dDXC1HrPqfUGt0RQFsjCv8iIhhEJ-AnU3LfY9g%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ1ZFTGgwZERYQzFIclBxZlVHdDBSUUZzakN2OGlJaGhFSi1BblUzTGZZOWd4QmlwZnM6Ly9iYWZrcmVpZXZjYzRoaTVidnlsa2h2dDVqNnVkbG9yY3FjM2VtZnA2aXJjZGJjY3B5YmhrbnpuNnk2eQ HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "first": "https://orb.domain1.com/services/orb/likes?id=hl%3AuEiCVELh0dDXC1HrPqfUGt0RQFsjCv8iIhhEJ-AnU3LfY9g%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ1ZFTGgwZERYQzFIclBxZlVHdDBSUUZzakN2OGlJaGhFSi1BblUzTGZZOWd4QmlwZnM6Ly9iYWZrcmVpZXZjYzRoaTVidnlsa2h2dDVqNnVkbG9yY3FjM2VtZnA2aXJjZGJjY3B5YmhrbnpuNnk2eQ&page=true",
  "id": "https://orb.domain1.com/services/orb/likes?id=hl%3AuEiCVELh0dDXC1HrPqfUGt0RQFsjCv8iIhhEJ-AnU3LfY9g%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ1ZFTGgwZERYQzFIclBxZlVHdDBSUUZzakN2OGlJaGhFSi1BblUzTGZZOWd4QmlwZnM6Ly9iYWZrcmVpZXZjYzRoaTVidnlsa2h2dDVqNnVkbG9yY3FjM2VtZnA2aXJjZGJjY3B5YmhrbnpuNnk2eQ",
  "last": "https://orb.domain1.com/services/orb/likes?id=hl%3AuEiCVELh0dDXC1HrPqfUGt0RQFsjCv8iIhhEJ-AnU3LfY9g%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ1ZFTGgwZERYQzFIclBxZlVHdDBSUUZzakN2OGlJaGhFSi1BblUzTGZZOWd4QmlwZnM6Ly9iYWZrcmVpZXZjYzRoaTVidnlsa2h2dDVqNnVkbG9yY3FjM2VtZnA2aXJjZGJjY3B5YmhrbnpuNnk2eQ&page=true",
  "totalItems": 2,
  "type": "OrderedCollection"
}
```

```
GET /services/orb/likes?id=hl%3AuEiCVELh0dDXC1HrPqfUGt0RQFsjCv8iIhhEJ-AnU3LfY9g%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ1ZFTGgwZERYQzFIclBxZlVHdDBSUUZzakN2OGlJaGhFSi1BblUzTGZZOWd4QmlwZnM6Ly9iYWZrcmVpZXZjYzRoaTVidnlsa2h2dDVqNnVkbG9yY3FjM2VtZnA2aXJjZGJjY3B5YmhrbnpuNnk2eQ&page=true HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/likes?id=hl%3AuEiCVELh0dDXC1HrPqfUGt0RQFsjCv8iIhhEJ-AnU3LfY9g%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ1ZFTGgwZERYQzFIclBxZlVHdDBSUUZzakN2OGlJaGhFSi1BblUzTGZZOWd4QmlwZnM6Ly9iYWZrcmVpZXZjYzRoaTVidnlsa2h2dDVqNnVkbG9yY3FjM2VtZnA2aXJjZGJjY3B5YmhrbnpuNnk2eQ&page=true&page-num=0",
  "orderedItems": [
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain2.com/services/orb",
      "id": "https://orb.domain2.com/services/orb/activities/eefb72ba-d942-4151-9e1d-775ee567076b",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiCVELh0dDXC1HrPqfUGt0RQFsjCv8iIhhEJ-AnU3LfY9g:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ1ZFTGgwZERYQzFIclBxZlVHdDBSUUZzakN2OGlJaGhFSi1BblUzTGZZOWd4QmlwZnM6Ly9iYWZrcmVpZXZjYzRoaTVidnlsa2h2dDVqNnVkbG9yY3FjM2VtZnA2aXJjZGJjY3B5YmhrbnpuNnk2eQ"
      },
      "published": "2022-01-31T21:18:30.0660672Z",
      "result": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiCVELh0dDXC1HrPqfUGt0RQFsjCv8iIhhEJ-AnU3LfY9g:uoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ1ZFTGgwZERYQzFIclBxZlVHdDBSUUZzakN2OGlJaGhFSi1BblUzTGZZOWc"
      },
      "to": [
        "https://orb.domain1.com/services/orb",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Like"
    },
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain3.com/services/orb",
      "id": "https://orb.domain3.com/services/orb/activities/20e5bf3a-7198-490f-9aab-d4ed8d38c35d",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiCVELh0dDXC1HrPqfUGt0RQFsjCv8iIhhEJ-AnU3LfY9g:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ1ZFTGgwZERYQzFIclBxZlVHdDBSUUZzakN2OGlJaGhFSi1BblUzTGZZOWd4QmlwZnM6Ly9iYWZrcmVpZXZjYzRoaTVidnlsa2h2dDVqNnVkbG9yY3FjM2VtZnA2aXJjZGJjY3B5YmhrbnpuNnk2eQ"
      },
      "published": "2022-01-31T21:18:30.0708023Z",
      "result": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiCVELh0dDXC1HrPqfUGt0RQFsjCv8iIhhEJ-AnU3LfY9g:uoQ-BeEtodHRwczovL29yYi5kb21haW4zLmNvbS9jYXMvdUVpQ1ZFTGgwZERYQzFIclBxZlVHdDBSUUZzakN2OGlJaGhFSi1BblUzTGZZOWc"
      },
      "to": [
        "https://orb.domain1.com/services/orb",
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

**Endpoint:** /services/orb/shares?id=[id]

The _Create_ activities that were _Announced_ are returned via this endpoint. If no paging parameters are specified in the URL
then the response contains information about the collection, i.e. the links to the first and last page, as well as the total number of
items in the collection. A subsequent request may be made using parameters that include a specified page number in order
to retrieve the actual items.

**Example**

```
GET /services/orb/shares?id=hl%3AuEiCXSUxo3mD0ErmcftIRLnQf1jIV9c3IZA-TIY8qBUKNPQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ1hTVXhvM21EMEVybWNmdElSTG5RZjFqSVY5YzNJWkEtVElZOHFCVUtOUFE HTTP/1.1
Host: orb.domain3.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "first": "https://orb.domain3.com/services/orb/shares?id=hl%3AuEiCXSUxo3mD0ErmcftIRLnQf1jIV9c3IZA-TIY8qBUKNPQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ1hTVXhvM21EMEVybWNmdElSTG5RZjFqSVY5YzNJWkEtVElZOHFCVUtOUFE&page=true",
  "id": "https://orb.domain3.com/services/orb/shares?id=hl%3AuEiCXSUxo3mD0ErmcftIRLnQf1jIV9c3IZA-TIY8qBUKNPQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ1hTVXhvM21EMEVybWNmdElSTG5RZjFqSVY5YzNJWkEtVElZOHFCVUtOUFE",
  "last": "https://orb.domain3.com/services/orb/shares?id=hl%3AuEiCXSUxo3mD0ErmcftIRLnQf1jIV9c3IZA-TIY8qBUKNPQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ1hTVXhvM21EMEVybWNmdElSTG5RZjFqSVY5YzNJWkEtVElZOHFCVUtOUFE&page=true&page-num=0",
  "totalItems": 1,
  "type": "OrderedCollection"
}
```

```
GET /services/orb/shares?id=hl%3AuEiCXSUxo3mD0ErmcftIRLnQf1jIV9c3IZA-TIY8qBUKNPQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ1hTVXhvM21EMEVybWNmdElSTG5RZjFqSVY5YzNJWkEtVElZOHFCVUtOUFE&page=true HTTP/1.1
Host: orb.domain3.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain3.com/services/orb/shares?id=hl%3AuEiCXSUxo3mD0ErmcftIRLnQf1jIV9c3IZA-TIY8qBUKNPQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ1hTVXhvM21EMEVybWNmdElSTG5RZjFqSVY5YzNJWkEtVElZOHFCVUtOUFE&page=true&page-num=0",
  "orderedItems": [
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain1.com/services/orb",
      "id": "https://orb.domain1.com/services/orb/activities/a7e745da-462f-40a0-a862-294c361f39bc",
      "object": {
        "items": [
          {
            "@context": "https://w3id.org/activityanchors/v1",
            "attachment": [
              {
                "contentObject": {
                  "properties": {
                    "https://w3id.org/activityanchors#generator": "https://w3id.org/orb#v0",
                    "https://w3id.org/activityanchors#resources": [
                      {
                        "id": "did:orb:uAAA:EiC6PgUkG8n4CEWsTF6F0OY2Bjyx0DPm0joNrchOA9Gy_Q"
                      },
                      {
                        "id": "did:orb:uAAA:EiDRMpGl8EJURWpGYa-3qXIFqU0pSl1uucxDWsLv1od68g"
                      },
                      {
                        "id": "did:orb:uAAA:EiBacjgwMrJo-53kBE7f29NkBKSwomMNm39TAKbPdhSW-w"
                      },
                      {
                        "id": "did:orb:uAAA:EiDQJXCDcJThg0jojTx3Im-SRFhkrqLWiB786x0rv_t2AQ"
                      },
                      {
                        "id": "did:orb:uAAA:EiAaO5gHQpGUlWhKInP0-GbELYTYfXPYteCAE0xCywatOQ"
                      }
                    ]
                  },
                  "subject": "hl:uEiC_aFLX8Qr_yUIcNQGAgP9QjJEz7AHYsLRniP9Lt6rnGA:uoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ19hRkxYOFFyX3lVSWNOUUdBZ1A5UWpKRXo3QUhZc0xSbmlQOUx0NnJuR0E"
                },
                "generator": "https://w3id.org/orb#v0",
                "tag": [
                  {
                    "href": "hl:uEiCjgc_X3nazAtzhTtmePFjjjX3-KF3nhhNkFReYQvvfmA",
                    "rel": [
                      "witness"
                    ],
                    "type": "Link"
                  }
                ],
                "type": "AnchorObject",
                "url": "hl:uEiBZqvWguHFaX7KvUWx1IOE42p2MbTPvBlV5CAALqpknAw"
              },
              {
                "contentObject": {
                  "@context": [
                    "https://www.w3.org/2018/credentials/v1",
                    "https://w3id.org/security/suites/jws-2020/v1"
                  ],
                  "credentialSubject": "hl:uEiBZqvWguHFaX7KvUWx1IOE42p2MbTPvBlV5CAALqpknAw",
                  "id": "https://orb.domain2.com/vc/73fd2e01-d7f8-4393-ae12-dbd5818fa828",
                  "issuanceDate": "2022-01-31T22:02:56.6881123Z",
                  "issuer": "https://orb.domain2.com",
                  "proof": [
                    {
                      "created": "2022-01-31T22:02:56.6884355Z",
                      "domain": "https://orb.domain2.com",
                      "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..Wp8qbwqVDReOSbRndZLNZ3AJ3Xws_n5XP0F_lrXZ1Ym_RR2_t7PcGMPM2tpGKhWi1lmqSZ7X9sz_MayVWw-nCA",
                      "proofPurpose": "assertionMethod",
                      "type": "Ed25519Signature2018",
                      "verificationMethod": "did:web:orb.domain2.com#orb2key"
                    },
                    {
                      "created": "2022-01-31T22:02:56.92Z",
                      "domain": "http://orb.vct:8077/maple2020",
                      "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..P5GYPJnfFsM6o87PBQUYv8GR48scVeVb4J37pDgi96pvcnMxqdcBRIUxHKFuKQX1gbwFJ2V0y0Ivtnosxdy1CQ",
                      "proofPurpose": "assertionMethod",
                      "type": "Ed25519Signature2018",
                      "verificationMethod": "did:web:orb.domain1.com#orb1key2"
                    },
                    {
                      "created": "2022-01-31T22:02:56.92Z",
                      "domain": "http://orb.vct:8077/maple2020",
                      "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..P5GYPJnfFsM6o87PBQUYv8GR48scVeVb4J37pDgi96pvcnMxqdcBRIUxHKFuKQX1gbwFJ2V0y0Ivtnosxdy1CQ",
                      "proofPurpose": "assertionMethod",
                      "type": "Ed25519Signature2018",
                      "verificationMethod": "did:web:orb.domain1.com#orb1key2"
                    }
                  ],
                  "type": "VerifiableCredential"
                },
                "generator": "https://w3id.org/orb#v0",
                "type": "AnchorObject",
                "url": "hl:uEiCjgc_X3nazAtzhTtmePFjjjX3-KF3nhhNkFReYQvvfmA"
              }
            ],
            "attributedTo": "https://orb.domain2.com/services/orb",
            "index": "hl:uEiBZqvWguHFaX7KvUWx1IOE42p2MbTPvBlV5CAALqpknAw",
            "published": "2022-01-31T22:02:56.6848463Z",
            "type": "AnchorEvent",
            "url": "hl:uEiCXSUxo3mD0ErmcftIRLnQf1jIV9c3IZA-TIY8qBUKNPQ:uoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ1hTVXhvM21EMEVybWNmdElSTG5RZjFqSVY5YzNJWkEtVElZOHFCVUtOUFE"
          }
        ],
        "totalItems": 1,
        "type": "Collection"
      },
      "published": "2022-01-31T22:02:57.7138717Z",
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

This endpoint returns an activity for the specified ID.

**Example**

```
GET /services/orb/activities/91dbb2e2-1040-4fd5-bd2e-1bcd67bdda8a HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

````json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "actor": "https://orb.domain1.com/services/orb",
  "id": "https://orb.domain1.com/services/orb/activities/91dbb2e2-1040-4fd5-bd2e-1bcd67bdda8a",
  "object": {
    "@context": "https://www.w3.org/ns/activitystreams",
    "actor": "https://orb.domain5.com/services/orb",
    "id": "https://orb.domain5.com/services/orb/activities/b9f63dd7-b3da-414f-8c59-894d964787c4",
    "object": "https://orb.domain2.com/services/orb",
    "to": "https://orb.domain1.com/services/orb",
    "type": "Follow"
  },
  "to": "https://orb.domain5.com/services/orb",
  "type": "Accept"
}
````

## Accept List

**Endpoint:** /services/orb/acceptlist

An [accept list](../activitypub.html#accept-list) is a database of server URLs that are authorized for a particular type of operation.

### POST

The accept-list is updated using a POST request to this endpoint.

**Example**

```
POST /services/orb/acceptlist HTTP/1.1
Host: orb.domain1.com
Content-Type: application/ld+json"
Accept-Encoding: gzip, deflate

[
  {
    "add": [
      "https://orb.domain2.com/services/orb",
      "https://orb.domain3.com/services/orb"
    ],
    "type": "follow"
  },
  {
    "add": [
      "https://orb.domain2.com/services/orb",
      "https://orb.domain3.com/services/orb"
    ],
    "type": "invite-witness"
  }
]
```

### GET

The accept-list is retrieved using a GET request to this endpoint.

**Example**

```
GET /services/orb/acceptlist HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json
Accept-Encoding: gzip, deflate
```

```json
[
  {
    "type": "follow",
    "url": [
      "https://orb.domain2.com/services/orb",
      "https://orb.domain3.com/services/orb"
    ]
  },
  {
    "type": "invite-witness",
    "url": [
      "https://orb.domain2.com/services/orb",
      "https://orb.domain3.com/services/orb"
    ]
  }
]
```
