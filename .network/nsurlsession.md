# NSURLSession

```swift

import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        // URLを定義.
        let urlstr: String = "http://www.apple.com"
        
        // URLが正常な書式なら.
        if let url = NSURL(string: urlstr) {
            
            // リクエストを生成.
            let request = NSURLRequest(URL: url)

            // 接続.
            NSURLSession.sharedSession().dataTaskWithRequest(request,
                completionHandler: { (data, response, error) -> Void in
                    dispatch_async(dispatch_get_main_queue(), {
                        
                        if error != nil {
                            if error!.code == -1022 {
                                print("ドメインが存在しない")
                            } else if error!.code == -1009 {
                                print("ネットワークに繋がっていない")
                            }
                        } else {

                            if let httpRes = response as? NSHTTPURLResponse {
                                // Response status 200の時
                                if httpRes.statusCode == 200 {
                                    if let htmldata = data {
                                        let datastring = NSString(data:htmldata, encoding:NSUTF8StringEncoding) as! String
                                        print("data:\(datastring)")
                                    }
                                }
                                // Response status 404の時
                                else if httpRes.statusCode == 404 {
                                        print("ページが存在しません")
                                }
                            }
                        }
                    })
            }).resume()
        }
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
}

```