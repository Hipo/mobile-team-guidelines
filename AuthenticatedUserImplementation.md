## Authenticated user implementation

Generally we are using singleton pattern for authenticated user model implementation in applications.

An example `User` implementation could be like the following;

```swift
class AuthenticatedUser: User {
    private static let shared = AuthenticatedUser()

	// variables
	let token: String
	...
	
	// methods	
	func saveToLocal() {
		...
	}
		
	func logout() {
		...
	}
	
	func fetch() {
		...
	}
}
```

Method explanations:

`func saveToLocal()`: Saves the authenticated user to NSUserDefaults or Keychain according to situation. 

`func logout()`: Required functionalities after a logout could be implemented within this method. For instance, calling relevant endpoint, clearing local data (from userdefaults, or Keychain), unregistering from push notifications.

`func fetch()`: In an application we often need to update authenticated user's data - for example, when the user created a new post and number of posts label needed to update. Using this method we can update the user whenever needed. A specific notification could also be posted to inform views all accross the application that is connected the user's information.

Usage: 

```swift
AuthenticatedUser.shared.saveToLocal()
AuthenticatedUser.shared.logout()
AuthenticatedUser.shared.fetch()
```

