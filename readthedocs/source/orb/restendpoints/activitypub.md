# ActivityPub Endpoints

The ActivityPub spec defines a number of REST endpoints that provide information about a [service](https://www.w3.org/TR/activitypub/#actor-objects).
The [ActivityAnchors](https://trustbloc.github.io/activityanchors/#actor-discovery) spec extends the ActivityPub spec to provide additional endpoints.

## Service

**Endpoint:** /services/orb

### GET

The Orb service is retrieved using the */services/orb* endpoint. The returned data is a JSON document
that contains REST endpoints that may be queried to return additional information.

**Example**

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

## Keys

**Endpoint:** /services/orb/keys/[id]

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
      "actor": "https://orb.domain2.com/services/orb",
      "id": "https://orb.domain2.com/services/orb/activities/33762e97-3826-4b24-86ac-91356ca75299",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "object": {
          "linkset": [
            {
              "anchor": "hl:uEiB9Chtelm2NlC00j8_I0Febv5Lre3Qsp-ElzpYPeO2HhA",
              "author": "https://orb.domain2.com/services/orb",
              "original": [
                {
                  "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiBEYPquNVjC7fIduIkd7za-va4KJ5_g6DlQCpBhb8G58w%22%2C%22author%22%3A%22https%3A%2F%2Forb.domain2.com%2Fservices%2Forb%22%2C%22item%22%3A%5B%7B%22href%22%3A%22did%3Aorb%3AuEiBvtB4n61hbCWzfbquORj6OD_j4OD4nCSXs4oVuVNuucA%3AEiBlWWTU5i71R2uOAq04RZlNC148RN44gRAzSU9t4_hVGg%22%2C%22previous%22%3A%22hl%3AuEiBvtB4n61hbCWzfbquORj6OD_j4OD4nCSXs4oVuVNuucA%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiAGmhR9leewo8DEVhzh5dL6XjwCjqO9H_nGXdBhPf8WlA%3AEiAPvYn4qoOwAQLjtBqur563OgicWQaH4lB1RMh9oEER0Q%22%2C%22previous%22%3A%22hl%3AuEiAGmhR9leewo8DEVhzh5dL6XjwCjqO9H_nGXdBhPf8WlA%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiAGmhR9leewo8DEVhzh5dL6XjwCjqO9H_nGXdBhPf8WlA%3AEiCQGgEaHyhghhRIsT4hav8GMwRl-EuyGP_DMULTNTfZUg%22%2C%22previous%22%3A%22hl%3AuEiAGmhR9leewo8DEVhzh5dL6XjwCjqO9H_nGXdBhPf8WlA%22%7D%2C%7B%22href%22%3A%22did%3Aorb%3AuEiAGmhR9leewo8DEVhzh5dL6XjwCjqO9H_nGXdBhPf8WlA%3AEiA4SRtKtlBGm-VS78mtZEiAOBysN_ra8zv66Gnw_uJw4A%22%2C%22previous%22%3A%22hl%3AuEiAGmhR9leewo8DEVhzh5dL6XjwCjqO9H_nGXdBhPf8WlA%22%7D%5D%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%7D%5D%7D",
                  "type": "application/linkset+json"
                }
              ],
              "profile": "https://w3id.org/orb#v0",
              "related": [
                {
                  "href": "data:application/json,%7B%22linkset%22%3A%5B%7B%22anchor%22%3A%22hl%3AuEiB9Chtelm2NlC00j8_I0Febv5Lre3Qsp-ElzpYPeO2HhA%22%2C%22profile%22%3A%22https%3A%2F%2Fw3id.org%2Forb%23v0%22%2C%22up%22%3A%5B%7B%22href%22%3A%22hl%3AuEiBvtB4n61hbCWzfbquORj6OD_j4OD4nCSXs4oVuVNuucA%3AuoQ-BeEJpcGZzOi8vYmFma3JlaWRwd3FwY3AyMnlsbWV3engzb3ZvaGVtcHVvYjc0cHFvYjZlNGVzbDNoY3F2eGZqdzVvb2E%22%7D%2C%7B%22href%22%3A%22hl%3AuEiAGmhR9leewo8DEVhzh5dL6XjwCjqO9H_nGXdBhPf8WlA%3AuoQ-BeEJpcGZzOi8vYmFma3JlaWFndGlraDNmcGh3Y3I0YnJjd2R0cTZsdXgybHk2YWZkdmR4dXA3dHJzNTJicXQzN3l3c3E%22%7D%5D%2C%22via%22%3A%5B%7B%22href%22%3A%22hl%3AuEiBEYPquNVjC7fIduIkd7za-va4KJ5_g6DlQCpBhb8G58w%3AuoQ-BeEJpcGZzOi8vYmFma3JlaWNlbWQ1azRua3l5bHc3ZWhueXJlbzY2bnY2eHd4YXVqNDc0ZHVkc3Vha3NicXc3cW56Nm0%22%7D%5D%7D%5D%7D",
                  "type": "application/linkset+json"
                }
              ],
              "replies": [
                {
                  "href": "data:application/json,%7B%22%40context%22%3A%5B%22https%3A%2F%2Fwww.w3.org%2F2018%2Fcredentials%2Fv1%22%2C%22https%3A%2F%2Fw3id.org%2Fsecurity%2Fsuites%2Fed25519-2020%2Fv1%22%5D%2C%22credentialSubject%22%3A%22hl%3AuEiB9Chtelm2NlC00j8_I0Febv5Lre3Qsp-ElzpYPeO2HhA%22%2C%22id%22%3A%22https%3A%2F%2Forb.domain2.com%2Fvc%2F36c995be-99bb-4d0b-9a47-7027aee67bfe%22%2C%22issuanceDate%22%3A%222022-03-17T19%3A50%3A48.7559537Z%22%2C%22issuer%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proof%22%3A%5B%7B%22created%22%3A%222022-03-17T19%3A50%3A48.7567186Z%22%2C%22domain%22%3A%22https%3A%2F%2Forb.domain2.com%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22_nOavU_Te7QCyj6GIxGtnER-SWvawrUygcqt-3HyV_3P_LahVxxZmuNtOdPtrRTI4aZJUM6NmUMq_7gxX5ZFDw%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain2.com%23orb2key%22%7D%2C%7B%22created%22%3A%222022-03-17T19%3A50%3A49.245Z%22%2C%22domain%22%3A%22http%3A%2F%2Forb.vct%3A8077%2Fmaple2020%22%2C%22proofPurpose%22%3A%22assertionMethod%22%2C%22proofValue%22%3A%22bzvRXEWAcskOjz6WgUDDos3e9SiS2DfwmRAwHeuO3rBc0vmbSF6DiVyqeNUc_cSQ8OBYasSiTnWZaJmaEHRVBw%22%2C%22type%22%3A%22Ed25519Signature2020%22%2C%22verificationMethod%22%3A%22did%3Aweb%3Aorb.domain1.com%23orb1key2%22%7D%5D%2C%22type%22%3A%22VerifiableCredential%22%7D",
                  "type": "application/ld+json"
                }
              ]
            }
          ]
        },
        "type": "AnchorEvent",
        "url": "hl:uEiA-eY77-FHYKR5EyMZc3JIwJIowz3dl91oY-14Wxi3STg:uoQ-BeEJpcGZzOi8vYmFma3JlaWI2cGdocHg2Y3IzYXVyNHJnaXl6b256ZXJxZXNmZGJ0M3hteDN2dWdoM2x5bG1tbG9zank"
      },
      "published": "2022-03-17T19:50:51.0429926Z",
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
which will be processed by the ActivityPub [Outbox](../activitypub.html#outbox-inbox). This endpoint is restricted by
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
      "endTime": "2022-03-18T14:46:52.9323194Z",
      "id": "https://orb.domain2.com/services/orb/activities/66089798-7f4f-402a-ad7a-7cfdca8d1da2",
      "object": {
        "linkset": [
          {
            "anchor": "hl:uEiD9QBcRjoxy6FHK3HGMlufTjpaBxJHYup2IEE9kiIVWnQ",
            "author": "https://orb.domain2.com/services/orb",
            "original": [
              {
                "href": "data:application/gzip;base64,H4sIAAAAAAAA/0zOy06DQBTG8Xc5brlZsNbZTUrR3qJoqbGmIcCclmNhpg4DRhve3dS46PrLl9//BBXJQ4MG2PsJMlmUSgODsmLthPgqtYfRIIh8HHmlGBQxJpu327vwMPsOzGc9flxHc7Va0nK+Bwuy1vy/jTk2zHWVzh2h6ozktVOo2m1Qd1Rgcx7AAjJY/7Glxh0wECSY0jlrOedsQmFgJ3HVJvm9/ez/bD4W6ex1+hIucj56kp0c62T6EA9xnXo3HPqtBUetdlThRcCXT8JRen8GrzoP+m3/GwAA///uMKlG8QAAAA==",
                "type": "application/linkset+json"
              }
            ],
            "profile": "https://w3id.org/orb#v0",
            "related": [
              {
                "href": "data:application/gzip;base64,H4sIAAAAAAAA/zzN4XKaMAAA4HfJ/rbTBOoK/9aq0GqyS4xEsuvtQsASISajiFLPd99tP/YA331X0Jpj81H1IP55Beqoa9eBGNRtfFqYeUSfNDu4yzhbpqsgTXB72vODV0+X1zQ/efSyWESNecnEkYI74Du3N231l/e9/4gnk3Ngyq+ue5+4rvgyTMEdGIz6N9Vdtf//fOe/7mdLFC6D6nFal0jTaivzb9G8eR3D/rd9/pEtV45jg1fv8cnR++dq0bsyZWf96YY1isbcPDQFgrUS4WVtyVBsokO+w0O5zTzN2H7NZYIPLMFoG+KkbrAlTGaZElw/sKRfVZxNtYBzDmWyQdGWi5ZsUBlS257lEc/WY2RyITttM5/vaECC2hepnub2ERJORyUuYcGZVykZddBAgtpBCT8rkAxKWxqZEqhQD+XOf0oBraLg9nZ7u/0JAAD//7nq1Zp/AQAA",
                "type": "application/linkset+json"
              }
            ],
            "replies": [
              {
                "href": "data:application/gzip;base64,H4sIAAAAAAAA/3SS31OjMBzE/5fcaykhQIE83SkorddTTqbq3dw4IflWQinBEPpDx//9Bm315mZ8zu5nN5s8o69cNQZ2BtHfqDSm7ahtb7fb8dYdK/1gE+yENtcgoDGS1Z29cdDoQ+hK8SrrgPdamr3d9dJAZ4Mgvu9EFsEED5Y/I/QBue6LCrhBFJU17RMZR9kJ/1mp3X5yll646fm87pd51bKT3Sy961syTZJoJaeLmyZDIyTF4Dw0ULoYC7VmsnHGXK3tDbeJwxlEBbZEMMGWRwi3WBEwSxShYEsMPgN/wHRdzxoOMTOAKCKYEAu7lhPmjkfdCfXJOPSCyA+cXwc16M+D0Qi1Wqklos/DTZkB8Rl0MvDerAfeAbfhhoY4COw1a2sYpjtSr3rdqm6oyboOtJGqmYMplTgKFqzuh+MnPb1lkydxniXgtEEast3TZRY8dqE3g+zMWgHce+asOUlnJv+mspW23DQPW8+LZBXGezz/frnK8UVZ5D+uH8+9hducxsPoZt8O/OTtXa/lQ8NMr48lN6DlUnL2TzGKhBR0CwX9b6kv9+36xlhR+TCpqsRP7q9KQmK3iflsPr21ZKRWN9jczVy+99DLe/LiNYIVNZy+fyT08jcAAP//B5krCMACAAA=",
                "type": "application/ld+json"
              }
            ]
          }
        ]
      },
      "startTime": "2022-03-18T14:36:52.9323194Z",
      "target": "https://w3id.org/activityanchors#AnchorWitness",
      "to": [
        "https://orb.domain1.com/services/orb",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Offer"
    }
  ]
}
```

### POST

A POST request to the inbox endpoint adds the activity contained in the request to the service's Inbox,
which will be processed by the ActivityPub [Inbox](../activitypub.html#outbox-inbox). This endpoint is restricted by
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
GET /services/orb/likes/hl%3AuEiCIji03h37FZw-c3H0ngzoleXw77Zi-rQzCj6Lpnng30A%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ0lqaTAzaDM3Rlp3LWMzSDBuZ3pvbGVYdzc3WmktclF6Q2o2THBubmczMEF4QmlwZnM6Ly9iYWZrcmVpZWlyeXd0cGIzNnl2dHE3aGc0cHV0eWdvcmZwZjZkeDNteXgyd3F6cXVwdWx1ejQ2YngyYQ HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains page information:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/likes/hl%3AuEiCIji03h37FZw-c3H0ngzoleXw77Zi-rQzCj6Lpnng30A%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ0lqaTAzaDM3Rlp3LWMzSDBuZ3pvbGVYdzc3WmktclF6Q2o2THBubmczMEF4QmlwZnM6Ly9iYWZrcmVpZWlyeXd0cGIzNnl2dHE3aGc0cHV0eWdvcmZwZjZkeDNteXgyd3F6cXVwdWx1ejQ2YngyYQ",
  "type": "OrderedCollection",
  "totalItems": 1,
  "first": "https://orb.domain1.com/services/orb/likes/hl%3AuEiCIji03h37FZw-c3H0ngzoleXw77Zi-rQzCj6Lpnng30A%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ0lqaTAzaDM3Rlp3LWMzSDBuZ3pvbGVYdzc3WmktclF6Q2o2THBubmczMEF4QmlwZnM6Ly9iYWZrcmVpZWlyeXd0cGIzNnl2dHE3aGc0cHV0eWdvcmZwZjZkeDNteXgyd3F6cXVwdWx1ejQ2YngyYQ?page=true",
  "last": "https://orb.domain1.com/services/orb/likes/hl%3AuEiCIji03h37FZw-c3H0ngzoleXw77Zi-rQzCj6Lpnng30A%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ0lqaTAzaDM3Rlp3LWMzSDBuZ3pvbGVYdzc3WmktclF6Q2o2THBubmczMEF4QmlwZnM6Ly9iYWZrcmVpZWlyeXd0cGIzNnl2dHE3aGc0cHV0eWdvcmZwZjZkeDNteXgyd3F6cXVwdWx1ejQ2YngyYQ?page=true&page-num=0"
}
```

