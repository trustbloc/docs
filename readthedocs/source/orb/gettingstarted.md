# Getting Started Tutorial

In this tutorial you'll start up two stand-alone Orb nodes (with in-memory database and message queue)
and create, update and resolve DIDs. Further in the tutorial you'll start up the Orb nodes along with a
[VCT](vct/introduction.html#verifiable-credential-transparency-vct) node, and you'll verify that the proofs
in an anchor linkset are in the VCT log.

## Setup

Install Docker Desktop from [here](https://docs.docker.com/get-docker/).

Clone the orb project:

```commandline
git clone git@github.com:trustbloc/orb.git
```

Build Orb and the [CLI](client/cli.html#cli):

```commandline
cd orb

make clean orb-docker build-orb-cli-binaries extract-orb-cli-binaries
```

## Orb with no VCT

Start two Orb instances (orb1.local and orb2.local) and a command-line container:

```commandline
cd orb/samples/tutorial

docker-compose -f docker-compose-cli.yml -f ../docker/docker-compose-dev.yml up&
```

In another terminal, open an interactive Docker shell:

```commandline
docker exec -ti cli /bin/bash

cd ./orb
```

Query non-existing DID:

```commandline
orb-cli did resolve --did-uri=did:orb:http:orb1.local:uAAA:EiBFejklGvpC6hn--gZEoiDnEaeineV8xP7p0AcH1-N33A --verify-resolution-result-type=all
```

Response:

```commandline
Error: failed to resolve did: failed to resolve did: DID does not exist
 [orb-cli] 2022/05/24 16:06:31 UTC - main.main -> CRITICAL Failed to run orb-cli: failed to resolve did: failed to resolve did: DID does not exist
```

Create a DID at orb1:

```commandline
orb-cli did create --domain=http://orb1.local --publickey-file=./create_publickeys.json --service-file=./create_services.json --recoverykey-file=./recover_publickey.pem --updatekey-file=./update_publickey.pem --did-anchor-origin=http://orb1.local | jq
```

Response:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/ed25519-2018/v1",
    "https://w3id.org/security/suites/jws-2020/v1"
  ],
  "id": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA",
  "verificationMethod": [
    {
      "controller": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA",
      "id": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#key1",
      "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
      "type": "Ed25519VerificationKey2018"
    },
    {
      "controller": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA",
      "id": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#key2",
      "publicKeyJwk": {
        "kty": "EC",
        "crv": "P-256",
        "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
        "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
      },
      "type": "JsonWebKey2020"
    }
  ],
  "service": [
    {
      "id": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#svc1",
      "priority": 1,
      "recipientKeys": [
        "key1"
      ],
      "serviceEndpoint": [
        {
          "routingKeys": [
            "key1"
          ],
          "uri": "https://example.com"
        }
      ],
      "type": "type1"
    },
    {
      "id": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#svc2",
      "priority": 2,
      "recipientKeys": [
        "key2"
      ],
      "serviceEndpoint": [
        {
          "routingKeys": [
            "key2"
          ],
          "uri": "https://example.com"
        }
      ],
      "type": "type2"
    }
  ],
  "authentication": [
    "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#key1",
    "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#key2"
  ]
}
```

Resolve the un-anchored DID at orb1:

**_NOTE:_**  Set DID_SUFFIX to the DID suffix in the _id_ field of the _did create_ response. For example,
did:orb:uAAA:<mark>EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w</mark>.

```commandline
export DID_SUFFIX=EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w

orb-cli did resolve --domain=http://orb1.local --did-uri=did:orb:uAAA:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA",
    "verificationMethod": [
      {
        "controller": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA",
        "id": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#key1",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      },
      {
        "controller": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA",
        "id": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#key2",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      }
    ],
    "service": [
      {
        "id": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#svc1",
        "priority": 1,
        "recipientKeys": [
          "key1"
        ],
        "serviceEndpoint": [
          {
            "routingKeys": [
              "key1"
            ],
            "uri": "https://example.com"
          }
        ],
        "type": "type1"
      },
      {
        "id": "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#svc2",
        "priority": 2,
        "recipientKeys": [
          "key2"
        ],
        "serviceEndpoint": [
          {
            "routingKeys": [
              "key2"
            ],
            "uri": "https://example.com"
          }
        ],
        "type": "type2"
      }
    ],
    "authentication": [
      "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#key1",
      "did:orb:uAAA:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#key2"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g",
    "canonicalId": "did:orb:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA",
    "equivalentId": [
      "did:orb:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA",
      "did:orb:hl:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQm1vaDg2ZlFwOEZXT05NdmpSYXNyOGxpQzU1NE4tbzJfMGVsdGxkUFl5NGc:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA"
    ],
    "method": {
      "updateCommitment": "EiDsq7uyQDTYJss8JAV68EcVb1sJHXgUl8knESbzvN4RWw",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMxIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MSJdLCJzZXJ2aWNlRW5kcG9pbnQiOlt7InJvdXRpbmdLZXlzIjpbImtleTEiXSwidXJpIjoiaHR0cHM6Ly9leGFtcGxlLmNvbSJ9XSwidHlwZSI6InR5cGUxIn0seyJpZCI6InN2YzIiLCJwcmlvcml0eSI6MiwicmVjaXBpZW50S2V5cyI6WyJrZXkyIl0sInNlcnZpY2VFbmRwb2ludCI6W3sicm91dGluZ0tleXMiOlsia2V5MiJdLCJ1cmkiOiJodHRwczovL2V4YW1wbGUuY29tIn1dLCJ0eXBlIjoidHlwZTIifV19LHsiYWN0aW9uIjoiYWRkLXB1YmxpYy1rZXlzIiwicHVibGljS2V5cyI6W3siaWQiOiJrZXkxIiwicHVibGljS2V5SndrIjp7ImNydiI6IkVkMjU1MTkiLCJrdHkiOiJPS1AiLCJ4IjoibzFiRzFVN0czQ05idEFMTWFmVWlGT3E4T0RyYVR5VlRtUHRSRE8xUVVXZyJ9LCJwdXJwb3NlcyI6WyJhdXRoZW50aWNhdGlvbiJdLCJ0eXBlIjoiRWQyNTUxOVZlcmlmaWNhdGlvbktleTIwMTgifSx7ImlkIjoia2V5MiIsInB1YmxpY0tleUp3ayI6eyJjcnYiOiJQLTI1NiIsImt0eSI6IkVDIiwieCI6ImJHTTlhTnVmcEtOUHhsa3lhY1UxaEdoUVhtX2FDOGhJelNWZUtEcHdqQnciLCJ5IjoiUGZkbUNPdElkVlkyQjZ1Y1I0b1FrdDZldlFkZFloT3lIb0RZQ2FJMkJKQSJ9LCJwdXJwb3NlcyI6WyJhdXRoZW50aWNhdGlvbiJdLCJ0eXBlIjoiSnNvbldlYktleTIwMjAifV19XSwidXBkYXRlQ29tbWl0bWVudCI6IkVpRHNxN3V5UURUWUpzczhKQVY2OEVjVmIxc0pIWGdVbDhrbkVTYnp2TjRSV3cifSwic3VmZml4RGF0YSI6eyJhbmNob3JPcmlnaW4iOiJodHRwOi8vb3JiMS5sb2NhbCIsImRlbHRhSGFzaCI6IkVpQzYyTkZQNTJJeXBpbGFnQ3FtdTJaQnRUd1ZBOGdYM003cGlQeW9UcURNOXciLCJyZWNvdmVyeUNvbW1pdG1lbnQiOiJFaUFWdGpYeC1XSFlmV2NBenpnYkhkY0FEaEYxS1ZTenB5NVRiX3hudTFjYnpBIn0sInR5cGUiOiJjcmVhdGUifQ==",
          "transactionTime": 1655130057,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g",
          "equivalentReferences": [
            "hl:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQm1vaDg2ZlFwOEZXT05NdmpSYXNyOGxpQzU1NE4tbzJfMGVsdGxkUFl5NGc"
          ]
        }
      ]
    }
  }
}
```

Get the hash of the anchor from the _resolve_ response (from the _canonicalId_ field in the metadata) and set the ANCHOR_HASH environment variable.
For example did:orb:<mark>uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ</mark>:EiDEfq8bA3CqrN3_s76O5uPhm3cGnV3D7oNloVvHfHTg3w:

```commandline
export ANCHOR_HASH=uEiB_V_lRcrutfhF2LvEIvRqsR2uO4j_1nbx6T6X-pUZQPQ
```

Resolve the canonical DID at orb1:

```commandline
orb-cli did resolve --domain=http://orb1.local --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:orb:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA",
    "verificationMethod": [
      {
        "controller": "did:orb:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA",
        "id": "did:orb:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#key1",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      },
      {
        "controller": "did:orb:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA",
        "id": "did:orb:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#key2",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      }
    ],
    "service": [
      {
        "id": "did:orb:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#svc1",
        "priority": 1,
        "recipientKeys": [
          "key1"
        ],
        "serviceEndpoint": [
          {
            "routingKeys": [
              "key1"
            ],
            "uri": "https://example.com"
          }
        ],
        "type": "type1"
      },
      {
        "id": "did:orb:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#svc2",
        "priority": 2,
        "recipientKeys": [
          "key2"
        ],
        "serviceEndpoint": [
          {
            "routingKeys": [
              "key2"
            ],
            "uri": "https://example.com"
          }
        ],
        "type": "type2"
      }
    ],
    "authentication": [
      "did:orb:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#key1",
      "did:orb:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA#key2"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g",
    "canonicalId": "did:orb:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA",
    "equivalentId": [
      "did:orb:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA",
      "did:orb:hl:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQm1vaDg2ZlFwOEZXT05NdmpSYXNyOGxpQzU1NE4tbzJfMGVsdGxkUFl5NGc:EiBqyfvEMLPdGl7tADKOMV6uP2ub3PW0RemzFrMAulDxWA"
    ],
    "method": {
      "updateCommitment": "EiDsq7uyQDTYJss8JAV68EcVb1sJHXgUl8knESbzvN4RWw",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMxIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MSJdLCJzZXJ2aWNlRW5kcG9pbnQiOlt7InJvdXRpbmdLZXlzIjpbImtleTEiXSwidXJpIjoiaHR0cHM6Ly9leGFtcGxlLmNvbSJ9XSwidHlwZSI6InR5cGUxIn0seyJpZCI6InN2YzIiLCJwcmlvcml0eSI6MiwicmVjaXBpZW50S2V5cyI6WyJrZXkyIl0sInNlcnZpY2VFbmRwb2ludCI6W3sicm91dGluZ0tleXMiOlsia2V5MiJdLCJ1cmkiOiJodHRwczovL2V4YW1wbGUuY29tIn1dLCJ0eXBlIjoidHlwZTIifV19LHsiYWN0aW9uIjoiYWRkLXB1YmxpYy1rZXlzIiwicHVibGljS2V5cyI6W3siaWQiOiJrZXkxIiwicHVibGljS2V5SndrIjp7ImNydiI6IkVkMjU1MTkiLCJrdHkiOiJPS1AiLCJ4IjoibzFiRzFVN0czQ05idEFMTWFmVWlGT3E4T0RyYVR5VlRtUHRSRE8xUVVXZyJ9LCJwdXJwb3NlcyI6WyJhdXRoZW50aWNhdGlvbiJdLCJ0eXBlIjoiRWQyNTUxOVZlcmlmaWNhdGlvbktleTIwMTgifSx7ImlkIjoia2V5MiIsInB1YmxpY0tleUp3ayI6eyJjcnYiOiJQLTI1NiIsImt0eSI6IkVDIiwieCI6ImJHTTlhTnVmcEtOUHhsa3lhY1UxaEdoUVhtX2FDOGhJelNWZUtEcHdqQnciLCJ5IjoiUGZkbUNPdElkVlkyQjZ1Y1I0b1FrdDZldlFkZFloT3lIb0RZQ2FJMkJKQSJ9LCJwdXJwb3NlcyI6WyJhdXRoZW50aWNhdGlvbiJdLCJ0eXBlIjoiSnNvbldlYktleTIwMjAifV19XSwidXBkYXRlQ29tbWl0bWVudCI6IkVpRHNxN3V5UURUWUpzczhKQVY2OEVjVmIxc0pIWGdVbDhrbkVTYnp2TjRSV3cifSwic3VmZml4RGF0YSI6eyJhbmNob3JPcmlnaW4iOiJodHRwOi8vb3JiMS5sb2NhbCIsImRlbHRhSGFzaCI6IkVpQzYyTkZQNTJJeXBpbGFnQ3FtdTJaQnRUd1ZBOGdYM003cGlQeW9UcURNOXciLCJyZWNvdmVyeUNvbW1pdG1lbnQiOiJFaUFWdGpYeC1XSFlmV2NBenpnYkhkY0FEaEYxS1ZTenB5NVRiX3hudTFjYnpBIn0sInR5cGUiOiJjcmVhdGUifQ==",
          "transactionTime": 1655130057,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g",
          "equivalentReferences": [
            "hl:uEiBmoh86fQp8FWONMvjRasr8liC554N-o2_0eltldPYy4g:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQm1vaDg2ZlFwOEZXT05NdmpSYXNyOGxpQzU1NE4tbzJfMGVsdGxkUFl5NGc"
          ]
        }
      ]
    }
  }
}
```

Resolve the DID at orb2. These should return 'not found' since orb2 is not following orb1:

```commandline
orb-cli did resolve --sidetree-url-resolution=http://orb2.local/sidetree/v1/identifiers --did-uri=did:orb:uAAA:${DID_SUFFIX} --verify-resolution-result-type=all
orb-cli did resolve --sidetree-url-resolution=http://orb2.local/sidetree/v1/identifiers --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX} --verify-resolution-result-type=all
```

Response:

```commandline
Error: failed to resolve did: failed to resolve did: DID does not exist
 [orb-cli] 2022/05/24 16:39:50 UTC - main.main -> CRITICAL Failed to run orb-cli: failed to resolve did: failed to resolve did: DID does not exist
