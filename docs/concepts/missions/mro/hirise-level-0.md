# Level0 HiRISE

## Data Products: EDR and RDR

The HiRISE team produces two types of official 
[Planetary Data System (PDS)](http://pds.nasa.gov/) 
data products from the HiRISE instrument:

  - **Experiment Data Record** (EDR).  
    HiRISE EDRs contain the data collected by 1 channel (of 28) on the
    instrument and transmitted back to earth.  For one HiRISE observation, 
    there are up to 28 EDR files to download for processing.

  - **Reduced Data Record** (RDR).  
    HiRISE RDRs contain a radiometric
    calibrated, map projected, mosaic of multiple channels from a single
    observation. These products are stored in [JPEG 2000](http://en.wikipedia.org/wiki/JPEG_2000) format.

## Data Acquisition

**Search for HiRISE images** on [PDS Image Atlas III](http://pds-imaging.jpl.nasa.gov/search/mro/hirise)
or [PDS Image Atlas IV (Beta)](https://pds-imaging.jpl.nasa.gov/beta/search?gather.common.instrument=HIRISE).  
*Data products from other instruments and spacecraft are also availble here.* 

Some filters that may be helpful are:

- Instrument (HiRISE)
- Target (Mars)
- Product Type (EDR, if processing in ISIS)
- Filter
- Latitude/Longitude
- Orbit Number
- Mission Phase

Or, **browse for images manually** on the [HiRISE Online Data Volume (UofA)](https://hirise-pds.lpl.arizona.edu/PDS/).

-----

??? info "HiRISE File Naming Conventions"

    Understanding the filename can be helpful when searching for data and
    managing files. The PDS naming convention for channel EDRs is

        AAA_BBBBBB_CCCC_DDD_E.IMG

    Where:

    - **AAA**: mission phase
        - **AEB** Aerobraking
        - **TRA** Transition
        - **PSP** Primary Science Phase
        - **REL** Relay
        - **Exx** Extended missions (if needed)
    - **BBBBBB**: Orbit Number
    - **CCCC**: Target Code 
        -   The latitudinal
            position of the center of the planned observation relative to the
            start of the orbit. The first three digits are whole degrees, the
            fourth digit the fractional degrees rounded to 0.5 degrees. (example
            Mars target codes on a descending orbit: 0900 for 90.0° or
            south-pole, 1800 for 180.0° or equator, 2700 for 270.0° or
            north-pole)
    - **DDD**: Filter/CCD Identifier (RED0-RED9, IR10, IR11, BG12, BG13)
    - **E** - Channel Number (0 or 1)

    For example, *PSP_002733_1880_RED5_0.IMG* is an image centered at
    188.0° latitudinal degrees from the start of the orbit around Mars,
    collected by channel 0 of CCD 5, red filter, during orbit 2733 of the
    primary science phase. See the [Software Interface Specification for
    HiRISE Experiment Data Record
    Products](http://hirise.lpl.arizona.edu/pdf/HiRISE_EDR_RDR_Vol_SIS_2007_05_22.pdf)
    (PDF) for additional details.

    Similarly, the PDS naming convention for RDR mosaicked observation
    products is

        AAA_BBBBBB_CCCC.IMG

[![HiRISE_PDS_Data_Node_ScreenShot.jpg](attachments/thumbnail/1014/300.png)](attachments/download/1014/HiRISE_PDS_Data_Node_ScreenShot.jpg "HiRISE_PDS_Data_Node_ScreenShot.jpg")

    HiRISE PDS Data Node



#### Browsing by Volume

In each volume, data are organized into subdirectories by product type, mission
phase, and orbit. Currently, the [HiRISE PDS Data
Node](http://hirise-pds.lpl.arizona.edu/PDS/) holds the data archive.

When you know the images that you would like to work with, you can go to
this area with an FTP tool (or web browser) and download the images. The
data areas of the FTP site are briefly described below:

  - **RDR** or **EDR** - for Isis processing, you'll want to browse the
    EDR directory \*\* **PSP** , etc. is the mission phase - Primary
    Science Phase (PSP) data is most likely what you'll want to use.
    \*\*\* **ORB_[start orbit]_[end orbit]** refers the orbit
    range. These directories contain the data files.



#### Related Resources

  - [Planetary Image Atlas](http://pds-imaging.jpl.nasa.gov/Missions/)
  - [Planetary Image Atlas Help
    Pages](http://pds-imaging.jpl.nasa.gov/Atlas/intro.html)


### HiRISE Image Access Solutions Viewer

[![IAS_Viewer_OpenDialog.jpg](attachments/thumbnail/1018/300.png)](attachments/download/1018/IAS_Viewer_OpenDialog.jpg "IAS_Viewer_OpenDialog.jpg")

    HiRISE IAS Viewer, Open Remote File Image Selection Dialog

The HiRISE Image Access Solutions (IAS) Viewer can be useful for
browsing the archive of observation mosaics (RDR products) when
searching for data in your region of interest. The viewer can be
launched from the HiRISE JPEG 2000 Viewing Tools page.

From the *File* menu, choose *Open Remote File...* to open the image
selection dialog. An image selection dialog, like the one shown on the
right, will appear.

Type in the following address in the *Server* field

    hijpip.hinet.lpl.arizona.edu:8064

and click the *Connect to Server* button.

Once the application has connected to the server, a navigable folder
menu will appear on the left side of the window. Click the plus sign (+)
beside each folder to expand the folder tree:

    PDS
      RDR
        PSP

[![IAS_Viewer_shot.jpg](attachments/thumbnail/1019/300.png)](attachments/download/1019/IAS_Viewer_shot.jpg "IAS_Viewer_shot.jpg")

    HiRISE IAS Viewer

The PSP folder contains folders for each set of orbits, named
ORB_[start orbit]_[end orbit]. Expand the folder for the orbit
range you'd like, the select the folder for the image of choice. Click
the image file (the file ending with the file extension JP2) from the
right side of the screen, and click the Open button to view the image in
the main window.

Selecting an image will give you a display like that shown to the right,
and you will have many options for manipulating the image.

**Note: only the HiRISE RDRs can be opened and viewed with the IAS
viewer.**

The JPEG 2000 files used by this viewer are accessible via the JPIP
server *hijpip.hinet.lpl.arizona.edu:8064* . Additional tools and
information about JPEG 2000 can be found in the [Wikipedia
JPEG 2000](http://en.wikipedia.org/wiki/JPEG_2000) entry.

The Image Access Solutions Viewer is a product of ITT Visual Information
Solutions. (Any use of trade, product, or firm names in Isis web pages,
documents, or publications is for descriptive purposes only and does not
imply endorsement by the U.S. Government.)



## Ingestion

-----

[![HiRISE_Fullres_Sample.png](attachments/thumbnail/1020/300.png)](attachments/download/1020/HiRISE_Fullres_Sample.png "HiRISE_Fullres_Sample.png")

    HiRISE image First 4000 lines of an 80000 line EDR taken
    during orbit number 2733. The image has been compressed
    four times in the line and sample directions.

In order to work with HiRISE data in Isis, the HiRISE EDR file must be
converted to an *Isis cube file* so Isis programs can read and process
the data.

EDR files should always have a file extension of *IMG* . These files
contain the image data as well as text describing the image data and the
state of the instrument at the time the image was taken. The text is in
the form of a standard [[media:PDS Sample label.txt|PDS label (click
to view example label file)]] located at the beginning of the file.
Only the information needed by other Isis programs is transferred from
the PDS label to the Isis cube label.

The program used to convert HiRISE EDR files to Isis cube files is
hi2isis. The following example shows the command line usage. The
resulting output file will be an Isis cube.

Example: ingesting a HiRISE EDR product into Isis:

    hi2isis from=PSP_002733_1880_RED4_0.IMG to=PSP_002733_1880_RED4_0.cub

The
[hi2isis](http://isis.astrogeology.usgs.gov/Application/presentation/Tabbed/hi2isis/hi2isis.html)
program also converts the image header, prefix and suffix data to Isis
Binary Large OBject (BLOBs) and has other parameters.



## SPICE ( **S** pacecraft & **P** lanetary ephemeredes, **I** nstrument **C** -matrix and **E** vent kernels)

-----

For additional information: [General SPICE Information](SPICE)

The application
[spiceinit](http://isis.astrogeology.usgs.gov/Application/presentation/Tabbed/spiceinit/spiceinit.html)
will add the appropriate SPICE information to the ISIS image cube. All
HiRISE Channels image cubes require spiceinit. Generally, you can simply
run
[spiceinit](http://isis.astrogeology.usgs.gov/Application/presentation/Tabbed/spiceinit/spiceinit.html)
with your input filename and no other parameters:

Example:

    spiceinit from=PSP_002733_1880_RED4_0.cub



## Raw Camera Geometry

-----

Once spiceinit has been successfully applied to a raw ISIS image cube an
abundance of information can be computed and retrieved for geometry and
photometry.

  - [**Camera Geometry Overview and Applications**](Camera_Geometry)

[*Goto* Level1](Level1_HiRISE)



## Image Attachments

[Pia_quick.png](attachments/download/1011/Pia_quick.png)
[View](attachments/download/1011/Pia_quick.png "View")
<span class="size"> (242 KB) </span> <span class="author"> Ian Humphrey,
2016-05-31 05:23 PM </span>

[Pia_prod.png](attachments/download/1012/Pia_prod.png)
[View](attachments/download/1012/Pia_prod.png "View")
<span class="size"> (190 KB) </span> <span class="author"> Ian Humphrey,
2016-05-31 05:23 PM </span>

[Pia_inst.png](attachments/download/1013/Pia_inst.png)
[View](attachments/download/1013/Pia_inst.png "View")
<span class="size"> (320 KB) </span> <span class="author"> Ian Humphrey,
2016-05-31 05:24 PM </span>

[HiRISE_PDS_Data_Node_ScreenShot.jpg](attachments/download/1014/HiRISE_PDS_Data_Node_ScreenShot.jpg)
[View](attachments/download/1014/HiRISE_PDS_Data_Node_ScreenShot.jpg "View")
<span class="size"> (113 KB) </span> <span class="author"> Ian Humphrey,
2016-05-31 05:24 PM </span>

[HIRISE_Image_Viewer_shot1.jpg](attachments/download/1015/HIRISE_Image_Viewer_shot1.jpg)
[View](attachments/download/1015/HIRISE_Image_Viewer_shot1.jpg "View")
<span class="size"> (203 KB) </span> <span class="author"> Ian Humphrey,
2016-05-31 05:24 PM </span>

[HIRISE_Image_Viewer_shot2.jpg](attachments/download/1016/HIRISE_Image_Viewer_shot2.jpg)
[View](attachments/download/1016/HIRISE_Image_Viewer_shot2.jpg "View")
<span class="size"> (187 KB) </span> <span class="author"> Ian Humphrey,
2016-05-31 05:24 PM </span>

[HIRISE_Image_Viewer_shot3.jpg](attachments/download/1017/HIRISE_Image_Viewer_shot3.jpg)
[View](attachments/download/1017/HIRISE_Image_Viewer_shot3.jpg "View")
<span class="size"> (147 KB) </span> <span class="author"> Ian Humphrey,
2016-05-31 05:24 PM </span>

[IAS_Viewer_OpenDialog.jpg](attachments/download/1018/IAS_Viewer_OpenDialog.jpg)
[View](attachments/download/1018/IAS_Viewer_OpenDialog.jpg "View")
<span class="size"> (111 KB) </span> <span class="author"> Ian Humphrey,
2016-05-31 05:25 PM </span>

[IAS_Viewer_shot.jpg](attachments/download/1019/IAS_Viewer_shot.jpg)
[View](attachments/download/1019/IAS_Viewer_shot.jpg "View")
<span class="size"> (154 KB) </span> <span class="author"> Ian Humphrey,
2016-05-31 05:25 PM </span>

[HiRISE_Fullres_Sample.png](attachments/download/1020/HiRISE_Fullres_Sample.png)
[View](attachments/download/1020/HiRISE_Fullres_Sample.png "View")
<span class="size"> (3.92 MB) </span> <span class="author"> Ian
Humphrey, 2016-05-31 05:25 PM </span>
