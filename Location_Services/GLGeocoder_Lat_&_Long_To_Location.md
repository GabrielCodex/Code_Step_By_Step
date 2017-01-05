
# Creating a Converting Lat and Long to location, Map, Pin, And Blue Circle

 For converting Lat and Long into a location


### Step 1 
GLGeocoder converts a coordinate (specified as a latitude and longitude) into a street,city,country, landmark, etc.
Add the line of code above the viewDidLoad but not in it. 


```swift
let geocoder = CLGeocoder()
```

### Step 3 

Add geocoder.reverse to the didUpdateLocations function. GeoCoder: GLGeoCoder takes the latitude and longitude and outputs the phyical location.

```swift
func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
        print("Oh woah, locations updated")
        //    dump(locations)
        guard let validLocation: CLLocation = locations.last else { return }
        
        self.latLabel.text = String(format: "Lat: %0.1f",validLocation.coordinate.latitude)
        self.longLabel.text = "Long: \(validLocation.coordinate.longitude)"
        
        //mapView.setCenter(validLocation.coordinate, animated: true)
        // this line zooms in on the actual location of the user NEW
        mapView.setRegion(MKCoordinateRegionMakeWithDistance(validLocation.coordinate, 500.0, 500.0), animated: true)
       // below code drops a pin. Click on pin and the title pops up
        let pinAnnotation: MKPointAnnotation = MKPointAnnotation()
        pinAnnotation.title = "Hey, Title"
        pinAnnotation.subtitle = "Bring the rain"
        pinAnnotation.coordinate = validLocation.coordinate
        mapView.addAnnotation(pinAnnotation)
        
        // need to add delegeate before this works so currenlty doen't work
        let circleOverlay: MKCircle = MKCircle(center: validLocation.coordinate, radius: 50.0)
        mapView.add(circleOverlay)
        
        // STEP 2 
        geocoder.reverseGeocodeLocation(validLocation) { (placemarks: [CLPlacemark]?, error: Error?) in
            if error != nil {
                dump(error!)
            }
            guard
                let validPlaceMarks: [CLPlacemark] = placemarks,
                let validPlace: CLPlacemark = validPlaceMarks.last
                else {
                    return
            }
            self.geocodeLocationLabel.text = "\(validPlace.name!) \t \(validPlace.locality!)"
        }
        // end of new thing
    }

```
### Step 4 

Remember to create a label

```swift

 internal var geocodeLocationLabel: UILabel = {
        let label: UILabel = UILabel()
        label.font = UIFont.systemFont(ofSize: 18, weight: UIFontWeightHeavy)
        return label
    }()
    
```