```

Resolve a discoverable DID at orb2. This should return 'not found' but when we wait a while it should be
available at orb2 since replication should have been triggered:

```commandline
orb-cli did resolve --sidetree-url-resolution=http://orb2.local/sidetree/v1/identifiers --did-uri=did:orb:http:orb1.local:${ANCHOR_HASH}:${DID_SUFFIX}  --verify-resolution-result-type=all
```

Wait a few seconds and resolve the same DID at orb2. This should return the DID document:

```commandline
orb-cli did resolve --sidetree-url-resolution=http://orb2.local/sidetree/v1/identifiers --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX}  --verify-resolution-result-type=all | jq
```

Have orb2 be a follower of orb1:

```commandline
orb-cli follower --outbox-url=http://orb2.local/services/orb/outbox --actor=http://orb2.local/services/orb --to http://orb1.local/services/orb --action=Follow
```

Response:

```commandline
success Follow id: "http://orb2.local/services/orb/activities/17a3f004-305a-44b2-84c6-7f5312ab007f"
```

Query the servers that orb2 is following:

```commandline
curl -s "http://orb2.local/services/orb/following?page=true&page-num=0" | jq
```

Response:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "http://orb2.local/services/orb/following?page=true&page-num=0",
  "items": [
    "http://orb1.local/services/orb"
  ],
  "totalItems": 1,
  "type": "CollectionPage"
}
```

