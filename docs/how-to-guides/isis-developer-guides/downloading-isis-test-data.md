## ISIS Test Data 
ISIS has of two types of tests: custom **Makefile** based tests, and **GTests**. The GTests use data from the ISIS3 repo along with the source, so no special data is required to run those, aside from the ISIS data area.

The Makefile tests depend on a separate source of data, including a few GB of input and expected output data used for testing ISIS applications. The Makefile tests use the `$ISISTESTDATA` environmental variable to locate that data, which [can be set the same way as the `$ISISDATA` variable](../../how-to-guides/environment-setup-and-maintenance/isis-data-area.md#setting-the-isisdata-environmental-variable).  The size of this test data decreases as we convert more Makefile tests to GTests.
 
### How to download the ISIS test data with rclone  
Test data is hosted in Amazon S3 storage buckets. We recommend using rclone to pull the data into a local directory. You can download rclone [from their website](https://rclone.org/downloads/) or by using a [conda environment](https://github.com/conda-forge/miniforge?tab=readme-ov-file#miniforge).  If you already have a conda environment, install rclone with

```sh
conda install –c conda-forge rclone
```

Once rclone is installed and `$ISISROOT` set, run: 

```sh
rclone --config $ISISROOT/etc/isis/rclone.conf sync asc_s3:asc-isisdata/isis_testData/ $ISISTESTDATA
```

where:

  - `$ISISTESTDATA` is the environment variable defining the location of the isis test data. 
  - `--config` overwrites the default rclone config with ISIS's own.
  - `asc_s3:` is the name of S3 configuration in the configuration file that ships with ISIS. This can be whatever you want to name it, in this case it is named remote. 
  - `asc-isisdata/isis_testData/` is the name of the S3 bucket you’re downloading from

Your $ISISTESTDATA directory should now contain a full clone of the ISIS test data for running Makefile based tests. 

!!! note "Downloading specific files"

    You can download specific files from the bucket by adding path data or file information to the first argument. For example, to download only the ‘base’ folder from the isis_testData bucket:

    ```sh
    rclone sync remote:asc-isisdata/isis_testData/base
    ```

!!! warning "rclone copy/sync can overwrite data"

    It's important to understand the difference between rclone’s `sync` and `copy` methods:

    -  `copy` will overwrite all data in the destination with data from source.
    - `sync` replaces only changed data.
    
    Syncing or copying in either direction (local -> remote or remote -> local) results in any changed data being overwritten without warning.
