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
   
#### Table operation in bigquery
In bigquery we can manage dataset tables, including creating, deleting, checking the existence, and getting the metadata of tables.

```
# Listing tables
dataset_ref = bigquery_client.dataset('dataset_name')
tables = list(bigquery_client.list_tables(dataset_ref))
for table in tables:
    print(table)

# Fetch table refrence
dataset = bigquery_client.dataset('dataset_name')
table = dataset.table('table_name')

# Check if a table exists
print(table.exists()) #return True if table found

# Delete an existing table
print(table.delete()) #return True if table deleted

# Create an empty table without a schema definition
print(table.create())

# Create an empty table with a schema definition
schema = [
    bigquery.SchemaField('full_name', 'STRING', mode='REQUIRED'),
    bigquery.SchemaField('age', 'INTEGER', mode='REQUIRED'),
]
table_schema_ref = dataset.table(table_name, schema)
print(table_schema_ref.create())

```

#### Datasets operation in bigquery
```
# Fetch dataset refrence
dataset_ref = bigquery_client.dataset(dataset_id)
dataset = bigquery.Dataset(dataset_ref)

# Creating a dataset
dataset.location = 'US' # Specify the geographic location where the dataset should reside.
dataset = bigquery_client.create_dataset(dataset)

# Listing datasets
datasets = list(bigquery_client.list_datasets())
for dataset in datasets:
  print('\t{}'.format(dataset.dataset_id))
  
# Delete a dataset that does not contain any tables
bigquery_client.delete_dataset(dataset_ref)

# Use the delete_contents parameter to delete a dataset and its tables
client.delete_dataset(dataset_ref, delete_contents=True)
  
