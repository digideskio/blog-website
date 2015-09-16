
# Run MongoDB

## Run MongoDB On Windows

```
	�����û�н���auth��������Secure Mode����, ��ô�Ͳ�Ҫʹ mongod.exe�ڹ��������Ͽɼ�.
```

### ����MOngoDB����

###### ���û�������
```
	�ڻ�����������ӻ������� D:\Program Files\MongoDB\Server\3.0\
	Ȼ����Path����ӣ� %MONGODB_HOME%\bin
```

###### data directory
```
	MongoDB ��Ҫһ��data directory���洢ȫ��������. MongoDBĬ�ϵ�data directory·����\data\db, 
	����������Ҫ����һ��data directory. ����������D�̴�����һ��������Ŀ¼: D:\mongodb\data\db.

	�����ͨ��--dbpathѡ���mongod.exe������һ��data directory.
	mongod.exe --dbpath D:\mongodb\data\db

	������data directory�����ո�Ļ�,��ô����Ҫʹ��""�����ǰ���������
	mongod.exe --dbpath "d:\test\mongo db data"
```

## ����MongoDB

###### ʹ��mongod.exe��������mongoDB
```
	mongod.exe
```

###### ������־
```
	���������������־�￴��
	waiting for connections on port 27017
```

#### �����з�ʽ����

MongoDB Ĭ�ϴ洢����Ŀ¼Ϊ/data/db/ (���� c:/data/db), Ĭ�϶˿� 27017,Ĭ�� HTTP �˿� 28017.
```
	mongod --dbpath=/data/db
```

#### �����ļ���ʽ����
MongoDB Ҳ֧��ͬ mysql һ���Ķ�ȡ���������ļ��ķ�ʽ���������ݿ�,�����ļ�����������:
```
	cat /etc/mongodb.cnf
```
����ʱ���ϡ�-f������,��ָ�������ļ�����:
```
	mongod -f /etc/mongodb.cnf
```

#### Daemon ��ʽ����
MongoDB �ṩ��һ�ֺ�̨ Daemon ��ʽ������ѡ��,ֻ�����һ���� --fork����������,,������õ��� �� --fork�������ͱ���Ҳ���� ��--logpath������,����ǿ�Ƶ�

```
	mongod --dbpath=/data/db --logpath=/data/log/r3.log --fork
```

#### mongod ����˵��
mongod �Ĳ�����Ϊһ�����, windows ����, replication ����, replica set ����,�Լ���������.�����оٵĶ���һ�����

mongod �Ĳ�����,û�������ڴ��С��صĲ���,�ǵ�, MongoDB ʹ�� os mmap ���������������ļ�����,����Ŀǰ���ṩ�������.�����ô��Ǵ����,
mmap ���������������ڴ�ʱЧ�ʺܸ�.��������������ϵͳ�����ڴ��,��д������ܿ��ܲ�̫�ȶ�,���׳��ִ������,���������µ� 1.8 �汾��,�����������ǰ�İ汾�Ѿ�
����һ���̶ȵĸ���.

###### mongod ����Ҫ�����У�
* dbpath ���� �����ļ����·��,ÿ�����ݿ�������д���һ����Ŀ¼,���ڷ�ֹͬһ��ʵ�������
�е� mongod.lock Ҳ�����ڴ�Ŀ¼��.
* logpath ���� ������־�ļ�
* logappend ���� ������־����׷��ģʽ��Ĭ���Ǹ�дģʽ��
* bind_ip ���� �������İ� ip,һ������Ϊ��,�����ڱ������п��� ip ��,������Ҫ���Ե���ָ��
* port ���� �������˿� . Web ����˿������ port �Ļ�����+1000
* fork ���� �Ժ�̨ Daemon ��ʽ���з���
* journal ���� ������־����,ͨ�����������־�����͵������ϵĻָ�ʱ��,�� 1.8 �汾����ʽ����,ȡ���� 1.7.5 �汾�е� dur ����.
* syncdelay ���� ϵͳͬ��ˢ�´��̵�ʱ��,��λΪ��,Ĭ���� 60 ��.
* directoryperdb ���� ÿ�� db ����ڵ�����Ŀ¼��,�������øò���.�� MySQL �Ķ�����ռ�����
* maxConns ���� ���������
* repairpath ���� ִ�� repair ʱ����ʱĿ¼.�����û�п��� journal,�쳣 down �������� ,����ִ�� repair����.

## ֹͣ���ݿ�

