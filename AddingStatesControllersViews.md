## Adding states to controllers, views

We often need to add states controllers or views for several situations. Enums are very handy to keep states, enabling us to make changes in class according to state that the controller or view is in.

### Controllers

Whenever we need to add new functionality to a controller we can list the each state of this controller, and make this controller to use the functionality 
only if it is in a specific state.

For a better illustration, let's say we have a view controller of post detail screen. There can be two situations where we need to initialize this controller with a post object - when coming from feed - or with a post identifier, coming from places where we don't have post object. 

First approach could be initializing the controller with two optional parameters as; `post: Post?` and `postIdentifier: Int?`;

```swift
class PostDetailViewController: UIViewController {
	var post: Post?
	var postIdentifier: Int?
	
	init(withPost: Post?, orPostIdentifier postIdentifier: Int?) {
		self.post = post
		self.postIdentifier = postIdentifier
	}
	
	override func viewDidLoad() {
		super.viewDidLoad()

		if let post = self.post {
    	   // configure view with the post
		} else if let identifier = postIdentifier {
		   // fetch post with the identifier
		}
	}
	
	...
}
```

Basically, there are two downsides with this approach. Firstly, we need to make nil checks to decide what state the controller is in. Nil checks doesn't actually explain any state, they are just for understanding an variables has any data inside it. Secondly, with new requirements when we need to add new features to this controller, we keep adding new parameters to initializer method. This will make understanding 
controller's state even harder and making readability so hard, also causing boilerplate code all accross the class.

A Better approach;

```swift
class PostDetailViewController: UIViewController {
	enum State {
		case post(Post)
		case postIdentifier(Int)
	}
	
	let state: PostDetailViewController.State
	
	init(withState state: PostDetailViewController.State) {
		self.state = state
	}
	
	override func viewDidLoad() {
	   super.viewDidLoad()

	   switch state {
        case .post(let post):
            // configure view with the post
        case .postIdentifier(let identifier):
            // fetch post with the identifier
      }
	}

	...
}
```

This way we know the state of controller all accross the class, and we can make decisions, add or remove code blocks, according to the state simply. It is more readable and maintainable now, we don't need to give any affort to know what a nil check actually means beacuse we have the controller's state in our hands already.


### View

States for views can be handy as well. A view can update itself with accordingly the state changes. For instance, let's say we're implementing a custom table view; it can have three states data, loading, empty. It shows data, a loading indicator, or nothing according to state it is in.

Example; 

```swift
class TableView: UITableView {
	enum State {
		case data
		case loading
		case empty
	}
	
	...
	
	func set(state: RequestsTableView.State) {
       switch state {
       case .data:
           setEmptyView(visible: false, animated: true)

           loadingIndicatorView.stopAnimating()
       case .loading:
           setEmptyView(visible: false, animated: true)

           loadingIndicatorView.startAnimating()
       case .empty:
           setEmptyView(visible: true, animated: true)

           loadingIndicatorView.stopAnimating()
        }
    }
}
``` 

Therefore, whenever we update the table view's apperance updating its state is all we need to do. If it needs to display a loading indicator we can simply call `tableView.update(state: .loading)`. Calling the same method with other states as parameter, we can make the table view display data or nothing as well.