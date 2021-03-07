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

    

    
    

