# Introduction

The Verifiable Credential Transparency (VCT) Witness ledger is based on certificate transparency 
[RFC6962](https://www.rfc-editor.org/rfc/rfc6962).
VCT logs are append-only ledgers of anchor credentials.
Because they're distributed and independent, 
anyone can query them to see what Anchor Credentials have been included and when. 
Because they're append-only, they are verifiable by Monitors.
VCT log may be named to enable periodical rollover.


Logs are cryptographically monitored by Orb node.
Monitors cryptographically check that Anchor Credential have been included in VCT log.

Orb specification extended [RFC6962](https://www.rfc-editor.org/rfc/rfc6962) to include [Verifiable Credential (VC)](https://trustbloc.github.io/did-method-orb/#bib-vc-data-model) objects. 

## Add Verifiable Credential Flow

VCT node will validate verifiable credential and add it to Trillian log. 

```{image} ../../_static/orb/add-vc.svg
```

See [add-vc](restendpoints.html#add-anchor-credential-to-log) REST endpoint for more information.

## VCT Monitoring

Monitors cryptographically check if verifiable credentials have been included in logs. They
can also monitor log for consistency. Monitors can be set up and run by anyone. 

VCT node supports the following monitoring API:

### Get Signed Tree Head (STH)

Retrieve latest Signed Tree Head as described in [RFC6962](https://datatracker.ietf.org/doc/html/rfc6962#section-4.3).
See [get-sth](restendpoints.html#get-signed-tree-head-sth) REST endpoint for more information.

### Get Signed Tree Head (STH) Consistency

Retrieve Merkle Consistency Proof between Two Signed Tree Heads
as per [RFC6962](https://datatracker.ietf.org/doc/html/rfc6962#section-4.4).

See [get-sth-consistency](restendpoints.html#get-signed-tree-head-sth-consistency) REST endpoint for more information.

### Get Entries

Retrieve entries from log as per [RFC6962](https://datatracker.ietf.org/doc/html/rfc6962#section-4.6).

See [get-entries](restendpoints.html#get-entries) REST endpoint for more information.

### Retrieve Entry and Proof from Log

Retrieve entry and Merkle audit proof from log as per [RFC6962](https://datatracker.ietf.org/doc/html/rfc6962#section-4.8).

See [get-entry-and-proof](restendpoints.html#retrieve-entry-and-proof-from-log) REST endpoint for more information.


### Retrieve Merkle Audit Proof from Log by Leaf Hash

Retrieve Merkle audit proof by leaf hash from log as per [RFC6962](https://datatracker.ietf.org/doc/html/rfc6962#section-4.5).

See [get-proof-by-hash](restendpoints.html#retrieve-merkle-audit-proof-from-log-by-leaf-hash) REST endpoint for more information.


## VCT Discovery and Administration 

### Webfinger

Retrieve discovery information about VCT log.

See [.well-known/webfinger](restendpoints.html#webfinger) REST endpoint for more information.

### Get Issuers

Retrieves log issuers (if configured). If the log issuer list has been configured for the log
then Verifiable Credential issuer ID has to be present in the log's issuer list
in order to add Verifiable Credential to the log.

See [get-issuers](restendpoints.html#get-issuers) REST endpoint for more information.
