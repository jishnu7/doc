# Android Test App

Test your game code live on a device with the Game Closure Test App.  You will host a server on your computer, and set up your mobile device to use WiFi.  Build and run the Test App on your device.

Select your server from the on-screen list, then select which game to test.  Making code changes on your computer can be pushed quickly to the device by restarting the game with the on-screen debug menu (see below).

## Setup

Be sure to read the [Android Setup Guide](./android-setup.html) and follow the instruction there to prepare for Android development with the Game Closure DevKit.

Turn on WiFi and connect your mobile device to the same LAN as your computer.

## Running

To run the Android Test App on your mobile device, connect it to your computer via a USB data cable, and enter the following command at a console:

~~~
$ devkit testapp native-android
~~~

It will build and attempt to install the Test App on your device and start the devkit server.

After installation you will be able to launch the Test App from the applications list on your Android device:

<div class="figure-wrapper">
<figure>
<img src="./assets/android/android-test-app-icon.png"></img>
<figcaption>Android Test App Icon on Device.</figcaption>
</figure>
</div>

### Server List

When the Test App starts on the device, it will display a list of servers on the LAN.  At first this will just contain the one 

<div class="figure-wrapper">
<figure>
<img src="./assets/android/android-test-app-servers.png"></img>
<figcaption>Android Test App Server List.</figcaption>
</figure>
</div>

### Game List

After selecting a server, the game list for that server will show up.

<div class="figure-wrapper">
<figure>
<img src="./assets/android/android-test-app-games.png"></img>
<figcaption>Android Test App Game List.</figcaption>
</figure>
</div>

Tap one of the games to start loading it.

### Loading Screen

The loading screen will display progress of downloading game resources.  Resources are only loaded if they change, so this process should be fairly fast initially, and almost instant after the first time.

<div class="figure-wrapper">
<figure>
<img src="./assets/android/android-test-app-loading.png"></img>
<figcaption>Android Test App Loading Screen.</figcaption>
</figure>
</div>

After loading completes, your game should start.

## Remote JavaScript Debugging

The [Chrome Web Inspector interface](./android-remote-debug.html) may be used to investigate coding errors on the mobile device from the comfort of your computer.  Using the Remote Debug feature you can investigate coding errors and view the console logs of Test App sessions.

### Restarting

To restart the game, tap the properties/menu button on your Android device.  This brings up a quit prompt to return to the game list:

<div class="figure-wrapper">
<figure>
<img src="./assets/android/android-test-app-menu.png"></img>
<figcaption>Android Test App Menu.</figcaption>
</figure>
</div>

From this menu you can also attempt a few other debug actions that may be useful.  Once back at the games list you can select the same or another game to run.
