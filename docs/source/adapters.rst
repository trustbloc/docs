########
Adapters
########

*******************
What is an Adapter?
*******************

TrustBloc's `Edge-Adapter <https://github.com/trustbloc/edge-adapter>`__ acts as a go-between for
Relying Party (RP) and Issuer components to support DIDComm operations.


TrustBloc's Edge-Adapter can be used to run an Issuer and an RP.

`Get the adapter here <https://github.com/trustbloc/edge-adapter/tree/master/cmd/adapter-rest>`__.

Here are the flags for the server::

                Start adapter-rest inside the edge-adapter

                Usage:
                        adapter-rest start [flags]

                Flags:
                        --didcomm-db-path string                 Path to database. Alternatively, this can be set with the following environment variable: ADAPTER_REST_DIDCOMM_DB_PATH
                        --didcomm-inbound-host string            Inbound Host Name:Port. This is used internally to start the didcomm server. Alternatively, this can be set with the following environment variable: ADAPTER_REST_DIDCOMM_INBOUND_HOST
                        --didcomm-inbound-host-external string   Inbound Host External Name:Port. This is the URL for the inbound server as seen externally. If not provided, then the internal inbound host will be used here. Alternatively, this can be set with the following environment variable: ADAPTER_REST_DIDCOMM_INBOUND_HOST_EXTERNAL
                        --dids-trustbloc-domain string           URL to the did:trustbloc consortium's domain. Alternatively, this can be set with the following environment variable: ADAPTER_REST_TRUSTBLOC_DOMAIN
                        --dsn string                             Datasource Name with credentials if required. Format must be <driver>:[//]<driver-specific-dsn>. Examples: 'mysql://root:secret@tcp(localhost:3306)/adapter', 'mem://test'. Supported drivers are [mem, mysql]. Alternatively, this can be set with the following environment variable: ADAPTER_REST_DSN
                        --dsn-timeout string                     Total time in seconds to wait until the datasource is available before giving up. Default:  seconds. Alternatively, this can be set with the following environment variable: ADAPTER_REST_DSN_TIMEOUT
                        --governance-vcs-url string              Governance VCS instance is running on. Format: HostName:Port.
                        -h, --help                                   help for start
                        -u, --host-url string                        URL to run the adapter-rest instance on. Format: HostName:Port.
                        --hydra-url string                       Base URL to the hydra service.Alternatively, this can be set with the following environment variable: ADAPTER_REST_HYDRA_URL
                        --log-level string                       Sets the logging level. Possible values are [DEBUG, INFO, WARNING, ERROR, CRITICAL] (default is INFO). Alternatively, this can be set with the following environment variable: ADAPTER_REST_LOGLEVEL (default "INFO")
                        --mode string                            Mode in which the edge-adapter service will run. Possible values: ['issuer', 'rp'].
                        --op-url string                          URL for the OIDC provider.Alternatively, this can be set with the following environment variable: ADAPTER_REST_OP_URL
                        --presentation-definitions-file string   Path to presentation definitions file with input_descriptors.
                        --request-tokens stringArray             Tokens used for http request  Alternatively, this can be set with the following environment variable: ADAPTER_REST_REQUEST_TOKENS
                        --static-path string                     Path to the folder where the static files are to be hosted under /ui.Alternatively, this can be set with the following environment variable: ADAPTER_REST_STATIC_FILES
                        --tls-cacerts stringArray                Comma-Separated list of ca certs path. Alternatively, this can be set with the following environment variable: ADAPTER_REST_TLS_CACERTS
                        --tls-serve-cert string                  Path to the server certificate to use when serving HTTPS. Alternatively, this can be set with the following environment variable: ADAPTER_REST_TLS_SERVE_CERT
                        --tls-serve-key string                   Path to the private key to use when serving HTTPS. Alternatively, this can be set with the following environment variable: ADAPTER_REST_TLS_SERVE_KEY
                        --tls-systemcertpool string              Use system certificate pool. Possible values [true] [false]. Defaults to false if not set. Alternatively, this can be set with the following environment variable: ADAPTER_REST_TLS_SYSTEMCERTPOOL
                        -r, --universal-resolver-url string          Universal Resolver instance is running on. Format: HostName:Port.

RP Adapter
==========

The Relying Party (RP) Adapter enables standard OpenID Connect flows on top of DIDComm.


Configuring the RP Adapter
--------------------------

The following is a snippet of a Docker Compose :superscript:`TM` file showing how `Edge Adapter <https://github.com/trustbloc/edge-adapter>`__ can be configured for use as an RP.

.. code:: yaml

  rp.adapter.rest.example.com:
    container_name: rp.adapter.rest.example.com
    image: ${RP_ADAPTER_REST_IMAGE}:latest
    environment:
      - ADAPTER_REST_HOST_URL=0.0.0.0:8070
      - ADAPTER_REST_TLS_CACERTS=/etc/tls/ec-cacert.pem
      - ADAPTER_REST_GOVERNANCE_VCS_URL=http://governance.vcs.example.com:8066
      - ADAPTER_REST_TLS_SYSTEMCERTPOOL=true
      - ADAPTER_REST_TLS_SERVE_CERT=/etc/tls/ec-pubCert.pem
      - ADAPTER_REST_TLS_SERVE_KEY=/etc/tls/ec-key.pem
      - ADAPTER_REST_DSN=mysql://rpadapter:rpadapter-secret-pw@tcp(mysql:3306)/
      - ADAPTER_REST_OP_URL=http://PUT-SOMETHING-HERE.com
      - ADAPTER_REST_PRESENTATION_DEFINITIONS_FILE=/etc/testdata/presentationdefinitions.json
      - ADAPTER_REST_DIDCOMM_INBOUND_HOST=0.0.0.0:8071
      - ADAPTER_REST_DIDCOMM_INBOUND_HOST_EXTERNAL=http://rp.adapter.rest.example.com:8071
      - ADAPTER_REST_TRUSTBLOC_DOMAIN=${BLOC_DOMAIN}
      - ADAPTER_REST_HYDRA_URL=https://hydra.trustbloc.local:4445
      - ADAPTER_REST_UNIVERSAL_RESOLVER_URL=http://did.rest.example.com:8072/1.0/identifiers
      - ADAPTER_REST_DSN_TIMEOUT=45
    ports:
      - 8070:8070
    entrypoint: ""
    command:  /bin/sh -c "adapter-rest start"
    volumes:
      - ../keys/tls:/etc/tls
      - ../testdata:/etc/testdata
    networks:
      - bdd_net
    depends_on:
      - hydra
      - mysql

See this example in full `here <https://github.com/trustbloc/edge-adapter/blob/master/test/bdd/fixtures/adapter-rest/docker-compose.yml>`__.


Deploying the RP Adapter
------------------------

To learn about integrating your OIDC client to a TrustBloc RP Adapter,
read our `integration guide <https://github.com/trustbloc/edge-adapter/blob/master/docs/rp/integration/relying_parties.md>`__.


Issuer Adapter
==============

This component is an intermediary to act on behalf of an Issuer to perform DIDComm related use cases.

Configuring the Issuer Adapter
------------------------------

The following is a snippet of a Docker Compose :superscript:`TM` file showing how `Edge Adapter <https://github.com/trustbloc/edge-adapter>`__ can be configured for use as an issuer.

.. code:: yaml

  issuer.adapter.rest.example.com:
    container_name: issuer.adapter.rest.example.com
    image: ${ISSUER_ADAPTER_REST_IMAGE}:latest
    environment:
      - ADAPTER_REST_HOST_URL=0.0.0.0:9070
      - ADAPTER_REST_GOVERNANCE_VCS_URL=http://governance.vcs.example.com:8066
      - ADAPTER_REST_TLS_CACERTS=/etc/tls/ec-cacert.pem
      - ADAPTER_REST_TLS_SYSTEMCERTPOOL=true
      - ADAPTER_REST_TLS_SERVE_CERT=/etc/tls/ec-pubCert.pem
      - ADAPTER_REST_TLS_SERVE_KEY=/etc/tls/ec-key.pem
      - ADAPTER_REST_DIDCOMM_INBOUND_HOST=0.0.0.0:9071
      - ADAPTER_REST_DIDCOMM_INBOUND_HOST_EXTERNAL=http://issuer.adapter.rest.example.com:9071
      - ADAPTER_REST_TRUSTBLOC_DOMAIN=${BLOC_DOMAIN}
      - ADAPTER_REST_UNIVERSAL_RESOLVER_URL=http://did.rest.example.com:8072/1.0/identifiers
      - ADAPTER_REST_DSN=mysql://issueradapter:issueradapter-secret-pw@tcp(mysql:3306)/
      - ADAPTER_REST_DSN_TIMEOUT=45
    ports:
      - 9070:9070
      - 9071:9071
    entrypoint: ""
    command:  /bin/sh -c "adapter-rest start"
    volumes:
      - ../keys/tls:/etc/tls
    networks:
      - bdd_net

See this example in full `here <https://github.com/trustbloc/edge-adapter/blob/master/test/bdd/fixtures/adapter-rest/docker-compose.yml>`__.


Deploying the Issuer Adapter
----------------------------

`Integration guide <https://github.com/trustbloc/edge-adapter/tree/master/docs/issuer>`__


Adapter Components (CHAPI + DIDComm)
====================================

.. image:: images/adapter_component_diagram.svg

Flows
=====

The Evidence and Driver's License (DL) Flow
-------------------------------------------

These components allow users to access services with a VC such as a Driver's License.
They are:

* Issuer Adapter
* RP Adapter


Combined DL, Evidence & Credit Score Flow
-----------------------------------------

Here is an overfiew of the `Bank Account usecase <https://github.com/trustbloc/edge-sandbox/blob/master/docs/demo/new-bank-account-usecase.md>`__.

This scenario shows how a person can open a bank account using both local and remote credentials.
A local credential is stored in a user's wallet while the remote credential is stored with a third-party.

In order to create the bank account, a Drivers License (local credential), Drivers Licence Evidence (remote credential)
and Credit Score (remote credential) are required.

These are issued as VCs from a `Drivers License Issuer <https://demo-issuer.sandbox.trustbloc.dev/drivinglicense>`__ and
a `Credit Score Issuer <https://demo-issuer.sandbox.trustbloc.dev/creditscore>`__.

This uses the `Adapter/DIDComm <https://github.com/trustbloc/edge-sandbox/blob/master/docs/demo/sandbox_adapter_playground.md>`__ flow.

Watch the demos below.

Creating a New Bank Account
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. raw:: html

         <iframe
                width="560"
                height="315"
                src="https://www.youtube.com/embed/0ZNmk6E2EFE"
                frameborder="0"
                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                allowfullscreen>
        </iframe>


DL, Evidence and Credit Score
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. raw:: html

         <iframe
                width="560"
                height="315"
                src="https://www.youtube.com/embed/JNUQaOwprT8"
                frameborder="0"
                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                allowfullscreen>
        </iframe>


