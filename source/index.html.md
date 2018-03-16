---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: cURL

  - java: Java
  
toc_footers:
   - <a href='https://device.pcloudy.com/signup' target="_blank">Sign Up for API Key</a>

includes:
  - errors

search: true
---

# Introduction

The pCloudy API is organized around REST. Our API has predictable, resource-oriented URLs, and uses HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which are understood by off-the-shelf HTTP clients.

	
# Authentication

> URL format:

```shell
curl -u "email:apiKey" <Cloud URL>/api/access

Note: 
	<Cloud URL>: Use https://device.pcloudy.com or https://us.pcloudy.com or https://ph.pcloudy.com or https://aus.pcloudy.com in case of public cloud otherwise use your custom cloud URL in case of private or on premise cloud

Ex: curl -u "test.user@pcloudy.com:3m4t8y47mmrz8zycmqbr3f" https://device.pcloudy.com/api/access

Make sure to replace `test.user@pcloudy.com:3m4t8y47mmrz8zycmqbr3f` with your Email and API key.

```
 

```java
Download the pCloudy java connector jar from the following link- http://pcloudy-content-distribution.s3.amazonaws.com/index.html

Connector con = new Connector("https://device.pcloudy.com");
String authToken = con.authenticateUser(emailid, apiKey);

Note: 
	Replace the 'https://device.pcloudy.com' with https://us.pcloudy.com or https://ph.pcloudy.com or https://aus.pcloudy.com in case of public sub clouds or your custom cloud URL in case of private or on premise cloud.
	
```



Authenticate your account when using the API by including your email-Id and secret API key(access Key) in the request. You can manage your API keys in the Account settings. Your API keys carry many privileges, so be sure to keep them secret! Do not share your secret API keys in publicly accessible areas such GitHub, client-side code, and so forth.
	Authentication to the API is performed via HTTP Basic Auth. Provide your API key as the basic auth username value. You do not need to provide a password.

	All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

<b>Note:</b> If you are using the Java,Dowload the pCloudy java connector jar from the link-
 <a href="http://pcloudy-content-distribution.s3.amazonaws.com/index.html" target = "_blank"> pCloudy java Connector</a>

###
Parameter |  Description
--------- |  -----------
email | your email id
accessKey | your access key(you will get it from Account Settings->API)



# Generic 

## Get User Details

```shell
URL format:
  
curl -H "Content-Type: application/json" -d '{"token": "authToken"}' <Cloud URL>/api/get_user_details
                                        		          
Ex: 
curl -H “Content-Type:application/json” -d ‘{“token”:”swqkm7f55v39g9kkdggkzs”}’ https://device.pcloudy.com/api/get_user_details
```

```java
UserDetailResult userDetail = con.getUserDetails(authToken);
```

You can get the total account details like userId, email_id, username, account balance, etc., by passing the authtoken(this will get when authentication response).


### Query Parameters

Parameter |  Description
--------- |  -----------
authToken | Authtoken(this will get from authenticate response)


## Upload App

```shell
URL format:
	curl -X POST -F "file=@/File_Path" -F "source_type=raw" -F "token=authToken" -F "filter=filter" <Cloud URL>/api/upload_file
Ex:
	curl -X POST -F "file=@/home/user/Downloads/pCloudy_Appium_Demo.apk" -F "source_type=raw" -F "token=swqkm7f55v39g9kkdggkzs" -F "filter=all" https://device.pcloudy.com/api/upload_file
```

```java
PDriveFileDTO uploadedApp = con.uploadApp(authToken, fileToUpload);

```


You can upload the apps to pcloudy cloud drive by using upload api. You can upload any file to cloud drive. Uploaded files/apps are available in MY App/Data section.


### URL Parameters

Parameter | Description
--------- | -----------
file | Path of uploaded apk or ipa file
source_type | Mention raw type(raw)
token | Authtoken(this will get from authenticate response)
filter | You can filter the file like apk or ipa when uploading(all/apk/ipa)

## Get Available Apps

```shell
URL Format:
	curl -H "Content-Type:application/json" -d '{"token":"authToken","limit": limit, "filter": "all"}' <Cloud URL>/api/drive
Ex:
	curl -H "Content-Type:application/json" -d '{"token":"swqkm7f55v39g9kkdggkzs","limit": 5, "filter": "all"}' https://device.pcloudy.com/api/drive
```

