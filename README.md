# Microsoft_Fabric_Uber_Data_Analytics_Project
This repo contains details about Microsoft Fabric Uber Data Analytics 

**Project Details**

1. Extracted the yellow_tripdata_2023-01 parquet data from the NYC website using ADF via HTTP Linked connection and loaded into Lakehouse as Parquet format
2. Transformed the data and created Facts and Dimension tables using Pyspark
3. Converted Pandas to Pyspark data frames and created DELTA format files
4. Using DELTA format files as source, created DELTA tables in the uber_lakehouse database
5. Compared and validated the record counts from the processed layer into tables- counts matched
6. Created Sample SQL Query for validations


Source: https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

File Name: https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-01.parquet

![image](https://github.com/vinaykm5758/Microsoft_Fabric_Uber_Data_Analytics_Project/assets/45409524/f8596ca0-43dc-4d55-8cc5-55d7fa0a1048)



Raw Layer: Loaded using ADF into Lakehouse Raw Folder

![image](https://github.com/vinaykm5758/Microsoft_Fabric_Uber_Data_Analytics_Project/assets/45409524/314c6821-b45a-463a-b660-084ab7c3ccb4)


DELTA FORMAT FILES has a great addition of "_delta_log" which is used to capture and perform ACID Transactions/ Versioning 

![image](https://github.com/vinaykm5758/Microsoft_Fabric_Uber_Data_Analytics_Project/assets/45409524/dae39ed0-2415-44db-bae1-ce5e1441dc30)


Processed Layer: Transformations performed using Python and Pyspark

![image](https://github.com/vinaykm5758/Microsoft_Fabric_Uber_Data_Analytics_Project/assets/45409524/1a49084f-3df1-464c-92f9-a77ae6004fcc)


Raw to Processed layer code URL: https://github.com/vinaykm5758/Microsoft_Fabric_Uber_Data_Analytics_Project/blob/main/Uber_Extract_Raw_to_Processed_Facts_and_Dimensions.ipynb



DELTA TABLES: Using Pyspark, converted Pandas data frames to Pyspark Dataframe and loaded them into DELTA format files and created DELTA Tables


![image](https://github.com/vinaykm5758/Microsoft_Fabric_Uber_Data_Analytics_Project/assets/45409524/b1a5cc55-c38b-4b81-a2c1-f1292b779567)


Creation of Delta tables using Pyspark code URL : https://github.com/vinaykm5758/Microsoft_Fabric_Uber_Data_Analytics_Project/blob/main/Uber_Processed_to_DELTA_Tables.ipynb


Final SQL QUERY:

%%sql

SELECT
f.VendorID, p.passenger_count, td.trip_distance, rc.RatecodeID, f.store_and_fwd_flag, pt.payment_type,
f.fare_amount, f.extra, f.mta_tax, f.tip_amount, f.tolls_amount, f.improvement_surcharge, f.total_amount
from fact_table_managed_delta f
join dim_passenger_count_managed_delta p on f.passenger_count_id = p.passenger_count_id
join dim_trip_distance_managed_delta td on f.trip_distance_id = td.trip_distance_id
join dim_rate_code_managed_delta rc on f.rate_code_id = rc.rate_code_id
join dim_payment_type_managed_delta pt on f.payment_type_id = pt.payment_type_id;


