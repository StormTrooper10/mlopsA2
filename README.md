# Airflow Data Extraction Pipeline
This project automates the lifecycle of data extraction, transformation, and versioning, utilizing Apache Airflow for orchestration, DVC for data versioning, and Git for code versioning.

## How To Run?
- Ensure necessary libraries are installed by referencing `requirements.txt`.
- Install Apache Airflow in a Python virtual environment with the following command:
```
pip install apache-airflow
```

- Set the Airflow environment variable by executing:
```
export AIRFLOW_HOME="root of this cloned repo"
```

- Initialize the Airflow database with:
```
airflow db init
```

- Run the scheduler to track DAG changes in a separate console:
```
airflow scheduler
```

- Start the Airflow webserver in a separate console and navigate to `http://localhost:8080`:
```
airflow webserver -p 8080
```

- Locate the DAG specified in the code within the Airflow UI and activate it by toggling the pause button.
- Trigger the DAG run manually by clicking the play button in the top right corner.

## DAG Documentation

### Extract Task:
This task extracts data from a list of URLs using the `extract_data` function. Each URL is processed sequentially, and the extracted data is aggregated.

### Preprocess Task:
The extracted data undergoes preprocessing using the `clean_data` function to ensure consistent formatting and cleanliness.

### Save Task:
This task saves the preprocessed data to a CSV file specified by the filename. Data is structured with 'id', 'title', 'description', and 'source' columns.

### DVC Push Task:
It adds the CSV file (`data/extracted.csv`) to the DVC repository using the `dvc add` command and pushes the changes to remote Google Drive storage via `dvc push`.

### Git Push Task:
This task handles Git operations to push the changes made to the Git repository. It includes commands for pulling changes, adding files, committing changes, and pushing to the remote repository.

## DAG Execution Order
Tasks are executed sequentially following this order:

`extract_task >> preprocess_task >> save_task >> dvc_push_task >> git_push_task`

The DAG is configured for manual execution (`schedule=None`) and lacks a specific schedule for automatic execution.

## Encountered Challenges
#### Challenge: Modifying Airflow configuration to detect DAGs in locations outside the Airflow folder.
Solution: Create a `dags` directory in the current folder, place DAGs there, and set the environment variable `export AIRFLOW_HOME="current folder"`.

#### Challenge: Installing Airflow in a Python virtual environment.
Solution: Execute `pip install apache-airflow`.