#### Control-C
#### shutdownServer()ָ��
```
	mongo --port 28013
	use admin
	db.shutdownServer()
```

## ���ù��߼�
MongoDB �� bin Ŀ¼���ṩ��һϵ�����õĹ���,��Щ�����ṩ�� MongoDB ����ά������
�ķ��㡣
* bsondump: �� bson ��ʽ���ļ�ת��Ϊ json ��ʽ������
* mongo: �ͻ��������й���,��ʵҲ��һ�� js ������,֧�� js �﷨
* mongod: ���ݿ�����,ÿ��ʵ������һ������,���� fork Ϊ��̨����
* mongodump/ mongorestore: ���ݿⱸ�ݺͻָ�����
* mongoexport/ mongoimport: ���ݵ����͵��빤��
* mongofiles: GridFS ������,��ʵ�ֶ����ļ��Ĵ�ȡ
* mongos: ��Ƭ·��,���ʹ���� sharding ����,��Ӧ�ó������ӵ��� mongos ������mongod
* mongosniff: ��һ���ߵ����������� tcpdump,��ͬ������ֻ��� MongoDB ��صİ�����,��������ָ���Ŀɶ��Ե���ʽ���
* mongostat: ʵʱ���ܼ�ع���

## ���� Replica Sets
* ���������ļ��洢·��
```
	mkdir E:/mongoData/data/r0
	mkdir E:/mongoData/data/r1
	mkdir E:/mongoData/data/r2
```
* ������־�ļ�·��
```
	mkdir E:/mongoData/log
```
* �������� key �ļ������ڱ�ʶ��Ⱥ��˽Կ������·�����������ʵ���� key file ���ݲ�һ�£����򽫲��������á�
```
	mkdir E:/mongoData/key
	echo "this is rs1 super secret key" > E:/mongoData/key/r0
	echo "this is rs1 super secret key" > E:/mongoData/key/r1
	echo "this is rs1 super secret key" > E:/mongoData/key/r2
```
* ���� 3 ��ʵ��
```
	mongod --replSet rs1 --keyFile E:/mongoData/key/r0 -fork --port 28010 --dbpath E:/mongoData/data/r0 --logpath=E:/mongoData/log/r0.log --logappend
	mongod --replSet rs1 --keyFile E:/mongoData/key/r1 -fork --port 28011 --dbpath E:/mongoData/data/r1 --logpath=E:/mongoData/log/r1.log --logappend
	mongod --replSet rs1 --keyFile E:/mongoData/key/r2 -fork --port 28012 --dbpath E:/mongoData/data/r2 --logpath=E:/mongoData/log/r2.log --logappend
```
* ���ü���ʼ�� Replica Sets
```
	mongo -port 28010
	
```


# Introduction

## A Quick Tour

ʹ��java ���������Ƿǳ��򵥵�,������Ҫȷ�����`classpath`�а���`mongo.jar`

### Making a Connection

Ϊ���ܹ�������MongoDB,��͵�Ҫ��Ҳ����Ҫ֪�����ӵ�database������. ������ݿ���Բ�����,��������ڵĻ�,MongoDB���Զ�����������ݿ�

����,�����ָ�����ӵķ������ĵ�ַ�Ͷ˿�,���������չʾ���������ӱ���`mydb`���ݿ�ķ�ʽ
```java
import com.mongodb.BasicDBObject;
import com.mongodb.BulkWriteOperation;
import com.mongodb.BulkWriteResult;
import com.mongodb.Cursor;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.DBCursor;
import com.mongodb.DBObject;
import com.mongodb.MongoClient;
import com.mongodb.ParallelScanOptions;
import com.mongodb.ServerAddress;

import java.util.List;
import java.util.Set;

import static java.util.concurrent.TimeUnit.SECONDS;

// To directly connect to a single MongoDB server (note that this will not auto-discover the primary even
// if it's a member of a replica set:
MongoClient mongoClient = new MongoClient();
// or
MongoClient mongoClient = new MongoClient( "localhost" );
// or
MongoClient mongoClient = new MongoClient( "localhost" , 27017 );
// or, to connect to a replica set, with auto-discovery of the primary, supply a seed list of members
MongoClient mongoClient = new MongoClient(Arrays.asList(new ServerAddress("localhost", 27017),
                                      new ServerAddress("localhost", 27018),
                                      new ServerAddress("localhost", 27019)));

DB db = mongoClient.getDB( "mydb" );
```

�����������`db`���󱣳���һ����MongoDB������ָ�����ݿ��һ������. ͨ�����������������ܶ���������

