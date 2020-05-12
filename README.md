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

## [CAP Theorem](https://en.wikipedia.org/wiki/CAP_theorem)
>In theoretical computer science, the CAP theorem, also named Brewer's theorem after computer scientist Eric Brewer, states that it is impossible for a distributed data store to simultaneously provide more than two out of the following three guarantees:
>* Consistency: Every read receives the most recent write or an error
>* Availability: Every request receives a (non-error) response, without the guarantee that it 
contains the most recent write
>* Partition tolerance: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes

[Wikipedia](https://en.wikipedia.org/wiki/CAP_theorem)

**Neo4j** is a bit tricky to place in this theorem, as there can be made an argument saying Neo4j isn't a distributed database, hence it does not fit in the theorem. Sharding is not available, however there are two clustering modes available in the enterprise edition. If Neo4j was to be placed in the theorem it would arguably be missing consistency, which often is the tradeoff with nosql databases.

**Redis** on the other hand, despite being a nosql database, is missing [availability](https://aphyr.com/posts/283-jepsen-redis), which makes perfectly good sense, seeing as the database per default only exsists in memory unless active action is taken to do otherwise.

## [ACID](https://en.wikipedia.org/wiki/ACID)
>In computer science, ACID (atomicity, consistency, isolation, durability) is a set of properties of database transactions intended to guarantee validity even in the event of errors, power failures, etc. In the context of databases, a sequence of database operations that satisfies the ACID properties (and these can be perceived as a single logical operation on the data) is called a transaction. For example, a transfer of funds from one bank account to another, even involving multiple changes such as debiting one account and crediting another, is a single transaction.

[Wikipedia](https://en.wikipedia.org/wiki/ACID)

**Neo4j** is fully ACID compliant, and full overview on Neo4j ACID overview can be found [here](https://www.graphgrid.com/neo4j-is-designed-to-be-your-source-of-truth-database/)

**Redis** unlike Neo4j is not [ACID compliant](https://stackoverflow.com/questions/14682470/redis-and-data-integrity), and is not intended to be either. Action can be taken trough lua scripting to achieve ACID like features, but they are not shipped out of the box.

## [Neo4j](https://neo4j.com)
Neo4j is a graph database which is truly relational, as they explain it themselves. The data is structured like a graph, which means nodes hold data about them selves, whilst the relationship between nodes describes their purpose and/or relations, as implied. This makes graph databases, Neo4j included, highly efficient at certain tasks. It's great for spotting tendencies in data, which were otherwise not obvious, and is also greatly suited for fraud detection, because of previous mentioned fact. Furthermore Neo4j is fast, because it doesn't have to itterate trough all the nodes to find a specific one, it's a direct lookup.

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
Redis is an open-source alternative, you can use as a NoSQL Database. Instead of saving the data locally on the hard drive, it's got a in-memory data structure store, has got the functionality to save it on the hard drive if needed though. Redis supports data structures such as strings, hashes, lists etc. 

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
