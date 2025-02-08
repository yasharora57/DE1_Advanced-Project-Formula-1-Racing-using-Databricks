# DE1_Project-Formula-1-Racing

## Formula 1 Racing Data Engineering 🏎️📊

### 📑 Contents:
- **Code**
- **Project Resources:** Data files, Data model, User Guide
- **Project Architecture**

The **Formula 1 Racing Data Engineering Project** is designed to ingest, process, and analyze historical and incremental F1 race data from **1950 to 2021** using the **Medallion Architecture** on **Databricks** with **Azure Data Lake Gen-2 (ADLS)** and **Azure Data Factory (ADF)**.

---

### 🚀 Key Features:

- **Tech Stack:** Databricks, ADLS Gen-2, ADF, Azure Key Vault, PySpark, SQL

- **Data Storage:**
  - **Raw Layer:** CSV & JSON files (full load & incremental data).
  - **Processed Layer:** Delta files for optimized querying
  - **Presentation Layer:** Delta files (race_results, driver_standings, constructor_standings, calculated_race_results) transformed from files in processed layer

> **Note:** 
> - **Full Load Files:** circuits, races, constructors, and drivers  
> - **Incremental Load Files:** results, pitstops, laptimes, and qualifying  
> - All files contain race data from **1950 to 2021** with a cutover (historical) file and delta files for different periods of 2021.

---

### ⚡ Data Processing:

- **Incremental Loading:**
  1. **Processed Layer:** 
     - Implemented for race results, pit stops, lap times, and qualifying data
     - Utilizes `dbutils` date parameterized widget to ingest corresponding date files from raw container
     - Files are written in the processed layer by using Merge (Upsert) operations with dynamic partition pruning based on `race_id` column
     - Managed delta tables pointing to these files are stored in the f1_processed schema
  2. **Presentation Layer:** 
     - Yearly driver and constructor standings are aggregated incrementally using partitioning of the `race_year` column and Merge (Upsert) operations
     - Incremental aggregation is performed in the transformation phase too in order to avoid full re-aggregation
     - Managed delta tables pointing to these files are stored in the f1_presentation schema

---

### 📊 Analytics & Insights:

- **Dominant Drivers & Teams:** 
  - Analysis based on total and average points
  - Race participation thresholds applied

- **Key Tables:**
  - **Race Results:** Integrated data from multiple sources
  - **Driver & Constructor Standings:** Yearly performance metrics
  - **Calculated Race Results:** Top 10 drivers by year

---

### 🔄 Automation with Data Factory:

- **Pipelines:**
  1. **Ingestion Pipeline:** 
     - Checks for new data folders with Get Metadata activity
     - Ingests files automatically in case file is not found with the help of IF activity
  2. **Transformation Pipeline:** 
     - Executes data transformations with defined dependencies, executing race_results file before driver and constructor standings
  3. **Master Pipeline:** 
     - Orchestrates the full ETL workflow by running ingestion pipeline before transformation pipeline

---

This project showcases an **end-to-end data pipeline** for real-time analytics and performance monitoring in **Formula 1 racing**. 🎽



