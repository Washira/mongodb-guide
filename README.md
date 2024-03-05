# mongodb-guide

- [mongodb-guide](#mongodb-guide)
  - [Introduction](#introduction)
  - [Getting Started](#getting-started)
    - [สร้าง Cluster](#สร้าง-cluster)
    - [Install MongoDB Shell (mongosh)](#install-mongodb-shell-mongosh)
    - [Connect to the database](#connect-to-the-database)
  - [Query API](#query-api)


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