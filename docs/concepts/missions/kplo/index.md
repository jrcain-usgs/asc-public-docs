# Korea Pathfinder Lunar Orbiter (KPLO, aka Danuri)

The KPLO is a spacecraft that began orbiting the moon in December 2022.  It is operated by the Korea Aerospace Research Institute (KARI).  It carries five KARI-Developed instruments (LUTI, PolCam, KMAG, KGRS, DTNPL) as well as NASA's ShadowCam.

## ShadowCam

ShadowCam was developed by ASU, MSSS, IM, and NASA, based on the LROC NAC but more sensitive.  Its purpose is to capture images of permanently-shadowed regions (PSRs) on the moon, and provide information about water ice.

[About ShadowCam - Intuitive Machines](https://shadowcam.im-ldi.com/about)

### ShadowCam Data Sources

TODO: List ShadowCam Data Sources

### Processing ShadowCam Data in ISIS.

In ISIS, Raw EDR ShadowCam images can be imported and calibrated as so:

```sh
shadowcam2isis from=M091671205SE.cub to=M091671205SE.isis.cub

shadowcamcal from=M091671205SE.isis.cub to=M091671205SE.cal.cub
```

Note that the ShadowCam Team also provides calibrated ISIS cubes as part of its data releases, which are ready-to-go and do not need to be imported or calibrated.  If run on a precalibrated ShadowCam cube, the import and calibration ISIS apps will notify that that their processing is not needed.