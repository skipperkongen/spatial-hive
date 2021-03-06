# HOWTO

This README file explains how to get the experiment described in the report for this project up and running. It is assumed that the operating system is Ubuntu or another linux distrubtion that supports the command tools used below. 

Download and unpack the following files

```
wget http://mirrors.dotsrc.org/apache/hadoop/common/hadoop-1.0.4/hadoop-1.0.4.tar.gz
tar xzf hadoop-1.0.4.tar.gz

wget http://ftp.download-by.net/apache/hive/hive-0.8.1/hive-0.8.1.tar.gz
tar xzf hive-0.8.1.tar.gz

wget http://ftp.download-by.net/apache/mahout/0.7/mahout-distribution-0.7.tar.gz
tar xzf mahout-distribution-0.7.tar.gz
```

Get third party libraries

```
wget http://www.vividsolutions.com/jts/bin/jts-1.8.0.zip
unzip jts-1.8.0.zip

wget https://github.com/downloads/hunterhacker/jdom/jdom-2.0.4.zip
unzip jdom-2.0.4.zip
mv jdom-2.0.4.jar /path/to/your/hadoop-1.0.4/lib/jdom-2.0.4.jar
mv lib/jts-1.8.jar /path/to/your/hadoop-1.0.4/lib/jts-1.8.jar
```

Set the following environment variables - This can also be done in ~/.bashrc. If set in ~/.bashrc, remember to call 'source ~/.bashrc' afterwards

```
export JAVA_HOME=/path/to/your/java
export HADOOP_INSTALL=/path/to/your/hadoop-1.0.4
export PATH=$PATH:$HADOOP_INSTALL/bin:$HADOOP_INSTALL/sbin
```

Check that hadoop is installed:

```
hadoop version
```

Follow the guide explained here:

[http://hadoop.apache.org/docs/r0.20.2/quickstart.html#PseudoDistributed](hadoop.apache.org/docs/r0.20.2/quickstart.html)

Get data set

````
wget http://download.cloudmade.com/europe/northern_europe/denmark/bornholm/bornholm.osm.bz2
bunzip2 bornholm.osm.bz2
```

Files from this project

```
mv OSM.jar /path/to/your/hadoop-1.0.4/OSM.jar
mv init.sql /path/to/your/hive-0.8.1/init.sql
mv runQuery.sql /path/to/your/hive-0.8.1/runQuery.sql
```

From hive-0.8.1 folder

```
mkdir auxjars
mv myUDF.jar /path/to/your/hive-0.8.1/auxjars/myUDF.jar
mv hadoop-env.sh /path/to/your/hadoop-1.0.4/conf/hadoop-env.sh
```

In hadoop-env.sh you need to set the path to MAHOUT_HOME and HIVE_HOME


Replace planet.osm with the file osm file you have downloaded from - 

```
hadoop fs -copyFromLocal planet.osm ./planet.osm
hadoop jar ./OSM.jar osmParsing.RunAll ./bornholm.osm
```

Create the directories for Hive

```
hadoop fs -mkdir /tmp
hadoop fs -mkdir /user/hive/warehouse
hadoop fs -chmod g+w /tmp
hadoop fs -chmod g+w /user/hive/warehouse
```

# Hive

```
cd /path/to/your/hive-0.8.1
hive -f init.sql
hive -f runQuery.sql
```

