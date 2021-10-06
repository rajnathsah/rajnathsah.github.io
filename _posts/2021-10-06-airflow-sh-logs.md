---
title: Automate airflow scheduler log cleanup.
date: 2021-10-06 00:00:00 +0000
description: Automate airflow scheduler log clean up using scheduler airflow dag.
img:  # Add image post (optional)
classes: wide
tags: [Apache Airflow, Python] # add tag
---
# Airflow
Airflow is a platform created by the community to programmatically author, schedule and monitor workflows.  

# Scheduler Logs
Airflow scheduler run continuously in background and generates lots of logs and it fills the file system. These logs needs to be clean up periodically to free space.

# Command to clean up log
find $AIRFLOW_HOME/logs/scheduler -type f -mtime +5 -delete

# Dag for running command daily
```python
from datetime import datetime, timedelta

from airflow.models import DAG
from airflow.operators.bash_operator import BashOperator


main_dag_id = 'scheduler_cleanup'


args = {
    'owner': 'Airflow',
    'start_date': datetime(2021, 7, 21),
    'provide_context': True
}



with DAG(
        main_dag_id,        
        catchup=False,
        concurrency=4,
        schedule_interval='@daily',
        default_args=args) as dag:

        clean_scheduler_logs = BashOperator(task_id='clean_scheduler_logs',
                            bash_command="find $AIRFLOW_HOME/logs/scheduler -type f -mtime +7 -delete")
       

        clean_scheduler_logs
```
