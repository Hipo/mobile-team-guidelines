# Best Practices

This chapter outlines best practices to use for developing in iOS and Android.

## iOS

### Architectures

### Development Advices

### Common libraries
* **Layout**
  * [SnapKit](https://github.com/SnapKit/SnapKit)
* **Networking**
  * [Alamofire](https://github.com/Alamofire/Alamofire)
* **Image Download**
  * [Kingfisher](https://github.com/onevcat/Kingfisher)
* **Mapping**
  * [Mapper](https://github.com/lyft/mapper)
* **Core Data**
  * [MagicalRecord](https://github.com/magicalpanda/MagicalRecord)
* **Crash Reporting**
  * [Fabric](https://cocoapods.org/pods/Fabric)
* **Keychain**
  * [KeychainAccess](https://github.com/kishikawakatsumi/KeychainAccess)
* **Utility**
  * [IGListKit](https://github.com/Instagram/IGListKit)
  * [ActiveLabel](https://github.com/optonaut/ActiveLabel.swift)
  * [SwiftDate](https://github.com/malcommac/SwiftDate)
  * [AnyFormatter](https://github.com/luximetr/AnyFormatKit)
  * [PhoneNumberKit](https://github.com/marmelroy/PhoneNumberKit)


### Issue tracking, development flow

* **Issue Tracking**
    * We are currently using [Codebase](https://hipo.codebasehq.com/) for issue tracking. At the home page, you can see list of projects that you enrolled. When you join the team, you will be able to see tickets that you should follow. We have milestones for each sprint. Generally it is a weekly titled. Issues are under these milestones with having priority, status and category labels. Every team member has a section on spesific project.
    * Before weekly huddles, tickets are set by priority via Project manager. So when you start working, you need to check-in according to high proirty tickets. On weekly huddles, these priority might change.
    * You can create tickets for example when you faced with a bug on project. Reproduction scenarios will absolutely help to us for resolving the issue. So please write scenarios step by step.
* **Pre-development**
    * Since we are using git version control ([Github client](https://github.com)), the codes that we write will be merged by senior developer of that project after code review. So when you are contributing, you need to branch out project from master and make commits to that branch.
    * That branch should have ticket numbers to keep track it easily on Codebase. Also, you can combine little tickets in one seperate branch.
    * For example, branch should look like _**5890-multi-batch-bublish-feature**_ it is easy to read and understandable that what you want to impelement.
    * You can change the status according your work, you can label it as **In Progress**, **Ready for Testing**, **Invalid**, etc. on Codebase
    * Sometimes we may have critical bugs, crashes etc and we need to fix these kind of issues. Our QA team will provide a reproduction scenario of the issue.
* **Post-development**
    * When you are done with branch, you need to create Pull Request for merging. Most likely, you will be provided comments, change requests for your Pull Request and if it is ready to go, it will be merged to codebase. You can comment the Pull Request link to the ticket.
    * Sometimes you have conflicts with your branch, before you creating pull request you need to resolve git conflicts.
    * Please include UI related files such as screenshots, animation videos etc. to your Pull Request, If you are deailing with developing UI features.
    * You can provide test scenarios in tickets that you tried for the issue.

## Android

### Common libraries

#### - Networking - Parsing
#### - Image Download
#### - Dependecy Injection
