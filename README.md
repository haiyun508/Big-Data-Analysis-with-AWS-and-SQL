# "Alexa, can you handle big data?"

## Background

Many of Amazon's shoppers depend on product reviews to make a purchase. Amazon makes these datasets publicly available. However, they are quite large and can exceed the capacity of local machines to handle. One dataset alone contains over 1.5 million rows; with over 40 datasets, this can be quite taxing on the average local computer. Our first goal for this project will be to perform the ETL process completely in the cloud and upload a DataFrame to an RDS instance. The second goal will be to use PySpark or SQL to perform a statistical analysis of selected data.

1. Create DataFrames to match production-ready tables from two big Amazon customer review datasets.
2. Analyze whether reviews from Amazon's Vine program are trustworthy.

- - -

### Step 1

* Use the furnished schema to create tables in our RDS database.

* Create two separate Google Colab notebooks and **extract** any two datasets from the list at [review dataset](https://s3.amazonaws.com/amazon-reviews-pds/tsv/index.txt), one into each notebook.

* For each notebook (one dataset per notebook), we did the following:

  * Count the number of records (rows) in the dataset.

  * **Transform** the dataset to fit the tables in the [schema file](../Resources/schema.sql). DataFrames should be able to match in data type and in column name.

  * **Load** the DataFrames that correspond to tables into an RDS instance. 

### Step 2 
In Amazon's Vine program, reviewers receive free products in exchange for reviews.

  ![vine01.png](Images/vine01.png)

Amazon has several policies to reduce the bias of its Vine reviews: [https://www.amazon.com/gp/vine/help?ie=UTF8](https://www.amazon.com/gp/vine/help?ie=UTF8).

But are Vine reviews truly trustworthy? We try to investigate whether Vine reviews are free of bias. We use PySpark and SQL to analyze the data.

* If you choose to use SQL, first use Spark on Colab to extract and transform the data and load it into a SQL table on your RDS account. Perform your analysis with SQL queries on RDS.



- - -

## Resources

[customer review datasets](https://s3.amazonaws.com/amazon-reviews-pds/tsv/index.txt)

- - -

## Hints and Considerations

* start each notebook with following code in the first cell and update the Spark version.

```python
import os
# Find the latest version of spark 3.0  from http://www-us.apache.org/dist/spark/ and enter as the spark version
# For example:
# spark_version = 'spark-3.0.1'
spark_version = 'spark-3.<spark version>'
os.environ['SPARK_VERSION']=spark_version

# Install Spark and Java
!apt-get update
!apt-get install openjdk-11-jdk-headless -qq > /dev/null
!wget -q http://www-us.apache.org/dist/spark/$SPARK_VERSION/$SPARK_VERSION-bin-hadoop2.7.tgz
!tar xf $SPARK_VERSION-bin-hadoop2.7.tgz
!pip install -q findspark

# Set Environment Variables
import os
os.environ["JAVA_HOME"] = "/usr/lib/jvm/java-11-openjdk-amd64"
os.environ["SPARK_HOME"] = f"/content/{spark_version}-bin-hadoop2.7"

# Start a SparkSession
import findspark
findspark.init()
```

* For connection to Postgres run the following code in the next cell.

```python
!wget https://jdbc.postgresql.org/download/postgresql-42.2.9.jar
```

