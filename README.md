# ![Colaberry School of Data Analytics] (http://www.colaberry.com/assets/img/colaberrylogosmall.png) + ![TPC] (http://www.tpc.org/tpc_common_library/images/TPC_logo.gif) + ![Hortonworks Data Platform](http://hortonworks.com/wp-content/themes/hortonworks/images/layout/header/hortonworks-logo.png) + ![Google Cloud Platform](https://cloud.google.com/_static/images/gcp-logo.png) + ![Hive] (https://hive.apache.org/images/hive_logo_medium.jpg)




hive-testbench
==============

A testbench for experimenting with Apache Hive at any data scale.

Overview
========

The hive-testbench is a data generator and set of queries that lets you experiment with Apache Hive at scale. The testbench allows you to experience base Hive performance on large datasets, and gives an easy way to see the impact of Hive tuning parameters and advanced settings.

Prerequisites
=============

You will need:
* Hadoop 2.2 or later cluster or Sandbox.
* Apache Hive.
* Between 15 minutes and 2 days to generate data (depending on the Scale Factor you choose and available hardware).
* If you plan to generate 1TB or more of data, using Apache Hive 13+ to generate the data is STRONGLY suggested.

Install and Setup
=================

All of these steps should be carried out on your Hadoop cluster.

Step 1: Prepare your environment.
In addition to Hadoop and Hive, before you begin ensure gcc is installed and available on your system path. If you system does not have it, install it using the fallowing commands.
Command: sudo yum install gcc*


 
Step 2: Decide which test suite(s) you want to use.
hive-testbench comes with data generators and sample queries based on both the TPC-DS and TPC-H benchmarks. You can choose to use either or both of these benchmarks for experiementation. More information about these benchmarks can be found in the fallowing link.
 http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/

Step 3: Download the code for hive-testbench from git hub.

	3.A.	If you do not install git on your machine,use the following command to install it.

		Command: $sudo yum install git

 


	3.B.	If you have already installed git, skip step A and download the code for hive-testbench from git using the 			fallowing command.

		Command: $git clone https://github.com/hortonworks/hive-testbench

 

Step 4: Change the directory to hive-testbench.

Command: cd hive-testbench

 

Check the list of files in hive-testbench

Command: ls -l

 
Step 5: Compile and package the appropriate data generator.

	5.A.	For TPC-DS, ./tpcds-build.sh downloads, compiles and packages the TPC-DS data generator. 
		Command: ./tpcds-build.sh
 
 
	5.B.	For TPC-H, ./tpch-build.sh downloads, compiles and packages the TPC-H data generator.
		Command: ./tpch-build.sh
 
 
Step 6: Setup mapreduce path.

	6.A.	Change the directory to home from hive-testbench.
		Command: cd
 
	6.B.	Change the user to root.
		Command: sudo su
 
	6.C.	Create a directory /hdp/apps/2.2.4.2-2/mapreduce in hdfs using the fallowing command.
		Command: sudo -u hdfs hdfs dfs -mkdir -p /hdp/apps/2.2.4.2-2/mapreduce/ 
 
	6.D.	Move the file /usr/hdp/2.2.4.2-2/hadoop/mapreduce.tar.gz from local to /hdp/apps/2.2.4.2-2/mapreduce/ in 			hdfs.
		Command: sudo -u hdfs hdfs dfs -put /usr/hdp/2.2.4.2-2/hadoop/mapreduce.tar.gz  hdp/apps/2.2.4.2-2/mapreduce/
 
	6.E.	Move the file /usr/hdp/2.2.4.2-2/hadoop-mapreduce/hadoop-streaming.jar from local to 						/hdp/apps/2.2.4.2-2/mapreduce/ in hdp.
		Command: sudo -u hdfs hdfs dfs -put /usr/hdp/2.2.4.2-2/hadoop-mapreduce/hadoop-streaming.jar 					/hdp/apps/2.2.4.2-2/mapreduce/

 

	6.F.	Change the Owner ship and Permissions for /hdp/apps/2.2.4.2-2/mapreduce.
		6.F.1.	Changing the ownership for /hdp
			Command: sudo -u hdfs hdfs dfs -chown -R hdfs:hadoop /hdp
 
		6.F.2.	Changing the permissions for /hdp/apps/2.2.4.2-2/mapreduce.
			Command: sudo –u hdfs hdfs dfs –chmod –R 555 /hdp/apps/2.2.4.2-2/mapreduce
 
		6.F.3.	Changing the permissions for /hdp/apps/2.2.4.2-2/mapreduce/mapreduce.tar.gz
			Command: sudo -u hdfs hdfs dfs -chmod -R 444 /hdp/apps/2.2.4.2-2/mapreduce/mapreduce.tar.gz
 
	6.G.	Exit from the root user.
		Command: exit
 
