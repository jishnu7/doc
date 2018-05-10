# Windows Guide

This guide will get you up and running with the Game Closure
DevKit. First, you'll download and install the DevKit, then get introduced
to `devkit`, our command-line tool for creating new projects
and development. Once everything is installed and ready
to go, please read the [Hello, World! Guide](../guide/hello-world.html)
to create your first application.


## Prerequisites

The following is a list of prequisites for running the Game Closure DevKit on Windows.
Download the following projects and refer to the sections below for configuration tips.

**Web and Native Development**

* [msysgit](http://msysgit.github.com/) (> 1.7.10)
	* The full installer should be used since it contains the `git bash` command prompt that is needed to run `devkit` and the commands below. THe default options during the installation process will work with the GC devkit.
* [Node.js](http://nodejs.org) (> 0.8)
* [npm](https://npmjs.org/package/windows)
* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/)
* [Chrome](http://www.google.com/chrome)
	* The Chrome web browser is the preferred development
environment because of its superior debugging tools. This is
the browser we use to develop the Game Closure DevKit and test against.

**Android Development**

* [Android SDK](http://developer.android.com/sdk/)
	* The full adt bundle comes as a zip file rather than an installer and is not required. The download listed under `use existing editor` will also work, is smaller, and also provides an installer.
* [Android NDK](http://developer.android.com/tools/sdk/ndk/)
* [Apache Ant](http://ant.apache.org/manual/install.html)

For more information, see the [Android Setup Guide](http://doc.gameclosure.com/native/android-setup.html).

###Installation Tip

The easiest way to install these packages is through their
respective installers. Note the location that each of the above dependencies is installed to because they will need to be added to your Windows environment PATH. For some of the above dependencies they come as zip downloads rather than installers. In these cases, they need to be unzipped and the location of the unzipped files needs to be added to your PATH. *[This is true for the **Android NDK**, **ant** build tools, and the **Android SDK** when using the adt bundle]*

## Add the tools to your PATH
Once the above dependencies are downloaded and installed, it is important that the following directories are listed in your PATH environment variable. Note that the location of these directies depends on where you installed the dependencies.

**Web and Native Development**

* **{npm install location}**
* **{java install location}**\\jdk1.7.0_13\\bin

**Android Development**

* **{android-ndk install location}**\\android-ndk-r9\\prebuilt\\windows\\bin
* **{android-ndk install location}**\\android-ndk-r9
* **{ant install location}**\\apache-ant-1.8.4\\bin
* **{android-sdk install location}**\\android-sdk\\tools
* **{android-sdk install location}**\\android-sdk\\platform-tools

Depending on your version of Windows, the full path to the NDK bin directory may look like this instead:

* **{android-ndk install location}**\\android-ndk-r9\\prebuilt\\windows-x86_64\\bin

If you need assistance configuring the path refer to this [document](http://docs.oracle.com/javase/tutorial/essential/environment/paths.html) which describes how to configure the java environment variables.

For example, your path may contain the following after adding everything. Note that some of the paths below are added automatically by installers and Windows. Also be aware that these paths will be different based on the location where you installed the dependencies.

```
C:\android-ndk-r9\prebuilt\windows\bin;
C:\android-ndk-r9;
C:\Program Files\Java\jdk1.7.0_13\bin;
C:\apache-ant-1.8.4\bin;
C:\Users\gc\AppData\Local\Android\android-sdk\tools;
C:\Users\gc\AppData\Local\Android\android-sdk\platform-tools;
C:\Users\gc\AppData\Roaming\npm\;
%SystemRoot%\system32;%SystemRoot%;
%SystemRoot%\System32\Wbem;
%SYSTEMROOT%\System32\WindowsPowerShell\v1.0\;
C:\Program Files\nodejs\
```

###JAVA HOME
Your **JAVA_HOME** environment variable should also be set to the location of your java directory. For example: `C:\Program Files\Java\jdk1.7.0_13`

For more information on managing environment variables in Windows, see [Microsoft's official docs](http://support.microsoft.com/kb/310519).

## Install the DevKit

With the prerequisites installed, we're ready to set up the
Game Closure DevKit. The library and tools are open source and
hosted in our [GitHub repository](https://github.com/hashcube/devkit).
We'll be using the `git` tool to keep our software updated,
and there is [lots of help](https://help.github.com)
available for how to use it.

To download the Game Closure DevKit, open a `git bash` terminal, and issue the following
commands:

~~~
$ git clone git@github.com:hashcube/devkit.git
$ cd devkit
~~~

This downloads the DevKit in to the `./devkit` directory located
in your current working path. Switch to that directory and
run the install script:

~~~
$ ./install.sh
~~~

This installs the command-line program `devkit` which is used
create and serve projects. To see a list of devkit commands,
simply run it in your terminal without any arguments:

~~~
$ devkit
~~~

This prints out a list of devkit commands. You can get
help for a particular command by typing `devkit help [command]`,
for example:

~~~
$ devkit help init
~~~

Some commands also take a `-h` argument to show help.

## Update the DevKit

To keep your copy of the DevKit up to date, run the update command:

~~~
$ devkit update
~~~

The update command is also able to check out older versions of the DevKit or different channels of DevKit versions.  Read the help menu with `devkit update --help` for more information on this flexible tool.

## Play With Examples

Once `devkit` is installed you're ready to check out a few
example projects. First, take a look at the [Hello, World! Guide](../guide/hello-world.html)
to see how to create a new project. After that, you should
be ready to browse some more complicated projects. There are
a bunch contained within the *examples addon*, which comes with
a fresh DevKit install.

This addon installs a collection of projects that demonstrate
some game development techniques using the Game Closure
DevKit. When you run devkit you'll be able to browse and run the
projects, and also see the annotated source for each.
