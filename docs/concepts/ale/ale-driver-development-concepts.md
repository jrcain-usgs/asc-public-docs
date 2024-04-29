# ALE Driver Architecture

## Drivers

There are many similarities between image-gathering spacecraft. 
For example, their output will generally include an image and a timestamp. 
Though not always attached, orientation data can often be found given that timestamp. 
Data should be able to be processed into a standard format.

But, each spacecraft also has it's particularities. 
One may gather image data in more bands than another.  The distortion of their lenses may differ. 
They may gather the same type of data, but store it in slightly different formats. 
To get data into a standard format, the unique properties of each spacecraft must be accounted for.

This is what ALE drivers are for.  If ALE has a driver for a spacecraft, 
it can return an ISD (Image Support Data) for images from that spacecraft.
(ALE may also need supplementary data along with the image, like labels or NAIF SPICE Kernels)

An ALE driver is a Python Class.  It uses Mixins to inherit shared functionality, 
and Methods to define per-spacecraft fuctionality.


## Mixins

- Inheritance structure in python
  - details/explanation
  - diagram
  - link to more info

- Why ALE uses mixins
  - Mixing common behaviors of spacecraft
    - Example from ALE


## Ale::load

- ? Not sure exactly what is intended for this section ?
- Load order of mixins
- `src/load.cpp`
- `ale/drivers/__init__.py > load`


## Overwriting Base Behavior

- Use ISIS ISDs as truth data, and ISIS classes as guidelines to instrument particularities

- Changing the order of Mixins
  - load mixin with desired behavior first (leftmost)
  - this overwrites behavior in further-right mixins

- When to make a new property in a driver
  - if spacecraft records data in a different way than others, not available in existing mixins