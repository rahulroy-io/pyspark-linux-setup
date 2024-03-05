## PySpark Set Up using Docker Container and AWS Glue

### Steps to install ubuntu container and install pyspark
**Ubuntu Container Setup:**
1. `docker run -it --name uspark -p 2200:22 ubuntu:latest` download docker file and run the file
2. `docker start uspark` Start Docker
3. `docker container attach uspark` Login into Docker
4. ssh setup on container
   - `passwd root` password setup
   - `apt install openssh-server`
   - `nano /etc/ssh/sshd_config` and under `# PermitRootLogin prohibit-password` add `PermitRootLogin Yes`
   - `service ssh restart`
  
5. `ssh root@localhost -p 2200` Connect to the container from the command line or terminal

**pySpark Installation:**
1. Run bellow self descrptive commands in container terminal
```bash
sudo apt install openjdk-8-jdk
mkdir hadoopSpark
wget <get url for spark hadoop https://spark.apache.org/downloads.html >
sudo apt install python
cd hadoopSpark
tar -xvf hadoop-3.2.3.tar.gz
```
2. Update .bashrc contents
```bash
sudo nano ~/.bashrc
# Append the following lines
export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
export SPARK_HOME="/home/ubuntu/temporal/hadoopSpark/spark-3.0.2-bin-hadoop3.2"
export PATH=$PATH:$SPARK_HOME/bin
export PYSPARK_PYTHON=python3
export PYTHONPATH=$SPARK_HOME/python:$PYTHONPATH
export PATH=$PATH:$JAVA_HOME/jre/bin
export SPARK_LOCAL_IP=127.0.0.1
```
3. Run a sample PySpark file (test.py)
```bash
spark-submit test.py
```

### [Glue Container Setup with builtin spark provided by AWS](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-libraries.html#develop-local-python) :

- Run bellow in powershell/command line and make sure mounted path -> `OneDrive\myProjects\dockerBind\awsglue` is available and `port 2200` is avaialble
```bash
docker run -it --name gluecontainer -p 2200:22 -v C:\Users\raulr\OneDrive\myProjects\dockerBind\awsglue:/home/glue_user/workspace/ amazon/aws-glue-libs:glue_libs_4.0.0_image_01
```
- setup sudo user in glue container -> use powershell to login into terminal as root `docker exec -u root -t -i 45bce70e325f /bin/bash` run the following commands in terminal
```bash
yum install -y passwd initscripts nano
usermod -aG wheel glue_user
sudo passwd glue_user 
<provide passwd>

yum install sudo

su using glue_user 
sudo su
<provide passwd>

```
- jupyter server setup
```bash
nano /home/glue_user/jupyter/jupyter_start.sh
#!/bin/bash
# source /home/glue_user/.bashrc
if [[ ! "$?" -eq 1 ]]; then
    livy-server start
    if [[ -z ${DISABLE_SSL} ]]; then
        echo "Starting Jupyter with SSL"
        jupyter lab --no-browser --ip=0.0.0.0 --allow-root --ServerApp.root_dir=/home/glue_user/workspace/jupyter_workspace/ --Serve$
    else
        echo "SSL Disabled"
        jupyter lab --no-browser --ip=0.0.0.0 --allow-root --ServerApp.root_dir=/home/glue_user/workspace/jupyter_workspace/ --Serve$
    fi
fi
```
start Jupiter server with `bash /home/glue_user/jupyter/jupyter_start.sh` and goto loopback ip_address:port (https://127.0.0.1:8888/lab)
- VSCode setup
  connect to container using remote extension in VSCode, in command pallete search -> open workspace settings (JSON) -> Add bellow config JSON as described in aws documentation
  ```bash
  {
    "python.analysis.extraPaths": [
        "/home/glue_user/aws-glue-libs/PyGlue.zip",
        "/home/glue_user/spark/python/lib/py4j-0.10.9-src.zip",
        "/home/glue_user/spark/python/"
    ]
  }
  ```
  
### delta lake setup
   - on terminal run the following commands `python3 -m pip install delta-spark`
   - while initializing spark use the following

    spark = SparkSession.builder\
            .config("spark.sql.extensions", "io.delta.sql.DeltaSparkSessionExtension") \
            .config("spark.sql.catalog.spark_catalog", "org.apache.spark.sql.delta.catalog.DeltaCatalog")\
            .appName('DML').getOrCreate()





### Common Issue
- ***after running ```pyspark``` command in terminal***
```bash
    Python 3.8.5 (default, Jul 28 2020, 12:59:40)
    [GCC 9.3.0] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    21/05/23 01:58:54 WARN Utils: Your hostname, DESKTOP-NVFBPT8 resolves to a loopback address: 127.0.0.1; using 192.168.137.1 instead (on interface eth0)
    21/05/23 01:58:54 WARN Utils: Set SPARK_LOCAL_IP if you need to bind to another address
    21/05/23 01:58:55 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
    Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
    Setting default log level to "WARN".
    To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 WARN Utils: Service 'sparkDriver' could not bind on a random free port. You may check whether configuring an appropriate binding address.
    21/05/23 01:58:56 ERROR SparkContext: Error initializing SparkContext.
    java.net.BindException: Cannot assign requested address: Service 'sparkDriver' failed after 16 retries (on a random free port)! Consider explicitly setting the appropriate       binding address for the service 'sparkDriver' (for example spark.driver.bindAddress for SparkDriver) to the correct binding address.
```
Fix
```bash
pyspark
# Add the following line in terminal
export SPARK_LOCAL_IP=127.0.0.1
```

- ***Issue with Managed Tables:***
  - Encountered an issue with managed tables where data was saved in a different location than specified by `spark.sql.warehouse.directory`.

   - **Resolution Steps:**
     1. **Understand Managed and Unmanaged Tables:**
        - Differentiate between managed and unmanaged tables.
        - Use `spark.sql.warehouse.directory` for specifying warehouse locations.
   
     2. **Path Precedence in Glue Container:**
        - Be aware that Glue catalog metadata takes precedence over local configurations.
        - This affects both managed and unmanaged tables, influencing data-saving locations.
   
     3. **Glue Container Setup Steps:**
        - **Create a Database:**
          - Establish databases for on-premises (local) and default (S3).
          - Utilize local paths like "path/to/local/directory" for on-prem databases.
   
        - **Configure Database Paths:**
          - Set up local paths for on-prem databases.
          - Glue uses the database path to organize data, creating a structure for table names.
