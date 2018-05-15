# Android Build setup

In Android, builds are done via Gradle.

These are a few pre-requisites to build a release apk.

* **BUILD\_NAME** - The name that will be used by testers to identify the build, for example: '1.1.1', '1.0-alpha', etc.
* **BUILD\_NUMBER** - A unique integer number identifying the build. This is used by Android to identify which build is the updated build. This should be an integer number, for example, 1, 111, 111, etc.
* **ANDROID\_APP\_ID** - This is the unique app identifier which is used to identify the app uniquely in the Google Play Store or can be used to identify if the build is dev, preprod or prod. These app ids may look like this: com.app.notetaker-dev, com.app.notetaker-alpha.
* **ANDROID\_KEYSTORE\_FILE** - This is the keystore file used to sign the app.
* **ANDROID\_KEYSTORE\_PASSWORD** - This is the keystore password used while creating the keystore.
* **ANDROID\_KEY\_ALIAS** - This is the alias used to create the keystore.
* **ANDROID\_KEY\_PASSWORD** - This is the password set for the key.

## Release variants

Ideally, every app has three release variants just like a typical backend application:

* Dev build - The app which connects to the staging/dev backend environment. This can also include additional libraries like TestFairy.
* Preprod build - The app which points to the preprod backend environment. This is usually very similar, if not identical to the production app.
* Prod build - The final apk which should be released to the Play Store.

Hence, we would need three different key stores for three different variants.

## Let's get started

The first step is to create a new keystore file.

Use the following command to create a keystore.

```bash
keytool -genkey -v -keystore dev_release.keystore -alias dev-alias -keyalg RSA -keysize 2048 -validity 10000
```

You would be prompted to enter ANDROID\_KEYSTORE\_PASSWORD, ANDROID\_KEY\_ALIAS, ANDROID\_KEY\_PASSWORD and a few other details.

Note these down somewhere and keep the keystore file safe.

The Android documentation has the following warning:

```text
Note about saving the keystore:

Once you publish the app on the Play Store, you will need to republish your app under a different package name (losing all downloads and ratings) if you want to change the signing key at any point. So backup your keystore and don't forget the passwords.
```

For the build to work the keystore file should be placed at `android/app/dev_release.keystore`.

_Make sure the keystore file is git ignored so that you don't check the file into git_

Now, create the following script.

> scripts/android/builder.sh

```bash
#!/bin/bash

set -e
cur_dir=`dirname $0`

echo "BUILDING ANDROID";
cd $cur_dir/../../android &&
./gradlew clean assembleRelease -PBUILD_NAME=$BUILD_NAME -PBUILD_NUMBER=$BUILD_NUMBER -PANDROID_APP_ID=$ANDROID_APP_ID -PMYAPP_RELEASE_STORE_FILE=$ANDROID_KEYSTORE_FILE -PMYAPP_RELEASE_KEY_ALIAS=$ANDROID_KEY_ALIAS -PMYAPP_RELEASE_STORE_PASSWORD=$ANDROID_KEYSTORE_PASSWORD -PMYAPP_RELEASE_KEY_PASSWORD=$ANDROID_KEY_PASSWORD && cd ..

echo "APK will be present at android/app/build/outputs/apk/app-release.apk"
```

Apart from this script, we would also need to modify:

> android/app/build.gradle

```text
...
def enableProguardInReleaseBuilds = false
...
...
def appID = System.getenv("ANDROID_APP_ID") ?: "com.notetaker"
def vCode = System.getenv("BUILD_NUMBER") ?: "0"
def vName = System.getenv("BUILD_NAME") ?: "1.0.local"

android {
    compileSdkVersion 23
    buildToolsVersion "23.0.1"
    defaultConfig {
        applicationId appID
        minSdkVersion 16
        targetSdkVersion 22
        versionCode Integer.parseInt(vCode)
        versionName vName
        ndk {
            abiFilters "armeabi-v7a", "x86"
        }
    }
  ...
  ...
  ...
    signingConfigs {
       release {
           if (project.hasProperty('MYAPP_RELEASE_STORE_FILE')) {
               storeFile file(MYAPP_RELEASE_STORE_FILE)
               storePassword MYAPP_RELEASE_STORE_PASSWORD
               keyAlias MYAPP_RELEASE_KEY_ALIAS
               keyPassword MYAPP_RELEASE_KEY_PASSWORD
           }
       }
   }
    buildTypes {
      release {
          minifyEnabled enableProguardInReleaseBuilds
          proguardFiles getDefaultProguardFile("proguard-android.txt"), "proguard-rules.pro"
          signingConfig signingConfigs.release
      }
    }
...
...
...
```

The above script tells Gradle to 1. clean 2. build a release apk

Also, all the important parameters are passed via environment variables. This ensures that we can: 1. Build the apk manually by running

```bash
  BUILD_NAME=1.1.1 BUILD_NUMBER=11 ANDROID_APP_ID='com.test.appid' ANDROID_KEYSTORE_FILE='dev_release.keystore' ANDROID_KEY_ALIAS='dev-alias' ANDROID_KEYSTORE_PASSWORD=<PASSOWORD> ANDROID_KEY_PASSWORD=<PASSWORD> sh ./scripts/android/builder.sh
```

1. Setup the environment variables in CI platform so that the CI can build the apk without manual intervention. Keep in mind that typically every CI provides a unique build number that you can pass to the script for BUILD\_NUMBER.

Woot! Its that simple to build a release apk for Android.

The built apk can be found at: `android/app/build/outputs/apk/app-release.apk`

## Windows users

If you are running Windows 10 64bit or higher you can enable Ubuntu bash shell on your systems and gain access to the full bash command line and run the script there. More on that here: [https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/)

Alternatively, you could install Cygwin on your system and run the scripts mentioned.

The code till here can be found on the **branch** [chapter/11/11.1](https://github.com/react-made-native-easy/note-taker/tree/chapter/11/11.1)