Query the servers that are following orb1:

```commandline
curl -s "http://orb1.local/services/orb/followers?page=true&page-num=0" | jq
```

Response:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "http://orb1.local/services/orb/followers?page=true&page-num=0",
  "items": [
    "http://orb2.local/services/orb"
  ],
  "totalItems": 1,
  "type": "CollectionPage"
}
```

Create another DID at orb1:

```commandline
orb-cli did create --domain=http://orb1.local --publickey-file=./create_publickeys.json --service-file=./create_services2.json --recoverykey-file=./recover_publickey.pem --updatekey-file=./update_publickey.pem --did-anchor-origin=http://orb1.local | jq
```

Response:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/ed25519-2018/v1",
    "https://w3id.org/security/suites/jws-2020/v1"
  ],
  "id": "did:orb:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
  "verificationMethod": [
    {
      "controller": "did:orb:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
      "id": "did:orb:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key1",
      "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
      "type": "Ed25519VerificationKey2018"
    },
    {
      "controller": "did:orb:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
      "id": "did:orb:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key2",
      "publicKeyJwk": {
        "kty": "EC",
        "crv": "P-256",
        "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
        "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
      },
      "type": "JsonWebKey2020"
    }
  ],
  "service": [
    {
      "id": "did:orb:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#svc3",
      "priority": 1,
      "recipientKeys": [
        "key3"
      ],
      "serviceEndpoint": [
        {
          "routingKeys": [
            "key3"
          ],
          "uri": "https://example.com"
        }
      ],
      "type": "type3"
    }
  ],
  "authentication": [
    "did:orb:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key1",
    "did:orb:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key2"
  ]
}
```

**_NOTE:_**  Set DID_SUFFIX to the DID suffix in the _id_ field of the _did create_ response. For example,
did:orb:uAAA:<mark>EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg</mark>.

```commandline
export DID_SUFFIX=EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJg

orb-cli did resolve --did-uri=did:orb:http:orb1.local:uAAA:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:orb:http:orb1.local:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
    "verificationMethod": [
      {
        "controller": "did:orb:http:orb1.local:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
        "id": "did:orb:http:orb1.local:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key1",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      },
      {
        "controller": "did:orb:http:orb1.local:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
        "id": "did:orb:http:orb1.local:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key2",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      }
    ],
    "service": [
      {
        "id": "did:orb:http:orb1.local:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#svc3",
        "priority": 1,
        "recipientKeys": [
          "key3"
        ],
        "serviceEndpoint": [
          {
            "routingKeys": [
              "key3"
            ],
            "uri": "https://example.com"
          }
        ],
        "type": "type3"
      }
    ],
    "authentication": [
      "did:orb:http:orb1.local:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key1",
      "did:orb:http:orb1.local:uAAA:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key2"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg",
    "canonicalId": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
    "equivalentId": [
      "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
      "did:orb:hl:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQm1sUTg5LThuamtFc1BBN3FDRFZRZjJqbWFxSXFXcjg2X1RKek5nRHo0aGc:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ"
    ],
    "method": {
      "updateCommitment": "EiDsq7uyQDTYJss8JAV68EcVb1sJHXgUl8knESbzvN4RWw",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImtleTEiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiRWQyNTUxOSIsImt0eSI6Ik9LUCIsIngiOiJvMWJHMVU3RzNDTmJ0QUxNYWZVaUZPcThPRHJhVHlWVG1QdFJETzFRVVdnIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9LHsiaWQiOiJrZXkyIiwicHVibGljS2V5SndrIjp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiYkdNOWFOdWZwS05QeGxreWFjVTFoR2hRWG1fYUM4aEl6U1ZlS0Rwd2pCdyIsInkiOiJQZmRtQ090SWRWWTJCNnVjUjRvUWt0NmV2UWRkWWhPeUhvRFlDYUkyQkpBIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJKc29uV2ViS2V5MjAyMCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMzIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MyJdLCJzZXJ2aWNlRW5kcG9pbnQiOlt7InJvdXRpbmdLZXlzIjpbImtleTMiXSwidXJpIjoiaHR0cHM6Ly9leGFtcGxlLmNvbSJ9XSwidHlwZSI6InR5cGUzIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaURzcTd1eVFEVFlKc3M4SkFWNjhFY1ZiMXNKSFhnVWw4a25FU2J6dk40Uld3In0sInN1ZmZpeERhdGEiOnsiYW5jaG9yT3JpZ2luIjoiaHR0cDovL29yYjEubG9jYWwiLCJkZWx0YUhhc2giOiJFaURGMFBmWHFGY3NrTU9ySlhtUDItWGRxQ1NwbmJIMDZIaC1jQ0dzLUx4OGd3IiwicmVjb3ZlcnlDb21taXRtZW50IjoiRWlBVnRqWHgtV0hZZldjQXp6Z2JIZGNBRGhGMUtWU3pweTVUYl94bnUxY2J6QSJ9LCJ0eXBlIjoiY3JlYXRlIn0=",
          "transactionTime": 1655130300,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg",
          "equivalentReferences": [
            "hl:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQm1sUTg5LThuamtFc1BBN3FDRFZRZjJqbWFxSXFXcjg2X1RKek5nRHo0aGc"
          ]
        }
      ]
    }
  }
}
```