```java

PDriveFileDTO[] apps = con.getAvailableApps(String authToken);
```


Get Available app will show you total files and apps what ever you uploaded in cloud drive.


### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
limit	  | How many files you want to display.
filter    | It will filter the files and display based on filter option(all/apk/ipa).

## Download File From Cloud
```shell
URL Format:

curl -O -H "Content-Type:application/json" -d '{"token":"TOKEN","filename":"FileName","dir":"data"}' <Cloud URL>/api/download_file


Ex:
	 curl -O -H "Content-Type:application/json" -d '{"token":"swqkm7f55v39g9kkdggkzs","filename":"pCloudy_Appium_Demo.apk","dir":"data"}' https://device.pcloudy.com/api/download_file
```

```java
File file= con.downloadFileFromCloud(authToken, "pCloudy_Appium_Demo.apk", "data");
```
You can download files from cloud drive. This will download the apps into your system.

### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
filename  | File name which you want to download
dir       | Directory name(data).

## Get Device List
```shell
URL Format:

curl -H "Content-Type: application/json" -d '{ "token":"authToken","duration":10, "platform":"platform", "available_now":"true"}' <Cloud URL>/api/device

Ex:
curl -H "Content-Type: application/json" -d '{ "token":"swqkm7f55v39g9kkdggkzs","duration":10, "platform":"android", "available_now":"true"}' https://device.pcloudy.com/api/devices
```

```java
For single device selection:
MobileDevice selectedDevice = con.chooseSingleDevice(authToken, "Android");

For Multiple device selection:

  List<MobileDevice>   selectedDevices = con.chooseMultipleDevices(authToken, "android");
```
This api helps to get the available devices in pcloudy with total information about the each device. In the response each device is having the id which is used for booking the device.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
duration  | Duration in minutes(How many minutes you want to the device).
platform  | Which platform devices(android or ios) you want to display.
available_now | true or false. True means display the available devices and false means display all devices(including busy and available).

## Book Device
```shell
URL Format:
	curl -H 'ContentType:application/json' -d '{"token":"authToken","duration":minutes, "id":device_id}' <Cloud URL>/api/book_device

EX: curl -H 'ContentType:application/json' -d '{"token":"swqkm7f55v39g9kkdggkzs","duration":5, "id":120}' https://device.pcloudy.com/api/book_device
```

```java
BookingDtoResult bookedDevice = con.bookDevice(authToken, 10, selectedDevice);
```
Book Device api book the device for your testing. For device booking you had to pass the authToken, how many minutes you want book and device id. Every device having the different id.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
duration  | Duration in minutes(How many minutes you want to book the device).
id  | Device id(will get from get devices api response).

## Get Device Page URL
```shell
URL Format:
curl -H "Content-Type: application/json" -d '{"token": "authToken","rid":"rid"}' <Cloud URL>/api/get_device_url

Ex: 
	
curl -H "Content-Type: application/json" -d '{"token": "swqkm7f55v39g9kkdggkzs","rid":"482013"}' https://device.pcloudy.com/api/get_device_url
```

```java
URL devicePageURL = con.getDevicePageUrl(authToken, bookedDevice);
```
This will give you the device page url. By this url you can open the device screen directly in the browser. This helps you to connect to the device directly by passing the authToken and rid.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
rid | Reservation id(will get from book device api response).

## Install and Launch app
```shell
URL format:
curl -H "Content-Type: application/json" -d '{"token": "authToken", "rid": "rid", "filename": "filename", "grant_all_permissions":"true/false"}' <Cloud URL>/api/install_app

Ex:
	
curl -H "Content-Type: application/json" -d '{"token": "swqkm7f55v39g9kkdggkzs", "rid": "482013", "filename": "pCloudy_Appium_Demo.apk", "grant_all_permissions":"true"}' https://device.pcloudy.com/api/install_app
```
```java
InstallAndLaunchResultDto result = con.installAndLaunchApp(authToken, bookedDevice, uploadedApp);
```
Install and launch api will install the app(apk/ipa) in the device and launch it on the device. App launch will work on Android devices, but in iOS device app will install but not launch(we are working on this).
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
rid | Reservation id(will get from book device api response)
filename | app name(Need to mention apk/ipa file name which is available in cloud drive)
grant_all_permissions | true/false(This is provide permissions for app). This is optional.

