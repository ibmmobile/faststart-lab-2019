# Run the app on Android phone

## Enable developer options and USB debugging on your Android phone

* Enable USB debugging on your Android phone as per the steps in [https://developer.android.com/studio/debug/dev-options.html](https://developer.android.com/studio/debug/dev-options.html)
    - Launch the Settings app on your phone. Select `About Device` -> `Software Info`. Tap `Build number` 7 times to enable developer options.
    - Return to Settings list. Select `Developer options` and enable `USB debugging`.
* If you are developing on Windows, then you need to install the appropriate USB driver as per instructions in [https://developer.android.com/studio/run/oem-usb.html](https://developer.android.com/studio/run/oem-usb.html).
* Connect the Android phone to your development machine by USB cable, and accept `allow` access on your phone.

## Setup API keys for using Google Maps

* Get an API key for using the Google Maps Android API as per instructions in [https://developers.google.com/maps/documentation/android-api/signup](https://developers.google.com/maps/documentation/android-api/signup).

    You can similarly get an API key for using the Google Maps SDK for iOS as per instructions in 
[https://developers.google.com/maps/documentation/ios-sdk/get-api-key](https://developers.google.com/maps/documentation/ios-sdk/get-api-key).

* Add the Google Maps API keys into the `MyWard` app as below:

    ```
    $ cd ../../IonicMobileApp/
    $ ionic cordova plugin add cordova-plugin-googlemaps --variable API_KEY_FOR_ANDROID="<Your_API_key_for_using_GoogleMaps_Android_API>" --variable API_KEY_FOR_IOS="<Your_API_key_for_using_GoogleMaps_SDK_for_iOS>" PLAY_SERVICES_VERSION=15.0.1
    ```

    >**Note**: Make sure you specify the latest PLAY_SERVICES_VERSION in the command above.

## Enable Android platform for Ionic application

* Add [Cordova platform for Android](https://cordova.apache.org/docs/en/latest/guide/platforms/android/)
    ```
    $ cd ../IonicMobileApp
    $ ionic cordova platform add android@6.3.0
    > cordova platform add android@6.3.0 --save
    ...
    ```
    >**Note**: Make sure the Cordova platform version being added is supported by the MobileFirst plug-ins. Site https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/ lists the supported levels.
    ```
    $ cordova platform version
    Installed platforms:
      android 6.3.0
    Available platforms: 
      blackberry10 ~3.8.0 (deprecated)
      browser ~4.1.0
      ios ~4.4.0
      osx ~4.0.1
      webos ~3.7.0
    ```

## Register the MyWard app to MFP server

```
$ cd ../IonicMobileApp
$ mfpdev app register
Verifying server configuration...
Registering to server:'https://mobilefoundation-71-hb-server.mybluemix.net:443' runtime:'mfp'
Updated config.xml file located at: .../Ionic-MFP-App/IonicMobileApp/config.xml
Run 'cordova prepare' to propagate changes.
Registered app for platform: android
```

>**Note**: At the time of creating a mobile foundation service, if you specified `No` to `Make this server the default?`, then you need to specify the name of your server profile (`Cloud-MFP` in our case) at the end of `mfpdev app register` command as shown below:
>```
>$ mfpdev app register Cloud-MFP
>```

Propagate changes by running `cordova prepare`
```
$ ionic cordova prepare
```

## Build/Run the Ionic application on Android phone

* Build Android application
    ```
    $ cd ../IonicMobileApp
    $ ionic cordova build android
    ```

    >**Note**: In case the Cordova build fails due to missing `ANDROID_HOME` and `JAVA_HOME` environment variables, then set those environment variables as per instructions in https://cordova.apache.org/docs/en/latest/guide/platforms/android/#setting-environment-variables. `ANDROID_HOME` should be set to the `Android SDK Location` that you noted during [Install Android Studio and Android SDK platform](#install-android-studio-and-android-sdk-platform). Command `/usr/libexec/java_home` returns the value to be used for setting `JAVA_HOME` on [macOS](http://mattshomepage.com/articles/2016/May/22/java_home_mac_os_x/). On other platforms you could run `java -XshowSettings:properties 2>&1 | grep 'java.home'` as mentioned [here](http://sbndev.astro.umd.edu/wiki/Finding_and_Setting_JAVA_HOME#Sample_Perl_Script:_java_home).

* Run application on Android device
    ```
    $ ionic cordova run android
    ```

    <img src="images/MyWardAppLoginPage.png" alt="MyWard App - Login Page" width="240" border="10" />

    <p><img src="images/MyWardAppHomePage.png" alt="MyWard App - Home Page" width="240" border="10" /> <img src="images/MyWardAppDetailPage.png" alt="MyWard App - Problem Detail Page" width="240" border="10" /> <img src="images/MyWardAppReportNewPage.png" alt="MyWard App - Report New Problem Page" width="240" border="10" /> </p>


## Update App Logo and Splash

Refer to Automating Icons and Splash Screens [https://blog.ionic.io/automating-icons-and-splash-screens/](https://blog.ionic.io/automating-icons-and-splash-screens/) for automating icon and splash screens.

Copy your desired app icon to `IonicMobileApp/resources/icon.png` and app splash to `IonicMobileApp/resources/splash.png`.

```
$ cd ../IonicMobileApp
$ ionic cordova resources
```

For running `ionic cordova resources` command, you would need to sign up on [ionicframework.com](https://ionicframework.com/) and specify the credentials on the command line.

## Build APK for uploading to Google Play Store

Refer [https://ionicframework.com/docs/intro/deploying/](https://ionicframework.com/docs/intro/deploying/) for further details on building and uploading apk to google play store.

* Add following lines at the end of `IonicMobileApp/platforms/android/proguard-project-mfp.txt`:
    ```
    -dontwarn okhttp3.internal.huc.**
    -keep class plugin.google.maps.** { *; }
    ```

* Create release build as below:
    ```
    $ cd ../IonicMobileApp
    $ ionic cordova build android --prod --release
    ```

* Set `ANDROID_HOME` environment variable as per instructions in [here](#buildrun-the-ionic-application-on-android-phone). On Mac, this is usually:
    ```
    export ANDROID_HOME=/Users/<username>/Library/Android/sdk
    ```

* Zip align release build as below:
    ```
    $ cd ./platforms/android/build/outputs/apk/
    $ ls
    android-release-unsigned.apk
    $ $ANDROID_HOME/build-tools/23.0.3/zipalign -v -p 4 android-release-unsigned.apk android-release-unsigned-aligned.apk
    $ ls 
    android-release-unsigned-aligned.apk	android-release-unsigned.apk
    ```  

* Create self signing certificate as below:

    Make a note of the `Keystore password` that you set. You would need it for signing your APK.

    ```
    $ keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-alias

    Enter keystore password:
    Re-enter new password: 
    What is your first and last name?
      [Unknown]:  Shiva Kumar H R
    What is the name of your organizational unit?
      [Unknown]:  ISL
    What is the name of your organization?
      [Unknown]:  IBM
    What is the name of your City or Locality?
      [Unknown]:  Bangalore
    What is the name of your State or Province?
      [Unknown]:  Karnataka
    What is the two-letter country code for this unit?
      [Unknown]:  IN
    Is CN=Shiva Kumar H R, OU=ISL, O=IBM, L=Bangalore, ST=Karnataka, C=IN correct?
      [no]:  yes

    Generating 2,048 bit RSA key pair and self-signed certificate (SHA256withRSA) with a validity of 10,000 days
    	for: CN=Shiva Kumar H R, OU=ISL, O=IBM, L=Bangalore, ST=Karnataka, C=IN
    Enter key password for <my-alias>
    	(RETURN if same as keystore password):  
    [Storing my-release-key.jks]
    
    $ ls
    android-release-unsigned-aligned.apk	android-release-unsigned.apk		my-release-key.jks
    ```

* Self sign APK as below:
    ```
    $ $ANDROID_HOME/build-tools/23.0.3/apksigner sign --ks my-release-key.jks --out myward.apk android-release-unsigned-aligned.apk
    Keystore password for signer #1: 
    $ ls
    android-release-unsigned-aligned.apk	my-release-key.jks
    android-release-unsigned.apk		myward.apk
    $ 
    ```

* Distribute `myward.apk` by uploading to Google Play Store or to your company's internal App store.

