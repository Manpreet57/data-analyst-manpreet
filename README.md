# Project Description: Descriptive Analysis of Council Voting Records
# Project Part 1: Council Voting Records Data Analytical Platform

Dataset: Council Voting Records:  https://opendata.vancouver.ca/explore/embed/dataset/council-voting-records/table/?disjunctive.meeting_type&disjunctive.vote_number&disjunctive.council_member&disjunctive.vote


## Table of Contents

- [Objective](#objective)
- [Dataset](#dataset)
- [Methodlogy](#Methodlogy)
- [Tools and Techniques](#TandT)
- [Deliverables](#Deliverables)
- [Conclusion](#conclusion)
  
## Objective

The objective of the project is to transfer data from the City of Vancouver to the AWS cloud platform and develop a complete data analytics pipeline. The purpose is to optimize data processing, analysis, and visualization across multiple city departments, while ensuring scalability, security, and efficiency in managing the data.
![image](https://github.com/user-attachments/assets/cb8c1496-5a52-40c2-b3fd-4a53ea6e7e74)

Data Analytical Question Formulation
1.	Descriptive Metrics
What is the overall voting distribution in 2023 compared to 2024? (e.g., number of "In Favour" vs. "Against" votes).
Metric: Average Opposition Vote Rate
2.	Diagnostic Metrics
Which factors (e.g., meeting type, time of vote) are most strongly associated with higher opposition vote rates.
Metric: Opposition Vote Rate by Meeting Type
3.	Predictive Metrics
Based on voting patterns observed in 2023 and 2024, can we predict the likelihood of unanimous decisions in council meetings for the year 2025?
Metric: Predicted Opposition Vote Rate
4.	Prescriptive Metric:
Metric: Recommended Actions to Reduce Opposition Vote Rate
What strategies can be implemented to reduce opposition vote rates in future council meetings?


## DataSet

1. Overview of the Dataset
The datasets contain council voting records for the years 2023 and 2024. These records document the decisions made during various council meetings, capturing details such as the type of meeting, vote date, vote number, council member names, their votes, and the outcome of each vote. The datasets provide a comprehensive view of how decisions were made and the participation of different council members in the decision-making process.
2. Structure of the Data
•	Meeting Type: Describes the type of council meeting (e.g., Public Hearing, Policy & Strategic Priorities, Council).
•	Vote Date: The specific date when the vote was conducted.
•	Vote Number: A unique identifier assigned to each vote.
•	Vote Start Date Time: The timestamp indicating when the vote started.
•	Council Member: The name of the council member who participated in the vote.
•	Vote: The choice made by the council member, typically "In Favour," "Against," or other possible options.
•	Decision: The outcome of the vote, such as "Carried Unanimously" or other possible results.
•	Vote Detail Id: A unique identifier for the specific vote record, useful for tracing or referencing.


## Methodology 
### 1: Data Storage Desgin
   
- Tool: S3 bucket structure

- Subfolders: Inside the bucket we have 2024 and 2023 folders and we have landing named folder for each year. Inside that we have our dataset name Council Voting Records. That we will be using further.

![image](https://github.com/user-attachments/assets/1668a4df-3f7b-4432-8854-eb93d70b5647)

### 2: Dataset Prepartion

Tool: AWS DataBrew
Key Tasks:

- Removing the rows with NA values.
- Dropping Less relevent columns.
- Renaming the metadata for better understanding.
- Conversion of numerical data types to integers.
- Two DataBrew jobs were created: CouncilVotingRecords-Job-2023 and CouncilVotingRecords-Job-2024.

![image](https://github.com/user-attachments/assets/03dfb361-5372-4437-b36f-95abebe65731)



### 3: Data Ingestion, Storage, Pipeline Design, Cleaning, Pipeline Implementation.
- Tool: AWS Glue ETL
- The dataset in Raw data which is cleaned with no Na and missing values. The next task is to covert the operational dataset into analytical dataset. 
AWS glue ETL will be used for this task and then we will making a new folder named 'curated' and then saving our analytical dataset into that.
- We will finding the opposition vote rate for 2023 and 2024.
- The Formula is:
 **Opposition Vote Rate=(No of In Opposition Vote Rate/ Total Number of Votes )**
- These are the following steps used to construct the ETL pipline
- Imported datasets for 2023 and 2024 from S3 raw folder.
- Changed schema, filtered out irrelevant rows, and dropped null values.
- Used join, union and aggregate functions to combine datasets and focus on opposition rate classification.
- AWS Glue’s ETL process cleaned and transformed the data:
- Targeted crucial dataset variables such as Vote Status.
- Combined data for multiple years and removed irrelevant or incomplete data.
- Final filtered data stored in the curated S3 folder in CSV format.
- The ETL Pipeline was implemented using AWS Glue, storing the cleaned datasets in the curated folder of the S3 bucket.

![image](https://github.com/user-attachments/assets/69015e13-d98b-4c43-a916-430c80b25e1f)

### 4: Data Structuring 
- Tools Used: AWS Athena
- The setup involves importing raw data from an S3 bucket in Amazon Web Services (AWS).
- The specific folder, named 'raw,' contains unprocessed data that is used for further structuring.
- SQL queries are then applied using Athena, an interactive query service that allows users to analyze data in S3 using standard SQL. This approach makes the data accessible for faster queries and enables easier analysis, reporting, and insights extraction, crucial for data-driven decision-making.

![image](https://github.com/user-attachments/assets/72077ff1-28a2-45da-af85-8acffa341377)

### 5: Data Analysis and Visualization
-Tools Used: MS Excel
- The Bar Graph was Created to display the number opposition rate vs year from AWS Athena in CSV format.

![image](https://github.com/user-attachments/assets/7b4b584d-2c32-4e64-b887-980606b699df)

### 6. Data Protection

Tools- KMS and S3 
- AWS KMS (Key Management Service): The encryption tool used to protect data at rest in the S3 bucket. AWS KMS manages encryption keys and applies server-side encryption (SSE-KMS) automatically when new objects are uploaded. The screenshot shows the encryption type and the specific Key ARN (Amazon Resource Name) used for the bucket.

- S3 Versioning and Replication: These features help avoid accidental data loss. Versioning stores multiple versions of an object, while cross-region replication copies data to other AWS regions for redundancy and higher accessibility, ensuring data durability and availability across locations.

![image](https://github.com/user-attachments/assets/a1f37872-66c1-4a9f-9b67-f3daa0ee9fe7)

![image](https://github.com/user-attachments/assets/661b4008-1140-4672-b125-97e96910a2a1)



### 7: Data Monitoring

Monitoring Tools: AWS CloudWatch and AWS CloudTrail

AWS CloudTrail:
- Used for tracking user activity and API usage within the AWS environment.
- In the image, a specific trail (govtandfinance-op-trail-manpreet) is created to log events in the US East (N. Virginia) region.
- The trail monitors API calls and logs them to an S3 bucket (aws-cloudtrail-logs-cvr), which can be further analyzed.
- CloudTrail helps in auditing and compliance by keeping track of all activities and changes in the AWS environment.

AWS CloudWatch:
- Used for real-time monitoring of AWS resources and applications.
- The dashboard displayed monitors various metrics like the number of objects and estimated changes for the application or service (govtandfinance-op-dashboard-manpreet).
- CloudWatch collects and tracks log files, generates alerts, and offers insights into resource utilization, application performance, and operational health.


![image](https://github.com/user-attachments/assets/3260ac12-e944-405c-ab87-97eac93ad49f)

![image](https://github.com/user-attachments/assets/1bf67fdd-ccb0-439a-bc3a-448798ed6a0a)

## Tools and Techniques:
1. AWS S3 Bucket
2. AWS Glue
3. AWS Glue DataBrew
4. AWS Athena
5. AWS EC2
6. AWS KMS (Key Management Services)
7. AWS CloudWatch
8. AWS CloudTrail

## Deliverables:
1. Data Questions and Discovery
2. S3 Storage Structure
3. Cleaned Datasets for 2023 and 2024
4. ETL Pipeline Design and Implementation (AWS Glue)
5. Structured SQL Queries in AWS Athena
6. Visualizations (Charts in MS Excel)
7. Data Published on General and Web Servers (EC2 Instances)
8. Data Protection (AWS KMS encryption)
9. S3 Versioning for data history
10. Cross-Region Replication for data availability

These deliverables offer a solid framework for analyzing property tax data, ensuring that data is securely stored, processed, and published while maintaining both integrity and availability.

## Conclusion:
This data analytics initiative assists the City of Vancouver in making informed decisions that benefit local businesses and residents. By examining zoning classification trends for properties, the city can enhance urban planning, stimulate the local economy, improve public services, and encourage sustainable development.














