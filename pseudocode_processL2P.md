** A file that Alison told me to make  **

*Some psuedocode...*

**Description:** Read in satellite data in L2P, regrid to L3, QC (apply bias correction, wind & quality level filters), return SSTs that pass QC checks and associated uncertainty estimate, recalculate "today's" bias correction)  <br>
Andy typing in another line to test Alison's theory...
Oo-err missus - it worked.  I bow before the superior knowledge of my colleague...

*Back to the "psuedocode"*

**Read in L2P files for a given sensor:**
- Read in parameters from configuration file (e.g. filename stem, QC thresholds for datatypes, etc.)
- Need date (GHRSST filenames are of the form YYYYMMDDHHMMSS*.nc)
- Obtain list of files for day in question
- Step through file list, reading in SST for each file:
    - Return fields of SST plus associated data needed for QC/filtering/etc. (SSES bias & SD, wind speed, Quality Level, lat/lon, time (e.g. GHRSST time in seconds))
    - Filter out data below QC threshold, wind speed threshold
    - Apply SSES bias (if desired), "yesterday's" bias, and QC against yesterday's analysis (anomaly threshold), "buddy checking"
    - Calculate "today's" bias (evolve bias field forwards) by obtaining difference field with today's reference, adjusting bias by weighting (e.g. 60:40 today:yesterday), smoothed over (e.g.) 2x2 degrees lat/lon
    - Add today's bias layer for each valid SST
    - Final output is on original L2P satellite grid:
        - SST (w/ SSES bias applied, if desired), SSES_SD, lat, lon, time, bias (interpolated from daily large-scale smoothed bias field for the dataset in question)
    
**Note: SST observation types and associated bias fields are best kept separate for day and night, e.g. VIIRS daytime is treated as a different dataset from VIIRS nighttime**

- Could export as a vector of "good" observations (plus associated info: lat, lon, time, etc.) so one file for daytime obs, and another for nighttime obs, for each sensor.  This will greatly reduce data volume in the presence of cloud/land.

- To speed up processing for regional test case, could simply throw out/ignore/skip files in list which contain no data within bounding box (should be able to use information in global metadata)

- Daily bias correction fields will need to be maintained/evolved on (e.g.) 0.1x0.1 degree grid.  Updating this on a daily basis will take a little thought (the main considerations have been given above, but there will be some details to be worked out, especially w.r.t. smoothing...)
    


