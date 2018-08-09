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
#
# gitignore contributors: remember to update Global/Xcode.gitignore, Objective-C.gitignore & Swift.gitignore

## Build generated
build/
DerivedData/

## Various settings
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
compile_commands.json

## Other
*.moved-aside
*.xccheckout
*.xcscmblueprint

## Obj-C/Swift specific
*.hmap
*.ipa
*.dSYM.zip
*.dSYM

## Playgrounds
timeline.xctimeline
playground.xcworkspace

# CocoaPods
Pods/
```


- **COCOAPODS**

  It is possible for a project not to have *CocoaPods* integrated. However, almost every project needs to benefit open-source libraries somewhere in the development process. For that, it is highly recommended to start a project with *CocoaPods* integrated.

  If there is an open-source library, you want to use in the project, you must add it as a pod unless there is a specific reason not to do. It would be nice to write it down to *README*. Also, if you want to add a pod, but it  does not meet all your requirements and you need to change the code, first you fork the project into Hipo repo, make your changes, then add ours into *podfile*. 

​      Sample **Podfile** 

```
platform :ios, '9.0'

use_frameworks!
inhibit_all_warnings!

def main_pods
  	#Analytics
    pod 'Intercom'
    pod 'Mixpanel-swift'
    
    #Debug
    pod 'Reveal-SDK', :configurations => ['Debug']

    #Layout
    pod 'PureLayout'
    
    #Model
    pod 'Mantle'
    pod 'KVOController'
    
    #Networking
    pod 'AWSS3'
    pod 'HIPNetworking', :git => 'https://github.com/Hipo/HIPNetworking.git'
    pod 'Kingfisher', '~> 4.0'
    
    #View
    pod 'ActiveLabel', :git => 'https://github.com/Hipo/ActiveLabel.swift.git', :branch => 'swift-4-migration'
    pod 'CCHLinkTextView'
    pod 'HIPImageCropper'
    pod 'VENTokenField', :git => 'https://github.com/Hipo/VENTokenField.git'
    pod 'PullToRefresher', :git => 'https://github.com/Hipo/PullToRefresh.git'
    pod 'SZTextView'
    
    #Utilities
    pod 'DeviceUtil'
    pod 'SSKeychain'
    pod 'PhoneNumberKit', '~> 2.1'
    pod 'AnyFormatKit', :git => 'https://github.com/Hipo/AnyFormatKit'
    
    #Structure
    pod 'IGListKit', '~> 3.0'
end

target 'Sample' do
  	# Pods for Sample
  	main_pods
end

target 'Sample-Preprod' do
  	# Pods for Sample-Preprod
  	main_pods
end

target 'Sample-Staging' do
  	# Pods for Sample-Staging
  	main_pods

  	pod 'Tryouts', :git => 'https://github.com/Hipo/Tryouts-iOS-SDK.git'
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

  Generally we have 3 targets which are *Production, Pre-Production, Staging*. According backend, we should create targets for each environment.  

  For each target we have build flag like `-APPSTORE`, `-PREPROD`. If project has satellite apps, they should have custom flag.

  To add a build flag, developer can add any build flag with opening a specific target's build settings and search "Preprocessor Macros" add build flag on necessary build configurations. 

  To gather information about build flags on compile time, we created a `Environment.swift` file. 

  	Sample **Environment.swift** 


```
//
//  Environment.swift
//  SampleApp
//
//  Created by Salih Karasuluoglu on 26.06.2018.
//  Copyright © 2018 Hippo Foundry. All rights reserved.
//
import Foundation

fileprivate enum AppTarget {
    case staging, preprod, prod
}

// MARK: Environment
@objc
class Environment: NSObject {
    
    // MARK: Singleton
    
    private static let instance = Environment()
    
    // MARK: Variables
    
    @objc class var current: Environment {
        return instance
    }
    
    @objc lazy var serverHost: String = {
        switch target {
        case .staging:
            return "https://staging.sampleapp.com"
        case .preprod:
            return "https://preprod.sampleapp.com"
        case .prod:
            return "https://sampleapp.com"
        }
    }()
    
    @objc lazy var serverApi: String = {
        return "\(serverHost)/api"
    }()
    
    @objc lazy var deeplinkScheme: String = {
        switch target {
        case .staging:
            return "sampleappstaging"
        case .preprod:
            return "sampleapppreprod"
        case .prod:
            return "sampleapp"
        }
    }()
    
    @objc lazy var deeplinkHost: String = {
        switch target {
        case .staging:
            return "staging.sampleapp.com"
        case .preprod:
            return "preprod.sampleapp.com"
        case .prod:
            return "sampleapp.com"
        }
    }()
    
    private let target: AppTarget
    
    // MARK: Initialization
    
    private override init() {
        #if APPSTORE
        target = .prod
        #elseif PREPROD
        target = .preprod
        #else
        target = .staging
        #endif
    }
}
```



- **STYLEGUIDE**

  This section has not been written yet.

  - **Layout**
  - **Color**
  - **Font**



- **FILE STRUCTURE**

  According to grouping files, they should be in right place in groups. 

  In general, the order of file structure should be in proper;

  1. Variables should be on top of methods
  2. The order of variables should be like this;
  	- Static variables
  	- Public variables
  	- Other variables
  3. Public methods should be on other methods in class.
  4. Delegate methods should be handled on a specific **extension**




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
