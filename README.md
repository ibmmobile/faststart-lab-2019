# Develop Enterprise-grade Hybrid Mobile App with Cloud-native Backend

When developing your enterprise mobile app that needs centralized hosting of data, use of cloud-native services such as [Cloudant No-SQL Database](https://www.ibm.com/cloud/cloudant) for storing textual data and [Object Storage service](https://www.ibm.com/cloud/object-storage) for storing image/video/audio data, allows you to quickly go from idea-conception to reality. The [Mobile Foundation service](https://www.ibm.com/cloud/mobile-foundation) available from IBM Cloud provides a scalable mobile access gateway for securely accessing those backend services, and it provides other essential mobile backend capabilities such as push notifications, app lifecycle management and app analytics.

This code pattern gives you step by step instructions for developing an [Ionic/Cordova](https://ionicframework.com/) based hybrid mobile app that securely connects to Cloudant and Object Storage services via IBM Mobile Foundation (aka MFP) service.

When you have completed this pattern, you will understand:
* How to authenticate users (through preemptive login) using MFP security adapter.
* How to write an MFP adapter that authenticates with Cloud Object Storage service and pass back the Authorization token to the mobile app.
* Recommended architectural patterns for coding the interaction between mobile app and Cloud Object Storage service.
* How to use [`imgcache.js`](https://github.com/chrisben/imgcache.js/) in Ionic app for caching images fetched from Cloud Object Storage service.
* How to show Google Maps in Ionic app as well as capture user’s geo-location & image from camera.
* How to upload the captured image from mobile app to Cloud Object Storage service.
* How to fetch data from Cloudant service to mobile app via MFP adapter as well as post new data to Cloudant.
* How to customize the Ionic app logo and splash, and build the release apk for uploading to public/private app stores.

## Pre-requisites

* Node.js
* Cordova
* Ionic
* Mobile Foundation CLI
* git
* Maven (for building the application)
* Java SDK (for building the application)
* Atom
* Android Studio
* Android SDK
* IBM Cloud account credentials
* IBM Mobile Foundation Service access credentials
* IBM Cloud NoSQL DB service

## Next Steps

* [Installation and Configuration](install.md)
* [Download source repo and customize](downloadrepo.md)
* [Build and deploy MFP adapters](builddeploy.md)
* [Run application on Android phone](runtheapp.md)




