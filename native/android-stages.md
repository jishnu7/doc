# Stages of Android Development

## Overview

Android cellphones are a friendly and flexible platform for game development, with a wide variety of mobile devices supporting it.  Using the Game Closure DevKit you may target your HTML5 games to run smoothly across all of the popular Android devices without worrying about specific hardware or OS differences between the devices.

Based on what stage of development your game is in, you may consider different types of builds:

<div class="figure-wrapper">
<figure>
<img src="./assets/android/android-stages.png"></img>
<figcaption>Android Stages of Development.</figcaption>
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

Test your game code live on the device with the [Game Closure Test App](./android-test-app.html).  You will host a server on your computer, and setup your mobile device to use WiFi.  Build and install the Test App on a USB-attached Android device with `devkit testapp native-android`.

Tap the installed "Test App" icon on your Android device.  Select your server from the on-screen list, then select which game to test.  Making code changes on your computer can be pushed quickly to the device by opening the on-screen menu and selecting the Restart option.

The same Chrome Web Inspector interface may be used to investigate coding errors on the mobile device from the comfort of your computer.  Simply attach your Android device via the USB cable and use the Remote Debug feature to trap exceptions and view the console logs of Test App sessions.

You may also use `adb logcat | grep JS` to view the JavaScript logs.

### 3. Building

[Build and install](./android-build.html) your game to a device connected via USB.  Simply run `devkit build native-android` to produce an APK, then install it with `adb install`.

The Chrome Web Inspector interface may also be used to inspect and debug JavaScript game code running on the Android device.  Simply attach your Android device via the USB cable and use the Remote Debug feature to trap exceptions and view the console logs of Test App sessions.

You may also use `adb logcat | grep JS` to view the JavaScript logs.

At this stage the mobile device no longer needs to be tethered to your computer and you can show off your game!

### 4. Deployment

The same APK that you built for testing may be uploaded to the Google Play Store.
