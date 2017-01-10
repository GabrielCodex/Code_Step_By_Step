# Location Services, latitude and longitude

## Gabriel remember the permission button. Refer to the complete project to see it. Remember to add it here

### Step 1

a. Import CoreLocation
```swift
Import CoreLocation
```

b. In your viewController add the delegate to the class, CLLocationManagerDelegate.
```swift
class DemoLocationViewController: UIViewController, CLLocationManagerDelegate {
```

c. In the Plist rmember to add, "Privacy - Location Always Usage Description" and add a description

### Step 2
Create a location manager to determine how accurate the gps setting should be. Without it would be constantly updating and drain the battery.

```swift
let locationManager: CLLocationManager = {
        let locMan: CLLocationManager = CLLocationManager()
        // more here later
        locMan.desiredAccuracy = 100.0
        locMan.distanceFilter = 50.0
        return locMan
    }()
```
### Step 3 
In viewDidLoad remember to to set the locationManager as a delegate.

```swift
locationManager.delegate = self
```
### Step 4
@@ Investiage more. The delegate calls and handles the authorization status


```swift
// MARK: - CLLocationManager Delegate
    func locationManager(_ manager: CLLocationManager, didChangeAuthorization status: CLAuthorizationStatus) {
        
        switch status {
        case .authorizedAlways, .authorizedWhenInUse:
            print("All good")
            manager.startUpdatingLocation()
            //      manager.startMonitoringSignificantLocationChanges()
        case .denied, .restricted:
            print("NOPE")
        case .notDetermined:
            print("IDK")
            locationManager.requestAlwaysAuthorization()
        }
    }
 ```
### Step 5 

The format used in self.latLabel allows you to select how many decimal place by changing the number.
```swift

func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
        print("Oh woah, locations updated")
        //    dump(locations)
        
        guard let validLocation: CLLocation = locations.last else { return }
        
        self.latLabel.text = String(format: "Lat: %0.1f",validLocation.coordinate.latitude)
        self.longLabel.text = "Long: \(validLocation.coordinate.longitude)"
    }

```

### Step 6 

I need this for error handling
```swift

func locationManager(_ manager: CLLocationManager, didFailWithError error: Error) {
        print("Error encounter")
        dump(error)
    }

```
### Step 7 
When the user presses the button for location services to begin 
``` swift

// MARK: - Actions
    internal func didPressPermissionsButton(sender: UIButton) {
        print("Tapped Permissions!")
        
        // Check for permissions
        switch CLLocationManager.authorizationStatus() {
            
        case .authorizedAlways, .authorizedWhenInUse:
            print("All good")
            
        case .denied, .restricted:
            print("NOPE")
            
            // UIApplication
            guard let validSettingsURL: URL = URL(string: UIApplicationOpenSettingsURLString) else { return }
            UIApplication.shared.open(validSettingsURL, options: [:], completionHandler: nil)
            
        case .notDetermined:
            print("IDK")
            locationManager.requestAlwaysAuthorization()
        }
        
    }


```
# The End



![](https://cloud.githubusercontent.com/assets/17558242/21366803/6930d752-c6c9-11e6-845b-e5f0c2d9e917.png)
