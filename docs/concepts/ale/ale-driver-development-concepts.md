# ALE Driver Development Concepts

## Drivers

- While there are similarities between spacecraft, each instrument has particularities in:
  - what data it collects/records
  - how that data is shown

- Drivers allow data from unique and differing spacecraft to be processed into a standard format.


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