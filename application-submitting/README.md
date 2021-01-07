# Application submitting lab

The goals of this labs are to:
- Submit a pyspark application using `spark-submit`
- Add custom application properties to the `spark-submit`
- Use a `.properties` files to store the properties

## Lab resources

- The `taxi_streaming_analysis.py` pyspark script reads streaming data from a socket and outputs aggregated results in parquet format to a HDFS directory
- The `app.properties` file defines Spark application properties
- The `yarn_kill_app.sh` can be used to easily kill a YARN application using a keyword and a username

## Useful links

- [Spark - Submitting Applications doc](https://spark.apache.org/docs/2.3.2/submitting-applications.html)
- [Spark - Configuration doc](https://spark.apache.org/docs/2.3.2/configuration.html)
- [Spark - Structured Streaming doc](http://spark.apache.org/docs/2.3.2/structured-streaming-programming-guide.html)

## Submitting the application

- Go to the `application-submitting` directory
- Use `spark-submit` to submit the application:
  ```
  spark-submit --master yarn \
  --deploy-mode cluster \
  ./taxi_streaming_analysis.py \
  APP_NAME \
  HOST_ADDRESS \
  HDFS_OUTPUT_DIRECTORY \
  -f PORT_NUMBER
  ```
  `APP_NAME` is a name to show in the list of running applications (you could choose it)
  
  `HDFS_OUTPUT_DIRECTORY` is a directory which will contain the results. If you work on edge server you could use relative path, for example:
  `/user/USERNAME/taxi-streaming-output` or full path `hdfs://HDFS_SERVER_HOST/user/USERNAME/taxi-streaming-output`. 
  Creation of the directory before the run is not necessarily. It will be created automatically. 
  
  `HOST_ADDRESS` and `PORT_NUMBER` show the resource of streaming data. For instance: `edge-1.au.adaltas.cloud` and `11111`.
  
  
- To stop the application using `yarn application`
  - Use the `-list` command to find your application:
    ```
    yarn application -list | grep USERNAME
    ```
  - Kill the application using the `-kill` command and the application ID:
    ```
    yarn application -kill APP_ID
    ```
- Before re-submitting the app, clear the output and the checkpoint directory:
  ```
  hdfs dfs -rm -r /HDFS_OUTPUT_DIRECTORY/*
  hdfs dfs -rm -r /user/USERNAME/checkpoint/*
  ```

## TO DO

1. Modify the `spark_taxi_streaming.py` file with your code from lab 3
2. Submit the application using `spark-submit`
3. Observe the results using Zeppelin (see `ece-2020/spark/ref/lab4` notebook)
4. Add custom application properties to the `spark-submit`
5. Write those properties in a `.properties` file
6. (Bonus) Write a bash script to automate the app submitting + directories cleaning in HDFS
7. (Bonus) Write a bash script to automate the application killing
