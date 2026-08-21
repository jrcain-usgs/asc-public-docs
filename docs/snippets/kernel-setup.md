??? note "Setting up Kernels for Online or Local Use"

    ### Using Online Kernels

    The IsisPreferences file is located at `$ISISROOT/IsisPreferences`. 
    Set `UseSpiceQL = "true"` in the `SpiceQL` group:

    ```sh title="Open IsisPreferences"
    nano $ISISROOT/IsisPreferences
    ```

    ```json title="SpiceQL Settings"
    Group = SpiceQL
      UseSpiceQL = "true"
    EndGroup
    ```

    ### Using Local Kernels

    Set an ISISDATA location (if you haven't already):

    ```sh
    export ISISDATA=/my/isisdata/folder
    ``` 

    and use the ISIS short-name for your mission 
    (see [Mission Names & Download Commands](https://astrogeology.usgs.gov/docs/how-to-guides/environment-setup-and-maintenance/isis-data-area/#mission-specific-data-areas)) 
    to download its kernels:

    ```sh
    downloadIsisData <mission> $ISISDATA
    ```

    WARNING: Some missions have large kernelsets - hundreds of GB!