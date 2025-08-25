# Azure Resources – Bee Haven Case Study

This document describes the Azure resources provisioned to support the **Bee Haven data pipeline**.  
The architecture follows a **Lakehouse pattern** with Bronze, Silver, and Gold layers managed through **Azure Data Lake Storage Gen2, Synapse Analytics, and Azure Data Factory (ADF)**.

---

## 1. Resource Group
- **Name**: `DSAI2-beehavenproject-rg`
- Purpose: Central container for all resources related to the Bee Haven project.

---

## 2. Storage

### Azure Blob Storage
- Holds initial raw data ingested into the environment.
- Example container: `bee-raw`

### Azure Data Lake Storage Gen2
- Core storage for the Lakehouse.
- Organized into **medallion architecture**:
/bronze
/new # fresh raw files from sensors
/archive # archived raw copies
/silver
/processing # intermediate cleaned data
/[tables] # structured datasets
/gold
/processing # staging before finalization
/[tables] # curated analytics-ready datasets


---

## 3. Azure Synapse Analytics

### Spark Pool
- **Name**: `dsai002pool`
- **Executor Size**: Small
- **Driver Size**: Small
- Dynamic allocation: 1 executor minimum, 1 executor maximum.

### Synapse Notebooks
- **Bronze → Silver Transformation**
  - `synapse_notebook_bronze_to_silver.ipynb`
  - Cleans hive data and enriches with weather API calls.
- **Silver → Gold Transformation**
  - `synapse_notebook_silver_to_gold.ipynb`
  - Creates analytics-ready, curated data models.

---

## 4. Azure Data Factory (ADF) Pipelines

### BronzeToSilver (`Pipeline_BronzeToSilver.json`)
- Moves new raw hive CSVs (`bronze/new`) to:
  - **Archive** (`bronze/archive`)
  - **Processing** (`silver/processing`)
- Executes **Synapse Bronze → Silver notebook** for cleaning & enrichment.
- Clears `bronze/new` after processing.

### SilverToGold (`Pipeline_SilverToGold.json`)
- Iterates through **Silver folders**.
- Copies new parquet files to **Gold/processing**.
- Executes **Synapse Silver → Gold notebook** to create curated analytics tables.
- Clears `gold/processing` after completion.

### DailyProcessing (`Pipeline_DailyProcessing.json`)
- Orchestration pipeline that runs the full chain daily:
  1. `BronzeToSilver`
  2. Waits 5 minutes
  3. `SilverToGold`

---

## 5. Linked Services & Datasets

- **Linked Service**
  - `AzureSynapseArtifacts1` → for notebook execution inside Synapse
- **Datasets**
  - `bronze_new` → new raw sensor data
  - `bronzearchive` → archived raw data
  - `Silver_processing` → intermediate silver stage
  - `silver_subfolders` → silver structured tables
  - `gold_processing` → gold staging layer

---

## 6. Scheduling & Automation
- **Daily trigger** runs `DailyProcessing`.
- Ensures hive and weather data are processed, enriched, and refreshed into the Gold layer.

