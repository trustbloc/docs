# Witness Policy Endpoints

**Endpoint:** /policy

### POST

Configures the witness policy as per [Witness Policy](../witnesspolicy.html#witness-policy).

**Example**

Request:

```
POST /policy HTTP/1.1
Host: orb.domain1.com
Content-Type: text/plain

MinPercent(100,batch) AND MinPercent(50,system)
```

### GET

Retrieves the current witness policy as per [Witness Policy](../witnesspolicy.html#witness-policy).

**Example**

Request:

```
GET /policy HTTP/1.1
Host: orb.domain1.com
Accept: text/plain
```

Response:

```
MinPercent(100,batch) AND MinPercent(50,system)
```
