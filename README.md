# DE1_Project-Formula-1-Racing

## Formula 1 Racing Data Engineering 🏎️📊

The **Formula 1 Racing Data Engineering Project** is designed to ingest, process, and analyze historical and incremental F1 race data from **1950 to 2021** using the **Medallion Architecture** on **Databricks** with **Azure Data Lake Gen-2 (ADLS)** and **Azure Data Factory (ADF)**.

---

### 🚀 Key Features:

- **Tech Stack:** Databricks, ADLS Gen-2, ADF, Azure Key Vault, PySpark, SQL

- **Data Storage:**
  - **Raw Layer:** CSV & JSON files (full load & incremental data)
  - **Processed Layer:** Managed Delta tables for optimized querying
  - **Presentation Layer:** Aggregated insights for analytics

> **Note:**
> - **Full Load Files:** circuits, races, constructors, and drivers  
> - **Incremental Load Files:** results, pitstops, laptimes, and qualifying  
> - All files contain race data from **1950 to 2021** with a cutover (historical) file and delta files for different periods of 2021.

---

### ⚡ Data Processing:

- **Incremental Loading:**
  1. **Processed Layer:**
     - Implemented for race results, pit stops, lap times, and qualifying data
     - Utilizes `dbutils` date parameterized widget
     - Merge (Upsert) operations with dynamic partition pruning based on `race_id` column
  2. **Presentation Layer:**
     - Yearly driver and constructor standings with incremental aggregation
     - Avoids full re-aggregation using partitioning of the `race_year` column

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
     - Checks for new data folders
     - Ingests files automatically
  2. **Transformation Pipeline:**
     - Executes data transformations with defined dependencies
  3. **Master Pipeline:**
     - Orchestrates the full ETL workflow

---

This project showcases an **end-to-end data pipeline** for real-time analytics and performance monitoring in **Formula 1 racing**. 🎽

