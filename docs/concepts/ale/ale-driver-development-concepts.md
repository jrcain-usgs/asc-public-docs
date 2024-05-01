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

Mixins allow code from different classes to be reused and mixed together in one class (or ALE driver, in our case). 
Mixins can be thought of like multiple inheritance, though one mixin is not usually the single parent/super of another class. 
They can also be thought of similar to an *include* at the top of the class. 

A class that uses a mixin is not typically a specialized version of that mixin. 
Rather, the mixin is one component among many in the class that is using it. 

??? example "Python Mixins Example"

    `class_c` may use methods from `mixin_A` and `mixin_B`, as well as its own methods.  Methods which overwrite others print `!` here.
    ```python
    class mixin_A():
        def method_1(self):
            print("mixin_A - method_1")
            
        def method_3(self):
            print("mixin_A - method_3 !")
            # super().method_3()   # would print "  mixin_B - method_3" if called from class_C

    class mixin_B():
        def method_2(self):
            print("mixin_B - method_2")
        
        def method_3(self):
            print("mixin_B - method_3")

        def method_5(self):
            print("mixin_B - method_5")

    class class_C(mixin_A, mixin_B):
        def method_4(self):
            print("class_C - method_4")

        def method_5(self):
            print("class_C - method_5 !")
            # super().method_5()   # would print "  mixin_B - method_5"
            
    driver = class_C()
    driver.method_1()
    driver.method_2()
    driver.method_3()
    driver.method_4()
    driver.method_5()
    ```

    This example will print:
    ```
    mixin_A - method1
    mixin_B - method2
    mixin_A - method3 !
    class_C - method4
    class_C - method5 !
    ```

    ## Method Resolution Order
    In the case of two methods with the same name, 
    the method from the first leftmost mixin will be used before methods from later mixins, 
    and the methods in the class overwrite methods from mixins.  
    `super()` can be used to access methods from parent classes or further-right mixins.

Mixins let ALE drivers mix and match common spacecraft/image data features.
For example, there are mixins that correspond to different camera types: 
whatever the other mixins and methods in a driver, 
any methods that correspond to a certain type of camera
can be included simply by adding a mixin like `Framer`.

More info can be found on ALE Driver mixins in 
[Creating ALE Drivers - Class Signature](../../how-to-guides/ale-developer-guides/creating-ale-drivers/#class-signature).


## ale.driver.load()

- `load()` returns a dictionary.
- `loads()` is a wrapper for `load()` that returns a string.

The `load()` function in ALE is for creating an ISD from a label. 
It runs through every class containing `_driver` in the name,
until one works and successfully creates an ISD (or until all fail). 
`load()` initially checks for an instrument_id and fails the driver if that does not succeed. 

Because of the sequential trial-and-error loading method, in some cases
a different driver may succeed before the desired driver is reached.


## Overwriting Base Behavior

The base behavior of a will sometimes need to be overwritten in favor of a more specific behavior for your driver.

!!! note "Use ISIS for Reference!"
    
    ISIS is your guide for creating drivers in ALE.

    ALE's ISDs should match closely with with your spacecraft's ISDs from ISIS.
    A **mismatch** between the ALE and ISIS ISDs (aside from cases of insignificant digits) 
    is an indication the ALE driver needs to be changed. The mismatched field may give an 
    indication of what method needs implementation or changing.

    A look at the related classes in ISIS may help you figure out how to implement the ALE Driver.

Methods in the driver itself will take precedence over methods in the mixin classes. 
Every driver will need some of its own methods, some drivers more than others. 
See [Creating ALE Drivers - Creating Methods to Customize the Driver](../../how-to-guides/ale-developer-guides/creating-ale-drivers/#creating-methods-to-customize-the-driver). 
Any time a spacecraft records data in a different way than others, it will need a method in its driver.

### Mixin Order

Occasionally, though the target behavior for a driver is already implemented in ALE, 
the Method Resolution Order may mean it gets overwritten. 
Methods from mixins on the left overwrite (and are children of) 
methods with the same name from mixins on the right.

Changing the order of of the mixins in the class signature 
can bring to the surface a different version of a method needed 
by the driver. **But be careful!** Changing the mixin order can 
also obscure other methods the driver needs.

### Editing Mixin Classes

Adding or changing a method in a mixin class will propogate that method to any drivers that use the mixin.
Take caution when editing a mixin class, as changes can affect many drivers.