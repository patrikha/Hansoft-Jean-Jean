Jean for Hansoft
================

Overview
--------
Jean for Hansoft is a Windows Service that you can use to automate  updates of different kinds in or between your Hansoft projects. Jean
allows you to add custom behaviors that are triggered when changes are made in a Hansoft project where a behavior typically performs a certain
kind of update of some Hansoft items. There are currently four example behaviors that potentially are useful as they are. You can also implement
your own custom behavior and plug it into Jean.

The example behaviors are:
* Number - Add a hierarchical or sequential number in a column in the Schedule or the Product Backlog  that is automatically updated. 
* Copy - Copy a column value from one item to another linked item. Useful for example to propagate values across projects.
* Derive - Derive a column value automatically based on an expression referring to other columns and/or parent and/or child items
* DefaultValue - Specify a default value for one or several columns that should be applied when items when are created.
* AssignReleaseRisk - Tracks the risk that product backlog items will miss their assigned release date.
* TrackLastStatusChange - Track when product backlog items last changed their status. 

Terms and conditions
--------------------
Jean for Hansoft by Svante Lidman (Hansoft AB) is licensed under what is known as an MIT License as stated in the [LICENSE.md](LICENSE.md).

This program is not part of the official Hansoft product or subject to its license agreement.
The program is provided as is and there is no obligation on Hansoft AB to provide support, update or enhance this program.
Questions can be sent to svante.lidman@hansoft.com and will be answered when other obligations so permit.

Building the program
--------------------
Jean can be built with the freely available [Visual Studio Express 2012 for Desktop] [1].

Jean is built using the [Topshelf] [2]
library for building Windows Services which you will need to download separately and then change the reference
to the Topshelf DLL in the Visual Studio Project as needed.

You will also need the [Hansoft SDK] [3] to be able to build the program. You will
also need to update the references to the appropriate 
Hansoft SDK DLL in the Visual Studio project (typically: HPMSdkManaged.x86) and make sure that the Hansoft SDK DLL (typically HPMSdk.x86.dll) is
in the same directory as your executable.

Reference documentation in the form of Html help files (.chm) are built for the ObjectWrapper, SimpleLogging, and Behavior.  To build the help files
you need to install [Doxygen] [4] and [Html Help Workshop] [5].

The Visual Studio Projects are currently setup to build for x86 (which also works fine on a 64 bit system). If you instead want to build for
x64 you need to change the build settings in the project and also change the reference to the x64 version of the Hansoft .Net API. You should
also copy the x64 version of the Hansoft SDK dll to your target directory (i.e., where Jean.exe is found).

[1]: http://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-windows-desktop  "Visual Studio Express 2012 for Desktop"
[2]: http://topshelf-project.com/                                                                  "TopShelf"
[3]: http://hansoft.com/support/downloads/                                                         "Hansoft SDK"
[4]: http://www.stack.nl/~dimitri/doxygen/                                                         "Doxygen"
[5]: http://www.microsoft.com/en-us/download/details.aspx?id=21138                                 "HTML Help Workshop"

Installation
------------
You can either build Jean for Hansoft from source or install a prebuilt version.

Note: You need to have Administrator rights on the machine where you are installing Jean for Hansoft.

Note: It is not reccommended that you install Jean for Hansoft on the same machine that hosts your Hansoft Server installation. As Jean
can perform arbitrarily complex behavior the server (CPU primarily) load generated by your Jean setup should be assessed before deploying it
on the same machine as your Hansoft Server. 

### Installing from your own build
Once you have built Jean and any custom behaviors you have created you need to put the following files into a suitable directory:
* Jean.exe
* Behavior.dll
* ObjectWrapper.dll
* Any behavior DLL's that you want to use (e.g. CopyBehavior.dll, NumberBehavior.dll, CustomBehavior.dll and so on)
* JeanSettings.xml
* Topshelf.dll
* The Hansoft SDK library, typically HPMSdk.x86.dll
* The managed wrapper for the SDK library, typically HPMSdkManaged_4_5.x86.dll

