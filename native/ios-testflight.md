# TestFlight Guide


##Team Setup

Browse to your [testflightapp.com](http://testflightapp.com) account.

If the user is new, click [Add Teammate] in the upper right.

1. Send them an invite and have them accept it.
2. Have them add a device to their test account.

Click the People tab at top left.
Create a new distribution list of the testers you want to have access to your app.
View your distribution list by selecting it on the bottom left.

Under the |Actions| drop-down at the right, select "Export Devices".  -- This exports the device IDs to `testflight_devices.txt`.

Browse to your developer.apple.com Member Center : Provisioning Portal : Devices section.
Use the **[Upload Devices]** or **[Add Devices]** feature to add the devices from the file.

Browse to your developer.apple.com Member Center : Provisioning Portal : Provisioning section.
Create or edit a provisioning profile for your app.
Click _Edit_ on the right, and select Modify.

While modifying iOS Development Provisioning Profile, include yourself in the Certificates list.
Select the devices for the users you want to give access to.
Click **[Download]** to download the .mobileprovision file.


##Building

Edit your `manifest.json` or your devkit global `config.json` to add the following keys:

~~~
ios:developer="Christopher Taylor"  (change to your name)
ios:provision="/Users/cat/Desktop/Kiwi_Run.mobileprovision"  (change to your file)
~~~

From your app directory run this command:
`$ devkit build native-ios --ipa --no-compress`
It will create an .IPA file under build/native-ios/appShortName/appShortName.ipa

Browse to your [testflightapp.com](http://testflightapp.com) account.
Since this is a new build, click **[Upload Build]** in the upper right.
Select the IPA file you built, and add some release notes, then hit **[Upload>>]**.

Click the Builds section at the top, and add that distribution list to the build.
You can optionally notify users via Email.

On the iPhone/iPad:
Open TestFlight and note that the new build is now available in the list.

Install and run!
