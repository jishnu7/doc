# Android Plugin System

Writing the Android native code for a DevKit plugin is covered in this guide, which uses the [Geolocation plugin](http://github.com/hashcube/geoloc) as an example.

For an overview of the plugin system, the requisite directory structure, and how to write the JavaScript component of the plugin, please see the [main plugin development guide](../guide/plugins.html).

### Usage

First install the plugin with `devkit install [plugin name]`.  For example:

~~~
devkit install geoloc
~~~

To test plugins you will want to add the required plugins to your game's manifest.json file:

~~~
"addons": [
	"geoloc"
],
~~~

And do a full build:

~~~
devkit debug native-android --clean --open
~~~

This will add the required libraries and code for the plugin to the native stack so that it will be present during runtime.

Some plugins require additional configuration.  Please read the README.md file that comes with the plugin to see what additional steps you should take to use it.

The Android Test App *does not* work with native plugins at this time.  You will see warnings that look like:

~~~
{plugins} WARNING: Event could not be delivered for plugin: GeolocPlugin
~~~

### Android Plugin: android/config.json

Under `addons/geoloc/android/config.json` is the configuration used while building Android targets with the GeoLocation plugin.

It specifies Java code, libraries, JARs, and manifest changes required to build the Android plugin:

~~~
{
		"copyGameFiles": [
			"leadbolt.jar"
		],
		"copyFiles": [
			"GeolocPlugin.java",
			"res/layout/airpush_notify.xml"
		],
		"libraries": [
		],
		"jars": [
			"leadbolt.jar"
		],
		"injectionXML": "manifest.xml",
		"injectionXSL": "manifest.xsl",
		"injectionSource": [
			{
				"regex": "LEADBOLT_PACKAGE",
				"keyForReplace": "leadBoltPackage"
			}
		]
}
~~~

##### copyGameFiles

Sometimes it's required to copy a file from the user's game directory into the final build product.  You can accomplish this by adding a string to the "copyGameFiles" array.

This will copy the given files from the game folder, relative to the game's `manifest.json` root directory, into the addon folder's `/android/` before performing any of the other build steps.

For example you can allow each game to specify a `leadbolt.jar` file that then gets used during building by also adding the .jar file to the "copyGameFiles" and "jars" array.

##### copyFiles

###### copyFiles: Java Code

This key should list any Java source code files in the `addons/geoloc/android/` directory that will be incorporated into the Android build.  This includes any .IADL files.  The `devkit` build scripts will pull the package name out of the Java files.

The TeaLeaf Java source code will be built first, and then the Activity for your game will be built.  It is in this second build step where your plugin code is built.

If your package is under `com.tealeaf.plugin.plugins`, then the file will be copied into the destination build directory under `src/com/tealeaf/plugin/plugins` and the `ant` build will incorporate them.

###### copyFiles: Resources

The `copyFiles` key can also be used to copy .xml and other resources into the `res/` directory for the Android build.  The way to do this is to create a new "res" directory under your plugin's "android" directory.  Then create subdirectories under "res" for any resources you want to include.  For each resource, add it to the `copyFiles` key.

For example, the [Airpush plugin](http://github.com/hashcube/airpush) includes an XML layout file.  Here is the plugin directory structure:

~~~
.
├── README.md
├── android
│   ├── AirpushPlugin.java
│   ├── airpush.jar
│   ├── config.json
│   ├── manifest.xml
│   ├── manifest.xsl
│   └── res
│       └── layout
│           └── airpush_notify.xml   <-- note this file location
├── index.js
├── ios
│   └── config.json
└── js
    └── airpush.js
~~~

And the android/config.json file has this resource listed under the `copyFiles` section:

~~~
	"copyFiles": [
		"res/layout/airpush_notify.xml"
	],
~~~

##### libraries

A list of .a static library files in the `addons/geoloc/android/` directory.

##### jars

A list of .jar files in the `addons/geoloc/android/` directory.

##### injectionXML

To add permissions and other AndroidManifest.xml keys, you can edit the `manifest.xml`.

For example to add permissions for the GeoLocation plugin, the ACCESS_FINE_LOCATION permission was added:

~~~
	<!--START_PLUGINS_MANIFEST-->
		<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
	<!--END_PLUGINS_MANIFEST-->
	<!--START_PLUGINS_ACTIVITY-->
		<intent-filter>
			<action android:name="globosocial.intent.action.LAUNCH" />
			<category android:name="android.intent.category.DEFAULT" />
		</intent-filter>
	<!--END_PLUGINS_ACTIVITY-->
	<!--START_PLUGINS_APPLICATION-->
		<meta-data android:name="tapjoyAppID" android:value="" />
	<!--END_PLUGINS_APPLICATION-->
~~~

Permissions should be added to the `PLUGINS_MANIFEST` section.  Application section changes will go in the `PLUGINS_APPLICATION` section as shown above.

The `PLUGINS_ACTIVITY` section can be used to add intents that the main activity will respond to, which allows third party apps to launch your game.

Additionally, any game `manifest.json` "Android" subkeys you would like to have available in your Java code should be added as values under the plugins application section as shown above for "tapjoyAppID."

##### injectionXSL

If you need to use XSLT parsing to modify the AndroidManifest.xml then you can provide a `manifest.xsl` that gets passed through xslt during the build.

This is the method you may use to access `manifest.json` keys.  In the following example, "tapjoyAppID" is routed from `android:tapjoyAppId` in `manifest.json` to a key that is accessible from the Java code:

~~~
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:android="http://schemas.android.com/apk/res/android">

	<xsl:param name="tapjoyAppID"></xsl:param>

	<xsl:template match="meta-data[@android:name='tapjoyAppID']">
		<meta-data android:name="tapjoyAppID" android:value="{$tapjoyAppID}"/>
	</xsl:template>

	<!--	<xsl:strip-space elements="*" />-->
	<xsl:output indent="yes" />

	<xsl:template match="comment()" />

	<xsl:template match="@*|node()">
		<xsl:copy>
			<xsl:apply-templates select="@*|node()" />
		</xsl:copy>
	</xsl:template>

</xsl:stylesheet>
~~~

Note that `manifest.json` keys typically follow the convention that the first character is lower-case and word breaks are camel-cased.

##### injectionSource

The "injectionSource" key is an array of JSON objects with the format:

~~~
	{
		"regex": "LEADBOLT_PACKAGE",
		"keyForReplace": "leadBoltPackage"
	}
~~~

It allows you to replace strings in the plugin Java code before building with strings from the game's `manifest.json` file.  In the example above, Java code like this:

~~~
private String leadBoltPackage = "LEADBOLT_PACKAGE";
~~~

Will be replaced with this before building:

~~~
private String leadBoltPackage = "com.mypackage";
~~~

Assuming that the game's `manifest.json` contains the key:

~~~
	"android": {
		"leadBoltPackage": "com.mypackage"
	}
~~~

### Android Plugin: Code

Your plugin class must be packaged under `com.tealeaf.plugin.plugins` with a `package com.tealeaf.plugin.plugins;` line:

~~~
package com.tealeaf.plugin.plugins;
~~~

Your Java code will implement the IPlugin interface that is imported with a `import com.tealeaf.plugin.IPlugin;` line.  There are a number of other useful imports that are worth looking into:

~~~
import com.tealeaf.plugin.IPlugin;
import com.tealeaf.event.*;
import com.tealeaf.EventQueue;
import com.tealeaf.logger;
import org.json.JSONObject;
import org.json.JSONArray;
import org.json.JSONException;
~~~

The IPlugin interface provides a number of callbacks for common events that occur while running games, which you can react to:

~~~
public interface IPlugin {
	public void onCreate(Activity activity, Bundle savedInstanceState);
	public void onCreateApplication(Context applicationContext);
	public void onResume();
	public void onStart();
	public void onPause();
	public void onStop();
	public void onDestroy();
	public void onNewIntent(Intent intent);
	public void onActivityResult(Integer request, Integer result, Intent data);
	public void setInstallReferrer(String referrer);
}
~~~

##### Reading manifest.json keys at runtime

If the keys were routed using the `manifest.xsl` method shown above, then the keys can be read at runtime like this:

~~~
	HashMap<String, String> manifestKeyMap = new HashMap<String,String>();

	public void onCreate(Activity activity, Bundle savedInstanceState) {
		PackageManager manager = activity.getBaseContext().getPackageManager();
		String[] keys = {"tapjoyAppID", "OtherKey"};
		try {
			Bundle meta = manager.getApplicationInfo(activity.getApplicationContext().getPackageName(),
					PackageManager.GET_META_DATA).metaData;
			for (String k : keys) {
				if (meta.containsKey(k)) {
					manifestKeyMap.put(k, meta.get(k).toString());
				}
			}
		} catch (Exception e) {
			logger.log("Exception while loading manifest keys:", e);
		}

		String tapjoyAppID = manifestKeyMap.get("tapjoyAppID");
	}
~~~

Note here that we are doing `meta.get(k).toString()`, where `k` is a manifest.json key under the "Android" section.  It is a mistake to do `meta.toString(k)` because this will cause an exception when the manifest.json key contains integer-like data such as "1234567".


##### Handling JavaScript Events

It is important that the JavaScript code specifies the exact plugin class name and method to call.  Note that the above class matches the JavaScript name:

~~~  
NATIVE.plugins.sendEvent("GeolocPlugin", "onRequest", '{"method":"getPosition"}');
~~~

So the corresponding class for this would expose an onRequest function:

~~~
public class GeolocPlugin implements IPlugin {
	public void onRequest(String jsonData) {
		try {
			JSONObject data = new JSONObject(jsonData);

			String method = data.optString("method", "getPosition");

			// Handle getPosition()
			if (method.equals("getPosition")) {
				logger.debug("{geoloc} Got position request");
			}
		} catch (Exception e) {
			logger.log(e);
		}
	}
}
~~~

##### Sending JavaScript Events

To set up an Event that will get passed back to JavaScript write a class that looks like this:

~~~
public class GeolocEvent extends com.tealeaf.event.Event {
	boolean failed;
	double longitude, latitude;
	public GeolocEvent(double longitude, double latitude) {
		super("geoloc"); // Choose an event name
		this.failed = false;
		this.longitude = longitude;
		this.latitude = latitude;
	}
}
~~~

The Event name is chosen in the constructor.  It should be somewhat unique to prevent naming conflicts.

To pass the event to JavaScript, create a new instance of your Event class and push it onto the `EventQueue`:

~~~
EventQueue.pushEvent(new GeolocEvent(83.2106, -10.1984));
~~~

The EventQueue will serialize the event for you into a string that can be read from JavaScript in the NATIVE event handler.

Your JavaScript wrapper can listen for this event by registering for it:

~~~
NATIVE.events.registerHandler('geoloc', function(e) {
	if (!e.failed) {
		logger.log("{geoloc} Got response:", e.longitude, e.latitude);
	}
});
~~~

It is a good idea to wrap this message passing code inside your JavaScript wrapper so that the user of your plugin does not need to call any NATIVE functions directly.