**_NOTE:_**  Set ANCHOR_HASH to the anchor hash of the DID in the _did resolve_ response.
For example, did:orb:<mark>uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA</mark>:EiAEjRECXpwFaqFYwoJT...

```commandline
export ANCHOR_HASH=uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA
```

Resolve the canonical DID at orb2:

```commandline
orb-cli did resolve --domain=http://orb2.local --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
    "verificationMethod": [
      {
        "controller": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
        "id": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key1",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      },
      {
        "controller": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
        "id": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key2",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      }
    ],
    "service": [
      {
        "id": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#svc3",
        "priority": 1,
        "recipientKeys": [
          "key3"
        ],
        "serviceEndpoint": [
          {
            "routingKeys": [
              "key3"
            ],
            "uri": "https://example.com"
          }
        ],
        "type": "type3"
      }
    ],
    "authentication": [
      "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key1",
      "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key2"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg",
    "canonicalId": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
    "equivalentId": [
      "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
      "did:orb:hl:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQm1sUTg5LThuamtFc1BBN3FDRFZRZjJqbWFxSXFXcjg2X1RKek5nRHo0aGc:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ"
    ],
    "method": {
      "updateCommitment": "EiDsq7uyQDTYJss8JAV68EcVb1sJHXgUl8knESbzvN4RWw",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImtleTEiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiRWQyNTUxOSIsImt0eSI6Ik9LUCIsIngiOiJvMWJHMVU3RzNDTmJ0QUxNYWZVaUZPcThPRHJhVHlWVG1QdFJETzFRVVdnIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9LHsiaWQiOiJrZXkyIiwicHVibGljS2V5SndrIjp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiYkdNOWFOdWZwS05QeGxreWFjVTFoR2hRWG1fYUM4aEl6U1ZlS0Rwd2pCdyIsInkiOiJQZmRtQ090SWRWWTJCNnVjUjRvUWt0NmV2UWRkWWhPeUhvRFlDYUkyQkpBIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJKc29uV2ViS2V5MjAyMCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMzIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MyJdLCJzZXJ2aWNlRW5kcG9pbnQiOlt7InJvdXRpbmdLZXlzIjpbImtleTMiXSwidXJpIjoiaHR0cHM6Ly9leGFtcGxlLmNvbSJ9XSwidHlwZSI6InR5cGUzIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaURzcTd1eVFEVFlKc3M4SkFWNjhFY1ZiMXNKSFhnVWw4a25FU2J6dk40Uld3In0sInN1ZmZpeERhdGEiOnsiYW5jaG9yT3JpZ2luIjoiaHR0cDovL29yYjEubG9jYWwiLCJkZWx0YUhhc2giOiJFaURGMFBmWHFGY3NrTU9ySlhtUDItWGRxQ1NwbmJIMDZIaC1jQ0dzLUx4OGd3IiwicmVjb3ZlcnlDb21taXRtZW50IjoiRWlBVnRqWHgtV0hZZldjQXp6Z2JIZGNBRGhGMUtWU3pweTVUYl94bnUxY2J6QSJ9LCJ0eXBlIjoiY3JlYXRlIn0=",
          "transactionTime": 1655130300,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg",
          "equivalentReferences": [
            "hl:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQm1sUTg5LThuamtFc1BBN3FDRFZRZjJqbWFxSXFXcjg2X1RKek5nRHo0aGc"
          ]
        }
      ]
    }
  }
}
```

Look at orb2's inbox:

