# Download Docker file and run the container
docker run -it --name uspark -p 2200:22 ubuntu:latest

# Start Docker
docker start uspark

# Login into Docker
docker container attach uspark

# SSH setup on the container
passwd root
apt install openssh-server
nano /etc/ssh/sshd_config
# PermitRootLogin prohibit-password
PermitRootLogin Yes
service ssh restart

# Connect to the container from the command line or terminal
ssh root@localhost -p 2200

# Installation Commands in Docker Container shell
sudo apt install openjdk-8-jdk
mkdir hadoopSpark
# Replace <get url for spark hadoop https://spark.apache.org/downloads.html > with the actual URL
wget <get url for spark hadoop https://spark.apache.org/downloads.html >
sudo apt install python
cd hadoopSpark
tar -xvf hadoop-3.2.3.tar.gz

# Update .bashrc contents
sudo nano ~/.bashrc
export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
export SPARK_HOME="/home/ubuntu/temporal/hadoopSpark/spark-3.0.2-bin-hadoop3.2"
export PATH=$PATH:$SPARK_HOME/bin
export PYSPARK_PYTHON=python3
export PYTHONPATH=$SPARK_HOME/python:$PYTHONPATH
export PATH=$PATH:$JAVA_HOME/jre/bin
export SPARK_LOCAL_IP=127.0.0.1

# Run a sample PySpark file (test.py)
spark-submit test.py

# Issue Fix
pyspark
# Add the following line in the interactive PySpark shell
export SPARK_LOCAL_IP=127.0.0.1

# GLUE
docker run -it --name gluecontainer -p 2200:22 -v C:\Users\raulr\OneDrive\myProjects\dockerBind\awsglue:/home/glue_user/workspace/ amazon/aws-glue-libs:glue_libs_4.0.0_image_01

# Delta Lake
python3 -m pip install delta-spark

# Python script using Delta Lake in Spark
spark = SparkSession.builder\
    .config("spark.sql.extensions", "io.delta.sql.DeltaSparkSessionExtension") \
    .config("spark.sql.catalog.spark_catalog", "org.apache.spark.sql.delta.catalog.DeltaCatalog")\
    .appName('DML').getOrCreate()
