Project Overview

This project demonstrates the implementation of a MapReduce job to perform a word count on a dataset using Apache Hadoop. The project utilizes Hadoop’s distributed processing capabilities to efficiently count word occurrences in a given text file.

Approach and Implementation

Mapper Logic

The Mapper class processes each line of input text, tokenizes words, and emits key-value pairs where the key is the word and the value is 1. This allows Hadoop to efficiently distribute and parallelize the word counting process.

Reducer Logic

The Reducer class aggregates the key-value pairs received from the Mapper, summing up the occurrences of each word to produce the final count for each unique word.

Execution Steps

1. Start the Hadoop Cluster
```bash
docker compose up -d
```
2. Build the Code
```bash
mvn install
```
3. Move JAR File to Shared Folder
```bash
mv target/*.jar shared-folder/input/data/
```
4. Copy JAR to Docker Container
```bash
docker cp shared-folder/input/data/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```
5. Move Dataset to Docker Container
```bash
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```
6. Connect to Docker Container
```bash
docker exec -it resourcemanager /bin/bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```
7. Set Up HDFS
```bash
hadoop fs -mkdir -p /input/dataset
hadoop fs -put ./input.txt /input/dataset
```
8. Execute the MapReduce Job
```bash
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/dataset/input.txt /output
```
9. View the Output
```bash
hadoop fs -cat /output/*
```
10. Copy Output from HDFS to Local Machine
```bash
hdfs dfs -get /output /opt/hadoop-3.2.1/share/hadoop/mapreduce/
exit
docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/ shared-folder/output/
```
Challenges Faced & Solutions

Docker Container File Transfer Issues: Initially faced issues with moving files between the host system and the container. Resolved by ensuring proper use of the docker cp command and correct path settings.

HDFS Permissions: Encountered permission errors when writing to HDFS. Resolved by checking directory permissions and ensuring the Hadoop user had the necessary write access.

Memory Management: Large datasets caused memory issues. Optimized performance by tuning Hadoop's memory allocation and parallel processing settings.

Sample Input and Output

input: 
Hadoop is a framework that allows for the distributed processing of large data sets across clusters of computers.
Hadoop is designed to scale up from single servers to thousands of machines, each offering local computation and storage.
Rather than rely on hardware to deliver high-availability, the library itself is designed to detect and handle failures at the application layer.
MapReduce is a core component of the Hadoop ecosystems.

output:
a       2
across  1
allows  1
and     2
application     1
at      1
clusters        1
component       1
computation     1
computers       1
core    1
data    1
deliver 1
designed        2
detect  1
distributed     1
each    1
ecosystems      1
failures        1
for     1
framework       1
from    1
hadoop  3
handle  1
hardware        1
highavailability        1
is      4
itself  1
large   1
layer   1
library 1
local   1
machines        1
mapreduce       1
of      4
offering        1
on      1
processing      1
rather  1
rely    1
scale   1
servers 1
sets    1
single  1
storage 1
than    1
that    1
the     4
thousands       1
to      4
up      1