Prac 1 Data Extraction Using Web Scraping

'''import pandas as pd
import requests
from bs4 import BeautifulSoup

url = "https://www.worldometers.info/world-population/population-by-country/"
src = requests.get(url)
print("Status code:", src.status_code)
soup = BeautifulSoup(src.content, "lxml")
table = soup.find("table")

 #Extract headers
cols = [th.get_text(strip=True) for th in table.find("thead").find_all("th")]

 #Extract rows
rows = table.find("tbody").find_all("tr")

data = []
for r in rows:
    row = [td.get_text(strip=True) for td in r.find_all("td")]
    data.append(row)

 #Create dataframe
df = pd.DataFrame(data, columns=cols)
print(df.head())'''

Prac 2 Create ML pipeline(workflow) using python libraries

'''# Import libraries
import pandas as pd
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.naive_bayes import GaussianNB
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score

# Load dataset
data = load_breast_cancer()

# Convert to DataFrame
df = pd.DataFrame(data.data, columns=data.feature_names)
df["class"] = data.target

# Features and target
X = df.drop("class", axis=1)
y = df["class"]

# Train test split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.25, random_state=42
)

# Create pipelines for models
pipelines = {
    "Logistic Regression": Pipeline([
        ("scaler", StandardScaler()),
        ("pca", PCA(n_components=3)),
        ("model", LogisticRegression())
    ]),
    "Decision Tree": Pipeline([
        ("scaler", StandardScaler()),
        ("pca", PCA(n_components=3)),
        ("model", DecisionTreeClassifier())
    ]),
    "Naive Bayes": Pipeline([
        ("scaler", StandardScaler()),
        ("pca", PCA(n_components=3)),
        ("model", GaussianNB())
    ]),
    "Random Forest": Pipeline([
        ("scaler", StandardScaler()),
        ("pca", PCA(n_components=3)),
        ("model", RandomForestClassifier())
    ])
}


 # Train models and check accuracy
for name, model in pipelines.items():
    model.fit(X_train, y_train)
    y_pred = model.predict(X_test)
    acc = accuracy_score(y_test, y_pred)
    print(name, "Accuracy:", acc)'''

Practical No 3 Working with NoSQL database – Hbase
Creating a Linkshare in hbase
$ hbase shell
>create ‘studentrj’,’studid’,’studname’,’studaddr’
>list
>describe ’studentrj’
>put ‘studentrj’,’1’,’studid:id’,101
>put ‘studentrj’,1’,’studname:name’,’Kartik’
>put ‘studentrj’,’1’’studaddr:addr’,’Mumbai’
>scan ‘studentrj

1. Get the value for column place for student with id 1 using the column key word . 
>get ‘studentdata’ , ‘1’ ,{COLUMN => ‘studentaddress : place’ }

2. Retrieve all column values for student with id 1 
>get ‘studentrj’ , ‘1’

3.Get the records for student with id 2 , 3 , and 4 
>get ‘studentrj’ , ‘02’ 
>get ‘studentrj’ , ‘03’ 
>get ‘studentrj’ , ‘04’
4.Get all the column values for column family student address for student with id=1 without using columns keyword 
>get ‘studentrj’ , ‘1’ , ‘studaddress’

5.Change the lastname for studentid 1 from original to Kelar and then check the record to see if the changes are done 
>put ‘studentrj’ , ‘1’ , ‘studname:lname’ , ‘Kelkar’

#Perfrom the following processes on the student table

1.Count the number of records 
>count ‘studentrj’

2.Disable the table 
>disable studentrj’

3.Verify if the table is disabled 
>is_disabled ‘studentrj’

4.Enable the table again and scan the table 
>enable ‘studentrj’ 
>scan ‘studentrj’

5. Delete the column middle name for student with id 4 
>delete ‘studentrj’ , ‘04’ , ‘studname : mname’

6.Delete all the column families and columns for record 4 
>deleteall ‘studentrj’ , ‘04’

7.Set the maximum number of cell changes for student address column family to 5
>alter ‘studentrj’ , NAME => ‘studaddress’ , VERSIONS =>5