### Installing the prebuilt version
There is a [prebuilt version] (Jean-Executable.zip) (Click "View Raw" to download) of Jean also that you can use and which will include all the files mentioned above. If you use the prebuilt version, make sure
that you have the [.NET Framework 4.5] (http://www.microsoft.com/en-us/download/details.aspx?id=30653)  and the [Visual Studio 2012 VC Redist] (http://www.microsoft.com/en-us/download/details.aspx?id=30679) installed on your intended server.

### Starting Jean for Hansoft
You can then install/uninstall the Windows Service from the command line like this:
	Jean install
	Jean uninstall

Once the service is installed you can manage it from Control Panel/Administrative Tools/Services. You can also start/stop it from the command line like this:
    Jean start
    Jean stop

For a description of all command line options see the [TopShelf documentation] (http://docs.topshelf-project.com/en/latest/overview/commandline).html

Troubleshooting
---------------
Check the Application section under Windows logs in the Windows Event Viewer for messages. Error messages are brute but should give you hint of
if there is a configuration problem or some other issue that prevents Jean from working as expected.

Settings
--------
There are settings for the Jean service as a whole including specifying the Hansoft server instance and database that you want to connect to.
You can also specify additional dll's to be loaded (for example different behaviors).

There are also settings for each behavior instance that specifies the parameters (if any) of the behavior. Each behavior section should be named
consistently with the class defining behavior and the dll where it is placed. For example, the DefaultValueBehavior is defined by the class
DefaultValueBehavior in DefaultValueBehavior.dll and is instantiated by adding a <DefaultValue> element to the settings file.

All settings are described in detail in the settings file [JeanSettings.xml] (JeanSettings.xml)

Note: If you change the settings you need to restart the Jean service for them to take effect.

Creating your own behaviors
---------------------------
You create your own behavior by subclassing the abstract base class AbstractBehavior (in Behavior.dll) and overriding the event handler
that you are interested in.

To get started take a look at the provided examples. The DefaultValue behavior is a very simple behavior but demonstrates a lot of the
mechanics.

Refer also to the detailed documentation of AbstractBehavior found in the [API Reference] (https://github.com/Hansoft/Hansoft-Jean-Behavior/blob/master/Behavior.chm).

Design
------
The foundational design ideas for Jean for Hansoft are:
* Easy to install and configure and safe to deploy
* Easy to add your own behaviors based on an abstraction of the Hansoft API and by abstracting away the Windows Service details and the
  Hansoft event handling
* Uncoupled behaviors

Limitations and gotchas
-----------------------
Not all columns are (yet) supported in the Object Wrapper, nor is support for all columns implemented in the provided example behaviors. Refer
to JeanSettings.xml to see which columns that are supported for each behavior and the API reference for the ObjectWrapper (ObjectWrapper.chm).

When you set up your behaviors you should be careful to not create circular chains of events.

About the ObjectWrapper
-----------------------
Jean is built on a wrapper around the Hansoft .Net API called ObjectWrapper. ObjectWrapper is a state-less object-oriented wrapper around the
Hansoft API to provide an abstraction that is in line with the end user mental model of the different artifacts in Hansoft. It is not specific
to Jean but can be used to build different kinds of SDK applications.

Please refer to the example behaviors for how to use ObjectWrapper in the context of Jean for Hansoft.

For detailed information about ObjectWrapper refer to the [ObjectWrapper API Reference] (https://github.com/Hansoft/Hansoft-ObjectWrapper/blob/master/ObjectWrapper.chm).

Why is it called Jean?
======================
Jean is the name of the butler in the play [Miss Julie] (http://en.wikipedia.org/wiki/Miss_Julie) by Swedish author and playwriter
[August Strindberg] (http://en.wikipedia.org/wiki/August_Strindberg).  
