# 🚀 Sparkify Data Warehouse on AWS Redshift

This project builds a fully scalable **cloud-based Data Warehouse** for **Sparkify**, a music streaming startup.  
The system extracts raw JSON files from **Amazon S3**, stages them in **Amazon Redshift**, and loads them into a **Star Schema** optimized for analytics.

---

## 📌 Project Overview

Sparkify’s user base and song catalog have expanded, requiring a shift from local processing to a cloud-based warehouse.

This project develops an automated **ETL pipeline** that:

- Loads song metadata & user activity logs from S3  
- Stages data in Redshift using the `COPY` command  
- Transforms and inserts data into fact & dimension tables  
- Supports analysis of user behavior and listening patterns  

---

## 📂 Datasets

### 🎵 Song Dataset
Stored as JSON files in S3 (subset of the Million Song Dataset).

Example:
```json
{
  "num_songs": 1,
  "artist_id": "ARJIE2Y1187B994AB7",
  "artist_location": "",
  "artist_name": "Line Renaud",
  "song_id": "SOUPIRU12A6D4FA1E1",
  "title": "Der Kleine Dompfaff",
  "duration": 152.92036,
  "year": 0
}
📑 Log Dataset
User activity logs generated from the Sparkify application.

Example:

json
Copy code
{
  "artist": null,
  "auth": "Logged In",
  "firstName": "Walter",
  "gender": "M",
  "page": "Home",
  "ts": 1541105830796,
  "userId": "39"
}
🏗️ Data Warehouse Schema (Star Schema)
⭐ Fact Table: songplays
Records each user songplay event.

Columns:

songplay_id (PK)

start_time

user_id

level

song_id

artist_id

session_id

location

user_agent

📘 Dimension Tables
users
user_id (PK)

first_name

last_name

gender

level

songs
song_id (PK)

title

artist_id

year

duration

artists
artist_id (PK)

name

location

latitude

longitude

time
start_time (PK)

hour

day

week

month

year

weekday

⚙️ Project Structure
bash
Copy code
|-- create_tables.py      # SQL table drop/create scripts
|-- etl.py                # ETL pipeline for loading data from S3 → Redshift
|-- sql_queries.py        # SQL queries for staging and final tables
|-- dwh.cfg               # Redshift cluster + IAM role configuration
|-- README.md             # Project documentation
🔧 How to Run the Project
1️⃣ Launch Redshift Cluster
Use your infrastructure script (Redshift_Cluster_IaC.py) to create:

Redshift Cluster

IAM Role with S3 read access

2️⃣ Configure dwh.cfg
Create a file named dwh.cfg in your project root:

ini
Copy code
[CLUSTER]
HOST=''
DB_NAME=''
DB_USER=''
DB_PASSWORD=''
DB_PORT=5439

[IAM_ROLE]
ARN=<YOUR_IAM_ROLE_ARN>

[S3]
LOG_DATA='s3://udacity-dend/log_data'
LOG_JSONPATH='s3://udacity-dend/log_json_path.json'
SONG_DATA='s3://udacity-dend/song_data'
3️⃣ Create Tables
Run the script:

bash
Copy code
python create_tables.py
This will drop old tables and create new staging + analytical tables.

4️⃣ Run the ETL Pipeline
Run:

bash
Copy code
python etl.py
This loads:

JSON logs + song data from S3 → Staging Tables

Transformed data → Fact + Dimension tables

🧪 Testing
Run these queries in Redshift to validate the load:

sql
Copy code
SELECT COUNT(*) FROM songplays;
SELECT COUNT(*) FROM users;
SELECT COUNT(*) FROM songs;
SELECT COUNT(*) FROM artists;
SELECT COUNT(*) FROM time;
🏁 Summary
This project demonstrates:

Cloud data warehousing using AWS Redshift

ETL development using Python

Star schema design for analytics

Large-scale JSON ingestion using S3 + COPY command

