# Private ID WebAssembly SDK

Powered by Private Identity® - https://private.id

CryptoNets™ is extremely fast, accurate and efficient face, voice and fingerprint identity Android library with full
privacy.

## Benefits

---

- Face biometric capture
- Encrypted face recognition
- 1:n biometric match in 60ms constant time
- Unlimited users (unlimited gallery size)
- Fair, accurate and unbiased
- Preserves user privacy with neural network cryptography + fully homomorphic encryption (CryptoNets)
- IEEE 2410 Standard for Biometric Privacy, ISO 27001, ISO 9001 compliant
- Exempt from GDPR, CCPA, BIPA, and HIPAA privacy law obligations
- Predicts in 50ms with or without network using local storage

## Download

---

Use Gradle

```groovy
// project's gradle or settings.gradle
repositories {
    maven { url 'https://jitpack.io' }
}

// app's gradle
implementation 'com.github.openinfer:cryptonets-lib:1.0'
```

Or download and add the `*.AAR` manually from this repository.

## How do I use CryptoNets™ Android library?

---

- Initialize the library once only
```kotlin
val fileStoragePath = context.filesDir.absolutePath + "/your_folder/"
val prividFheFace = PrividFheFace(fileStoragePath, context) 
prividFheFace.FHEConfigureUrl("https://devel.private.id/node/", 42)
prividFheFace.FHEConfigureUrl("00000000000000001962", 46)
```

- Load image as `bitmap` and convert the bitmap to `ImageRawDataInfo`:
```kotlin
val bitmap = yourLoadImageMethod()
val imageInfo = CommonMethods.getImageDetailLib(it, orientation) // ImageRawDataInfo
```

- Use  `ImageRawDataInfo` with the `prividFheFace` instance:

  - Check image's validity:
    ```kotlin
    val responseIsValid = prividFheFace.isValid(imageRawDataInfo.imageData, imageRawDataInfo.width, imageRawDataInfo.height, 0)
    if (responseIsValid.status == 0) {
        // valid
    } else {
        // invalid
    }
    ```
  - Predict user:
    ```kotlin
    val responsePredict = prividFheFace.predict(imageRawDataInfo.imageData, imageRawDataInfo.width, imageRawDataInfo.height)
    if (responsePredict.status == 0) { 
        // user exists
        val uuid = responsePredict.piModel.uuid
        val guid = responsePredict.piModel.guid
    } else {
        // no user
    }
    ```
  - Enroll user:
    
    - We need 10 valid images to enroll
    ```kotlin
    private val enrollImageByteArray = arrayListOf<ByteArray>() // array of images
    // check images' validity
    val responseIsValid = prividFheFace.isValid(imageRawDataInfo.imageData, imageRawDataInfo.width, imageRawDataInfo.height, 1)
    if (responseIsValid.status == 0) {
        enrollImageByteArray.add(imageRawDataInfo.imageData)
        if (enrollImageByteArray.size == 10) { // enroll after 10 valid images
            val enrollResult = prividFheFace.enroll(enrollImageByteArray, imageRawDataInfo.height, imageRawDataInfo.width, imageRawDataInfo.byteCount)
            if (enrollResult.status == 0) {
                val guid = enrollResult.piModel.guid
                val uuid = enrollResult.piModel.uuid
            } else {
                // enroll failed
            }
        }
    }
    ```
    
  - Delete user:
    ```kotlin
    val deleteResult = prividFheFace.delete(uuid)
    if (deleteResult.status == 0) {
        // success
    } else {
        // failed
    }
    ``` 

## Compatibility

---

- <b>Min Android SDK</b>: CryptoNets™ requires a minimum API level of 21
- <b>Compile Android SDK</b>: CryptoNets™ requires you to compile against API 32 or lower. API 33 is not supported now.


## Samples

---

This repository contains all available samples: https://github.com/openinfer/samples



