# mongodb-guide

- [mongodb-guide](#mongodb-guide)
  - [Introduction](#introduction)
  - [Getting Started](#getting-started)
    - [สร้าง Cluster](#สร้าง-cluster)
    - [Install MongoDB Shell (mongosh)](#install-mongodb-shell-mongosh)
    - [Connect to the database](#connect-to-the-database)
  - [Query API](#query-api)
  - [`mongosh` Create Database](#mongosh-create-database)
    - [Show all databases](#show-all-databases)
    - [Change or Create a Database](#change-or-create-a-database)
  - [`mongosh` Create Collection](#mongosh-create-collection)
  - [`mongosh` Insert](#mongosh-insert)
  - [`mongosh` Find](#mongosh-find)
    - [Find Data](#find-data)
      - [`find()`](#find)
      - [`findOne()`](#findone)
    - [Querying Data](#querying-data)
    - [Projection](#projection)
      - [Error Case](#error-case)
  - [`mongosh` Update](#mongosh-update)
    - [`updateOne()`](#updateone)
    - [Insert if not found](#insert-if-not-found)
    - [`updateMany()`](#updatemany)
  - [`mongosh` Delete](#mongosh-delete)
    - [`deleteOne()`](#deleteone)
    - [`deleteMany()`](#deletemany)
  - [Query Operators](#query-operators)
    - [Comparison](#comparison)
    - [Logical](#logical)
    - [Evaluation](#evaluation)
  - [Update Operators](#update-operators)
    - [Fields](#fields)
    - [Array](#array)
  - [Aggregation Pipelines](#aggregation-pipelines)
    - [Aggregation `$group`](#aggregation-group)
    - [Aggregation `$limit`](#aggregation-limit)
    - [Aggregation `$project`](#aggregation-project)
    - [Aggregation `$sort`](#aggregation-sort)


## Introduction

MongoDB คือ `document database`

document database คือ database ที่เก็บข้อมูลในรูปแบบของ document ซึ่ง document คือข้อมูลที่เก็บในรูปแบบของ `key-value pair` โดย document จะเป็น `JSON object` หรือ `BSON object` ซึ่งเป็น binary ของ JSON object

ตัวอย่าง document ที่เก็บใน MongoDB

```javascript
{
	title: "Post Title 1",
	body: "Body of post.",
	category: "News",
	likes: 1,
	tags: ["news", "events"],
	date: Date()
}
```

## Getting Started

### สร้าง Cluster

1. ไปที่เว็บไซต์ [MongoDB](https://www.mongodb.com/)
2. คลิกที่ `Try Free` และลงทะเบียน

### Install MongoDB Shell (mongosh)

มีหลายวิธีที่สามารถใช้เพื่อเชื่อมต่อกับ MongoDB ได้ แต่วิธีที่ง่ายที่สุดคือการใช้ MongoDB Shell (mongosh)

ตรวจสอบการติดตั้งโดยการพิมพ์คำสั่งต่อไปนี้ใน terminal

```bash
mongosh --version
```

### Connect to the database

ใน MongoDB Atlas dashboard ตรง `Databases` > คลิกปุ่ม `Connect` > เลือก `Connect with the MongoDB Shell` และคุณจะได้รับ URI ที่สามารถใช้เพื่อเชื่อมต่อกับ database ได้

ตัวอย่าง URI ที่ได้จาก MongoDB Atlas

```bash
mongosh "mongodb+srv://cluster0.abcde.mongodb.net/<dbname>" --apiVersion 1 --username <username>
```

หลังจากนั้น จะสามารถ prompt ให้ใส่ password ของ user ที่ใช้เพื่อเชื่อมต่อกับ database ได้

## Query API

Query API ใน MongoDB คือการใช้คำสั่งเพื่อติดต่อกับ database

มี 2 วิธีในการ query ข้อมูลใน MongoDB คือ

1. CRUD Operations
2. Aggregation Pipelines

เราสามารถใช้ `MongoDB Query API` เพื่อ

- ค้นหาข้อมูล ด้วย `mongosh`, Compass, VS Code, หรือ ภาษาโปรแกรมมิ่งอื่น ๆ
- โอนย้ายข้อมูลโดย aggregation pipelines
- join ข้อมูลจากหลาย collection
- กราฟและ geospatial queries
- Full-text search
- index ข้อมูลเพื่อเพิ่มประสิทธิภาพในการ query
- Time-series analysis

## `mongosh` Create Database

เมื่อ connect ครั้งแรก เราจะเห็น database ที่มีชื่อว่า `myFirstDatabase`

### Show all databases

ใช้คำสั่ง

```bash
show dbs
```

เราจะเห็นว่า ไม่มี `myFirstDatabase` แสดงออกมา เพราะว่า `myFirstDatabase` ไม่มี collection ใด ๆ ที่ถูกสร้างไว้

### Change or Create a Database

อย่างเช่น ถ้าต้องการสร้าง database ชื่อว่า `blog` ใช้คำสั่ง

```bash
use blog
```

## `mongosh` Create Collection

มี 2 วิธี

1. ใช้คำสั่ง `db.createCollection('<collection>')`
   
```bash
db.createCollection('posts')
```

2. สามารถสร้าง collection ในระหว่างการ insert document โดยใช้คำสั่ง `db.<collection>.insertOne()`
   
```bash
db.posts.insertOne({"title": "Post 1"})
```

Remember: ถ้า collection ไม่มี document ใด ๆ อยู่ มันจะไม่แสดงออกมาใน `show collections` หรือ `db.getCollectionNames()` จนกว่าจะมี document อยู่ใน collection นั้น ๆ

## `mongosh` Insert

ใช้คำสั่ง `db.<collection>.insertOne()` เพื่อ insert document ใน collection

```bash
db.posts.insertOne({
  title: "Post 1",
  body: "Body of post 1",
  category: "News",
  likes: 1,
  tags: ["news", "events"],
  date: Date()
})
```

หรือใช้คำสั่ง `db.<collection>.insertMany()` เพื่อ insert หลาย document ใน collection

```bash
db.posts.insertMany([
  {
    title: "Post 2",
    body: "Body of post 2",
    category: "News",
    likes: 2,
    tags: ["news", "events"],
    date: Date()
  },
  {
    title: "Post 3",
    body: "Body of post 3",
    category: "News",
    likes: 3,
    tags: ["news", "events"],
    date: Date()
  }
])
```

## `mongosh` Find

### Find Data

มี 2 วิธีในการค้นหาข้อมูลใน MongoDB คือ `find()` และ `findOne()`

#### `find()`

ใช้คำสั่ง `db.<collection>.find()` เพื่อค้นหา document ใน collection

```bash
db.posts.find()
```

หรือใช้คำสั่ง `db.<collection>.find().pretty()` เพื่อแสดงข้อมูลในรูปแบบที่สวยงาม

```bash
db.posts.find().pretty()
```

#### `findOne()`

ใช้คำสั่ง `db.<collection>.findOne()` เพื่อค้นหา document แรกที่เจอใน collection

```bash
db.posts.findOne()
```

### Querying Data

ใช้คำสั่ง `db.<collection>.find()` และใส่เงื่อนไขเพื่อค้นหาข้อมูลที่ต้องการ

```bash
db.posts.find({ title: "Post 1" })
```

หรือใช้คำสั่ง `db.<collection>.find()` และใส่เงื่อนไขเพื่อค้นหาข้อมูลที่ต้องการ และใช้ projection เพื่อเลือกเฉพาะ field ที่ต้องการ

```bash
db.posts.find({ title: "Post 1" }, { title: 1, _id: 0 })
```

### Projection

ใช้ projection เพื่อเลือกเฉพาะ field ที่ต้องการ อย่างเช่น ถ้าต้องการเลือกเฉพาะ field `title` และ `date` ใช้คำสั่ง

```bash
db.posts.find({}, { title: 1, date: 1 })
```

จะได้ผลลัพธ์เป็น

```bash
[
  {
    _id: ObjectId("62c350dc07d768a33fdfe9b0"),
    title: 'Post Title 1',
    date: 'Mon Jul 04 2022 15:43:08 GMT-0500 (Central Daylight Time)'
  },
  {
    _id: ObjectId("62c3513907d768a33fdfe9b1"),
    title: 'Post Title 2',
    date: 'Mon Jul 04 2022 15:44:41 GMT-0500 (Central Daylight Time)'
  },
  {
    _id: ObjectId("62c3513907d768a33fdfe9b2"),
    title: 'Post Title 3',
    date: 'Mon Jul 04 2022 15:44:41 GMT-0500 (Central Daylight Time)'
  },
  {
    _id: ObjectId("62c3513907d768a33fdfe9b3"),
    title: 'Post Title 4',
    date: 'Mon Jul 04 2022 15:44:41 GMT-0500 (Central Daylight Time)'
  }
]
```

สังเกตว่า field `_id` จะแสดงออกมาเสมอ ถึงแม้ว่าไม่ได้ระบุ

เราสามารถใช้ `1` เพื่อแสดง field ที่ต้องการ และ `0` เพื่อไม่แสดง field ที่ไม่ต้องการ

```bash
db.posts.find({}, { _id: 0, title: 1, date: 1 })
```

จะได้ผลลัพธ์เป็น

```bash
[
  {
    title: 'Post Title 1',
    date: 'Mon Jul 04 2022 15:43:08 GMT-0500 (Central Daylight Time)'
  },
  {
    title: 'Post Title 2',
    date: 'Mon Jul 04 2022 15:44:41 GMT-0500 (Central Daylight Time)'
  },
  {
    title: 'Post Title 3',
    date: 'Mon Jul 04 2022 15:44:41 GMT-0500 (Central Daylight Time)'
  },
  {
    title: 'Post Title 4',
    date: 'Mon Jul 04 2022 15:44:41 GMT-0500 (Central Daylight Time)'
  }
]
```

#### Error Case

เราไม่สามารถใช้ `0` และ `1` ในการเลือก field ในคราวเดียวกัน ในการเลือก field ให้ใช้ `1` เท่านั้น (ยกเว้น field `_id` ที่สามารถใช้ `0` ในคราวเดียวกันได้ ดังตัวอย่างก่อนหน้า)

ยกตัวอย่างเช่น ถ้าไม่ต้องการเลือก field `category` ใช้คำสั่ง

```bash
db.posts.find({}, { category: 0 })
```

จะได้ผลลัพธ์เป็น

```bash
[
  {
    _id: ObjectId("62c350dc07d768a33fdfe9b0"),
    title: 'Post Title 1',
    body: 'Body of post.',
    likes: 1,
    tags: [ 'news', 'events' ],
    date: 'Mon Jul 04 2022 15:43:08 GMT-0500 (Central Daylight Time)'
  },
  {
    _id: ObjectId("62c3513907d768a33fdfe9b1"),
    title: 'Post Title 2',
    body: 'Body of post.',
    likes: 2,
    tags: [ 'news', 'events' ],
    date: 'Mon Jul 04 2022 15:44:41 GMT-0500 (Central Daylight Time)'
  },
  {
    _id: ObjectId("62c3513907d768a33fdfe9b2"),
    title: 'Post Title 3',
    body: 'Body of post.',
    likes: 3,
    tags: [ 'news', 'events' ],
    date: 'Mon Jul 04 2022 15:44:41 GMT-0500 (Central Daylight Time)'
  },
  {
    _id: ObjectId("62c3513907d768a33fdfe9b3"),
    title: 'Post Title 4',
    body: 'Body of post.',
    likes: 4,
    tags: [ 'news', 'events' ],
    date: 'Mon Jul 04 2022 15:44:41 GMT-0500 (Central Daylight Time)'
  }
]
```

เราจะได้ error ทันทีถ้า ใช้ `1` และ `0` ในการเลือก field ในคราวเดียวกัน

```bash
db.posts.find({}, {title: 1, date: 0})
```

จะได้ error แบบนี้

```bash
MongoServerError: Cannot do exclusion on field date in inclusion projection
```

## `mongosh` Update

การอัพเดท document ใน MongoDB มี 2 วิธี คือ `updateOne()` และ `updateMany()`

parameter ของ `updateOne()` และ `updateMany()` ตัวแรก คือ query ที่ต้องการอัพเดท และตัวที่สอง คือ document ที่ต้องการอัพเดท

### `updateOne()`

ใช้คำสั่ง `db.<collection>.updateOne()` เพื่อ update document แรกที่เจอใน collection (คล้ายกับ `findOne()`)

ตัวอย่างเช่น ถ้าต้องการอัพเดท document ที่มี field `title` เป็น `Post Title 1` และ อัพเดท field `likes` ของ document นั้น ใช้คำสั่ง

```bash
db.posts.updateOne({ title: "Post Title 1" }, { $set: { likes: 2 } })
```

จะได้ผลลัพธ์เป็น

```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedId: null
}
```

check document ที่อัพเดท

```bash
db.posts.find({ title: "Post Title 1" })
```

จะได้ผลลัพธ์เป็น

```bash
[
  {
    _id: ObjectId("62c350dc07d768a33fdfe9b0"),
    title: 'Post Title 1',
    body: 'Body of post.',
    category: 'News',
    likes: 2,
    tags: [ 'news', 'events' ],
    date: 'Mon Jul 04 2022 15:43:08 GMT-0500 (Central Daylight Time)'
  }
]
```

### Insert if not found

ถ้า document ที่ต้องการอัพเดทไม่มีอยู่ ให้เพิ่ม parameter `upsert: true` ในการอัพเดท

เช่น

```bash
db.posts.updateOne( 
  { title: "Post Title 5" }, 
  {
    $set:
      {
        title: "Post Title 5",
        body: "Body of post.",
        category: "Event",
        likes: 5,
        tags: ["news", "events"],
        date: Date()
      }
  },
  { upsert: true }
)
```

จะได้ผลลัพธ์เป็น

```bash
{
  acknowledged: true,
  insertedId: ObjectId("62c358734684a30f296cfe5f"),
  matchedCount: 0,
  modifiedCount: 0,
  upsertedCount: 1
}
```

### `updateMany()`

ใช้คำสั่ง `db.<collection>.updateMany()` เพื่อ update document ทั้งหมดที่เจอใน collection

ตัวอย่างเช่น ต้องการอัพเดท field `likes` ของ document ทั้งหมดให้เป็น  `1` เราจะใช้ operator `inc`

```bash
db.posts.updateMany({}, { $inc: { likes: 1 } })
```

จะได้ผลลัพธ์เป็น

```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 5,
  modifiedCount: 5,
  upsertedCount: 0
}
```

## `mongosh` Delete

การลบ document ใน MongoDB มี 2 วิธี คือ `deleteOne()` และ `deleteMany()`

### `deleteOne()`

ใช้คำสั่ง `db.<collection>.deleteOne()` เพื่อลบ document แรกที่เจอใน collection (คล้ายกับ `findOne()`)

ตัวอย่างเช่น ถ้าต้องการลบ document ที่มี field `title` เป็น `Post Title 5` ใช้คำสั่ง

```bash
db.posts.deleteOne({ title: "Post Title 5" })
```

จะได้ผลลัพธ์เป็น

```bash
{ acknowledged: true, deletedCount: 1 }
```

### `deleteMany()`

ใช้คำสั่ง `db.<collection>.deleteMany()` เพื่อลบ document ทั้งหมดที่เจอใน collection

ตัวอย่างเช่น ถ้าต้องการลบ document ที่มี field `category` เป็น `Technology` ใช้คำสั่ง

```bash
db.posts.deleteMany({ category: "Technology" })  
```

จะได้ผลลัพธ์เป็น

```bash
{ acknowledged: true, deletedCount: 1 }
```

## Query Operators

ในการ query ข้อมูลใน MongoDB มี `query operators` ที่สามารถใช้เพื่อ query ข้อมูลได้

### Comparison

ใช้ `comparison operators` เพื่อ query ข้อมูลเอามาเปรียบเทียบ

- `$eq` เท่ากับ
- `$ne` ไม่เท่ากับ
- `$gt` มากกว่า
- `$gte` มากกว่าหรือเท่ากับ
- `$lt` น้อยกว่า
- `$lte` น้อยกว่าหรือเท่ากับ
- `$in` อยู่ใน array

### Logical

ใช้ `logical operators` เพื่อ query ข้อมูลเอามาเปรียบเทียบทาง logic กับ query อื่น ๆ

- `$and` และ
- `$or` หรือ
- `$nor` ไม่ใช่หรือ
- `$not` ไม่ใช่

### Evaluation

ใช้ `evaluation operators` เพื่อ query ข้อมูลเอามาเปรียบเทียบทางการประเมินค่า

- `$regex` ใช้ regular expression
- `$exists` ตรวจสอบว่า field นั้น ๆ มีอยู่หรือไม่
- `$type` ตรวจสอบ type ของ field
- `$mod` ตรวจสอบเงื่อนไขที่เป็นเลข
- `$text` ใช้ text search
- `$where` ใช้ JavaScript expression
- `$jsonSchema` ใช้ JSON schema
- `$geoIntersects` ใช้ geospatial queries
- `$geoWithin` ใช้ geospatial queries
- `$near` ใช้ geospatial queries
- `$nearSphere` ใช้ geospatial queries
- `$all` ตรวจสอบว่า array นั้น ๆ มีทุกค่าหรือไม่
- `$elemMatch` ตรวจสอบว่า array นั้น ๆ มีค่าที่ตรงกับเงื่อนไขหรือไม่
- `$size` ตรวจสอบขนาดของ array
- `$bitsAllClear` ตรวจสอบว่า bit ทั้งหมดใน field นั้น ๆ มีค่าเป็น 0 หรือไม่
- `$bitsAllSet` ตรวจสอบว่า bit ทั้งหมดใน field นั้น ๆ มีค่าเป็น 1 หรือไม่
- `$bitsAnyClear` ตรวจสอบว่า bit ใด ๆ ใน field นั้น ๆ มีค่าเป็น 0 หรือไม่
- `$bitsAnySet` ตรวจสอบว่า bit ใด ๆ ใน field นั้น ๆ มีค่าเป็น 1 หรือไม่
- `$comment` ใช้ comment ใน query
- `$meta` ใช้ metadata ใน text search
- `$slice` ใช้ projection ใน array
- `$elemMatch` ใช้ query ใน array
- `$bitsAllClear` ใช้ bit ใน query
- `$bitsAllSet` ใช้ bit ใน query
- `$bitsAnyClear` ใช้ bit ใน query
- `$bitsAnySet` ใช้ bit ใน query

## Update Operators

ในการ update ข้อมูลใน MongoDB มี `update operators` ที่สามารถใช้เพื่อ update ข้อมูลได้

### Fields

ใช้ `fields operators` เพื่อ update ข้อมูลใน field ของ document

- `$set` ใช้เพื่อ set ค่าใหม่ใน field ของ document
- `$unset` ใช้เพื่อลบ field ของ document
- `$rename` ใช้เพื่อเปลี่ยนชื่อ field ของ document
- `$inc` ใช้เพื่อเพิ่มค่าของ field ของ document
- `$mul` ใช้เพื่อคูณค่าของ field ของ document
- `$min` ใช้เพื่อเปรียบเทียบค่าของ field ของ document และเลือกค่าที่น้อยที่สุด
- `$max` ใช้เพื่อเปรียบเทียบค่าของ field ของ document และเลือกค่าที่มากที่สุด
- `$currentDate` ใช้เพื่อ set ค่าของ field ให้เป็น current date หรือ current timestamp

### Array

ใช้ `array operators` เพื่อ update ข้อมูลใน array ของ document

- `$addToSet` ใช้เพื่อเพิ่มค่าใหม่ใน array ของ field ของ document และไม่เพิ่มค่าซ้ำ
- `$pop` ใช้เพื่อลบค่าของ field ของ document ที่เป็น array และเลือกค่าแรกหรือค่าสุดท้าย
- `$pull` ใช้เพื่อลบค่าของ field ของ document ที่เป็น array ตามเงื่อนไขที่กำหนด
- `$push` ใช้เพื่อเพิ่มค่าใหม่ใน array ของ field ของ document

## Aggregation Pipelines

Aggregation Pipelines ใน MongoDB คือการใช้คำสั่งเพื่อโอนย้ายข้อมูล และทำการคำนวณข้อมูล โดยการใช้หลายๆ ขั้นตอน เรียกว่า `stages` โดยแต่ละ `stage` จะทำการโอนย้ายข้อมูล และทำการคำนวณข้อมูล และส่งผลลัพธ์ไปยัง `stage` ถัดๆ ไป

ข้อควรระวังคือ ลำดับของ `stage` เป็นเรื่องสำคัญ แต่ละ `stage` จะทำการ aggregate document ที่ได้จาก `stage` ก่อนหน้าเท่านั้น

ตัวอย่าง

```bash
db.posts.aggregate([
  {
    $match: { likes: { $gt: 1 } }
  },
  {
    $group: { _id: "$category", totalLikes: { $sum: "$likes" } }
  }
])
```

จะได้ผลลัพธ์เป็น

```bash
[ { _id: 'News', totalLikes: 3 }, { _id: 'Event', totalLikes: 8 } ]
```

ตัวอย่างในบทเรียน เราจะใช้ dataset จาก [MongoDB Sample Dataset](https://docs.atlas.mongodb.com/sample-data/)

### Aggregation `$group`

ใช้ `$group` เพื่อ group ข้อมูล และทำการคำนวณข้อมูล โดย `_id` expression

อย่าสับสนกับ `_id` ของ document ที่เก็บใน collection ที่เราสร้างขึ้นมา ในการใช้ `$group` ให้ใช้ `_id` expression ที่ต้องการ

ในตัวอย่าง เลือกใช้ database `sample_airbnb` collection `listingsAndReviews` และใช้คำสั่ง
  
```bash
db.listingsAndReviews.aggregate(
    [ { $group : { _id : "$property_type" } } ]
)
```

จะได้ผลลัพธ์เป็น

```bash
[
  { _id: 'Aparthotel' },
  { _id: 'Apartment' },
  { _id: 'Barn' },
  { _id: 'Bed and breakfast' },
  { _id: 'Boat' },
  { _id: 'Boutique hotel' },
  { _id: 'Bungalow' },
  { _id: 'Cabin' },
  { _id: 'Camper/RV' },
  { _id: 'Campsite' },
  { _id: 'Casa particular (Cuba)' },
  { _id: 'Castle' },
  { _id: 'Chalet' },
  { _id: 'Condominium' },
  { _id: 'Cottage' },
  { _id: 'Earth house' },
  { _id: 'Farm stay' },
  { _id: 'Guest suite' },
  { _id: 'Guesthouse' },
  { _id: 'Heritage hotel (India)' }
]
```

### Aggregation `$limit`

ใช้ `$limit` เพื่อจำกัดจำนวน document ที่ต้องการ เพื่อส่งเป็น output ไปยัง `stage` ถัดๆ ไป

ในตัวอย่าง เลือกใช้ database `sample_mflix` collection `movies` และใช้คำสั่ง

```bash
db.movies.aggregate([ { $limit: 1 } ])
```

จะได้ผลลัพธ์เป็น

```bash
[
  {
    _id: ObjectId("573a1390f29313caabcd4135"),
    plot: 'Three men hammer on an anvil and pass a bottle of beer around.',
    genres: [ 'Short' ],
    runtime: 1,
    cast: [ 'Charles Kayser', 'John Ott' ],
    num_mflix_comments: 0,
    title: 'Blacksmith Scene',
    fullplot: 'A stationary camera looks at a large anvil with a blacksmith behind it and one on either side. The smith in the middle draws a heated metal rod from the fire, places it on the anvil, and all three begin a rhythmic hammering. After several blows, the metal goes back in the fire. One smith pulls out a bottle of beer, and they each take a swig. Then, out comes the glowing metal and the hammering resumes.',
    countries: [ 'USA' ],
    released: ISODate("1893-05-09T00:00:00.000Z"),
    directors: [ 'William K.L. Dickson' ],
    rated: 'UNRATED',
    awards: { wins: 1, nominations: 0, text: '1 win.' },
    lastupdated: '2015-08-26 00:03:50.133000000',
    year: 1893,
    imdb: { rating: 6.2, votes: 1189, id: 5 },
    type: 'movie',
    tomatoes: {
      viewer: { rating: 3, numReviews: 184, meter: 32 },
      lastUpdated: ISODate("2015-06-28T18:34:09.000Z")
    }
  }
]
```

จากตัวอย่าง เราจะเห็นว่ามีเพียง document เดียวที่ได้จาก output

### Aggregation `$project`

ใช้ `$project` เพื่อเลือก field ที่ต้องการ และเปลี่ยนชื่อ field ที่ต้องการ

มันจะคล้ายกับ `projection` ในการ query ข้อมูล แต่ `$project` ใช้ใน aggregation pipelines

ในตัวอย่าง เลือกใช้ database `sample_restaurants` collection `restaurants` และใช้คำสั่ง

```bash
db.restaurants.aggregate([
  {
    $project: {
      "name": 1,
      "cuisine": 1,
      "address": 1
    }
  },
  {
    $limit: 5
  }
])
```

จะได้ผลลัพธ์เป็น
  
```bash
[
  {
    _id: ObjectId("5eb3d668b31de5d588f4292a"),
    address: {
      building: '2780',
      coord: [ -73.98241999999999, 40.579505 ],
      street: 'Stillwell Avenue',
      zipcode: '11224'
    },
    cuisine: 'American',
    name: 'Riviera Caterer'
  },
  {
    _id: ObjectId("5eb3d668b31de5d588f4292b"),
    address: {
      building: '7114',
      coord: [ -73.9068506, 40.6199034 ],
      street: 'Avenue U',
      zipcode: '11234'
    },
    cuisine: 'Delicatessen',
    name: "Wilken'S Fine Food"
  },
  {
    _id: ObjectId("5eb3d668b31de5d588f4292c"),
    address: {
      building: '2206',
      coord: [ -74.1377286, 40.6119572 ],
      street: 'Victory Boulevard',
      zipcode: '10314'
    },
    cuisine: 'Jewish/Kosher',
    name: 'Kosher Island'
  },
  {
    _id: ObjectId("5eb3d668b31de5d588f4292d"),
    address: {
      building: '469',
      coord: [ -73.961704, 40.662942 ],
      street: 'Flatbush Avenue',
      zipcode: '11225'
    },
    cuisine: 'Hamburgers',
    name: "Wendy'S"
  },
  {
    _id: ObjectId("5eb3d668b31de5d588f4292e"),
    address: {
      building: '1007',
      coord: [ -73.856077, 40.848447 ],
      street: 'Morris Park Ave',
      zipcode: '10462'
    },
    cuisine: 'Bakery',
    name: 'Morris Park Bake Shop'
  }
]
```

field `_id` จะแสดงออกมาเสมอ ถึงแม้ว่าไม่ได้ระบุ

เราสามารถใช้ `1` เพื่อแสดง field ที่ต้องการ และ `0` เพื่อไม่แสดง field ที่ไม่ต้องการ

และข้อจำกัดคือ ไม่สามารถใช้ `1` และ `0` ในการเลือก field ในคราวเดียวกันได้ เหมือนกันกับการ projection ในการ query ข้อมูล

### Aggregation `$sort`

ใช้ `$sort` เพื่อเรียงลำดับข้อมูล

ในตัวอย่าง เลือกใช้ database `sample_airbnb` collection `listingsAndReviews` และใช้คำสั่ง

```bash
db.listingsAndReviews.aggregate([ 
  { 
    $sort: { "accommodates": -1 } 
  },
  {
    $project: {
      "name": 1,
      "accommodates": 1
    }
  },
  {
    $limit: 5
  }
])
```

เราจะใช้ `1` เพื่อเรียงลำดับข้อมูลจากน้อยไปมาก และ `-1` เพื่อเรียงลำดับข้อมูลจากมากไปน้อย

จะได้ผลลัพธ์เป็น

```bash
[
  {
    _id: '19990097',
    name: 'House close to station & direct to opera house....',
    accommodates: 16
  },
  { _id: '19587001', name: 'Kaena O Kekai', accommodates: 16 },
  {
    _id: '20958766',
    name: 'Great Complex of the Cellars',
    accommodates: 16
  },
  {
    _id: '12509339',
    name: 'Barra da Tijuca beach house',
    accommodates: 16
  },
  {
    _id: '20455499',
    name: 'DOWNTOWN VIP MONTREAL ,HIGH END DECOR,GOOD VALUE..',
    accommodates: 16
  }
]
```

จากตัวอย่าง เราจะเห็นว่ามีเพียง 5 document ที่ได้จาก output และเรียงลำดับข้อมูล field `accommodates` จากมากไปน้อย



