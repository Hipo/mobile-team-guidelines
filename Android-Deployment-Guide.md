##Android Deployment Guide 

We are using jenkins for continues integration. Jenkins  is connected with application’s git repo and tryouts. When you push your code with a version tag to GitHub, jenkins is building a signed release apk file and uploading it to tryouts. Tryouts is posting release info at related slack channel. 

###Jenkins Setup

1. Click **New Item**
2. Type your project name into the edit text at top.
3. Scroll down to bottom and type the project name which you will copy all setup from into **Copy from** edit text.  (!! Copy from is case sensitive)
4. Replace all copied app names with the new app name. 
5. Jenkins setup is complete

###Tryouts Setup

1. Create a new app for each flavor
2. Send invitations to people who are working at this project
3. Go to **Integrations** tab and get  **API Key**, **API Secret** and **App ID**. We will use them in the app for jenkins setup.
4. Click **webhook URL creator for Slack** and choose the slack channel for the release messages will be sent. 
5. Tryouts setup is complete.

###App Setup

1. Copy jenkins folder from one of the previous android project. It contains 3 files. **build.sh**, **jenkins.sh** and **sign-and-upload.sh**
2. Keypoint in **build.sh** is *"./gradlew clean aRelease"*. aRelease means build relese for all build variants. You can also specify the releases you want to get individually or as a group. (*aProd, aStaging, aStagingRelease aPreprodRelease aProdRelease...*)
3. Don't need to change anything in **jenkins.sh** file.
4. **sign-and-upload.sh** is the tricky part.

Add these blocks for each of your flavors.

```
 export TRYOUTS_APPNAME_APP_ID = <TRYOUTS APP ID>
 export TRYOUTS_APPNAME_APP_TOKEN = <TROUTS-API-KEY>:<TRYOUTS-API-SECRET>
```

```
if [ ! -z "$TRYOUTS_APPNAME_APP_ID" ] && [ ! -z "$TRYOUTS_JOHN_STAGING_APP_TOKEN" ]; then
  echo ""
  echo "***************************"
  echo "* Uploading App Name Release to Tryouts    *"
  echo "***************************"
  curl https://tryouts.io/applications/$TRYOUTS_APPNAME_APP_ID/upload/ \
    -F status="2" \
    -F notify="0" \
    -F notes="$RELEASE_NOTES" \
    -F build="@$OUTPUTDIR/<app-flavor>/release/app-name-staging-release.apk" \
    -H "Authorization: $TRYOUTS_APPNAME_APP_TOKEN"
fi
```

###Sending a Release

- Checkout master branch
- Set version numbers in build.gradle. 
- Commit your build number change with release message. Ex:"**Released 1.0.1-1**"
- Add tag to your commit Ex: "**1.0.1-1**"
- Push commit and tag at once. 
- Jenkins will be triggered after you push this commit. It will send a release to tryouts. Tryouts will share release info at slack. 

####Naming conventions for release

- Initial release: **1.0.0-1**
- Test release: **1.0.0-13**
- Store relese: **1.0.0**
- Multiple app release at once: **1.0.0-1&1.0.1-1**
- Git commit message: "**Released 1.0.1-1.0.2**"

####Semantic Versioning
MAJOR.MINOR.PATCH-BETA

- MAJOR version when you make incompatible API changes,
- MINOR version when you add functionality in a backwards-compatible manner, and
- PATCH version when you make backwards-compatible bug fixes.
- BETA version when you create inhouse release.
