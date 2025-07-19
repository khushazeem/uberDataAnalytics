# Uber Data Analytics | Modern Data Engineering GCP Project

![GCP](https://img.shields.io/badge/GCP-4285F4?style=for-the-badge&logo=google-cloud&logoColor=white)
![Mage](https://img.shields.io/badge/Mage.ai-1F174F?style=for-the-badge&logo=mage&logoColor=white)
![BigQuery](https://img.shields.io/badge/BigQuery-669DF6?style=for-the-badge&logo=google-bigquery&logoColor=white)
![Looker](https://img.shields.io/badge/Looker-4285F4?style=for-the-badge&logo=looker&logoColor=white)

This project dives into the world of Uber data analytics using modern data engineering practices on Google Cloud Platform (GCP). We'll build an end-to-end ETL pipeline to process, warehouse, and visualize Uber trip data. The project leverages **Mage.ai** for building a robust ETL pipeline, **Google Cloud Storage** for raw data storage, **BigQuery** for data warehousing, and **Looker Studio** for creating insightful dashboards.

---

## Project Architecture

The project follows a modern data stack architecture, ensuring scalability and maintainability.

1.  **Data Ingestion**: Raw Uber trip data (in CSV or Parquet format) is uploaded to a Google Cloud Storage (GCS) bucket.
2.  **ETL Pipeline (Mage)**: A Mage pipeline is triggered to orchestrate the data flow.
    * **Extract**: The pipeline's Data Loader fetches the raw data from the GCS bucket.
    * **Transform**: A Transformer script cleans the data, performs type casting, removes duplicates, and engineers new features (e.g., calculating trip duration).
    * **Load**: The transformed, clean data is loaded into a structured table in Google BigQuery.
3.  **Data Warehousing**: Google BigQuery serves as our data warehouse, storing the structured data and enabling fast, complex SQL queries for analysis.
4.  **Data Visualization**: Google Looker Studio connects directly to BigQuery, providing a user-friendly interface to build interactive dashboards and reports for business intelligence.

<p align="center">
  <img src="path/to/your/architecture_diagram.png" alt="Project Architecture Diagram" width="800"/>
</p>
<p align="center">
  <em>(Suggestion: Create a simple diagram and add it here)</em>
</p>

---

## Technologies Used

* **Cloud Provider**: Google Cloud Platform (GCP)
* **ETL Orchestration**: [Mage.ai](https://www.mage.ai/)
* **Data Lake / Storage**: Google Cloud Storage (GCS)
* **Data Warehouse**: Google BigQuery
* **Data Visualization**: Google Looker Studio
* **Programming Language**: Python

---

## Dataset

This project uses the publicly available **TLC Trip Record Data** from New York City. The dataset contains detailed, anonymized information about trips taken in yellow and green taxis and for-hire vehicles. You can download the data from the [official website](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page).

For this project, we will be using the Parquet format for efficiency.

---

## Setup and Configuration

Follow these steps to set up the project environment on your local machine and GCP.

### 1. Prerequisites
* A Google Cloud Platform (GCP) account with billing enabled.
* [gcloud CLI](https://cloud.google.com/sdk/docs/install) installed and authenticated.
* [Git](https://git-scm.com/downloads) installed.
* [Python 3.9+](https://www.python.org/downloads/) installed.

### 2. GCP Configuration
1.  **Create a GCP Project**: If you don't have one already, create a new project in the GCP Console.
2.  **Enable APIs**: Enable the following APIs for your project:
    * `IAM API`
    * `Cloud Storage API`
    * `BigQuery API`
3.  **Create a Service Account**:
    * Navigate to `IAM & Admin` > `Service Accounts`.
    * Create a new service account with the following roles:
        * `Storage Admin` (for GCS access)
        * `BigQuery Admin` (for BigQuery access)
    * Create and download a JSON key for this service account. **Keep this key secure!**

### 3. Project Setup
1.  **Clone the Repository**:
    ```bash
    git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
    cd your-repo-name
    ```
2.  **Set up GCS Bucket**:
    * Create a new GCS bucket in your GCP project to store the raw data.
    * Upload the downloaded TLC Trip Record data to this bucket.
3.  **Set up BigQuery**:
    * Create a new **Dataset** in BigQuery. This dataset will hold the table created by our ETL pipeline.
4.  **Set up Mage**:
    * Install Mage.ai:
      ```bash
      pip install mage-ai
      ```
    * Initialize a new Mage project:
      ```bash
      mage init uber_data_project
      ```
    * Copy the pipeline files from this repository into your new `uber_data_project` directory.
    * **Configure GCP Credentials for Mage**:
        * Create a file named `io_config.yaml` inside your Mage project directory (`uber_data_project/io_config.yaml`).
        * Add the following configuration, pointing to your GCP Project ID and the path to your service account JSON key:
          ```yaml
          version: 0.1.0
          default:
            GOOGLE_SERVICE_ACC_KEY_FILEPATH: "/path/to/your/gcp-credentials.json"
            GOOGLE_PROJECT_ID: "your-gcp-project-id"
          ```
    * Start Mage:
      ```bash
      mage start uber_data_project
      ```
    * Open your browser to `http://localhost:6789` to access the Mage UI.

---

## ETL Pipeline Walkthrough

The core of this project is the Mage pipeline. You can view and edit it from the Mage UI.

### 1. Data Loader (`data_loader.py`)
This block connects to your GCS bucket and loads the specified Uber dataset file into a Pandas DataFrame.

### 2. Transformer (`transformer.py`)
This block performs the data cleaning and feature engineering. Key transformations include:
* Converting `tpep_pickup_datetime` and `tpep_dropoff_datetime` to datetime objects.
* Filtering out trips with zero passengers.
* Creating a `trip_id` column for a unique identifier.
* Renaming columns to follow a consistent `snake_case` format.

### 3. Data Exporter (`data_exporter.py`)
This final block takes the transformed DataFrame and loads it into your designated BigQuery table. It handles the schema creation and ensures data is correctly partitioned and clustered for optimized query performance.

---

## Data Visualization in Looker Studio

Once the data is in BigQuery, you can connect Looker Studio to create an interactive dashboard.

1.  Open Looker Studio and create a new **Data Source**.
2.  Select the **BigQuery** connector.
3.  Authorize access and choose your GCP project, dataset, and the table created by the Mage pipeline.
4.  Build charts and scorecards to visualize key metrics, such as:
    * Total number of trips over time.
    * Average fare amount by day of the week.
    * Most popular pickup and dropoff locations.
    * Relationship between trip distance and fare amount.

<p align="center">
  <em>(Suggestion: Add a screenshot of your final dashboard here)</em>
</p>
<p align="center">
  <img src="path/to/your/dashboard_screenshot.png" alt="Looker Studio Dashboard" width="800"/>
</p>

---

## Contributing

Contributions are welcome! If you'd like to contribute, please fork the repository and create a pull request. You can also open an issue with the tag "enhancement".

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request

---

## License

This project is licensed under the MIT License - see the `LICENSE` file for details.

---

## Acknowledgements
* Thanks to the NYC Taxi and Limousine Commission (TLC) for making the trip data publicly available.
* The team at [Mage.ai](https://www.mage.ai/) for creating an excellent open-source data pipeline tool.
