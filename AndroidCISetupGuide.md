
## Android CI Setup Guide

### :eye: Overview

There are three Bitrise workflows that are integrated to our projects. One of them provides a build check after push. It's called as **Commit Check**. Second one is responsible for deploying app to Google Play Internal App Distribution Channel. This workflow is called as **Deploy To Firebase** and works after pushing tags with (-) in it (For example 1.0.1-3) to any branch. Third one is responsible for generating App Bundle and send it to Slack channel. This workflow is called as **Deploy Store Release** and works after pushing tags without dash (-) in it (For example 1.0.1) to any branch.

[Yml Generator](https://yml-generator.netlify.app/) can be used to create .yml easily

## :rocket: Creating Project
To create a project, account needs to be part of Hipo organisation. Otherwise Hipo will not be found under accounts section. If you can not see Hipo, ask Taylan abi to send invitation to that account. 

Go to https://app.bitrise.io/apps/add

**STEP 1 - CHOOSE ACCOUNT**

- Select **Hipo** and set privacy of the app as **Private**

**STEP 2 - CONNECT YOUR REPOSITORY**

- Select the repository of the app.

**STEP 3 - SETUP REPOSITORY ACCESS**

- If you are one of the admin of the repository, click **No, auto-add SSH key** and Bitrise will add SSH key automatically. If you are not, click **I need to** and add SSH key manually into your Github Account.

**STEP 4 - CHOOSE BRANCH**

- Type **master** and click next. Selected  branch should include the configuration of the project as Bitrise will configure the app based on that.

**STEP 5 - VALIDATING REPOSITORY**

- In this step Bitrise will validate repository. We should wait it to be finished.

**STEP 6 - PROJECT BUILD CONFIGURATION**

- Specify Module -> **app**

- Specify Variant -> **[VARIANT THAT YOU WANT TO BUILD WITH]** This can be changed later easily in Environment Variables section.

**STEP 7 - APP ICON**

- Bitrise will automatically select app icon from Github Repository. If it doesn't show app icon properly, you need to add it manually. 

**STEP 8 - WEBHOOK SETUP**

- If you are one of the admin of the repository, click **Register a Webhook for me!** and Bitrise will register webhook automatically. If you are not, click **Skip the Webhook registration.** After completing setup, go to Project -> {} Code -> Incoming Webhooks -> Setup Manually -> Select Github -> Copy Webhook Url -> Send it to Taylan Abi with [setup guide](https://github.com/bitrise-io/bitrise-webhooks#github---setup--usage)

## :heavy_dollar_sign: Environment Variables
This section is being used to create global variables to use in workflows. Here are some example variables;

- **PROJECT_LOCATION** = .

- **MODULE** = app

- **VARIANT** = prodRelease 

(This variant should not include signingConfig which uses local properties. Otherwise build step will be failed). To overcome this, there should be 2 different variants which are; 

```
releaseLocal {
	signingConfig signingConfigs.releaseLocal
	...
}
	
release {
	...
}
```
This is the only change needs to be done on project side to complete Bitrise integration.

- **GRADLE_PATH** = ./gradlew

- **GRADLE_TASK_COMMIT_CHECK** = assembleProdRelease

- **STAGING_VARIANT** = stagingRelease

- **PROD_VARIANT** = prodRelease

- **FIREBASE_STAGING_APP_ID** = This ID can be found in Firebase Console -> Project settings.

- **FIREBASE_PROD_APP_ID** = This ID can be found in Firebase Console -> Project settings.

- **FIREBASE_APP_DIST_GROUP** = qa 

This is the name of the distribution group created in Firebase App Distribution. This can be found in Firebase Console -> App Distribution -> Testers & Groups. 

- **FIREBASE_APP_DIST_STAGING_DOWNLOAD_LINK** = https://appdistribution.firebase.dev/XXX

- **FIREBASE_APP_DIST_PROD_DOWNLOAD_LINK** = https://appdistribution.firebase.dev/XXX

These two urls above can be found in Firebase Console -> App Distribution -> Invite Links. They will be used for slack message buttons after deploying app to Firebase App Distribution.
 
- **SLACK_MESSAGE_ICON_URL** = https://firebase.google.com/images/brand-guidelines/logo-vertical.png

- **SLACK_FIREBASE_BOT_NAME** = Bitrise - Firebase App Distribution
 
- **SLACK_BUNDLE_BOT_NAME** = Bitrise - App Bundle

- **SLACK_MESSAGE_COLOR** = #3DDC84

## :lock: Secrets

**SLACK_WEBHOOK_URL** = To get webhook url, [slack app](https://api.slack.com/apps?new_app) needs to be created. This webhook will be used to send messages to slack channel.

**FIREBASE_TOKEN** = To get firebase token, in terminal, type **firebase login:ci**, select account to create token with and get the token from terminal.


## :memo: Signing

To sign the build, we need to provide signing config to Bitrise. Go to Project -> Code Signing. Upload keystore file and fill alias and passwords fields.

## :hammer: Workflows

As mentioned in the overview, there will be 3 workflows which are; 

* commit-check
* deploy-to-firebase
* deploy-store-release

### commit-check

Steps 1, 2, 3, 4 and 6 are autogenerated and no need to make any changes. Other autogenerated steps can be removed. commit-check workflow should be as below;

1. Activate SSH key
2. Git Close Repository
3. Bitrise.io Cache:Pull
4. Install missing Android SDK components
5. Gradle Runner
	1. Optional path to the grandle build file to use: $GRADLE_BUILD_FILE_PATH
	2. Gradle task to run: $GRADLE_TASK_COMMIT_CHECK (Defined in Env Vars. this is gradle task to run for commit check.)
	3. Gradlew file path: $GRADLE_PATH (Defined in Env Vars.)
6. Bitrise.io Cache:Push

### deploy-to-firebase


1. Activate SSH key
2. Git Close Repository
3. Bitrise.io Cache:Pull
4. Install missing Android SDK components
5. TESTS - This is optional. If project has tests in it, then test steps needs to be added.
6. Build Staging Release (Step original name: **Android Build**)
	1. Project Location: **$BITRISE_SOURCE_DIR**
	2. Module: **$MODULE**
	3. Variant: **$STAGING_VARIANT**
	4. Build type: **apk** *Firebase doesn't support bundle yet*
7. Sign Staging Release (Step original name: **Android Sign**)
	1. App file path: **$BITRISE_APK_PATH**
	2. Keystore url: **$BITRISEIO_ANDROID_KEYSTORE_URL**
	3. Keystore password: **$BITRISEIO_ANDROID_KEYSTORE_PASSWORD**
	4. Keystore alias: **$BITRISEIO_ANDROID_KEYSTORE_ALIAS**
	5. Private key password: **$BITRISEIO_ANDROID_KEYSTORE_PRIVATE_KEY_PASSWORD**
8. Distribute Staging App to Firebase (Step original name: **Firebase App Distribution**)
	1. Firebase token: **$FIREBASE_TOKEN**
	2. App Path: **$BITRISE_SIGNED_APK_PATH**
	3. Firebase App ID: **$FIREBASE_STAGING_APP_ID**
	4. Test Groups: **$FIREBASE_APP_DIST_GROUP**
9. Build Prod Release (Step original name: **Android Build**)
	1. Project Location: **$BITRISE_SOURCE_DIR**
	2. Module: **$MODULE**
	3. Variant: **$PROD_VARIANT**
	4. Build type: **apk** *Firebase doesn't support bundle yet*
10. Sign Prod Release (Step original name: **Android Sign**)
	1. App file path: **$BITRISE_APK_PATH**
	2. Keystore url: **$BITRISEIO_ANDROID_KEYSTORE_URL**
	3. Keystore password: **$BITRISEIO_ANDROID_KEYSTORE_PASSWORD**
	4. Keystore alias: **$BITRISEIO_ANDROID_KEYSTORE_ALIAS**
	5. Private key password: **$BITRISEIO_ANDROID_KEYSTORE_PRIVATE_KEY_PASSWORD**
11. Distribute Prod App to Firebase (Step original name: **Firebase App Distribution**)
	1. Firebase token: **$FIREBASE_TOKEN**
	2. App Path: **$BITRISE_SIGNED_APK_PATH**
	3. Firebase App ID: **$FIREBASE_PROD_APP_ID**
	4. Test Groups: **$FIREBASE_APP_DIST_GROUP**
12. Bitrise.io Cache:Push
13. Send Firebase Release Notification (Step original name: **Send a Slack message**)
	1. Slack Webhook URL: **$SLACK_WEBHOOK_URL**
	2. Text of the message to send: **:rocket: Released $BITRISE_GIT_TAG (Android)**
	3. URL to an image to use as the icon for the message: **$SLACK_MESSAGE_ICON_URL**
	4. The bot's username for the message: **$SLACK_FIREBASE_BOT_NAME**
	5. Message color: **$SLACK_MESSAGE_COLOR**
	6. A list of buttons attached to the message as link buttons: 
		* [APP_NAME] + [VARIANT] | ${FIREBASE_APP_DIST_PROD_DOWNLOAD_LINK}
		* [APP_NAME] + [VARIANT] | ${FIREBASE_APP_DIST_STAGING_DOWNLOAD_LINK}
	
### deploy-store-release
Create new workflow based on **deploy-to-firebase** and add 2 steps at the end of the workflow;
1. Deploy Prod Bundle (Step original name: **Deploy to Bitrise.io - Apps, Logs, Artifacts**)
	1. Deploy directory or file path: **$BITRISE_SIGNED_AAB_PATH**
	2. Enable public page for the App?: **false**
	3. Compress the artifacts into one file: **false**
2. Share Prod Bundle Upload Link to Slack (Step original name: **Send a Slack message**)
	1. Slack Webhook URL: **$SLACK_WEBHOOK_URL**
	2. Text of the message to send: **:rocket: App Bundle Created for ($BITRISE_GIT_TAG)**
	3. URL to an image to use as the icon for the message: **$SLACK_MESSAGE_ICON_URL**
	4. The bot's username for the message: **$SLACK_BUNDLE_BOT_NAME**
	5. Message color: **$SLACK_MESSAGE_COLOR**
	6. A list of buttons attached to the message as link buttons:
		* Build URL|$BITRISE_BUILD_URL
		
## :zap: Triggers
**PUSH**

|   | Push branch | Workflow |
|:-:| :---------: | :------: |
| 1 | * | commit-check |

**PULL REQUEST**

- [ ] Add new workflow to send slack message about pull requests

**TAG**

|   | Tag     | Workflow             |
|:-:| :-----: | :------------------: | 
| 1 | * . * . * - * | deploy-to-firebase   |
| 2 | * . * . *   | deploy-store-release | 