Practical no 4 Simulating Datawarehouse environment
Part 1 Create a datawarehouse database named college1. 
2. Check the created database
$hive
>create database college;
>use college;
>create table student(rollno int,name string,course string,marks float) row format delimited fields terminated by ’,’ stored as textfile;
>exit

>gedit studdata.txt
Inside enter:
101,Kartik,DS,35
102,Shruti,CS,45
103,Samruddhi,IT,35
Contorl+S and close it
$hive
>use college;
>describe formatted student;
>load data local inpath ‘/home/cloudera/studdata.txt’ into table student;
>select*from student;


Practical No 5 Working with different types of tables in hive
Internal Table : 
$hive 
> create database licdw; 
> show databases; 
> use licdw;

> create table customer ( id int, 
name string, dob string, 
email string, contactno string , 
address string, gender string) 
row format delimited fields terminated by ‘,’ ; 
>show tables ;
> exit
$gedit studdata.txt
$ hive
> describe formatted customer; 

>load data local inpath ‘/home/cloudera/customer.csv’ into table customer ; 
>select * form customer ;

External
>gedit policydetails.csv
>gedit policydetails.csv

>create external table policy_details (id int , 
name string, 
type string, 
age_criteria int, 
tenure int , 
maturity string) row format delimited fields terminated by ‘,’ 
stored as textfile location ‘/home/cloudera/policy_detail_data’ ; 
>load data local inpath ‘/home/cloudera/Desktop/policydetails.csv’ into table policy_details; 
> select * from policy_details ;

>hdfs dfs -ls hdfs://quickstart.cloudera:8020/home/cloudera/policy_detail_data 
> hdfs dfs -cat hdfs://quickstart.cloudera:8020/home/cloudera/policy_detail_data/policydetails.csv

Temporary Table
Temp Table :
>create temporary table policy_details_temp ( id int,
name string,
type string,
age_criteria int ,
tenure int,
maturity string)
> describe policy_details_temp ;
> insert into policy_details_temp values (32, ‘abc’ , ‘h’ , 32 , 3 , ‘40%’);
>select * from policy_details_temp;
> show tables ;
Close the existing hive session and open a new hive session
>use licdw;
>show tables 

Practical 5 : B. Demonstrating table partitioning , clustering (Bucketing in
hive)
1. Create and partition a table named emppartition on the mobile no column and load data into it
> create table emppartition( emp_id int ,
 name string ) partitioned by (m_no string)
 row format delimited fields terminated by ‘,’ ;
> set hive.exec.dynamic.partition.mode = nonstrict;
> insert into emppartition partition(m_no) values (1, ‘uday’ , ‘89272861’) ;
> insert into emppartition partition(m_no) values (2, ‘jay’ , ‘89272861’) ;
> insert into emppartition partition(m_no) values (3, ‘nikhil’ , ‘47252258’) ;
> select * from emppartition ;
2. Create a table emp_buck_no_partition with only bucketing on the m_no column and
load data into table.
> create table emp_buck_no_partition( emp_id int , name string , m_no string)
 clustered by(m_no) into 5 buckets ;
> insert into emp_buck_no_partition values(1 , ‘abc’ , ‘929272916’) ;
> select * from emp_buck_no_partition ;
3. Create a table named emp_buck_with_partition on m_no column and load data intoit.
> create table emp_buck_with_partition ( emp_id int , name string , m_no string)
 partitioned by (m_no2 string) clustered by(m_no) into 5 buckets ;
> set hive.exec.dynamic.partition.mode = nonstrict ;
> insert into emp_buck_with_partition partition(m_no2) values(1, ‘xyz’ , ‘8927927’ , ‘12345’ ) ;

8. Delete column family student address from student table
>alter ‘studentdata’ , ‘delete’ => ‘studaddress’ 
9. Check if the student table exists using exists and list keywords , now drop table student and again check if the student exists 
>exists ‘studentdata’ 
>list 
>disable ‘studentdata’ 
> drop ‘studentdata’ 
>list 
>exists ‘studentdata’


    
Practicsl 7 A simple pyspark driver program

