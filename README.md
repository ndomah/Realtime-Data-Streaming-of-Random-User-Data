# Real-time Data Streaming of Random User Data
## Project Overview
In this project, I built a robust, scalabale, and fault-tolerant pipeline using a modern tech stack. The pipeline ingests, processes, and stores [random user-generated data from an API](https://randomuser.me/), showcasing the integration of tools such as Python, Docker, Apache Airflow, Kafka, Spark, Cassandra, and PostgreSQL. 
## Tech Stack
- **Docker**: Containerization for isolating and managing services.
- **Apache Airflow**: Workflow orchestration to manage and schedule tasks.
- **Apache Kafka**: Distributed streaming platform for high-throughput data ingestion.
- **Apache Spark**: Unified analytics engine for real-time data processing.
- **Apache Cassandra**: NoSQL database for scalable and fault-tolerant data storage.
- **PostgreSQL**: Relational database for metadata and structured data storage.
## Architecture
![architecture](https://github.com/ndomah/Realtime-Data-Streaming-of-Random-User-Data/blob/main/architecture.png)
1. **Data Ingestion**: Random user data is fetched using an API and streamed to Kafka topics.
2. **Data Processing**: Kafka streams are processed using Spark and structured into Cassandra.
3. **Data Orchestration**: Airflow handles workflow orchestration, triggering Spark and other tasks.
4. **Data Storage**: Data is persisted in Cassandra for NoSQL storage and PostgreSQL for relational data.
## Workflow
### 1. Folder Structure and Installation
Have the following installed:
- Docker and Docker Compose
- Python
- Apache Airflow dependencies (`pip install -r requirements.txt`)
Write scripts and configure the following folder structure:
```
├── dags
│   └── kafka_stream.py
├── docker-compose.yml
├── requirements.txt
├── script
│   └── entrypoint.sh
└── spark_stream.py
```
### 2. Spinning Up Docker Containers
Use `docker-compose.yml` file to orchestrate the environment. Run the following command to spin up all necessary services:
```
docker-compose up -d
```
This command sets up:
- Kafka (Zookeeper, Broker, Schema Registry, Control Center)
- Apache Spark (Master and Worker nodes)
- Apache Cassandra
- PostgreSQL
- Apache Airflow (Webserver and Scheduler)
### 3. Streaming Data with Kafka
The [kafka_stream.py](https://github.com/ndomah/Realtime-Data-Streaming-of-Random-User-Data/blob/main/dags/kafka-stream.py) fetches random user data from the [Random User API](https://randomuser.me/) and streams it to a Kafka topic.

Key Dag Tasks:
- `get_data`: Fetch random user data from the API
- `stream_data`: Publish structured data to the Kafka topic `users_created`

**Execute the DAG**: Access the Airflow web UI at `http://localhost:8080` and trigger the `user_automation` DAG. 
### 4. Processing Data with Spark
The [spark_stream.py](https://github.com/ndomah/Realtime-Data-Streaming-of-Random-User-Data/blob/main/spark_stream.py) script processes the Kafka stream and writes the data to Cassandra. 

Key steps include:
- Connecting to Kafka to read streaming data
- Defining the schema for structured data processing
- Writing the processed data to a Cassandra table

**Run the Spark job**:
```
spark-submit --master spark://localhost:7077 spark_stream.py
```
Monitor the Spark cluster at `http://localhost:9090`.
### 5. Persisting Data in Cassandra
Cassandra stores the processed data in the `spark_streams.created_users` table.

Query the table using:
```
docker exec -it cassandra cqlsh -u cassandra -p cassandra localhost 9042
```
Inspect the data:
```
SELECT * FROM spark_streams.created_users;
```
### Additional Notes
#### Docker Compose Highlights
The [docker_compose.yml](https://github.com/ndomah/Realtime-Data-Streaming-of-Random-User-Data/blob/main/docker-compose.yml) file defines all the services and their configurations, including health checks, networking, and volumes.
#### Entrypoint Script
The [entrypoint.sh](https://github.com/ndomah/Realtime-Data-Streaming-of-Random-User-Data/blob/main/script/entrypoint.sh) script initializes Airflow's database, installs requirements, and starts the webserver. 
#### Airflow DAG
The [Airflow DAG](https://github.com/ndomah/Realtime-Data-Streaming-of-Random-User-Data/blob/main/dags/kafka-stream.py) integrates with Kafka to automate data ingestion and streaming tasks.
### Visualization
- **Kafka Control Center**: Access at `http://localhost:9021` to monitor Kafka topics.
- **Airflow UI**: Access at `http://localhost:8080` to manage workflows.
- **Spark UI**: Access at `http://localhost:9090` to monitor Spark jobs.
