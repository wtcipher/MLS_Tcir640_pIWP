# MLS_Tcir640_pIWP


Regress MLS 640GHz Tcir to CALIOP ice observations, so as to rebuild an alternative longterm, consistent ice product for the entire MLS lifespan. The rebuilt pIWP is focused on the upper troposphere to lower stratosphere 10-20 km, covering 64 degree N-S, from 2004 to 2020 (the latest so far).

**There are two files in this repositary.**
- **mlsTcir_calIWP_matched_cat_B10ch1_ALL_8x4_2004-2020.sav**
- **retrieval_final_anom_corr_B10Ch1_ALL_8x4_2004-2020.sav**


### 1.  mlsTcir_calIWP_matched_cat_B10ch1_ALL_8x4_2004-2020.sav
This has the filtered and gridded (8x4 longitude by latitude) MLS 640-GHz cloud-induced radiances (Tcir), matched to the CALIOP data availability. 

This file has the following fields saved:
- **definition of grids: MLS and CALIOP data are gridded into those grids and vertical intervals**
**lonRng, lonGrid[45], lonEdgeL[45], lonEdgeR[45]**: 
longitude grids 0-360 with interval of 8 degrees
**latRng, latGrid[45], latEdgeL[45], latEdgeR[45]**: 
latitude grids 90S to 90N with interval of 4 degrees
**isoh[21], isoh_low[21], isoh_high[21]**: 
vertical levels and their boundaries, 0-20 km with 1km interval

 -  **CALIOP (CAL) related variables**
    **timeCAL**:  time structure of CALIOP 2008-2017, including 
              yrArr[105], monArr[105], lArr[105], fArr[105].
    **pIWP_lev[21]**: CALIOP pIWP levels in interval of 1 km. 
                  So 10 means integrated 10-20 km.
    **pIWP[45, 45, 21, 105]**: CALIOP partial ice water path integrated from certain altitude to 20km, in units of g/m2
 
 - **MLS related variables**
 **timeMLS**: time structure of MLS for 2004-2020, including 
          yrArr[197], monArr[197], lArr[197], fArr[197].
 **Tcir_lev[21]**: MLS Tcir at 21 tangent heights from 0 to 20 km. 
 **Tcir[45, 45, 21, 197]**: MLS 640-GHz Tcir after filtering, in K. 
 

 - **matched MLS and CALIOP records and the regression of the two in functioins of latitude, altitudes, and months**
 **timeMCH**: matched months in structure for 2008-2017, including 
             yrArr[105], monArr[105], 
             lArr[105], fArr[105].
  **rgnLat[2, 20]**: 20 latitude bands running from 82S to 82N with 8 degree intervals
  **rgnTitle[20]**: the string of those 20 latitude bands, namely 
          [82-72S], [72-64S], [64-56S], [56-48S], [48-40S], [40-32S], [32-24S],  [24-16S], [16-8S], [8S-0], 
          [0, 8N], [8-16N], [16-24N], [24-32N], [32-40N], [40-48N], [48-56N], [56-64N], [64-72N], [72-82N]
  **corr_lev[21]**: 21 levels from 0 to 20km 
  **corr[21, 21, 105, 20]**: at each level, for each month and in each latitude bands
  **corr_sig[21, 21, 105, 20]**: correlation significant or not (1-significant, >0-not significant)
  **slope[21, 21, 105, 20]**: regression slope within each latitude bands at each level, for each month
  **const[21, 21, 105, 20]**: regression constant within each latitude bands at each level, for each month 
 

Note that in order to seek the fair regression correlation between MLS Tcir and CALIOP pIWP, we sampled MLS at the CALIOP data availability. The CALIOP data availability can be found here: https://www-calipso.larc.nasa.gov/tools/data_avail/index.php?d=2017
That said, in pairing MLS to CALIOP data, we ignored those following dates as CALIOP data are not available:
2009/02 17-28, 10d;
2009/03 01-12, 12d;
2011/06 07-14, 8d;
2012/01 23-31, 9d;
2012/02 01-02, 2d;
2012/03 07-21, 15d;
2013/05 16-27, 12d;
2014/01 06-13, 8d;
2014/02 21-28, 8d;
2015/06 19-29, 11d;
2016/01 28-31, 4d;
2016/02 01-28, 28d;
2016/03 01-14, 14d;
2017/07 15-31, 17d;
2017/08 01-04, 4d;
2017/09 05-15, 11d;
2018/09 13-24, 14d;
2018/10 15-18, 4d.






### 2.  retrieval_final_anom_corr_B10Ch1_ALL_8x4_2004-2020.sav
This file has the final retrieved pIWP based on the regression relation built from MLS 640-GHz Tcir to CALIOP pIWP.
Based on the first file "mlsTcir_calIWP_matched_cat_B10ch1_ALL_8x4_2004-2020.sav", we have the regreession 

         pIWP_CALIOP (j,k,t) = α(j,k,t) x [-T_cir (j,k,t)]+ β(j,k,t)      

Where α is the slope in unit of g/m2/K that represents the rate of change in pIWP as Tcir (in unit of K) changes, and β is the intercept in unit of g/m2; all in functions of latitude bands (j), altitudes (k), and months (t). 

Then, we calculate the climatology of coeffects α(j,k,t) and β(j,k,t) into into α' (j,k,12) and β' (j,k,12). Using the look-up-tables, we apply the climatological regression coefficients to the monthly gridded MLS Tcir from August 2004 through December 2020 for each month/altitude/latitude, resulting in derived MLS pIWP to the latest MLS observing period, i.e.,

         pIWP_MLS (j,k,t)=α'(j,k,12) x [-T_cir (j,k,t)]+ β'(j,k,12)                
Here, months (t) running from August 2004 to December 2020, and all January’s pIWPMLS is obtained from using January’s coefficient with that month’s Tcir. 


This file has the following fields saved:
 -  **definition of grids: MLS and CALIOP data are gridded into those grids and vertical intervals**
    **lonRng, lonGrid[45], lonEdgeL[45], lonEdgeR[45]**: longitude grids 0-360 with interval of 8 degrees
    **latRng, latGrid[45], latEdgeL[45], latEdgeR[45]**: latitude grids 90S to 90N with interval of 4 degrees
    **isoh[21], isoh_low[21], isoh_high[21]**: vertical levels and their boundaries, 0-20 km with 1km interval


 -  **CALIOP (CAL) related variables**
    **timeCAL**:  time structure of CALIOP 2008-2017, including 
              yrArr[105], monArr[105], lArr[105], fArr[105].
    **pIWP_lev[21]**: CALIOP pIWP levels in interval of 1 km. 
                  So 10 means integrated 10-20 km.
    **pIWP_CAL[45, 45, 21, 105]**: CALIOP partial ice water path integrated from certain altitude to 20km, in units of g/m2
 
 
 -  **MLS related variables**
    **timeMLS**: time structure of MLS for 2004-2020, including 
          yrArr[197], monArr[197], lArr[197], fArr[197].
    **pIWP_lev[21]**: MLS derived pIWP  at 21 tangent heights from 0 to 20 km. 
    **pIWP_MLS[45, 45, 21, 197]**: MLS 640-GHz Tcir derived pIWP, g/m2. 
    
    
