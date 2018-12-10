## Adding states to views

We often need to add states to views for several situations. Enums are very handy to keep states, enabling us to set different behaviours for the view with different states.

### Use case

A view can update itself with accordingly the state changes. For instance, let's say we're implementing a custom table view; it would have three states as data, loading, empty. It shows data, a loading indicator, or nothing according to state it is in.

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

Therefore, whenever we update the table view's apperance, updating the state is just what we need to do. If it needs to display a loading indicator we can simply call `tableView.update(state: .loading)`. Likewise, calling the same method with other states as parameter, we can make the table view display data or nothing as well.