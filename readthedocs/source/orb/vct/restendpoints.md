# REST Endpoints

## Add Anchor Credential to Log

**Endpoint:** "/{alias}/v1/add-vc"

Add Anchor Credential to log.

Parameter: Anchor Credential.

**Example**

```
POST /maple2020/v1/add-vc HTTP/1.1
Host: witness.com
```
Request:

```json
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://w3id.org/security/suites/jws-2020/v1",
    "https://w3id.org/security/suites/ed25519-2020/v1"
  ],
  "credentialSubject": "hl:uEiDKH_3UYP_4_F8BAV7mOPa6IDrTMuMShNNwHVyfam7CBg",
  "id": "https://orb.domain1.com/vc/41ae2e2f-4974-4425-ac0e-a1b14b2cfea3",
  "issuanceDate": "2022-05-19T22:16:19.7471831Z",
  "issuer": "https://orb.domain1.com",
  "type": "VerifiableCredential"
}
```

Output:

sct_version:  The version of the SignedCertificateTimestamp structure.

id:  The log ID, base64 encoded.

timestamp:  The SCT timestamp, in decimal.

extensions:  An opaque type for future expansion.

signature:  The SCT signature, base64 encoded.


```json
{
  "svct_version": 0,
  "id": "c0JZOeGbBoFbJYTJpin68J2IhCHr1muAEi4QCY7cTko=",
  "timestamp": 1652998579753,
  "extensions": "",
  "signature": "eyJhbGdvcml0aG0iOnsic2lnbmF0dXJlIjoiRUNEU0EiLCJ0eXBlIjoiRUNEU0FQMjU2REVSIn0sInNpZ25hdHVyZSI6Ik1FVUNJUURJdFJRZ1ZKZFJTQytIQjN5NFVqb2x5UnNvcW5lR0NHUHFaTXNDdjBOT25BSWdTMjFUZzdaNC93bDZvZm4yZVRmeG9aL1ZZMHhUdHc4R3I1MlVSa0UwVVljPSJ9"
}
```

## Get Signed Tree Head (STH)

**Endpoint:** "/{alias}/v1/get-sth"

