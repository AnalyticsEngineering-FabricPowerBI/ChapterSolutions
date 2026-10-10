from airflow import DAG
from airflow.operators.python import PythonOperator
from datetime import datetime

def step1_task():
    print("Run a pipeline.")

def step2_task():
    print("Run a notebook.")

def step3_task():
    print("Refresh a Power BI model.")

with DAG(
    dag_id="Sample_DAG",
    start_date=datetime(2026, 1, 1),
    schedule_interval="@daily",
    catchup=False
) as dag:

    step1 = PythonOperator(task_id="step1_task", python_callable=step1_task)
    step2 = PythonOperator(task_id="step2_task", python_callable=step2_task)
    step3 = PythonOperator(task_id="step3_task", python_callable=step3_task)

    step1 >> step2 >> step3