> Note:
>
> `MongoClient`ʵ��ʵ����ά���Ŷ�������ݿ��һ�����ӳ�. ��ʹ�ڶ��̵߳������,��Ҳֻ��Ҫһ��`MongoClient`ʵ��, �ο�[concurrency doc page]()


`MongoClient`����Ƴ�һ���̰߳�ȫ���̹߳������. һ������������,���һ�����ݿ⼯Ⱥ����������һ��`MongoClient`ʵ��,Ȼ�����������Ӧ�ó����ж�ʹ����һ��ʵ��. �������һЩ����ԭ���㲻�ò��������`MongoClient`ʵ��,��ô����Ҫע���������㣺

* all resource usage limits (max connections, etc) apply per MongoClient instance
* ���ر�һ��ʵ��ʱ,�����ȷ���������`MongoClient.close()`�������ȫ������Դ

New in version 2.10.0: The MongoClient class is new in version 2.10.0. For releases prior to that, please use the Mongo class instead.

### Authentication (Optional)

MongoDB�����ڰ�ȫģʽ������, ����ģʽ��,��Ҫͨ����֤���ܷ������ݿ�. ��������ģʽ�����е�ʱ��, �κοͻ��˶������ṩһ��֤��.��java Driver��,��ֻ��Ҫ�ڴ���`MongoClient`ʵ��ʱ�ṩһ��֤��.
```java
MongoCredential credential = MongoCredential.createMongoCRCredential(userName, database, password);
MongoClient mongoClient = new MongoClient(new ServerAddress(), Arrays.asList(credential));
```

MongoDB֧�ֲ�ͬ����֤����,����ο�[the access control tutorials]()

### Getting a Collection

�����Ҫʹ��һ��collection,��ô�������Ҫ����`getCollection(String collectionName)`����,Ȼ��ָ����collection���ƾͺ�

```java
DBCollection coll = db.getCollection("testCollection");
```

һ��������collection����,����Ϳ���ִ�������������,��ѯ���ݵȵȵĲ�����

### Setting Write Concern

��2.10.0����汾��,Ĭ�ϵ�write concern��`WriteConcern.ACKNOWLEDGED`���������ͨ������ķ������ɸı���
```java
mongoClient.setWriteConcern(WriteConcern.JOURNALED);
```

��Ӧwrite concern�ṩ�˺ܶ���ѡ��. ����,���Ĭ�ϵ�write concern�ֱ���������ݿ�,collection,�Լ������ĸ��²���������.

Changed in version 2.10.0: Prior to version 2.10.0, the default write concern is WriteConcern.NORMAL. Under normal circumstances, clients will typically change this to ensure they are notified of problems writing to the database.


### Inserting a Document

һ����ӵ����collection����,��Ϳ������collection�в���document. ����,���ǿ��Բ���һ��������������һ��json�ĵ�
```json
{
   "name" : "MongoDB",
   "type" : "database",
   "count" : 1,
   "info" : {
               x : 203,
               y : 102
             }
}
```

ע��,�����������������һ����Ƕ���ĵ�.��Ҫ��������һ���ĵ�,���ǿ���ʹ��`BasicDBObject`����ʵ�֣�
```java
BasicDBObject doc = new BasicDBObject("name", "MongoDB")
        .append("type", "database")
        .append("count", 1)
        .append("info", new BasicDBObject("x", 203).append("y", 102));
coll.insert(doc);
```


### Finding the First Document in a Collection Using findOne()

�����Ҫ�鿴�ղŲ�����ĵ�,���ǿ��Լ򵥵ص���`findOne()`,����������ø�collection�еĵ�һ���ĵ�.�������ֻ�Ƿ���һ���ĵ�����(��`find()`�᷵��һ��`DBCursor`����),��collection��ֻ��һ���ĵ���ʱ��,���Ƿǳ����õ�.
```java
DBObject myDoc = coll.findOne();
System.out.println(myDoc);
```
������£�
```json
{ "_id" : "49902cde5162504500b45c2c" ,
  "name" : "MongoDB" ,
  "type" : "database" ,
  "count" : 1 ,
  "info" : { "x" : 203 , "y" : 102}}

```

>Note:
>
> `_id`Ԫ����MongoDB�Զ���ӵ�����ĵ��е�. ��ס,MongoDB�ڲ��ԡ�_��/��$����ͷ����Ԫ������

### Adding Multiple Documents

������һЩ������ѯ��ʱ��,������Ҫ����������,���������һЩ�򵥵��ĵ���collection��.
```json
{
   "i" : value
}
```

