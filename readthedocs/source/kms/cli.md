# CLI
The KMS Command Line Interface (KMS CLI) is a unified tool that provides a consistent interface for interacting with KMS. 

## Create Keystore
This command is used for creating a keystore.

### Usage
```
keystore create [flags]
```

### Flags
* `controller` _[string]_ - DID of keystore controller.
* `url` _[string]_ - URL of KMS server.
* `auth-token` _[string]_ - The Auth token.
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.

### Example

#### create keystore cmd
```
keystore create --controller did:example:123456 --url https://localhost:8078
--tls-cacerts fixtures/keys/tls/ec-cacert.pem
```

## Create Key
This command is used for creating keys in the keystore.

### Usage
```
key create [flags]
```

### Flags
* `keystore` _[string]_ - ID of the keystore.
* `type` _[string]_ - Type of the key.
* `url` _[string]_ - URL of KMS server.
* `auth-token` _[string]_ - The Auth token.
* `tls-cacerts ` _[array|string]_ - Array of one or more CA cert paths.
* `tls-systemcertpool ` _[boolean]_ - Flag whether to use system certificate pool.

### Example

#### create key cmd
```
key create --keystore {keystoreID} --url https://localhost:8078 --type ED25519
--tls-cacerts fixtures/keys/tls/ec-cacert.pem
```