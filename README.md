# marquez-airflow

[![CircleCI](https://circleci.com/gh/MarquezProject/marquez-airflow/tree/master.svg?style=shield)](https://circleci.com/gh/MarquezProject/marquez-airflow/tree/master)

A library that integrates Airflow DAGs with Marquez for automatic metadata collection.

# Requirements
 - Python 3.5+
 - apache-airflow 1.10.0+
 - marquez-client
 
 # Installation
 
 ```
pip install marquez-airflow
```
 
 # Usage
 
 Once the library is installed in your system, your current DAGs need to be modified slightly by changing the DAG to a MarquezDAG, see example below:
 
```python
from marquez.airflow import MarquezDag as DAG
from airflow.operators.dummy_operator import DummyOperator
from datetime import datetime


DAG_NAME = 'my_DAG_name'

default_args = {
    'mqz_namespace': 'namespace_1',
    'mqz_location': 'github://data-dags/dag_location/',
    'mqz_input_datasets': ["s3://some_data", "s3://more_data"],
    'mqz_output_datasets': ["s3://output_data"],
    
    'owner': ...,
    'depends_on_past': False,
    'start_date': ...,
}

dag = DAG(DAG_NAME, schedule_interval='*/10 * * * *',
          default_args=default_args, description="yet another DAG")

run_this = DummyOperator(task_id='run_this', dag=dag)
run_this_too = DummyOperator(task_id='run_this_too', dag=dag)
run_this_too.set_upstream(run_this)
```

