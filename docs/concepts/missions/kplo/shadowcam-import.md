# Using ShadowCam Data in ISIS

## Import & Calibration

In ISIS, Raw EDR ShadowCam images can be imported and calibrated as so:

```sh
shadowcam2isis from=edr/M076039010SE.cub to=M076039010SE.2isis.cub
shadowcamcal from=M076039010SE.2isis.cub to=M076039010SE.cub
```

Note that the ShadowCam Team also provides calibrated ISIS cubes as part of its data releases, which are ready-to-go and do not need to be imported or calibrated.  If run on a precalibrated ShadowCam cube, the import and calibration ISIS apps will notify that that their processing is not needed.

## Attaching SPICE

After import and calibration, attach spice information with:

```sh
spiceinit from=M076039010SE.cub
```

To initialize with a specific DEM, run `spiceinit` with `shape=user` instead, specifying your DEM with the `model=` option:

```sh
spiceinit from=M076039010SE.cub shape=user model=$ISISDATA/base/dems/LRO_LOLA_LDEM_global_128ppd_20100915.cub
```

*The LOLA Global DEM is included in the ISIS Data Area. However, for higher resolution data a local DEM/DTM is recommended.  See [ShadowCam - Calculating Photometric Angles](shadowcam-photometric-angles.md) for more details.*