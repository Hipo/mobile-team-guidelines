
## Android CI Setup Guide

### Overview

There are two Github action workflows that are integrated to our projects. One of them provides a build check after push. It's called as **Push Build Test**. The other one is responsible for deploying app to Google Play Internal App Distribution Channel. This workflow is called as **Deploy Release to Google Play** and works after pushing tags to any branch. This can be restricted to master branch later on with two line change on the workflow file.

## Branch Build Test
This workflow will be triggered after every push unless it's tag branch. The workflow can be integrated in any development phase of the project but needs to be customized according to **productFlavors** on the app.

```yaml
name: Branch Build Test

on:
  push:
    branches-ignore:
      - 'refs/tags/**'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@master
      - name: set up JDK 1.8
        uses: actions/setup-java@master
        with:
          java-version: 1.8
      - name: Check If App Builds At All Flavors
        run: sudo ./gradlew :app:assembleProdDebug :app:assembleProdRelease :app:assembleStagingDebug :app:assembleStagingRelease
      - name: Find APKs
        run: sudo find . -iname '*.apk'
```

## Deploy Release to Google Play

This workflow will be triggered after every tag for release. Before getting release, don't forget to increase **Version Code** and **Version Name** of the project. If these variables don't get increased by the developer. This workflow will fail automatically by Google Play Console.

 To integrate this workflow, 
-  You need to give permission to service account with the name  **_GithubActionsPublish_** for the app with this link  https://play.google.com/apps/publish#AdminPlace. After opening the page, click **_Change Permissions_** **[ss1]** and after that, click **_Add App_** and select the newly integrated app **[ss2]**.
- Add **maven { url "https://plugins.gradle.org/m2/" }** as repository and **classpath "com.github.triplet.gradle:play-publisher:{last_stable_version}"** as dependency to project's **root build.gradle**.
- Get the JSON file needed for play-publisher library with this link (https://console.cloud.google.com/apis/credentials/serviceaccountkey) and Leave JSON checked. [ss3]
- Save JSON as **publish.json** and move to under **app/** folder.
- Add **apply plugin: 'com.github.triplet.play'** to after **apply plugin: 'com.android.application'** in your **app/build.gradle** file.
- Add lines below to **app/build.gradle** file. Don't forget to change lines according to your needs.
```java
play {  
  serviceAccountCredentials = file("publish.json")  
  track = "internal"   // distribute to internal channel.
  /* 
	  If the app didn't get approved from Google Play yet. 
	  Change release status to the "draft".
  */ 
  releaseStatus = "completed" 
  userFraction = 1
  /*
	  If you don't want to publish as App Bundle,
	  change defaultToAppBundles to false.
  */  
  defaultToAppBundles = true  
}
```

- After doing all steps above, you can add **Deploy Release to Google Play** workflow below.

```yaml
name: Deploy Release to Google Play

on:
  push:
    tags:
      - '*'

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@master
    - name: set up JDK 1.8
      uses: actions/setup-java@master
      with:
        java-version: 1.8
    - name: Publish App to Google Play Internal Channel.
      run: ./gradlew publish
```
