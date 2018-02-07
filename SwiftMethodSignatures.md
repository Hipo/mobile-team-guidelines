In swift we can declare methods in different ways, like;

```swift
fileprivate func configureCommentCell(_ cell: PostCommentCell, atIndexPath indexPath: IndexPath) {
	...
}
```                  

```swift
fileprivate func configure(_ cell: PostCommentCell, at indexPath: IndexPath) {
	...
}
```                  

For now there is no strict rule to pick one of two. However, for consistency we recommend to use the approach which is used in an existing project.  

