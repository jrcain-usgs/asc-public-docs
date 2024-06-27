Status:

  No. | Status                           | Notebooks     |                   |                  |                           | notes
 -----|----------------------------------|---------------|-------------------|------------------|---------------------------|-------------------|
  16  | Total                            |
   4  | Working                          | bundle-adjust | sensor-utils      | usgscsm-isis-cmp | reduced-normal-equations* | (* with warnings) |
   4  | File not Located                 | data-snooping | footprint-control | lm-bundle-adjust |                           |                   |
   3  | Ale; no driver.to_dict           | cassini-iss   | lrolroc-nac^      | kaguya-tc^       |                           | (^ listed twice)  |
   1  | Ale; no find_child_frame         | mdis-isis-csm-cmp                 |
   2  | knoten.create_csm returns None?  | mdis-dem      | mdis-stereo       |
   1  | Math Error                       | bundle-adjust-distributed         |
   3  | Not Examined in Depth            | kaguya-tc     | lrolroc-nac       | footprint-comparison |


# bundle-adjust-distributed.ipynb
- result = f.result()
  - LinAlgError: Singular matrix

# bundle-adjust.ipynb
- Working!

# cassini-iss.ipynb
- Had to download and edit spice data for this
- 'CassiniIssPds3LabelNaifSpiceDriver' object has no attribute 'to_dict'

# data-snooping.ipynb
- On Hold: File not found: control_network_metrics/registration_quality/cubes.lis

# footprint-comparison.ipynb
- Download File: data/J03_045994_1986_XN_18N282W_sorted.cub
- Need 2 ISDs for comparison
  - example is from 2 different versions of ALE
  - Produce these and store in repo?

# footprint-control.ipynb
- On Hold: Files not found: europa/11ESMORPHY01_Island9... _add.net, .net, .lis

# kaguya-tc.ipynb
- Compressed .sl2 files with spc and ctg files? 
- Download File: images/TC2W2B0_01_00366S490E1640.img

# lm-bundle-adjust.ipyn
- On Hold: File not found: /scratch/csm2020/data/cubes2.lis

# lrolroc-nac.ipynb
- Failed to find metakernels.  (need to download naif spice data)

# mdis-dem.ipynb
- AttributeError: 'NoneType' object has no attribute 'getImageSize'
  - knoten.csm.create_csm returns `None`?

# mdis-isis-csm-cmp.ipynb
- driver.frame_chain has no attribute 'find_child_frame'

# mdis-stereo.ipynb
- AttributeError: 'NoneType' object has no attribute 'getImageSize'
  - knoten.csm.create_csm returns `None`?

# reduced-normal-equations.ipynb
- Working with warning:
  - FutureWarning: Series.__getitem__ treating keys as positions is deprecated.
    In a future version, integer keys will always be treated as labels (consistent with DataFrame behavior).
    To access a value by position, use `ser.iloc[pos]`

# reduced-normal-with-cholesky.ipynb
- On Hold: File not found: /scratch/csm2020/test_bundles/cubes_smithed_ground.lis
- requirement: from sksparse.cholmod import cholesky

# sensor-utils.ipynb
- Working!

# usgscsm-isis-cmp.ipynb
- Working!