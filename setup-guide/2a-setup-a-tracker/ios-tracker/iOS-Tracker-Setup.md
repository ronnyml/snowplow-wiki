<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [**Step 2: Setup a Tracker**](Setting-up-a-Tracker) > [**iOS tracker**](iOS-tracker-setup)

## Contents

- 1. [Overview](#overview)
- 2. [Integration options](#integration-options)
  - 2.1 [Tracker compatibility](#compatibility)  
  - 2.2 [Dependencies](#dependencies)
- 3. [Setup](#setup)
  - 3.1 [CocoaPods](#cocoapods)
  - 3.2 [Manual setup](#manual_setup)

<a name="overview" />
## 1. Overvew

The [Snowplow iOS Tracker](https://github.com/snowplow/snowplow-ios-tracker) lets you add analytics to your [iOS] [ios]-based applications.

The Tracker should be relatively straightforward to setup if you are familiar with iOS development using CocoaPods.

Ready? Let's get started.

[Back to top](#top)

<a name="integration-options" />
## 2. Integration options

<a name="compatibility" />
### 2.1 Tracker compatibility

With iOS backward compatibility is limited to a small range which it makes it easy to support. Hence, our goal was to make the iOS Tracker compatible with iOS 6+ with easy availability via CocoaPods.

[Back to top](#top)

<a name="dependencies" />
### 2.2 Dependencies

The tracker has dependencies limited to the [AFNetworking][afnetworking] and [FMDB][fmdb] libraries for networking and database management respectively. Both of which have iOS 6+ support as well. If you're installing via CocoaPods, these dependencies are recursively downloaded for your project so you don't have to worry about it.

[Back to top](#top)

<a name="setup" />
## 3. Setup

<a name="cocoapods" />
### 3.1 CocoaPods
We support installing the iOS Tracker via CocoaPods since it's the easiest way to install the tracker. Doing so is simple:

1. Install CocoaPods using `gem install cocoapods`
2. Create the file `Podfile` in your XCode project directory, if you haven't created one already
3. Add the followinling line into it:
```
pod 'SnowplowTracker'
```
4. Run `pod install` in the same directory

[Back to top](#top)

<a name="manual_setup" />
### 3.2 Manual Setup

If you prefer not to use CocoaPods, you can grab the tracker from our [GitHub repo][ios-tracker-github] and import it into your project.

#### 3.2.1 Clone the tracker

First, git clone the latest version of the tracker to your local machine:

    git clone https://github.com/snowplow/snowplow-objc-tracker.git

If you don't have git installed locally, [install it] [git] first.

#### 3.2.2 Copy the tracker into your project

You only need to copy the tracker's `Snowplow` sub-folder into your XCode project's folder. The command will look something like this:

    cp -r snowplow-objc-tracker/Snowplow MyObjcApp/MyObjcApp/

Replace `MyObjcApp` with the name of your own app, and tweak the source code sub-folder accordingly.

Next, drag and drop the sub-folder `MyObjcApp/MyObjcApp/Snowplow` into your XCode project's workspace:

[[/setup-guide/images/setup-objc-tracker-manual-1.png]]

The suggested defaults for adding `Snowplow` are fine, so click **Finish**:

[[/setup-guide/images/setup-objc-tracker-manual-2.png]]

#### 3.2.1 Add the FMDB dependency

The tracker is dependent on [FMDB] [fmdb] version 2.3, an Objective-C wrapper around SQLite.

As before, git clone it and copy the source into your XCode project's folder:

```
git clone https://github.com/ccgus/fmdb.git
cd fmdb && git checkout v2.3
cp -r src/fmdb ../MyObjcApp/MyObjcApp/
```

As before, drag and drop the sub-folder `MyObjcApp/MyObjcApp/fmdb` into your XCode project's workspace.

Finally, you will need to edit `Snowplow/SnowplowEmitter.m` and `Snowplow/SnowplowEventStore.m` in X-Code and change:

```objective-c
#import <FMDB.h>
```

to:

```objective-c
#import "FMDB.h"
```

#### 3.2.2

#### Reminder

That's it! Remember when 

[ios]: https://developer.apple.com/technologies/ios/
[ios-tracker-github]: https://github.com/snowplow/snowplow-ios-tracker

[afnetworking]: https://github.com/AFNetworking/AFNetworking
[fmdb]: https://github.com/ccgus/fmdb

[git]: http://git-scm.com/downloads
[fmdb]: https://github.com/ccgus/fmdb
