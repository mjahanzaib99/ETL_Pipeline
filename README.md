
# ETL Pipeline, MongoDB Loader, and Scheduler (Google Drive Version)

This repository contains three main components:
1. **ETL Pipeline** (`etl_pipeline.ipynb`)
2. **MongoDB Loader** (`load_to_db.ipynb`)
3. **Scheduler** (`scheduler.py`)

The files are designed to work with data stored on **Google Drive**, and this version requires mounting Google Drive to access and save data.

### 1. **ETL Pipeline (`etl_pipeline.ipynb`)**
The `etl_pipeline.ipynb` notebook fetches stock data from multiple sources (Alpha Vantage API, Google Sheets, JSON, CSV, and BigQuery), cleans the data, and merges it into a single DataFrame. It then exports the final data into a CSV file stored on **Google Drive**.

**Key Features:**
- Data is pulled from:
  - **Alpha Vantage API** (for intraday stock data)
  - **Google Sheets** (using `gspread` library)
  - **JSON** (stock data in JSON format)
  - **CSV** (Yahoo stock data)
  - **BigQuery** (Google Cloud BigQuery stock data)
- Data is cleaned and transformed:
  - Conversion of date columns to UTC timezone (ISO 8601 format).
  - Numerical columns are converted to appropriate types (float, integer).
  - Missing values are handled via forward-fill or mean imputation.
- Finally, it outputs the cleaned data to a CSV file saved directly in your Google Drive.

### 2. **MongoDB Loader (`load_to_db.ipynb`)**
The `load_to_db.ipynb` notebook takes the cleaned stock data (which is saved in Google Drive) and uploads it to a MongoDB database. It uses the `pymongo` library to perform the insertion.

**Key Features:**
- Loads cleaned data from a CSV file stored on **Google Drive**.
- Connects to MongoDB using `pymongo` and uploads the data into a specified collection (`5_stock_data`).
- Supports connection to a cloud-based MongoDB service.

### 3. **Scheduler (`scheduler.py`)**
The `scheduler.py` script schedules the execution of both the **ETL pipeline** and **MongoDB loader** notebooks. It uses the `schedule` library to run the notebooks at specific intervals, such as daily at midnight.

**Key Features:**
- Runs the **ETL pipeline** and **MongoDB loader** notebooks automatically on a scheduled basis.
- Logs the status of each job to a log file on **Google Drive** for monitoring and debugging.

### How to Use

1. **Set Up Your Environment**
   Ensure that you have Python installed on your system. Then, install the necessary dependencies using the `requirements.txt` file.

   ```bash
   pip install -r requirements.txt
   ```

2. **Mount Google Drive**
   To work with Google Drive, you need to mount it so that you can access your files and save the output data.

   - **In Google Colab**:
     If you are running this project in Google Colab, you can easily mount Google Drive using the following code in the first cell:

     ```python
     from google.colab import drive
     drive.mount('/content/drive')
     ```

     - After running the code, you'll be asked to authenticate and grant access to Google Drive.

3. **Configure Credentials**
   - **Google Sheets**: Ensure your Google credentials are properly configured to access Google Sheets. You may need to enable the Google Sheets API and authenticate using OAuth2.
   - **MongoDB**: Provide the connection string for your MongoDB database. Ensure that the MongoDB URI is correctly configured in the script.
   - **Alpha Vantage API**: Insert your **Alpha Vantage API Key** in the `etl_pipeline.ipynb` file.

4. **Running the Scheduler**
   - The scheduler runs the **ETL pipeline** and **MongoDB loader** notebooks automatically at the specified time.
   - To start the scheduler, simply run the `scheduler.py` script:

   ```bash
   python scheduler.py
   ```

   This will continuously check the schedule and run the jobs as configured.

5. **Logs**
   - The scheduler logs the status of each task to a log file stored on Google Drive, located at:
   `/content/drive/My Drive/ETL_Pipeline_Muhammad Waqas Ali_DS-019_2023-24/logs/etl_scheduler.log`.

6. **File Structure**
```
├── etl_pipeline.ipynb        # ETL Pipeline Jupyter notebook
├── load_to_db.ipynb          # MongoDB Loader Jupyter notebook
├── scheduler.py              # Scheduler script to run the jobs automatically
├── requirements.txt          # Dependencies for the project
├── logs/                     # Logs directory to store scheduler logs
│   └── etl_scheduler.log     # Log file for scheduler
├── /content/drive/My Drive/ETL_Pipeline_Muhammad Waqas Ali_DS-019_2023-24/ # Google Drive directory for data
```

### Dependencies
This project requires the following Python packages:
- pandas
- requests
- gspread
- gspread-dataframe
- oauth2client
- google-auth
- google-auth-oauthlib
- google-api-python-client
- pymongo
- schedule
- google-cloud-bigquery
- nbconvert

These dependencies can be installed using the following command:

```bash
pip install -r requirements.txt
```

### License
This project is licensed under the MIT License.

---

**Note**: Make sure to replace the placeholder values like MongoDB URI, API keys, and file paths with your actual values to ensure the scripts work correctly. Also, ensure your Google Drive is mounted to access the necessary files and save output data.

---