Retrieve latest Signed Tree Head as described in [RFC6962](https://datatracker.ietf.org/doc/html/rfc6962#section-4.3)

**Example**

```
GET /maple2020/v1/get-sth HTTP/1.1
Host: witness.com
```

Output:

```json
{
  "tree_size": 24,
  "timestamp": 1652906464409,
  "sha256_root_hash": "E2HaBxp1VbZg1Mx/OSMeDSo/94yKXY1+cD0UzoH1kIk=",
  "tree_head_signature": "eyJhbGdvcml0aG0iOnsic2lnbmF0dXJlIjoiRUNEU0EiLCJ0eXBlIjoiRUNEU0FQMjU2REVSIn0sInNpZ25hdHVyZSI6Ik1FVUNJUURRTk1UQ2tqVWxOZTgwU090bUErRmFaZG5wWGEvR3BQSzVXdWpETmFlc2dnSWdPRDd4Q3VYVWR4OWVOWVlTeTM5K2gzL0htUkE0dEIyVlVHYkZZOUZQT0RVPSJ9"
}
```

## Get Signed Tree Head (STH) Consistency

**Endpoint:** "/{alias}/v1/get-sth-consistency?first=5&second=10

Retrieve Merkle Consistency Proof between Two Signed Tree Heads 
as per [RFC6962](https://datatracker.ietf.org/doc/html/rfc6962#section-4.4).

Parameters:

first:  The tree_size of the first tree

second:  The tree_size of the second tree

Both tree sizes must be from existing v1 STHs (Signed Tree Heads).

**Example**

http://witness.com/maple2020/v1/get-sth-consistency?first=20&second=23

```
GET /maple2020/v1/get-sth-consistency?first=20&second=23 HTTP/1.1
Host: witness.com
```

Output:

consistency:  An array of Merkle Tree nodes, base64 encoded.

```json
{
  "consistency": [
    "a7jghQovOhwnucOdJLbPD/I/OoiY+qprT/mk+LVmeL4=",
    "Ml0LmRpcONX42qwxHAb/qw3rFrhqmEH/Bdw5rMGMedw=",
    "M/Qu3X7+iS23AWUrgU+plLBRUi3WDHcodMV2+oUcQbM="
  ]
}
```

## Get Entries

**Endpoint:** "/{alias}/v1/get-entries?start=0&end=3"

Retrieve entries from log as per [RFC6962](https://datatracker.ietf.org/doc/html/rfc6962#section-4.6).

Parameters:

start:  0-based index of first entry to retrieve

end:  0-based index of last entry to retrieve

**Example**

```
GET /maple2020/v1/get-entries?start=0&end=3 HTTP/1.1
Host: witness.com
```

Output:

entries:  An array of objects, each consisting of

leaf_input:  The base64-encoded MerkleTreeLeaf structure.

extra_data:  The base64-encoded unsigned data pertaining to the log entry.

```json
{
  "entries": [
    {
      "leaf_input": "eyJ2ZXJzaW9uIjowLCJsZWFmX3R5cGUiOjEwMCwidGltZXN0YW1wZWRfZW50cnkiOnsidGltZXN0YW1wIjoxNjUyOTA2MzY1NTYzLCJlbnRyeV90eXBlIjoxMDAsInZjX2VudHJ5IjoiZXlKQVkyOXVkR1Y0ZENJNld5Sm9kSFJ3Y3pvdkwzZDNkeTUzTXk1dmNtY3ZNakF4T0M5amNtVmtaVzUwYVdGc2N5OTJNU0lzSW1oMGRIQnpPaTh2ZHpOcFpDNXZjbWN2YzJWamRYSnBkSGt2YzNWcGRHVnpMMnAzY3kweU1ESXdMM1l4SWl3aWFIUjBjSE02THk5M00ybGtMbTl5Wnk5elpXTjFjbWwwZVM5emRXbDBaWE12WldReU5UVXhPUzB5TURJd0wzWXhJbDBzSW1OeVpXUmxiblJwWVd4VGRXSnFaV04wSWpvaWFHdzZkVVZwUkd4cFZuZHVOVTFWTW1oSlZUTjBSbmRWTUZaVFZrcEhNVzl5Wm1wUFdWQTNPSFZoZG1vME1GSTBOVUVpTENKcFpDSTZJbWgwZEhCek9pOHZiM0ppTG1SdmJXRnBiakV1WTI5dEwzWmpMMlJrT1RVMU16RXlMV1kyTnprdE5EYzFZaTFpTlRrM0xUVmlNMkV5WldZNE5EUmlaaUlzSW1semMzVmhibU5sUkdGMFpTSTZJakl3TWpJdE1EVXRNVGhVTWpBNk16azZNalV1TlRVeE5EYzBORm9pTENKcGMzTjFaWElpT2lKb2RIUndjem92TDI5eVlpNWtiMjFoYVc0eExtTnZiU0lzSW5SNWNHVWlPaUpXWlhKcFptbGhZbXhsUTNKbFpHVnVkR2xoYkNKOSIsImV4dGVuc2lvbnMiOm51bGx9fQ==",
      "extra_data": "bnVsbA=="
    },
    {
      "leaf_input": "eyJ2ZXJzaW9uIjowLCJsZWFmX3R5cGUiOjEwMCwidGltZXN0YW1wZWRfZW50cnkiOnsidGltZXN0YW1wIjoxNjUyOTA2MzcxNTQxLCJlbnRyeV90eXBlIjoxMDAsInZjX2VudHJ5IjoiZXlKQVkyOXVkR1Y0ZENJNld5Sm9kSFJ3Y3pvdkwzZDNkeTUzTXk1dmNtY3ZNakF4T0M5amNtVmtaVzUwYVdGc2N5OTJNU0lzSW1oMGRIQnpPaTh2ZHpOcFpDNXZjbWN2YzJWamRYSnBkSGt2YzNWcGRHVnpMMnAzY3kweU1ESXdMM1l4SWl3aWFIUjBjSE02THk5M00ybGtMbTl5Wnk5elpXTjFjbWwwZVM5emRXbDBaWE12WldReU5UVXhPUzB5TURJd0wzWXhJbDBzSW1OeVpXUmxiblJwWVd4VGRXSnFaV04wSWpvaWFHdzZkVVZwUTFSa1FXOVVUR1pmZGxwemVFUnhNblV0ZVcxdlRsOWZOMWxUWldKVGFHOXVSMGhNTkRSaGJsSm9Ua0VpTENKcFpDSTZJbWgwZEhCek9pOHZiM0ppTG1SdmJXRnBiakV1WTI5dEwzWmpMMkZsTkRGbE1URTRMVE5oTWpVdE5HVXhOQzA1TW1VMExUQmxaVGc1TldWak1UTTFaaUlzSW1semMzVmhibU5sUkdGMFpTSTZJakl3TWpJdE1EVXRNVGhVTWpBNk16azZNekV1TlRNMk16TTVORm9pTENKcGMzTjFaWElpT2lKb2RIUndjem92TDI5eVlpNWtiMjFoYVc0eExtTnZiU0lzSW5SNWNHVWlPaUpXWlhKcFptbGhZbXhsUTNKbFpHVnVkR2xoYkNKOSIsImV4dGVuc2lvbnMiOm51bGx9fQ==",
      "extra_data": "bnVsbA=="
    },
    {
      "leaf_input": "eyJ2ZXJzaW9uIjowLCJsZWFmX3R5cGUiOjEwMCwidGltZXN0YW1wZWRfZW50cnkiOnsidGltZXN0YW1wIjoxNjUyOTA2MzczNDc1LCJlbnRyeV90eXBlIjoxMDAsInZjX2VudHJ5IjoiZXlKQVkyOXVkR1Y0ZENJNld5Sm9kSFJ3Y3pvdkwzZDNkeTUzTXk1dmNtY3ZNakF4T0M5amNtVmtaVzUwYVdGc2N5OTJNU0lzSW1oMGRIQnpPaTh2ZHpOcFpDNXZjbWN2YzJWamRYSnBkSGt2YzNWcGRHVnpMMnAzY3kweU1ESXdMM1l4SWl3aWFIUjBjSE02THk5M00ybGtMbTl5Wnk5elpXTjFjbWwwZVM5emRXbDBaWE12WldReU5UVXhPUzB5TURJd0wzWXhJbDBzSW1OeVpXUmxiblJwWVd4VGRXSnFaV04wSWpvaWFHdzZkVVZwUW5wUmNHUkZaMHBLYmxjMFZXaFphMHRJVm5aNU9IVnZlSHBvV2xjM1gwWXlMVXhCZVdkbmQyd3dXV2NpTENKcFpDSTZJbWgwZEhCek9pOHZiM0ppTG1SdmJXRnBiakV1WTI5dEwzWmpMMlJtWXpBNU9EQTBMV1UyWVRNdE5EY3paUzFpTkRsaExUbGhZamN4TUdOaU9EUmpZU0lzSW1semMzVmhibU5sUkdGMFpTSTZJakl3TWpJdE1EVXRNVGhVTWpBNk16azZNek11TkRjeE5Ea3dPVm9pTENKcGMzTjFaWElpT2lKb2RIUndjem92TDI5eVlpNWtiMjFoYVc0eExtTnZiU0lzSW5SNWNHVWlPaUpXWlhKcFptbGhZbXhsUTNKbFpHVnVkR2xoYkNKOSIsImV4dGVuc2lvbnMiOm51bGx9fQ==",
      "extra_data": "bnVsbA=="
    },
    {
      "leaf_input": "eyJ2ZXJzaW9uIjowLCJsZWFmX3R5cGUiOjEwMCwidGltZXN0YW1wZWRfZW50cnkiOnsidGltZXN0YW1wIjoxNjUyOTA2MzgwNTA3LCJlbnRyeV90eXBlIjoxMDAsInZjX2VudHJ5IjoiZXlKQVkyOXVkR1Y0ZENJNld5Sm9kSFJ3Y3pvdkwzZDNkeTUzTXk1dmNtY3ZNakF4T0M5amNtVmtaVzUwYVdGc2N5OTJNU0lzSW1oMGRIQnpPaTh2ZHpOcFpDNXZjbWN2YzJWamRYSnBkSGt2YzNWcGRHVnpMMnAzY3kweU1ESXdMM1l4SWl3aWFIUjBjSE02THk5M00ybGtMbTl5Wnk5elpXTjFjbWwwZVM5emRXbDBaWE12WldReU5UVXhPUzB5TURJd0wzWXhJbDBzSW1OeVpXUmxiblJwWVd4VGRXSnFaV04wSWpvaWFHdzZkVVZwUkV0M09FVnVkbWhrVTNwZllVZDJhWE5FTURGNWVVZHJjV2RDVHpSeWFIVkhWRjh6Tm5OeFdqTTNTMUVpTENKcFpDSTZJbWgwZEhCek9pOHZiM0ppTG1SdmJXRnBiakV1WTI5dEwzWmpMMlpsT0RrNE0ySmtMV1F4TldVdE5HSmlNaTA0WldRNUxXSmlNR1V3WkRVMVpXRXdPQ0lzSW1semMzVmhibU5sUkdGMFpTSTZJakl3TWpJdE1EVXRNVGhVTWpBNk16azZOREF1TlRBeU5qVTRXaUlzSW1semMzVmxjaUk2SW1oMGRIQnpPaTh2YjNKaUxtUnZiV0ZwYmpFdVkyOXRJaXdpZEhsd1pTSTZJbFpsY21sbWFXRmliR1ZEY21Wa1pXNTBhV0ZzSW4wPSIsImV4dGVuc2lvbnMiOm51bGx9fQ==",
      "extra_data": "bnVsbA=="
    }
  ]
}
```

## Retrieve Entry and Proof from Log

**Endpoint:** "/{alias}/v1/get-entry-and-proof?leaf_index=1&tree_size=23"

Retrieve entry and Merkle audit proof from log as per [RFC6962](https://datatracker.ietf.org/doc/html/rfc6962#section-4.8).

Parameters:

leaf_index:  The index of the desired entry.

tree_size:  The tree_size of the tree for which the proof is desired.

**Example**

```
GET /maple2020/v1/get-entry-and-proof?leaf_index=1&tree_size=23 HTTP/1.1
Host: witness.com
```
Output:

leaf_input:  The base64-encoded MerkleTreeLeaf structure.

extra_data:  The base64-encoded unsigned data pertaining to the log entry.

audit_path:  An array of base64-encoded Merkle Tree nodes proving the inclusion of entry.

```json
{
  "leaf_input": "eyJ2ZXJzaW9uIjowLCJsZWFmX3R5cGUiOjEwMCwidGltZXN0YW1wZWRfZW50cnkiOnsidGltZXN0YW1wIjoxNjUyOTA2MzcxNTQxLCJlbnRyeV90eXBlIjoxMDAsInZjX2VudHJ5IjoiZXlKQVkyOXVkR1Y0ZENJNld5Sm9kSFJ3Y3pvdkwzZDNkeTUzTXk1dmNtY3ZNakF4T0M5amNtVmtaVzUwYVdGc2N5OTJNU0lzSW1oMGRIQnpPaTh2ZHpOcFpDNXZjbWN2YzJWamRYSnBkSGt2YzNWcGRHVnpMMnAzY3kweU1ESXdMM1l4SWl3aWFIUjBjSE02THk5M00ybGtMbTl5Wnk5elpXTjFjbWwwZVM5emRXbDBaWE12WldReU5UVXhPUzB5TURJd0wzWXhJbDBzSW1OeVpXUmxiblJwWVd4VGRXSnFaV04wSWpvaWFHdzZkVVZwUTFSa1FXOVVUR1pmZGxwemVFUnhNblV0ZVcxdlRsOWZOMWxUWldKVGFHOXVSMGhNTkRSaGJsSm9Ua0VpTENKcFpDSTZJbWgwZEhCek9pOHZiM0ppTG1SdmJXRnBiakV1WTI5dEwzWmpMMkZsTkRGbE1URTRMVE5oTWpVdE5HVXhOQzA1TW1VMExUQmxaVGc1TldWak1UTTFaaUlzSW1semMzVmhibU5sUkdGMFpTSTZJakl3TWpJdE1EVXRNVGhVTWpBNk16azZNekV1TlRNMk16TTVORm9pTENKcGMzTjFaWElpT2lKb2RIUndjem92TDI5eVlpNWtiMjFoYVc0eExtTnZiU0lzSW5SNWNHVWlPaUpXWlhKcFptbGhZbXhsUTNKbFpHVnVkR2xoYkNKOSIsImV4dGVuc2lvbnMiOm51bGx9fQ==",
  "extra_data": "bnVsbA==",
  "audit_path": [
    "40IbH54WlW/7XICpmttJmOPv4/DaRsCZDc4osAdzPxg=",
    "aaNL9DnYEGLdCjbCV72u6vZ/yBj/Mo0b5Wt7h5zdKMI=",
    "B4KHZzvpTXVOUtyI2skBE6SO8CZFcZl0hDAybAOhV/M=",
    "3yME5CgWt6xUnb3jlz10puvcZT4vRsQcxp9+dceMdrg=",
    "Verp7o4zRhZPJvIxEzCzPNPtw2ZKYj9HCYKtHpDY6RY="
  ]
}
```

Decoded leaf_input:

```json
{
  "version": 0,
  "leaf_type": 100,
  "timestamped_entry": {
    "timestamp": 1652906371541,
    "entry_type": 100,
    "vc_entry": "eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvMjAxOC9jcmVkZW50aWFscy92MSIsImh0dHBzOi8vdzNpZC5vcmcvc2VjdXJpdHkvc3VpdGVzL2p3cy0yMDIwL3YxIiwiaHR0cHM6Ly93M2lkLm9yZy9zZWN1cml0eS9zdWl0ZXMvZWQyNTUxOS0yMDIwL3YxIl0sImNyZWRlbnRpYWxTdWJqZWN0IjoiaGw6dUVpQ1RkQW9UTGZfdlpzeERxMnUteW1vTl9fN1lTZWJTaG9uR0hMNDRhblJoTkEiLCJpZCI6Imh0dHBzOi8vb3JiLmRvbWFpbjEuY29tL3ZjL2FlNDFlMTE4LTNhMjUtNGUxNC05MmU0LTBlZTg5NWVjMTM1ZiIsImlzc3VhbmNlRGF0ZSI6IjIwMjItMDUtMThUMjA6Mzk6MzEuNTM2MzM5NFoiLCJpc3N1ZXIiOiJodHRwczovL29yYi5kb21haW4xLmNvbSIsInR5cGUiOiJWZXJpZmlhYmxlQ3JlZGVudGlhbCJ9",
    "extensions": null
  }
}
```

## Retrieve Merkle Audit Proof from Log by Leaf Hash

**Endpoint:** "/{alias}/v1/get-proof-by-hash?tree_size=24&hash=bsmXlruDXTFrWOfFQ2WRBERc51rMk3hvRdOAROIZmHI="

Retrieve Merkle audit proof by leaf hash from log as per [RFC6962](https://datatracker.ietf.org/doc/html/rfc6962#section-4.5).

Parameters:

hash:  A base64-encoded v1 leaf hash.

tree_size:  The tree_size of the tree on which to base the proof.

**Example**

```
GET /maple2020/v1/get-proof-by-hash?tree_size=24&hash=bsmXlruDXTFrWOfFQ2WRBERc51rMk3hvRdOAROIZmHI= HTTP/1.1
Host: witness.com
```
Output:

leaf_index:  The 0-based index of the end entity corresponding to the "hash" parameter.

audit_path:  An array of base64-encoded Merkle Tree nodes proving the inclusion of the entry.

```json
{
  "leaf_index": 2,
  "audit_path": [
    "6pKexnNWd+pRegRB+0kUct/yIlPhEP8F4YASINoP3PI=",
    "KB/U4HPHLwE36ZZCcWhBhk9M24242oDPe3kJxAUkfu0=",
    "m+GAt5OxvxxX5kNBDqqHkoHJHeJxcitAzc0wJMbIlQs=",
    "GkRLjI7Sb6pkqjU63baSCyoAiqRYrBbY7Efz+cjlXY4=",
    "XuYjKQt/oygHn96E4tSJrGsT4ZqLAvlsui9TS4+6f3g="
  ]
}
```

## VCT Discovery and Administration Endpoints

### WebFinger

**Endpoint:** "/.well-known/webfinger?resource={log-id}"

Retrieve discovery information about VCT log.

**Example**

```
GET /.well-known/webfinger?resource=https://witness.com/maple2020 HTTP/1.1
Host: witness.com
```
Output:

```json
{
  "subject": "https://witness.com/maple2020",
  "properties": {
    "https://trustbloc.dev/ns/ledger-type": "vct-v1",
    "https://trustbloc.dev/ns/public-key": "MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEfCc/5CT+K59Dv7+r+MiVX+ARfMeFK9CwdLlicTyjoNJdhFfP4/wnVfXg+vLjrqBYFsYzgokTSTZBSk72WF1RrQ=="
  },
  "links": [
    {
      "rel": "self",
      "href": "https://witness.com/maple2020"
    }
  ]
}
```

### Get Issuers

**Endpoint:** "/{alias}/get-issuers"

Retrieves log issuers (if configured). If the log issuer list has been configured 
then Anchor Credential issuer ID has to be present in the log's issuer list 
in order to add Anchor Credential to the log.

**Example**

```
GET /maple2020/v1/get-issuers HTTP/1.1
Host: witness.com
```

Output:

```json
["did:key:zUC724vuGvHpnCGFG1qqpXb81SiBLu3KLSqVzenwEZNPoY35i2Bscb8DLaVwHvRFs6F2NkNNXRcPWvqnPDUd9ukdjLkjZd3u9zzL4wDZDUpkPAatLDGLEYVo8kkAzuAKJQMr7N7"]
```

### Health Check

**Endpoint:** "/healthcheck"

Returns VCT status.

**Example**

```
GET /healthcheck HTTP/1.1
Host: witness.com
```
Output:

```json
{
  "current_time": "2022-05-19T17:56:54.0594632Z",
  "status": "success"
}
```

### Metrics

**Endpoint:** "/metrics"

Returns VCT metrics.

**Example**

```
GET /metrics HTTP/1.1
Host: witness.com
```
