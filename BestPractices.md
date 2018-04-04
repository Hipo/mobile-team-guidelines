# Best Practices

This chapter outlines best practices to use for developing in iOS and Android.

## iOS

### Architectures

### Development Advices

### Common libraries
* **Layout**
	* [SnapKit](https://github.com/SnapKit/SnapKit)
	* [PureLayout](https://github.com/PureLayout/PureLayout)

* **Networking**
	* [HIPNetworking](https://github.com/Hipo/HIPNetworking)
	* [Alamofire](https://github.com/Alamofire/Alamofire)
	* [AWSS3](https://github.com/aws/aws-sdk-ios)

* **Image Download**
	* [Kingfisher](https://github.com/onevcat/Kingfisher)
	* [AlamofireImage](https://github.com/Alamofire/AlamofireImage)

* **Mapping**
	* [ObjectMapper](https://github.com/Hearst-DD/ObjectMapper)
	* [Mapper](https://github.com/lyft/mapper)
	
* **Core Data**
	* [MagicalRecord](https://github.com/magicalpanda/MagicalRecord)
	
* **Crash Reporting**
	* [Fabric](https://cocoapods.org/pods/Fabric)
	* [Crashlytics](https://cocoapods.org/pods/Crashlytics)
	 
* **Keychain**
	* [KeychainAccess](https://github.com/kishikawakatsumi/KeychainAccess)

* **Others**
	* [IGListKit](https://github.com/Instagram/IGListKit)
	* [ActiveLabel](https://github.com/optonaut/ActiveLabel.swift)
	* [SwiftDate](https://github.com/malcommac/SwiftDate)
	


### Issue tracking, development flow

* **Ticket Managment**

	We are currently using [Codebase](https://hipo.codebasehq.com/) for issue tracking. At the home page, you can see list of projects that you enrolled. When you join the team, you will be able to see tickets that you should follow.
	* Projects have weekly sprints that displays tickets for every project member.
	* Tickets are displayed with their priority, status and category labels.
	* Ticket priorities are set by the Project Manager for each milestone. 
	* Milestone tickets should be handled according to their priority.
	* When a developer start to work on a ticket, it should be updated to "In Progress" status.
	* When the developer completes the task, ticket should be directed to Senior Developer of the project for PR Review progress with "Ready for Testing" status.
	* If there is an edge case that should be tested, the developer should note that case in the ticket. 
	* After the PR Review, If Senior Developer of the project requests changes, status of ticket to should be changed "Reopen" and directed to the developer. If  PR is approved, ticket should be directed to QA.
	* Every team member should check-in daily with their assigned tickets that they will work on via ReportZ.

* **Ticket Branching**
	* A ticket that contains many features can be separated to multiple tickets.
	* A ticket can be branched to separate tickets when you complete several tasks in a ticket, but there are other things you didn't tackle. 
	
* **Branch Creation**
	* In general, a branch should contain implementation of a ticket.
	* Tickets that have small bug implementations can be combined in a single branch.
	* Branches should be created from master. If the developer is blocked by PR review process, branch can be created from the branch you need.
	* Branches should be named as _**ticketnumber-ticket-title**_. For example, a branch should look like _**5890-multi-batch-bublish-feature**_.	
* **How To Open a Pull Request**
	* Ticket link should be added to PR description.
	* Developer should provide screenshots if there is an UI change.
	* If there is an animation or multiple views that are implemented in that branch, GIF or video should be added.
	* Senior developer of the project should be assigned to Pull Request review.
	* Conflicts should be resolved before you assign the senior developer.
		
* **How To Review a Pull Request**
	


## Android

### Common libraries

#### - Networking - Parsing
#### - Image Download
#### - Dependecy Injection
