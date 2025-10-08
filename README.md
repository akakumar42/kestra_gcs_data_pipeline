# kestra_gcs_data_pipeline

This project leverages [Kestra](https://kestra.io/) to automate an end-to-end ETL (Extract, Transform, Load) pipeline for the NYC Taxi dataset, integrating with Google Cloud Platform (GCP) services for scalable data storage and analytics.

## Overview

The pipeline automates the following steps:
- **Data Extraction:** Downloads monthly NYC Taxi CSV data (yellow or green taxi) from the [DataTalksClub public repository](https://github.com/DataTalksClub/nyc-tlc-data/releases).
- **Cloud Storage:** Uploads the extracted data to a Google Cloud Storage (GCS) bucket.
- **Data Warehousing:** Loads and merges the data into partitioned tables in Google BigQuery, supporting both yellow and green taxi schemas.
- **Parameterization:** Allows users to select taxi type, year, and month for flexible data ingestion.
- **Resource Provisioning:** Automates the creation of GCS buckets and BigQuery datasets if they do not exist.
- **Configuration Management:** Uses Kestra key-value pairs for managing GCP credentials, project IDs, dataset names, and bucket names.

## Project Structure

- [`gcp_setup.yaml`](gcp_setup.yaml): Kestra flow to provision GCS buckets and BigQuery datasets.
- [`set_kv_pair.yaml`](set_kv_pair.yaml): Sets up required GCP configuration as key-value pairs in Kestra.
- [`gcp_taxi.yaml`](gcp_taxi.yaml): Main ETL pipeline flow for extracting, uploading, and loading NYC Taxi data.
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

3. **Customization**
   - All GCP resource names and credentials are managed via Kestra key-value pairs for easy reconfiguration.
   - The pipeline supports both yellow and green taxi schemas, handling their differences automatically.

## Prerequisites

- [Kestra](https://kestra.io/) installed and configured.
- A Google Cloud Platform project with permissions for GCS and BigQuery.
- Service account credentials for GCP (set as `GCP_CRED` in Kestra).

## Getting Started

1. **Clone this repository**
2. **Set your GCP configuration** in [`set_kv_pair.yaml`](set_kv_pair.yaml).
3. **Provision resources** by running [`gcp_setup.yaml`](gcp_setup.yaml).
4. **Run the ETL pipeline** using [`gcp_taxi.yaml`](gcp_taxi.yaml).

## Notes

- Ensure your GCS bucket name is globally unique.
- Update the `GCP_PROJECT_ID` and other key-value pairs to match your GCP environment.
- The pipeline is modular and can be extended for additional data sources or transformations.

## References

- [NYC TLC Data](https://github.com/DataTalksClub/nyc-tlc-data/releases)
- [Kestra Documentation](https://kestra.io/docs/)
- [Google Cloud Storage](https://cloud.google.com/storage)
- [Google BigQuery](https://cloud.google.com/bigquery)

---

*Crafted with automation and scalability in mind, this project aims to make cloud-based data engineering accessible and reproducible for all.*
