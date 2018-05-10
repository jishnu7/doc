# Building for iOS

## Overview

Running your game on an iPhone or iPad for the first time is that "I made it!" moment where it finally feels real.  This guide will help you get from developing in a browser to showing your new game.

Building your game to run on a mobile device is done using the Game Closure DevKit `devkit` command-line tool.  See the [iOS Setup Guide](./ios-setup.html) for steps to get prerequisites, supported mobile devices, and steps on getting started.

## Building for Xcode

To build your game enter:

~~~
$ devkit debug native-ios
~~~

An Xcode project window will pop up.  If you installed the simulators during setup, then you can use the drop-down list at the top to select an iPad or iPhone simulator and which iOS version to simulate.

It is recommended to do as much of your testing as possible with the simulator before using the physical device.  The simulator does not require an Apple Developer account, and it does not require additional configuration.

For a more thorough release-mode build enter:

~~~
$ devkit release native-ios
~~~

## Watching JavaScript Logs

JavaScript logs are generated when you use `console.log` from within your code, and the Game Closure DevKit will also write out helpful messages.

The logs will scroll in the Debug area at the bottom of the Xcode project window in the right-side tab.

## Installing to a Connected Device

Select your mobile device from the list at the top and hit the Play button to install.

If your mobile device is not showing up, then insure that you have added the device to your portal and that you have a valid mobile provisioning profile installed.  You may also try hitting the Refresh button in the lower-right in the Organizer Library Provisioning Profiles view under Devices.

You will be able to see JavaScript logs scrolling in Xcode while running the game on the connected device.

## Building for TestFlight or iTunes Connect

Once you are satisfied with how your game looks, and if you'd like to distribute test builds of your game, [TestFlight](http://testflightapp.com/) is a great tool that we recommend for distributing your game to testers.

When testing is complete you can use the same built image of your game to upload to the [iTunes Connect](http://itunesconnect.apple.com) website to apply for entry into the iTunes Store.

### Generate an App ID

To build for TestFlight or iTunes, browse to the [Apple Developer website](http://developer.apple.com).  Log in with your Apple Developer account and navigate to the iOS Provisioning Portal.

Select the App IDs tab on the left.  Create a [New App ID] with the button on the right.  This is where you choose a bundle identifier.  This can be a simple string like "beards.on.boards" that should be globally unique.

Once your App ID appears in the App IDs list, you can use the Configure action on your App ID to enable In-App Purchases.

### Generate a Mobile Provisioning Profile

Select the Provisioning tab on the left.  Create a [New Profile], selecting team members who can make builds for your App ID.  Select the App ID you created.  And select the devices that can install the App.

After your App ID provisioning profile is added to the list of Development Provisioning Profiles, use the [Download] action to download the .mobileprovision file and take note of where it exists on your computer.

### Build an .IPA

Run the build command with the `--ipa` flag to create an .IPA file for your game:

~~~
$ devkit build native-ios --ipa --provision /Users/bboy/Desktop/BeardsOnBoards.mobileprovision --name="Bob Baxter"
~~~

## Troubleshooting Issues

If you run into problems, try our [iOS troubleshooting guide](./ios-troubleshooting.html).

### Appendix: Build Options

General build options:

+ `--help` / `-h` : Display this help menu
+ `--debug` / `-d` : Create a debug build (Default)
+ `--release` / `-r` : Create a release build
+ `--clean` / `-c` : Clean build before compilation (Default)
+ `--no-clean` : Do not clean before compile
+ `--open` / `-o` : Open the Xcode project after building (Default)
+ `--no-open` : Do not open the Xcode project after building
+ `--compress` : Compress JavaScript during build (Default)
+ `--no-compress` : Do not compress JavaScript during build

IPA generation options:

+ `--ipa` / `-i` : Generate appName.ipa file as output
+ `--provision` / `-p` : Path to the .mobileprovision profile file
+ `--name` / `-n` : Name of iPhone Developer key on Keychain Access list

When --ipa is specified, Xcode will not pop up after the build.

### Advanced Usage: Examples

**For a fast debug build:**

~~~
$ devkit debug native-ios
~~~

This specifies a debug build, cleans before building, does not compress JavaScript, and will open Xcode after build completes.

**For a thorough release build:**

~~~
$ devkit release native-ios
~~~

This specifies a release build, cleans before building, compresses JavaScript, and will open Xcode after build completes.

**For an IPA release build:**

~~~
$ devkit release native-ios --ipa --provision "/Users/bboy/Desktop/BeardsOnBoards.mobileprovision" --name="Bob Baxter"
~~~

This specifies a release build, cleans before building, compresses JavaScript, signs an IPA file with the given mobile provision for Bob Baxter, and does not open Xcode.
