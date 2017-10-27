# Project Structures

This chapter outlines the directory structures and various other basic requirements for iOS & Android projects.

## iOS



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

