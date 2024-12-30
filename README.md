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
- `stream_data`: Publish structured data to the Kafka topic `users_created`.
**Execute the DAG**: Access the Airflow web UI at `http://localhost:8080` and trigger the `user_automation` DAG. 
