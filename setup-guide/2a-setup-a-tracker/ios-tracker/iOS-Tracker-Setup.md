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

[ios]: https://developer.apple.com/technologies/ios/
[ios-tracker-github]: https://github.com/snowplow/snowplow-ios-tracker

[afnetworking]: https://github.com/AFNetworking/AFNetworking
[fmdb]: https://github.com/ccgus/fmdb