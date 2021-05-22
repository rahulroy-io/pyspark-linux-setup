https://towardsdatascience.com/installing-pyspark-with-java-8-on-ubuntu-18-04-6a9dea915b5b

Installation Commands

    sudo apt install openjdk-8-jdk
    mkdir /home/ubuntu/temporal/hadoopSpark
    wget <url hadoop>
    sudo apt install python

.bashrc contents

    sudo nano .bashrc
    export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
    export SPARK_HOME="/home/ubuntu/temporal/hadoopSpark/spark-3.0.2-bin-hadoop3.2"
    export PATH=$PATH:$SPARK_HOME/bin
    export PYSPARK_PYTHON=python3
    export PYTHONPATH=$SPARK_HOME/python:$PYTHONPATH
    export PATH=$PATH:$JAVA_HOME/jre/bin

    
To run a sample file test.py

    spark-submit test.py
    

Issue
    
    pyspark
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
    
    
Fix

    add in .bashrc contents
    export SPARK_LOCAL_IP=127.0.0.1
