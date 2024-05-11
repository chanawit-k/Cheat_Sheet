# How to Using Docker Compose and Dockerfile [Ref](https://github.com/chanawit-k/recipes-app-api)

## 1. Dockerfile
ไฟล์ที่มีคำสั่งในการสร้างภาพ (image) สำหรับคอนเทนเนอร์ Docker กำหนดรายละเอียดต่าง ๆ ได้ เช่น ระบบปฏิบัติการพื้นฐาน แพคเกจที่จำเป็น และคำสั่งในการรันโปรแกรม
```bash
FROM python:3.9-alpine3.13 # ระบุผู้ดูแลรักษา
# https://hub.docker.com/_/python 
LABEL maintainer="boomhulahoop.com"

ENV PYTHONUNBUFFERED 1

COPY ./requirements.txt /tmp/requirements.txt
COPY ./requirements.dev.txt /tmp/requirements.dev.txt
# คัดลอกไฟล์ requirements ไปยังไดเรกทอรี่ชั่วคราว /tmp/
COPY ./app /app # คัดลอกไดเรกทอรี่ app ไปยัง /app ใน Container
WORKDIR /app # กำหนดไดเรกทอรี่ทำงานเป็น /app
EXPOSE 8000 # เปิด Port 8000 ของ Container

ARG DEV=false  # ตั้งค่า argument DEV เริ่มต้นเป็น false
RUN python -m venv /py && \
    /py/bin/pip install --upgrade pip && \
    apk add --update --no-cache postgresql-client && \
    apk add --update --no-cache --virtual .tmp-build-deps \
        build-base postgresql-dev musl-dev && \
    /py/bin/pip install -r /tmp/requirements.txt && \
    if [ $DEV = "true" ]; \
        then /py/bin/pip install -r /tmp/requirements.dev.txt ; \
    fi && \
    rm -rf /tmp && \
    apk del .tmp-build-deps && \
    adduser \
        --disabled-password \
        --no-create-home \
        django-user
    
# สร้าง Virtual Environment Python (/py)
# Upgrade pip
# ติดตั้งไคลเอนต์ PostgreSQL
# ติดตั้ง Build Dependencies
# Install packages จากไฟล์ requirements.txt
# ถ้า DEV=true ก็ Install packages เพิ่มจากไฟล์ requirements.dev.txt
# ลบไดเรกทอรี่ /tmp ออก
# ลบไฟล์ชั่วคราวและ Build Dependencies ออก
# สร้างผู้ใช้ django-user โดยไม่ต้องรหัสผ่านและไม่สร้างโฮมไดเรกทอรี่ (เป็นมาตรฐานการปฏิบัติเพื่อความปลอดภัย)

ENV PATH="/py/bin:$PATH"

USER django-user
```
## 2. Docker-compose
docker-compose เป็นเครื่องมือที่ช่วยกำหนดและจัดการ multi-container applications บนระบบ Docker โดยจะอ้างอิงไปยัง Dockerfile เพื่อสร้าง Docker images สำหรับแต่ละ container ตามที่กำหนดใน docker-compose.yml
```bash 
version: "3.9" # ระบุเวอร์ชันของ Docker Compose

services:
  app:
    build:
      context: . # Build context คือไดเรกทอรี่ปัจจุบัน
      args:
        - DEV=true # ส่งค่า argument DEV=true ไปให้ Dockerfile ขณะสร้าง image
    ports:
      - "8000:8000"
      # Map port 8000 ของ container ไปยัง port 8000 ของโฮสต์ เพื่อให้สามารถเข้าถึงได้จากภายนอก
    volumes:
      - ./app:/app
      # Mount ไดเรกทอรี่ app จากโฮสต์ไปยัง /app ใน container เพื่อสามารถแก้ไขโค้ดโดยไม่ต้องสร้างใหม่
    command: >
       sh -c "python manage.py wait_for_db &&
             python manage.py migrate &&
             python manage.py runserver 0.0.0.0:8000"
    environment:
      - DB_HOST=db
      - DB_NAME=devdb
      - DB_USER=devuser
      - DB_PASS=changeme
      # ใช้ Config DB ใน setting.py 
    depends_on:
      - db

  db:
    image: postgres:13-alpine
    volumes:
      - dev-db-data:/var/lib/postgresql/data # path ที่อยู่ DB
    environment:
      - POSTGRES_DB=devdb
      - POSTGRES_USER=devuser
      - POSTGRES_PASSWORD=changeme

volumes:
  dev-db-data:
```
## 3. คำสั่งที่ใช้
```bash
docker-compose build
```
- หน้าที่หลัก คือ การสร้างหรือรีบิลด์อิมเมจ Docker สำหรับเซอร์วิสต่างๆ ที่กำหนดใน docker-compose.yml
- ไม่ได้รันคอนเทนเนอร์ คำสั่งนี้เพียงแค่สร้างอิมเมจเท่านั้น

```bash 
docker-compose up
``` 
- สร้าง Docker image และรัน container ตามที่กำหนดใน docker-compose.yml
- หลังจากนั้นจะเริ่มรัน Django development server

```bash 
docker-compose run --rm app sh -c "<your-command>" 
``` 
- รัน one-time command ใน container แล้วหยุดทันที เช่น:
- docker-compose run --rm app sh -c "python manage.py migrate"
- docker-compose run --rm app sh -c "python manage.py collectstatic"

```bash 
docker-compose stop 
``` 
- หยุดการรัน containers

```bash
docker-compose down
```
- หยุดและลบ containers

