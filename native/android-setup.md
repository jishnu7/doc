# Android Setup Guide

## Overview

Testing your game on your cellphone for the first time is an exhilerating affair full of nail-biting anticipation. The process of getting there for Android devices has been stream-lined for you in the Game Closure DevKit.

This guide is mainly for Linux or OSX.  To install on Windows please follow the [Windows install guide](../guide/windows-guide.html) first.

## Prerequisites

You will need some tools for Android development:

+ Mac OS X 10.6 (or newer) computer
+ USB sync cable for your cellphone
+ Android SDK 15
+ Android NDK r9
+ [Command Line Tools for Xcode](https://developer.apple.com/downloads) or [Xcode](https://itunes.apple.com/us/app/xcode/id497799835)
+ Game Closure DevKit (devkit)

Note that we require Android NDK r9. Beware r8e, which has a serious bug. For more information, see [Ingo Muschenetz's explanation and patch](http://developer.appcelerator.com/blog/2013/03/correcting-a-bug-in-the-latest-google-ndk-r8e.html).

For a guide on installing the Game Closure DevKit [please see this documentation](../guide/install.html).

### Supported Mobile Devices

Nearly all Android cellphones and tablets are supported, including all devices powered by processors in the ARM9 family on the ARMv5TE(J) architecture.  This means all TI OMAP processors.  For example the first Motorola Droid phone was powered by a TI OMAP 3430 built for the ARMv7 instruction set.

Android version 2.2 (API level 8) is the minimum required version.  The original Motorola Droid received an over-the-air update for this in late 2010.  According to the [Android developer site](http://developer.android.com/about/dashboards/) only about 2.6% of cellphones that access Google Play are unsupported at this time by the Game Closure DevKit.  The first Android phone (the T-Mobile G1 / HTC Dream) is unsupported, as an example.

## Install the Command Line Tools for Xcode

The command line tools are just as good as Xcode for the GCDK and take up far less disk space, so get those unless you already have Xcode installed:

[Command Line Tools for Xcode](https://developer.apple.com/downloads)

Yes, you do need an Apple ID to download the packages, sorry about that. If you install Xcode from the App Store, the Command Line Tools are now an optional installation. You’ll need to enable them under Preferences > Downloads.

## Install the Android SDK

Download and install the
[Android SDK](http://developer.android.com/sdk/) to your
local machine. If you are using the
[Homebrew](http://mxcl.github.com/homebrew/) package
manager, run:

~~~
$ brew update
$ brew install android
~~~

Running `android` at the command-line brings up a GUI
front-end that will allow you to install various android 
API targets. Install the SDK Platform under Android 4.0.3 
(API 15), Android Support Library in the Extras sections 
at the bottom of the list, and the Android SDK Platform-tools 
in the tools section at the top.

<div class="figure-wrapper">
<figure>
<img src="./assets/android/packages.png" />
<figcaption>All the checked items are required for android installation.</figcaption>
</figure>
</div>

You will then be prompted with an Accept window. Check `Accept All`
and wait for the download and installation to finish.

<div class="figure-wrapper">
<figure>
<img src="./assets/android/accept-all.png" />
<figcaption>Accept installation of the required packages.</figcaption>
</figure>
</div>

## Install the Android NDK

The Android NDK is also required for Android development.  An easy way to install the Android NDK is to use homebrew:

~~~
$ brew update
$ brew install android-ndk
~~~

The homebrew install script will add the required NDK tools to your path. 

## Install Android Plugin for devkit

devkit is the command-line tool you will use from the Game Closure DevKit to perform native builds.  Make sure your version of devkit is up to date with `devkit update`.

The first step to doing Android development with devkit is to install the Android plugin.  At a command-line enter the command:

~~~
$ devkit install native-android
~~~

This downloads and installs the Android plugin for devkit.

## Generating a Keystore

To digitally sign your game's binary file for release, you will need to generate a keystore file.  The purpose and process are described on the [Android Developer site](http://developer.android.com/tools/publishing/app-signing.html).  Note that the keystore is only required for release builds. You can skip this step for now if you will only be debugging your game.

A quick-start example:

~~~
 $ keytool -genkey -v -keystore beards.keystore -alias "bearded bobs" -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password: fuzzyfiggin
Re-enter new password: fuzzyfiggin
What is your first and last name?
  [Unknown]:  Bob Baxter
What is the name of your organizational unit?
  [Unknown]:  SDK
What is the name of your organization?
  [Unknown]:  Game Closure
What is the name of your City or Locality?
  [Unknown]:  Mountain View
What is the name of your State or Province?
  [Unknown]:  CA
What is the two-letter country code for this unit?
  [Unknown]:  US
Is CN=Bob Baxter, OU=SDK, O=Game Closure, L=Mountain View, ST=CA, C=US correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
	for: CN=Bob Baxter, OU=SDK, O=Game Closure, L=Mountain View, ST=CA, C=US
Enter key password for <bearded bobs>
	(RETURN if same as keystore password):
[Storing beards.keystore]
~~~

You will want to copy your keystore to a convenient location and then reference it in the `config.json` file in the root of your Game Closure DevKit folder.

An example "android" section in DevKit's `config.json` file:

~~~
	"android": {
		"root": "/Users/bbaxter/cleanroom/android",
		"key": "bearded bobs",
		"keystore": "/Users/bbaxter/cleanroom/devkit/beards.keystore",
		"storepass": "fuzzyfiggin",
		"keypass": "fuzzyfiggin"
	}
~~~

Note this is not in the `manifest.json` file in your game folder but in the `config.json` file in your devkit folder.

The `storepass` is the keystore password you typed initially.  And `keypass` is the key password, which will be the same as the keystore password if you opted not to enter one.

## Setting Up Your Game Manifest

Every game includes and must have a manifest.json file with configuration information and more for each game. Several fields are required for a properly configured game for Android.

- The splash screen and icons for your game should be added before building for Android devices.
- If you are using any custom TrueType fonts be sure to include those in the manifest file.
- For a complete reference see [documentation on the manifest.json](../guide/manifest.html) file.

In addition to the normal sections in the manifest file you may have already filled in, Android requires the following sections:

#####Google Play

Add the app version code from Google Play here:

~~~
	"android": {
		"versionCode": 1
	},
~~~

#####Icons

The icon sizes used for Android are listed below.  Be sure to include at least this set of icons.  [See the manifest documentation](../guide/manifest.html) for file formats and other details.

~~~
"android": {
  "icons": {
    "36": "resources/icons/android36.png",
    "48": "resources/icons/android48.png",
    "72": "resources/icons/android72.png",
    "96": "resources/icons/android96.png"
  }
}
~~~
  
#####Orientation

Choose an orientation for your game, which is a combination of portrait and/or landscape.  Landscape means that the long side of the device is horizontal, and portrait means that the long side of the device will be vertical.  Unspecified orientations will be disallowed.

On tablets, both portrait and landscape orientations will allow upside-down or rightside-up rotations.  On a phone, for portrait orientation only rightside-up will be allowed.

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

On Android, it is crucial that custom .TTF file names match an internal font name.

#####Splash Screen

Define splash screen images for your game.  For the complete list of image sizes required and other details [see the manifest documentation](../guide/manifest.html).

If you are also releasing on iOS then all the sizes are mandatory.  However, splash screens are somewhat optional for Android.  Leaving them out completely will cause the game to display black until it is loaded.  When your game starts running, it will select the best splash image among the ones provided to match the device's screen size, and will rotate it to match the orientation.

Providing fewer splash images will reduce the download time for your game, but the trade-off will be a splash image that looks worse on some devices.  The Landscape splash images can be left off entirely for Android without any ill effects and is recommended.

Once your game is configured properly for Android, you're ready to install it!

## Configuring a New Android Device

If you are not able to find the "Developer options" menu, you will need to perform a "magic knock" to unlock it on Android 4.2 or newer.  Following this simple procedure:

1.  Open the Settings app.
2.  Select "About phone" at the bottom of the list.
3.  Scroll to the bottom to see the "Build number" item.
4.  Tap on the Build number five times.

To begin building for an Android device you need to enable USB Debugging on the test device.  The process to enable debugging is:

1.  Open the Settings app.
2.  Select "Applications" at the middle of the list.
3.  Select "Development settings" aka "Developer options".
4.  Tap the check box next to USB debugging and tap OK.

It is recommended to set the USB Charge mode to "Charge Only" while connected so that the phone will not unmount its internal storage while connected via USB.  To do this:

1.  Connect the phone to your computer via the USB cable.
2.  Pull open the Android notification toaster (hold the top of the screen and swipe down).
3.  Select USB Connection.
4.  Select "Charge Only" and tap OK.

## Troubleshooting Issues

If you run into problems, try our [Android troubleshooting guide](./android-troubleshooting.html).

### Appendix: Manual Install of the Android Plugin for devkit

**This process should only be done as a last resort.** Make sure your version of devkit is up to date with `devkit update`.

Clone the Game Closure
[Android GitHub repository](https://github.com/hashcube/native-android). Switch to this directory and make sure everything is up-to-date:

~~~
$ git clone https://github.com/hashcube/native-android
$ cd native-android
$ git checkout develop
$ git submodule update --init
~~~

To let devkit know where to find the android repository,
update the **config.json** file located in the root of the
devkit install:

~~~
{
  "android": {
    "root": "path/to/native-android"
  }
}
~~~
