# ReactNativeLogin
mpk file for the Standard React Native SSO Login
**This will only work when running from a deployed environment and compiled with Native Builder.** 

 

**This will not work on the Make it Native App because the emulator will not see your actual app name.**  

 

 

# Mendix Setup 

- Add React Native SSO Login from the ConocoPhillips Mendix App Store to your app 

- Install the following dependencies: 

- --add app store module 'Native Mobile Resources' instead of Actions 
- --add app store module 'Nanoflow Commons' 

- --add app store module 'JWT' 

--add app store module 'Community Commons' from Billy's notes 


- create native navigation profile 

--For the Responsive Navigation, create a home page in the main module OR use the Home_Web? - there already? 

- add anonymous role in project security that has access to login anon and system user and Native Mobile Anonymous? 

- Add a "ConfigurationOverview" page to Responsive Navigation to show Login.NativeSSO_ConfigurationOverview 

- On the Native Mobile Navigation add the Add the "Login.SSO_Native_Anonymous" page as the home screen for Anonymous users only. Make sure it is visible for only Anonymous  

- Add the "SNIP_OnResume" to the these page templates and any new ones you create.   

- Add the following for the Security options for your app 

- Login.Administrator and Login.User hav Administrator and User

- Rights on the Token entity in the Login Domain model for both the User and the Administrator role should have full read and write 

 

# Azure Setup 

- Set up an app registration in Azure for your app. It should include 

- In the implicit grant section you can select both the access and id token options. Testing has shown that only the id token is needed.

- In the Redirect URI section set the callback to yourappname:// 

- Set the api permissions for MicrosoftGraph as follows 


```
email 

Mail.Read 

offline_access 

profile
```
 

 

- In the Expose Api section set the api permissions to https://yourappname-environment/user_impersonation  

 

# Deploy your app to the cloud 

## Configuration  

- Go to your app in the web and add the configuration variables in the configuration page. 

- To set your configuration values you must be signed in to the application. You can do that by accessing the configuration page using the MxAdmin account or configure SAML for the responsive portion of your web application. 

  

# Build and Run your app in Mendix Studio 

 
Since the application is now updated to 8.17 it can make use of the Build Native Mobile App option under the project menu. You should use this option rather than the following instructions. Refer to the Mendix documentation for instructions on use the new option instead of the manual steps below. Your app must be upgraded to 8.15 to use the option under the project menu and to use this module your app must be updated to 8.17. You will still need to set up an account in Git Hub and in MSAppcenter as well as providing a .plist file and certificate for signing. At this writing Ryan Token or Andrew Dodson can assist you with that. 



# Deprecated instructions follow:
#  
Download Native Builder 3.2 

 

https://docs.mendix.com/howto/mobile/native-build-locally 

https://www.dropbox.com/sh/hpw7sshut9bco68/AABackrr75rPSgW7u5LBMkMra?dl=0&preview=native-builder-v3.2.0.zip 

 

Set up your app in appcenter and github 

https://docs.mendix.com/howto/mobile/deploying-native-app 


Clean your deployment directory 

 

Run Native Builder  - Prepare 

native-builder.exe prepare --github-access-token yourgithubtoken --appcenter-api-token yourmsappcentertoken --java-home "C:\Program Files\AdoptOpenJDK\jdk-11.0.3.7-hotspot" --mxbuild-path "C:\Program Files\Mendix\8.10.2.394\modeler\mxbuild.exe" --project-path "C:\Users\kollms\Documents\Mendix\SSOTestSMK-main_2\SSOTestSMK.mpr"  --runtime-url "https://ssotestsmk100-test.mendixcloud.com"  --projectName "SSOTestSMK" --app-identifier "com.conocophillips.SSOTestSMK" --app-name "SSOTestSMK" --mendix-version "8.10.2" --platform "ios" 

 

Run Native Builder  -  Build 

BE SURE TO CHANGE THE BUILD NUMBER BELOW IF WHEN DOING YOUR 2nd through nth build. 
 

native-builder.exe build --project-name "SSOTestSMK" --app-version "1.0.1" --build-number "1" --mendix-version "8.10.2"  --platform "ios" 


Update the info.plist file 

In order to redirect back to your application from a web browser, you must specify a unique URI to your app. This will be done on GITHUB or in XCode or Android Studio (Android). 

Go to the Github repository and change the MASTER branch plist  (IOS -> NativeTemplate -> Info.plist to include the example below.    

When completed Click Commit directly to master branch. 

Example for IOS: 

<key>CFBundleURLTypes</key>   

<array>   

<dict>  

<key>CFBundleURLName</key>   

<string>com.conocophillips.ssotestsmk</string>   

<key>CFBundleURLSchemes</key>  

<array>  

<string>ssotestsmk</string>  

</array>  

</dict>   

 

Example for Android: 

 

<intent-filter> <action android:name="android.intent.action.VIEW" /> <category android:name="android.intent.category.DEFAULT" /> <category android:name="android.intent.category.BROWSABLE" /> <data android:scheme="my-scheme" android:host="my-host" android:pathPrefix="" /> </intent-filter>   

   

Once this is completed you need to increment the build number and do another build. The change from above will be reflected in your new build. You only need to make this plist change one time.  

 

Signing your App 

Once your build is on app center contact the mobility team to sign your app. (Currently Andrew Dodson or Ryan Token) 

Then you can install the app onto your device.  
