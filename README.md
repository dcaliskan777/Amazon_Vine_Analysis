# Amazon_Vine_Analysis
This is a project analyzing Amazon reviews written by members of the paid Amazon Vine program.

## Overview

In this project, I have picked up software products ![](https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Software_v1_00.tsv.gz) from approximately 50 datasets ![](https://s3.amazonaws.com/amazon-reviews-pds/tsv/index.txt). Each one contains reviews of a specific product, from clothing apparel to wireless products. I have used PySpark to perform the ETL process to extract the dataset, transform the data, connect to an AWS RDS instance, and load the transformed data into pgAdmin. Then, I have used PySpark to determine if there is any bias toward favorable reviews from Vine members in the dataset.

The results are presented and a summary of the analysis has written. 

### Purpuse

The purpuse of the project is to crate four tables which are custenmers, products, Review_id, vine; to apload them in a AWS database by pgAdmin; and to analyse vine table.


## Results

### Perform ETL on Amazon Software Product Reviews

Database named database16 are created in AWS and the scheme of four tables are created in pgAdmin bey the following quiry:

![](resources/pg_1.jpg)

The dataset was extracted by the code

> from pyspark import SparkFiles
>
> url = "https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Software_v1_00.tsv.gz"
> 
> spark.sparkContext.addFile(url)
> 
> df = spark.read.option("encoding", "UTF-8").csv(SparkFiles.get("amazon_reviews_us_Software_v1_00.tsv.gz"), >sep="\t", header=True, inferSchema=True)\

Data frame customers_df was created by the code

> customers_df = df.groupby("customer_id").count().withColumnRenamed("count", "customer_count")

The first 20 rows of the dataframe is given below.

![](resources/customers.jpg)

Data frame products_df was created by the code

> products_df = df.select(["product_id","product_title"]).drop_duplicates()

The first 20 rows of the dataframe is given below.

![](resources/products.jpg)

Data frame review_id_df was created by the code

> review_id_df = df.select(["review_id","customer_id","product_id","product_parent", to_date("review_date", 'yyyy-> MM-dd').alias("review_date")])

The first 20 rows of the dataframe is given below.

![](resources/review_id.jpg)

Data frame vine_df was created by the code

> vine_df = df.select(["review_id","star_rating","helpful_votes","total_votes","vine","verified_purchase"])

The first 20 rows of the dataframe is given below.

![](resources/vine.jpg)



## Conclusion

