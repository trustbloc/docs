# Rotate VCT logs

Rotate VCT logs process requires new configuration for both VCT server and Orb node.


## Configure VCT

Add new log to VCT [logs parameter](parameters.html#logs) 

VCT_LOGS=maple2023:rw@orb.trillian.log.server:8090,maple2022:rw@orb.trillian.log.server:8090

In order for changes to take effect an administrator has to re-start VCT server.

## Configure Orb

Configuring Orb node with new log requires setting up new VCT log URL for Orb domain, 
adding that new log URL to log monitoring list and removing old log URL from log monitoring list. 
An administrator may want to wait some time before deactivating old log in order to allow 
for all items in the old log to be processed and for log to be verified. 

Rotate VCT Log Steps for Orb Node:
- configure Orb node with new VCT log URL
- add new log URL to log monitoring list
- remove old log URL from log monitoring list 

  
### Configure Orb node with new VCT log

An administrator can configure new VCT log per Orb domain by posting new log URL to /log endpoint. 
See [log configuration](log.html#log-configuration) REST endpoint for more information.

```json
POST /log HTTP/1.1
Host: orb.domain1.com
Content-Type: application/ld+json

http://orb.vct:8077/maple2023
```

### Activate monitoring for new log

Activate monitoring of new log by posting to log-monitor REST endpoint.

```json
POST /log-monitor HTTP/1.1
Host: orb.domain1.com
        
{
  "activate": [
    "http://orb.vct:8077/maple2023"
  ]
}
```

### Deactivate monitoring for old log

De-activate monitoring of old log by posting to log-monitor REST endpoint.

POST /log-monitor HTTP/1.1
Host: orb.domain1.com

```json
{
  "deactivate": [
    "http://orb.vct:8077/maple2022"
  ]
}
```

### List active/inactive logs for log monitoring service

**Endpoint:** "/log-monitor?status=active"

Retrieve active log list for log monitoring service.

Parameters:

status:  active or inactive; it defaults to active if status parameter is not provided

**Example**

```
GET /log-monitor?status=active HTTP/1.1
Host: orb.domain1.com
```
Output:

```json
{
  "active": [
    {
      "log_url": "http://orb.vct:8077/maple2022",
      "sth_response": {
        "tree_size": 24,
        "timestamp": 1654869615262,
        "sha256_root_hash": "GDCyCWRPqGPtrgNtj1iFGxwSg0emoxuq/W1Dc4lEiro=",
        "tree_head_signature": "eyJhbGdvcml0aG0iOnsic2lnbmF0dXJlIjoiRUNEU0EiLCJ0eXBlIjoiRUNEU0FQMjU2REVSIn0sInNpZ25hdHVyZSI6Ik1FVUNJUUNXSEl2Z3hUZHdJWjdMdk5HVVcxZitlMW5IQ21Hc0dseGRYV0VlRy9Dckl3SWdOeWlGWTR4VDg3V1JrVkFaTHlXSkFZdjlPU2h5VWZvSU1JelJIWDNBTDRJPSJ9"
      },
      "pub_key": "MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEfCc/5CT+K59Dv7+r+MiVX+ARfMeFK9CwdLlicTyjoNJdhFfP4/wnVfXg+vLjrqBYFsYzgokTSTZBSk72WF1RrQ==",
      "active": true
    },
    {
      "log_url": "http://orb.vct:8077/maple2023",
      "sth_response": {
        "tree_size": 0,
        "timestamp": 1654868145703,
        "sha256_root_hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
        "tree_head_signature": "eyJhbGdvcml0aG0iOnsic2lnbmF0dXJlIjoiRUNEU0EiLCJ0eXBlIjoiRUNEU0FQMjU2REVSIn0sInNpZ25hdHVyZSI6Ik1FWUNJUURCWjJwRFJlNVEzdHVieE1IV2pIUjVwcTZJVnNaT0xsU1BxeUl0VmhrVXFnSWhBUEhxU1hvU3gvTTdvemlMZGdKWlNFeDc1bFZQVDVCQzExRnQ0dkZIZ1dCSCJ9"
      },
      "pub_key": "MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEfCc/5CT+K59Dv7+r+MiVX+ARfMeFK9CwdLlicTyjoNJdhFfP4/wnVfXg+vLjrqBYFsYzgokTSTZBSk72WF1RrQ==",
      "active": true
    }
  ]
}
```


**Example**

```
GET /log-monitor?status=inactive HTTP/1.1
Host: orb.domain1.com
```
Output:

```json
{
  "inactive": [
    {
      "log_url": "http://orb.vct:8077/maple2020",
      "sth_response": {
        "tree_size": 48,
        "timestamp": 1654869871315,
        "sha256_root_hash": "qlODFYrB140S4ZCYY6+ipISvTALA3x2jEs3bpV0UBrI=",
        "tree_head_signature": "eyJhbGdvcml0aG0iOnsic2lnbmF0dXJlIjoiRUNEU0EiLCJ0eXBlIjoiRUNEU0FQMjU2REVSIn0sInNpZ25hdHVyZSI6Ik1FVUNJUURadEJjVVJROUhuZWJ3UnJrTVJsbXZDZm4yT1BpTWNBK250V2JuL05xNkl3SWdkN1FPcGI0WWNMTkU4N1ZxZ1VoWFFxMFM0c0JaZ2tCV2NRMG45NTd4ZUNNPSJ9"
      },
      "pub_key": "MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEfCc/5CT+K59Dv7+r+MiVX+ARfMeFK9CwdLlicTyjoNJdhFfP4/wnVfXg+vLjrqBYFsYzgokTSTZBSk72WF1RrQ==",
      "active": false
    }
  ]
}
```

