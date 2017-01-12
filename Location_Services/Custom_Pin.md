### Custom Pin code 

swift ```

func mapView(_ mapView: MKMapView, viewFor annotation: MKAnnotation) -> MKAnnotationView? {
    // Don't want to show a custom image if the annotation is the user's location.
    guard !(annotation is MKUserLocation) else {
        return nil
    }

    // Better to make this class property
    let annotationIdentifier = "AnnotationIdentifier"

    var annotationView: MKAnnotationView?
    if let dequeuedAnnotationView = mapView.dequeueReusableAnnotationView(withIdentifier: annotationIdentifier) {
        annotationView = dequeuedAnnotationView
        annotationView?.annotation = annotation
    }
    else {
        annotationView = MKAnnotationView(annotation: annotation, reuseIdentifier: annotationIdentifier)
        annotationView?.rightCalloutAccessoryView = UIButton(type: .detailDisclosure)
    }

    if let annotationView = annotationView {
        // Configure your annotation view here
        annotationView.canShowCallout = true
        annotationView.image = UIImage(named: "yourImage")
    }

    return annotationView
}


```
