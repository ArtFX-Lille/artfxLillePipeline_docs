# A to Z VFX Workflow
*version 2026.1.0 for ArtFX 2026 promotion*

<p>

In this document, you will learn the proper workflow for your exports and for your EXR files from your footage. <br>
Once your edit in **DaVinci Resolve** is locked (final version or close to final), you can begin the **VFX Workflow**.
</p>

## Table of Contents

[Preliminary Information](#preliminary-information)<br>
[I. Convert .MXF to .EXR with Baked Metadata](#i-convert-mxf-to-exr-with-baked-metadata-using-arri-reference-tool-art)<br>
[II. Replace .MXF with ART .EXRs Using Reconform](#ii-replace-mxf-with-art-exrs-using-reconform)


## Preliminary Information

- This workflow follows the **Color Pipeline** developed by Gwénégan and Garrett. You can find the full documentation here : 
- The first step in the workflow uses the **ARRI Reference Tool (ART)** to bake the camera into each frame. Tristan created a script to bridge DaVinci Resolve to this tool. Documentation is available here : 
- For artists. Our color workflow is based on the **ARRI Reveal Color Science**. This means that in every DCC software used for **texturing, shading, or rendering,** you must ensure your display LUT is set to : **“Live Arri Reveal - Rec.1886 Rec.709 - Display”**. 
  
  - In Nuke : <img src="assets/images/nukeArriRevealLUT_001.png">
  - In Houdini : <img src="assets/images/houdiniArriRevealLUT_001.png">
  - In Substance :
  - In Mari :

## I. Convert MXF to EXR with Baked Metadata Using ARRI Reference Tool (ART)
<p>

The first is to export your **ARRI plates** using the **Tristan ARRI Reference Tool Bridge**. The goal is to replace your **timeline .mxf files** with **EXR sequences** that include baked camera metadata for each frame. For now, you can store these files in your **Shooting / OnSetProduction** folder.
</p>
<figure>
  <img src="assets/images/arriref_tool_output_001.jpg" height=600>
  <figcaption> ARRI Reference Tool Output </figcaption>
</figure>
<p>

The CMD script exports your footage using the *source name* of your clips, so you’ll need to rename them manually. You can use tools like **Bulk Rename Utility** or **PowerRename**, whichever you prefer. For this example we are using PowerRename:
</p>

<img src="assets/images/power_rename_step001.png" height=265>
<img src="assets/images/power_rename_step_0002.jpg" height=265>

Proper Naming Convention : `sqXXXX_shXXXX_art_plate0001_ap0_vXXXX.####.exr`

<p>

Once the files are correctly renamed, import your footage into your **Prism project** under the appropriate **shot**.

1. Go to the shot corresponding to this footage
2. Create a new identifier named **art_plate0001_ap0** (Type: 2D)
3. Use the **“Copy Path for Next Version”** function
</p>
<p>

This will automatically create an empty **v0001** folder containing a `.json` file. This ``.json`` file acts as a reference for the correct naming convention. <br>
Whenever you manually import external files into your Prism project, use the **“Copy Path for Next Version”** option to ensure consistency.
</p>

<img src="assets/images/prismProduct_versionCopyPath_001.png" height=296>
<img src="assets/images/prismProduct_versionCopyPath_002.png" height=296>

Once all your **ART EXRs** are exported and organized, you can move on to the next step.

## II. Replace .MXF with ART .EXRs using reconform
<p>

In this step, we’ll replace the **original MXF files** in your DaVinci Resolve timeline with the **ART EXR sequences**.

1. **Import the EXR images sequences** <br>
   In **DaVinci Resolve**, create a new **bin** and import all your **ART EXR images sequences** into it.
2. **Duplicate your timeline** <br>
   Before making any changes, **duplicate your edit timeline** to keep a safe backup of the original MXF version.
3. **Prepare for reconform** <br>

    - In your timeline, select the clips you want to replace.
    - Right-click to one of the selected and choose **“Conform Lock Enabled”**. <br>
      This unlocks the conform settings for those clips and prepares them for reconform.

<img src="assets/images/">

4. **Run the reconform process**

      - Right-click on the duplicate timeline
      - Go to **Timeline → Reconform from Bins.**
      - The **Conform from Bins** window will appear.
  