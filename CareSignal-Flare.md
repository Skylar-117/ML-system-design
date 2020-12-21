# Flare

The prediction core of the CareSignal platform.

## Structure (in progress...)
```
flare --------------------------------- # Flare main folder
├── loader ---------------------------- # ELT - Data loading module
│   ├── mysql.py ---------------------- # MySQL connection and database loading
│   └── s3.py ------------------------- # AWS S3 connection and data downloading
├── transformer ----------------------- # ELT - Data transforming module
│   ├── config.py --------------------- # Configurations
│   ├── custom_functions.py ----------- # Repeatedly used SQL queries as functions
│   └── sql_queries.py ---------------- # SQL queries for creating intermediate tables
├── predictor ------------------------- # ML - Prediction module
│   ├── preprocessing.py -------------- # Data preprocessing (including feature engineering)
│   ├── pipeline.py ------------------- # Pipeline of the entire process
│   └── prediction.py ----------------- # Prediction function
├── utils ----------------------------- # Utility functions
│   └── functions.py ------------------ # Custom utility functions
├── models ---------------------------- # Offline model pickle files
└── data ------------------------------ # External datasets
    └── external_YYYY-MM-DD.gz -------- # External data including CS_geo/CS_rucc2013/CS_stats, where CS_geo is the deluxe database bought from area-codes.com
```

## Example
This shows an example of executing load.py and transform.py to perform Extract/Load and Transform step of the ELT pipelines.
```shell
(flare) (base) ➜  flare git:(master) ✗ python load.py
=> Refreshed directory
=> Loading S3 bucket epx-backup...
=> Downloading data from bucket epx-backup...
=> Data has been downloaded and saved to /Users/dan/Code/flare/dump/sql-1607456591397.zip
=> Loading into EpxProd...
2.32MiB 0:00:06 [ 369KiB/s] [================================>] 100%            
 786MiB 0:20:09 [ 666KiB/s] [================================>] 100%            
=> Loaded EpxProd!
Time cost: 21.89min

(flare) (base) ➜  flare git:(master) ✗ python transform.py
=> Refreshed directory
=> Creating table CS_realPatients ... 98411 rows
=> Creating table CS_activatedPatients ... 58864 rows
=> Creating table CS_svtrain ... 4143325 rows
=> Creating table CS_svpred ... 418818 rows
=> Creating table CS_svtrain_agg ... 311129 rows
=> Creating table CS_svpred_agg ... 35866 rows
=> Creating table CS_firstLastSession ... 39465 rows
=> Creating table CS_svtrain_agg_earliest_c7nrs ... 28025 rows
=> Creating table CS_svtrain_agg_death ... 39406 rows
=> Creating table CS_svtrain_agg_final ... 161350 rows
=> Creating table CS_svpred_agg_final ... 16992 rows
=> Exported bundled external datasets to /Users/dan/Code/flare/flare/data/external_2020-12-07.gz

Finished! Cursor has been closed and MySQL server has been disconnected.
Time cost: 16.16min
```
