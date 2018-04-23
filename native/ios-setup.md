# iOS Setup Guide

## Overview

Running your game on an iPhone or iPad for the first time is that \"I made it!\" moment when it finally feels real.  This guide will help you get from developing in a browser to showing your new game off at parties.

Building your game to run on a mobile device is done using the Game Closure DevKit `devkit` command-line tool.  

## Prerequisites

You will need some tools for iOS development:

+ Mac OS X 10.6 (or newer) computer
+ iPhone/iPad sync cable
+ Xcode 4.4 (or newer) and Xcode command-line tools
+ Game Closure DevKit (devkit)

For a guide on installing the Game Closure DevKit [please see this documentation](../guide/install.html).

### Supported Mobile Devices

The iOS devices supported are:

_iPhone_: iPhone 3GS (2009), iPhone 4, iPhone 4S, iPhone 5.

_iPad_: All devices.

_iPod_: Touch 3rd Gen (2009), Touch 4th Gen, Touch 5th Gen.

Other devices **may** work but are not explicitly supported.

## How to install Xcode Prerequisites

In order to develop for iOS, Xcode is required.  Xcode can be downloaded from the [iTunes App store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).

Additionally, the Xcode command-line tools **are required**.  To download these, open Xcode and go to the *preferences* menu under the *Xcode* menu.  Next, go to the *Downloads* tab and install the command-line tools.  It is recommended to also install the simulator from this tab so that you may test your games on your computer rather than on a physical device.

## Apple Developer Account

For your initial use of the DevKit there is no reason to set up an Apple Developer account.  So you may want to skip this section and come back later.  Without an account you will not be able to install your game on a mobile device.  Note that you can run your game on the iOS Simulator, which is packaged with Xcode, without an Apple Developer account.

To do iOS development you will need an [Apple Developer account](https://developer.apple.com/programs/register/) which costs $99.  To build and install your app on your personal cellphone this is also necessary.  By getting an Apple Developer account you will have access to the Apple Developer website and tools to generate provisioning profiles.

To get started with iOS development, you will need to access the Member Center on the  [Apple Developer website](http://developer.apple.com).  Enter your Apple ID and Password.  Select your team.  Enter the iOS Provisioning Portal.  Select the Certificates tab.  Follow the instructions to request, download, and install your Development Certificate.

Start Xcode.  Open the Organizer.  Select "Provisioning Profiles" on the left tab.  In the lower-right corner hit Refresh.  This should download and install the new certificate on your computer.  At this point you should be able to install to devices that have been added to your developer portal (see below).

## Install iOS Plugin for devkit

devkit is the command-line tool you will use from the Game Closure DevKit to perform native builds.  Make sure your version of devkit is up to date with `devkit update`.

The first step to doing iOS development with devkit is to install the iOS plugin.  At a command-line enter the command:

~~~
$ devkit install native-ios
~~~

This downloads and installs the iOS plugin for devkit.  The download may take some time since it is a large plugin, so please be patient.

## Setting Up Your Game Manifest

Every game includes and must have a manifest.json file with configuration information and more for each game. Several fields are required for a properly configured game for iOS.  
  
- The splash screen and icons for your game should be added before building for iOS devices.  
- If you are using any custom TrueType fonts be sure to include those in the manifest file.  
- For a complete reference see [documentation on the manifest.json](../guide/manifest.html) file.

In addition to the normal sections in the manifest file you may have already filled in, iOS requires the following sections:

#####iTunes App Store

Add these fields to insure that In-App Purchases will work.

~~~
	"ios": {
		"bundleID": "com.billy.boards",
		"appleID": "1234567",
		"version": "1.0.0"
	},
~~~

#####Icons

The icons for iOS are listed below.  Be sure to include at least this set of icons.  [See the manifest documentation](../guide/manifest.html) for file formats and other details.

~~~
"ios": {
  "icons": {
    "renderGloss": false,
    "57": "resources/icons/ios57.png",
    "72": "resources/icons/ios72.png",
    "76": "resources/icons/ios76.png",
    "114": "resources/icons/ios114.png",
    "120": "resources/icons/ios120.png",
    "144": "resources/icons/ios144.png",
    "152": "resources/icons/ios152.png"
  }
}
~~~

References:

[1] Apple provides a [guide for icon images](http://developer.apple.com/library/ios/#documentation/userexperience/conceptual/mobilehig/IconsImages/IconsImages.html).

#####Orientation

Choose an orientation for your game, which is a combination of portrait and/or landscape.  Landscape means that the long side of the device is horizontal, and portrait means that the long side of the device will be vertical.  Unspecified orientations will be disallowed.

On the iPad, both portrait and landscape orientations will allow upside-down or rightside-up rotations.  On the iPhone, for portrait orientation only rightside-up will be allowed.

~~~
	"supportedOrientations": [
		"landscape"
	],
~~~

~~~
	"supportedOrientations": [
		"portrait"
	],
~~~

~~~
	"supportedOrientations": [
		"landscape", "portrait"
	],
~~~

#####TrueType Fonts

Add a list of TrueType font files to the manifest if you are using them:

~~~
	"ttf": [
		"resources/fonts/Arial Black.ttf"
	],
~~~

On iOS, it is crucial that custom fonts do not have the same name as a default system font.  A complete list of default iOS fonts is available at [iosfonts.com](http://iosfonts.com).

#####Splash Screen

Define splash screen images for your game.  For the complete list of image sizes required and other details [see the manifest documentation](../guide/manifest.html).

For iPhone, the portrait splash images are used, and when your game specifies landscape mode, those images will be rotated to match the screen orientation.

Once your game is configured properly for iOS, you're ready to install it!

## Setting Up a New Mobile Device

If you have set up an Apple Developer account then you will need to add new mobile devices to your portal before using them.

In Xcode, open the Organizer.  On the devices tab on the left, select the name of the new connected device.  At the bottom of the Organizer, press the (+) [Add To Portal] button.  This will add the device to your developer portal.

At this point you are ready to start building for iOS!  Check out the [building guide](./ios-build.html) for information on building your game, or the [test app guide](./ios-test-app.html) for instructions on how to use the test app for expedited development and testing.

## Troubleshooting Issues

If you run into problems, try our [iOS troubleshooting guide](./ios-troubleshooting.html).

### Appendix: Manual Install of iOS Plugin for devkit

Clone the Game Closure
[iOS GitHub repository](https://github.com/hashcube/native-ios). Switch to this directory and make sure everything is up-to-date:

~~~
$ git clone https://github.com/hashcube/native-ios
$ cd native-ios
$ git checkout develop
$ git submodule update --init
~~~

Edit the devkit configuration file **config.json** located in the root of the devkit install to point to the install location of the iOS plugin:

~~~
{
  "ios": {
    "root": "path/to/native-ios"
  }
}
~~~
