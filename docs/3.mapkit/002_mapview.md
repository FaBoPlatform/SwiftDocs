# 地図の縮尺の変更

![Preview mapkit002](img/mapkit002.png)

```swift fct_label="Swift 4.x/Swift 3.x"
//
//  ViewController.swift
//  MapKit002
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit
import MapKit

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // MapViewを生成.
        let mapView : MKMapView = MKMapView(frame: CGRect(x: 0, y: 0, width: self.view.frame.size.width, height: self.view.frame.size.height))
        
        // MapViewの中心位置を指定.
        mapView.centerCoordinate = CLLocationCoordinate2DMake(37.331741, -122.030333)
        
        // 縮尺を変更.
        // 倍率を指定.
        let span : MKCoordinateSpan = MKCoordinateSpan(latitudeDelta: 0.1, longitudeDelta: 0.1)
        
        // MapViewで指定した中心位置とMKCoordinateSapnで宣言したspanを指定する.
        let region : MKCoordinateRegion = MKCoordinateRegion(center: mapView.centerCoordinate, span: span)
        
        // MapViewのregionプロパティにregionを指定.
        mapView.region = region
        
        // MapViewをviewに追加.
        self.view.addSubview(mapView)
        
    } 
}
```

```swift fct_label="Swift 2.3"
//
//  ViewController.swift
//  MapKit002
//
//  Created by Misato Morino on 2016/08/15.
//  Copyright © 2016年 Misato Morino. All rights reserved.
//

import UIKit
import MapKit

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // MapViewを生成.
        let mapView : MKMapView = MKMapView(frame: CGRectMake(0, 0, self.view.frame.size.width, self.view.frame.size.height))
        
        // MapViewの中心位置を指定.
        mapView.centerCoordinate = CLLocationCoordinate2DMake(37.331741, -122.030333)
        
        // 縮尺を変更.
        // 倍率を指定.
        let span : MKCoordinateSpan = MKCoordinateSpan(latitudeDelta: 0.1, longitudeDelta: 0.1)
        
        // MapViewで指定した中心位置とMKCoordinateSapnで宣言したspanを指定する.
        let region : MKCoordinateRegion = MKCoordinateRegion(center: mapView.centerCoordinate, span: span)
        
        // MapViewのregionプロパティにregionを指定.
        mapView.region = region
        
        // MapViewをviewに追加.
        self.view.addSubview(mapView)
        
    } 
}
```

## 2.3と3.0の差分

* CGRectMakeが廃止

## Reference
* MKMapView
    * [https://developer.apple.com/reference/mapkit/mkmapview](https://developer.apple.com/reference/mapkit/mkmapview)
* MapKit Data Types
    * [https://developer.apple.com/reference/mapkit/1612567-mapkit_data_types](https://developer.apple.com/reference/mapkit/1612567-mapkit_data_types)
