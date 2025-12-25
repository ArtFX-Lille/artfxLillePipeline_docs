# A to Z VFX Workflow
*version 2026.1.0 for ArtFX 2026 promotion*

<p>

In this document, you will learn the proper workflow for your exports and for your EXR files from your footage. <br>
Once your edit in **DaVinci Resolve** is locked (final version or close to final), you can begin the **VFX Workflow**.
</p>

## Table of Contents

[Preliminary Information](#preliminary-information)<br>
[1. Convert .MXF to .EXR with Baked Metadata](#1-convert-mxf-to-exr-with-baked-metadata-using-arri-reference-tool-art)<br>
[2. Replace .MXF with ART .EXRs Using Reconform](#2-replace-mxf-with-art-exrs-using-reconform)<br>
[3. Technical Grade and Export all Plates](#3-technical-grade-and-export-all-plates)<br>
[4. Export your References Plates](#4-export-your-references-plates)


## Preliminary Information

- This workflow follows the **Color Pipeline** developed by Gwénégan and Garrett. You can find the full documentation here : 
- The first step in the workflow uses the **ARRI Reference Tool (ART)** to bake the camera into each frame. Tristan created a script to bridge DaVinci Resolve to this tool. Documentation is available here : 
- For artists. Our color workflow is based on the **ARRI Reveal Color Science**. This means that in every DCC software used for **texturing, shading, or rendering,** you must ensure your display LUT is set to : **“Live Arri Reveal - Rec.1886 Rec.709 - Display”**. 
  
  - In Nuke : <img src="assets/images/nukeArriRevealLUT_001.png">
  - In Houdini : <img src="assets/images/houdiniArriRevealLUT_001.png">
  - In Substance :
  - In Mari :

## 1. Convert MXF to EXR with Baked Metadata Using ARRI Reference Tool (ART)
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

## 2. Replace .MXF with ART .EXRs using reconform

In this step, we’ll replace the **original MXF files** in your DaVinci Resolve timeline with the **ART EXR sequences**.

### 2.1. Import the EXR images sequences

   In **DaVinci Resolve**, create a new **bin** and import all your **ART EXR images sequences** into it.

### 2.2. Duplicate your timeline

Before making any changes, **duplicate your edit timeline** to keep a safe backup of the original MXF version.
You can keep your original track and duplicate it. For example here I used the second track to rename the shots (on the “Name Clip”) to get the proper shot cuts.

<img src = "assets/images/DaVinciResolve_duplicateTrack.jpg" height=250>

### 2.3 Prepare for reconform

- In your timeline, select the clips you want to replace.
- Right-click to one of the selected and choose **“Conform Lock Enabled”**. <br>
This unlocks the conform settings for those clips and prepares them for reconform.

<img src="assets/images/daVinciConformLock_001.png" height=500>

### 2.4. Run the reconform process

- Right-click on the duplicate timeline
- Go to **Timeline → Reconform from Bins.**

<img src = "assets/images/daVinci_reconform_step001.png" width = 512>

- The **Conform from Bins** window will appear.

### 2.5 Configure the settings

- Select the bin that contains your **ART EXR shots**.
- Enable both **“Timecode”** and **“Source Timecode”**.

<img src="assets/images/daVinci_reconform_step002.png" width= 512>

<p>

This process will automatically locate the correct **ART EXR** sequences based on their source timecode and replace the corresponding **MXF files** in your timeline. <br>
Once the reconform is complete, your edit will now reference the baked **ART EXR plates** instead of the MXF sources. Be sure to verify that every conformed shot is well placed → you can use the “Disable Video Track” button to compare with the MXF.
</p>
<img src = "assets/images/daVinci_reconform_timeline.jpg">

<p>

When your new **EXR files** have successfully replaced the MXF sources in your timeline, you can **archive the original MXF files** stored on the **Data PFE** (on your external HDD) and then **delete them from the Data PFE** to free up storage space. Every VFX Project has 10 To storage.

### ⚠️ Make sure all necessary backups or archive versions are saved before deleting the MXFs if you do this.

## 3. Technical Grade and Export all Plates
The third step is to perform a **technical grade** on all your shots, following the [**VFX Color Pipeline**](../color_workflow/COLOR_WORKFLOW.md).

### 3.1 Set Up the Color Pipeline - Version 1 (TECH GRADE Rec709 LUT)
   
- Go to the Color Tab.
- Add a **Color Space Transform** node that converts from **AP0 (Linear)** to **AWG4 (LogC4)** →  This ensures your **Timeline Working Space** is in **AWG4–LogC4**.

  <img src = "assets/images/daVinci_reconform_step003.png" height = 350>

- Add another **serial node** and apply the **ARRI Reveal LUT**: `ARRI_LogC4-to-Gamma24_Rec709-D65_v1_65`

  <img src = "assets/images/daVinciResolve_lutSetup.jpg" height = 350>

This converts your output back to the correct Rec.709 ARRI Reveal color space. Your **technical grade** should be done **between these two nodes**. This is where you neutralize all of your footage.

<figure>
  <img src = "assets/images/daVinci_reconform_step004.png">
  <figcaption> 
  
  This setup is referred to as **Version 1 – Before Comp Setup** *(see the Color Pipeline documentation)*. 
  </figcaption>
</figure>

### 3.2. Set Up the Color Pipeline - Version 2 (EXR AP0 EXPORT)
   
Once your **technical grade** is complete, proceed to the **export stage**. At this point, create a **new grade version** of your clip

<img src = "assets/images/daVinci_gradeVersionCreate.jpg" height = 512>

This **Version 2** will be the configuration used **before exporting your VFX plates**. Replace the **ARRI Reveal LUT** with a Color Space Transform going from **LogC4 → AP0**. 

<figure>
  <img src = "assets/images/daVinci_reconform_step005.png">
  <figcaption>

  This setup is referred to as **Version 2 – Before Comp Setup** *(see the Color Pipeline documentation)*.
  </figcaption>
</figure>

If you want to switch between versions in the color tab you can use the shortcut Ctrl+B.

### 3.3 Prepare Metadata and Naming Before Export
<p>

Before exporting, you’ll use **DaVinci Resolve’s Variables** to generate the correct file naming convention automatically.
</p>

- Go to the **Metadata** panel and verify all information for each VFX shot.
- Ensure the following metadata fields are correctly filled out:
  
  - `Name`: This will act as the **SHOT name**. We use the Name field instead of the Shot metadata field because the same clip may be used in multiple shots. This workaround ensures proper **Data Burn-in generation** and will also help us keep accurate track of shot usage later.
  - `Scene`: This corresponds to the Sequence.
  - `Comments`: This field represents the plate version.
    Use the following logic: If the shot contains only one plate → set the value to *plate0001*. <br>
    If multiple plates are used for the same shot → assign one version per plate (*plate0001, plate 0
    002, plate0003*, etc...).
  - `ISO`: This field is critical for the denoising step. Verify that all plates have the correct ISO information.

<figure>
  <img src = "assets/images/daVinci_metadata001.jpg" height = 490>
  <img src = "assets/images/daVinci_metadata002.jpg" height = 490>
  <img src = "assets/images/daVinci_metadata003.jpg" height = 490>
</figure>

### 3.4. Export Settings

- Go to the Delivery tab.
- Use the Custom preset : `plate_ap0_preset`

  <img src = "assets/images/daVinci_plateAP0_out.jpg" width = 256>

- You need to fill manually the **Location** tab since the preset doesn’t save it. To get the proper path and avoid manual Identifier creation inside of prism, here is the path:

> `V:\L5_2026\[PJC]\03_Production\Shots\sq%SCENE%\sh%CLIPNAME%\Renders\2dRender\%COMMENTS%_ap0\v0001` <br>
> [PJC] = The 3 letters of your project. <br>
> %SCENE% (our sequence), %CLIPNAME% (our Shot number) and %COMMENTS% (our plate number) are the Resolve’s variables which use the metadata of your clip.

<img src = "assets/images/daVinci_plateAP0_path.jpg">

Once this is set, the preset will automatically export all shots into the correct **Prism directory**.

<img src = "assets/images/prismPlateOutput.jpg" height = 400>

### 3.5. Preset Configuration Summary

The preset includes the following settings :

- **Automatic file naming** using Resolve variables: `sqXXXX_shXXXX_plate000X_ap0_v0001.####.exr`.
- **Individual clips**.
- **EXR RGB Half** with PIZ compression.
- **Render at source resolution**.
- **Disable sizing and blanking output** (no transform or blanking)
- **12-frame handles** (24 total).
- **4-digit frame numbers**.
- **Start frame** 989 *(frame 1001 is always the first frame of your shot in the edit)*.
### 3.6. Render
<p>

Once all your footage is properly set up with the correct metadata and export settings, you can **start rendering** your VFX plates. <br>
Don’t publish this on Shotgrid but put the plate status of the exported plates in “Complete". <br>
</p>

<img src="assets/images/plateAP0_sg.jpg">

## 4. Export your References Plates
<p>

In this step, you’ll export the **reference plate**s as *.mov* files for the artists. <br>
These files need to carry the **final format**, **final edit**, and **final color**, including any **transforms**, **retimes**, and **grading** baked in.
</p>

### 4.1 Prepare the Timeline

- Duplicate your edit timeline and rename it: `refExport`
- We will now export each shot as a **single .mov file** with:
  - Final frame cut
  - Final timing
  - Final color grade
  - All transforms and effects applied
  - Data burn-in
  - VFX description list

Below is the reference look you should obtain at this end of this process:

<img src="assets/images/refPlate_example.jpg">

> If the shot is a Computer Generated Image (CGI, 2D Animation, Motion-Design…), put a storyboard / 3D layout as a reference plate.

### 4.2. Apply the Data Burn-In

Use the Data Burn-In preset called `Reference_DataBrunInPreset`.

<img src="assets/images/refPlate_dataBurningPreset.jpg">
<p>

You may customize this preset (for example, adding your movie logo). <br>
Save it as a new custom preset for your project. <br>
</p>

> For example : `[PJC]_DataBurnIn`

For now, this is what you should have:

<img src="assets/images/refPlate_step001.jpg">

### 4.3. Add Blackbands & VFX Description Text

We must manually add the black bands and overlay the VFX description.

- **Create a new track** → name it *Crop Ratio*
- Add an **Adjustment Clip**
- Go to **Inspector → Video → Cropping**
- Apply the correct Crop Ratio values : <br>
For the Scope format :

  - Height: 1080
  - Scope height: 858
  - Crop value = `(1080 - 858) / 2 = 111`
  
<img src="assets/images/refPlate_step002.jpg" width=700>

- Add a **Text node for each shot**, place it in the **bottom-left corner**, and write your VFX task list.
  
<img src="assets/images/refPlate_step003.jpg" width=700>

### 4.4 Export the Single Reference Plate (.mov)

Go to the Delivery tab and select the preset :
<img src ="assets/images/refPlate_singlePreset.jpg">

<p>

  **Location Setup**:

      [YourEditingFolder]/
        referencePlate/
          singleReferenceClip/
              v0001/
</p>

### ⚠️ Important
<p>

Make sure your timeline has **all your color grading active**, with the **ARRI Reveal LUT enabled**, so the final look is properly baked into the MOV.<br>

Render your single reference plate.

<img src="assets/images/singleRefPlate_output.jpg" height=128>

### 4.5 Generate Individual Reference Clips

Once the `singleReferencePlate.mov` is rendered: <br>

- In DaVinci Resolve, create a new bin: `ReferenceCut → SingleReferenceClip`
- Import the `.mov` file you just exported.
- Duplicate your timeline again → rename it: *refCutExport*
- Create a new track called **CUT**, place your `.mov` on this track, and disable Data Burn-In if needed.

<img src="assets/images/refPlate_step004.jpg" width=700>

Your imported MOV should perfectly match the original edit. <br>

### 4.6 HD Timeline Resolution

We want the Final reference export to be delivered in **1920 x 1080 HD.** <br>

To update the timeline resolution : <br>

- Right-click on your timeline *refCutExport*
- Go to **Timeline → Timeline Settings**
- Set the resolution to **1920 x 1080 HD**
- Under Mismatched Resolution, select: "**Scale entire image to fit**"
- Click **OK** to apply

<img src="assets/images/refPlate_cut.jpg" width=700>

### 4.7 Perform Scene Cut Detection

- Lock all tracks **except the CUT track**.

<img src="assets/images/daVinci_singleRefLock.jpg" width=700>

- Then in **Timeline → Detect Scene Cuts**

<img src="assets/images/daVinci_detectSceneCuts.jpg" width=300>

DaVinci Resolve will automatically slice the single reference plate into individuals shots.

<img src="assets/images/daVinci_refPlate_autoCut.jpg" width=700>

Review all cuts to make sure they are correct. When approved, delete all the other tracks.

<img src="assets/images/daVinci_singelRefCuttedFinal.jpg" width=700>

### 4.8 Export individual References Plates

In the delivery page, use the preset `individualReference_preset` 
<img src="assets/images/indivRefPlate_preset.jpg">

**Location setup :**

In your project directory : <br>

    06_Edit/
      referencePlate/
        individualReferenceClip/
          v0001/

<img src="assets/images/daVinci_indivRefPlateOutFolder.jpg" width=700>

Render all the individual reference shots.

<img src="assets/images/daVinci_indivRefPlateOutFiles.jpg" width=700>

### 4.9 Ingest the Reference Plates in Prism

For each shot :

- Open the **Media** tab of the shot in Prism
- In the **Identifiers** tab → Right-click → **Ingest Media**

<img src="assets/images/prism_ingestMediaClick.jpg" width=700>

- Assign the identifier according to naming conventions : `ref_plate_rec709`
- Drag and drop the correct `.mov` into the **Media Path**.

<img src="assets/images/prism_ingestRefPlate.jpg">

Then click **Create**.

<img src="assets/images/prism_refPlateIngestOut.jpg" width=700>

The plate is now correctly ingested in the Prism pipeline and can be **published to ShotGrid**, inside the **Department → ref**. <br>

You may delete all the **individual reference .mov files** to save storage.
**Do NOT delete the v0001 folder** - it must remain as a record, in case a `v0002` is ever needed.

<img src="assets/images/refPlate_delFiles.jpg" width=700>