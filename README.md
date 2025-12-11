# Australian Energy Sector: ETL Pipeline & Data Warehouse

## 1. Project Overview
This Data Engineering project implements an end-to-end **ETL (Extract, Transform, Load)** pipeline to integrate Australia's historic electricity generation data (2014-2024) with future renewable energy projects and socio-economic census data.

The system ingests data from disparate sources (APIs, CSVs, Excel), performs rigorous cleaning and geospatial augmentation, and loads the transformed data into a **DuckDB** spatial database using a **Star Schema** to optimize analytical queries.

**Key Features:**
* **Automated Ingestion:** Retrieval from CER (Clean Energy Regulator), NGER APIs, and ABS (Census data).
* **Geospatial Augmentation:** Enriched 5,000+ facility records with precise coordinates using the **Google Geocoding API** and caching strategies.
* **Data Warehouse Modeling:** Designed a **Star Schema** in DuckDB handling temporal, spatial, and economic dimensions.

## 2. Tech Stack
* **Language:** Python 3.9+
* **ETL & Data Processing:** Pandas, NumPy, Requests
* **Geospatial Engineering:** GeoPandas, Google Maps API, GeoAlchemy2
* **Database & SQL:** DuckDB (with Spatial Extension), JupySQL
* **Visualization:** Matplotlib, Seaborn

## 3. Architecture & Workflow

### Step 1: Data Extraction
* **NGER API:** Iteratively fetched 10 years of emission and production data.
* **CER Data:** Scraped datasets for Accredited, Committed, and Probable renewable power stations.
* **ABS Data:** Ingested regional economic indicators (Business Entries/Exits) via Excel.

### Step 2: Transformation & Cleaning
* **Entity Resolution:** Implemented logic to handle "Corporate Total" vs. "Facility" reporting discrepancies.
* **Deduplication:** Resolved complex Joint Venture reporting issues to prevent double-counting of emissions.
* **Geocoding:** Converted raw addresses to Latitude/Longitude coordinates using Google Maps API with a local cache to optimize API quota usage.

### Step 3: Loading & Modeling (Star Schema)
The data is loaded into DuckDB using a Star Schema design to support efficient OLAP queries:

* **Fact Table:** `elec.energy` (Production, Emissions)
* **Dimension Tables:**
    * `elec.facility` (Static attributes, Geospatial location)
    * `elec.time` (Financial year windows)
    * `elec.economy` (State-level economic indicators)
    * `elec.grid` (Connection status)


<img width="500" alt="database schema" src="https://github.com/user-attachments/assets/4c57dbea-c050-4b6b-8fa9-a4bbd53691b1" />


## 4. My Contributions (Group Project)
This project was developed as a collaborative effort. **My core contributions focused on the Extraction and Transformation pipeline:**

* **Pipeline Architecture:** Developed the Python scripts to fetch and normalize heterogeneous data sources (API + CSV + Excel).
* **Complex Data Cleaning:** Engineered the logic to standardize facility names and resolve Joint Venture duplicates, ensuring high data integrity for the downstream warehouse.
* **Integration Strategy:** Designed the merge logic to align state-level economic data with point-level energy facility data.

*Collaborators handled the Geocoding API implementation and final SQL schema definitions.*

## 5. Setup & Usage

### Prerequisites
* Python 3.8+
* A Google Cloud API Key (for Geocoding)

### Installation
1.  Clone the repository:
    ```bash
    git clone [https://github.com/Constance-K/Australian-Energy-ETL-Pipeline.git](https://github.com/Constance-K/Australian-Energy-ETL-Pipeline.git)
    ```
2.  Install dependencies:
    ```bash
    pip install -r requirements.txt
    ```
3.  **Configuration:**
    * Open `energy_etl_pipeline.ipynb`.
    * Replace `YOUR_GOOGLE_API_KEY_HERE` with your actual Google Maps API Key.

### Running the Pipeline
Run the Jupyter Notebook. The script will:
1.  Initialize `raw_data/` and `clean_data/` directories.
2.  Execute the extraction and cleaning scripts.
3.  Generate the `australian_energy.db` DuckDB file with the fully populated Star Schema.
