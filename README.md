# Futures Prices are Useful Predictors of the Spot Price of Crude Oil

Reinhard Ellwanger and Stephen Snudden (2023), *The Energy Journal*, 44(4), 65–82. [Read the paper](https://doi.org/10.5547/01956574.44.4.rell)

Which futures price should you use to forecast next month’s average crude oil price? In Table 1, the one-month-ahead forecast of the real price of West Texas Intermediate (WTI) has a mean squared prediction error ratio of **0.59** using end-of-month prices and **1.00** using monthly averages. Both forecasts target the same monthly average and use the same no-change benchmark. The paper also finds that longer-dated futures became better predictors of spot prices after the mid-2000s.

## Downloads

| What would you like to do? | File |
| --- | --- |
| Reproduce the paper’s monthly Stata results | [Original replication package (.zip)](./ReplicationCode.zip?raw=true) |
| Construct and evaluate your own futures-based forecasts | [Simplified Stata code, data, and instructions (.zip)](./SimplifiedFuturesForecasts.zip?raw=true) |
| Follow a one-month-ahead forecast in Excel | [Worked WTI example (.xlsx)](./Futures_ExpostData_Example.xlsx?raw=true) |

Each ZIP contains its own data, Stata programs, results workbook, and original instructions (`Readme.docx`). To inspect the code on GitHub, open the [replication driver](./stata/ReplicationCode/driver.do) or the [simplified driver](./stata/SimplifiedFuturesForecasts/driver.do).

## Run either Stata package

1. Extract the ZIP to a writable folder. Keep its subfolders together and close `Results.xlsx` in Excel before running Stata.
2. Set Stata’s working directory to the extracted folder that contains `driver.do`. For the full replication, the commands below assume you extracted the archive under `C:/research`. For the simplified exercise, use `SimplifiedFuturesForecasts` in the first line.

   ```stata
   cd "C:/research/ReplicationCode"
   do "driver.do"
   ```

3. Open `Results.xlsx` in that folder. Columns B–H report mean squared prediction error (MSPE) ratios for horizons of 1, 3, 6, 9, 12, 24, and 36 months. A ratio below one means the futures forecast has a smaller MSPE than the no-change forecast. Columns J–P report the fraction of correctly predicted directions of change. Parentheses contain test p-values.

The authors used **Stata 16 on Windows** and report about one hour for the full run. Use Stata/SE or Stata/MP for the supplied programs, which set `maxvar` to 20,000. On macOS or Linux, adjust Windows path separators where needed. On a case-sensitive filesystem, match the call in `Forecasts_M/NCB/NCB.do` to the supplied lowercase filename `startagain.do`. [Stata edition limits](https://www.stata.com/products/detailed-size-limits/)

### Which published results are in the replication package?

| Paper table | Results available from the supplied files |
| --- | --- |
| 1 | Nominal end-of-month WTI and real monthly-average WTI. The default run reports horizons through 36 months. The driver also identifies the optional 48- and 60-month specifications. |
| 2 | WTI futures–spot spread forecasts with fixed or estimated coefficients. |
| 3 | Alternative WTI futures curves and forecast constructions, including the delivery-timing adjustment. |
| 4 | WTI, Brent, and U.S. refiners’ acquisition cost of imported crude oil (RAC). The supplied RAC run uses the WTI spread. The paper also reports a Brent-spread version. |
| 5 | Real WTI using ex-post or real-time inflation measures. The standalone Excel workbook also illustrates the one-month nominal and real ex-post results. |
| 6 | The paper reports quarterly forecasts. The archived Stata driver covers monthly forecasts. |
| 7 | The adjusted futures forecast and the end-of-month no-change forecast. The paper also reports EIA, AR, and VAR comparisons. |

Run logs go to `Forecasts_M/FUTURES/LOGS/`. The neighboring `EVOL/` folder holds CSV files showing how accuracy and p-values change with the evaluation endpoint. The no-change program uses the corresponding folders under `Forecasts_M/NCB/`. To keep evolution CSVs for several oil series, copy them after each series runs because the futures CSV filenames omit the series name.

## Construct a forecast with the simplified package

The simplified driver evaluates adjusted end-of-month forecasts of real monthly-average WTI, Brent, and RAC (`table1`). It also estimates the intercept, the slope, or both in the WTI futures–spot spread model (`table2`). The adjustment assigns the observed front contract to the two-month horizon and uses the slope across the first 12 contracts to impute the one-month price.

The simplified driver runs this WTI specification.

```stata
do "FUTURES.do" wti 2000 12 2020 12 0 1 3 1
```

The two year–month pairs are **forecast origins**. December 2000 supplies the forecast for January 2001 at the one-month horizon. The final four arguments request the real monthly-average target (`0`), output sheet `table1` (`1`), the adjusted end-of-month model (`3`), and real-time data (`1`). Edit the corresponding line in the simplified driver to change the exercise.

To see the **forecast values themselves**, start in the extracted `SimplifiedFuturesForecasts` folder and run the following commands with the supplied data. The last line displays the WTI forecast path formed in December 2020, indexed by target month.

```stata
cd "Forecasts_M/FUTURES"
do "gendata.do" wti 0 3 1
list time f_fut3_12_2020 if inrange(time, tm(2021m1), tm(2021m12)), noobs
```

To extend the exercise beyond the archived sample, update the daily futures workbooks in `Data_Historical/CompileFutures/`, the monthly spot and fixed-horizon futures series in `Data_Historical/MasterFile_CDataM.csv`, and the CPI and RAC vintages in `Data_RealTime/MASTERFILE_RealTime_Data.xlsx`. The preparation and evaluation programs also specify trading-day cutoffs, forecast-origin years, and a May 2021 evaluation vintage. Update those settings for the new sample.

## Follow the Excel example

Open the workbook in Excel. The **Nominal EoM WTI Futures** sheet contains the adjusted end-of-month WTI curve. On **Nominal WTI**, row 3 shows the first forecast. The December 1991 curve predicts **$19.08** per barrel for January 1992. The no-change forecast is December’s average of **$19.50**, and the realized January average is **$18.79**.

Columns H and I align the two forecasts with the target month. Columns J and K calculate their squared errors. Cell **N3** divides the futures MSPE by the no-change MSPE over 349 months, January 1992 through January 2021. It gives **0.5668** on **Nominal WTI** and **0.6055** on **Real Expost WTI**. The latter sheet deflates the futures price with the target month’s realized U.S. CPI. For a real-time inflation forecast, use the Stata example above.

## Data and citation

The study uses U.S. Energy Information Administration spot prices, daily futures prices from Haver, and U.S. CPI vintages from FRASER and the Federal Reserve Bank of Philadelphia. Section 3 of the paper describes the data and forecast evaluation. The article’s Creative Commons Attribution (CC BY) notice appears on page 65.

For the futures-based forecasts, please cite Reinhard Ellwanger and Stephen Snudden (2023), “Futures Prices are Useful Predictors of the Spot Price of Crude Oil,” *The Energy Journal*, 44(4), 65–82. [DOI](https://doi.org/10.5547/01956574.44.4.rell)

If you use the end-of-month no-change method, please also cite Reinhard Ellwanger and Stephen Snudden (2023), “Forecasts of the Real Price of Oil Revisited: Do They Beat the Random Walk?”, *Journal of Banking & Finance*, 154, 106962. [DOI](https://doi.org/10.1016/j.jbankfin.2023.106962)

For questions about the files, contact [Stephen Snudden](mailto:ssnudden@wlu.ca).
