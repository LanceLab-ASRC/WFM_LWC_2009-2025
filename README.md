**Hourly Liquid Water Content (LWC) and Visibility data from Whiteface Mountain (WFM), 2009 through 2025.**

**Also, in-cloud average LWC associated with each cloud water sample collected from WFM over this same time period.**

# LWC Calculation Workflow

## Compile hourly LWC data

1. Compile all hourly LWC data for 2009 through 2017 from ALSC (e.g. $\color{#2ea44f}{\text{WFC.2009.MET.R.xlsx}}$)
2. Adjust minute resolution ASRC LWC data (2018 through 2025) for baseline drift, based on elevated values during clear air periods (see [**Adjustments**](#adjustments), below)
3. Calculate hourly avg ASRC LWC values (2018 through 2025, not previously published elsewhere)
4. Filter out LWC values below zero or above 2.0 g/m^3 from the hourly average datasets (set to NaN)
5. Time sync and evaluate hourly avg LWC data for 2022 through 2025 based on PVM/visibility sensor comparison
6. Fill gaps in Gerber PVM dataset with LWC calculated based on visibility sensor readings (see [**Merge_LWC_Vis**](#merge-lwc-vis), below)
7. Append ASRC hourly average merged LWC data to ALSC data

## Calculate in-cloud LWC average values associated with cloud water samples

1. Compile list of DumpDateTimes and SampleIDs for all cloud water samples
   * 1.1 Start with [Chris GitHub](https://github.com/LanceLab-ASRC/WhitefaceMountainCloudTrends) dateW and LabID data for 2009 through 2017 ($\color{#2ea44f}{\text{AllCloudandMetData.csv}}$) 
       * dateW = DateON (start time for cloud water sampling interval)
       * convert dateW to Dump_DateTime (end time for cloud water sampling interval)
         * for 2009 through 2013, Dump_DateTime = DateON + 3 hrs
         * for 2014 through 2017, Dump_DateTime = DateON + 12 hrs
       * dateW in 2014 are incorrect (neither DumpDateTime NOR DateON)
         * DumpDateTime in 2014 now calculated based on hourly Pooled Volume dataset (see [**DumpTimes2014**](#dumptime2014), below)
   * 1.2 Append [Archana GitHub](https://github.com/LanceLab-ASRC/WFM-Cloud-Water-Datasets-2018-2024) DumpDateTime and ID data for 2018 through 2024 (e.g. $\color{#2ea44f}{\text{2018.xlsx}}$)
   * 1.3 Add 2025 Dump_DateTime and Sample IDs from 2025 cloud water master list
2. Calculate average in-cloud LWC average for each cloud water sample (see [**calc_avg_incloud_LWC2**](#calc-avg-incloud-lwc), below)
3. Perform linear fit of in-cloud LWC averages
4. Calculate annual means for in-cloud LWC averages each summer (see [**calc_annual_mean_sampleLWC**](#calc-annual-mean-sample-lwc), below)

## Coding for Calculations _(Done in **Igor Pro**)_

<a name="merge-lwc-vis">Merge_LWC_Vis</a>
![Igor Function for filling in gaps based on visibility sensor](/IgorFunction_Screenshots/Merge_LWC_Vis.png)

<a name="dumptimes2014">DumpTimes2014</a>
![Igor Function for determining Dump Times](/IgorFunction_Screenshots/DumpTimes2014.png)

<a name="calc-avg-incloud-lwc">calc_avg_incloud_LWC2</a>
![Igor Function for calculaing average in-cloud LWC for each sample](/IgorFunction_Screenshots/calc_avg_incloud_LWC2.png)

<a name="calc-annual-mean-sample-lwc">calc_annual_mean_sample_LWC</a>
![Igor Function for calculating annual means for in-cloud LWC averages over each summer](/IgorFunction_Screenshots/calc_annual_mean_sample_LWC.png)

## $\color{#2f81f7}{\text{Adjustments}}$

- 1-min resolution LWC data in 2018 was shifted +0.04 Aug 3 - 8
- 1-min resolution LWC data in 2018 was shifted -0.04 Aug 10 - 17
- 1-min resolution LWC data in 2020 was shifted -1.63 July 15 - 16
- 1-min resolution LWC data in 2020 was shifted -0.08 June 23 - 25
- 1-min resolution LWC data in 2022 was shifted +0.1 June 19 - July 11
- 1-min resolution LWC data in 2023 was shifted -0.02 June 20 - Sept 29
- 1-min resolution LWC data in 2023 was shifted +0.01 July 6 - 19
- 1-min resolution LWC data in 2024 was shifted -0.01 June 14 - 17
- 1-min resolution LWC data in 2024 was shifted -0.02 June 17 - 22
- 1-min resolution LWC data in 2024 was shifted -0.058 June 26 - 28

Then, 1-min resolution LWC data averaged hourly in 2018-2025

- AveragXSecs("DateTimeW","LWC_value","LWC_hrlyavg","LWC_hrlystd","LWC_hrlynum",3600)