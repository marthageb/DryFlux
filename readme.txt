#Data and code generated for DryFlux 
#Initial submission to PNAS on 16April2020 "Ecohydrological water-carbon coupling improves dryland carbon flux prediction of average uptake, interannual variability, and drought"

Authors:
 	Barnes, Mallory L.; Farella, Martha M.; Scott, Russell L.; Moore, David J.P.; Ponce-Campos. Guillermo E.; Biederman, Joel A.; MacBean, Natasha; Litvak, Marcy E.; and Breshears, David D. 

Year:
    2021

Title:
    Data and code for DryFlux

Corresponding Author for Code:   
  Martha Farella, Indiana University, O'Neill School of Public and Environmental Affairs, farellam@iu.edu

License:
    MIT
    
DOI:
 
 
---------------------------------------------
## Summary
Contains code and data used for DryFlux and the analysis presented in PNAS manuscript, "Ecohydrological water-carbon coupling improves dryland carbon flux prediction of average uptake, interannual variability, and drought"


---------------------------------------------
## Files and Folders

code: code used in the analysis presented in the manuscript. 
    MATMAPelev.R: get metadata for the Flux tower sites. 1.download SRTM elevation tiles and extract values for site locations 2. download WorldClim MAT/MAP tiles and extract data for site locations; 3. combine 1 and 2 into a single dataframe; 4. determine the OzFlux sites that fall within the 'dryland regions' defined by United Nations Environment World Monitoring Centre; 5. get boundaries for US, mexico, and Australia territories
    SPEI: code used to compute SPEI and extract meterological variables for flux tower locations. CRU data downloaded from: https://crudata.uea.ac.uk/cru/data/hrg/cru_ts_4.04/cruts.2004151855.v4.04/ meterological variables downloaded included: precipitation (pre), tmin (tmn), tmax (tmx), vapor pressure (vap), potential evapotranspiration (pet), and Tavg (tmp) for time periods: 1991-2000; 2001-2010; 2011-2019; citation: Harris et al. (2020) doi:10.1038/s41597-020-0453-3
        combineNCD.R: combine the seperate cru .nc files into a single file that contains oberservations from 1991 - 2019 for each variable needed to calculate spei (pet and precip)
        computeSPEI.R: This script computes the global SPEI dataset at different time scales. One netCDF file covering the whole globe and time period is generated for each time scale,e.g. spei01.nc for a time scale of 1 month, etc. Output files are stored on DryFlux/data/outputNcdf
        functions.R: dependencies for 'computeSPEI.R'
        SPEIextract: code used to extract cru data values for flux tower locations, pre-process for downstream analysis, and create rasters for upscaling; lines 13:90 are for SPEI extraction; lines 92:144 are for the other meterological variables; lines 146:177 are for last month precip and Tavg; lines 179:340 are for OzFlux sites; lines 345:438 raster layer creation
    MODISprocessing.R: used to convert hdf files to a single .csv file for each year where ndvi and evi values are extracted for flux tower locations and data formatted (single row/observation for each site/date) for downstream analysis also create .tif EVI and NDVI raster layers; .hdf data products downloaded from: https://e4ftl01.cr.usgs.gov/MOLT/MOD13C1.006/ 16th day global 0.5deg CMG MOD13C1; Date downloaded is the date closest to the 15th/16th of each month; citation: Didan, K. (2015). MOD13C1 MODIS/Terra Vegetation Indices 16-Day L3 Global 0.05Deg CMG V006 [Data set]. NASA EOSDIS Land Processes DAAC. Accessed 2020-10-13 from https://doi.org/10.5067/MODIS/MOD13C1.006
    daylength.R: determine daylength of sites based on dates of CRU dates; create daylength rasters; daylength calculated using the 'daylength' function in the 'geosphere' R package; daylength calculated on the 15th or 16th of each month in accordance with CRU dates for each flux tower location.; raster stack created for the whole world with daylengths for each of the 348 CRU dates
    CleanFlux.R: get fluxtower GPP values for mid-month time frames from the daily Fluxtower MatLab files
    MOD17A2Hdl.txt: code used on google EarthEngine to download individual .csv files of MODIS 8-day global 500m GPP data products (MOD17A2H) for each flux tower site.
    MODIS_GPP.R: get MODIS GPP values in format needed for downstream analysis. MODIS 8-day global 500m GPP data products (MOD17A2H) downloaded with google EarthEngine using code on 'MOD17A2Hdl.txt'. 
    Fluxcom_extract.R: extract daily Fluxcom predictions for tower locs, then calc mid-month GPP vals; daily Fluxcom GPP predictions requested from Fluxcom data administrator
    OxFlux.R: get OzFlux GPP values formatted for downstream analysis
    data_prepro.R: combine all response and explanatory variables together into a single dataset for model building, testing, and analysis
    RF.R: Build the Random Forest Machine Learning Models, save model outputs and evaluate performance
    applyRF.R: Apply the random forest model to raster layers to predict global GPP
    RFevaluation.R: Evaluate model performance. Create a dataframe with observed and predicted GPP values from DryFlux, MODIS, and Fluxcom. Create Fig1a-c, Fig2a-b, Table S2, Figs. S2, S3, S5, and S6
    RFseasonal.R: Evaluate seasonal trends in RF predictions at SW USA and OzFluxsites. Create Fig1d-f, Figs. S4 and S7
    RFspatial_maps.R: calcualte mid-monthly composites of Fluxcom GPP predictiosn, calculate annual GPP estimates for Fluxcom and DryFlux, create annual difference maps (Fig2c,d; Fig3c,d), calcualte DryFlux GPP z-scores globally and create maps (Figs3a,b)

        
        
        
