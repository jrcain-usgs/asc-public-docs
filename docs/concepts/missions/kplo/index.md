# Korea Pathfinder Lunar Orbiter (KPLO, aka Danuri)

The KPLO is a spacecraft that began orbiting the moon in December 2022.  It is operated by the Korea Aerospace Research Institute (KARI).  It carries five KARI-Developed instruments (LUTI, PolCam, KMAG, KGRS, DTNPL) as well as NASA's ShadowCam.

## ShadowCam

ShadowCam was developed by ASU, MSSS, IM, and NASA, based on the LROC NAC but more sensitive.  Its purpose is to capture images of permanently-shadowed regions (PSRs) on the moon, and provide information about water ice.

[About ShadowCam - Intuitive Machines](https://shadowcam.im-ldi.com/about)

## ShadowCam Data Sources

TODO: List ShadowCam Data Sources

## Processing ShadowCam Data in ISIS.

### Import & Calibration

In ISIS, Raw EDR ShadowCam images can be imported and calibrated as so:

```sh
shadowcam2isis from=M091671205SE.cub to=M091671205SE.isis.cub

shadowcamcal from=M091671205SE.isis.cub to=M091671205SE.cal.cub
```

Note that the ShadowCam Team also provides calibrated ISIS cubes as part of its data releases, which are ready-to-go and do not need to be imported or calibrated.  If run on a precalibrated ShadowCam cube, the import and calibration ISIS apps will notify that that their processing is not needed.

### Attaching SPICE

After import and calibration, attach spice information with:

```sh
spiceinit from=M091671205SE.cal.cub
```

To initialize with a specific DEM, run `spiceinit` with `shape=user` instead, specifying your DEM with the `model=` option:

```sh
spiceinit from=M091671205SE.cal.cub shape=user model=$ISISDATA/base/dems/LRO_LOLA_LDEM_global_128ppd_20100915.cub
```

### Calculating Photometric Angles


`phocube` can calculate incedence and emission angles for an image.  Use the LOCAL version of each if using your own DEM.  It creates a band (an array with a measurement at each pixel) for each specified item.  In this example, it makes a band for incedence angles, and a band for emission angles:

```sh
phocube from=M091671205SE.cal.cub to=M091671205SE.angles.cub localemission=true localincidence=true phase=false
```
