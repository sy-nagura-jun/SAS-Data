# SAS-Data
This is the repository found at [Wireless Innovation Forum](https://github.com/Wireless-Innovation-Forum/SAS-Data). It holds all data related to the [Wireless Innovation Forum / Spectrum Access System](https://github.com/Wireless-Innovation-Forum/Spectrum-Access-System) GitHub repository,
which are stored in Git LFS (Large File storage).

## WARNING

The full data is about 50GB in size. Because of quota limitation, please only clone when necessary. 
If cloning fails, it can be that the transfer quota has been reached for the month. In that case, please contact:
  https://github.com/LeePucker

## Data integration in SAS environment

### Cloning the repository
Clone this repository using GIT.

```
    git clone https://github.com/Wireless-Innovation-Forum/SAS-Data.git
```

In case you do not have GIT LFS already installed and enabled, these files are not real
zip files yet, but simple text trackers to the LFS (Large File Storage) storage.


### Retrieving the raw files from LFS

To actually get the real data stored in LFS, you need to perform:

```
    # Install git lfs (if not already done)
    git lfs install
    
```

Then pull the data (ie fetch and checkout):

```
    git lfs pull
```

### Extracting the zip files

To actually unzip the tiles, you can use the provided script 'extract_geo.py':

```
    python extract_geo.py
```

This will extract all the zip files in place.

### Plugging the data to the SAS code

One way is to specify the location of your NED and NLCD data location in the file
`src/harness/reference_models/geo/CONFIG.py` of the main Spectrum-Access-Systems repository.
(See: https://github.com/Wireless-Innovation-Forum/Spectrum-Access-System/blob/master/src/harness/reference_models/geo/CONFIG.py).

Another possibility is to move the extracted files into a folder `data/geo/ned/` and `data/geo/nlcd`
of the main repository (default target of the CONFIG.py file).


## NED Terrain data

The `ned/` folder contains the USGS National Elevation Data (NED) in 1x1 degrees tiles with grid every 1 arcsecond.

Content for each tile:

  - floatnXXwYYY_std.[prj,hdr] : geo referencing header files
  - floatnXXwYYY_std.flt : raw data in ArcFloat format.

An updated version of some of the tiles are provided with the prefix usgs_ned_1_nXXwYYY_gridfloat_std: 
these data correspond to newer data gathering techniques (primarily LIDAR).

This reference data corresponds to a snapshot of the latest USGS data available from July 2017.

The data is read by the NED terrain driver found in harness/src/reference_models/geo/terrain.py, 
and in particular is used by the reference propagation models in harness/src/reference_models/propagation.

Header files are provided for enabling the data to be displayed on any GIS tool.

## NLCD Land Cover data

The `nlcd/` folder contains the data for the USGS NLCD (National Land Cover Data), retiled in 1x1 degrees tile with grid every 1 arcsecond.

Content for each tile:

  - nlcd_nXXwYYY_ref.[prj,hdr] : geo referencing header files
  - nlcd_nXXwYYY_ref.int : raw data

This reference data corresponds to a retiling operation of the original NLCD data snapshot of 2014, 
using the set of following (provided) scripts:

 - `src/data/retrieve_orig_nlcd.py`: retrieve the original 2014 NLCD data
 - `src/data/retile_nlcd.py`: retile the data into 1x1 degrees tiles with grid point at multiple of 1 arcsecond.

The retiled data can be read by the NLCD driver found in harness/src/reference_models/geo/nlcd.py.

They can be displayed in any GIS tool, thanks to header files provided for georeferencing.

Note: because SAS is defined to use NLCD only at multiple of 1 arcseconds (for PPA/WISP zones mainly), the present retiling 
does not loose information compared to the original geodata in the original Albers projection.



