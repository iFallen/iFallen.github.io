---
layout: post
title: NSURLSession GET/POST/IMAGE/JSON
excerpt: "Test:NSURLSession-GET/POST/Image/JSON"
tags: [iOS,Swift]
comments: true
---

[版权所有,转载请注明出处!](http://ifallen.github.com)


## `NSURLSession Piece Code For Beginnner`
---

#### 1. `GET Request`

```
func getRequest() {
        let session = NSURLSession.sharedSession()
        let task = session.dataTaskWithURL(NSURL(string: "http://127.0.0.1:5000/api/list")!) { (data, response, error) in
            if let data = data {
                let string = String.init(data: data, encoding:NSUTF8StringEncoding)
                print(string)
                do{
                    let obj = try NSJSONSerialization.JSONObjectWithData(data, options: []) as! [String:AnyObject]
                    let results = obj["results"] as? [String]
                }catch{
                    print(error)
                }
            }
        }
        task.resume()
    }
```

#### 2. `POST Request`


```
func postRequest() {
        let session = NSURLSession.sharedSession()
        let request = NSMutableURLRequest(URL: NSURL(string: "http://127.0.0.1:5000/api/login")!)
        request.HTTPMethod = "POST"
        let params = "name=admin&password=admin"
        request.HTTPBody = params.dataUsingEncoding(NSUTF8StringEncoding)
        let task = session.dataTaskWithRequest(request) { (data, response, error) in
            if let data = data {
                let string = String.init(data: data, encoding:NSUTF8StringEncoding)
                print(string)
                do{
                    let obj = try NSJSONSerialization.JSONObjectWithData(data, options: []) as! [String:AnyObject]
                    let results = obj["results"] as? [String]
                }catch{
                    print(error)
                }
            }
        }
        task.resume()
    }
```

#### 3. `Load Image`

```
func loadWebImage() {
        let session = NSURLSession.sharedSession()
        let task = session.downloadTaskWithURL(NSURL(string: "http://git.oschina.net/qcrm/ImageTemp/raw/master/1.jpg")!) { (url:NSURL?, response:NSURLResponse?, error:NSError?) in
            if error != nil {
                print("Load Image Data Failed...")
            }else {
                if let url = url, data = NSData(contentsOfURL: url),image = UIImage(data: data) {
                    dispatch_async(dispatch_get_main_queue(), { 
                        self.imgCenter.image = image
                    })
                }
            }
        }
        task.resume()
    }
```
	
