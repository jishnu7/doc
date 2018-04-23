# iOS Remote JavaScript Debugging

## Overview

On a mobile iOS device it is possible to watch console logs, inspect JavaScript objects, set breakpoints in running JavaScript code, and break on exceptions with the Game Closure DevKit.  This feature is called Remote JavaScript Debugging and is only available in the Game Closure iOS Test App.

When your JavaScript code throws an exception running on the native device but not the browser, normally you would be out of luck.  A typical way to debug these issues would be to laborously sprinkle log statements throughout your code to hopefully gain enough visibility into what it is doing.

A more direct approach would be to break on the exception and inspect the JavaScript objects leading to the exception, or set a breakpoint on code leading up to the problem and watch it develop.  Remote JavaScript Debugging hooks you directly into the code execution and allows you to fully and transparently analyze the problem.

## Setup

Run `devkit serve` and launch a Chrome web browser.  Navigate to the Game Closure DevKit webapp at `http://localhost:9200` in the web browser.  Select "Remote Debug" from the left-hand navigation pane.  After a moment you will see a web inspector app in the right-hand pane.

Remote JavaScript debugging is only available when using the [Test App](./ios-test-app.html).  To install the test app, launch its Xcode project ([see test app documentation](./ios-test-app.html)), attach a mobile device, and run the Xcode project on the device.

## Console Usage

When the Test App on the mobile device loads a game, the remote debugger will connect to the mobile device.  The web inspector pane will display the console output from your game.

At the console you are able to run JavaScript commands that are evaluated in the JavaScript context of the mobile device.  Furthermore, while execution is paused you may execute JavaScript in the scope of the current function.  The console will display log messages you would normally see at the Xcode console.  It will also show when scripts are loaded and other useful information for analyzing the execution of your game.

## Sources Usage

Click over to the Sources tab to view game source code and interact with the web inspector as you would with a familiar web inspector session in the browser.  The most useful debugging features will work remotely.

<div class="figure-wrapper">
<figure>
<img src="./assets/ios/ios-debugger.png" />
<figcaption>Remote iOS Debugger.</figcaption>
</figure>
</div>

There are two tabs for scripts on the left.  The left tab contains your game source code, and the right tab entitled "Content Scripts" contains the DevKit JavaScript source code.

To break on exceptions, start the debugging session before the exception occurs, and click on the (") pause button in the lower left until it displays blue.  In this state, when an exception occurs in JavaScript, the remote debugger will break on the exception line.

While at a breakpoint you can mouse-over or enter a variable name in the console to inspect its value.

### Sources Tools: Lower-Left Toolbar

<div class="figure-wrapper">
<figure>
<img src="./assets/ios/ios-debugger-left-tools.png" />
<figcaption>Debug lower-left toolbar.</figcaption>
</figure>
</div>

From left to right:

+ Console pop-up
+ Break on Exceptions: Black = Off, Blue or Purple = On
+ Prettify: Unmangle JavaScript

### Sources Tools: Upper-Right Toolbar

<div class="figure-wrapper">
<figure>
<img src="./assets/ios/ios-debugger-right-tools.png" />
<figcaption>Debug upper-right toolbar.</figcaption>
</figure>
</div>

From left to right:

+ Pause or resume JavaScript interpreter where it is currently executing
+ While execution is paused: Step over currently selected line
+ While execution is paused: Step into currently selected function
+ While execution is paused: Step out of current function scope
+ Disable or enable all breakpoints

## Troubleshooting Issues

If the debugger becomes unresponsive, attempt to refresh the browser window.  If that doesn't work, force-quit your game and restart it also.
