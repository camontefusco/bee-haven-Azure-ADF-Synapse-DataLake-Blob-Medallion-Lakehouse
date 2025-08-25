# Bee Haven – Azure Medallion Lakehouse

## 🌍 Project Overview
Bee Haven is a collective of beekeepers focused on sustainable honey production and bee health research.  
This portfolio project demonstrates how I built a **data lakehouse on Azure** to integrate **hive sensor data** with **weather API data**, following the **medallion architecture (Bronze → Silver → Gold)**.

The work highlights my **data engineering, data science, and cloud skills** in designing scalable pipelines for real-world analytics.

---

## 🏛️ Medallion Architecture
This project follows the **medallion lakehouse pattern**:
- **Bronze**: raw ingestion (CSV + API)  
- **Silver**: cleaned & structured data  
- **Gold**: curated, analytics-ready outputs  

Each layer improves data quality and prepares it for advanced insights.  
([Microsoft reference](https://learn.microsoft.com/en-us/azure/databricks/lakehouse/medallion)

---

## 📊 Datasets
- `flow_schwartau.csv` – hive airflow  
- `humidity_schwartau.csv` – humidity readings  
- `temperature_schwartau.csv` – hive temperature  
- `weight_schwartau.csv` – hive weight  
- `api_weather_sample.json` – enriched weather data  

---

## 📓 Notebooks
- `01_data_prep.ipynb` → preprocessing raw data  
- `02_Intro_to_APIs.ipynb` → API fundamentals  
- `03_weather.ipynb` → fetch & transform weather data  
- `synapse_notebook_bronze_to_silver.ipynb` → Bronze → Silver transformation  
- `synapse_notebook_silver_to_gold.ipynb` → Silver → Gold transformation  

---

## ☁️ Azure Architecture
- **Blob Storage / ADLS Gen2**: medallion storage hierarchy  
- **Synapse Analytics (Spark Pool)**: data transformation at scale  
- **Azure Data Factory (ADF)**: pipeline orchestration  

📖 Full details: [`architecture/azure_resources.md`](architecture/azure_resources.md)

📷 **Medallion Overview**  
![Medallion Architecture](screenshots/medallion_architecture.png)

---

## 🔄 Pipelines (ADF)
- **BronzeToSilver**  
  - Move raw → archive  
  - Clean & enrich via Synapse notebook  
![ADF Pipeline](screenshots/Pipeline_BronzeToSilver.png)

- **SilverToGold**  
  - Transform Silver parquet → curated Gold data  
![ADF Pipeline](screenshots/Pipeline_SilverToGold.png)

- **DailyProcessing**  
  - Orchestrates full Bronze → Silver → Gold flow daily 
![ADF Pipeline](screenshots/Pipeline_DailyProcessing.png)


Pipeline JSON exports: [`pipelines/`](pipelines/)  

---

## 🛠️ Skills & Techniques Demonstrated
- **Data Engineering**
  - Medallion architecture (Bronze/Silver/Gold)  
  - Data Lakehouse design with ADLS Gen2 + Synapse  
  - ADF pipeline orchestration & scheduling  

- **Data Science**
  - Data cleaning & preprocessing  
  - Weather data enrichment via API  
  - Exploratory analysis in notebooks  

- **Cloud & DevOps**
  - Azure resource provisioning  
  - CI-ready pipeline JSONs  
  - Modular, reproducible repo structure  

---

## 📂 Repo Structure
bee-haven-azure-medallion-lakehouse/
├── README.md
├── data/
├── notebooks/
├── pipelines/
├── screenshots/
├── architecture/
│ └──azure_resources.md

## 📬 Contact
Carlos Montefusco
📧 cmontefusco@gmail.com
🔗 GitHub: /camontefusco
