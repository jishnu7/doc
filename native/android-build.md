# Building for Android

## Overview

Testing your game on your cellphone for the first time is an exhilerating affair full of nail-biting anticipation. The process of getting there for Android devices has been stream-lined for you in the Game Closure DevKit.

Building your game to run on a mobile device is done using the Game Closure DevKit `devkit` command-line tool.  See the [Android Setup Guide](./android-setup.html) for steps to get prerequisites, supported mobile devices, and steps on getting started.

## Building an .APK

To build your game for debugging, open a console and change directory to your game's top-level folder.

~~~
$ cd whack-that-mole
$ ls
README.md     manifest.json resources     src
$ devkit build native-android --no-compress --debug --clean
[build]   Building: debug/native-android
...
~~~

By specifying the --no-compress option, devkit will not try to compress JavaScript files.  This speeds up the build process and is recommended for most pre-release builds.

Combining the --no-compress and --debug options will also allow you to do [remote JavaScript debugging](./android-remote-debug.html) from your computer.

**Note:** For convenience the debug builds can also be done with command

~~~
$ devkit debug native-android
~~~

If you want to build your game for the Google Play store use the command

~~~
$ devkit release native-android
~~~

When the build completes, you will see a line such as:

~~~
[android]   saved to /Users/gc/sandbox/whack-that-mole/build/debug/native-android/whackthatmole.apk
~~~

This indicates where the output APK file has been saved.  You can then install it to a connected device.

## Installing to a Connected Device

Install an application to a device connected via a USB data cable using the Android Debug Bridge (ADB):

`$ adb install -r <path-to-apk>`

The -r flag means to attempt a reinstall.  Note that this will fail if the APK is signed differently (mixing debug and release versions) so you may need to uninstall your game from the phone to reinstall.

## Troubleshooting Issues

If you run into problems, try our [Android troubleshooting guide](./android-troubleshooting.html).

### Appendix: devkit Debug

`devkit debug native-android` is equivalent to running `devkit build native-android --debug --no-compress`

### Appendix: devkit Release

`devkit release native-android` is equivalent to running `devkit build native-android` with no arguments.

### Appendix: Build Options

General build options:

+ `--help` / `-h` : Display this help menu
+ `--debug` / `-d` : Create a debug build
+ `--release` / `-r` : Create a release build (Default)
+ `--clean` / `-c` : Clean build before compilation
+ `--no-clean` : Do not clean before compile
+ `--compress` : Compress JavaScript during build (Default)
+ `--no-compress` : Do not compress JavaScript during build
+ `--install` : Install after build
+ `--open` : Install and open after build
+ `--logging` : Print logs in release mode (debug mode always prints logs)

### Appendix: Example Usage

**For a fast debug build:**

~~~
$ devkit debug native-android
$ adb install -r build/debug/native-android/whackthatmole.apk
~~~

This specifies a debug build, cleans before building, does not compress JavaScript, and can be used with the remote JavaScript debugger.

**For a thorough release build:**

~~~
$ devkit release native-android
$ adb install -r build/release/native-android/whackthatmole.apk
~~~

This specifies a release build, cleans before building, compresses JavaScript, and will sign the resulting APK for release.
