---
author: "Jacob Case"
title: "TrashMapper - iOS Project - Development - Part9"
date: "2022-06-13"
description: "Implementation of FireBase, Real-time database and Storage"
tags: ["Project", "Swift", "UIKit", "Annotations"]
ShowToc: True
---

## Motivation

In this post I want explore the option of using both CoreData and Firebase and performing a sync between them. Early in the brainstorming stages I brought up this topic but dismissed it due to some research around FireBase FireStore and ability to handle storing data offline and then syncing when service is available.

Doing some reading on StackOverFlow, it sounds like it's always preferred to have a single database managing both on and offline content. There are cases ([example](https://stackoverflow.com/questions/45558384/firebase-database-local-storage)) where existing apps use both CoreData and Firebase realtime DB and storage and it works well.

For learning sake, I'd like to explore using CoreData with Realtime DB and storage. This will require thinking about both local datastore data structures and cloud JSON structure. Later in this post I also hope to break down which commands will be used for setting up the flat cloud JSON structure and how to setup those functions.

---
## CoreData & FireBase (Realtime DB / Storage)

I want to briefly outline the functional differences both CoreData and Firebase will serve within this app:

#### FireBase Utility
- Creating users
- Deleting users
- User Authentication
- Storing post data in cloud (sync with CoreData)

#### CoreData Utility
- Storing post data locally

#### DataManager Utility
- Checking for
---
