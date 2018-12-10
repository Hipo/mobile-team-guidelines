In swift we can declare methods in different ways, like;

```swift
func configureCommentCell(_ cell: PostCommentCell, atIndexPath indexPath: IndexPath) {
	...
}
```                  

```swift
func configure(_ cell: PostCommentCell, at indexPath: IndexPath) {
	...
}
```                  

For now there is no strict rule to pick one of two. However, for consistency we recommend to use just one approach for any project.  

Also an invalid signature could be like this;

```swift
func configureComment(cell: PostCommentCell, atIndexPath: IndexPath)
```