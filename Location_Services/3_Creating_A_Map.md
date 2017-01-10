# Map


### Step 1 

Import MapKit

### Step 2 

Need to create a map. Below code is done programmatically

```swift 
 internal var mapView: MKMapView = {
        let map: MKMapView = MKMapView()
        map.mapType = MKMapType.hybridFlyover
        
        return map
    }()


```

### Step 3
Add the mapView to the view

```swift
func setupViewHierarchy() {
        
        // lables
        self.view.addSubview(latLabel)
        self.view.addSubview(longLabel)
        self.view.addSubview(geocodeLocationLabel)
        
        // button
        self.view.addSubview(permissionButton)
        
        STEP 3 // map
        self.view.addSubview(mapView)
        
        
        // actions
        permissionButton.addTarget(self, action: #selector(didPressPermissionsButton(sender:)), for: .touchUpInside)
    }

```

### Step 4 
Add constraints to the mapView

```swift
 func configureConstraints() {
        let _ = [
            latLabel,
            longLabel,
            permissionButton,
            geocodeLocationLabel,
            mapView,
            
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
           
            // map 
            mapView.topAnchor.constraint(equalTo: geocodeLocationLabel.bottomAnchor, constant: 16.0),
            mapView.leadingAnchor.constraint(equalTo: self.view.leadingAnchor),
            mapView.trailingAnchor.constraint(equalTo: self.view.trailingAnchor),
            mapView.bottomAnchor.constraint(equalTo: permissionButton.topAnchor, constant: -16.0),
            
            
            ].map { $0.isActive = true }
        
        
    }

```
