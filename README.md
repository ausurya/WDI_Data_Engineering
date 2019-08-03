## DATA

The data was downloaded from :
https://www.kaggle.com/worldbank/world-development-indicators

The World Development Indicators from the World Bank contain over a thousand annual indicators of economic development from hundreds of countries around the world.

The three basic data files are:
1. WDICountry -> This describes the details of all the countries in the world
2. WDIData -> This describes the details of all the countries and it's indicator values from year 1960 to year 2017
3. WDISeries -> This describes the details of all the indicators of development 

The above files are uploaded into s3 buckets created in aws. The reason is that hdfs is difficult to maintain and we have only few files for now.

## STEPS TO EXECUTE THIS 

EMR cluster:
I have spinned off an EMR cluster on AWS. This cluster has 3 nodes each of m5.xlarge type. My reasoning was that this type[4 cores and 16gb ram] will be good enough to handle the data set parallely. 
Note: Before running this cluster make sure there is a bootstrapping action of installing the pandas and other necessary libraries. So that all of these are insalled in every node in the cluster. This file is also included in the repository.

Download the notebook and attach this to the EMR cluster and start running.
[Note: Change the s3 path to the ones which you have created]

A script can also be created so that we can ssh into the cluster and run our script on the cluster. This will be useful in phase 2 of my project where I am planning to schedule this entire process monthly using AIRFLOW.

## Analytic Requirements 
1. Country Data set
  -> Clean up the country data[ for any duplicate records and valid country_iso_code]
  -> Keep columns iso code,country code, name, long name,region income group

Data Model
A singular table containing cleaned above columns written as a csv file

2. Check the penetration of cellular and broadband coverage with respect to the population of eacy country on a yearly basis

Data Model
A singular table containing the following columns
Year,Population[categories],cellular subscriptions,broadband subscription

## PIPELINE
The data from the s3 bucket is read into data frames on the cluster->Using spark I process the data by following the steps like 
1. Cleaning the data
2. Transform the data according to the requirements
3. Perform data quality checks -> Finally I write the final tables back to s3 buckets.

## Following phase

Using Airflow I will be creating a pipeline and scheduling the jobs once a month assuming the world bank releases this data monthly.
