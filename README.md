# swift_component_backgroundimage

- imageView object를 parameter로 만들거나 global variable로 선언해야함

```swift

/** Set up Image Background **/
//arg1 - hosting view
//arg2 - network
//arg3 - local
func setUpImageBackground(parentView: UIView, urlString: String? = nil, imageName: String? = nil) {
    guard let backgroundImageView: UIImageView = backgroundImageView else {
        print("\(TAG) - setUpImageBackground(): background imageview doesn't exist!")
        return
    }
    parentView.addSubview(backgroundImageView)
    backgroundImageView.topAnchor.constraint(equalTo: parentView.topAnchor).isActive = true
    backgroundImageView.leftAnchor.constraint(equalTo: parentView.leftAnchor).isActive = true
    backgroundImageView.rightAnchor.constraint(equalTo: parentView.rightAnchor).isActive = true
    backgroundImageView.bottomAnchor.constraint(equalTo: parentView.bottomAnchor).isActive = true
    if let urlString: String = urlString {
        print("\(TAG) - setUpImageBackground(): Network Image Background SetUp")
        if let url: URL = URL(string: urlString) {
            URLSession.shared.dataTask(with: url) { (data, response, error) in
                if let error: Error = error {
                    print("\(self.TAG) - setUpImageBackground: \(error)")
                    return
                }
                if let data = data {
                    if let image: UIImage = UIImage(data: data) {
                        DispatchQueue.main.async {
                             self.backgroundImageView?.image = image
                        }
                    }
                }
            }.resume()
        }
    } else {
        print("\(TAG) - setUpImageBackground(): Local Image Background SetUp")
        guard let localFileName: String = imageName else {
            print("\(TAG) - setUpImageBackground(): No Local Image is was provided")
            return
        }
        backgroundImageView.image = UIImage(named: localFileName)
    }
    parentView.sendSubviewToBack(backgroundImageView)
}


```