## Execute ADB
```shell
URL format:
curl -H "Content-Type: application/json" -d '{"token": "authToken", "rid": "rid", "adbCommand": "adb Command"}' <Cloud URL>/api/execute_adb	

Ex:
	curl -H "Content-Type: application/json" -d '{"token": "swqkm7f55v39g9kkdggkzs", "rid": "482013", "adbCommand": "adb shell getprop | grep model"}' https://device.pcloudy.com/api/execute_adb
```
```java
String adbdevices =con.executeAdbCommand(authToken, bookedDevice, "adb shell getprop | grep model");
```
This api help to execute the commands on the device. This api works only for Android devices.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
rid | Reservation id(will get from book device api response)
adbCommand | adb command which you want to execute on device.

## Release Device
```shell
URL format:
curl -H "Content-Type:application/json" -d '{"token":"authToken","rid":"rid"}' <Cloud URL>/api/release_device	

Ex:
	curl -H "Content-Type: application/json" -d '{"token": "swqkm7f55v39g9kkdggkzs", "rid": "482013"}' https://device.pcloudy.com/api/release_devices
```
```java
con.releaseInstantAccessBooking(authToken, bookedDevice);
```
This api help you to release the device.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
rid | Reservation id(will get from book device api response)

## Push File
```shell
URL format:
curl -H "Content-Type:applicatio/json" -d '{"token":"authToken","rid":"RID","pDriveFile":"filename"}' <Cloud URL>/api/pushFileToSwapBox	

Ex:
	curl -H "Content-Type: application/json" -d '{"token": "swqkm7f55v39g9kkdggkzs", "rid": "482013", "pDriveFile":"pCloudy_Appium_Demo.apk"}' https://device.pcloudy.com/api/pushFileToSwapBox
```
```java
con.SwapboxAPIs().pushFile(authToken, bookedDevice, uploadedApp);
```
This api push the file or apk from Cloud Drive to the device swapBox.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
rid | Reservation id(will get from book device api response)
pDriveFile | File name which you want to push.


## Pull File
```shell
URL format:
curl -H "Content-Type:applicatio/json" -d '{"token":"TOKEN","rid":"RID","pDriveFile":"filename"}' <Cloud URL>/api/pullFileFromDevice	

Ex:
	curl -H "Content-Type: application/json" -d '{"token": "swqkm7f55v39g9kkdggkzs", "rid": "482013", "pDriveFile":"pCloudy_Appium_Demo.apk"}' https://device.pcloudy.com/api/pullFileFromDevice
```
```java
PDriveFileDTO pDriveFile = con.SwapboxAPIs().pullFile(authToken, bookedDevice, appPathInSwapbox);

```
This api pull the file or apk from device swapBox to the cloud drive.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
rid | Reservation id(will get from book device api response)
pDriveFile | File name which you want to pull.

## Capture Device Screenshot
```shell
URL format:
curl -H "Content-Type:application/json" -d '{"token":"authToken", "rid":"rid", "skin":"true"}' <Cloud URL>/api/capture_device_screenshot
Ex:
curl -H "Content-Type:application/json" -d '{"token":"swqkm7f55v39g9kkdggkzs", "rid":"482013", "skin":"true"}' https://device.pcloudy.com/api/capture_device_screenshot
```
```java
CaptureDeviceScreenshotResultDto dto = con.takeDeviceScreenshot(authToken, Integer.valueOf(bookedDevice.rid), true);

```
This api takes the screen shot of the device. This screenshot helps you to figure out the steps done by automation process.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
rid | Reservation id(will get from book device api response)
skin | true(Means take the screenshot).

