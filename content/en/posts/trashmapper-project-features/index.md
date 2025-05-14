---
author: "Jacob Case"
title: "TrashMapper - iOS Project - Features"
date: "2022-05-12"
description: "Requirements for project application, TrashMapper"
tags: ["Project", "Swift", "UIKit"]
ShowToc: True
---

I had started learning from www.hackingwithswift.com back in February and made it about half way through the "100 Days of Swift". One of the projects at that time required integrating Apple's Mapkit and assigning info points on certain coordinates.

While completing that project I tried to think of applications that would be interesting to make outside of what's available today. One that came to mind was tagging areas where there are large amounts of litter or garbage. A step further would be a crowd-sourced application where many people contribute to tagging locations on a map, coordinating clean-up groups, and even showing them on the map real-time.

Some of the inspiration came from the time I lived in San Diego. Anytime I was at the beach I usually noticed at least one person walking around with a grabber and bucket doing a trash walk.

---
## Requirements

Before diving straight into coding, I thought I'd lay out some requirements/features for myself to help take the idea from head to sketch.

### Description

TrashMapper is a crowd-sourced application where users can submit a post of where garbage has been observed. The post will be uploaded to a database and distributed to other users similar to other social media applications.


### Features - Initial Draft

- Use of a map to display user submitted markers/posts
- Login / Sign-up via email (firebase?)
- Current Location Button
- Submit post Button
- Bottom controller bar on main mapview screen
  - Button for tableview showing posts with pictures, string, distance to site
  - Button for user settings (account information, deletion, update, etc)
  - Button for main mapview
- On main mapview
  - Circle button with 'add' symbol for adding a post
  - Button for current user location (zooms map to current user location)
  - Markers on map that are user selectable
- Selecting a Marker on Mapkit
  - Shows a picture from a post
  - Able to open marker post to another view with larger picture and description
- Selecting a post on tableView
  - Shows a larger picture of post
  - show distance
  - show description string

### User settings

- Delete account
- Update email
- Update password


### User/Post Data

Users will have the ability to sign up via email and then contribute to the application. Users will have ability to delete their account via a settings tab or page.

User posts will consist of:
- User ID
- Date
- Picture showing the type of garbage/litter
- A short description (limited in characters?)
- Current user location (coordinates) where trash was observed

---
