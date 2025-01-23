# **Weather Data Analytics Pipeline**  
This project demonstrates the development of a scalable, near real-time data pipeline for ingesting, storing, and analyzing weather data. The pipeline leverages AWS services such as DynamoDB, S3, and Lambda, integrated with Snowflake for advanced analytics and reporting. By fetching hourly weather updates from a public API, the system ensures continuous data collection, efficient storage, and seamless processing for downstream analysis. The end goal is to enable actionable insights and historical trend analysis using a robust and cost-effective architecture.  
## **Architecture Overview**  
### **Data Flow:**  
A Lambda function fetches data from a Weather API and stores it in DynamoDB.  
DynamoDB Streams triggers another Lambda function to process the data and write it to an S3 bucket.  
Finally, Snowflake ingests the data from S3 for further analysis.  
![image](https://github.com/user-attachments/assets/58a997f5-d468-497f-8a6f-2abbd07ad6e8)  
### **Technologies Used:**  
DynamoDB: For real-time data storage.  
S3: For intermediate data storage in CSV format.  
Snowflake: For structured data analysis.  
Lambda: To orchestrate data movement and processing.  
## **2. DynamoDB Setup**
Create a DynamoDB Table:  
Partition Key: City  
Sort Key: Timestamp (to handle hourly weather updates).  
Use DynamoDB Streams to monitor and act upon table changes.  
I selected Amazon DynamoDB for this project due to its low-latency read and write capabilities, which ensure efficient handling of real-time weather data. Additionally, DynamoDB Streams was utilized to enable the continuous flow of updated data to Amazon S3 in near real-time. This setup allows for seamless integration between DynamoDB and downstream systems, ensuring that the most up-to-date data is always available for analysis and processing  
![image](https://github.com/user-attachments/assets/b1ab95e2-fd57-46b7-930f-11da0d23d9ae)  
## **3. Weather Data Ingestion**  
Lambda Function to Fetch Weather Data:  
Fetch weather data for predefined cities.  
Process the API response to extract relevant metrics (e.g., temperature, wind speed, humidity).  
Save the data into the DynamoDB table in JSON format.  
For weather data ingestion, I used an AWS Lambda function to fetch weather data from the Weather API. Lambda was chosen for its serverless architecture and cost-effectiveness in processing periodic tasks. The Lambda function was scheduled to execute every hour using Amazon EventBridge (formerly CloudWatch Events). This scheduling ensures that the weather data is updated hourly, providing fresh and accurate data for storage in DynamoDB and further downstream usage.  
![image](https://github.com/user-attachments/assets/d27b4316-d922-4203-9f0b-2d288f8e2c63)  
## **4. Data Movement to S3**
Set Up an S3 Bucket:  
Create an S3 bucket to store processed CSV files.  
Lambda Function for Stream Processing:  
Triggered by DynamoDB Streams.  
Extract the New Image from the stream.  
Convert the data to a pandas DataFrame.  
Save the data as a CSV file in S3.  
To ensure scalable and cost-effective storage, I configured DynamoDB Streams to capture real-time updates from the DynamoDB table. These updates were processed using an AWS Lambda function, which transformed and loaded the data into an Amazon S3 bucket in near real-time. S3 was chosen for its high durability, scalability, and integration capabilities with downstream tools like Snowflake. The data was stored in a structured format (e.g., Parquet or CSV) to optimize storage efficiency and facilitate faster querying during analytics. This approach ensures that all ingested weather data is archived for historical analysis while maintaining near real-time availability.  
![image](https://github.com/user-attachments/assets/949bcddf-7863-4105-99d8-ce1888b98a4d)  
## **5. Snowflake Integration**
Data Loading:  
Use Snowflake Stages:  
External Stage: Load data from S3 into Snowflake tables.  
Utilize SQL commands to automate the ingestion process  
To enable advanced data analytics and reporting, I integrated Snowflake with the data stored in Amazon S3. I used Snowflake’s native support for external stages to securely access the S3 bucket and load data directly into Snowflake tables. This integration was configured to handle large volumes of data efficiently, leveraging Snowflake’s auto-scaling compute resources for fast query performance. Additionally, Snowflake’s time-travel feature was utilized to retain historical versions of the data, ensuring robust data traceability and auditability. This setup allowed for seamless querying, aggregation, and reporting of weather data for further analysis.  

![image](https://github.com/user-attachments/assets/b82214cc-b353-4388-a2e3-8d742858c713)  


## **6. Key Benefits of the System**  
Real-time ingestion of weather data.  
Scalability with DynamoDB for transactional workloads.  
Efficient data storage and retrieval via S3 and Snowflake.  
Flexibility to expand for additional data sources or processing.  



