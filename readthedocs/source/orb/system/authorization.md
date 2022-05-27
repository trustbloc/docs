# Authorization

Authorization in Orb is typically performed using bearer tokens for client-to-server communication.
For server-to-server communication, authentication is first performed using HTTP signatures and then each endpoint
performs its own authorization.

## Bearer Tokens

For endpoints that require authorization, a client must add a [bearer token](https://datatracker.ietf.org/doc/html/rfc6750#section-2.1)
to the HTTP request header as follows:

```
Authorization:[Bearer mytoken]
```

The server matches the bearer token in the request header against the required token(s) for the particular endpoint.
Each REST endpoint may be
[configured](../parameters.html#auth-tokens-def) to require tokens for both read (GET) and write (POST)
requests. If no token is defined for an endpoint then no authorization is performed. Multiple tokens may be defined
for read and write requests. If more than one token is defined then authorization succeeds if any of the tokens is found
in the request header. If a token for the request is required but not found in the request header then
[HTTP signature](#http-signatures) verification is performed.

```{image} ../../_static/orb/auth-bearer-token.svg
```

## HTTP Signatures

A common HTTP client within Orb is used for all server-to-server communications. If HTTP signatures are
[enabled](../parameters.html#enable-http-signatures) then the HTTP client sets additional headers on the HTTP
request.

### Headers

The following headers are added to the request before it is sent: _Date_, _Digest_, and _Signature_. For example:

```
Date:[Thu, 17 Feb 2022 14:29:24 GMT]
Digest:[SHA-512=vXXy/lk6D+XE8fYRTL7OS7izBUn6ntk60Rn97/gOSnbSxHZuaPvPTw1FW427qLqYpA0xGcXyPDQh4ujwret4aw==]
Signature:[keyId="https://orb.domain1.com/services/orb/keys/main-key",algorithm="Ed25519",headers="(request-target) Date Digest",signature="8AGVZi+xaDQ2kgD4sZUd4e2c2oOIkxzou2MoSSvQv72QJeSsoLa8+qJ1A+w2xkTDHNDfBTG8T/mNmtmYouv9Ag=="]
```

#### Date Header

The _Date_ header is set to the current date/time.

#### Digest Header

The _Digest_ header contains the hash of the request body, prepended by the algorithm. If a body was not included
in the request then _Digest_ will be empty.

#### Signature Header

The _Signature_ header contains a comma-separated string of field-values (i.e. field1=value1,field2=value2,...)
where the fields are defined as follows:

##### keyId

The value of the _keyId_ field is the URI of the public key that may be used to verify the signature.
The value of _keyId_ must be resolvable via HTTP or another protocol.

##### algorithm

The _algorithm_ field contains the algorithm used to sign the request. Orb uses
[KMS](../../kms/index.html#key-management-system-kms) to sign the request using the _Ed25519_ algorithm.

##### headers

The _headers_ field declares the set of headers that should be signed. The value of this field is a space-separated
string that contains the following set of fields:
1) **(request target)** - Includes the request method (GET, POST) and the request URI. For example: POST https://orb.domain1.com.
2) **Date** - Points to the [Date](#date-header) header.
3) **Digest** - Points to the [Digest](#digest-header) header (this value is not included if the _Digest_ header is empty).

##### signature

The _signature_ field contains the base64-encoded signature of the headers that are declared in the
[headers](#headers) field.

### Signature Verification

On receiving a request, the Orb server retrieves the URI specified in the value of the [keyId](#keyid) field from
the [Signature](#signature-header) header and then sends a request to this URI. The response of the request is in
the following format:

```json
{
  "id": "https://orb.domain1.com/services/orb/keys/main-key",
  "owner": "https://orb.domain1.com/services/orb",
  "publicKeyPem": "-----BEGIN PUBLIC KEY-----\nMCowBQYDK2VwAyEArK46BYVBHCM1Th+kKCFzabVmbTmTXRL5SwH+m2WvKKY=\n-----END PUBLIC KEY-----\n"
}
```

The value in the [signature](#signature) field is then verified using the public key. If not valid then an _HTTP 401 Unauthorized_ response is sent
to the client. If valid, the _owner_ of the key is retrieved. (The owner is an ActivityPub
[service](https://trustbloc.github.io/activityanchors/#actor-discovery) (actor)). Following is a sample response:

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
    "publicKeyPem": "-----BEGIN PUBLIC KEY-----\nMCowBQYDK2VwAyEArK46BYVBHCM1Th+kKCFzabVmbTmTXRL5SwH+m2WvKKY=\n-----END PUBLIC KEY-----\n"
  },
  "shares": "https://orb.domain1.com/services/orb/shares",
  "type": "Service",
  "witnesses": "https://orb.domain1.com/services/orb/witnesses",
  "witnessing": "https://orb.domain1.com/services/orb/witnessing"
}
```

The value of the _publicKey.id_ field in the ActivityPub service (actor) is then validated against the ID of the public
key to ensure that they match. If they don't match then an _HTTP 401 Unauthorized_ response is sent to the client.
If they _do_ match then authentication has succeeded and the request, along with the actor, is forwarded to the
appropriate handler. (The actor may be used by the handler to perform authorization.)

```{image} ../../_static/orb/httpsignatures.svg

```

Note that public keys and actors are cached (with an [expiry](../parameters.html#apclient-cache-expiration))
so that remote calls aren't required each time a signature verification is performed.
