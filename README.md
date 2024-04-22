```bash
git clone https://github.com/big-data-europe/docker-hadoop
cd docker-hadoop
docker-compose up -d
docker exec -it namenode bash
hdfs dfs -ls /
hdfs dfs -mkdir -p /user/root
hdfs dfs -ls /user/
exit
wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-examples/2.7.1/hadoop-mapreduce-examples-2.7.1-sources.jar
wget https://gist.githubusercontent.com/jsdario/6d6c69398cb0c73111e49f1218960f79/raw/8d4fc4548d437e2a7203a5aeeace5477f598827d/el_quijote.txt
docker cp hadoop-mapreduce-examples-2.7.1-sources.jar namenode:/tmp
docker cp el_quijote.txt namenode:/tmp
docker exec -it namenode bash
hdfs dfs -mkdir /user/root/input
cd /tmp
hdfs dfs -put el_quijote.txt /user/root/input
```

Check http://localhost:9870

```bash
hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.WordCount input output
hdfs dfs -cat /user/root/output/*
hdfs dfs -cat /user/root/output/part-r-00000 > /tmp/quijote_wc.txt
exit
docker cp namenode:/tmp/quijote_wc.txt .
docker-compose down
```
