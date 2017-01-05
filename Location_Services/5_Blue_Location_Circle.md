# Adding Blue Circle

### Step 1 
Add MKMapViewDelegate to the class 

```swift
class DemoLocationViewController: UIViewController, CLLocationManagerDelegate, MKMapViewDelegate 

``` 
### Step 2 
In viewDidLoad make mapView the delegeate

```swift 

mapView.delegate = self
```

### Step 3
```swift
// MARK: - MKMapView Delegeate
    
    func mapView(_ mapView: MKMapView, rendererFor overlay: MKOverlay) -> MKOverlayRenderer {
        let circleRenderer: MKCircleRenderer = MKCircleRenderer(circle: overlay as! MKCircle)
    
        circleRenderer.lineWidth = 1.0
        circleRenderer.strokeColor = UIColor.blue
        circleRenderer.fillColor = UIColor.blue.withAlphaComponent(0.2)
        return circleRenderer
    }
```

### Step 4 
Add the below to the fuction: func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation])
```swift

let circleOverlay: MKCircle = MKCircle(center: validLocation.coordinate, radius: 50.0)
        mapView.add(circleOverlay)
        
```

