# Stages of iOS Development

## Overview

The iPhone and iPad mobile platform is immensely popular, and you can use the Game Closure DevKit to target your HTML5 games to run smoothly on these mobile devices.  This is a high-level view of the progression for iOS game development using the Game Closure DevKit.

For iOS development you will need to get an Apple Developer account, which Apple  $99/year.  This gives you access to Apple's Developer web portal, which allows you to manage provisioning profiles.  For more detail, follow the links below.

* Based on what stage of development your game is in, you may consider different types of builds:

<div class="figure-wrapper">
<figure>
<img src="./assets/ios/stages.png"></img>
<figcaption>iOS Stages of Development.</figcaption>
</figure>
</div>

### 1. Web Development

Test your code, art, and sound assets entirely in a web browser simulating a mobile device.  Quickly iterate using the Game Closure DevKit web interface without doing any setup required for building on the mobile device.

Investigate coding errors, step through running code, pause execution, and inspect code objects using the Chrome built-in Web Inspector as with normal HTML5 game development.  Improve performance by finding code hot-spots with the CPU Profiler to target optimization.  Hunt down memory leaks using the Heap Profiler to identify object pile-ups.

<div class="figure-wrapper">
<figure>
<img src="./assets/web-dev.png"></img>
<figcaption>DevKit Device Emulator.</figcaption>
</figure>
</div>

For more information see the [basic DevKit guides](../guide/install.html).

### 2. Test App Development

Test your game code live on the device with the [Game Closure Test App](./ios-test-app.html).  The test app will allow you to quickly test your game on a device without having to recompile and install a new application every time you make a change.  You will host a server on your computer, and setup your mobile device to use WiFi.  Build and install the Test App on a USB-attached iOS device with `devkit testapp native-ios`.

The same Chrome [Web Inspector interface](./ios-remote-debug.html) may be used to investigate coding errors on the mobile device from the comfort of your computer.

### 3. Building

Use the command-line tools from the Game Closure DevKit to [build your game code](./ios-build.html) into an Xcode project.  The full list of command-line arguments can be brought up by running `devkit build native-ios --help`.

<div class="figure-wrapper">
<figure>
<img src="./assets/ios/ios-build-console.png"></img>
<figcaption>iOS Build in Terminal.</figcaption>
</figure>
</div>

Build your game to a device connected via an Apple USB cable.  Note that the iPhone 4 and 5 use different data cables.  Using the SDK build tools, you will create an Xcode project containing your code, art, splash screens, icons, and sound assets.  Console logs will be visible in Xcode while it is running.

<div class="figure-wrapper">
<figure>
<img src="./assets/ios/ios-build-xcode.png"></img>
<figcaption>iOS Build in Xcode.</figcaption>
</figure>
</div>

At this stage you may test your in-app purchases using an Apple test account created through the Apple Developer website.

At this stage the mobile device no longer needs to be tethered to your computer and you can show off your game!

### 4. Group Testing

Build your game to an IPA file that can be distributed using [TestFlight](http://testflightapp.com) if you so choose.  Testers signed up with TestFlight can use your app all over the world on their mobile Apple devices.

For a guide on how to use TestFlight with the Game Closure DevKit, click [here](./ios-testflight.html).

### 5. Deployment

The same IPA file use for group testing will be uploaded to the iTunes Connect website for release.  Please see our [iOS build guide](./ios-build.html) on how to create an .IPA file for release (or for testing).
