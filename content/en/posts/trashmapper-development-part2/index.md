---
author: "Jacob Case"
title: "TrashMapper - iOS Project - CoreLocation - Part2"
date: "2022-05-25"
description: "Incorporate accuracy checks with core location, TimeOuts for no location found, integrate start/stop location into single button"
tags: ["Project", "Swift", "UIKit", "CoreLocation"]
ShowToc: True
---

## Motivation

In the last post, I was able to tap a button on screen and begin collecting location data. I also had a stop button that would invoke `stopLocationManager()`.

Within this post I'll cover updates to the `getLocation()` method, merging of start/stop updating location button, and bounding how long we search for new locations based on some tools that Apple provides us.

---

## Location Change & Updating

Here I show that the start and stop button merged into a single button. The title of the button updates as we begin searching for current location.

I initially set the location "None" as provided Xcode Simulator. I then changed the location to "Apple" and it began updating with the headquarters coordinates. This is good and shows that the app will wait until there is a valid location before it begins updating.

![post](StartandStopMerged.gif)

## Timeout for Unknown Location

Although the core location service waits for a valid location, this can be a problem. If a valid location is never found, core location will continue searching forever. For this reason a 30second timer is implemented in startLocationManager() and will call didTimeOut() method. This will then call stopLocationManager() and throw a customized NSError object to locationError variable.

```Swift
func startLocationManager() {
    if CLLocationManager.locationServicesEnabled() {
        locationManager.delegate = self
        locationManager.desiredAccuracy = kCLLocationAccuracyNearestTenMeters
        locationManager.startUpdatingLocation()
        updatingLocation = true

        timer = Timer.scheduledTimer(timeInterval: 30, target: self, selector: #selector(didTimeOut), userInfo: nil, repeats: false)
    }
}

@objc func didTimeOut() {
    print("***Time Out***")
    if location == nil {
        stopLocationManager()
        locationError = NSError(domain: "MyLocationsErrorDomain", code: 1, userInfo: nil)
        getStatus()
    }
}
```

It's helpful to see how to build a custom NSError with domain name, code, and userinfo.

## Stop Updating with No Location Change

On top of incorporating a timer, I've also made changes to how often the location updates based on current vs new locations.

In this case, we check if locations are older than 5 seconds. If so, we ignore them. I also check the horizontal accuracy and if it's less than 0 (invalid) it's ignored.

If we don't have a location, then use last good known location or if the horizontal accuracy of current location is worse than new location, swap the two.

Lastly, if the accuracy doesn't appear changed then stop updating the location because the user hasn't moved. This will preserve battery by disabling the radios upon stopping the location manager from updating.

```Swift
func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
    let newLocation = locations.last!
    print("didUpdateLocations \(newLocation)")

    //Last location is old/cached, ignore and continue search
    if newLocation.timestamp.timeIntervalSinceNow < -5 {
        return
    }

    //ignore invalid accuracy
    if newLocation.horizontalAccuracy < 0 {
        return
    }

    //If there was no previous location, or the accuracy is larger than the most recent one, adopt new location
    if location == nil || location!.horizontalAccuracy > newLocation.horizontalAccuracy {
        //new location to be used, clear error
        locationError = nil
        location = newLocation
    }

    //current location and new location accuracy the same, stop updating location
    if newLocation.horizontalAccuracy <= locationManager.desiredAccuracy {
        print("locations similar")
        stopLocationManager()
    }
    getStatus()
}
```

## Next Steps

Now that the core location code is functional and decently robust, I'll start adding views to capture the user post content.
