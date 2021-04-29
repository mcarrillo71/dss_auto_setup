Lab : Spark Standalone Integration
1. In this lab, we’ll set up and test a Spark Standalone integration. It will allow you to run spark locally on the DSS server. This is only useful for testing purposes. This is not a production setup, since in order to leverage fully Spark capabilities you would need to connect to a Spark cluster in order to distribute your workload on multiple nodes. After the integration, we’ll log into DSS and verify that all is working well.
2. Stop dss.
3. Download the Hadoop and Spark Standalone integrations
cd​ /data/dataiku ./data_dir_design/bin/dss stop
export DSS_VERSION="<DSS_VERSION>"
# Example:
# export DSS_VERSION="9.0.1"
#wget https://cdn.downloads.dataiku.com/public/dss/"$DSS_VERSION"/dataiku-dss-hadoop3-standalone-libs-generic-hadoop3-"$DSS_VERSION".tar.gz
wget https://cdn.downloads.dataiku.com/public/dss/9.0.1/dataiku-dss-hadoop-standalone-libs-generic-hadoop3-9.0.1.tar.gz
#wget https://cdn.downloads.dataiku.com/public/dss/"$DSS_VERSION"/dataiku-dss-spark-standalone-"$DSS_VERSION"-3.0.1-generic-hadoop3.tar.gz
wget https://cdn.downloads.dataiku.com/public/dss/9.0.1/dataiku-dss-spark-standalone-9.0.1-3.0.1-generic-hadoop3.tar.gz

4. Now we’re ready to run the Hadoop and Spark integration script.
./data_dir_design/bin/dssadmin install-hadoop-integration -standalone generic-hadoop3 -standaloneArchive /data/dataiku/dataiku-dss-hadoop3-standalone-libs-generic-"$DSS_VERSION" .tar.gz
./data_dir_design/bin/dssadmin install-spark-integration
-standaloneArchive /data/dataiku/dataiku-dss-spark-standalone-"$DSS_VERSION"-2.4.3-generic- hadoop3.tar.gz
It should take a few seconds to complete the integration. If successful, you should see a message like this at the end of the Hadoop integration:
A similar message is written at the end of the Spark integration:
[2020/03/19-16:42:27.017] [main] [INFO] [x] - **** Hadoop flavor ****
[GLOBAL] [no:auth] CLI: Enabled Hadoop integration (from command-line) : - /general-settings.json
- /connections.json
4
  {"flavor":"generic","hiveSupportsMREngine":false,"hive3":false} [2020/03/19-16:42:27.054] [main] [INFO] [dip.commitbehavior] - Candidate commit [GLOBAL] [no:auth] CLI: Enabled Spark (from command-line) :
- /general-settings.json
5. Now we can run a spark test. Copy the following into the terminal and hit enter to execute a spark job, it will test the spark standalone we just installed
 export DSS_VERSION="<DSS_VERSION>"
# Example
export DSS_VERSION="9.0.1" /data/dataiku/dataiku-dss-"$DSS_VERSION"/spark-standalone-home/bin/spark -submit --class org.apache.spark.examples.SparkPi --master local --deploy-mode client ./dataiku-dss-"$DSS_VERSION"/spark-standalone-home/examples/jars/spark-e xamples_2.11-2.4.3.jar
When the job finishes, you should see a line showing an approximation of the value of pi, as shown below:
 This demonstrates that the DSS host is able to communicate with Spark, so we are ready to do the installation!
6. Restart dss and then navigate to the frontend UI and login as the admin user.
./data_dir_design/bin/dss start
7. There’s one thing we’ll want to change in DSS for the spark configuration. Login as the ADMIN_USER and go to ADMIN > SETTINGS > SPARK. Scroll down to the “default” Spark Configuration. Add a key of “spark.master” with the value “local”. Then click “OK” and Save the Spark settings.
 5

  8. Now we can test that spark works. Log into DSS as your data science user and open the Basketball Project. Towards the end of the flow you’ll find a SparkSql recipe between “Baksetball_nScores” AND “Basketball_nScores_Prepared”. Click the dataset just after the recipe and select “build”. We know our input data set exists, so we can run this one non-recursive. Click Build Dataset and allow the job to finish.
 1. When the job is finished, go back to your Notebooks (Click Notebooks in the menu or ‘G+N’ on the keyboard) and create a new python notebook with the pyspark template.
6

 2. Modify the line below to include the name of your dataset in hdfs, then select Kernel > Restart and Run All to execute the code.
mydataset = dataiku.Dataset(​"Basketball_nScores"​) The results should look like the following:
3. We’ve validated that Spark is set up correctly. Congratulations! This is the end of the exercise.
