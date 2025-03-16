# Step 1: Copy the CSV file to the Namenode container
docker cp /mnt/sda2/WorkSpace/Hadoop-demo/singapore_taxi_bookings.csv namenode:/tmp/singapore_taxi_bookings.csv

# Step 2: Upload the CSV file to HDFS
docker exec -it namenode /bin/bash
hdfs dfs -mkdir -p /user/hadoop/input
hdfs dfs -put /tmp/singapore_taxi_bookings.csv /user/hadoop/input/
exit

# Step 3: Compile the Maven project
cd MapReduce
mvn clean package

# Step 4: Copy the compiled JAR file to the Namenode container
docker cp MapReduce/MapReduce/target/taxi-distance-calculator-1.0-0.jar namenode:/tmp/TaxiDistanceCalculator.jar

# Step 5: Execute the MapReduce job
docker exec -it namenode /bin/bash
hadoop jar /tmp/TaxiDistanceCalculator.jar com.demo.TaxiDistanceCalculator /user/hadoop/input/singapore_taxi_bookings.csv /user/hadoop/output
exit