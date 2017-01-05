
# Converting Latitude and Longitude to location

 For converting Latitude and Longitude into a location
 
### Step 1 
GLGeocoder converts a coordinate (specified as a latitude and longitude) into a street,city,country, landmark, etc.
Add the line of code above the viewDidLoad but not in it. 

```swift
let geocoder = CLGeocoder()
```

### Step 2 

Add geocoder.reverse to the didUpdateLocations function. geocoder(aka GLGeoCoder) takes the latitude and longitude and outputs the phyical location.

```swift
func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
        print("Oh woah, locations updated")
        //    dump(locations)
       // Start of Step 2 
        guard let validLocation: CLLocation = locations.last else { return }
    
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












