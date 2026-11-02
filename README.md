# Airflow Lab 1: K-Means Clustering Pipeline

An Apache Airflow DAG (`Airflow_Lab1`) that runs an ML workflow in Docker:

1. `load_data_task`: load `dags/data/file.csv`
2. `data_preprocessing_task`: clean and scale features (MinMaxScaler)
3. `build_save_model_task`: fit K-Means for a range of k, save the model to `dags/model/model.sav`
4. `load_model_task`: load the model and pick the optimal k with the elbow method (`kneed`)

## Run it

Requires Docker Desktop (4GB+ memory).

```bash
echo -e "AIRFLOW_UID=$(id -u)" > .env      # Windows: AIRFLOW_UID=50000
mkdir -p logs plugins config
docker compose up airflow-init
docker compose up
```

Open http://localhost:8080 (login `airflow2` / `airflow2`), enable and trigger `Airflow_Lab1`.
The optimal number of clusters is printed in the `load_model_task` logs.

Stop with `docker compose down`.

## Changes from the course lab

- `docker-compose.yaml` (Airflow 2.9.2): disabled example DAGs, added `pandas scikit-learn kneed`, added a `working_data` volume, changed admin login.
- `dags/airflow.py`: the course file imports `PythonOperator` from `airflow.providers.standard` (Airflow 3), which fails on the 2.9.2 image. Switched to `airflow.operators.python`.
