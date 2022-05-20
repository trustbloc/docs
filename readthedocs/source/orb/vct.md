# Verifiable Credential Transparency (VCT)

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
