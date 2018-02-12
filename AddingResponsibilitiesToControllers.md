## Adding new responsibilities to controllers

So often we need to make additions to existing controllers. There can be different approaches to follow while implementing these changes. Approach of implementation to pick is important to keep the code as maintainable as possible, or to avoid breaking other parts.

To make things clearer let's assume we have a screen being used for posting videos.

### Inheritance approach

We can leverage inheritance to initialize a new controller with the new property. We can move common code into a new base view controller and create another controller to be initialized with the asset parameter.

One benefit is that code much more easy to maintain because responsibilities are separated, and we don't think twice while changing something if it affects or breaks another parts.

```swift
class BaseVideoPostViewController: BaseViewController {
    private let uploadManager: VideoUploadManager
        
    // MARK: - Initialization
    
    init(with uploadManager: UploadManager) {
        self.uploadManager = uploadManager
        
        super.init()
    }
    
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    // MARK: - View lifecycle
    
    override func viewDidLoad() {
        super.viewDidLoad()

			...
    }
    
    // MARK: - Actions
    
    func postButtonDidTap(_ button: UIButton) {
			createVideoPost()
    }
    
    // MARK: - Video Upload
            
    private func createVideoPost() {
			...
    }            
```
```swift
class VideoURLPostViewController: BaseVideoPostViewController {
    private let videoURL: URL

    init(withVideoURL url: URL, and uploadManager: VideoUploadManager) {
        self.videoURL = url

        super.init(with: uploadManager)
    }
    
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}
```
```swift
class VideoAssetPostViewController: BaseVideoPostViewController {
	private let asset: PHAsset
	private lazy var progressView = UIProgressView()
	private let assetFetcher: VideoAssetFetcher

	init(with asset: PHAsset, and uploadManager: VideoUploadManager) {
		self.asset = asset
		self.assetFetcher = VideoAssetFetcher(with: asset)
        
		super.init(with: uploadManager)        
	}
    
	required init?(coder aDecoder: NSCoder) {
		fatalError("init(coder:) has not been implemented")
	}
    
	override func viewDidLoad() {
		super.viewDidLoad()
        
		setupProgressViewLayout()
		displayVideoThumbnail()
        
		updateRightNavigationBarButton()
	}

	// MARK: - Layout
    
	private func setupProgressViewLayout() {
		...
	}
    
	fileprivate func displayVideoThumbnail() {
		...
	}
    
	fileprivate func updateRightNavigationBarButton() {
		...
	}
    
	// MARK: - Visibility
    
	fileprivate func removeProgressViewLayoutAnimated() {
		...
	}    		
}
```

Notice that `VideoAssetPostViewController` has some additional properties and relevant ui logic as well. Because we need some additional effort to access a video url, or video image of a PHAsset variable, we should show a progress bar in the screen to show up while fetching video from PHAsset. This separation of logic saves us from thinking in a controller that already has lots of responsibilities, properties, methods. We have lightweight, easily understandable controllers rather than massive massive ones that is responsible for everything, with this approach while adding a new responsibility.




