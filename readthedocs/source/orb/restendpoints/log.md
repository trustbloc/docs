# Log Endpoint

**Endpoint:** /log

## GET

Returns the URL of the current VCT [log](../vct/log.html#log-configuration).

**Example**

```
GET /log HTTP/1.1
Host: orb.domain1.com
```

Response contains the log URL:

```text
https://orb.vct:8077/maple2020
```

## POST

Sets the URL of the current VCT [log](../vct/log.html#log-configuration).

**Example**

```text
POST /log HTTP/1.1
Host: orb.domain1.com
Content-Type: text/plain

https://orb.vct:8077/maple2020
```
