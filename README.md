# Data Pre Processing

This repository is home to the access/importing of L2 Data Resources:

From this a consensus emerged on high level objective and secondary needs (L1/L2 system engineering requirements)

### Level 1 requirements (the foundational requirements)

Study Area & Temporal Coverage:
 - Time period 2011-present (forward stream)
 - Start with VIIRS era (2012?)
 - North and South Atlantic Ocean Basins (GOES-16)

5 sensors to blend from polar orbiting  IR, MW and Geostationary IR using night+day, AM and PM orbits
 - [L2P S-NPP VIIRS (PM)](https://doi.org/10.5067/GHVRS-2PO28) [2012-Feb-01 to Present]
 - [L2P MetopB AVHRR (AM)](https://doi.org/10.5067/GHMTB-2PS28) [2012-Oct-19 to Present]
 - [L2P GCOM AMSR2 and realtime dataset (PM)](https://doi.org/10.5067/GHAM2-2PR8A) [2012-Jul-2 to Present]
 - [L2P GOES-16](https://doi.org/10.5067/GHG16-2PO27) [2017-Dec-15 to Present]
 - [L2P SEVIRI (Metosat)]()
 

Reference SST is [CMC L4](https://podaac.jpl.nasa.gov/dataset/CMC0.1deg-CMC-L4-GLOB-v3.0)

### Level 2 requirements (probably not attainable this week)

Reference is the ASTR/SLSTR series IR radiometers or in situ data
SST adjusted using a diurnal warming model
Motion compensation
A global output vs. regional proof of concept


### L2 & L3 Aggregate Product Deliverables

 * Native geo-location retained for each pixel  (no super obs)
 * Re-Packing includes SST, Quality Level (3-5), SSES, Wind, and others
 * Re-Packing includes flagged daytime screened SST using a wind speed (which one?)
 
**NOTES:**
Daily 24 hour product as diurnal free as possible
Reference is the ASTR/SLSTR series IR radiometers or in situ data