```commandline
curl -s "http://orb2.local/services/orb/inbox?page=true&page-num=0" | jq
```

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "http://orb2.local/services/orb/inbox?page=true&page-num=0",
  "orderedItems": [
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "http://orb1.local/services/orb",
      "id": "http://orb1.local/services/orb/activities/e9b5b0c0-85dc-4f24-8077-64c846b0578b",
      "object": {
        "@context": "https://www.w3.org/ns/activitystreams",
        "actor": "http://orb2.local/services/orb",
        "id": "http://orb2.local/services/orb/activities/eab55426-ddc8-42b3-98c9-9011a3e46199",
        "object": "http://orb1.local/services/orb",
        "to": "http://orb1.local/services/orb",
        "type": "Follow"
      },
      "to": "http://orb2.local/services/orb",
      "type": "Accept"
    },
    {
      "@context": "https://www.w3.org/ns/activitystreams",
      "actor": "http://orb1.local/services/orb",
      "id": "http://orb1.local/services/orb/activities/217b2908-f605-4c6f-b483-6359e1db3398",
      "object": {
        "@context": "https://w3id.org/activityanchors/v1",
        "object": {
          "linkset": [
            {
              "anchor": "hl:uEiC7l-eOSKfFgvBAj8P5tDS8uJLMgqyP5FuDT_8lRs7xZQ",
              "author": [
                {
                  "href": "http://orb1.local/services/orb"
                }
              ],
              "original": [
                {
                  "href": "data:application/gzip;base64,H4sIAAAAAAAA/1yOzU7CQBRG3+W6BQZSEJ3dGEABJRITjCFdTDsDvTBwYX46QNN3N2Vl3H7JOd+pwOBx77QHvq5AHvOCLHAoDA9jFH192e2y4Uj0g00cPl1+vl+S5I0UXqfD3rPan9vbrJSz7iBCC2Twd3pdQWH1ptF4f+KMkc16HUO5NMxpW2KuXbNBnbYAvT78RRQqTjbjQQjBxzh6XcZAh8G5WExuXzOaTxdS+/fy0eDkqsRKxAmtbp/zj/Yy3n0nSxs0+n+F44zFBFWH7Lb5fii7UKd1Wv8GAAD///xpexUBAQAA",
                  "type": "application/linkset+json"
                }
              ],
              "profile": [
                {
                  "href": "https://w3id.org/orb#v0"
                }
              ],
              "related": [
                {
                  "href": "data:application/gzip;base64,H4sIAAAAAAAA/1TNTW+CMACA4f/SXXUqlYDc/MApGyiIH7iYhdoKLY3FAgU1/Pdl3nZ/87xPwOk1K0gJrO8niK/nVEhggZRblU2nBu+S1ebzMk/UZMzMtV7ONmblfLnJ7b7W59Us/DF5UBjN0QcdkEtxoZy8pFSSy59Tlnlh9Xo1pPhdyKQnJHpTfdCeOkDR+F/6Wo6HpGEMGbPxsJKwoGYT7ScQLgSm96UxGOHs1k2Qip2+XluV8LsTYu8EXgT1ipoKQYe6G71Ampei6YhFB1fh7S73w4CTj/wWMWz7YTA4M+8Rh+lwv8NT9+EtkRbkZJtBN+RZDO3yqDlatBW1dziD9tSe2t8AAAD//48HdpUnAQAA",
                  "type": "application/linkset+json"
                }
              ],
              "replies": [
                {
                  "href": "data:application/gzip;base64,H4sIAAAAAAAA/4ySXXObPBSE/8vJLRgQn+bqtY0zftO49VdsJ51MR0gCCwg4kgA7mfz3DklqZzpNp7fS7uo5R/sM/5GqVOygIPwOO6X2MjSMtm17rd2rRGog0woMIhhlpeK4kEZjgXYW2py+yjBRvOHqiEuyq8RnKslILbg6GrLmikkja6WOTGT+o5xR5LpW/2S51+BMtqzjjBEF4TO8QUAIuyKsx3zgsEOWxX40cGphSx4cbjdD255UlB//960+zR/1NG7wlem2HYZgyck78gudfVt+SS7TZjjIgpmromVQX11P08fjzL2so9WPoFhI/3A3Bw32okp4wTr778NUIr5oTNBAsAJCKHiZS6ZAA3Xcs275g1fqa17mcP+iAafvKaHRea1eURFcGA0xnBihwLQS3UVerDu47+gYUaQnno2RRYOYkj5owKWscUlYhFUHhEyEdDPQkbeyvNB2Qifo2Z4ZeA6y797lTPzpzbe5qqRbLREMK0Y/z+u7qG+5XSCtHjAv/xY4q8W+kh0clpIJxatyytSuor8Ea1zU3fWT46+vM4JXfLK6XOVB5q9anK2z9OjNN+3G5dtR5VKlUDsJHlMcIGvnr+10lm0fHLUYXaEcJUufBxEZbg53Ods02c1tvqqj0/ph/NatJU9LrGrBuo6BBg0TPOEEf2ALgXIatiwOzxNdjCdfJ3xr3ZIbPJ16C3MYRdZ467V8saebqX8zmT+xeh1Lcz6Fl/Ofr1/jcVyw0anIoL1X4cPR/cvPAAAA//+M9CZ8qQMAAA==",
                  "type": "application/ld+json"
                }
              ]
            }
          ]
        },
        "type": "AnchorEvent",
        "url": "hl:uEiAEtDeVURdVG6-0vyyyaQERtMrwr8-7PbKqv-JIjZuBKg:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQUV0RGVWVVJkVkc2LTB2eXl5YVFFUnRNcndyOC03UGJLcXYtSklqWnVCS2c"
      },
      "published": "2022-06-13T14:25:00.0312114Z",
      "to": [
        "http://orb1.local/services/orb/followers",
        "https://www.w3.org/ns/activitystreams#Public"
      ],
      "type": "Create"
    }
  ],
  "totalItems": 2,
  "type": "OrderedCollectionPage"
}
```

Update the DID:

```commandline
orb-cli did update --domain=http://orb1.local --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX} --add-publickey-file=./update_publickeys.json --signingkey-file=./update_privatekey.pem --nextupdatekey-file=./nextupdate_publickey.pem --tls-systemcertpool=true
```

Response:

```commandline
successfully updated DID did:orb:uEiAISqEWoGwXgKEtx7Hgt1GXJkh0jPpq3w6bZ6_8FIo5nA:EiAEjRECXpwFaqFYwoJT5XuuUTqwigSNrOOlq4sZKlrnJgroot@af0b0d3c551a
```

Resolve the DID on orb1:

```commandline
orb-cli did resolve --domain=http://orb1.local --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
    "verificationMethod": [
      {
        "controller": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
        "id": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key3",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      },
      {
        "controller": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
        "id": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key4",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      }
    ],
    "authentication": [
      "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key3",
      "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key4"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiD6XU_tO_8n1PmJneefWZlmoHVFG8huAZZeLP3xTX7kYg",
    "canonicalId": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
    "equivalentId": [
      "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
      "did:orb:hl:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQm1sUTg5LThuamtFc1BBN3FDRFZRZjJqbWFxSXFXcjg2X1RKek5nRHo0aGc:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ"
    ],
    "method": {
      "updateCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImtleTEiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiRWQyNTUxOSIsImt0eSI6Ik9LUCIsIngiOiJvMWJHMVU3RzNDTmJ0QUxNYWZVaUZPcThPRHJhVHlWVG1QdFJETzFRVVdnIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9LHsiaWQiOiJrZXkyIiwicHVibGljS2V5SndrIjp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiYkdNOWFOdWZwS05QeGxreWFjVTFoR2hRWG1fYUM4aEl6U1ZlS0Rwd2pCdyIsInkiOiJQZmRtQ090SWRWWTJCNnVjUjRvUWt0NmV2UWRkWWhPeUhvRFlDYUkyQkpBIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJKc29uV2ViS2V5MjAyMCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMzIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MyJdLCJzZXJ2aWNlRW5kcG9pbnQiOlt7InJvdXRpbmdLZXlzIjpbImtleTMiXSwidXJpIjoiaHR0cHM6Ly9leGFtcGxlLmNvbSJ9XSwidHlwZSI6InR5cGUzIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaURzcTd1eVFEVFlKc3M4SkFWNjhFY1ZiMXNKSFhnVWw4a25FU2J6dk40Uld3In0sInN1ZmZpeERhdGEiOnsiYW5jaG9yT3JpZ2luIjoiaHR0cDovL29yYjEubG9jYWwiLCJkZWx0YUhhc2giOiJFaURGMFBmWHFGY3NrTU9ySlhtUDItWGRxQ1NwbmJIMDZIaC1jQ0dzLUx4OGd3IiwicmVjb3ZlcnlDb21taXRtZW50IjoiRWlBVnRqWHgtV0hZZldjQXp6Z2JIZGNBRGhGMUtWU3pweTVUYl94bnUxY2J6QSJ9LCJ0eXBlIjoiY3JlYXRlIn0=",
          "transactionTime": 1655130300,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg",
          "equivalentReferences": [
            "hl:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQm1sUTg5LThuamtFc1BBN3FDRFZRZjJqbWFxSXFXcjg2X1RKek5nRHo0aGc"
          ]
        },
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJyZW1vdmUtcHVibGljLWtleXMiLCJpZHMiOlsia2V5MSIsImtleTIiXX0seyJhY3Rpb24iOiJyZW1vdmUtc2VydmljZXMiLCJpZHMiOlsic3ZjMyJdfSx7ImFjdGlvbiI6ImFkZC1wdWJsaWMta2V5cyIsInB1YmxpY0tleXMiOlt7ImlkIjoia2V5MyIsInB1YmxpY0tleUp3ayI6eyJjcnYiOiJFZDI1NTE5Iiwia3R5IjoiT0tQIiwieCI6Im8xYkcxVTdHM0NOYnRBTE1hZlVpRk9xOE9EcmFUeVZUbVB0UkRPMVFVV2cifSwicHVycG9zZXMiOlsiYXV0aGVudGljYXRpb24iXSwidHlwZSI6IkVkMjU1MTlWZXJpZmljYXRpb25LZXkyMDE4In0seyJpZCI6ImtleTQiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiUC0yNTYiLCJrdHkiOiJFQyIsIngiOiJiR005YU51ZnBLTlB4bGt5YWNVMWhHaFFYbV9hQzhoSXpTVmVLRHB3akJ3IiwieSI6IlBmZG1DT3RJZFZZMkI2dWNSNG9Ra3Q2ZXZRZGRZaE95SG9EWUNhSTJCSkEifSwicHVycG9zZXMiOlsiYXV0aGVudGljYXRpb24iXSwidHlwZSI6Ikpzb25XZWJLZXkyMDIwIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaUFWdGpYeC1XSFlmV2NBenpnYkhkY0FEaEYxS1ZTenB5NVRiX3hudTFjYnpBIn0sImRpZFN1ZmZpeCI6IkVpRFpxRnFzNFJ6bHdBYmtPUWh1YWJxQTZRRDBLU1RrYW5CR2N4ZGZiRlE1aFEiLCJyZXZlYWxWYWx1ZSI6IkVpREZiaFZsZk5Qb3FFS3N4Ul9LRW1PdzJjYlZHcGtvcnFxWnRER0FnU3kwdmciLCJzaWduZWREYXRhIjoiZXlKaGJHY2lPaUpGWkVSVFFTSXNJbXRwWkNJNklqQXlOVEUxWVdFMUxUa3laVEV0TkRsbVl5MDVPVGs0TFRZNU5XVXdNakEzWmpBNFpDSjkuZXlKa1pXeDBZVWhoYzJnaU9pSkZhVUZtVUhOUVUzWkNNemM1UlVGTlpWWXpVMjU0WkhGTmVrUmphM28zZG01b1RWSXpXR3c1Ykd0bGJXOW5JaXdpZFhCa1lYUmxTMlY1SWpwN0ltTnlkaUk2SWtWa01qVTFNVGtpTENKcmRIa2lPaUpQUzFBaUxDSjRJam9pTTBaTFJUSnlPVFJNYVZJd1IyWnhWRlJVT1hkMGRFMTVWV1pGVVd0Zk4zbE5kRWN5U2tseGVscDVTU0lzSW5raU9pSWlmWDAuQUYtVUJ5ZWZ4WERwZXFUUkZxTDVlUFhnNk1saWxkN2g4NzNHLUpCbDEtTWoxeTZiY0JmMTVXYWNraVlqWFhDR25CbXpkQUlKSUtNWVBTUHVoSHluRFEiLCJ0eXBlIjoidXBkYXRlIn0=",
          "transactionTime": 1655130434,
          "type": "update",
          "canonicalReference": "uEiD6XU_tO_8n1PmJneefWZlmoHVFG8huAZZeLP3xTX7kYg",
          "equivalentReferences": [
            "hl:uEiD6XU_tO_8n1PmJneefWZlmoHVFG8huAZZeLP3xTX7kYg:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpRDZYVV90T184bjFQbUpuZWVmV1psbW9IVkZHOGh1QVpaZUxQM3hUWDdrWWc"
          ]
        }
      ]
    }
  }
}
```

Resolve the DID on orb2:

```commandline
orb-cli did resolve --domain=http://orb1.local --did-uri=did:orb:${ANCHOR_HASH}:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
    "verificationMethod": [
      {
        "controller": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
        "id": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key3",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      },
      {
        "controller": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
        "id": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key4",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      }
    ],
    "authentication": [
      "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key3",
      "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ#key4"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiD6XU_tO_8n1PmJneefWZlmoHVFG8huAZZeLP3xTX7kYg",
    "canonicalId": "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
    "equivalentId": [
      "did:orb:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ",
      "did:orb:hl:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQm1sUTg5LThuamtFc1BBN3FDRFZRZjJqbWFxSXFXcjg2X1RKek5nRHo0aGc:EiDZqFqs4RzlwAbkOQhuabqA6QD0KSTkanBGcxdfbFQ5hQ"
    ],
    "method": {
      "updateCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImtleTEiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiRWQyNTUxOSIsImt0eSI6Ik9LUCIsIngiOiJvMWJHMVU3RzNDTmJ0QUxNYWZVaUZPcThPRHJhVHlWVG1QdFJETzFRVVdnIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9LHsiaWQiOiJrZXkyIiwicHVibGljS2V5SndrIjp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiYkdNOWFOdWZwS05QeGxreWFjVTFoR2hRWG1fYUM4aEl6U1ZlS0Rwd2pCdyIsInkiOiJQZmRtQ090SWRWWTJCNnVjUjRvUWt0NmV2UWRkWWhPeUhvRFlDYUkyQkpBIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJKc29uV2ViS2V5MjAyMCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMzIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MyJdLCJzZXJ2aWNlRW5kcG9pbnQiOlt7InJvdXRpbmdLZXlzIjpbImtleTMiXSwidXJpIjoiaHR0cHM6Ly9leGFtcGxlLmNvbSJ9XSwidHlwZSI6InR5cGUzIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaURzcTd1eVFEVFlKc3M4SkFWNjhFY1ZiMXNKSFhnVWw4a25FU2J6dk40Uld3In0sInN1ZmZpeERhdGEiOnsiYW5jaG9yT3JpZ2luIjoiaHR0cDovL29yYjEubG9jYWwiLCJkZWx0YUhhc2giOiJFaURGMFBmWHFGY3NrTU9ySlhtUDItWGRxQ1NwbmJIMDZIaC1jQ0dzLUx4OGd3IiwicmVjb3ZlcnlDb21taXRtZW50IjoiRWlBVnRqWHgtV0hZZldjQXp6Z2JIZGNBRGhGMUtWU3pweTVUYl94bnUxY2J6QSJ9LCJ0eXBlIjoiY3JlYXRlIn0=",
          "transactionTime": 1655130300,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg",
          "equivalentReferences": [
            "hl:uEiBmlQ89-8njkEsPA7qCDVQf2jmaqIqWr86_TJzNgDz4hg:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQm1sUTg5LThuamtFc1BBN3FDRFZRZjJqbWFxSXFXcjg2X1RKek5nRHo0aGc"
          ]
        },
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJyZW1vdmUtcHVibGljLWtleXMiLCJpZHMiOlsia2V5MSIsImtleTIiXX0seyJhY3Rpb24iOiJyZW1vdmUtc2VydmljZXMiLCJpZHMiOlsic3ZjMyJdfSx7ImFjdGlvbiI6ImFkZC1wdWJsaWMta2V5cyIsInB1YmxpY0tleXMiOlt7ImlkIjoia2V5MyIsInB1YmxpY0tleUp3ayI6eyJjcnYiOiJFZDI1NTE5Iiwia3R5IjoiT0tQIiwieCI6Im8xYkcxVTdHM0NOYnRBTE1hZlVpRk9xOE9EcmFUeVZUbVB0UkRPMVFVV2cifSwicHVycG9zZXMiOlsiYXV0aGVudGljYXRpb24iXSwidHlwZSI6IkVkMjU1MTlWZXJpZmljYXRpb25LZXkyMDE4In0seyJpZCI6ImtleTQiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiUC0yNTYiLCJrdHkiOiJFQyIsIngiOiJiR005YU51ZnBLTlB4bGt5YWNVMWhHaFFYbV9hQzhoSXpTVmVLRHB3akJ3IiwieSI6IlBmZG1DT3RJZFZZMkI2dWNSNG9Ra3Q2ZXZRZGRZaE95SG9EWUNhSTJCSkEifSwicHVycG9zZXMiOlsiYXV0aGVudGljYXRpb24iXSwidHlwZSI6Ikpzb25XZWJLZXkyMDIwIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaUFWdGpYeC1XSFlmV2NBenpnYkhkY0FEaEYxS1ZTenB5NVRiX3hudTFjYnpBIn0sImRpZFN1ZmZpeCI6IkVpRFpxRnFzNFJ6bHdBYmtPUWh1YWJxQTZRRDBLU1RrYW5CR2N4ZGZiRlE1aFEiLCJyZXZlYWxWYWx1ZSI6IkVpREZiaFZsZk5Qb3FFS3N4Ul9LRW1PdzJjYlZHcGtvcnFxWnRER0FnU3kwdmciLCJzaWduZWREYXRhIjoiZXlKaGJHY2lPaUpGWkVSVFFTSXNJbXRwWkNJNklqQXlOVEUxWVdFMUxUa3laVEV0TkRsbVl5MDVPVGs0TFRZNU5XVXdNakEzWmpBNFpDSjkuZXlKa1pXeDBZVWhoYzJnaU9pSkZhVUZtVUhOUVUzWkNNemM1UlVGTlpWWXpVMjU0WkhGTmVrUmphM28zZG01b1RWSXpXR3c1Ykd0bGJXOW5JaXdpZFhCa1lYUmxTMlY1SWpwN0ltTnlkaUk2SWtWa01qVTFNVGtpTENKcmRIa2lPaUpQUzFBaUxDSjRJam9pTTBaTFJUSnlPVFJNYVZJd1IyWnhWRlJVT1hkMGRFMTVWV1pGVVd0Zk4zbE5kRWN5U2tseGVscDVTU0lzSW5raU9pSWlmWDAuQUYtVUJ5ZWZ4WERwZXFUUkZxTDVlUFhnNk1saWxkN2g4NzNHLUpCbDEtTWoxeTZiY0JmMTVXYWNraVlqWFhDR25CbXpkQUlKSUtNWVBTUHVoSHluRFEiLCJ0eXBlIjoidXBkYXRlIn0=",
          "transactionTime": 1655130434,
          "type": "update",
          "canonicalReference": "uEiD6XU_tO_8n1PmJneefWZlmoHVFG8huAZZeLP3xTX7kYg",
          "equivalentReferences": [
            "hl:uEiD6XU_tO_8n1PmJneefWZlmoHVFG8huAZZeLP3xTX7kYg:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpRDZYVV90T184bjFQbUpuZWVmV1psbW9IVkZHOGh1QVpaZUxQM3hUWDdrWWc"
          ]
        }
      ]
    }
  }
}
```

Shut down the environment:

```commandline
docker-compose -f docker-compose-cli.yml -f ../docker/docker-compose-dev.yml down
```

## Orb with VCT

Start two Orb instances (orb1.local and orb2.local), [VCT](vct/introduction.html#verifiable-credential-transparency-vct)
(includes the Google Trillian server and signer) and a command-line container:

```commandline
cd orb/samples/tutorial