Step 7: Decide how much data you want to generate.

You need to decide on a "Scale Factor" which represents how much data you will generate. Scale Factor roughly translates to gigabytes, so a Scale Factor of 100 is about 100 gigabytes and one terabyte is Scale Factor 1000. Decide how much data you want and keep it in mind for the next step. If you have a cluster of 4-10 nodes or just want to experiment at a smaller scale, scale 1000 (1 TB) of data is a good starting point. If you have a large cluster, you may want to choose Scale 10000 (10 TB) or more. The notion of scale factor is similar between TPC-DS and TPC-H.
If you want to generate a large amount of data, you should use Hive 13 or later. Hive 13 introduced an optimization that allows far more scalable data partitioning. Hive 12 and lower will likely crash if you generate more than a few hundred GB of data and tuning around the problem is difficult. You can generate text or RCFile data in Hive 13 and use it in multiple versions of Hive.
In this case, My suggestion is do not do it more then 10(10GB), otherwise it will take you lots of time for generate and load the data.

Step 8: Generate and load the data.

The scripts tpcds-setup.sh and tpch-setup.sh generate and load data for TPC-DS and TPC-H, respectively. General usage is tpcds-setup.sh scale_factor [directory] or tpch-setup.sh scale_factor [directory]

	8.A.	Change the directory to hive-testbench.
		Command: cd hive-testbench
 
	8.B.	Loading the 10GB of data for TPC-DS.
		Command: ./tpcds-setup.sh 10
 
		It will download all the tables data into a database.
 

	8.C.	Loading 10GB data for TPCH-H.
		Command: ./tpch-setup.sh 10
 
		It will download the table’s data into database.
 





Step 9: Running the Queries.

More than 50 sample TPC-DS queries and all TPC-H queries are included to try. You can use hive, beeline or the SQL tool of your choice. Note that the database is named based on the Data Scale chosen in step 7. At Data Scale 10, your database will be named tpcds_bin_partitioned_orc_10. At Data Scale 1000 it would be named tpcds_bin_partitioned_orc_1000. You can always use the below command to get a list of available databases.
Command: show databases

	9.A.	Running a sample query in TPC-DS.

		9.A.1.	Change the directory.

			Command: cd

 

			Change the directory to hive-testbench.
	
			Command: cd hive-testbench	
	 
	
		9.A.2.	Change the directory to sample-queries-tpcds.

			Command: cd sample-queries-tpcds

 

		9.A.3.	You can use the fallowing command to find the list of queries available in TPC-DS to Execute.

			Command: ls -l

 

			To view the source code of the query use the fallowing command.

			Command: cat query12.sql

 
		9.A.4.	Connect to hive.

			Command: hive

 

		9.A.5.	Check the list of database available.

			Command: show databases;

 

		9.A.6.	Use the database that contain all the tables data.

			Command: use tpcds_bin_partitioned_orc_10;

 

		9.A.7.	The fallowing command is used to Execute the query.

			Command: source query15.sql;

			Decription of Query15.sql: Report the total catalog sales for customers in selected geographical 				regions or who made large purchases for a given year and quarter.

			The description of each query can be found in the fallowing link.

			http://www.slideshare.net/hortonworks/apache-hive-013-performance-benchmarks

 
	
	 

 

			Similar way you can execute all the queries in TPC-DS.

	9.B.	Running a sample query in TPC-H.

		9.B.1.	Change the directory.

			Command: cd

 

			Change the directory to hive-testbench.
	
			Command: cd hive-testbench		 

		9.B.2.	Change the directory to sample-queries-tpch.

			Command: cd sample-queries-tpch

 

		9.B.3.	You can use the fallowing command to find the list of queries available in TPC-H to Execute.

			Command: ls -l

 
			To view the source code of the query use the fallowing command.
			Command: cat tpch_query1.sql
 


		9.B.4.	Connect to hive.

			Command: hive

 

		9.B.5.	Check the list of database available.

			Command: show databases;

 

		9.B.6.	Use the database that contain all the tables data.

			Command: use tpch_flat_orc_10;

 

		9.B.7.	The fallowing command is used to Execute the query.

			Command: source tpch_query1.sql;

 

 

			Similar way you can execute all the queries in TPC-H.



Feedback
========

If you have questions, comments or problems, visit the [Hortonworks Hive forum](http://hortonworks.com/community/forums/forum/hive/).

If you have improvements, pull requests are accepted.