Request first page:

```
GET /services/orb/likes/hl%3AuEiCIji03h37FZw-c3H0ngzoleXw77Zi-rQzCj6Lpnng30A%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ0lqaTAzaDM3Rlp3LWMzSDBuZ3pvbGVYdzc3WmktclF6Q2o2THBubmczMEF4QmlwZnM6Ly9iYWZrcmVpZWlyeXd0cGIzNnl2dHE3aGc0cHV0eWdvcmZwZjZkeDNteXgyd3F6cXVwdWx1ejQ2YngyYQ?page=true&page-num=0 HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains items from the first page:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain1.com/services/orb/likes/hl%3AuEiCIji03h37FZw-c3H0ngzoleXw77Zi-rQzCj6Lpnng30A%3AuoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ0lqaTAzaDM3Rlp3LWMzSDBuZ3pvbGVYdzc3WmktclF6Q2o2THBubmczMEF4QmlwZnM6Ly9iYWZrcmVpZWlyeXd0cGIzNnl2dHE3aGc0cHV0eWdvcmZwZjZkeDNteXgyd3F6cXVwdWx1ejQ2YngyYQ?page=true&page-num=0",
  "type": "OrderedCollectionPage",
  "totalItems": 2,
  "orderedItems": [
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain3.com/services/orb",
      "id": "https://orb.domain3.com/services/orb/activities/27d18894-d917-4502-ac53-3df76a312f94",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiCIji03h37FZw-c3H0ngzoleXw77Zi-rQzCj6Lpnng30A:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ0lqaTAzaDM3Rlp3LWMzSDBuZ3pvbGVYdzc3WmktclF6Q2o2THBubmczMEF4QmlwZnM6Ly9iYWZrcmVpZWlyeXd0cGIzNnl2dHE3aGc0cHV0eWdvcmZwZjZkeDNteXgyd3F6cXVwdWx1ejQ2YngyYQ"
      },
      "published": "2022-03-18T14:42:06.8823334Z",
      "result": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiCIji03h37FZw-c3H0ngzoleXw77Zi-rQzCj6Lpnng30A:uoQ-BeEtodHRwczovL29yYi5kb21haW4zLmNvbS9jYXMvdUVpQ0lqaTAzaDM3Rlp3LWMzSDBuZ3pvbGVYdzc3WmktclF6Q2o2THBubmczMEE"
      },
      "to": [
        "https://orb.domain1.com/services/orb",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Like"
    },
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain2.com/services/orb",
      "id": "https://orb.domain2.com/services/orb/activities/bf3a32bb-a33e-45e2-84e9-c9a9f4e4a430",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiCIji03h37FZw-c3H0ngzoleXw77Zi-rQzCj6Lpnng30A:uoQ-CeEtodHRwczovL29yYi5kb21haW4xLmNvbS9jYXMvdUVpQ0lqaTAzaDM3Rlp3LWMzSDBuZ3pvbGVYdzc3WmktclF6Q2o2THBubmczMEF4QmlwZnM6Ly9iYWZrcmVpZWlyeXd0cGIzNnl2dHE3aGc0cHV0eWdvcmZwZjZkeDNteXgyd3F6cXVwdWx1ejQ2YngyYQ"
      },
      "published": "2022-03-18T14:42:06.9495521Z",
      "result": {
        "@context": "https://w3id.org/activityanchors/v1",
        "type": "AnchorEvent",
        "url": "hl:uEiCIji03h37FZw-c3H0ngzoleXw77Zi-rQzCj6Lpnng30A:uoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQ0lqaTAzaDM3Rlp3LWMzSDBuZ3pvbGVYdzc3WmktclF6Q2o2THBubmczMEE"
      },
      "to": [
        "https://orb.domain1.com/services/orb",
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
GET /services/orb/shares/hl%3AuEiBJQHYkD30RGKVlJ205wghkNQ_RTaZPxJkeGr5DDyaLIQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQkpRSFlrRDMwUkdLVmxKMjA1d2doa05RX1JUYVpQeEprZUdyNUREeWFMSVE HTTP/1.1
Host: orb.domain3.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains page information:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "first": "https://orb.domain3.com/services/orb/shares/hl%3AuEiBJQHYkD30RGKVlJ205wghkNQ_RTaZPxJkeGr5DDyaLIQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQkpRSFlrRDMwUkdLVmxKMjA1d2doa05RX1JUYVpQeEprZUdyNUREeWFMSVE?page=true",
  "id": "https://orb.domain3.com/services/orb/shares/hl%3AuEiBJQHYkD30RGKVlJ205wghkNQ_RTaZPxJkeGr5DDyaLIQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQkpRSFlrRDMwUkdLVmxKMjA1d2doa05RX1JUYVpQeEprZUdyNUREeWFMSVE",
  "last": "https://orb.domain3.com/services/orb/shares/hl%3AuEiBJQHYkD30RGKVlJ205wghkNQ_RTaZPxJkeGr5DDyaLIQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQkpRSFlrRDMwUkdLVmxKMjA1d2doa05RX1JUYVpQeEprZUdyNUREeWFMSVE?page=true&page-num=0",
  "totalItems": 1,
  "type": "OrderedCollection"
}
```

