# Kafka Stock Market Simulation

This project is a data engineering simulation of real-time stock market data using Kafka. It consists of a Producer that streams stock market data into Kafka and a Consumer that reads the data and stores it into AWS S3.

Project Structure

-**.gitignore**: Specifies files to be ignored by Git.
-**consumer.ipynb**: Jupyter notebook for the Kafka Consumer that reads messages from the Kafka topic and writes them to an S3 bucket.
-**producer.ipynb**: Jupyter notebook for the Kafka Producer that streams real-time stock market data from a CSV file into the Kafka topic.
market.csv: Sample stock market data (from Kaggle) used for streaming.
command_kafka.txt: Instructions to set up and run Kafka on an AWS EC2 instance.
Prerequisites
To run this project, you will need the following:

Kafka: Download and install Kafka from Apache Kafka Downloads.
Python: Install the required Python packages using:
bash
Copy code
pip install kafka-python pandas s3fs
AWS EC2 Instance: Set up an EC2 instance to run Kafka.
AWS S3 Bucket: Ensure you have an S3 bucket set up to store consumer data.
Java: Kafka requires Java 8 (or newer) to run.
Setup Instructions
Kafka Setup on AWS EC2
Install Java:

bash
Copy code
sudo yum install java-1.8.0-openjdk
java -version
Download and Install Kafka:

bash
Copy code
wget https://downloads.apache.org/kafka/3.3.1/kafka_2.12-3.3.1.tgz
tar -xvf kafka_2.12-3.3.1.tgz
cd kafka_2.12-3.3.1
Start Zookeeper:

bash
Copy code
bin/zookeeper-server-start.sh config/zookeeper.properties
Start Kafka: Open another terminal and start Kafka:

bash
Copy code
export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"
bin/kafka-server-start.sh config/server.properties
Configure Kafka to Use Public IP: Edit the Kafka server.properties file:

bash
Copy code
sudo nano config/server.properties
Change the ADVERTISED_LISTENERS to the public IP of your EC2 instance.

Create Kafka Topic:

bash
Copy code
bin/kafka-topics.sh --create --topic demo_test --bootstrap-server <Your Public IP>:9092 --replication-factor 1 --partitions 1
Running the Producer
The producer.ipynb notebook reads stock market data from a CSV file (market.csv) and streams the data to a Kafka topic in real-time.

To start the producer:

Open producer.ipynb.
Run the notebook to start streaming the data into the Kafka topic.
Running the Consumer
The consumer.ipynb notebook listens to the Kafka topic and stores the streamed data into an AWS S3 bucket.

To start the consumer:

Open consumer.ipynb.
Run the notebook to start consuming the data and writing it to your S3 bucket.
Bugs & Tips
Amazon Linux 2 AMI vs Amazon Linux 2023 AMI:

IMPORTANT: Make sure to select the same machine image "Amazon Linux 2 AMI (HVM), SSD Volume Type" instead of the default "Amazon Linux 2023 AMI". If you use the wrong image, Kafka WILL get stuck when both the producer and consumer are running, and you might spend hours trying to figure out why. Save yourself the trouble and use "Amazon Linux 2 AMI" right from the start.
Security Group Inbound Settings:

I encountered issues when trying to create Kafka topics. The solution was to change the inbound security group settings to "All Traffic" and "Anywhere-IPv4". After adjusting these settings, restart both Zookeeper and Kafka, then try creating the topic again. This fixed the issue for me.
AWS Pricing:

Be aware of AWS pricing before you start using EC2 and S3 extensively. Check the pricing details on AWS to avoid unexpected costs.
Files
consumer.ipynb: This file contains the Kafka consumer that reads data from the Kafka topic and stores it in an S3 bucket.
producer.ipynb: The Kafka producer reads stock market data from market.csv and sends it to the Kafka topic.
command_kafka.txt: Step-by-step guide for setting up Kafka on an EC2 instance, starting the Zookeeper and Kafka server, and creating Kafka topics.
market.csv: A sample CSV file containing stock market data. You can replace this with your own data or download a dataset from Kaggle.
Data Source
The stock market data used in this project can be sourced from Kaggle. You can either download your own dataset or use the one provided in this project (market.csv).

Future Enhancements
Add support for multiple consumers.
Implement data transformation or aggregation before storing it in S3.
Deploy the consumer as a scalable AWS Lambda function.
Contact
If you have any questions or face any issues with the project, feel free to reach out via email: abhisheikj@gmail.com
