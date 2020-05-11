# Mini project 2
In this project, atleast two databases has to be compared on a set criteria. We've chosen to compare [redis](https://redis.io/) and [neo4j](https://neo4j.com/)

## Preparing the data
In order to make a fair comparison, a somewhat random dataset was generated using [mockaroo](https://mockaroo.com). Leading up to the exam project, an initial draft for the exam project was made, and the data generated here should emulate one of the classes used in the exam project. Hence the data is products, with no relations or references, only fields.

## Selecting the database operations

## Comparison criteria
 
## Neo4j

### Benchmarks

#### Import/Insert
308000 Lines - 144.15 seconds (2 Minutes and 24 seconds)
```cypher
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