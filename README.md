
# 📈 StreamStonks: Real-Time Stock Market Data Pipeline with Kafka and AWS

## 🧠 Introduction

**StreamStonks** is a real-time data processing pipeline simulating stock market activity. It integrates a Kafka-based message broker system with AWS services (S3, Glue, and Athena) to process and analyze stock data. The project demonstrates a full end-to-end data streaming architecture with producer-consumer logic, cloud storage, metadata cataloging, and query capabilities.

## 🗂️ Table of Contents

- [Features](#-features)
- [Architecture](#-architecture)
- [Installation](#-installation)
- [Usage](#-usage)
- [Configuration](#-configuration)
- [Dependencies](#-dependencies)
- [Examples](#-examples)
- [Troubleshooting](#-troubleshooting)


## 🌟 Features

- Simulates stock data using Python and CSV datasets.
- Uses Apache Kafka as the message broker.
- Runs Kafka on AWS EC2 instances.
- Stores data in Amazon S3.
- Automatically catalogs data with AWS Glue.
- Enables SQL querying using Amazon Athena.
- Fully scalable and modular architecture.

## 🏗️ Architecture

```
[CSV Dataset
or Real-time Stock Data] --> [Python Stock Simulator (Producer)] --> [Kafka on EC2] --> [Consumer]
    |                                                                  |
    └---> [Kafka Producer using Boto3]                    └---> [Amazon S3] --> [AWS Glue Crawler]
                                                                       ↓
                                                                 [AWS Glue Data Catalog]
                                                                       ↓
                                                                 [Amazon Athena]
```

> For a visual reference, see the included `Architecture.jpg`.

## ⚙️ Installation

1. **Kafka Setup on EC2:**
   ```bash
   wget https://archive.apache.org/dist/kafka/3.3.1/kafka_2.12-3.3.1.tgz
   tar -xvf kafka_2.12-3.3.1.tgz
   sudo yum install java-1.8.0
   java -version
   ```

2. **Start ZooKeeper and Kafka:**
   ```bash
   # ZooKeeper
   bin/zookeeper-server-start.sh config/zookeeper.properties

   # Kafka
   export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"
   bin/kafka-server-start.sh config/server.properties
   ```

3. **Create a Kafka Topic:**
   ```bash
   bin/kafka-topics.sh --create --topic stock --bootstrap-server <EC2_PUBLIC_IP>:9092 --replication-factor 1 --partitions 1
   ```

## 🚀 Usage

1. **Run the Producer:**
   - Located in `Producer.ipynb`
   - Simulates stock data from `indexProcessed.csv`
   - Publishes messages to Kafka topic `stock`

2. **Run the Consumer:**
   - Located in `Consumer.ipynb`
   - Consumes messages from Kafka and writes to S3 bucket

3. **Catalog and Query:**
   - Use AWS Glue Crawler to crawl S3 bucket and build schema in Data Catalog
   - Use Amazon Athena to run SQL queries on the cataloged data

## ⚙️ Configuration

- Update `server.properties` to expose EC2 public IP:
  ```
  advertised.listeners=PLAINTEXT://<EC2_PUBLIC_IP>:9092
  ```

- Make sure the S3 bucket name and region are correctly configured in both producer and consumer notebooks.

## 📦 Dependencies

- Python 3.8+
- Boto3
- Pandas
- Apache Kafka 3.3.1
- AWS SDKs (Glue, Athena, S3)

## 🧪 Examples

```python
# Sample stock data format sent to Kafka
{
  "timestamp": "2025-03-28T14:05:30Z",
  "symbol": "AAPL",
  "price": 156.23
}
```

## 🛠️ Troubleshooting

- **Kafka connection issues?** Check that the EC2 instance's security group allows inbound traffic on port 9092.
- **Data not showing in Athena?** Ensure the AWS Glue Crawler is configured to crawl the correct S3 path and the schema has been created.
- **Producer/Consumer errors?** Confirm Kafka topic name and bootstrap server details are correct.

