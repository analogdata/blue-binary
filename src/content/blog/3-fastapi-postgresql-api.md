---
title: Building a RESTful API with FastAPI and PostgreSQL
author: Rajath Kumar K S
pubDatetime: 2024-12-04T13:00:00.000+00:00
slug: fastapi-postgresql-api
featured: true
draft: false
tags:
  - Python
  - FastAPI
  - PostgreSQL
  - Database
description:
  "Learn how to build a complete RESTful API using FastAPI and PostgreSQL. This guide covers database setup, models, and CRUD operations."
---

FastAPI makes it incredibly easy to connect to a PostgreSQL database using ORMs like SQLAlchemy or Tortoise ORM. In this blog, you’ll learn how to:

- Set up a PostgreSQL database.
- Integrate it with FastAPI using an ORM.
- Create database models and schema.
- Implement full CRUD (Create, Read, Update, Delete) functionality.

## Code Example: Connecting to PostgreSQL

```python
from fastapi import FastAPI
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "postgresql://user:password@localhost/db_name"
engine = create_engine(DATABASE_URL)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Connected to PostgreSQL!"}
```