���ǿ�����һ��ѭ���в��ϵز�������
```java
for (int i=0; i < 100; i++) {
    coll.insert(new BasicDBObject("i", i));
}
```

ע�⣺���ǿ�����ͬһ��collection�в��������ͬԪ�ص��ĵ�.����MongoDBҲ����Ϊ`schema-free`

### Counting Documents in A Collection

ͨ�����ϵĲ��������Ѿ�������101���ĵ�,����ͨ��`getCount()`���������һ��.
```java
System.out.println(coll.getCount());
```

### Using a Cursor to Get All the Documents

�����Ҫ���collection�е�ȫ���ĵ�,���ǿ���ʹ��`find()`����. `find()`����һ��`DBCursor`����,���ǿ���ͨ�������ö����ȡ����ƥ������������ĵ�.
```java
DBCursor cursor = coll.find();
try {
   while(cursor.hasNext()) {
       System.out.println(cursor.next());
   }
} finally {
   cursor.close();
}
```

### Getting A Single Document with A Query

���ǿ�����`find()`��������һ����ѯ����, ͨ���ò����ҵ������з���������ĵ��Ӽ�. ������չʾ��������Ҫ�ҵ�i��7�������ĵ�.
```java
BasicDBObject query = new BasicDBObject("i", 71);

cursor = coll.find(query);

try {
   while(cursor.hasNext()) {
       System.out.println(cursor.next());
   }
} finally {
   cursor.close();
}
```

�ô���ֻ�����һ���ĵ�
```json
{ "_id" : "49903677516250c1008d624e" , "i" : 71 }
```

��Ҳ���Դ�������ʵ�����ĵ��в鿴`$`���������÷���
```java
db.things.find({j: {$ne: 3}, k: {$gt: 10} });
```

ʹ����Ƕ��`DBObject`,`$`���Կ�����������ʽ�ִ�
``` java
query = new BasicDBObject("j", new BasicDBObject("$ne", 3))
        .append("k", new BasicDBObject("$gt", 10));

cursor = coll.find(query);

try {
    while(cursor.hasNext()) {
        System.out.println(cursor.next());
    }
} finally {
    cursor.close();
}
```


### Getting A Set of Documents With a Query

���ǿ���ʹ�ò�ѯ�����collection�е�һ���ĵ�����.����,����ʹ��������﷨����ȡ����i > 50���ĵ�
```java
// find all where i > 50
query = new BasicDBObject("i", new BasicDBObject("$gt", 50));

cursor = coll.find(query);
try {
    while (cursor.hasNext()) {
        System.out.println(cursor.next());
    }
} finally {
    cursor.close();
}
```

���ǻ����Ի��һ������(20 < i <= 30)�ĵ�����
```java
query = new BasicDBObject("i", new BasicDBObject("$gt", 20).append("$lte", 30));
cursor = coll.find(query);

try {
    while (cursor.hasNext()) {
        System.out.println(cursor.next());
    }
} finally {
    cursor.close();
}
```


### MaxTime

MongoDB2.6 ��Ӳ�ѯ��ʱ������

```java
coll.find().maxTime(1, SECONDS).count();
```

������������н�`maxTime`����Ϊ1s,��ʱ�䵽���ѯ�������

### Bulk operations

Under the covers MongoDB is moving away from the combination of a write operation followed by get last error (GLE) and towards a write commands API. These new commands allow for the execution of bulk insert/update/remove operations. There are two types of bulk operations:

1. Ordered bulk operations. ��˳��ִ��ȫ���Ĳ���,��������һ��дʧ�ܵ�ʱ��,�˳�
2. Unordered bulk operations. ����ִ��ȫ������, ͬʱ�ռ�ȫ������.�ò�������֤����˳��ִ��

����չʾ��������˵������ʾ��
```java
// 1. Ordered bulk operation
BulkWriteOperation builder = coll.initializeOrderedBulkOperation();
builder.insert(new BasicDBObject("_id", 1));
builder.insert(new BasicDBObject("_id", 2));
builder.insert(new BasicDBObject("_id", 3));

builder.find(new BasicDBObject("_id", 1)).updateOne(new BasicDBObject("$set", new BasicDBObject("x", 2)));
builder.find(new BasicDBObject("_id", 2)).removeOne();
builder.find(new BasicDBObject("_id", 3)).replaceOne(new BasicDBObject("_id", 3).append("x", 4));

BulkWriteResult result = builder.execute();

// 2. Unordered bulk operation - no guarantee of order of operation
builder = coll.initializeUnorderedBulkOperation();
builder.find(new BasicDBObject("_id", 1)).removeOne();
builder.find(new BasicDBObject("_id", 2)).removeOne();

result = builder.execute();
```


