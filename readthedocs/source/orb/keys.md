# Keys
Orb use two crypto keys to do the following operations:

- Sign HTTP requests between orb servers.
- Sign VC.

## Establish orb key
There are 2 ways to establish the orb key:

- Import a pre-existing private key and id into KMS at Orb startup via Orb configuration
- Prior to orb startup, create a new key within KMS and establish orb configuration

### Add pre-existing private key to orb configuration
When Orb server start will import the private key and id into KMS sever.

#### Steps to create VC sign key
- Create ed25519 private key.
- Configure ORB_VC_SIGN_PRIVATE_KEYS with value of ed25519 private key as base64.

#### Example VC sign key
```
ORB_KMS_ENDPOINT=https://orb.kms
ORB_VC_SIGN_PRIVATE_KEYS=orbkey1=9kRTh70Ut0MKPeHY3Gdv/pi8SACx6dFjaEiIHf7JDugPpXBnCHVvRbgdzYbWfCGsXdvh/Zct+AldKG4bExjHXg
ORB_VC_SIGN_ACTIVE_KEY_ID=orbkey1
```

#### Steps to create HTTP sign key
- Create ed25519 private key.
- Configure ORB_PRIVATE_KEY with value of ed25519 private key as base64.

#### Example HTTP sign key
```
ORB_KMS_ENDPOINT=https://orb.kms
ORB_HTTP_SIGN_PRIVATE_KEY=orbkey1=9kRTh70Ut0MKPeHY3Gdv/pi8SACx6dFjaEiIHf7JDugPpXBnCHVvRbgdzYbWfCGsXdvh/Zct+AldKG4bExjHXg
ORB_HTTP_SIGN_ACTIVE_KEY_ID=orbkey1
```

### Create private key in KMS and configure orb to use
Key need to be created in KMS before orb server starting.

#### Steps to create VC sign key
- Create KMS keystore using KMS [cli](../kms/cli.html).
- Create ed25519 key in KMS using KMS [cli](../kms/cli.html).
- Configure ORB_VC_SIGN_KEYS_ID with key id.

#### Example VC sign key
```
ORB_KMS_STORE_ENDPOINT=https://orb.kms/keystore
ORB_VC_SIGN_KEYS_ID=orbkey1
ORB_VC_SIGN_ACTIVE_KEY_ID=orbkey1
```

#### Steps to create HTTP sign key
- Create KMS keystore using KMS [cli](../kms/cli.html).
- Create ed25519 key in KMS using KMS [cli](../kms/cli.html).
- Configure ORB_HTTP_SIGN_ACTIVE_KEY_ID with key id.

#### Example HTTP sign key
```
ORB_KMS_STORE_ENDPOINT=https://orb.kms/keystore
ORB_HTTP_SIGN_ACTIVE_KEY_ID=orbkey1
```

## Storing
The keys will be managed and stored in KMS please refer to this [doc](../kms/keys.html) for more details.
Orb is using Scenario 1 in kms doc.

## Rotation
There is two ways to rotate orb key:

- Import a pre-existing private key and id into KMS at Orb startup via Orb configuration
- Prior to orb startup, create a new key within KMS and establish orb configuration

### Add pre-existing private key to orb configuration
When you need to rotate the key just add the new private key to orb configuration.

#### Steps to rotate VC sign key
- Create new ed25519 private key.
- Configure ORB_VC_SIGN_PRIVATE_KEYS with new value of ed25519 private key as base64.
- Change the active key id to new key id.

#### Example VC sign key
```
ORB_KMS_ENDPOINT=https://orb.kms
ORB_VC_SIGN_PRIVATE_KEYS=orbkey1=9kRTh70Ut0MKPeHY3Gdv/pi8SACx6dFjaEiIHf7JDugPpXBnCHVvRbgdzYbWfCGsXdvh/Zct+AldKG4bExjHXg,orbkey2=bwpFhQXFhhPQCkAt3fmj9t05hnuwVqiUkBjaXV9QBeisrjoFhUEcIzVOH6QoIXNptWZtOZNdEvlLAf6bZa8opg
ORB_VC_SIGN_ACTIVE_KEY_ID=orbkey2
```

#### Steps to rotate HTTP sign key
- Create new ed25519 private key.
- Replace old key in ORB_HTTP_SIGN_PRIVATE_KEY with new value of ed25519 private key as base64.
- Change the active key id to new key id.

#### Example HTTP sign key
```
ORB_KMS_ENDPOINT=https://orb.kms
ORB_HTTP_SIGN_PRIVATE_KEYS=orbkey2=bwpFhQXFhhPQCkAt3fmj9t05hnuwVqiUkBjaXV9QBeisrjoFhUEcIzVOH6QoIXNptWZtOZNdEvlLAf6bZa8opg
ORB_HTTP_SIGN_ACTIVE_KEY_ID=orbkey2
```

### Create private key in KMS and configure orb to use
When you need to rotate the key just create new key in KMS and add key id to orb configuration.

#### Steps to rotate VC sign key
- Create new ed25519 key in KMS using KMS [cli](../kms/cli.html).
- Configure ORB_VC_SIGN_KEYS_ID with key id.
- Change the active key id to new key id.

#### Example VC sign key
```
ORB_KMS_STORE_ENDPOINT=https://orb.kms/keystore
ORB_VC_SIGN_KEYS_ID=orbkey1,orbkey2
ORB_VC_SIGN_ACTIVE_KEY_ID=orbkey2
```

#### Steps to rotate HTTP sign key
- Create new ed25519 key in KMS using KMS [cli](../kms/cli.html).
- Change the active key id to new key id.

#### Example HTTP sign key
```
ORB_KMS_STORE_ENDPOINT=https://orb.kms/keystore
ORB_HTTP_SIGN_ACTIVE_KEY_ID=orbkey2
```

## Distribute
The key will be managed and stored in KMS please refer to this [doc](../kms/keys.md) for more details.
Orb is using Scenario 1 in kms doc.

### Impact of loss
- Http signatures it's short-lived no impact of loss.
- Can't verify VC signed before loss.

### Impact of compromise
- TBD