Request first page:

```
GET /services/orb/shares/hl%3AuEiBJQHYkD30RGKVlJ205wghkNQ_RTaZPxJkeGr5DDyaLIQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQkpRSFlrRDMwUkdLVmxKMjA1d2doa05RX1JUYVpQeEprZUdyNUREeWFMSVE?page=true HTTP/1.1
Host: orb.domain3.com
Accept: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
Accept-Encoding: gzip, deflate
```

Response contains items from the first page:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://orb.domain3.com/services/orb/shares/hl%3AuEiBJQHYkD30RGKVlJ205wghkNQ_RTaZPxJkeGr5DDyaLIQ%3AuoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQkpRSFlrRDMwUkdLVmxKMjA1d2doa05RX1JUYVpQeEprZUdyNUREeWFMSVE?page=true&page-num=0",
  "type": "OrderedCollectionPage",
  "totalItems": 1,
  "orderedItems": [
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "https://orb.domain1.com/services/orb",
      "id": "https://orb.domain1.com/services/orb/activities/59ffacbf-3f1e-4c62-909d-910a2cb3dc4e",
      "object": {
        "items": [
          {
            "@context": "https://w3id.org/activityanchors/v1",
            "object": {
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
            },
            "type": "AnchorEvent",
            "url": "hl:uEiBJQHYkD30RGKVlJ205wghkNQ_RTaZPxJkeGr5DDyaLIQ:uoQ-BeEtodHRwczovL29yYi5kb21haW4yLmNvbS9jYXMvdUVpQkpRSFlrRDMwUkdLVmxKMjA1d2doa05RX1JUYVpQeEprZUdyNUREeWFMSVE"
          }
        ],
        "totalItems": 1,
        "type": "Collection"
      },
      "published": "2022-03-18T14:39:02.6989689Z",
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

## Accept List

**Endpoint:** /services/orb/acceptlist

An [accept list](../activitypub.html#accept-list) is a database of server URLs that are authorized for a particular type of operation.

### GET

The accept-list is retrieved using a GET request to this endpoint.

**Example**

Request:

```
GET /services/orb/acceptlist HTTP/1.1
Host: orb.domain1.com
Accept: application/ld+json
Accept-Encoding: gzip, deflate
```

Response contains the accept-list for both _follow_ and _invite-witness_:

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

### POST

The accept-list is updated using a POST request to this endpoint. Services may
be added and removed from the accept-list for _Follow_ and _Invite_ witness activities.

**Example**

Request to add domain2 and domain3 to the _follow_ and _invite-witness_ accept-list as well as remove domain4
from the _follow_ accept-list:

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
    "remove": [
      "https://orb.domain4.com/services/orb",
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