> Note:
> 
For servers older than 2.6 the API will down convert the operations. To support the correct semantics for BulkWriteResult and BulkWriteException, the operations have to be done one at a time. It��s not possible to down convert 100% so there might be slight edge cases where it cannot correctly report the right numbers.


### parallelScan

MongoDB 2.6 ������`parallelCollectionScan`����, ������ͨ��ʹ�ö���α��ȡ����collection.
```java
ParallelScanOptions parallelScanOptions = ParallelScanOptions
        .builder()
        .numCursors(3)
        .batchSize(300)
        .build();

List<Cursor> cursors = coll.parallelScan(parallelScanOptions);
for (Cursor pCursor: cursors) {
    while (pCursor.hasNext()) {
        System.out.println((pCursor.next()));
    }
}
```

���collection����IO���������Ż�.

> Note:
>
> `ParallelScan`����ͨ��`mongos`����

## Quick Tour of the Administrative Functions

### Getting A List of Databases

ͨ������Ĵ�������Ի�ȡһ���������ݿ��б�
```java
MongoClient mongoClient = new MongoClient();

for (String s : mongoClient.getDatabaseNames()) {
   System.out.println(s);
}
```

����`mongoClient.getDB()`�����ᴴ��һ�����ݿ�. ���������������ݿ�д������ʱ,�����ݿ�Żᱻ����. ���糢�Դ���һ�����Ի���һ��collection���߲���һ���ĵ�.

### Dropping A Database

ͨ��`MongoClient`ʵ����Ҳ����`drop`��һ�����ݿ�
```java
MongoClient mongoClient = new MongoClient();
mongoClient.dropDatabase("databaseToBeDropped");
```

### Creating A Collection

�����ַ�ʽ����collection��
1. �����һ�������ڵ�collection�г��Բ���һ���ĵ�,��ô��collection�ᱻ��������
2. ����ֱ�ӵ���`createCollection`����

���������չʾ�˴���1M��С��collection
```java
db = mongoClient.getDB("mydb");
db.createCollection("testCollection", new BasicDBObject("capped", true)
        .append("size", 1048576));
```

### Getting A List of Collections

�����ͨ������ķ�ʽ���һ�����ݿ⵱�п���collection�б�
```java
for (String s : db.getCollectionNames()) {
   System.out.println(s);
}
```

��������ӻ������
```
system.indexes
testCollection
```

>Note:
>
> `system.indexes` collection���Զ�������, �����������ݿ������е�����, ���Բ�Ӧ��ֱ�ӷ�����

### Dropping A Collection

�����ͨ��`drop()`����ֱ��drop��һ��collection
```java
DBCollection coll = db.getCollection("testCollection");
coll.drop();
System.out.println(db.getCollectionNames());
```

### Getting a List of Indexes on a Collection

����չʾ����λ��һ��collection���������б�
```java
List<DBObject> list = coll.getIndexInfo();

for (DBObject o : list) {
   System.out.println(o.get("key"));
}
```

�����ʵ�����������������
```json
{ "v" : 1 , "key" : { "_id" : 1} , "name" : "_id_" , "ns" : "mydb.testCollection"}
{ "v" : 1 , "key" : { "i" : 1} , "name" : "i_1" , "ns" : "mydb.testCollection"}
{ "v" : 1 , "key" : { "loc" : "2dsphere"} , "name" : "loc_2dsphere" , ... }
{ "v" : 1 , "key" : { "_fts" : "text" , "_ftsx" : 1} , "name" : "content_text" , ... }
```


### Creating An Index

MongoDB֧������,�������ǿ������ɵز��뵽һ��������.���������Ĺ��̷ǳ���,��ֻ��Ҫָ�����������ֶ�,�㻹����ָ����������������(1)�����½���(-1).
```java
coll.createIndex(new BasicDBObject("i", 1));  // create index on "i", ascending
```


### Geo indexes

MongoDB֧�ֲ�ͬ�ĵ���ռ�����,�������������,���ǽ�����һ��`2dsphere`����, ���ǿ���ͨ����׼`GeoJson`��ǽ��в�ѯ. ��Ҫ����һ��`2dsphere`����,������Ҫ�������ĵ���ָ��`2dsphere`���������.
```java
coll.createIndex(new BasicDBObject("loc", "2dsphere"));
```

