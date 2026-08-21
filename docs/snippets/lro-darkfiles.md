??? note "LRO Dark Files needed for Calibration"

    If you haven't yet set up ISISDATA, pick a path to install data to:
    ```sh
    export ISISDATA=/my/isisdata/folder
    ```

    This command downloads the LRO calibration files, including the Dark-Files needed for calibration (~5GB), without downloading the whole LRO kernel set, which is quite large.

    ```sh
    downloadIsisData lro $ISISDATA --no-kernels
    ```