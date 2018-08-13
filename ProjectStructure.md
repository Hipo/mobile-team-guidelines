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

Sample **Podfile** 

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


- **TARGET MANAGEMENT**

  Generally we have three targets which are *Production, Pre-Production, Staging*. According backend, we should create targets for each environment. 

  You can then add swift compiler flags so that you can check the current configuration through code. To do that, goto _Target->Build Settings->Swift Compiler->Custom flags->Other Swift Flags_ section and edit the flags for different environments. Add the flags using the sytax, "-D{target_name}" like "-DAPPSTORE", "-DPREPROD", and "-DSTAGING". If you have satellite applications in the project, you can set the custom flags using the same method. 

  To get the relevant configuration about build flags on compile time, you can use the `Environment.swift` file. 

Sample **Environment.swift** 

```
import Foundation

fileprivate enum AppTarget {
    case staging, preprod, prod
}

// MARK: Frameworks
struct IntercomBundle {
	// MARK: Variables
    lazy var token: String = {
       switch target {
           case .staging, .preprod:
           	   return "intercom_test_token"
           case .prod:
               return "intercom_prod_token"
       } 
    }()
    
    private let target: AppTarget
    
    // MARK: Initialization
    fileprivate init(target: AppTarget) {
        self.target = target
    }
}

struct FrameworkBundle {
    // MARK: Variables
    lazy var intercom: IntercomBundle = {
        return IntercomBundle(target: target)
    }()
    
    private let target: AppTarget
    
    // MARK: Initialization
    fileprivate init(target: AppTarget) {
        self.target = target
    }
}

struct Environment {
    // MARK: Singleton
    private static let instance = Environment()
    
    // MARK: Variables
    static var current: Environment {
        return instance
    }
    
    lazy var serverHost: String = {
        switch target {
        case .staging:
            return "https://staging.sampleapp.com"
        case .preprod:
            return "https://preprod.sampleapp.com"
        case .prod:
            return "https://sampleapp.com"
        }
    }()
    
    lazy var serverApi: String = {
        return "\(serverHost)/api"
    }()
    
    lazy var frameworks: FrameworkBundle = {
        return FrameworkBundle(target: target)
    }()
    
    private let target: AppTarget
    
    // MARK: Initialization
    private init() {
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

  In Hipo, we do not use interface builder, except **Launch Screen**, to build the UI components. Since they are implemented in the code, it is an important job for us to design styling code in a well-organized way. 
  The general rule we should consider is to use the three types of configuration, _application-based, screen-based_ and_ component-based styling_, for the respected properties. This will be especially helpful to standardize the code to be able to answer the requirements of the different projects, customers and designers.
  
  - **Application-based**: To support the shared styling for the same interface elements in the application.
  - **Screen-based**: To support the shared styling for the interface elements in the same screen, but having different styling from the application-based configuration.
  - **Component-based**: To support the shared styling for the interface elements in the same containing component, but having different styling from the application-based and the related screen-based configurations.

We are using a shared pod (currently in progress) for styling, and also the common UI code, which can be found in [https://github.com/Hipo/HIPUIKit-Sample]: here.


- **FILE STRUCTURE**

  The developers can follow his/her own rules to the code structure; however, there are a few rules everyone should consider.
  
  - Whatever structure you choose, you must be consistent throughout the whole project. If you join an existing project, you should make an effort to keep it on. However, if you see a inconvenient rule, and want to change it, you should do it everywhere in the project before pushing it to master branch.
  - You should fix the all issues generated by Swift linter as soon as possible. These rules are strict and decide by the whole team. Thus if you want to change any of them, you should first talk to the team, explain your reasons. For other issues, you can check 'Swift Style Guide' before you decide a rule or consult the other team members.
  - Pragma marks and extensions are very helpful for the other team/project members to follow and understand the code/project; therefore, it is highly recommended to use them as much as possible.
  
  For a reference, you can also check the shared pod, which can be found in [https://github.com/Hipo/HIPUIKit-Sample]: here.




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