�в�ͬ�ķ�ʽȥ��ѯ`2dsphere`����,������������ҵ���500m���ڵ�λ��.
```java
BasicDBList coordinates = new BasicDBList();
coordinates.put(0, -73.97);
coordinates.put(1, 40.77);
coll.insert(new BasicDBObject("name", "Central Park")
                .append("loc", new BasicDBObject("type", "Point").append("coordinates", coordinates))
                .append("category", "Parks"));

coordinates.put(0, -73.88);
coordinates.put(1, 40.78);
coll.insert(new BasicDBObject("name", "La Guardia Airport")
        .append("loc", new BasicDBObject("type", "Point").append("coordinates", coordinates))
        .append("category", "Airport"));


// Find whats within 500m of my location
BasicDBList myLocation = new BasicDBList();
myLocation.put(0, -73.965);
myLocation.put(1, 40.769);
myDoc = coll.findOne(
            new BasicDBObject("loc",
                new BasicDBObject("$near",
                        new BasicDBObject("$geometry",
                                new BasicDBObject("type", "Point")
                                    .append("coordinates", myLocation))
                             .append("$maxDistance",  500)
                        )
                )
            );
System.out.println(myDoc.get("name"));
```

����ο�[geospatial]()�ĵ�

### Text indexes

MongoDB��֧��`text`����,����������֧�ִ�String�������ı�. `text`�������԰����κ��ֶ�,���Ǹ��ֶε�ֵ������String����String����.��Ҫ����һ��`text`����,ֻ��Ҫ�������ĵ���ָ��`text`������.
```java
// create a text index on the "content" field
coll.createIndex(new BasicDBObject("content", "text"));
```

MongoDB2.6 �Ժ�`text`�����ڽ�����Ҫ�Ĳ�ѯ������,���ҳ�Ϊ��һ��Ĭ�ϵķ�ʽ.
```java
// Insert some documents
coll.insert(new BasicDBObject("_id", 0).append("content", "textual content"));
coll.insert(new BasicDBObject("_id", 1).append("content", "additional content"));
coll.insert(new BasicDBObject("_id", 2).append("content", "irrelevant content"));

// Find using the text index
BasicDBObject search = new BasicDBObject("$search", "textual content -irrelevant");
BasicDBObject textSearch = new BasicDBObject("$text", search);
int matchCount = coll.find(textSearch).count();
System.out.println("Text search matches: "+ matchCount);

// Find using the $language operator
textSearch = new BasicDBObject("$text", search.append("$language", "english"));
matchCount = coll.find(textSearch).count();
System.out.println("Text search matches (english): "+ matchCount);

// Find the highest scoring match
BasicDBObject projection = new BasicDBObject("score", new BasicDBObject("$meta", "textScore"));
myDoc = coll.findOne(textSearch, projection);
System.out.println("Highest scoring document: "+ myDoc);
```

����Ĵ���Ӧ�������
```java
Text search matches: 2
Text search matches (english): 2
Highest scoring document: { "_id" : 1 , "content" : "additional content" , "score" : 0.75}
```

�������text search,�ο�[text index and $text query operator]()

# Replica
# Deploy a Replica Set

��ƪ�̳̽���������λ����������еĲ����п��Ʒ��ʵ�`mongod`��������`replica set`.

