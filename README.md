# How to Use This Program
This Python program is designed to read CSV files from a specified directory, transform the data, and load it into a PostgreSQL database. It assumes a schema configuration and requires specific environment variables to function. Below is a detailed guide on how to use the program.

## Requirements

1. **Python Libraries**: Ensure you have the required Python libraries installed:
   - `pandas`
   - `psycopg2`
   - `glob`
   - `json`
   - `re`
   - `sys`
   - `os`

   You can install the required libraries using pip:
   ```bash
   pip install pandas psycopg2
   

### Environment Variables 

SRC_BASE_DIR - Base directory where CSV files and schema file are located.

DB_HOST - Hostname of the PostgreSQL server.

DB_PORT - Port number of the PostgreSQL server.

DB_NAME - Name of the PostgreSQL database.

DB_USER - Username for the PostgreSQL database.

DB_PASS - Password for the PostgreSQL database.

### Program Structure
Schema File: The program expects a schemas.json file in the base directory specified by SRC_BASE_DIR. This file should define the schema of your CSV files.

CSV Files: Place your CSV files in subdirectories within SRC_BASE_DIR. You need to adjust for any file name prefixes in the db_loader function and should be organized under directories named after the dataset (ds) names.

Usage
1. Direct Execution
To run the program, simply execute the script. It will process all datasets defined in the schemas.json file:
```
bash
python app.py
```
2. Specifying Datasets
If you want to process only specific datasets, you can pass a JSON-encoded list of dataset names as a command-line argument:

bash
Copy code
python app.py '["dataset1", "dataset2"]'
Replace dataset1 and dataset2 with the names of your datasets as specified in the schemas.json file.

### Description of Functions
get_column_names(schemas, ds_name, sorting_key='column_position'): Retrieves and sorts the column names for a given dataset based on the schema.

read_csv(file, schemas): Reads a CSV file, applying the schema to ensure correct column names and chunk sizes for processing.

to_sql(df, db_conn_uri, ds_name): Loads a DataFrame into the PostgreSQL database.

db_loader(src_base_dir, db_conn_uri, ds_name): Handles the loading of CSV files for a specific dataset into the database.

process_files(ds_names=None): Main function that coordinates the processing of datasets. It sets up the database connection and handles exceptions.

### Error Handling
If no files are found for a dataset, a NameError is raised.
Any other exceptions during processing will be caught and printed.
Example
Prepare your environment:
```
bash

export SRC_BASE_DIR=/path/to/data
export DB_HOST=localhost
export DB_PORT=5432
export DB_NAME=mydatabase
export DB_USER=myuser
export DB_PASS=mypassword
```

Run the script:
```
bash
python app.py
```

Or, to process specific datasets:
```
bash
python app.py '["dataset1", "dataset2"]'
```
Ensure your schemas.json is correctly configured and placed in the specified base directory for the program to operate correctly.

