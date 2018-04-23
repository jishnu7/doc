# Project Manifest Options

Each game has its configuration stored in the
`manifest.json` file, located in the root directory of the
project. This file is auto-generated when a project is
initialized and controls the build process for your
game. These options are stored in [JSON](http://www.json.org)
format and can be edited by hand. For example, a minimal
`manifest.json` configuration might look something like this:

~~~
{
  "appID": "abcdefghijklmnopqrstuvwxyz012345",
  "shortName": "helloWorld",
  "title": "Hello, World!",
  "supportedOrientations": ["portrait"],
  "studio": {
    "name": "Jolly Fun Times, Inc.",
	"domain": "example.com"
  }
}
~~~

Manifest options are per game, they affect not only how the
game appears in the devkit development environment, but also
where to find image and font resources, store third-party service
keys, and configures the environment to build for native devices.

From inside the JavaScript code for your game, these options are
accessible at runtime via the CONFIG global variable.  For example: `CONFIG.title` and `CONFIG.appID`.


## Supported Configuration Settings

**Note:** If you're getting parse errors from your
`manifest.json` file, make sure to run it through a tool
such as [JSONLint](http://jsonlint.com), the JSON file
format can be a bit fickle at times.


### appID
1. `{string}` ---Auto-generated ID.

This is a unique identifier for your game used by devkit and
is auto-generated when a new project is created. The `appID`
is a string containing only alpha-numeric characters.

### shortName
1. `{string}` ---Defaults to the directory name of a new project.

The `shortName` is an internal code name used for links to
your project by the devkit tools and to name output files
during the build process. This is a alpha-numeric
string---which *does not* start with a number---and should
be unique to your game studio.  It may contain dots (.) or dashes (-).  It may *not* contain spaces ( ) nor underscores (_).

### title
1. `{string}`

The full title of the game which is displayed on menus in
the simulator and under the application icon when installed
on a phone.

### description
1. `{string}`

A brief description of your game.

### studio
1. `{object}`
    * `name {string}` ---The studio name.
    * `domain {string}` ---The URL of the studio.

Information about the studio publishing the game.

### version
1. `{string}`

The version number or string for your game, for example:
`'v0.1-beta'`. This is provided as a convenience for game
developers, so use whatever convention you wish.

### sdkVersion
1. `{string}`

The version of the Game Closure DevKit used to build this
project. This value is automatically filled in when the game
is run by devkit.

### group
1. `{string}`

If a group name is present, the project will be displayed in
a separate tab in the devkit Web Interface. All games that
contain the same group name will appear in a list together.

### doc
1. `{string}` ---URL.

This is a URL link pointing to documentation for the
project. This is used for the examples to display the
annotated source code and displayed in the devkit project list.

### supportedOrientations
1. `{array} = ["portrait"]` ---Options are `"portrait"`, `"landscape"`, or both.

Specify which orientations are allowed on the mobile
device. The simulator will default to the first entry in the
array, for example:

~~~
"supportedOrientations": ["landscape", "portrait"],
~~~

### icon
1. `{string}` ---Path to app store game icon, 512x512px.

This is the icon that will be displayed in the Game Closure DevKit web interface for your game.

~~~
"icon": "resources/icons/icon512.png",
~~~

### splash

1. `{object}`
	* `autoHide {boolean} = false` ---Automatically hide the splash image when the app starts.
	* `universal {string}` ---Path to a high resoution universal splash image that will automatically be used to fill in any of the missing sizes for the splash keys below.
	* `portrait480 {string} (optional)` ---Path to splash image, 320x480px, topside-left.
	* `portrait960 {string} (optional)` ---Path to splash image, 640x960px, topside-left.
	* `portrait1024 {string} (optional)` ---Path to splash image, 768x1024px, topside-up.
	* `portrait1136 {string} (optional)` ---Path to splash image, 640x1136px, topside-left.
	* `portrait2048 {string} (optional)` ---Path to splash image, 1536x2048px, topside-up.
	* `landscape768 {string} (optional)` ---Path to splash image, 1024x768px, topside-up.
	* `landscape1536 {string} (optional)` ---Path to splash image, 2048x1536px, topside-up.
	* `music {string}` ---Path to music to play on startup.

For easy usage, set `autoHide` set to true and provide a `universal` image.
For a portrait game you should specify a portrait image for universal,
oriented topside-up.  For a landscape game you should specify a landscape
image, oriented topside-up.  The DevKit will automatically crop and scale
the image to fit other screen sizes.  For this `universal` image, the long
side of the image should be around 2048 pixels for good presentation on
larger device screens.

The splash section is used to specify splash screen images
for Android and iPhone/iPad devices. All splash screens
should be PNG image files at 8-bit 3-channel RGB color.

Setting the `autoHide` property to `true` causes the
splash screen to be automatically removed after loading
completes. If `autoHide` is `false`---the default---the
developer can manually remove the splash screen by calling `GC.hidePreloader()`.

Example simple `manifest.json` settings:

~~~
"splash": {
  "autoHide": true,
  "universal": "resources/splash/portrait2048.png",
}
~~~

Example advanced `manifest.json` settings:

~~~
"splash": {
  "autoHide": true,
  "portrait480": "resources/splash/portrait480.png",
  "portrait960": "resources/splash/portrait960.png",
  "portrait1024": "resources/splash/portrait1024.png",
  "portrait1136": "resources/splash/portrait1136.png",
  "portrait2048": "resources/splash/portrait2048.png",
  "landscape768": "resources/splash/landscape768.png",
  "landscape1536": "resources/splash/landscape1536.png"
}
~~~

### scaleDPR
1. `{boolean} = false`

Mobile browser flag for setting device DPI or low DPI.

### unlockViewport
1. `{boolean} = false`

If this is set to `true`, scrolling in a mobile web browser will display the address bar.

### nativeURLScheme
1. `{string} = "tealeaf"`

Defines the URL protocol for cross-app communication on
native devices which allows an app to be opened from a url
in the browser.

### disableNativeViews
1. `{boolean} = false`

Use a scene graph system written in entirely in JavaScript
instead of the one implemented in native code. This can be
used for debugging purposes when testing a game on a native
device to pinpoint where in the stack an error is occurring.

### android
1. `{object}`
    * `versionCode {number}` ---Google Play version code.
    * `"icons" {object}` contains:
	    * `36 {string}` ---Path to small game app icon, size: 36x36px.
    	* `48 {string}` ---Path to medium game app icon, size: 48x48px.
	    * `72 {string}` ---Path to large game app icon, size: 72x72px.
    	* `96 {string}` ---Path to extra-large game app icon, size: 96x96px.
    	* `144 {string}` ---Path to Nexus 10 game app icon, size: 144x144px.

Device specific settings for Android phones and tablets.  The Google Play store version code can be set here, and app icon paths are set in this section.

Icons are displayed in the simulator and are used for
your game on a mobile device. All icons are PNG image
files, using 8-bit 3/4 channel RGBA. Android icons should have any
glossy effects prerendered in the image file, if desired.

In the `manifest.json` file, include the icon images like this:

~~~
"android": {
  "versionCode": 1,
  "icons": {
    "36": "resources/icons/android36.png",
    "48": "resources/icons/android48.png",
    "72": "resources/icons/android72.png",
    "96": "resources/icons/android96.png",
    "144": "resources/icons/android144.png"
  }
}
~~~

### ios
1. `{object}`
    * `bundleID {string}` ---The game bundle ID from iTunes Connect.
    * `appleID {string}` ---The game Apple ID from iTunes Connect.
    * `version {string}` ---The game version from iTunes Connect.
    * `"icons" {object}` contains:
    	* `renderGloss {boolean}` ---Specify `true` to have Xcode render gloss over your icon images.
	    * `57 {string}` ---Path to iPhone/iPod Touch game app icon, size: 57x57px.
	    * `72 {string}` ---Path to iPad game app icon, size: 72x72px.
	    * `76 {string}` ---Path to iOS 7 iPad non-retina game app icon, size: 76x76px.
	    * `114 {string}` ---Path to retina iPhone/iPod Touch game app icon, size: 114x114px.
	    * `120 {string}` ---Path to iOS 7 retina iPhone game app icon, size: 120x120px.
	    * `144 {string}` ---Path to retina iPad game app icon, size: 144x144px.
	    * `152 {string}` ---Path to iOS 7 retina iPad game app icon, size: 152x152px.

Device specific settings for iOS phones and tablets.  In this section you can copy settings from iTunes Connect to have your game hooked up properly for in-app purchases.  You should also specify icon images in this section.

Icons are displayed in the DevKit web emulator and are used to represent your game in the app list when installed on a mobile device.  All icons are PNG image files, using 8-bit 3/4 channel RGBA.

These icons can be rendered flat since as the "glossy" effect can added during the build process.  Specify `"renderGloss": true` to enable this feature.

In the `manifest.json` file, include the icon images like this:

~~~
"ios": {
  "bundleID": "ggshooter",
  "appleID": "12345678",
  "version": "1.0.0",
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

Note that your app will be rejected from the iOS App Store if any of these images are missing.

### TrueType Fonts (TTF)

Provide a list of TrueType fonts that are used by your game.

~~~
	"ttf": [
		"resources/fonts/Arial Black.ttf"
	],
~~~

For Android targets, it is crucial that custom .TTF file names match a name inside the font file.

For iOS targets, it is crucial that custom fonts do not have the same name as a default system font.  A complete list of default iOS fonts is available at [iosfonts.com](http://iosfonts.com).

### mpMetricsKey
1. `{string}`

The MixPanel metrics key.

### fonts
1. `{array}`

Internal data saved for custom bitmap fonts. Do not edit this.