## Start Performance Data
```shell
URL format:
curl -H "Content-Type:application/json" -d '{"token":"authToken", "rid":"rid", "pkg":"app package name"}' <Cloud URL>/api/start_performance_data
Ex:
curl -H "Content-Type:application/json" -d '{"token":"swqkm7f55v39g9kkdggkzs", "rid":"482013", "pkg":"com.pcloudy.appiumdemo "}' https://device.pcloudy.com/api/start_performance_data
```
```java
con.startPerformanceData(authToken, bookedDevice.rid, install.packageName);
```
This api help to start the performance data of the app under testing. In this you will get battery, CPU, memory, internet and framerendering data on android device and for iOS will get the CPU, memory and network pockets under testing on that device. This data is stored in cloud drive.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
rid | Reservation id(will get from book device api response)
pkg | App package name (will get from install and launch api response)

## Download Performance Data
```shell
URL format:
curl -H "Content-Type:application/json" -d '{"token":"authToken", "rid":"rid", "filename":"fileName"}' <Cloud URL>/api/download_manual_access_data
Ex:
curl -H "Content-Type:application/json" -d '{"token":"swqkm7f55v39g9kkdggkzs", "rid":"482013", "filename":"filename"}' https://device.pcloudy.com/api/download_manual_access_data
```
```java
PDriveFileDTO[] files = con.cloudDriveFileList(authToken, bookedDevice.rid);
	for (PDriveFileDTO file : files) {
	File downloadedFile = con.downloadManualPerformanceData(authToken, bookedDevice.rid, file);
}
```
This helps you to download the performance data which was stored in cloud drive.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
rid | Reservation id(will get from book device api response)
filename | performance file name.

# Automation 

## Book Devices For Appium

```shell
URL format: 
curl -H "Content-Type: application/json" -d '{"token":"authToken","duration":"duration","platform":"platform","devices":[deviceId1,deviceId2,---],"session":"sessionname","overwrite_location":"true"}' <Cloud URL>/api/appium/init

Ex: 
curl -H "Content-Type: application/json" -d '{"token":"swqkm7f55v39g9kkdggkzs","duration":"10","platform":"android","devices":[120,256],"session":"Sample session","overwrite_location":"true"}' https://device.pcloudy.com/api/appium/init
```
```java
BookingDtoDevice[] bookedDevices = con.AppiumApis().bookDevicesForAppium(authToken, selectedDevices, BOOKINGDURATION, sessionName);
```
This api book the devices for appium execution.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
duration | duration in minutes you want to book the each device.
platform | on which flatform devices you want to book (android/ios).
devices | provide array of devices id's for multiple device booking.
session | Enter session name 
overWrite_location | true

## Init AppiumHub for App

```shell
URL format: 
curl -H "Content-Type: application/json" -d '{"token":"authToken","app":"filename"}' <Cloud URL>/api/appium/execute

Ex: 
curl -H "Content-Type: application/json" -d '{"token":"swqkm7f55v39g9kkdggkzs","app":"pCloudy_Appium_Demo.apk"}' https://device.pcloudy.com/api/appium/execute
```
```java
con.AppiumApis().initAppiumHubForApp(authToken, alreadyUploadedApp);
```
This initiates the appium on devices for that app. The app you had to upload by using upload api which is available in Generic api.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
app | provide app name on which app you want to test.

## Get Appium EndPoint
```shell
URL format: 
curl -H "Content-Type:application/json" -d '{"token":"authToken"}' <Cloud URL>/api/appium/endpoint

Ex: 
curl -H "Content-Type:application/json" -d '{"token":"swqkm7f55v39g9kkdggkzs"}' https://device.pcloudy.com/api/appium/endpoint
```
```java
URL endpoint = con.AppiumApis().getAppiumEndpoint(authToken);
```
This get the appium Endpoint url.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)

## Get Appium Report Folder

```shell
URL format: 
curl -H "Content-Type:application/json" -d '{"token":"authToken"}' <Cloud URL>/api/appium/folder

Ex: 
curl -H "Content-Type:application/json" -d '{"token":"swqkm7f55v39g9kkdggkzs"}' https://device.pcloudy.com/api/appium/folder
```
```java
URL reportFolderOnPCloudy = con.AppiumApis().getAppiumReportFolder(authToken);
```
This gives you the report folder path. In the report you will get the total tests pass, failed and more information about test cases.
### URL Parameters

Parameter | Description
--------- | -----------
token | Authtoken(this will get from authenticate response)
