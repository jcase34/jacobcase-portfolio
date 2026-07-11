---
author: "Jacob Case"
title: "TrashMapper - iOS Project - Development - Part3"
date: "2022-05-26"
description: "Integration of Firebase into project. Implement MapView and updated Tab bar/Nav bar controllers."
tags: ["Project", "Swift", "UIKit", "CoreLocation"]
ShowToc: True
---

## Updates

This post will be brief but cover some key points moving forward in this project.

From my previous post outlining some key steps:

1. Outline functionality for:

   - CLLocation Manager & related delegates
     - Mostly complete, major functionality included
   - Camera usability, storing current photo
   - Paths to photos, storage URL, etc
   - userPost Object and related methods
   - userPost structs and definitions
   - Firebase cocoa pod integration
     - Added, no analytics included for now. May integrate later

2. Build out the User Interface
   - Add Apple Mapkit
     - Added to unique view controller with tab
     - Needs constraints on the view/tab
   - Add tableBar with tableView, MapView, userProfile buttons
     - In progress, two tab bars included
     - May not do the tableView for initial release
   - Add getUserLocation button
   - Add addPost button

## Simulating on MapView and Tabs

Views coming together nicely. To link the tab bar controllers to other views I used the `Relationship Segue -> View Controller`.

This [site](https://guides.codepath.com/ios/Tab-Bar-Controller-Guide) has some nice reference info on how to use tab bar controllers and set their relationships.

![Tabs](MapViewTabBar.gif)

## Next steps

### Requiring Service?

In the first stages of building this app, I had it in my head that I was going save user post data on device prior to sending to the cloud.

Last night however, as I lay watching this cinematic masterpiece, I realized I was making the requirements too strict for this initial release.

{{< youtube 6CNivRkEBrA >}}

Instead of having a complex 'feed' check where I verify that all posts on device are uploaded to cloud and vice versa, I'm just going to require that the device have good GPS and network connection in order to submit a post.

### Building out the Data Model

What I'd also like to tackle soon is building out the Data Model that I plan to upload to firebase when a post is committed. I'll most likely start with just a picture, string, date, and time.

Here is a first draft that I will revisit.

```Swift
public struct userPost {
  let date: Date                //current date
  let time: Time                //current time
  let username: userName        //username of who submitted the post
  var userLocation: CLLocation  //User location (lat/long) to be submitted in post
  let imageURL: URL             //where to select image either on device or image picker
  let catageory:                //...some category for trash quantity/type
  let description: String       //Allow user to write out a short description of tagged location?
}
```

### Adding Buttons to MapView

This last one is self explanatory. Add buttons for both creating a user post and zooming to current location.
