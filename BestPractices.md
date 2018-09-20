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
	Reviewing pull request is not easy to do. Reviewer should consider codes in multiple perspectives. 
	* Your responsibility is make sure that code is correct and high quality before gets merged.
	* Reviewer should understand the approach is being taken and what the pull request is trying to achieve.
	* The code should have clear variable names and functions to reviewers understand what's going on and logics should be separated for each logic.
	* Everyone make mistakes, when you review the code take consider best practices.
	* Commented out codes should not included in PR.
	* Since we are using linters for each project, it warns developer to fix warnings / errors before pushing changes, somehow developer may miss it. You can start checking warnings and errors before review process. These warnings should be fixed before merging.
	* Codes should be defensive instead of throwing errors and causing crashes.
	* Git clients have very productive diff views to understand what changes in PR. When you start to review, you will have to review codes line by line. 
	* If something wrong or problematic in any piece of code, it should be requested for changes.
	* After finalizing review, if there is something broken or weird, you can comment it out on PR.

	


## Android

### Architectures

### Development Advices

* Do not put too much code in activities.
* Code blocks that are used more than one activities should put into *Utility* class or *Base* classes.
* Rules of code guidelines must be followed while writing the code. 
  * [**[*Java*]** Code Guideline](https://github.com/ahmetmentes/android-guidelines)
  * [**[*Kotlin*]** Code Guideline](https://android.github.io/kotlin-guides/style.html)
* Simple values that needs to be persisted in application can be put into SharedPreferences.
* It's a good idea to make custom view attributes changeable from XML file.
* *Do not use* hardcoded strings in layout and put all strings to **strings.xml** file.
* All colors must be declared in **colors.xml** file to use.
* If the exact same text has **more than one** version with different caps try to use **textAllCaps** to reduce the number of defined strings
* Repetitive usages of padding, margin and related attributes values must be put into dimen to change easily.
* Putting all style related layout attributes is a good practice to use in repetitive views.
* Passwords of signingConfigs should be put into **gradle.properties** file.
* Use different package name for debug, pre-production builds and etc.
* Avoid using too much nested layouts. (Nested Layouts are not performance-friendly) Using constraint layout in those situations may be useful.
* Library usage should be minimal and essential ones for specific application must be used only.
* You can insert sample data in your layout preview by using the **tools:** prefix instead of android: with any <View> attribute from the Android framework. This is useful when the attribute's value isn't populated until runtime but you want to see the effect beforehand, in the layout preview. Building removes these attributes so there is no effect on your APK size or runtime behavior.

### Common libraries
* **Networking**
  * [OkHttp](http://square.github.io/okhttp/)
  * [Retrofit](http://square.github.io/retrofit/)
     * Converters
       * [GsonConverter](https://github.com/square/retrofit/tree/master/retrofit-converters/gson)
       * [JacksonConverter](https://github.com/square/retrofit/tree/master/retrofit-converters/jackson)
* **Image Download**
  * [Glide](https://github.com/bumptech/glide)
* **Dependency Injection**
  * [Dagger](https://github.com/google/dagger)
  * [Koin](https://github.com/InsertKoinIO/koin)
* **Maps**
  * [Google Maps Utils](https://github.com/googlemaps/android-maps-utils)
* **Image Zoom**
  * [PhotoView](https://github.com/chrisbanes/PhotoView)
* **Reactive Extension**
  * [RxJava](https://github.com/ReactiveX/RxJava)
* **Catching Memory Leaks**
  * [LeakCanary](https://github.com/square/leakcanary)
* **Advanced Video Player**
  * [ExoPlayer](https://github.com/google/ExoPlayer)
* **View Injection Library [*Java Only*]**
  * [ButterKnife](http://jakewharton.github.io/butterknife/)
* **Testing**
  * [Mockito](https://site.mockito.org/)
  * [Espresso](https://developer.android.com/training/testing/espresso/)
  * [Robolectric](http://robolectric.org/)
