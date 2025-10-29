# ARRI Reference Tool Bridge Documentation
<p> This is the official ARRI Reference Tool Bridge for DaVinciResolve Studio <br>
Through this documentation, you will learn how to use the ARRI Reference Tool Bridge tool, its use cases and how to install it. </p>

## Table of Contents

[Installation](#installation) <br>
[Using the Tool](#using-the-tool) <br>
[Uninstallation](#uninstallation) <br>

## Installation

### Prerequisites
<p> To use properly ARRI Reference Tool Bridge for DaVinciResolve, you must have : </p>

- DaVinciResolve Studio Version 20.2 or later
- ARRI Reference Tool CMD v0.4.1 or later
- Python v3.10 or later

<p>

The ARRI Reference Tool CMD v0.4.1 must be installed inside your `C:/Program Files/ARRI/ARRIReferenceTool_cmd/` directory. If it's not the case, create the folder and put the content of the ZIP file from [ARRI Reference Tool](https://www.arri.com/en/learn-help/learn-help-camera-system/tools/arri-reference-tool) website. 
</p>

<p>

Python v3.10 should be installed in your `C:/Program Files/Python/` directory. You can install Python directly from the official [Python](https://www.python.org/downloads/windows/) website.

</p>

### ⚠️ Attention : Only DaVinciResolve Studio version (paid version) is compatible with the ARRI Reference Tool Bridge. </br> Otherwise, the workflow integration plugin won't be recognized by DaVinci Resolve.

---

### Installing ARRI Reference Tool Bridge
<p>

Follow those steps to install ARRI Reference Tool Bridge for DaVinciResolve Studio :
</p>

1. Go to the directory : `C:\Program Data\Blackmagic Design\DaVinci Resolve\Support`.
   
   > If you can't see the folder `Program Data` in your `C:\` drive, check those options :
   >
   > - **On Windows**
   >    
   >    1. In Windows explorer, click on the **View** <img src="assets/images/windows_display_hidden_step01.jpg" width="50"> button.
   >
   >    2. Then go to **Show > Hidden items**
   >
   >       <img src= "assets/images/windows_display_hidden_step02.jpg" width="400">

2. Inside the directory, create a folder called `Workflow Integration Plugins`.
3. Paste the `arriRefToolBridge.py` file and the `assets` folder inside the `Workflow Integration Plugins` folder.
4. Restart DaVinciResolve if it was already opened.

## Using the tool

Before running the tool, you must have a DaVinci Resolve **project** opened and a **timeline** opened. <br>
Then, click on `Workspace > Workflow Integration Plugins`

## Uninstallation
<p>

To remove the tool, you just have to delete  `arriRefToolBridge.py` file and the `assets` folder from the `Workflow Integration Plugins` folder.