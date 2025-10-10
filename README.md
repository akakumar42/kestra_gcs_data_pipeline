# kestra_gcs_data_pipeline

This project leverages [Kestra](https://kestra.io/) to automate an end-to-end ETL (Extract, Transform, Load) pipeline for the NYC Taxi dataset, integrating with Google Cloud Platform (GCP) services for scalable data storage and analytics.

## Overview

The pipeline automates the following steps:
- **Data Extraction:** Downloads monthly NYC Taxi CSV data (yellow or green taxi) from the [DataTalksClub public repository](https://github.com/DataTalksClub/nyc-tlc-data/releases).
- **Cloud Storage:** Uploads the extracted data to a Google Cloud Storage (GCS) bucket.
- **Data Warehousing:** Loads and merges the data into partitioned tables in Google BigQuery, supporting both yellow and green taxi schemas.
- **Transformation:** Runs [dbt](https://www.getdbt.com/) models on BigQuery for advanced data transformation and analytics, orchestrated via the new `gcp_dbt.yaml` Kestra flow.
- **Parameterization:** Allows users to select taxi type, year, and month for flexible data ingestion.
- **Resource Provisioning:** Automates the creation of GCS buckets and BigQuery datasets if they do not exist.
- **Configuration Management:** Uses Kestra key-value pairs for managing GCP credentials, project IDs, dataset names, and bucket names.
- **Scheduling:** Supports automated, scheduled ETL runs using the `gcp_taxi_schedule.yaml` flow.

## Project Structure

- [`gcp_setup.yaml`](gcp_setup.yaml): Kestra flow to provision GCS buckets and BigQuery datasets.
- [`set_kv_pair.yaml`](set_kv_pair.yaml): Sets up required GCP configuration as key-value pairs in Kestra.
- [`gcp_taxi.yaml`](gcp_taxi.yaml): Main ETL pipeline flow for extracting, uploading, and loading NYC Taxi data.
- [`gcp_taxi_schedule.yaml`](gcp_taxi_schedule.yaml): Scheduled Kestra flow to automate periodic ETL runs.
- [`gcp_dbt.yaml`](gcp_dbt.yaml): Kestra flow to orchestrate dbt transformations on BigQuery after data ingestion.
- `README.md`: Project documentation (this file).

## How It Works

1. **Setup GCP Resources**
   - Run [`gcp_setup.yaml`](gcp_setup.yaml) to create the required GCS bucket and BigQuery dataset.
   - Use [`set_kv_pair.yaml`](set_kv_pair.yaml) to configure your GCP project, location, bucket, and dataset.

2. **Run the ETL Pipeline**
   - Execute [`gcp_taxi.yaml`](gcp_taxi.yaml) to:
     - Select taxi type, year, and month.
     - Download the corresponding CSV file.
     - Upload it to GCS.
     - Create or update partitioned tables in BigQuery.
     - Merge new data, ensuring no duplicates via unique row IDs.

3. **Transform Data with dbt**
   - Use [`gcp_dbt.yaml`](gcp_dbt.yaml) to run dbt models on your BigQuery data.
   - This enables advanced analytics, data cleansing, and business logic transformations in a modular, version-controlled way.

4. **Automate with Scheduling**
   - Use [`gcp_taxi_schedule.yaml`](gcp_taxi_schedule.yaml) to schedule ETL runs (e.g., monthly or as needed).
   - This enables hands-off, recurring data ingestion and processing.

5. **Customization**
   - All GCP resource names and credentials are managed via Kestra key-value pairs for easy reconfiguration.
   - The pipeline supports both yellow and green taxi schemas, handling their differences automatically.
   - dbt models can be extended or customized to fit your analytics needs.

## Prerequisites

- [Kestra](https://kestra.io/) installed and configured.
- A Google Cloud Platform project with permissions for GCS and BigQuery.
- Service account credentials for GCP (set as `GCP_CRED` in Kestra).
- [dbt](https://docs.getdbt.com/docs/introduction) project configured for BigQuery (for use with `gcp_dbt.yaml`).

## Getting Started

1. **Clone this repository**
2. **Set your GCP configuration** in [`set_kv_pair.yaml`](set_kv_pair.yaml).
3. **Provision resources** by running [`gcp_setup.yaml`](gcp_setup.yaml).
4. **Run the ETL pipeline** using [`gcp_taxi.yaml`](gcp_taxi.yaml) or schedule it with [`gcp_taxi_schedule.yaml`](gcp_taxi_schedule.yaml).
5. **Run dbt transformations** using [`gcp_dbt.yaml`](gcp_dbt.yaml) after data ingestion.

## Notes

- Ensure your GCS bucket name is globally unique.
- Update the `GCP_PROJECT_ID` and other key-value pairs to match your GCP environment.
- The pipeline is modular and can be extended for additional data sources or transformations.
- Scheduled flows help automate data freshness and reduce manual intervention.
- dbt integration enables robust, testable, and maintainable analytics workflows.

## References

- [NYC TLC Data](https://github.com/DataTalksClub/nyc-tlc-data/releases)
- [Kestra Documentation](https://kestra.io/docs/)
- [Google Cloud Storage](https://cloud.google.com/storage)
- [Google BigQuery](https://cloud.google.com/bigquery)
- [dbt Documentation](https://docs.getdbt.com/)

---

*Crafted with automation and scalability in mind, this project aims to make cloud-based data engineering accessible and reproducible for all.*
