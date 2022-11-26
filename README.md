# Private ID Android library - CryptoNets™

Powered by Private Identity® - https://private.id

CryptoNets™ is extremely fast, accurate and efficient face, voice and fingerprint identity Android library with full
privacy.

## Benefits

- Face biometric capture
- Encrypted face recognition every 200ms
- 1:N biometric match in 60ms constant time
- Human age estimation
- Unlimited users (unlimited gallery size)
- Fair, accurate and unbiased
- Preserves user privacy with neural network cryptography + fully homomorphic encryption (CryptoNets)
- IEEE 2410 Standard for Biometric Privacy, ISO 27001, ISO 9001 compliant
- Exempt from GDPR, CCPA, BIPA, and HIPAA privacy law obligations
- Predicts in 50ms with or without network using local storage

## How do I use CryptoNets™ Android library?

### Prerequisite
- Sign up on the waitlist on https://private.id to obtain your API Key.

### How To Install
- Use Gradle

```groovy
// project's gradle or settings.gradle
repositories {
    maven { url 'https://jitpack.io' }
}

// app's gradle
implementation 'com.github.openinfer:cryptonets-lib:1.1'
```

- Or download and add the `*.AAR` manually from this repository.

### Implementation
- Warning: all methods except the `initialization` process should be called off UI thread to avoid ANR issue.

#### Initialization:
- initialize the library once only in your application class
```kotlin
val config = PrivateIdentityConfig
        .Builder(context, "Your api key")
        .serverUrl("Your server url") //optional
        .localStorage("Your storage path") // optional - default is your internal app's folder
        .logEnabled(true) // optional - default is false
        .build()
val pi = PrivateIdentity(config)
```
#### ImageData
- This is the class that converts `Bitmap` to `bytes` to interact with the library. 

```kotlin
val bitmap = yourLoadImageMethod()
val imageData = ImageData(bitmap) // this should be call off UI thread to avoid ANR
```

#### Validate Input Image

- The function detects if there is a valid face in the 'ImageData` object:
```kotlin
val faceValidateResult = privateIdentity.validate(imageData)
```
- In `faceValidateResult`, we have `ageFactor` and `faceValidation`:

```java
public enum FaceValidation {
  InvalidImage(-100),
  NoFace(-1),
  ValidBiometric(0),
  ImageSpoof(1),
  VideoSpoof(2),
  TooClose(3),
  TooFaraway(4),
  TooFarToRight(5),
  TooFarToLeft(6),
  TooFarUp(7),
  TooFarDown(8),
  TooBlurry(9),
  GlassesOn(10),
  MaskOn(11),
  ChinTooFarLeft(12),
  ChinTooFarRight(13),
  ChinTooFarUp(14),
  ChinTooFarDown(15),
  ServerError(27),
}
```

#### Authenticate/Predict A User
- The function detects if there is a valid face in the `ImageData` object and tries to authenticate a user on back-end
```kotlin
val facePredictResult = privateIdentity.predict(imageData)
// Result:
// - FacePredictResult - return null means invalid image
// - FacePredictResult - empty means user is not registered
// - FacePredictResult contains uuid, guid if the user is registered
```

#### Register A New User
- Before registering a new user, we should check whether the image is valid or not:
```kotlin
val faceValidateResult = privateIdentity.validateToEnroll(imageData)
if (faceValidateResult.faceValidation == FaceValidation.ValidBiometric) { // image is valid
  val faceEnrollResult = privateIdentity.enroll(imageDataList) // the imageDataList must be not empty
  // faceEnrollResult - returns null means failed
  // faceEnrollResult contains uuid, guid if the process is running successfully
}
```
- Note: we can `enroll` a new user with a single image, but it's recommended to enroll with more than 5 images.

#### Delete A User Data
If you want to delete user data from the back-end you can do it using the user identifier (uuid) previously obtained from `enroll` or `predict` method:
```kotlin
val faceDeleteResult = privateIdentity.delete(uuid)
// FaceDeleteResult - returns null if failed
// FaceDeleteResult object with status and message
```

## Compatibility

- <b>Min Android SDK</b>: CryptoNets™ requires a minimum API level of 21
- <b>Compile Android SDK</b>: CryptoNets™ requires you to compile against API 32 or lower. API 33 is not supported now.


## Samples

This repository contains all available samples: https://github.com/openinfer/samples



