# List of topics

**Philosophy & Features:** performance, JSON, BSON, fault tolerance, disaster recovery, horizontal scaling, and the Mongo shell

**CRUD:** Create, Read, Update, and Delete operations

**Indexing:** single key, compound, multi-key, mechanics, and performance

**Aggregation:** pipeline, operators, memory usage, sort, skip, and limit

**Replication:** configuration, oplog concepts, write concern, elections, failover, and deployment to multiple data centers

**Sharding:** components, when to shard, balancing, shard keys, and hashed shard keys

**Application Administration:** data files, journaling, authentication, and authorization

**Server Administration:** performance analysis, storage engines, diagnostics and debugging, maintenance, backup, and recovery


## Philosophy & Features:

Why we should use more than one collection ?
 1. When you are querying for posts but this one collection contains not only the same documents and it is slower and indexes are less efficiently.  
 2. If we create a query for getting list of documents that contain one of the types "water", "earth", "air" in one collection, however it is slower than creating three separately collections based on the above each type.   
 
### Databases

- Each database is stored in separate files on disk.
- Separate databases are useful when storing data for several application or users on the same MongoDB server
  
#### Reserved DB names
  - admin - this is root db
  - local - to store any collections that should be local to a single server
  - config - to storing information about the shards
  
*Namespace* - db name + collection name + subcollection name (cms.blog.posts)

### Getting and Starting

/data/db/ - default data directory

#### Running the Shell

**mongo** is a CLI for MongoDB.
  
 ```bash
 $ mongo
 ```
 
The **mongo** attemps to connect to the **mongod** before starting.

The shell connects to db on a MongoDB server and assigns this db to the global variable db.
 
#### Basic operations with the Shall
 
CRUD - create, read, update, delete

Create - insert document to the collection
 ```
  > db.blog.insert({ ... })
 ```
Read - query a collection.
 ```
  > db.blog.find({ ... })
 ```
Update - modify document(s)
 ```
  > db.blog.update({ ... }, { ... })
 ```
Delete - remove permanently document(s) from collection
 ```
  > db.blog.remove({ ... })
 ```
 
#### Data types
 
 - null - null value or nonexistent field
 - boolean - true/false
 - number - 64-bit floating point number (for 4-byte or 8-byte signed integers use NumberInt() and NumberLong() respectively)
 - string - string in UTF-8 characters
 - date - new Date() in milliseconds since the epoch
 - regular expression - javascript regular expression
 - array - list of values
 - embedded document - objectsvtgjdc.le
 - object id - 12-byte ID for documents (ObjectId())
 - binary data - for saving non-UTF-8 strings
 - code - contain js arbitrary js code

"why objectIds?"

It is difficult and time-consuming to synchronize autoincrementing primary keys across multiple servers. Because MongoDB was designed to be a distributed database, it was important to be able to generate unique identifiers in a sharded environment.

12-byte 

#### Connecting

```
 > mongo http://localhost:30000/mydb
```

Starting without attempt to connect to db

```
 > mongo --nodb
```

after that you can connecting to specific host and db

```
 > conn = new Mongo("my-host:303030");
 > db = conn.getDB("my-db")
```

#### Running Scripts

```
 > mongo script.js app.js
```

--quite - create pipe of scripts

```
 > mongo --quiet server:303030/my-db script.js app.js
```

 - **use my-db** - db.getSisterDB("foo")
 - **show dbs** - db.getMongo().getDBs()
 - **show collections** - db.getCollectionNames()
 
run() - use to run command-line programs from the shell 

For running some script each time when you run mongo use .mongorc.js. This file useful for preventing some special functions to execute for example dropDatabase and deleteIndexes

--norc - disable loading .mongorc.js


### CRUD

#### Insert
 
 ```
  > db.blog.insert({ ... })
 ```
 
 "_id" will be added automatically.
 
 
 ```
  > db.blog.batchInsert([{ ... },{ ... }])
 ```
 
#### Delete

 This will remove all documents of the blog collection

 ```
  > db.blog.remove()
  > db.blog.remove({ ... })
 ```
 
 However, for removing all collection faster use 
 
 ```
  > db.blog.drop()
 ```
 
#### Updating

```
 > db.blog.update({ "topic": "The funny cat" }, { "topic": "The funny dog"})
```

If you need to update topic but you dont know how many topics in the collection. If you attempt to change this document you will recieve the error notification. The following step should be to pass the _id field in the first parameter of updat emethod, because _id is unique.

 
