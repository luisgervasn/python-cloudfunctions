# Python Cloud Functions

Author: Juan Vicente Herrera Ruiz de Alejo

GitHub: @juanviz

Affiliation: ICAI


This repository contains example Python code designed to run on Google Cloud Functions. It includes two sample functions: one triggered by Cloud Storage events and another that responds to HTTP requests.

## Functions

### 1. `process_file`
- **Trigger**: Cloud Storage upload event
- **Description**: This function is triggered when a file is uploaded to a specified Cloud Storage bucket. It processes text files (`.txt`) by downloading and printing their content. For other file types, it simply logs the upload.
- **Use Case**: Automate processing of uploaded files, such as data ingestion or file validation.

### 2. `http_hello_world`
- **Trigger**: HTTP request
- **Description**: A simple HTTP function that accepts a JSON payload with a `name` field and responds with a greeting. If no name is provided, it defaults to "World".
- **Use Case**: Basic web API endpoint for testing or simple services.

## Prerequisites

- Google Cloud Platform (GCP) account
- Google Cloud SDK (gcloud) installed and configured
- A Cloud Storage bucket (for the `process_file` function)
- Python 3.7+ (for local testing)

## Dependencies

The required Python packages are listed in `requirements.txt`:

- `google-cloud-storage`: For interacting with Google Cloud Storage
- `flask`: For handling HTTP requests (used by Cloud Functions runtime)

## Deployment

### Deploy `process_file` (Cloud Storage Trigger)

1. Ensure you have a Cloud Storage bucket created in your GCP project.

2. Deploy the function using the gcloud CLI:

   ```bash
   gcloud functions deploy process_file \
     --runtime python39 \
     --trigger-resource YOUR_BUCKET_NAME \
     --trigger-event google.storage.object.finalize \
     --entry-point process_file \
     --source . \
     --allow-unauthenticated
   ```

   Replace `YOUR_BUCKET_NAME` with your actual bucket name.

### Deploy `http_hello_world` (HTTP Trigger)

1. Deploy the function using the gcloud CLI:

   ```bash
   gcloud functions deploy http_hello_world \
     --runtime python39 \
     --trigger-http \
     --entry-point http_hello_world \
     --source . \
     --allow-unauthenticated
   ```

2. After deployment, note the function URL provided in the output.

## Usage

### `process_file`
- Upload a file to your Cloud Storage bucket.
- Check the Cloud Functions logs to see the processing output.

### `http_hello_world`
- Send an HTTP GET or POST request to the function URL.
- For POST with JSON: `{"name": "Your Name"}`
- Response: `"Hello, Your Name from Cloud Functions!"`

## Local Testing

For local development, you can use the Cloud Functions Framework:

1. Install the framework: `pip install functions-framework`

2. Run the HTTP function locally:
   ```bash
   functions-framework --target http_hello_world --signature-type http
   ```

3. For the storage function, testing locally requires mocking events, which is more complex.

## Contributing

Feel free to modify the functions or add new ones. Ensure to update `requirements.txt` if new dependencies are added.

## License

This project is licensed under the MIT License. See MIT License for details.
