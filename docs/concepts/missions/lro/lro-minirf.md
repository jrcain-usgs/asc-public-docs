
# Working with Lunar Reconnaissance Orbiter MiniRF Data

# **ISIS3** Processing of MiniRF S-zoom data

## S-Zoom (LSZ)

### Data Acquisition

### Data Ingestion

Note:  
* Both the .LBL and .IMG must be in the same directory together  
* The LBL and IMG extensions must be in upper or lower case consistent
with each other (.LBL/.IMG or .lbl/.img).

Example command:

    mrf2isis from=LSZ_04485_1CD_XKU_87S205_V1.LBL to=LSZ_04485_1CD_XKU_87S205_V1_lev1.cub

The application
[spiceinit](http://isis.astrogeology.usgs.gov/Application/presentation/Tabbed/spiceinit/spiceinit.html)
will add the appropriate SPICE information to the ISIS3 image cube.
Successful spiceinit allows for further geometric processing of the
data.

### spiceinit (Load NAIF SPICE kernels)

[SPICE (Spacecraft & Planetary ephemeredes, Instrument C-matrix and
Event kernels)](SPICE)

Note:  
For the moon, ISIS3 currently defaults to the global LOLA for the local
topographic shapemodel.

Example command:  
Specify the "Smithed" SPK kernels for LRO (not a default in spiceinit)

    spiceinit from=LSZ_04485_1CD_XKU_87S205_V1_lev1.cub spksmithed=true

## Generate Stoke Parameter Images

### S1

    fx f1=LSZ_04485_1CD_XKU_87S205_V1_lev1.cub+1  f2=LSZ_04485_1CD_XKU_87S205_V1_lev1.cub+2  
       equation="f1 + f2" 
       to=LSZ_04485_1CD_XKU_87S205_V1_S1.cub


### S2

    fx f1=LSZ_04485_1CD_XKU_87S205_V1_lev1.cub+1  f2=LSZ_04485_1CD_XKU_87S205_V1_lev1.cub+2  
       equation="f1 - f2" 
       to=LSZ_04485_1CD_XKU_87S205_V1_S2.cub


### S3

    fx f1=LSZ_04485_1CD_XKU_87S205_V1_lev1.cub+3 
       equation="f1 * 2.0" 
       to=LSZ_04485_1CD_XKU_87S205_V1_S3.cub


### S4

    fx f1=LSZ_04485_1CD_XKU_87S205_V1_lev1.cub+4 
       equation="f1 * -2.0" 
       to=LSZ_04485_1CD_XKU_87S205_V1_S4.cub


## Replace invalid pixels along the vertical edge beyond the lunar data for the S1 file

The output pixels will be replaced with NULL  
Example Command:

    mask from=LSZ_04485_1CD_XKU_87S205_V1_S1.cub to=LSZ_04485_1CD_XKU_87S205_V1_S1_NoZ.cub 
         min=-20.0 max=0.0 preserve=outside spixels=NULL

## Stack the masked S1_NoZ output with S2, S3 and S4

Example Commands:

    ls LSZ_04485_1CD_XKU_87S205_V1_S1_NoZ.cub > stack.lis
    ls LSZ_04485_1CD_XKU_87S205_V1_S2.cub >> stack.lis
    ls LSZ_04485_1CD_XKU_87S205_V1_S3.cub >> stack.lis
    ls LSZ_04485_1CD_XKU_87S205_V1_S4.cub >> stack.lis
    
    
    cubeit fromlist=stack.lis to=LSZ_04485_1CD_XKU_87S205_V1_STACK.cub


## Replace invalid pixels for stacked S2, S3 and S4

The NULL values in the S1\_NoZ will applied to the remaining bands

    bandtrim from=LSZ_04485_1CD_XKU_87S205_V1_STACK.cub to=LSZ_04485_1CD_XKU_87S205_V1_STACK-TRIM.cub

## Reduce the 4-band stacked, cleaned cube to approximately 15 meters/pixel (from the original 30 meters/pixel)

Reduce will average the input pixels which results in an improved output
instead of specifying the reduced resolution within a map template for
cam2map

    reduce from=LSZ_04485_1CD_XKU_87S205_V1_STACK-TRIM.cub to=LSZ_04485_1CD_XKU_87S205_V1_STACK-TRIM_15m.cub 
           lscale=2 sscale=2

## Create a maptemplate to map project the stacked and reduced file

Refer to:
[Learning\_About\_Map\_Projections](Learning_About_Map_Projections)

Example map template for MiniRF (to apply on a polar image in this
example):

Group = Mapping  
ProjectionName = POLARSTEREOGRAPHIC  
CenterLongitude = 0.0  
TargetName = MOON  
EquatorialRadius = 1737400.0  
PolarRadius = 1737400.0  
LatitudeType = Planetocentric  
LongitudeDirection = PositiveEast  
LongitudeDomain = 360  
PixelResolution = 14.806323449292  
Scale = 2048.0  
CenterLatitude = -90.0  
End\_Group

## Map project the stacked and reduced file

For a mosaic, remember to make sure all images are projected to the same
projection, clat, clon and map scale/resolution.  
For resolution, specify the scale in meters/pixel in cam2map.

Example cam2map command:

    cam2map from=LSZ_04485_1CD_XKU_87S205_V1_STACK-TRIM_15m.cub to=LSZ_04485_1CD_XKU_87S205_V1_STACK-TRIM_15m_map.cub 
            pixres=camera resolution=2048 warpalgorithm=forwardpatch patchsize=5 map=predefined.map

To create a mosaic, refer to: **Power Tip: Making Mosaics**
[Learning\_About\_Map\_Projections](Learning_About_Map_Projections)

### CPR

Create an 8-bit CPR cube from the Stokes-1 and Stokes-4 bands of the
stacked cube or the multi-band mosaic:

    fx f1=LSZ_04485_1CD_XKU_87S205_V1_STACK-TRIM_15m_map.cub+1 
       f2=LSZ_04485_1CD_XKU_87S205_V1_STACK-TRIM_15m_map.cub+4 
       to=LSZ_04485_1CD_XKU_87S205_V1_STACK-TRIM_15m_map_CPR.cub+UnsignedByte+0.0:2.53 
       equation="(f1-f2)/f1+f2)" mode=cubes