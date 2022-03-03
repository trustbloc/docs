# Key
Orb use one crypto key to do the following operations.

- Sign http requests between orb servers.
- Sign VC.

## Creation
There is two ways to create orb key.

### Private key
When Orb server start will import the private key and id into KMS sever.

### Steps to create key
- Create ed25519 private key.
- Configure orb with value of ed25519 private key as base64.

#### Example Orb Configuration
```
ORB_KMS_ENDPOINT=https://orb.kms
ORB_PRIVATE_KEYS=orbkey1=9kRTh70Ut0MKPeHY3Gdv/pi8SACx6dFjaEiIHf7JDugPpXBnCHVvRbgdzYbWfCGsXdvh/Zct+AldKG4bExjHXg
ORB_ACTIVE_KEY_ID=orbkey1
```

### KMS key id
Key need to be created in KMS before orb server starting.

#### Steps to create key
- Create KMS keystore using KMS cli.
- Create ed25519 key in KMS using KMS cli.
- Configure orb with key id.

#### Example Orb Configuration
```
ORB_KMS_STORE_ENDPOINT=https://orb.kms/keystore
ORB_KEYS_ID=orbkey1
ORB_ACTIVE_KEY_ID=orbkey1
```

## Storing
The key will be managed and stored in KMS please refer to this [doc](../kms/keys.md) for more details.
Orb is using Scenario 1 in kms doc.

## Rotation
There is two ways to rotate orb key.

### Private key
When you need to rotate the key just add the new private key to orb configuration.

#### Steps to rotate
- Create new ed25519 private key.
- Configure orb with new value of ed25519 private key as base64.
- Change the active key id to new key id.

#### Example Orb Configuration
```
ORB_KMS_ENDPOINT=https://orb.kms
ORB_PRIVATE_KEYS=orbkey1=9kRTh70Ut0MKPeHY3Gdv/pi8SACx6dFjaEiIHf7JDugPpXBnCHVvRbgdzYbWfCGsXdvh/Zct+AldKG4bExjHXg,orbkey2=bwpFhQXFhhPQCkAt3fmj9t05hnuwVqiUkBjaXV9QBeisrjoFhUEcIzVOH6QoIXNptWZtOZNdEvlLAf6bZa8opg
ORB_ACTIVE_KEY_ID=orbkey2
```

### KMS key id
When you need to rotate the key just create new key in KMS and add key id to orb configuration.

#### Steps to rotate
- Create new ed25519 key in KMS using KMS cli.
- Configure orb with key id.
- Change the active key id to new key id.

#### Example Orb Configuration
```
ORB_KMS_STORE_ENDPOINT=https://orb.kms/keystore
ORB_KEYS_ID=orbkey1,orbkey2
ORB_ACTIVE_KEY_ID=orbkey2
```

## Distribute
The key will be managed and stored in KMS please refer to this [doc](../kms/keys.md) for more details.
Orb is using Scenario 1 in kms doc.

### Impact of loss
- Http signatures it's short-lived no impact of loss.
- Can't verify VC signed before loss.

### Impact of compromise
- TBD
