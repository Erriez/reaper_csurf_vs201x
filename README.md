# Building REAPER CSurf with Visual Studio
This repository contains a REAPER Surface Controller (CSurf) project for all Reaper versions, 
running on Windows 32 and 64 bit. It describes step by step how to build and debug with 
Microsoft Visual Studio (Free Community Edition).

## Branches
This Feature/SingleCSurf branch contains one single CSurf controller as 
requested on [GitHub](https://github.com/Erriez/reaper_csurf_vs201x/issues/1) 
It's based on the BCF2000 as starting point and listed in the Reaper | 
Preferences | Control/OSC/web dialog box as "My Device".

## Introduction
A [surface controller](https://en.wikipedia.org/wiki/Audio_control_surface) allows users to control
a DAW ([Digital Audio Workstation](https://en.wikipedia.org/wiki/Digital_audio_workstation)), in 
this case REAPER. Supporting new surface controllers in REAPER is not well documented. The goal of 
this project to provide you a quick-start.

The free C++ REAPER Extension SDK is required to build a Surface Controller for REAPER. It is based 
on an old Visual C++ 6 project and supports a 32-bit version only for REAPER. Upgrading the project 
to a newer Visual Studio versions requires some project changes and knowledge which are already 
applied.

## Prerequisites
* Windows (7/8/10)
* Microsoft Visual Studio (Tested with the free Community Edition)
* REAPER version 4 or 5, 32 or 64 bit
* REAPER Extension SDK
* A compatible surface, for example:
  * MCU compatible surface such as:
    * [Behringer X-Touch](https://www.google.nl/search?q=behringer+x-touch) 
    * [Mackie Controller](https://www.google.nl/search?q=mackie+mcu+pro)
  * [Behringer BCF2000](https://www.google.nl/search?q=behringer+bcf2000)
  * [Presonus faderport](https://www.google.nl/search?q=presonus+faderport)
* C++ knowledge

### Installation
1. Install Visual Studio 2013, 2015 or 2017 (Free Community Edition) or higher.

2. Download REAPER SDK from:
https://www.reaper.fm/sdk/plugin/reaper_extension_sdk.zip

3. Extract ```reaper_extension_sdk.zip``` for example into ```C:\reaper_extension_sdk\```.

4. Add an environment variable to the REAPER Extension SDK:
* Start ```Windows Explorer```
* Right mouse click ```This PC```
* Click ```Advanced system settings```
* Click ```Environment Variables...```  
* Create a new ```System variable:```
  * Variable name: ```REAPER_EXTENSION_SDK```
  * Variable value: ```C:\reaper_extension_sdk```

5. Clone this repository with the GIT command: 
```git clone https://github.com/Erriez/reaper_csurf_vs201x.git```
or download and extract the ZIP of this repository.

6. Open the solution ```Builds\VisualStudio<VERSION>\reaper_csurf.sln``` with Visual Studio.  
   Note: Replace ```<VERSION>``` with the Visual Studio version, for example 2013, 2015 or 2017.

7. Select in the toolbar:
 * ```Debug``` (Default)
 * For REAPER 64-bit: ```x64``` (Default)
 * For REAPER 32-bit: ```x86```

8. Open the ```Solution Explorer``` | Right mouse click ```reaper_csurf``` | Properties: 
   * Select ```Configuration```: ```Debug``` or ```Release```.
   * Select ```Platform```: ```Win32``` or ```x64```.
   * Set ```Debugging | Command:```
     * For REAPER 32-bit: ```C:\Program Files (x86)\REAPER\reaper.exe```
     * For REAPER 64-bit: ```C:\Program Files\REAPER (x64)\reaper.exe```
   * Check the REAPER Extension SDK environment variable:
     * ```VC++ Directories | Include Directories:``` should contain ```$(REAPER_EXTENSION_SDK)```
       or the full path to the REAPER Extension SDK.
   * Set ``` Build Events | Post-Build Event | Command Line:``` 
     * For REAPER 32-bit: ```copy "x64\Debug\reaper_csurf_x86.dll" "%APPDATA%\REAPER\UserPlugins\"```
     * For REAPER 64-bit: ```copy "x64\Debug\reaper_csurf_x64.dll" "%APPDATA%\REAPER\UserPlugins\"```
     
9. Rename original REAPER CSurf DLL to something else to prevent conflicts between the original and
   new CSurf: 
   * For REAPER 32-bit: ```C:\Program Files\REAPER\Plugins\reaper_csurf.dll.org```
   * For REAPER 64-bit: ```C:\Program Files\REAPER (x64)\Plugins\reaper_csurf.dll.org```

10. Set a breakpoint in ```csurf_main.cpp``` function ```REAPER_PLUGIN_ENTRYPOINT()```.

11. Click the green ```Local Windows Debugger``` button to build and start debugging.

12. REAPER will be started automatically and the breakpoint should be hit. Now you're ready to debug 
    and add support for new surfaces or change the behavior of existing surfaces.
    
13. In REAPER click ```Tools | Preferences | Control/OSC/web``` Click ```Add``` to add a surface.

### Wiki
A more detailed description of the CSurf project settings and code is located on
[the Wiki page](https://github.com/Erriez/reaper_csurf_vs2015/wiki).

### FAQ

> Q: Is OSX supported?  

A: Unfortunately, I could not find any documentation how build it. 
Volunteers are welcome to send me a fully tested pull-request. I don't have a MAC, so can't test it. 

### Contact
Please use the [REAPER forum](https://forum.cockos.com/showthread.php?p=1884391).