''' # Import libraries
from pyspark.sql import SparkSession

 # Create Spark session
spark = SparkSession.builder.appName("SquareNumbers").getOrCreate()

 # Get Spark context
sc = spark.sparkContext

# Create RDD
numbers = sc.parallelize([1, 2, 3, 4, 5])

# Square each number
squared_numbers = numbers.map(lambda x: x * x)

# Show result
print(squared_numbers.collect())

# Stop Spark
spark.stop()'''

Practical 7 B. Working with different transformation and action 
on rdd by fetching data from external file.

'''from pyspark import SparkContext
sc = SparkContext("local","Iris")
iris = sc.textFile("iris.csv")
print(iris.collect())
# Partition info
for i,p in enumerate(iris.glom().collect()):
    print("Partition",i+1,":",len(p),"records")
# Repartition
iris = iris.repartition(8)
print("Total Partitions:",iris.getNumPartitions())
# Remove header
header = iris.first()
data = iris.filter(lambda x: x!=header)
# Map and flatMap
print(data.map(lambda x:x.split(",")).collect())
flat = iris.flatMap(lambda x:x.split(","))
print(flat.take(5))
# Filter Virginica
print(iris.filter(lambda x:"virginica" in x.lower()).map(lambda x:(x,1)).collect())
# Species count
species = ['setosa','versicolor','virginica']
count = iris.flatMap(lambda x:x.split(",")) \
            .filter(lambda x:x.lower() in species) \
            .map(lambda x:(x,1)) \
            .reduceByKey(lambda a,b:a+b)
print(count.collect())
sc.stop()'''

Practical 8 Data Exploration using Spark SQL. 

'''from pyspark.sql import SparkSession
# Create Spark Session
spark = SparkSession.builder.appName("IrisSQL").getOrCreate()
# Load CSV file
df = spark.read.csv("iris.csv", header=True, inferSchema=True)
# Show data
df.show(5)
print("Total rows:", df.count())
# Select columns
df.select("`Sepal.Length`","`Petal.Length`").show()
# Filter records
df.filter(df.Species == "setosa").show()
# GroupBy and count species
df.groupBy("Species").count().show()
# Create temporary SQL table
df.createOrReplaceTempView("iris")
# Run SQL query
spark.sql("SELECT Species, AVG(`Petal.Length`) FROM iris GROUP BY Species").show()
# Sort data
df.orderBy("`Sepal.Length`").show()
# Stop Spark
spark.stop()'''

Pracitcal 9 Create a PySpark MLlib.
'''
from pyspark.sql import SparkSession
from pyspark.ml.feature import StringIndexer, VectorAssembler
from pyspark.ml.classification import LogisticRegression
from pyspark.ml.evaluation import MulticlassClassificationEvaluator

# Create Spark Session
spark = SparkSession.builder.appName("IrisClassification").getOrCreate()

# Read CSV (no header in file)
df = spark.read.csv("flowers.csv", header=False, inferSchema=True)

# Assign column names
df = df.toDF("sepal_length",
             "sepal_width",
             "petal_length",
             "petal_width",
             "species")

# Show dataset
df.show(5)
print("Total Rows:", df.count())

# Convert species (string) to numeric label
indexer = StringIndexer(inputCol="species", outputCol="label")
df = indexer.fit(df).transform(df)

# Combine feature columns
assembler = VectorAssembler(
    inputCols=["sepal_length","sepal_width","petal_length","petal_width"],
    outputCol="features"
)

data = assembler.transform(df)

# Split dataset
train, test = data.randomSplit([0.8,0.2], seed=42)

# Logistic Regression Model
lr = LogisticRegression(featuresCol="features", labelCol="label")
model = lr.fit(train)

# Predictions
pred = model.transform(test)
pred.select("species","prediction").show()

# Model Evaluation
evaluator = MulticlassClassificationEvaluator(
    labelCol="label",
    predictionCol="prediction",
    metricName="accuracy"
)

accuracy = evaluator.evaluate(pred)
print("Accuracy:", accuracy)

# Stop Spark
spark.stop()'''
