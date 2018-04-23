# iOS Test App

Test your game code live on a device with the Game Closure Test App.  You will host a server on your computer, and set up your mobile device to use WiFi.  Build and run the Test App on your device using Xcode.

Select your server from the on-screen list, then select which game to test.  Making code changes on your computer can be pushed quickly to the device by restarting the game with a circular gesture (See Restarting below).

## Setup

Be sure to read the [iOS Setup Guide](./ios-setup.html) and follow the instruction there to prepare for iOS development with the Game Closure DevKit.

Turn on WiFi and connect your mobile device to the same LAN as your computer.

## Installing the Test App

To run the iOS Test App on your mobile device, connect it to your computer via a USB data cable, and enter the following command at a console:

~~~
$ devkit testapp native-ios
~~~

The iOS Test App Xcode project will open after a moment.  Select a device to install it to at the top, and hit the Play button to build and install the Test App to the device.

After installation you will be able to launch the Test App from its app icon on the home screen of your iOS device:

<div class="figure-wrapper">
<figure>
<img src="./assets/ios/ios-test-app-icon.png"></img>
<figcaption>iOS Test App Icon on Device.</figcaption>
</figure>
</div>

This will also run the devkit Server, which is needed for the Test App to run properly.

## Running the devkit Server

An instance of the DevKit devkit server must be running to serve games for the test app.  To start a new devkit server without building the Test App enter this command at a console:

~~~
$ devkit serve
~~~

This will start hosting the Test App Server on your LAN.

### Server List

When the Test App starts on the device, it will display a list of servers on the LAN.  At first this will just contain the one 

<div class="figure-wrapper">
<figure>
<img src="./assets/ios/ios-test-app-server-list.png"></img>
<figcaption>iOS Test App Server List.</figcaption>
</figure>
</div>

### Game List

After selecting a server, the game list for that server will show up.

<div class="figure-wrapper">
<figure>
<img src="./assets/ios/ios-test-app-game-list.png"></img>
<figcaption>iOS Test App Game List.</figcaption>
</figure>
</div>

Tap one of the games to start loading it.

### Loading Screen

The loading screen will display progress of downloading game resources.  Resources are only loaded if they change, so this process should be fairly fast initially, and almost instant after the first time.

<div class="figure-wrapper">
<figure>
<img src="./assets/ios/ios-test-app-loading.png"></img>
<figcaption>iOS Test App Loading Screen.</figcaption>
</figure>
</div>

After loading completes, your game should start.

## Remote JavaScript Debugging

The [Chrome Web Inspector interface](./ios-remote-debug.html) may be used to investigate coding errors on the mobile device from the comfort of your computer.  Using the Remote Debug feature you can investigate coding errors and view the console logs of Test App sessions.

### Restarting

To restart the game, make a rotation gesture on the screen:  Swipe one finger up on the left while swiping another finger down on the right.  This brings up a quit prompt to return to the game list:

<div class="figure-wrapper">
<figure>
<img src="./assets/ios/ios-test-app-quit.png"></img>
<figcaption>iOS Test App Restart Menu.</figcaption>
</figure>
</div>

Once back at the games list you can select the same or another game to run.
