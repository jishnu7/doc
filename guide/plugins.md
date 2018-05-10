# DevKit Native Plugins

The Game Closure DevKit supports a plugin system that enables you to develop unique features for your game in native code and JavaScript that will seamlessly interoperate with and persist through upgrades to new versions.

+ These plugins can be easily shared privately with your internal development team or shared back to Game Closure to benefit the rest of the HTML5 game development community.

+ Plugins allow your game to use a minimum set of permissions, which will prevent users from getting scared off.

+ Native plugin code has access to the `manifest.json` file contents so you can configure the plugins through that file.

+ Native code and JavaScript interact through a message passing interface.

The JS Event System is used to send events to JavaScript, and a function is used to deliver events from JavaScript to native code.  This native interface is the same for both iOS and Android.

## Plugin Example: GeoLocation

To demonstrate the plugin system, take the [GeoLocation Plugin](https://github.com/hashcube/geoloc) as an example.  It will be referenced throughout the documentation.

To install the Geolocation addon, run `devkit install geoloc`.  It will be installed under `devkit/addons/geoloc`.  You can also manually install addons under the `addons` directory.

If you would like to add your plugin to the list of plugins that can be automatically installed with `devkit install` please open up a discussion with us.

## Using plugins from your game

The user of the plugin must include it in the `manifest.json` file under the "addons" section:

~~~
	"addons": [
		"geoloc"
	],
~~~

Furthermore at the top of the game's `src/Application.js`:

~~~
import plugins.geoloc.install;
~~~

This imports the `install.js` file under `devkit/addons/geoloc/js/`.  This file checks if it is running on native or in the browser, and will add a JS wrapper to the `navigator` object for native.

What to import may vary from plugin to plugin, so please refer to the documentation that is provided with the plugin for usage instructions.  Plugin developers should provide documentation in a README.md file so that users know how to set it up.

## Plugin Directory Structure

The plugin system for iOS consists of several pieces:  There will be a JavaScript wrapper for the native functions.  A configuration file describes what files and dependencies need to be added to the native project before building.  And of course there is the native source code that interacts with your JavaScript wrapper.

A normal DevKit plugin has a simple directory structure:

~~~
├── README.md
├── android
│   ├── GeolocPlugin.java
│   ├── config.json
│   ├── manifest.xml
│   └── manifest.xsl
├── ios
│   ├── GeoLoc.h
│   ├── GeoLoc.mm
│   └── config.json
├── js
│   └── install.js
├── index.js
└── package.json
~~~

All plugins should have a `README.md` or similar documentation to describe how to install it and platform-specific considerations when using the plugin.

The `index.js` file is used by the addon system to notify the addon of `devkit` events.  And the `package.json` file is a nodejs configuration file that specifies npm dependencies for the `index.js` source code.

The `android` and `ios` directories contain configuration and code for the supported native targets.

The `js` directory contains JavaScript files that will be added to the JavaScript include path under the `plugins.myAddonName` namespace.

#### ./index.js

The index JavaScript file can export several methods that get invoked by `devkit` for different reasons:

+ `exports.init(common)` : Called when the addon is upgraded or first installed.

+ `exports.load(common)` : Called on all addons when the addon is loaded on `devkit` startup before a build begins.

~~~
exports.init = function (common) {
	var logger = new common.Formatter('geoloc');
	logger.log("Initializing");
	exports.load();
};

exports.load = function (common) {
	var logger = new common.Formatter('geoloc');
	logger.log("Loading");
}
~~~

Usually native plugins do not need to do anything in the index.js file, but the option is there if you need it.  Furthermore, you can leave out the index.js and package.json files entirely without causing any problems.

#### ./js/

The `js` directory contains JavaScript files that will be added to the JavaScript include path under `import plugins.addonName.*`.  In the case of geoloc, this is `import plugins.geoloc.install;` to import the `install.js` file.

You do not need to add a plugin to a game's `manifest.json` "addons" section to use JavaScript imports that it provides.

#### ./android/

Please see the [Android Plugin documentation](../native/android-plugin.html) for a description of the files under the `android` directory.

#### ./ios/

Please see the [iOS Plugin documentation](../native/ios-plugin.html) for a description of the files under the `ios` directory.

### Sending JavaScript Events to Native Code

To communicate with your native code from JavaScript, the `NATIVE.plugins.sendEvent` function is used to send data to the plugin, and the `NATIVE.events.registerHandler` function is used to receive data from the plugin.

For example, to send the GeoLocation plugin a request called "onRequest" with a JSON object as a parameter:

~~~
var e = {id:1, method:"getPosition", arguments:{}};
NATIVE.plugins.sendEvent("GeolocPlugin", "onRequest", JSON.stringify(e));
~~~

This causes the `onRequest` function in the `GeolocPlugin` native class to be invoked.

Another way to improve the readability of your code is to split the `NATIVE.plugins.sendEvent` call into a convenient wrapper.  In the following example from the [Facebook plugin](http://github.com/hashcube/facebook/), `pluginSend("login");` can be used instead of the long-hand form above:

~~~
function pluginSend(evt, params) {
	NATIVE && NATIVE.plugins.sendEvent("FacebookPlugin", evt,
				JSON.stringify(params || {}));
}

var Facebook = Class(function () {
	this.login = function() {
		pluginSend("login");
	};
});

exports = new Facebook();
~~~

### Receiving JavaScript Events from Native Code

To receive data from the plugin via the Events subsystem:

~~~
NATIVE.events.registerHandler('geoloc', function(evt) {
	if (evt.failed) {
	} else {
	}
});
~~~

The following `pluginOn` and `invokeCallbacks` functions are helpful wrappers from the [Facebook plugin](http://github.com/hashcube/facebook/) you can drop into your own plugin to easily handle callbacks from native code:

~~~
function pluginOn(evt, next) {
	NATIVE && NATIVE.events.registerHandler(evt, next);
}

function invokeCallbacks(list, clear) {
	// Pop off the first two arguments and keep the rest
	var args = Array.prototype.slice.call(arguments);
	args.shift();
	args.shift();

	// For each callback,
	for (var ii = 0; ii < list.length; ++ii) {
		var next = list[ii];

		// If callback was actually specified,
		if (next) {
			// Run it
			next.apply(null, args);
		}
	}

	// If asked to clear the list too,
	if (clear) {
		list.length = 0;
	}
}

var Facebook = Class(function () {
	var loginCB = [];

	this.init = function(opts) {
		pluginOn("facebookState", function(evt) {
			invokeCallbacks(loginCB, true, evt.state === "open");
		});
	}

	this.login = function(next) {
		loginCB.push(next);

		pluginSend("login");
	};
});

exports = new Facebook();
~~~

##### Matching requests to responses

If it is necessary to match a request with the response from the plugin, it is good practice to include an `id` field in the request, which can be provided in the response to match it with the request.

This is not demonstrated here since GeoLocation does not need to match responses with requests.  A JavaScript array keyed with these ids is a good way to remember the callback functions to call when a response arrives.

### Plugin JavaScript Design Recommendations

Note that the `geoloc` plugin is a special case that breaks out of the normal js.io class system to add features to the global `navigator` object.  The preferred way to develop addon JavaScript is by using the normal js.io class system.

For example, for the [MoPub](https://github.com/hashcube/mopub/) plugin there is a `js/moPub.js` file:

~~~
var MoPub = Class(function () {
	this.showInterstitial = function () {
		logger.log("{moPub} Showing interstitial");

		NATIVE && NATIVE.plugins.sendEvent("MoPubPlugin", "showInterstitial",
				JSON.stringify({}));
		}
	};
});

exports = new MoPub();
~~~

The JS object is imported in games that use it with:

~~~
import plugins.mopub.moPub as moPub;
~~~

And then games can use the `showInterstitial` method:

~~~
moPub.showInterstitial();
~~~

### Android Integration

Please see the [Android Plugin documentation](../native/android-plugin.html).

### iOS Integration

Please see the [iOS Plugin documentation](../native/ios-plugin.html).
