---
author: "Jacob Case"
title: "TrashMapper - iOS Project - CoreLocation - Part1"
date: "2022-05-24"
description: "Testing out core location, noting features of core location for user experience, notifications, etc"
tags: ["Project", "Swift", "UIKit", "CoreLocation"]
ShowToc: True
---

## Motivation

In my previous post I had created a list of to-do items. Normally you'd see these broken out in some Trello board. I may migrate to that once I make more headway but for now doing it in this fashion.

1. Outline functionality for:

   - CLLocation Manager & related delegates -----> In progress
   - Camera usability, storing current photo
   - Paths to photos, storage URL, etc
   - userPost Object and related methods
   - userPost structs and definitions
   - Firebase cocoa pod integration

2. Build out the User Interface
   - Add Apple Mapkit
   - Add tableBar with tableView, MapView, userProfile buttons
   - Add getUserLocation button
   - Add addPost button

---

# Core Location

In this post I will cover the functionality I'm looking for with Core Location. Imports, delegates, and error handling to be included. Since the location services are the primary driver in this app I wanted to spend some time solidifying my understanding of the code.

## Imports and Initial Setup

Before setting up some simple methods to query user location, I'll import the following: `import CoreLocation` and `import CoreLocationUI`. I'll be using CoreLocation first to setup a location manager object. CoreLocationUI will eventually be used in tandem with a "get current user location" button.

Next I'll make the current view controller conform to the CLLocationManagerDelegate just to get up and running.

Within the storyboard, I'll make a simple button to request current user location and embed the whole thing in a tab bar controller for future use.

### Simple UI with Button

{{< figure src="InitialUISetup.jpg" >}}

Below is my initial code setup to test out the "Get Location" button.

### Start Updating Location

```
import UIKit
import CoreLocation
import CoreLocationUI

class CurrentLocationViewController: UIViewController, CLLocationManagerDelegate {

    //1. create location manager object
    let locationManager = CLLocationManager()

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.

    }

    //2. Button action to request user data
    @IBAction func getLocation(_ sender: Any) {
        let authStatus = locationManager.authorizationStatus
        if authStatus == .notDetermined {
            locationManager.requestWhenInUseAuthorization()
            return
        }

        //3. Assign current view controller has acting delegate for CLLocationManager
        locationManager.delegate = self
        locationManager.desiredAccuracy = kCLLocationAccuracyNearestTenMeters
        locationManager.startUpdatingLocation()
    }

    //4. Delegate Methods
    func locationManager(_ manager: CLLocationManager, didFailWithError error: Error) {
        print("didFailWithError \(error)")
    }

    func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
        let newLocation = locations.last!
        print("diddUpdateLocations \(newLocation)")
    }

}
```

#### Steps

Along with the code above, the info.plist was updated to allow location access while in use. This is required before actually pulling location data.

1. Creat location manager object
2. Tie button functionality to getLocation method
3. Check authorization, assign delegate, and start updating location
   - Start updating location will return immediately and call didUpdateLocations method
4. Delegate methods, did update locations will simply print out current user location with accuracy of ten meters.

#### Simulator

![post](ShowCoordinates.gif)

## Error handling & Location Requests

#### CLError, domain, and codes

Since getting GPS coordinates and information can be error prone, I need to consider handling a few scenarios:

- User lost service/line of sight with satellite
  - CLError.network or CLError.locationUnknown
- User turned GPS off
  - CLError.network
- User didn't allow app to request location initially
  - CLError.denied

Apple provides general error codes:

```
case locationUnknown
  //A constant that indicates the location manager was unable to obtain a location value right now.
case denied
  //A constant that indicates the user denied access to the location service.
case promptDeclined
  //A constant that indicates the user didn’t grant the requested temporary authorization.
case network
  //A constant that indicates the network was unavailable or a network error occurred.
case headingFailure
  //A constant that indicates the location manager can’t determine the heading.
case rangingUnavailable
  //A constant that indicates ranging is disabled.
case rangingFailure
  //A constant that indicates a general ranging error occurred.
```

With these in mind I will update the `didFailWithError` method.

I think it's important at this point to call out what isn't critical for the app (as defined to this point). This is in part due to the structure of the userPosts which I haven't concretely spelled out yet.

- User heading
- Addresses (reverse geocoding)
- Altitude/speed

Here is the new error handling method and also a getStatus method to check in on where we are with location services.

```Swift
func getStatus() {
    let statusMessage: String

    //Check error code and provide a print output
    //Location error stored with "didFailWithError"

    //pull error domain and error code from location error
    if let error = locationError as NSError? {
        if error.domain == kCLErrorDomain && error.code == CLError.denied.rawValue {
            statusMessage = "Location Services Disabled"
        } else {
            statusMessage = "Error Getting Location"
        }
    } else if !CLLocationManager.locationServicesEnabled() {
        statusMessage = "Location Services Disabled"
    } else if updatingLocation {
        statusMessage = "Searching..."
    } else {
        statusMessage = "Tap 'Get Location' to Start"
    }

    print(statusMessage)
}

// View controller to manage this method
// If there is an error, cast it as NSError and check the code against "unknown location"
func locationManager(_ manager: CLLocationManager, didFailWithError error: Error) {
    print("didFailWithError \(error.localizedDescription)")

    if (error as NSError).code == CLError.locationUnknown.rawValue {
        print("Location Unknown")
        return
    }

    locationError = error
    stopLocationManager()
    getStatus()
}
```

#### Forcing an error

Starting a simulator from scratch and declining the location service for the app, I receive the following error:

```
didFailWithError The operation couldn’t be completed. (kCLErrorDomain error 1.)
```

A few things to explain to myself: The domain error is just a way to communicate which framework the error has come from. According to Apple's documentation there are a number of pre-defined NSError domains. In this case we receive an error from kCLErrorDomain. The error code `error 1` matches against the CLError enum, which is the .denied case.

#### Stop Requesting Location

Another thing to add is a request to stop the location. This will be handy later on so will implement a simple method for it now.

I added an additional button to the main GUI to link with stopLocation method.

```Swift
// Button action to trigger stop updating location method
@IBAction func stopLocation(_ sender: Any) {
    print("Stop Upating Location")
    stopLocationManager()
    getStatus()
}

func stopLocationManager() {
    if updatingLocation {
        locationManager.stopUpdatingLocation()
        locationManager.delegate = nil
        updatingLocation = false
    }
}
```

#### Simulator with Start/Stop Updating Location

![post](StartStopLocation.gif)

## Next Steps

Thinking out loud, it would be good to have functionality that checks if the user location is changing or not. If it's not, there should be no reason to continue updating the location and draining battery.

The error handling is a little disheveled. It might be better represented by a switch statement or something to improve organization.

Having the userPost struct may also be helpful at this time to define just so that I can practice creating and storing posts as needed with necessary location information.
