# GCP Metric Anomaly Detector

A command-line tool written in Go to monitor Google Cloud Platform (GCP) metrics and detect anomalies using statistical analysis (Z-score).

This tool connects to your GCP project, fetches historical and recent data for specified metrics, and flags any recent data points that deviate significantly from the historical norm.

## Key Features

- **Configurable Monitoring:** Specify which metrics to watch using a simple `config.yaml` file.
- **Statistical Detection:** Uses Z-score to statistically determine if a metric value is an anomaly.
- **Secure Authentication:** Leverages Google's Application Default Credentials (ADC) for secure, keyless authentication.
- **Lightweight & Fast:** Built with Go for efficient performance.

## Technology Stack

- **Language:** [Go](https://go.dev/)
- **Platform:** [Google Cloud Platform (GCP)](https://cloud.google.com/)
- **API:** [Google Cloud Monitoring API](https://cloud.google.com/monitoring/api)

## Getting Started

Follow these instructions to get a copy of the project up and running on your local machine.

### Prerequisites

You must have the following software installed on your machine:

1.  [**Go**](https://go.dev/dl/) (version 1.18 or higher)
2.  [**Google Cloud CLI**](https://cloud.google.com/sdk/docs/install)

### Installation & Usage

1.  **Clone the repository:**

    ```bash
    git clone [https://github.com/Shivang139/GCP-Metric-Anomaly-Detector.git](https://github.com/Shivang139/GCP-Metric-Anomaly-Detector.git)
    cd GCP-Metric-Anomaly-Detector
    ```

2.  **Set up your GCP Project:**

    - Create a new project in the Google Cloud Console.
    - Enable the **Cloud Monitoring API** for your project.
    - Ensure your project is linked to an active **Billing Account**.

3.  **Authenticate with GCP:**

    - Run the following command in your terminal. This will open a browser window for you to log in and grant permissions.

    ```bash
    gcloud auth application-default login
    ```

4.  **Configure the Application:**

    - Open the `config.yaml` file.
    - Set your `project_id` to your Google Cloud Project ID.
    - Customize the `metrics`, `filters`, and other parameters as needed.

5.  **Build and Run:**

    - **Build the executable:**
      ```bash
      go build
      ```
    - **Run the detector:**

      ```bash
      # For Windows
      .\gcp-anomaly-detector.exe

      # For macOS/Linux
      ./gcp-anomaly-detector
      ```

## Configuration (`config.yaml`)

The behavior of the detector is controlled by the `config.yaml` file.

```yaml
metrics:
  - '[custom.googleapis.com/otel/foo_connection_count](https://custom.googleapis.com/otel/foo_connection_count)'
  - '[custom.googleapis.com/otel/foo_current_connections](https://custom.googleapis.com/otel/foo_current_connections)'
filters:
  [custom.googleapis.com/otel/foo_connection_count](https://custom.googleapis.com/otel/foo_connection_count): 'resource.type="generic_task" AND metric.labels."environment"="production"'
  [custom.googleapis.com/otel/foo_current_connections](https://custom.googleapis.com/otel/foo_current_connections): 'resource.type="generic_task" AND metric.labels."environment"="production"'
polling_time: 60       # Polling time in seconds
project_id: your-gcp-project-id # GCP Project ID
baseline_duration: 7   # Baseline duration in days
recent_duration: 60    # Recent metrics duration in minutes
z_score_threshold: 3.00 # Z-score threshold for anomaly detection
```

- **`metrics`**: A list of GCP metric types to monitor.
- **`filters`**: Specific filters to apply to each metric query for more granular selection.
- **`polling_time`**: The interval in seconds at which the tool should check for new data.
- **`project_id`**: Your unique Google Cloud Project ID.
- **`baseline_duration`**: The lookback period in days to establish what "normal" behavior is.
- **`recent_duration`**: The lookback period in minutes for recent data to check against the baseline.
- **`z_score_threshold`**: The number of standard deviations a data point must be from the mean to be considered an anomaly. A value of `3.0` is a common starting point.

## 🛡️ Authentication

This project uses **Application Default Credentials (ADC)** for authentication. The program automatically finds credentials set up by the `gcloud` CLI.

**DO NOT** store any JSON service account keys or other secrets in this repository. Ensure your `.gitignore` file is configured to exclude `*.json` files.

## License

This project is licensed under the MIT License - see the `LICENSE.md` file for details.
