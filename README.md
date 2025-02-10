# DE1_Project-Formula-1-Racing

## Formula 1 Racing Data Engineering 🏎️📊

## How to Run the Project (Without Using ADF)

Follow these steps to set up and run the project:

1. **Create Required Azure Resources:**
   - **ADLS Gen2 (Azure Data Lake Storage Gen2)**
   - **Azure Databricks**
   - **Azure Key Vault**

2. **Set Up ADLS Containers:**
   - In your ADLS Gen2 account, create three containers:
     - `raw`
     - `processed`
     - `presentation`

3. **Mount ADLS in Databricks:**
   - Mount your ADLS Gen2 account as an external storage on Databricks using SAS tokens.
   - Ensure that the mount point is properly configured so your notebooks can access the data in ADLS.

4. **Configure Secrets:**
   - Create secrets for the SAS keys in Azure Key Vault.
   - Link these secrets to a Databricks Secret Scope so that they can be referenced securely in your notebooks.

5. **Update the Setup File:**
   - Open the `set-up` file and update the following parameters:
     - Data lake name
     - SAS tokens
     - Key names  
   Make sure the values reflect your actual Azure resources.

6. **Configure Paths in the Configuration Files:**
   - In the `includes` folder (inside the `configuration` file), update the path names to correctly refer to the three containers (`raw`, `processed`, and `presentation`).

7. **Run the Project Files in Sequence:**
   - **Raw:** Execute all the files in the `raw` folder.
   - **Ingestion:** Run only the file `0.ingest_all_files.py` from the ingestion folder.
   - **Transformation (Trans):** Run all the files in the `trans` folder.
   - **Analysis:** Run all the files in the `analysis` folder.

Following these steps will allow you to load data from your ADLS containers into Databricks, process and transform the data, and finally perform the necessary analyses as part of your project.

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



