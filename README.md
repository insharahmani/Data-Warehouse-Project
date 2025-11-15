# 🚀 Sparkify Data Warehouse on AWS Redshift

This project builds a scalable **cloud-based Data Warehouse** for Sparkify, a music streaming startup.  
It extracts raw JSON files from **Amazon S3**, stages them in **Amazon Redshift**, and loads them into a **Star Schema** optimized for analytics.

---

## 📌 Project Overview

Sparkify’s growing dataset required a cloud-based warehouse for faster analytics and scalability.  
This project creates an automated **ETL pipeline** that:

- Loads song metadata & user activity logs from S3  
- Uses Redshift `COPY` command to load staging tables  
- Transforms and inserts data into fact & dimension tables  
- Supports analytics on user listening behavior  

---

## 📂 Datasets

### 🎵 Song Dataset (JSON)
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
```

### 📑 Log Dataset (JSON)
Example:
```json
{
  "artist": null,
  "auth": "Logged In",
  "firstName": "Walter",
  "gender": "M",
  "page": "Home",
  "ts": 1541105830796,
  "userId": "39"
}
```

---

## 🏗️ Data Warehouse Schema (Star Schema)

### ⭐ Fact Table — `songplays`
Tracks events of users playing songs.

Columns:
- songplay_id  
- start_time  
- user_id  
- level  
- song_id  
- artist_id  
- session_id  
- location  
- user_agent  

### 📘 Dimension Tables

#### `users`
- user_id  
- first_name  
- last_name  
- gender  
- level  

#### `songs`
- song_id  
- title  
- artist_id  
- year  
- duration  

#### `artists`
- artist_id  
- name  
- location  
- latitude  
- longitude  

#### `time`
- start_time  
- hour  
- day  
- week  
- month  
- year  
- weekday  

---

## ⚙️ Project Structure

```
|-- create_tables.py      # Creates/drops Redshift tables
|-- etl.py                # ETL pipeline: S3 → Redshift
|-- sql_queries.py        # SQL queries for tables & ETL
|-- dwh.cfg               # Redshift & IAM configuration
|-- README.md             # Project documentation
```

---

## 🔧 How to Run the Project

### 1️⃣ Launch Redshift Cluster
Use your **Redshift_Cluster_IaC.py** script to create:
- Redshift cluster  
- IAM Role with S3 read permissions  

---

### 2️⃣ Create `dwh.cfg`

Create this file in your project directory:

```
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
```

---

### 3️⃣ Create Tables

Run:
```bash
python create_tables.py
```

---

### 4️⃣ Run the ETL Pipeline

Run:
```bash
python etl.py
```

This performs:
- COPY from S3 → Staging tables  
- Insert into Fact + Dimension tables  

---

## 🧪 Testing

Test table load in Redshift:

```sql
SELECT COUNT(*) FROM songplays;
SELECT COUNT(*) FROM users;
SELECT COUNT(*) FROM songs;
SELECT COUNT(*) FROM artists;
SELECT COUNT(*) FROM time;
```

---

## 🏁 Summary

This project demonstrates:
- Cloud data warehousing with **AWS Redshift**  
- ETL pipeline development using **Python**  
- Star schema design for analytics  
- Ingestion of JSON logs using **S3 + COPY**  

