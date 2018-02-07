## Adding new responsibilities to controllers

So often we need to make additions to existing controllers. There can be different approaches to follow while implementing these changes. Approach of implementation to pick is important to keep the code as maintainable as possible, or to avoid breaking other parts.

To make things clearer let's assume we have a screen being used for posting videos. VideoPostViewController would be a good name in this scenario. A basic skeleton for the controller would be like below;

```swift
class VideoPostViewController: BaseViewController {
    private let videoURL: URL
    private let uploadManager: VideoUploadManager
        
    // MARK: - Initialization
    
    init(with url: URL, and uploadManager: VideoUploadManager) {
        self.videoURL = url
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

Basically this controller is initialized by the video's local url, and an uploadManager instance which is responsible for uploading the video to the server.

Now assume that we also need to call this controller from another place of codebase with another parameter(with an PHAsset object for instance). We can achieve this requirement with three different ways;

### Optionals approach

We can simply add a new property into existing controller which results as;

```swift
class VideoPostViewController: BaseViewController {
	private let videoURL: URL?
	private let asset: PHAsset?
	private let uploadManager: VideoUploadManager
        
	// MARK: - Initialization
    
	init(with url: URL?, asset: PHAsset?, and uploadManager: VideoUploadManager) {
		self.videoURL = url
		self.uploadManager = uploadManager

		super.init()
	}
    
	required init?(coder aDecoder: NSCoder) {
		fatalError("init(coder:) has not been implemented")
	}
    
	// MARK: - View lifecycle
    
	override func viewDidLoad() {
		super.viewDidLoad()
			
		if let videoUrl = self.videoUrl {
			// use url for some configuration
		} else if let asset = self.asset {
			// use asset for some configuration
		}
			
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
}            
```

Adding a new property caused both properties to be optionals. Because the properties are optionals now we must add nil checks or unwrapping lines whenever we want to use one of these properties. Generally initializing classes with properties that is not optionals would be a better approach to get rid of additional nil checks.

### State approach

As another approach we can embed the optional properties into a state enum like;

```swift
class VideoPostViewController: BaseViewController {
	enum State {
		case url(URL)
		case asset(PHAsset)
	}
	 	 
	private let uploadManager: VideoUploadManager
        
	// MARK: - Initialization
    
	init(with state: VideoPostViewController.State, and uploadManager: UploadManager) {
		self.state = state
		self.uploadManager = uploadManager
        
		super.init()
	}
    
	required init?(coder aDecoder: NSCoder) {
		fatalError("init(coder:) has not been implemented")
	}
    
	// MARK: - View lifecycle
    
	override func viewDidLoad() {
		super.viewDidLoad()

		switch state {
		case .post(let post):
			// configure view with the post
		case .postIdentifier(let identifier):
			// fetch post with the identifier
		}

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
}            
```

With this approach we can avoid nil checks. However we still need to check which property is available to use whenever we need one of them. Also the controller would end up lots of switch statements, because we have no chance to avoid it to access post or identifier values.

### Inheritance approach

We can also leverage inheritance to initialize a new controller with the new property. We can move common code into a new base view controller and create another controller to be initialized with the asset parameter. Thus, there is no need any optional or switch statements. 

Another benefit is that code much more easy to maintain because responsibilities are separated, and we don't think twice while changing something if it affects or breaks another parts.

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

Notice that `VideoAssetPostViewController` has some additional properties and relevant ui logic as well. Because we need some additional effort to access a video url, or video image of a PHAsset variable, we should show a progress bar in the screen to show up while fetching video from PHAsset. This separation of logic saves us from thinking in a controller that already has lots of responsibilities, properties, methods, ending up lightweight, easily understandable controllers rather than massive massive ones that is responsible for everything.    