docker-compose -f docker-compose-cli.yml -f ../docker/docker-compose-dev-vct.yml up&
```

In another terminal, open an interactive Docker shell:

```commandline
docker exec -ti cli /bin/bash

cd ./orb
```

Add a VCT log to orb1:

```commandline
orb-cli log update --url http://orb1.local/log --log http://orb.vct:8077/maple2022
```

Response:

```commandline
Domain log has successfully been updated.
```

Create a DID at orb1:

```commandline
orb-cli did create --domain=http://orb1.local --publickey-file=./create_publickeys.json --service-file=./create_services.json --recoverykey-file=./recover_publickey.pem --updatekey-file=./update_publickey.pem --did-anchor-origin=http://orb1.local | jq
```

Response:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/suites/ed25519-2018/v1",
    "https://w3id.org/security/suites/jws-2020/v1"
  ],
  "id": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA",
  "verificationMethod": [
    {
      "controller": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA",
      "id": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA#key1",
      "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
      "type": "Ed25519VerificationKey2018"
    },
    {
      "controller": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA",
      "id": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA#key2",
      "publicKeyJwk": {
        "kty": "EC",
        "crv": "P-256",
        "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
        "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
      },
      "type": "JsonWebKey2020"
    }
  ],
  "service": [
    {
      "id": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA#svc1",
      "priority": 1,
      "recipientKeys": [
        "key1"
      ],
      "serviceEndpoint": [
        {
          "routingKeys": [
            "key1"
          ],
          "uri": "https://example.com"
        }
      ],
      "type": "type1"
    },
    {
      "id": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA#svc2",
      "priority": 2,
      "recipientKeys": [
        "key2"
      ],
      "serviceEndpoint": [
        {
          "routingKeys": [
            "key2"
          ],
          "uri": "https://example.com"
        }
      ],
      "type": "type2"
    }
  ],
  "authentication": [
    "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA#key1",
    "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA#key2"
  ]
}
```

