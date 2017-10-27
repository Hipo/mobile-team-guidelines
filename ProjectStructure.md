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
models
network
utils
```

Core package should include base files. For example BaseActivity, BaseFragment, ProjectApplication etc. These files are basic and associate with almost whole of the project.

When you need to use one design feature more than one, you can create its customview. Thus, you can control same features from one place.



