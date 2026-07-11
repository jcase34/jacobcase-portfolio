---
author: "Jacob Case"
title: "TrashMapper - iOS Project - Development - Part8"
date: "2022-06-13"
description: "Walkthrough on setting up annotations for the MapView, custom annotations, and annotationViews."
tags: ["Project", "Swift", "UIKit", "Annotations"]
ShowToc: True
---

## Updates

In the last post, I mainly focused on the CreatePostViewController and made some minor tweaks to help user experience.

Prior to that were some discussions around Firebase, postModel, and annotations.

In this post I'm going to focus on map annotations and see if I can nail down a customized annotation object and view to display on the map.

---
### Adding Map Annotations

The intention is to create custom annotations/views for all user submitted posts once this app is up and running.

For example, when posts are fetched from Firebase (still sorting that out), there will need to be a dataManager that handles passing those posts into a custom annotation objects. I'll rely on the "viewFor" delegate method to handle creating the annotationView for all of the custom objects, and dequeueing them to make a more CPU efficient way of displaying all of the views on the MapView.


#### Apple Guide to Adding Annotations
Per Apple, there are some steps needed to add annotations on the mapView.

1. Define an annotation object
  - Can be a simple point annotation or custom object that conforms to MkAnnotation Protocol
  - This is the data behind the annotation, the view is separate
  - At minimum needs a coordinate
2. Define an annotation view
  - Is the view a pin? Custom image? What does it look like?
3. Implement the "viewFor" annotation delegate method to handle creating the annotation view

```swift
func mapView(_ mapView: MKMapView,
               viewFor annotation: MKAnnotation) -> MKAnnotationView? {
                 //some code to create an annotation view
                 //and return it to delegate (i.e MapViewController)
                 //which will display the annotation view on the mapView
               }
```

4. Add the annotation object using addAnnotation(someAnnotationObject)

So this is a helpful outline, but doesn't discuss what happens under the hood while code is executing. In the next section I break down the flow in code.

#### Implementation of Adding Annotations in Swift
1. Assign a delegate for MKMapViewDelegate. In this case it will be the MapViewController.

```swift
  //set the current view controller as the delegate for mapView
  mapView.delegate = self
```

2. Register any custom annotation objects. In the previous example, this was a "TaggedLocationAnnotation".
```Swift
  //Use this method to register one or more views that you use to display annotations on your map.
  //Register your classes before adding any annotations to the map.
  //register taggedLocationAnnotation as an MKMarkerAnnotation
  mapView.register(MKMarkerAnnotationView.self, forAnnotationViewWithReuseIdentifier: NSStringFromClass(TaggedLocationAnnotation.self))

```

3. Create the custom annotation object
```Swift
  //Define the annotation object
  mapAnnotation = TaggedLocationAnnotation()
```
Where a TaggedLocationAnnotation object is defined as:

```Swift
  class TaggedLocationAnnotation: NSObject, MKAnnotation {

    // This property must be key-value observable, which the `@objc dynamic` attributes provide.
    @objc dynamic var coordinate = CLLocationCoordinate2D(latitude: 37.795_316, longitude: -122.393_760)

    var title: String? = NSLocalizedString("Trash in park", comment: "test")

  }
```

4. Add annotation -> This method will ask the delegate to execute the "viewFor" annotation method.
```Swift
  //add sample map annotation
  mapView.addAnnotation(mapAnnotation!)
```

5. MapViewController to execute viewFor annotation delegate method.
```Swift
  //delegate method to create an MKAnnotation view
  func mapView(_ mapView: MKMapView, viewFor annotation: MKAnnotation) -> MKAnnotationView? {

        //continue on if the annoatation to be added is not a user location
        guard !annotation.isKind(of: MKUserLocation.self) else {
            return nil
        }

        //create optional annotationView
        var annotationView: MKAnnotationView?

        //if we have a valid annotation, cast it as taggedLocationAnnotation
        //pass the annotation to the setupTaggedLocationAnnotation function
        //and return output to annotationView to be added to mapView
        if let annotation = annotation as? TaggedLocationAnnotation {
            annotationView = setupTaggedLocationAnnotation(for: annotation, on: mapView)
        }

        return annotationView
    }
```
Below is the implementation for the function setupTaggedLocationAnnotation(), which is essentially overriding our delegate method and adding customizations to the annotationView.

```swift
func setupTaggedLocationAnnotation(for annotation: TaggedLocationAnnotation, on mapView: MKMapView) -> MKAnnotationView {

        //identifier is the custom TaggedLocationAnnotation class
        let identifier = NSStringFromClass(TaggedLocationAnnotation.self)

        //re-use annotations by dequeuing
        let annotationView = mapView.dequeueReusableAnnotationView(withIdentifier: identifier, for: annotation)

        //cast the annotationView as a MKMarkerAnnotation
        if let markerAnnotationView = annotationView as? MKMarkerAnnotationView {

            //MkMarkerAnnotation allows customization
            markerAnnotationView.animatesWhenAdded = true
            markerAnnotationView.canShowCallout = true
            markerAnnotationView.markerTintColor = UIColor.purple



            // Provide an image view to use as the accessory view's detail view.
            let pinImage = UIImage(named: "trash_in_park.jpg")
            let newPinImage = pinImage?.resized(withBounds: CGSize(width: 100, height: 100))


            markerAnnotationView.detailCalloutAccessoryView = UIImageView(image: newPinImage)
        }

        return annotationView
    }

```

### Where to go from here?

Even though this post is brief, I wanted to understand the annotation objects, views, and how they are created/re-used by Apple's functions.

Now that I understand how to create annotationViews for all of the objects, I will need to create additional methods that extract certain contents from fetched userPosts, and loading images, subtitles, and other info into each annotationView.








---
