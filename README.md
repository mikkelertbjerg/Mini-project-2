# Mini project 2
*The objective of this assignment was to provide us with experience in analysis and design of database applications that could benefit from NoSQL databases. Our task was to select two or more databases of different NoSQL types, and compare their features and performance in storing, scaling, providing and processing big data.*

## Preparing the data
In order to make a fair comparison, a somewhat random dataset was generated using [mockaroo](https://mockaroo.com). Leading up to the exam project, an initial draft for the exam project was made, and the data generated here should emulate one of the classes used in the exam project. Hence the data is products, with no relations or references, only fields.

## Selecting the database operations

# Comparison criteria
 To compare the 2 NoSQL databases we've chosen, we decided to run different benchmarks on them, primarily comparing time spent to complete a given task.  
 
 We'll be looking into the following:
 * Inserting data. (In our case, 300.000+ items at one time.)
 * Retrieving data.
## Neo4j

### Benchmarks

#### Import/Insert
308000 Lines - 144.15 seconds (2 Minutes and 24 seconds)
```graph
LOAD CSV FROM 'file:///products4.csv' AS row
WITH toInteger(row[0]) AS productId, row[1] AS productName, toFloat(row[2]) AS unitCost, row[3] AS ProductCatagory
MERGE (p:Product {productId: productId})
  SET p.productName = productName, p.unitCost = unitCost, p.productCatagory = ProductCatagory
RETURN count(p)
```

## Redis
### Benchmarks

#### Import/Insert
308000 Lines - 3.65 seconds

#### Retrieve/Get
Sending get request via python script and retriving the answer from redis server:
308000 keys and a corresponding 924000 values - 4 seconds
Redis server processing time: 2.86 seconds