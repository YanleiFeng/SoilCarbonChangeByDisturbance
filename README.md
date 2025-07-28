# SoilCarbonChangeByDisturbance
This is a developing script for understanding soil changes under multiple disturbance using NEON datasets.

Script 1: NEON_accessData_DISTURBANCEname.ipynb

Purpose: To extract site and chemical data from NEON datasets related to this type of disturbances and save relevant subsets for downstream analysis. This documentation uses fire disturbance as an example hereafter

Input Files:
eventType.csv	NEON metadata on disturbance events
sls_chemistry.csv	NEON soil chemistry measurements across sites

Output Files:
fire.csv	Filtered events related to fire disturbances
slsChemistry_fire_disturbance.csv	Soil chemistry data for fire-affected sites

Workflow Overview

    1. Load Event Metadata
    File: eventType.csv
    Operation: Read all disturbance events recorded by NEON.

    2. Filter for disturbance Events. All the other disturbances follow the similar process
    Filters events where eventType == "fire".

    3. Export Fire Event Records
    Saves the filtered fire events to fire.csv.

    4. Identify Fire-Affected Sites
    Extracts unique site IDs where fire events occurred.

    5. Load Soil Chemistry Dataset
    File: sls_chemistry.csv
    Operation: Load full soil chemistry records across all NEON sites.

    6. Filter Chemistry Data by Fire Sites
    Filters soil data to only include sites affected by fire.

    7. Export Filtered Soil Chemistry
    Saves filtered soil chemistry to slsChemistry_fire_disturbance.csv.

Notebook 2: SoilOrganicCarbon_byFireDisturbance-byPlot-yearlyAfter.ipynb

Purpose: To assess the impact of fire disturbances on soil organic carbon (SOC) at individual plots within NEON sites by:
1. Linking fire events to site-specific soil chemistry data
2. Filtering valid plots with known disturbance dates
3. Aggregating and plotting SOC over time, relative to fire occurrence
4. Generating summary statistics of post-disturbance SOC trends

Input Files
naturalDisturbance.csv	Fire disturbance metadata per site
slsChemistry_fire_disturbance.csv	NEON soil chemistry data for fire-affected sites

Output
a csv file of dicts with per-plot SOC summaries across years

Workflow Overview:

    1. Load Input Datasets
    Loads fire disturbance dates and filtered soil chemistry data.

    2. Validate Plots with Disturbance Dates
    Not all plots have SOC data on the dates that disturbance happen, so the workflow includes 
    steps that iterates through sites to match plots with valid disturbance dates and chemistry measurements. 
    
    Filters out plots without valid data or usable timestamps.

    3. Define Plot-level SOC Analysis Function, this is a reusable function that performs year-wise aggregation of 
    SOC before and after the fire for each plot, and it handles multiple measurements and temporal gaps.
    # Filters chemistry data by plot
    # Aligns data relative to fire year, a 10-20 year time offset has been used here, which means this script will 
    record all the SOC 10 years before the disturbance dates and 20 years after the disturbance dates (There could be 
    multiple same type of disturbances happen in a similar time period, so the SOC_before is calculated before the 
    earliest disturbance and SOC_after is calculated after the latest disturbance time).
    # Group the SOC by Organic layer and Mineral layer (Calculates average organic C percent for each year before and 
    after the disturbance, separately for M and O horizons.)
    # Aggregates SOC annually and also record
    # Returns summary (e.g., mean SOC by year offset)
    
    4. Run Analysis for All Valid Plots
    Iteratively applies the analysis to each valid plot and collects the results.