data: data used in analysis presented in the manuscript
    site_locs.csv : site meta data for Southwestern NA sites including: site ID, site name, lat/long, vegetation classification (determined from MODIS IGBP land cover classification at flux tower location), MAT, MAP, and elevation (from FluxNet) 
    Ozsites.csv: site meta data from FluxNET for the Australian sites
    site_meta.csv: meta data for SW USA sites (output from 'MATMAPelev.R')
    aus_meta.csv: meta data for the Australian sites (output from 'MATMAPelev.R')
    CRU: meterological data downloaded from CRU
        cru_ts4.04.YR01.YR02.var.dat.nc : raw data for timeframe 'YR01.YR02' for each meterological 'var' (precipitation (pre), tmin (tmn), tmax (tmx), vapor pressure (vap), potential evapotranspiration (pet), and Tavg (tmp))
        pet1991.2019.nc : pet values for 1991 - 2019, output from 'combineNCD.R' and needed for continuous SPEI calculations over the study period
        precip1991.2019.nc : precip values for 1991 - 2019, output from 'combineNCD.R' and needed for continuous SPEI calculations over the study period
        dates.csv: dates of the CRU observations
    outputNcdf: SPEI calculations over the study period in 1, 3, 6, and 12 month intervals. output from 'computeSPEI.R'
    MOD13C1: MODIS 16-day NDVI and EVI data products (MOD13C1); data inputs used in 'MODISprocessing.R'; each subfolder contains monthy .hdf files for each year (subfolder name)
    Flux_raw: MatLab files for each fluxtower site containing daily NEP, GEP, Reco, ET, precip, Tair, VPD, Rnet, and Rsolar
    MOD17A2H: folder containing the .csv files of 8-day MODIS GPP (MOD17A2H) data products (output from EarthEngine code 'MOD17A2Hdl.txt')
    STRM: folder containing STRM elevation rasters covering all site locations
    WorldClim: folder containing .zip files of MAT and MAP for study sites
    spatial: folder containing spatial data needed for analysis
        USbounds: shape file and associated files of USA state boundaries
        MXbounds: shape file and associated files of Mexico territory boundaries
        AUSbounds: shape file and associated files of Australia territory boundaries
        sites: shape file and associated files SW USA Flux tower sites
        AUS_sites: shape file and associated files OzFlux tower sites
        globalMAT.tif: global MAT raster
        globalMAP.tif: global MAP raster
        global_elev.tif: global elevation raster
    Drylands_dataset_2007: folder containing spatial data defining dryland regions defined by the United Nations Environment World Monitoring Centre
    Fluxcom: folder containing daily Fluxcom GPP predictions acquired link from Fluxcom data administrator
    OzFlux: folder containing OzFlux site data. Data downloaded from https://fluxnet.org/data/download-data
    RFdatanew.csv: output from 'data_prepro.R' all of the data needed for Random Forest analysis on the SW USA sites.
    aus_RFdata.csv: output from 'data_prepro.R' all of the data needed for Random Forest analysis on the Australian OxFlux sites