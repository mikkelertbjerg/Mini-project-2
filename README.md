# Mini project 2
*The objective of this assignment was to provide us with experience in analysis and design of database applications that could benefit from NoSQL databases. Our task was to select two or more databases of different NoSQL types, and compare their features and performance in storing, scaling, providing and processing big data.*

## Preparing the data
In order to make a fair comparison, a somewhat random dataset was generated using [Mockaroo](https://mockaroo.com). Leading up to the exam project, an initial draft for the exam project was made, and the data generated here should emulate one of the classes used in the exam project. Hence the data is products, with no relations or references, only fields.

## Selecting the database operations

## Comparison criteria
 To compare the 2 NoSQL databases we've chosen, we decided to run different benchmarks on them, primarily comparing time spent to complete a given task.  
 
 We'll be looking into the following:
 * Inserting data. (In our case, 300.000+ items at one time.)
 * Retrieving data.
## Neo4j
Write short about Neo4j
### Benchmarks

#### Import/Insert

// NOTE: Skriv noget om MERGE i stedet for CREATE.

308000 Lines - 924000 Properties - 2073ms
```graph
LOAD CSV FROM 'file:///dataset.csv' AS row
WITH toInteger(row[0]) AS productId, row[1] AS productName, toFloat(row[2]) AS unitCost, row[3] AS ProductCatagory
CREATE (p:Product)
  SET p.productName = productName, p.unitCost = unitCost, p.productCatagory = ProductCatagory
RETURN count(p)
```

#### Retrieve/Get



## Redis
Write short about redis
### Benchmarks

#### Import/Insert
308000 Lines - 3.65 seconds
```python
import redis
import pandas as pd
import time

r = redis.Redis()

df = pd.read_csv("dataset.csv")

products = df.values.tolist()
with r.pipeline() as pipe:
    i = 2
    while i < len(products):
        product = products[i]
        pipe.set(i,f"{product[1]},{product[2]},{product[3]}")
        i += 1
    now = time.time()
    print(now)
    pipe.execute()
    print(time.time() - now)
```

#### Retrieve/Get
Sending get request via python script and retrieving the answer from redis server:
308000 keys and a corresponding 924000 values - 4 seconds
Redis server processing time: 2.86 seconds
```python
now = time.time()
retrievedKeys = r.keys()
with r.pipeline() as pipe:
    
    for x in retrievedKeys:
        pipe.get(x)
    
    now2 = time.time()    
    retrievedValued = pipe.execute()
print(f"total time: {time.time() - now} Time Only on retrieval of values: {time.time() - now2}")
```

# Misc
Technical specifications of the machine tested on:
* CPU: I7-6700K @ 
* GPU: NVIDIA 1070 GTX
* Memory: 32GB 2133mHz
* OS: Windows 10 (64-bit)
* Hard Drive: M.2 SSD

[Dataset](misc/dataset.csv)