Resolve the DID at orb1 to get the anchor hash:

**_NOTE:_**  Set DID_SUFFIX to the DID suffix in the _id_ field of the _did create_ response.
For example, did:orb:uAAA:<mark>EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg</mark>.

```commandline
export DID_SUFFIX=EiCGIu4PrFTVEZRb8SOZt4nbZRF7Wp2_qX4Zt0czROKHxg

orb-cli did resolve --domain=http://orb1.local --did-uri=did:orb:uAAA:${DID_SUFFIX} --verify-resolution-result-type=all | jq
```

Response:

```json
{
  "@context": [
    "https://w3id.org/did-resolution/v1"
  ],
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1",
      "https://w3id.org/security/suites/jws-2020/v1"
    ],
    "id": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA",
    "verificationMethod": [
      {
        "controller": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA",
        "id": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA#key1",
        "publicKeyBase58": "BzcCdRP41BvfyYUq2aC5U5RXdp4zXjYfduubF6EuE79R",
        "type": "Ed25519VerificationKey2018"
      },
      {
        "controller": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA",
        "id": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA#key2",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "P-256",
          "x": "bGM9aNufpKNPxlkyacU1hGhQXm_aC8hIzSVeKDpwjBw",
          "y": "PfdmCOtIdVY2B6ucR4oQkt6evQddYhOyHoDYCaI2BJA"
        },
        "type": "JsonWebKey2020"
      }
    ],
    "service": [
      {
        "id": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA#svc1",
        "priority": 1,
        "recipientKeys": [
          "key1"
        ],
        "serviceEndpoint": [
          {
            "routingKeys": [
              "key1"
            ],
            "uri": "https://example.com"
          }
        ],
        "type": "type1"
      },
      {
        "id": "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA#svc2",
        "priority": 2,
        "recipientKeys": [
          "key2"
        ],
        "serviceEndpoint": [
          {
            "routingKeys": [
              "key2"
            ],
            "uri": "https://example.com"
          }
        ],
        "type": "type2"
      }
    ],
    "authentication": [
      "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA#key1",
      "did:orb:uAAA:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA#key2"
    ]
  },
  "didDocumentMetadata": {
    "versionId": "uEiCVI1cESlp5f_H503m_WZn-Zu3jt6f0j1bqc8xE79hRkw",
    "canonicalId": "did:orb:uEiCVI1cESlp5f_H503m_WZn-Zu3jt6f0j1bqc8xE79hRkw:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA",
    "equivalentId": [
      "did:orb:uEiCVI1cESlp5f_H503m_WZn-Zu3jt6f0j1bqc8xE79hRkw:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA",
      "did:orb:hl:uEiCVI1cESlp5f_H503m_WZn-Zu3jt6f0j1bqc8xE79hRkw:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQ1ZJMWNFU2xwNWZfSDUwM21fV1puLVp1M2p0NmYwajFicWM4eEU3OWhSa3c:EiDMMnBARgGo1J7Y6v3p3CFv5Wu5XcNEpQSmisoufLyrVA"
    ],
    "method": {
      "updateCommitment": "EiDsq7uyQDTYJss8JAV68EcVb1sJHXgUl8knESbzvN4RWw",
      "recoveryCommitment": "EiAVtjXx-WHYfWcAzzgbHdcADhF1KVSzpy5Tb_xnu1cbzA",
      "published": true,
      "anchorOrigin": "http://orb1.local",
      "publishedOperations": [
        {
          "operation": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImtleTEiLCJwdWJsaWNLZXlKd2siOnsiY3J2IjoiRWQyNTUxOSIsImt0eSI6Ik9LUCIsIngiOiJvMWJHMVU3RzNDTmJ0QUxNYWZVaUZPcThPRHJhVHlWVG1QdFJETzFRVVdnIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9LHsiaWQiOiJrZXkyIiwicHVibGljS2V5SndrIjp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiYkdNOWFOdWZwS05QeGxreWFjVTFoR2hRWG1fYUM4aEl6U1ZlS0Rwd2pCdyIsInkiOiJQZmRtQ090SWRWWTJCNnVjUjRvUWt0NmV2UWRkWWhPeUhvRFlDYUkyQkpBIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJKc29uV2ViS2V5MjAyMCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJzdmMxIiwicHJpb3JpdHkiOjEsInJlY2lwaWVudEtleXMiOlsia2V5MSJdLCJzZXJ2aWNlRW5kcG9pbnQiOlt7InJvdXRpbmdLZXlzIjpbImtleTEiXSwidXJpIjoiaHR0cHM6Ly9leGFtcGxlLmNvbSJ9XSwidHlwZSI6InR5cGUxIn0seyJpZCI6InN2YzIiLCJwcmlvcml0eSI6MiwicmVjaXBpZW50S2V5cyI6WyJrZXkyIl0sInNlcnZpY2VFbmRwb2ludCI6W3sicm91dGluZ0tleXMiOlsia2V5MiJdLCJ1cmkiOiJodHRwczovL2V4YW1wbGUuY29tIn1dLCJ0eXBlIjoidHlwZTIifV19XSwidXBkYXRlQ29tbWl0bWVudCI6IkVpRHNxN3V5UURUWUpzczhKQVY2OEVjVmIxc0pIWGdVbDhrbkVTYnp2TjRSV3cifSwic3VmZml4RGF0YSI6eyJhbmNob3JPcmlnaW4iOiJodHRwOi8vb3JiMS5sb2NhbCIsImRlbHRhSGFzaCI6IkVpQ1d6RjJHRXBrbnNiR3pXaGpmM01PUWFobGdVRGJuYk5yWWZCTmFoZzV4R0EiLCJyZWNvdmVyeUNvbW1pdG1lbnQiOiJFaUFWdGpYeC1XSFlmV2NBenpnYkhkY0FEaEYxS1ZTenB5NVRiX3hudTFjYnpBIn0sInR5cGUiOiJjcmVhdGUifQ==",
          "transactionTime": 1655130610,
          "type": "create",
          "anchorOrigin": "http://orb1.local",
          "canonicalReference": "uEiCVI1cESlp5f_H503m_WZn-Zu3jt6f0j1bqc8xE79hRkw",
          "equivalentReferences": [
            "hl:uEiCVI1cESlp5f_H503m_WZn-Zu3jt6f0j1bqc8xE79hRkw:uoQ-BeEVodHRwOi8vb3JiMS5sb2NhbC9jYXMvdUVpQ1ZJMWNFU2xwNWZfSDUwM21fV1puLVp1M2p0NmYwajFicWM4eEU3OWhSa3c"
          ]
        }
      ]
    }
  }
}
```

