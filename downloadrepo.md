# Download source repo and customize

## Clone repo

```
$ git clone https://github.com/IBM/Ionic-MFP-App.git
$ cd Ionic-MFP-App
```

## Update App ID, Name and Description

Update `IonicMobileApp/config.xml` as below. Change `id`, `name`, `description` and `author` details appropriately.

```
<?xml version='1.0' encoding='utf-8'?>
<widget id="org.mycity.myward" version="0.0.1" xmlns="http://www.w3.org/ns/widgets" xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:mfp="http://www.ibm.com/mobilefirst/cordova-plugin-mfp">
    <name>MyWard</name>
    <description>Get your civic issues resolved by posting through this app.</description>
    <author email="shivahr@gmail.com" href="https://developer.ibm.com/code/author/shivahr/">Shiva Kumar H R</author>
...
```

## Install Android SDK Platform 23 (or higher)

* Launch Android Studio.
* Click on `Configure` -> `SDK Manager`
* Make a note of the `Android SDK Location`.
* Under `SDK Platforms`, select `Android 6.0 (Marshmallow) API Level 23` or higher. Click `Apply` and then click `OK`. This will install Android SDK Platform on your machine.

* Edit `IonicMobileApp/config.xml` and specify the API level in `android-targetSdkVersion` as shown below.

    ```
    <preference name="android-minSdkVersion" value="16" />
    <preference name="android-targetSdkVersion" value="23" />
    ```

## Specify Cloudant credentials in MFP adapter

* Open `MobileFoundationAdapters/MyWardData/src/main/adapter-resources/adapter.xml` and update the following properties to point to the Cloudant database created you have created [earlier](/install.md#create-cloudant-database).
* Update `key` and `password` with the Cloudant API key as [generated](/install.md#generate-cloudant-api-key).
* For property `account`, specify the Cloudant Dashboard URL portion upto (and including) *-bluemix.cloudant.com* as shown in the [snapshot](/install.md#generate-cloudant-api-key).
 * For property `DBName`, leave the default value of `myward` as-is.

    ```
    <mfp:adapter name="MyWardData" ...>
      <property name="account" displayName="Cloudant account" defaultValue=""/>
      <property name="key" displayName="Cloudant key" defaultValue=""/>
      <property name="password" displayName="Cloudant password" defaultValue=""/>
      <property name="DBName" displayName="Cloudant DB name" defaultValue="myward"/>
      ...
    </mfp:adapter>
    ```

## Specify Cloud Object Storage credentials in MFP Adapter

* Open `MobileFoundationAdapters/MyWardData/src/main/adapter-resources/adapter.xml` and update the following properties to point to the Cloud Object Storage created [earlier](/install.md#create-ibm-cloud-object-storage-service-and-populate-it-with-sample-data).
* Specify value for `bucketName` as created [earlier](/install.md#create-ibm-cloud-object-storage). 
* Specify `serviceId` and `apiKey` created [earlier](/install.md#create-service-id-and-api-key-for-accessing-objects).
* While creating the bucket in IBM Cloud Object Storage, if you selected a different Location/Resiliency, then update the `endpointURL` as per the specification in https://cloud.ibm.com/docs/services/cloud-object-storage/basics/endpoints.html#select-regions-and-endpoints.

```
<mfp:adapter name="MyWardData" ...>
  ...
  <property name="endpointURL" displayName="Cloud Object Storage Endpoint Public URL" defaultValue="https://s3-api.us-geo.objectstorage.softlayer.net"/>
  <property name="bucketName" displayName="Cloud Object Storage Bucket Name" defaultValue=""/>
  <property name="serviceId" displayName="Cloud Object Storage Service ID" defaultValue=""  />
  <property name="apiKey" displayName="Cloud Object Storage API Key" defaultValue=""/>
</mfp:adapter>
```
