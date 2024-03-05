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
    - [`find()`](#find)
    - [`findOne()`](#findone)


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

มี 2 วิธีในการค้นหาข้อมูลใน MongoDB คือ `find()` และ `findOne()`

### `find()`

ใช้คำสั่ง `db.<collection>.find()` เพื่อค้นหา document ใน collection

```bash
db.posts.find()
```

หรือใช้คำสั่ง `db.<collection>.find().pretty()` เพื่อแสดงข้อมูลในรูปแบบที่สวยงาม

```bash
db.posts.find().pretty()
```

### `findOne()`

ใช้คำสั่ง `db.<collection>.findOne()` เพื่อค้นหา document แรกที่เจอใน collection

```bash
db.posts.findOne()
```