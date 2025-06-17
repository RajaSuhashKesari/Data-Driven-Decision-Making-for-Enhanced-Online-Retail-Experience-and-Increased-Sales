# This Project provides the Batch Processing and Stream Processing Solution

## 1. Implemented Batch Processing in Azure Data Engineering Services for Online Retails Dataset.
![image](https://github.com/user-attachments/assets/30048bbe-f7a9-454c-9270-10dd4c464b6b)

![image](https://github.com/user-attachments/assets/d7a1e7ec-70f8-4b4e-8a24-2c345c1e513d)

## Medallion Architecture :-
The Medallion architecture structures data in a multi-tier approach —bronze, silver and gold tier— taking into account and encouraging data quality as it moves through the transformation process (from raw data to valuable business insights).
![image](https://github.com/user-attachments/assets/7bf87a8d-1cdd-4324-a9e0-a2db07032f73)
###### 1. Bronze layer: This phase marks the input of raw data, which is stored as it is collected, usually from a variety
of sources and in formats such as CSV or JSON. The data is usually raw data and varies in quality and
structure.
###### 2. Silver layer: At this point, the data is processed and transformed to achieve cleaner, more structured data.
Tasks such as filtering, validation and normalisation of the data are carried out and stored in efficient
formats. This phase may include defined schemas and additional metadata.
###### 3. Gold layer: This stage contains data already prepared for analysis and business use. In the Gold layer,
advanced transformations and aggregations are performed to create rich data sets. The data is structured,
optimised for fast queries and can be enriched with additional information or merged with other data
sources for deeper insights.
## Dataset used in this project:-
Brazilian E-Commerce Public Dataset by Olist:-Welcome! This is a Brazilian ecommerce public
dataset of orders made at Olist Store. The dataset has informa􀆟on of 100k orders from 2016 to
2018 made at muliple marketplaces in Brazil. Its features allows viewing an order from
multiple dimensions: from order status, price, payment and freight performance to customer
location, product atributes and finally reviews written by customers. We also released a
geoloca􀆟on dataset that relates Brazilian zip codes to lat/lng coordinates.
Datasets and its details:-

### 1. Customers Dataset
This dataset has information about the customer and its location. Use it to identify unique customers in the
orders dataset and to find the orders delivery location.At our system each order is assigned to a unique
customer_id. This means that the same customer will get different ids for different orders. The purpose of
having a customer_unique_id on the dataset is to allow you to identify customers that made repurchases at
the store. Otherwise you would find that each order had a different customer associated with.

### 2. Geolocation Dataset
This dataset has information Brazilian zip codes and its lat/lng coordinates. Use it to plot maps and find
distances between sellers and customers.

### 3. Order Items Dataset
This dataset includes data about the items purchased within each order.
Example:
The order_id = 00143d0f86d6fbd9f9b38ab440ac16f5 has 3 items (same product). Each item has the freight
calculated accordingly to its measures and weight. To get the total freight value for each order you just have
to sum.
The total order_item value is: 21.33 * 3 = 63.99
The total freight value is: 15.10 * 3 = 45.30
The total order value (product + freight) is: 45.30 + 63.99 = 109.29

### 4. Payments Dataset
This dataset includes data about the orders payment options.

### 5. Order Reviews Dataset
This dataset includes data about the reviews made by the customers.
After a customer purchases the product from Olist Store a seller gets notified to fulfill that order. Once the
customer receives the product, or the estimated delivery date is due, the customer gets a satisfaction survey
by email where he can give a note for the purchase experience and write down some comments.

### 6. Order Dataset
This is the core dataset. From each order you might find all other information.

### 7. Products Dataset
This dataset includes data about the products sold by Olist.

### 8. Sellers Dataset
This dataset includes data about the sellers that fulfilled orders made at Olist. Use it to find the seller location
and to identify which seller fulfilled each product.

### 9. Category Name Translation
Translates the product_category_name to english.
## Data Schema:-
![image](https://github.com/user-attachments/assets/644a15f3-9cd6-4ce0-9d98-c601cc18e31b)

## Azure Services Required for Data Engineering:-
###### 1. Resource Group
###### 2. Azure Data Lake Storage Gen2.
###### 3. Azure Data Factory.
###### 4. Azure Databricks.
###### 5. Azure Synapse analytics.

## Data ingestion and Load to Bronze folder in ADLS Gen2 using ADF:-
--Before ingestion we need to focus on the what is required to ingest.
###### 1. Pipeline for which will extract data from sources and load to destination.
###### 2. Datasets to store the metadata of the source files and to point them.
###### 3. Linked Services to maintain the connection between ADF and sources and destinations
### Creating Linked services :-
As our data required for bronze layer is in the Github and MySQL database and we have load the data to ‘Bronze’ Folder in ADLS Gen2 
So, we required three linked services:-
###### 1.	Http Linked Service to connect Github.
###### 2.	MySQL Linked Service to Connect MySQL database.
###### 3.	Azure Data Lake Storage Gen2  Linked service to connected to ‘Bronze’ folder

#### I created three linked Services
###### 1.Created http linked service and  LS_for_Github as its name
![image](https://github.com/user-attachments/assets/140f3b31-c781-4cc8-abac-2fa4ae05b421)

###### 2.Created MySQL linked service and LS_for_MySQL_database as its name
![image](https://github.com/user-attachments/assets/d79f55c5-f395-49f9-b2f7-d76fa66668c8)

###### 3.Created Azure Data Lake Storage Gen2 linked service and LS_AzureDataLakeStorageGen2 as its name
![image](https://github.com/user-attachments/assets/37c4ff0e-878c-4727-8247-15444b7030b2)

#### After Creating we need 4 Datasets so I created them 
###### 1.	DS_MySql_order_payments :-This dataset likely refers to order payments data stored in a MySQL database.
###### 2.	DS_olist_bronze_csv_file:-This dataset is pointing to the location where the data is going to be loaded after ingestion .
###### 3.	DS_olist_source_files_csv:- This dataset is refers to the original datasets which are csv format in the Github.
###### 4.	DS_source_metadata_json:- This dataset is refers to the metadata file in github which contains the metadata about the source files and their locations.
 ![image](https://github.com/user-attachments/assets/29e5f87d-06f4-4367-adfd-0733346145fb)

###### we need to a pipeline which is going to ingest the data from multiple source and load them into the bronze folder so, I created a pipline which is going to ingest the data in github and Mysql table in to Azure Data Lake Storage Gen2 ‘Bronze’ folder.
![image](https://github.com/user-attachments/assets/49af3fd5-ccda-4547-b160-2a48725e7d41)
 
After Execution of pipeline the datasets are copied from Github and MySQL database to ‘Bronze’ folder.
![image](https://github.com/user-attachments/assets/427d2e76-ffc7-4dbd-aa10-9c6db36bae53)
The data is successfully ingested to bronze layer using azure data factory.

## 2. Implemented Stream Processing in Azure Data Engineering Services for Online Retails Streaming Dataset.
![image](https://github.com/user-attachments/assets/da54ee76-0429-4700-91ac-a3a1abf61236)

### Dataflow and Design :-
![image](https://github.com/user-attachments/assets/ef434fcc-9ab6-4c6d-a80c-fff7344dc311)

### final Real-Time Dashboard
![image](https://github.com/user-attachments/assets/46160ca4-7295-4ea2-bd7c-0ad695c4f792)
![image](https://github.com/user-attachments/assets/7f458e7f-8da8-46aa-8417-8a03ac60fd5c)
![image](https://github.com/user-attachments/assets/be421ba6-5058-44c9-bd6c-a83f9c7ff16f)


