# Project Structures

This chapter outlines the directory structures and various other basic requirements for iOS & Android projects.



## iOS

Project structure is the first thing we should pay attention when starting a project. Generally, there are a few rules which every iOS team member should follow.



- **GIT** 

  All projects in Hipo will be hosted in 'Github'. So, first thing we should do is to put a standard *.gitignore* file into the project. We do not push installed pods (CocoaPods) to git, therefore; do NOT forget to add the restriction into gitignore because, before, we had experienced that some pods might have too much size to be hosted in Github. Also, it would be nice to add a *README* file with general info about the project and how to setup development environment.

      Sample **.gitignore** 

```
# OS X temporary files that should never be committed
.DS_Store
.Trashes
*.swp

# Xcode
*.pbxuser
!default.pbxuser
*.mode1v3
!default.mode1v3
*.mode2v3
!default.mode2v3
*.perspectivev3
!default.perspectivev3
*.xcworkspace
!default.xcworkspace
xcuserdata
*.moved-aside
compile_commands.json

# CocoaPods
build/SampleProject/Pods/

## Build generated
build/SampleProject/DerivedData
```


- **COCOAPODS**

  It is possible for a project not to have *CocoaPods* integrated. However, almost every project needs to benefit open-source libraries somewhere in the development process. For that, it is highly recommended to start a project with *CocoaPods* integrated.

  If there is an open-source library, you want to use in the project, you must add it as a pod unless there is a specific reason not to do. It would be nice to write it down to *README*. Also, if you want to add a pod, but it  does not meet all your requirements and you need to change the code, first you fork the project into Hipo repo, make your changes, then add ours into *podfile*. 

​      Sample **Podfile** 

```
platform :ios, '9.0'

use_frameworks!

def main_pods
  pod 'Fabric'
  pod 'Crashlytics'
  pod 'TwitterKit', '3.2.2'
  pod 'Digits', '3.0.2'
  pod 'GoogleMaps', '2.5.0'
  pod 'GooglePlaces', '2.5.0'
  pod 'MMNumberKeyboard'
  pod 'Google/SignIn'
  pod 'SAMKeychain'
  pod 'Intercom'
  pod 'FBSDKCoreKit'
  pod 'FBSDKCoreKit'
  pod 'FBSDKLoginKit'
  pod 'ModelMapper'
end

target 'Sample' do
  # Pods for Sample
  main_pods
end

target 'Sample-Preprod' do
  # Pods for Sample-Preprod
  main_pods
end
```



- **GROUPING FILES**

  This is the one thing every iOS developer in Hipo must put effort. There are several groups which needs to be included in almost every project.

  1. Classes
     - API
     - Application
     - Extensions
     - Managers
     - Models
     - Protocols
     - ViewControllers
     - Views
     - Utility
  2. Resources
     - Assets
     - Fonts
     - LaunchScreen
     - Localization
  3. Support
     - Info.plist
     - Dev-info.plist

  New concepts such as *Animators, Datasources, Delegates, Layouts, ViewModels* etc. should also be grouped with the same systematic.

  The files created under these general concepts should be grouped by feature-set such as *Common, Feed, Profile, Publish* etc. , as well. 

  Under feature levels, you do not have such grouping instructions, but it is encouraged to create new groups for every sensible concept. Additionally, you should sort all your groups and files by alphabetical order. These will make project structure more readable.

  Besides this grouping strategy, there are a few key points you should bear in mind. First. you should maintain a logical order by putting source files in groups. For example, if you create a view class for a view controller positioned under *Classes>ViewControllers>Profile*, the file should be put under *Classes>Views>Profile*. This way, all groups will be in sync by the same hierarchical order. And if you have a class which is used for multiple feature-set, then these files should be moved to 'Common' groups and their names should be changed if needed. Moreover, the group structure within both *XCode* and *filesystem* MUST be identical. It is a big plus for a well-organized project.

  ​

- **TARGET MANAGEMENT**

  This section has not been written yet.

  ​


- **STYLEGUIDE**

  This section has not been written yet.

  - **Layout**
  - **Color**
  - **Font**



- **FILE STRUCTURE**

  This section has not been written yet.




## Android

### Package Structure

File order is one of important thing to start the project. Files should be seperate groups which needs to be included in every project.

```
core
customviews
features
network
models
utils
```

Core package should includes base files. For example BaseActivity, BaseFragment, ProjectApplication etc. These files are basic and associate with almost whole of the project.

When you need to use one design feature more than one, you can create its customview. Thus, you can control same features from one place.

Features package can seperates subpackages screen feature to screen feature as login, signup, main. Subpackages should includes screen related file like its activity, fragment, adapter etc.

Api setting files should be in network packages. Api responses can located in models package.

Lastly utils package should includes files that special to your project and needs to use frequently like getting current time, download file, cropping image etc. 