Get hash of anchor from the _resolve_ response (from the _canonicalId_ field in the metadata) and set the ANCHOR_HASH environment variable.
For example, did:orb:<mark>uEiBtg3HASdLSUHLYds6givvkjJX_DEb8cqONqpKuq_Y_aA</mark>:EiCGIu4PrFTVEZRb8SOZ...

Verify that the proof(s) in the anchor linkset are in the VCT log:

```commandline
export ANCHOR_HASH=uEiBtg3HASdLSUHLYds6givvkjJX_DEb8cqONqpKuq_Y_aA

orb-cli vct verify --cas-url=http://orb1.local/cas --anchor=${ANCHOR_HASH}
```

Response:

```json
[
  {
    "domain": "http://orb.vct:8077/maple2022",
    "proofValue": "z5LcS2Aa8zGVwunZKFap9DJZYs3YaRgU2MD5kfMjYyK5oLfBZUZieZBsvkpdqqjUZeZCBJqBQnJ6r6esQUwQXmMhG",
    "found": true,
    "leafIndex": 0,
    "auditPath": null
  }
]
```

You'll notice that _leafIndex_ is 0 and _auditPath_ is empty since there's only one entry in the Merkle tree.

Create another DID at orb1 and use `orb-cli vct verify` to verify that the proofs are in the log.

```json
[
  {
    "domain": "http://orb.vct:8077/maple2022",
    "proofValue": "JnP2nE4jLiHsmR65q1a6Tp-HUKZanjpCstm7qAljIE3Z6Y2mEcNarOESiu1gFlsonz1soQJe69piE7qZnTqrAw",
    "found": true,
    "leafIndex": 1,
    "auditPath": [
      "rgWgpjyv6227zFbcpyRv3YR1vbLbXaOuCeYoF7uDaY4="
    ]
  }
]
```

You'll now notice that _leafIndex_ is 1 and _auditPath_ has one entry.

Shut down the environment:

```commandline
docker-compose -f docker-compose-cli.yml -f ../docker/docker-compose-dev-vct.yml down
```
