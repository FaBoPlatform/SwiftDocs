# NSURLSession

## Example

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
                                // http status code 200-300の時
                                if httpRes.statusCode >= 200 && httpRes.statusCode < 400{
                                    if let htmldata = data {
                                        let datastring = NSString(data:htmldata, encoding:NSUTF8StringEncoding) as! String
                                        print("data:\(datastring)")
                                    }
                                }
                                // http status code 400-500の時
                                else if httpRes.statusCode >= 400 {
                                        print("エラー")
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
## error code

| error code | 意味 |
| -- | -- |
| -1022 | NSURLErrorDomain |
| -1008 | The Internet connection appears to be offline. |

## http status code

| status code | 意味 |
| -- | -- |
| 2xx系 | 正常処理 |
| 3xx系 | リダイレクト |
| 4xx系 | 異常処理 |
| 5xx系 |内部エラー |




