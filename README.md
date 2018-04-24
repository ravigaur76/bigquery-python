# bigquery-python
Simple Python client for interacting with Google BigQuery

Basically there are 2 types for interacting with Google BigQuery using python.
  1. BigQuery client libraries.
  2. BigQuery get client.
  
## 1. BigQuery client libraries
#### Install Dependencies
  Install [pip](https://pip.pypa.io/en/stable/) and [virtualenv](https://virtualenv.pypa.io/en/stable/) if you do not already have them.

#### Instalation
```
pip install --upgrade google-cloud-bigquery
```    
#### Setting up authentication.</br>
1. Go to the Create service account key page in the GCP Console using below link.</br>
    https://console.cloud.google.com/apis/credentials/serviceaccountkey</br>
2. From the Service account drop-down list, select New service account.</br>
3. Enter a name into the Service account name field.</br>
4. From the Role drop-down list, select Project > Owner.</br>
5. Click Create. A JSON file that contains your key downloads to your computer.</br>
6. Set the environment variable GOOGLE_APPLICATION_CREDENTIALS to the file path of the JSON file that contains your service account key like below path.</br>
   export GOOGLE_APPLICATION_CREDENTIALS="[PATH]" </br>
   Replace [PATH] with the file path of the JSON file that contains your service account key.
   
#### Basic Usage
```
# Imports the Google Cloud client library
from google.cloud import bigquery
import uuid

# Instantiates a client
bigquery_client = bigquery.Client()

# The name for the new dataset
dataset_id = 'my_new_dataset'

# Submit an async query
query_execute = bigquery_client.run_async_query(str(uuid.uuid4()), 'select * from abc')

# Use standard SQL syntax.
query_execute.use_legacy_sql = False

# Wait for query to finish.
query_execute.begin()
query_execute.result()

# fetch rows. 
rows = query_execute.query_results().fetch_data()

# print rows.
for row in rows:
    print(row)



```
   