�����Ҫ�������п��Ʒ��ʹ��ܵ�`replica set`,�ο�[Deploy Replica Set and Configure Authentication and Authorization](http://docs.mongodb.org/manual/tutorial/deploy-replica-set-with-auth/). �������Ҫ��һ��������MongoDB�ϲ���`replica set`, ���Բο�[Convert a Standalone to a Replica Set](http://docs.mongodb.org/manual/tutorial/convert-standalone-to-replica-set/). ���ڸ����`replica set`������Ϣ,�ο�[Replication](http://docs.mongodb.org/manual/replication/)��[Replica Set Deployment Architectures](http://docs.mongodb.org/manual/core/replica-set-architectures/)

## Overview

����������Ա��`replica sets`���㹻Ӧ�������зֺ��������͵�ϵͳʧ��. ��Щsets���㹻��������Ӧ���ֲ�ʽ���͵Ķ�����. `Replica sets`Ӧ�ñ�֤���ĳ�Ա����ά����һ��������. ���������ܹ���֤������[elections](http://docs.mongodb.org/manual/core/replica-set-elections/). ������ڶ�`replica sets`�����,�ο�[Replication overview](http://docs.mongodb.org/manual/core/replication-introduction/)

�����Ĳ�����: ��������Ҫ��Ϊ`replica set`��Ա��`mongod`, Ȼ������`replica set`, ���`mongod`��ӵ�`replica set`��.

## Requirements

For production deployments, you should maintain as much separation between members as possible by hosting the mongod instances on separate machines. When using virtual machines for production deployments, you should place each mongod instance on a separate host server serviced by redundant power circuits and redundant network paths.

����������׶�, ��Ӧ�þ����ڲ�ͬ�������ϲ������`mongod`�ĳ�Ա. ��ʹ����������������������ʱ, ��Ӧ���ڲ�ͬ�������������϶�����һ��'mongod'.

���㴴��`replica set`֮ǰ, ������ȼ��������������ܹ�����ÿһ����Ա���ܹ��໥������. һ���ɹ���`replica set`����, ÿһ����Ա���ܹ����ӵ���������Ա. ������μ������,�ο�[Test Connections Between all Members](http://docs.mongodb.org/manual/tutorial/troubleshoot-replica-sets/#replica-set-troubleshooting-check-connection)

## Considerations When Deploying a Replica Set

### Architecture

�������׶�, ��`replica set`�����ĳ�Ա����ͬһ̨������. ������ܵĻ�, �󶨵�MongoDB��׼�˿�27017��. ʹ��`bind_ip`ѡ��ȷ��MongoDB��������úõĵ�ַ��������Ӧ�ó��������.

���`replica set`�ڲ�ͬ�Ļ����ڲ���, ��ôӦ��ȷ���������`mongod`ʵ�������ڵ�һվ����.�ο�[Replica Set Deployment Architectures]()

### Connectivity

ȷ�����������е�`replica set`��Ա�Ϳͻ��˵������ܹ���ȫ�͸�Ч�ش���:

* ����һ�������˽������. ȷ����������һ������վ�����·�ɲ�ͬ��Ա�� �����е�����.
* ���÷��ʿ����ܹ���ֹδ֪�Ŀͻ������ӵ� `replica set`��
* ��������ͷ���ǽ�����Ա��վ�ͳ�վ���������������MongoDB��Ĭ�϶˿ں����������.

����ȷ��`replica set`��ÿ����Ա������ͨ���ɽ�����`DNS`����`hostname`���ʵ�. ��Ӧ��ǡ����������`DNS`���ƻ���ͨ��`/etc/hosts`�ļ���ӳ���������

### Configuration
Specify the run time configuration on each system in a configuration file stored in /etc/mongodb.conf or a related location. Create the directory where MongoDB stores data files before deploying MongoDB.

For more information about the run time options used above and other configuration options, see Configuration File Options.

## Procedure

����Ĳ����������`access control`ʧЧ���������β���replica set

###### 1. Start each member of the replica set with the appropriate options.


����`mongod`Ȼ��ͨ��`replSet`ѡ���趨`replica set`����, ��`replica set`�����һ����Ա. �����Ҫ�����������в���,�ο�[Replication Options]()

������Ӧ�ó��������˶��`replica set`, ÿһ��`replica set`��Ӧ����һ������������. ĳЩ���������`replica set`���ƽ�`replica set`���ӽ��з���.

������һ��ʾ����
```
	mongod --replSet "rs0"
```

��Ҳͨ�������ļ�����`replica set`����. �����Ҫͨ�������ļ�����`mongod`, ��ô����Ҫ`--config`ѡ��ָ�������ļ�
```
mongod --config $HOME/.mongodb/config
```
����������׶�, �����ͨ������һ�����ƽű��������������. ���ǿ��ƽű���ʹ�ó����˸ý̵̳Ľ��ܷ�Χ.

> ע��:
>
> ������c��û�д���C:/data/db, ��ô���׳� ��Hotfix KB2731284 or later update is not installed. �Լ� C:\data\db not found ������. 
>
> ��ô�����Ҫ�������ϼ��� --dbpath ѡ����

###### 2. Connect a mongo shell to a replica set member.

����չʾ��������ӵ���`localhost:27017`�����е�`mongod`:
```
mongo
```

###### 3. Initiate the replica set.

������`mongo`shell��ʹ��`rs.initiate()`���ó�Ա.
```
rs.initiate()
```
MongoDBʹ��`replica set`Ĭ������������һ��������ǰ��Ա��`replica set`

> ע��:
>
> ������̴����Ҫ�����ӵ�ʱ��, ������Ҫ���ĵ��Ե�һ��.

###### 4. Verify the initial replica set configuration.

��`mongo`shell��ʹ��`rs.conf()`���`replica set`����:
```
rs.conf()
```

�����`replica set`��������������Ľṹ
```json
{
   "_id" : "rs0",
   "version" : 1,
   "members" : [
      {
         "_id" : 1,
         "host" : "mongodb0.example.net:27017"
      }
   ]
}
```

###### 5. Add the remaining members to the replica set.

��`mongo`shell��ʹ��`rs.add()`�������������Ա:
```
rs.add("mongodb1.example.net")
rs.add("mongodb2.example.net")
```

�����һ��֮��,��ͻ����һ��ӵ���������ܵ�`replica set`. �µ�`replica set`��ѡ��һ����Ҫ����.

###### 6. Check the status of the replica set.

��`mongo`shell��ʹ��`rs.status()`�����鿴`replica set`״̬.
```
rs.status()
```

## Replication Introduction

`Replication` �����ڶ�̨������������ͬ����һ������.

## Purpose of Replication

Replication provides redundancy and increases data availability. With multiple copies of data on different database servers, replication protects a database from the loss of a single server. Replication also allows you to recover from hardware failure and service interruptions. With additional copies of the data, you can dedicate one to disaster recovery, reporting, or backup.

In some cases, you can use replication to increase read capacity. Clients have the ability to send read and write operations to different servers. You can also maintain copies in different data centers to increase the locality and availability of data for distributed applications.

`Replication`�ṩ�˼��ٺ�������ݵĿ�����. 

��ĳЩ�����, ͨ��`replication`������߶����ݵ�����. 

## Replication in MongoDB

A replica set is a group of mongod instances that host the same data set. One mongod, the primary, receives all write operations. All other instances, secondaries, apply operations from the primary so that they have the same data set.

The primary accepts all write operations from clients. Replica set can have only one primary. Because only one member can accept write operations, replica sets provide strict consistency for all reads from the primary. To support replication, the primary logs all changes to its data sets in its oplog. See primary for more information.

![replica-set-read-write-operations-primary](/images/mongodb/replica-set-read-write-operations-primary.png)

Diagram of default routing of reads and writes to the primary.

The secondaries replicate the primary��s oplog and apply the operations to their data sets. Secondaries�� data sets reflect the primary��s data set. If the primary is unavailable, the replica set will elect a secondary to be primary. By default, clients read from the primary, however, clients can specify a read preferences to send read operations to secondaries. Reads from secondaries may return data that does not reflect the state of the primary. See secondaries for more information.

![replica-set-primary-with-two-secondaries](/images/mongodb/replica-set-primary-with-two-secondaries.png)

Diagram of a 3 member replica set that consists of a primary and two secondaries.
You may add an extra mongod instance to a replica set as an arbiter. Arbiters do not maintain a data set. Arbiters only exist to vote in elections. If your replica set has an even number of members, add an arbiter to obtain a majority of votes in an election for primary. Arbiters do not require dedicated hardware. See arbiter for more information.

Diagram of a replica set that consists of a primary, a secondary, and an arbiter.

![replica-set-primary-with-secondary-and-arbiter](/images/mongodb/replica-set-primary-with-secondary-and-arbiter.png)

An arbiter will always be an arbiter. A primary may step down and become a secondary. A secondary may become the primary during an election.

## Asynchronous Replication

Secondaries apply operations from the primary asynchronously. By applying operations after the primary, sets can continue to function without some members. However, as a result secondaries may not return the most current data to clients.

See Replica Set Oplog and Replica Set Data Synchronization for more information. See Read Preference for more on read operations and secondaries.

## Automatic Failover

When a primary does not communicate with the other members of the set for more than 10 seconds, the replica set will attempt to select another member to become the new primary. The first secondary that receives a majority of the votes becomes primary.

![replica-set-trigger-election](/images/mongodb/replica-set-trigger-election.png)

Diagram of an election of a new primary. In a three member replica set with two secondaries, the primary becomes unreachable. The loss of a primary triggers an election where one of the secondaries becomes the new primary
See Replica Set Elections and Rollbacks During Replica Set Failover for more information.

## Additional Features

Replica sets provide a number of options to support application needs. For example, you may deploy a replica set with members in multiple data centers, or control the outcome of elections by adjusting the priority of some members. Replica sets also support dedicated members for reporting, disaster recovery, or backup functions.

See Priority 0 Replica Set Members, Hidden Replica Set Members and Delayed Replica Set Members for more information.


# Shard
