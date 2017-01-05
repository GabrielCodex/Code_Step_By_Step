
# Creating a Map, Pin, And Blue Circle

### Step 1 for putting a map on the screen

Import MapKit

### Step 2 

Add geocoder.reverse to the didUpdateLocations function. 

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

### Step 3 
Need to create a map. Below code is done programmatically

```swift 
 internal var mapView: MKMapView = {
        let map: MKMapView = MKMapView()
        map.mapType = MKMapType.hybridFlyover
        
        return map
    }()


```

### Step 4 
Add the mapView to the view

```swift
func setupViewHierarchy() {
        
        // lables
        self.view.addSubview(latLabel)
        self.view.addSubview(longLabel)
        self.view.addSubview(geocodeLocationLabel)
        
        // button
        self.view.addSubview(permissionButton)
        
        STEP 4 // map
        self.view.addSubview(mapView)
        
        
        // actions
        permissionButton.addTarget(self, action: #selector(didPressPermissionsButton(sender:)), for: .touchUpInside)
    }

```

### Step 5 
Add constraints to the mapView

```swift
 func configureConstraints() {
        let _ = [
            latLabel,
            longLabel,
            permissionButton,
            geocodeLocationLabel,
     */STEP 5/*  mapView,
            
            ].map{ $0.translatesAutoresizingMaskIntoConstraints = false }
        
        let _ = [
            // labels
            latLabel.topAnchor.constraint(equalTo: self.topLayoutGuide.bottomAnchor, constant: 8.0),
            latLabel.centerXAnchor.constraint(equalTo: self.view.centerXAnchor),
            
            longLabel.topAnchor.constraint(equalTo: latLabel.bottomAnchor, constant: 8.0),
            longLabel.centerXAnchor.constraint(equalTo: self.view.centerXAnchor),
            
            geocodeLocationLabel.topAnchor.constraint(equalTo: longLabel.bottomAnchor),
            geocodeLocationLabel.centerXAnchor.constraint(equalTo: self.view.centerXAnchor),
            
            // buttons
            permissionButton.bottomAnchor.constraint(equalTo: self.bottomLayoutGuide.topAnchor, constant: -16.0),
            permissionButton.centerXAnchor.constraint(equalTo: self.view.centerXAnchor),
           
           STEP 5 // map 
            mapView.topAnchor.constraint(equalTo: geocodeLocationLabel.bottomAnchor, constant: 16.0),
            mapView.leadingAnchor.constraint(equalTo: self.view.leadingAnchor),
            mapView.trailingAnchor.constraint(equalTo: self.view.trailingAnchor),
            mapView.bottomAnchor.constraint(equalTo: permissionButton.topAnchor, constant: -16.0),
            
            
            ].map { $0.isActive = true }
        
        
    }

```










