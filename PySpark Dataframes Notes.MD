# PySpark Data Frames Basic

We need to import SparkSession then create one to execute spark dataframe commands

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("Any Name").getOrCreate()
```
Spark dataframes can read many type of files like csv,text,json etc.Its easier to read in csv
as it has options like inferSchema-spark will automcatically infer the schema based on the data.header - will take the first row as columns.

These options are not there with other formats?

```python
df = spark.read.csv('test.csv',inferSchema=True,header=True)
```

Other commands
```python
df.show()   # shows the table 20 records
df.printSchema()   # shows the schema of the data frame
df.columns   #shows the list of columns
df.describe.show()  #shows statistical summary of numerical columns

```

To define our own schema

```python
from pyspark.sql.types import (StructField,StructField,
                             IntegerType,StringType)
#giving import in () for multi line

data_schema = [StructField('Age',IntegerType(),True),StructField('Name',StringType(),True)]

#True means it can have null values

final_struc = StructType(fields=data_schema)
df = spark.read.csv('test',schema=final_struc)

```

Selecting columns using select returns dataframe objects.Data frame object has more options
so its better to use

``` python
type(df.select('Channel'))  #this returns a dataframe object
type(df['Channel'])  #this returns a column object
df.select(['Channel','Region','Milk']).show()  #select multiple columns
type(df.head()) - # returns row object
df.withColumn('newChannel',df['Channel']*2).show() #new column with some operation on existing column
df.withColumn('Channel',df['Channel']*2).show() #some operation on existing column
df.withColumnRenamed('newChannel','Channel').show()  #Renaming a existing column

```

withColumn and withColumnRenamed so make any permanent change.Its just a view. To make permanet change move it to variable.

### Sql Queries
We can use Sql queries to perform all the operations.This is an option but dataframe options can handle similar operations.We will create a temp table View .Then perform Sql operations on them

``` python
df.createOrReplaceTempView("TempTableName")
results = spark.sql("select * from TempTableName")
results.show()

```

### Basic Sql like operations with dataframe
We can do this using the filter.We use select along with filter to get specific columns.We can give multiple conditions in filter in () and logical operators ex () & ()
& - and
| - or
~ - not operator
``` python
df.filter(df['Channel'] == 1).show()  #this will fetch all the columns and rows satisfying the condition
df.filter(df['Channel'] == 1).select('Channel').show() #this will fetch only column Channel satisfying the conditon
df.filter((df['Channel'] == 1) &( df['Region'] == 3)).select(['Channel','Region']).show() # for multiple condition fetch
```

### .show() and .collect()
.show() - It shows only content.
.collect() - It shows metadata. We can perform other operations on them.


### groupBy and Aggregate functions
We can use groupby to perform aggregate functions.

```python
df.groupBy('Region').avg().show()   
df.agg({'Channel':'sum'}).show()     # direct aggregate function of sum on Channel column 
df.groupBy('Channel').agg({'Region':'sum'}).show() #combining groupBy and agg
```
### Functions
There a lot of functions available in dataframes
```python
from pyspark.sql.functions import countDistinct,avg,stddev,format_number
df.select(format_number('Channel',2)).show() #to format the number to show upto 2 decimal places 
df.select(avg('Channel')).show()
df.select(avg('Channel').alias('Avg Channel')).show()   #we can use alias to give a proper column name

```
### Sort using orderby
```python

df.orderBy("Channel").show()  # default ascending order
df.orderBy("Channel","Region").show() # multiple sort fields
df.orderBy(df["Channel"].desc()).show()  # descending order
```

### Missing Data
Spark will populate null values for missing data.We might need to modify or remove them
```python
df.na.drop().show()
df.na.drop(thresh=2).show()   # drop only if two null values in a row
df.na.drop(how='any').show()   # drop only if  any one value in a row is null
df.na.drop(how='all').show()    # drop only if all values in a row is null
df.na.drop(subset=['Channel']).show()  #drop only if Channel column has null
df.na.fill('xyx').show()  # only string column null values will be updated with xyz
df.na.fill(1).show()  # only numeric column null values will be update with xyz
df.na.fill(9,subset=['Channel']).show()  #fill only column Channel null values with 9

```
### Populating Mean in null values
We need to use the mean function for this
``` python
mean_value = df.select(mean(df['Channel'])).collect()
mean_Channel= mean_value[0][0]
df.na.fill(mean_Channel,subset=['Channel']).show()
```
