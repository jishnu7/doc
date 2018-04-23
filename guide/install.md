# 1. Quick Start: Installation

This guide will get you up and running with the Game Closure
DevKit. First, you'll download and install DevKit, then get introduced
to `devkit`, our command-line tool for creating new projects
and development. Once everything is installed and ready
to go, please read the [Hello, World! Guide](../guide/hello-world.html)
to create your first application.

This guide is mainly for Linux or OSX.  To install on Windows please follow the [Windows install guide](../guide/windows-guide.html) first.

## Prerequisites

Install the following software before installing the Game Closure DevKit:

* [Git](http://git-scm.com) (> 1.7.10)
* [Node.js](http://nodejs.org) (> 0.8)
* [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html) ---Will auto-install on OSX.
* [Chrome](http://www.google.com/chrome)

The easiest way to install these packages is through their
respective installers. Alternatively, you can use a package
manager, such as [Homebrew](http://mxcl.github.com/homebrew/)
on OSX, to keep your system updated. Just be aware that
you'll need to download and set up the [Xcode Command-line Tools](https://developer.apple.com/xcode/)
to build these packages.

The Chrome web browser is the preferred development environment because of its superior debugging toolset.

If you're using OSX and building for native devices, you'll need to install the following from the [Apple Developer Site](https://developer.apple.com/downloads/).  You may need to advance to the second page of downloads to find each of these items:

* XCode Command Line tools (November 2012 or newer)
* XCode (4.4 or newer)

## Recommended Code Editor

On MacOSX, we recommend editing your game's JavaScript code with [Sublime Text 3 Beta](http://www.sublimetext.com/3) with the [NBSP plugin](https://github.com/possan/sublime_unicode_nbsp) installed.

The NBSP plugin allows you to easily catch extended ASCII multi-character codes like NBSP that tend to find their way into JavaScript files.  This leads to hard-to-debug problems where the code looks fine in the editor, but a browser like Chrome will choke when your code is evaluated.  Usually the error will be something like "Unexpected token {".  This can also happen in JSON files you edit.

Once you have Sublime Text 3 installed, to install the NBSP plugin:

+ Open "Sublime Text" application.
+ In the "Sublime Text" menu in the upper left, select "Preferences" => "Browse Packages…"
+ Your User folder should open in Finder.
+ Drag the "sublime_unicode_nbsp.py" file from the plugin into your User folder.

Now problematic characters like NBSP will be highlighted in red and you can delete them and replace them with normal spaces.

## Install DevKit

With the prerequisites installed, we're ready to set up the
Game Closure DevKit. The library and tools are open source and
hosted in our [GitHub repository](https://github.com/hashcube/devkit).
We'll be using the `git` tool to keep our software updated,
and there is [lots of help](https://help.github.com)
available for how to use `git`.

To download the Game Closure DevKit, issue the following
commands at the terminal:

~~~
$ git clone https://github.com/hashcube/devkit
~~~

This downloads the DevKit in to the `./devkit` directory located
in your current working path.  A good place to put it would be in your
home directory.  Switch to that directory and run the install script:

~~~
$ cd devkit
$ ./install.sh
~~~

This installs the command-line program `devkit`, which is used to
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

Some commands can also take a `-h` argument to show help.

## Update the DevKit

To keep your copy of the DevKit up to date, run the update command:

~~~
$ devkit update
~~~

The update command is also able to check out older versions of the DevKit or different channels of DevKit versions.  Read the help menu with `devkit update --help` for more information on this flexible tool.

## Hello, World!

Once `devkit` is installed you're ready to create a new game project.  Take a look at the [Hello, World! Guide](../guide/hello-world.html) to see how to get started using the Game Closure DevKit.

