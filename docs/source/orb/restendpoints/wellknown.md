# .well-known Endpoints

The [.well-known](https://datatracker.ietf.org/doc/html/rfc5785) endpoints are used for discovery of services within an Orb domain.

## did-orb

**Endpoint:** /.well-known/did-orb

### GET

***Example***

Request:

```
GET /.well-known/did-orb HTTP/1.1
Host: orb.domain1.com
Accept: application/json
```

Response:

```json
{
  "resolutionEndpoint": "https://orb.domain1.com/sidetree/v1/identifiers",
  "operationEndpoint": "https://orb.domain1.com/sidetree/v1/operations"
}
```

## host-meta

**Endpoint:** /.well-known/host-meta

### GET

***Example***

Request:

```
GET /.well-known/host-meta HTTP/1.1
Host: orb.domain1.com
Accept: application/json
```

Response:

```json
{
  "links": [
    {
      "rel": "self",
      "type": "application/jrd+json",
      "template": "https://orb.domain1.com/.well-known/webfinger?resource={uri}"
    },
    {
      "rel": "self",
      "type": "application/activity+json",
      "href": "https://orb.domain1.com/services/orb"
    }
  ]
}
```

## host-meta.json

**Endpoint:** /.well-known/host-meta.json

### GET

***Example***

Request:

```
GET /.well-known/host-meta.json HTTP/1.1
Host: orb.domain1.com
```

Response:

```json
{
  "links": [
    {
      "rel": "self",
      "type": "application/jrd+json",
      "template": "https://orb.domain1.com/.well-known/webfinger?resource={uri}"
    },
    {
      "rel": "self",
      "type": "application/activity+json",
      "href": "https://orb.domain1.com/services/orb"
    }
  ]
}
```
## did.json

**Endpoint:** /.well-known/did.json

### GET

***Example***

Request:

```
GET /.well-known/did.json HTTP/1.1
Host: orb.domain1.com
```

Response:

```json
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:web:orb.domain1.com",
  "verificationMethod": [
    {
      "id": "did:web:orb.domain1.com#orb1key2",
      "controller": "did:web:orb.domain1.com",
      "type": "Ed25519VerificationKey2018",
      "publicKeyBase58": "Cd5DEcovdeweWWkMzXL5XmvtL5ov1jgcf7i6itnbVz8m"
    },
    {
      "id": "did:web:orb.domain1.com#orb1key",
      "controller": "did:web:orb.domain1.com",
      "type": "Ed25519VerificationKey2018",
      "publicKeyBase58": "245RycEHD7YAaaELTdUBLGF66iehCzpT3qy1jHLZoTMX"
    }
  ],
  "authentication": [
    "did:web:orb.domain1.com#orb1key2",
    "did:web:orb.domain1.com#orb1key"
  ],
  "assertionMethod": [
    "did:web:orb.domain1.com#orb1key2",
    "did:web:orb.domain1.com#orb1key"
  ],
  "capabilityDelegation": [
    "did:web:orb.domain1.com#orb1key2",
    "did:web:orb.domain1.com#orb1key"
  ],
  "capabilityInvocation": [
    "did:web:orb.domain1.com#orb1key2",
    "did:web:orb.domain1.com#orb1key"
  ]
}
```

## webfinger

**Endpoint:** /.well-known/webfinger?resource={uri}

### GET

***Example***

Request information about a specific DID:

```
GET /.well-known/webfinger?resource=did:orb:uEiDYK49ULtocB0ecZB5kzdo1oCAbJEJmAr65xpoWDq3Y1w:EiAWa2VAwDV3iC7q2nWygTy4b5AuwfTnf1g_ZYPZJFnJRA
Host: orb.domain1.com
Accept: application/json
```

Response:

```json
{
  "properties": {
    "https://trustbloc.dev/ns/anchor-origin": "https://orb.domain1.com",
    "https://trustbloc.dev/ns/min-resolvers": 1
  },
  "links": [
    {
      "rel": "self",
      "type": "application/did+ld+json",
      "href": "https://orb.domain1.com/sidetree/v1/identifiers/did:orb:uEiB9vhEm-4i8Vn1NSLcjYH50IsLCuGYDsKHpCEn6T0VHMg:EiAWa2VAwDV3iC7q2nWygTy4b5AuwfTnf1g_ZYPZJFnJRA"
    },
    {
      "rel": "via",
      "type": "application/ld+json",
      "href": "hl:uEiB9vhEm-4i8Vn1NSLcjYH50IsLCuGYDsKHpCEn6T0VHMg:uoQ-BeEJpcGZzOi8vYmFma3JlaWQ1eHlpc242NGl4cmxoMnRraXc0cndhN3R1ZWxibWZvZGdhb3lrZDJpaWpoNWU2cmtoZ2k"
    },
    {
      "rel": "service",
      "type": "application/activity+json",
      "href": "https://orb.domain1.com/services/orb"
    }
  ]
}
```

## nodeinfo

**Endpoint:** /.well-known/nodeinfo

### GET

Returns the [NodeInfo](system.html#node-info-endpoints) endpoints that may be queried to provide general information about an Orb server.

***Example***

Request:

```
GET /.well-known/nodeinfo HTTP/1.1
Host: orb.domain1.com
Accept: application/json
```

Response:

```
{
  "links": [
    {
      "rel": "http://nodeinfo.diaspora.software/ns/schema/2.0",
      "href": "https://orb.domain1.com/nodeinfo/2.0"
    },
    {
      "rel": "http://nodeinfo.diaspora.software/ns/schema/2.1",
      "href": "https://orb.domain1.com/nodeinfo/2.1"
    }
  ]
}
```
