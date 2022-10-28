# ETL-Api-to-S3-using-Airflow
In this project I would create a data engineering workflow that would answer a question “What are the top trending tags appearing in StackOverFlow this month?” Although here only one question is being answered, such queries can be integrated later and my main focus is to create ETL and ELT workflows to implement this task and display the data on Power BI to the end business users for their decision making.

Workflow :


Technology Used : Python, Airflow, Amazon S3, Snowflake, DBT

This project has been built in 5 steps.

Fetching and transforming the data using Stackoverflow API.
Loading the data to Amazon S3 bucket and orchestrating it using Apache Airflow
Copying/ Fetching the data from Amazon S3 to Snowflake Datawarehouse.
Integrating DBT with snowflake to create the data model and creating a nightly refresh job.
Displaying the DBT model created on Power BI to make business decisions.

Fetching the data using API and loading it to S3 bucket

I have made use of Airflow to automate this process by creating a data pipeline
The data was fetched using requests library using Python
Boto3 library is used to write this data as csv file on S3 bucket
Airflow DAG is created using the Python Operator.




Code :
Importing libraries : 



DAG Tasks :





DAG :




Airflow Data Pipeline DAG


Here fetching and transformation has been done in the first step and loading of tata has been done in the second step.

Successful Data load in S3 bucket


Till here we have extracted, transformed and loaded the data to our destination therefore our ETL process is complete. 
ELT
Loading S3 data on snowflake to perform data modeling

I am now going to load S3 bucket data which we have created to snowflake. We can orchestrate this process using Airflow by breathing another node in DAG but for now I’ll load the data using a query and demonstrate the ELT process separately. 

Creating a table in snowflake to store the data :


Migrating the data from S3 to snowflake : 


We can see our data has been extracted and loaded in the snowflake from S3 bucket.




Now we would transform this data using DBT by creating a data model which would basically display the data in a way our business users want to be displayed.


Creating DBT model
The DBT needs to be connected to snowflake and all the connections and how it is done could also be demonstrated but for now I’m focusing on the code and implementation part.

Created a model which would drop some unnecessary columns and would add an additional column million_serches which would tell the business user if a particular tag was searched over a million times or not.










We would run the dbt model using dbt run command and after a successful run we would be able to see the contents of this model in snowflake : 


Snowflake also demonstrates the query which it executed to generate this model : 

Thus our data has been transformed!

Running DBT Cloud job

In order for our business users to make decisions effectively, we must refresh this data on a daily basis and see how the stack overflow trends are changing. To do that DBT provides a dbt cloud job where we can run our models according to our schedule and our models become up to date and no relevant and latest data is missed.

Run overview of one of the job runs :


This would be running every night and keep our data modeling up to date.
















Integrating Snowflake with Power BI and show visualization

This is the last stage of our project and Power BI has an easy way of connecting to snowflake. I have connected our created data model and this is how it is displayed on powerbi. 


The business users can derive insights from this data like :
Why some languages are mentioned mode and some less?
Is there an underrated language that is in more demand but searched less? etc
Conclusion 

This is an end to end data engineering project where the focus is mainly on implementation. I am willing to demonstrate this project live and would love the feedback on it and ways to improve my implementation style.
