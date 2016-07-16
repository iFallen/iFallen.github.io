---
layout: post
title: iTunes SearchUtils 
excerpt: "Test:iTunes SearchUtils Software/eBook.."
tags: [iOS,Swift]
categories: [iOS-Swift]
comments: true
---

[版权所有,转载请注明出处!](http://ifallen.github.io)


## `iTunes SearchUtils Software/eBook..`
---


```

//
//  Search.swift
//  StoreSearch
//
//  Created by JuanFelix on 3/31/16.
//  Copyright © 2016 JuanFelix. All rights reserved.
//

///itunes software  search

import UIKit

typealias SearchComplete = (content: [String : AnyObject]?,success: Bool) -> Void

class SearchUtils {
    class func searchAppsWithName(name: String,completion: SearchComplete?) {
        
        if !name.isEmpty {
            // 1
            let session = NSURLSession.sharedSession()
            // 2 检索API地址
            let url = urlWithSearchText(name)
            // 3
            let dataTask = session.dataTaskWithURL(url, completionHandler: { (data, response, error) in
                // 4
                if let error = error where error.code == -999{//Request canceled
                    dispatch_async(dispatch_get_main_queue()) {
                        completion?(content:nil,success:false)
                    }
                    
                    return
                }
                if let httpResponse = response as? NSHTTPURLResponse where httpResponse.statusCode == 200,
                     let data = data, dictionary = self.parseJSON(data) {
                    dispatch_async(dispatch_get_main_queue()) {
                        completion?(content:dictionary,success:true)
                    }
                }
            })
            // 5
            dataTask.resume()
        }else {
            dispatch_async(dispatch_get_main_queue()) {
                completion?(content: nil,success: false)
            }
        }
    }
    //term  : search text
    //limit : data count
    //entity: search type eg:software/ebook
    private class func urlWithSearchText(searchText: String) -> NSURL {
        let escapedSearchText = searchText.stringByAddingPercentEncodingWithAllowedCharacters(NSCharacterSet.URLQueryAllowedCharacterSet())!
        let urlString = String(format: "https://itunes.apple.com/search?term=%@&limit=30&entity=software", escapedSearchText)
        return NSURL(string: urlString)!
    }
    
    //Json Data To Obj
    private class func parseJSON(data: NSData) -> [String: AnyObject]? {
        do{
            return try NSJSONSerialization.JSONObjectWithData(data, options: []) as? [String: AnyObject]
        } catch {
            print("JSON Error: \(error)")
            return nil
        }
    }
}
```
	
