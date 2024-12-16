# pySpark-101
To learn PySpark and all the essentials, it's crucial to understand its foundation, architecture, and functionality. Here's a structured guide to help you get started and become proficient in PySpark.

## Definations

1. **Transformation**
> A method on a dataframe which returns another dataframe.

2. **Action**
> A method on a dataframe which returns a value.

Spark has a great breakdown of which operations are classified as transformations and actions.

3. **Catalyst Optimizer**
> A Spark mechanism which assesses all the transformations it has to run, and figures out the most efficient way to run them together.

This concept is very important, as when paired with lazy evaluation, it speeds up your data pipelines! 

_Note: The catalyst optimizer does not apply to Resilient Distributed Datasets (RDDs) - only dataframes and datasets. We’ll be focusing on dataframes in this article._

---

## **Step 1: Understand Spark Basics**
1. **What is Apache Spark?**
   - Open-source distributed computing framework.
   - Used for large-scale data processing.
   - Supports multiple languages (Python, Java, Scala, R).
   - Fast in-memory computation.

2. **Spark Components:**
   - **Core**: Handles basic distributed processing and task scheduling.
   - **SQL**: SQL queries on structured data.
   - **Streaming**: Real-time data processing.
   - **MLlib**: Machine learning library.
   - **GraphX**: Graph computation.

3. **Spark Architecture:**
   - **Driver**: Coordinates the execution.
   - **Executor**: Executes tasks on worker nodes.
   - **Cluster Manager**: Manages resources (e.g., YARN, Mesos, Standalone).

---

## **Step 2: Setting Up PySpark**
1. **Install PySpark:**
   ```bash
   pip install pyspark
   ```
2. **Verify Installation:**
   ```bash
   python -c "import pyspark; print(pyspark.__version__)"
   ```
3. **Run PySpark Shell:**
   ```bash
   pyspark
   ```
4. **Set Up in IDEs** (Jupyter, VSCode):
   - Configure `SPARK_HOME` and `PYTHONPATH`.
   - Example for Jupyter:
     ```bash
     pip install findspark
     ```
     ```python
     import findspark
     findspark.init()
     ```

---

## **Step 3: Core PySpark Concepts**
1. **Resilient Distributed Dataset (RDD):**
   - Immutable distributed collections of objects.
   - [Lazy evaluation](https://medium.com/@john_tringham/spark-concepts-simplified-lazy-evaluation-d398891e0568) and fault-tolerant.
   ```python
   from pyspark import SparkContext
   sc = SparkContext()
   rdd = sc.parallelize([1, 2, 3, 4])
   print(rdd.collect())
   ```

Additional Reading [Caching, Persistance and Checkpoints](https://medium.com/@john_tringham/spark-concepts-simplified-cache-persist-and-checkpoint-225eb1eef24b)

2. **DataFrame:**
   - Distributed collection of data organized into named columns (like SQL tables).
   ```python
   from pyspark.sql import SparkSession
   spark = SparkSession.builder.appName("Example").getOrCreate()
   data = [(1, "Alice"), (2, "Bob")]
   df = spark.createDataFrame(data, ["id", "name"])
   df.show()
   ```

3. **Transformations vs Actions:**
   - **Transformations**: Lazy operations (e.g., `map`, `filter`).
   - **Actions**: Trigger execution (e.g., `collect`, `count`).

---

## **Step 4: Essential PySpark Operations**
1. **RDD Operations:**
   - **Transformations:**
     ```python
     rdd = sc.parallelize([1, 2, 3, 4])
     squared = rdd.map(lambda x: x * x)
     print(squared.collect())
     ```
   - **Actions:**
     ```python
     print(rdd.reduce(lambda x, y: x + y))
     ```

2. **DataFrame Operations:**
   - **Basic Operations:**
     ```python
     df.select("name").show()
     df.filter(df.id > 1).show()
     ```
   - **SQL Queries:**
     ```python
     df.createOrReplaceTempView("people")
     spark.sql("SELECT * FROM people").show()
     ```

3. **Joins:**
   ```python
   df1 = spark.createDataFrame([(1, "Alice"), (2, "Bob")], ["id", "name"])
   df2 = spark.createDataFrame([(1, "HR"), (2, "IT")], ["id", "dept"])
   df1.join(df2, "id").show()
   ```

---

## **Step 5: Advanced PySpark Topics**
1. **Spark Streaming:**
   - Process real-time data from sources like Kafka.
   ```python
   from pyspark.sql.functions import split
   lines = spark.readStream.format("socket").option("host", "localhost").option("port", 9999).load()
   words = lines.select(split(lines.value, " ").alias("word"))
   query = words.writeStream.format("console").start()
   query.awaitTermination()
   ```

2. **Machine Learning with MLlib:**
   - Example of logistic regression:
     ```python
     from pyspark.ml.classification import LogisticRegression
     from pyspark.ml.feature import VectorAssembler
     data = spark.createDataFrame([(0, 0.0, 1.0), (1, 1.0, 0.0)], ["label", "feature1", "feature2"])
     assembler = VectorAssembler(inputCols=["feature1", "feature2"], outputCol="features")
     output = assembler.transform(data)
     lr = LogisticRegression(featuresCol="features", labelCol="label")
     model = lr.fit(output)
     model.transform(output).show()
     ```

3. **Partitioning:**
   - Efficient data shuffling and processing.

4. **Optimization:**
   - Caching, persistence, and broadcasting variables.

---

## **Step 6: Building Real-Time Projects**
1. **ETL Pipelines:**
   - Extract data from sources (e.g., CSV, databases).
   - Transform data (e.g., filter, group, aggregate).
   - Load data into a target system.

2. **Visualization Integration:**
   - Use PySpark with tools like Tableau or Grafana for visualization.

3. **Custom Project Ideas:**
   - Real-time log processing and anomaly detection.
   - Batch processing for data warehouses.
   - E-commerce recommendation systems.

---

## **Step 7: Learn by Doing**
- Practice with datasets from [Kaggle](https://www.kaggle.com/).
- Create projects that mimic real-world scenarios.
- Use distributed clusters for hands-on experience.

---

### **Resources**
- **Documentation:** [PySpark Docs](https://spark.apache.org/docs/latest/api/python/)
- **Books:**
  - *Learning Spark* by Holden Karau.
- **Courses:**
  - Coursera: *Big Data Analysis with Spark*.
