# Deploy the MFP Adapters

## Build and Deploy the MFP adapters

### Build and deploy `UserLogin` adapter as below.

```
$ cd MobileFoundationAdapters/
$ cd UserLogin
$ mfpdev adapter build
$ mfpdev adapter deploy
```

>**Note**: If you have specified `No` to `Make this server the default?` at the time of creating a mobile foundation service, then you need to specify the name of your server profile (`Cloud-MFP` in our case) at the end of `mfpdev adapter deploy` command as shown below.
>```
>$ mfpdev adapter deploy Cloud-MFP
>```

### Build and deploy `MyWardData` adapter as below.

```
$ cd ../MyWardData/
$ mfpdev adapter build
$ mfpdev adapter deploy
```

## Launch MFP dashboard and verify adapter configurations

* Launch MFP Dashboard as below:
    * In the [IBM Cloud dashboard](https://cloud.ibm.com/dashboard/), under `Cloud Foundry Services`, click on the `Mobile Foundation` service you created. The service overview page that gets shown, will have the MFP dashboard embedded within it. You can also open the MFP dashboard in a separate browser tab by appending `/mfpconsole` to the *url* mentioned at the time of creating the mobile foundation service.
    * Inside the MFP dashboard, in the list on the left, you will see the `MyWardData` and `UserLogin` adapters listed.

* Verify MFP Adapter configuration as below:
    * Inside the MFP dashboard, click on the `MyWardData` adapter. Under `Configurations` tab, you should see the various properties for accessing Cloudant database and Cloud Object Storage as shown below. As an alternative to specifying those property values in `MobileFoundationAdapters/MyWardData/src/main/adapter-resources/adapter.xml`, you can deploy the adapters with empty `defaultValue`, and once the adapter is deployed, change the values on this page.

        <img src="images/MyWardDataConfigurations.png" alt="Option to specify the configuration properties for accessing Cloudant NoSQL DB and Cloud Object Storage in deployed MFP Adapter" width="640" border="10" />

    * Click on `Resources` tab. You should see the various REST APIs exposed by `MyWardData` adapter as shown below. The `Security` column should show the protecting scope `UserLogin` against each REST method.
    
        <img src="images/MyWardDataProtectingScope.png" alt="The REST APIs of MyWardData adapter are protected by UserLogin security scope" width="640" border="10" />
