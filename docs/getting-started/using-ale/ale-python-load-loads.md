# ALE in Python - `load` and `loads`

## Prerequisites




## `load` and `loads`

The `ale.drivers.load` and `ale.drivers.loads` functions are
the main interface for generating ISDs. Simply pass them the path to your image
file/label and they will attempt to generate an ISD for it.

```py
import ale

image_label_path = "/path/to/my/image.lbl"
isd_string = ale.loads(image_label_path)
```
  
