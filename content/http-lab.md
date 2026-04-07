---
title: "Лабораторная работа: Протокол HTTP"
date: 2026-04-07
draft: false
description: "Изучение HTTP протокола, отправка GET и POST запросов"
tags: ["HTTP", "API", "netcat", "curl", "Postman"]
menu: "main"
weight: 4
---

## Цель работы
Изучить основы протокола HTTP, освоить отправку GET и POST запросов различными инструментами (netcat, cURL, Postman).

## Используемые инструменты

| Инструмент | Назначение |
|------------|------------|
| **netcat (nc)** | Ручное формирование HTTP-запросов в CLI |
| **cURL** | Автоматизированные запросы в CLI |
| **Postman** | Работа с API в графическом интерфейсе |

---
![GET запрос через netcat](/images/http-lab/get-nc.png)

![POST запрос через netcat](/images/http-lab/post-nc.png)

![GET через cURL](/images/http-lab/curl-get.png)

![POST через cURL](/images/http-lab/curl-post.png)

![Postman запрос](/images/http-lab/postman-cbr.png)
<img src="/images/http-lab/get-nc.png" alt="GET netcat">
<img src="/images/http-lab/post-nc.png" alt="POST netcat">
<img src="/images/http-lab/curl-get.png" alt="curl GET">
<img src="/images/http-lab/curl-post.png" alt="curl POST">
<img src="/images/http-lab/postman-cbr.png" alt="Postman">

## 1. GET запрос через netcat

**Команда:**
```
nc httpbin.org 80
GET /get HTTP/1.1
Host: httpbin.org
```

**Ответ сервера:**
```json
{
  "args": {},
  "headers": {
    "Host": "httpbin.org",
    "X-Amzn-Trace-Id": "Root=1-69d5029c-18539c4f3cf721d2406e797c"
  },
  "origin": "77.234.216.36",
  "url": "http://httpbin.org/get"
}
```
---
2. POST запрос через netcat

Команда:
```
nc httpbin.org 80
POST /post HTTP/1.1
Host: httpbin.org
Content-Type: application/json
Content-Length: 27

{"name": "John", "age": 30}
```
Ответ сервера:

```json
{
  "args": {},
  "data": "{\"name\": \"John\", \"age\": 30}",
  "files": {},
  "form": {},
  "headers": {
    "Content-Length": "27",
    "Content-Type": "application/json",
    "Host": "httpbin.org",
    "X-Amzn-Trace-Id": "Root=1-69d502de-03e9bc2e2183f9d531086f5f"
  },
  "json": {
    "age": 30,
    "name": "John"
  },
  "origin": "77.234.216.36",
  "url": "http://httpbin.org/post"
}
```
![POST запрос через netcat](/images/http-lab/post-nc.png)
![POST запрос через netcat](https://i.postimg.cc/r0CQXWk3/post-nc.png)

---

3. GET запрос через cURL

Команда:

```
curl http://httpbin.org/get
```
Ответ сервера:

```json
{
  "args": {},
  "headers": {
    "Accept": "*/*",
    "Host": "httpbin.org",
    "User-Agent": "curl/8.7.1",
    "X-Amzn-Trace-Id": "Root=1-69d50433-54f3724d5e7e0109325133eb"
  },
  "origin": "77.234.216.36",
  "url": "http://httpbin.org/get"
}
```
![GET через cURL](/images/http-lab/curl-get.png)

---

4. POST запрос через cURL

Команда:

```
curl -X POST -H "Content-Type: application/json" -d '{"name":"John","age":30}' http://httpbin.org/post
```
Ответ сервера:

```json
{
  "args": {},
  "data": "{\"name\":\"John\",\"age\":30}",
  "files": {},
  "form": {},
  "headers": {
    "Accept": "*/*",
    "Content-Length": "24",
    "Content-Type": "application/json",
    "Host": "httpbin.org",
    "User-Agent": "curl/8.7.1",
    "X-Amzn-Trace-Id": "Root=1-69d50448-49352361333d21674c7b5786"
  },
  "json": {
    "age": 30,
    "name": "John"
  },
  "origin": "77.234.216.36",
  "url": "http://httpbin.org/post"
}
```
![POST через cURL](/images/http-lab/curl-post.png)

---

5. Запрос к API Банка России через Postman

Выбранная валюта

Доллар США (код валюты: R01235)

Период

01.04.2026 – 07.04.2026

URL запроса
```
https://www.cbr.ru/scripts/XML_dynamic.asp?date_req1=01/04/2026&date_req2=07/04/2026&VAL_NM_RQ=R01235
```
Результат в Postman

![Postman запрос](/images/http-lab/postman-cbr.png)

Ответ сервера (XML, фрагмент)

```xml
<?xml version="1.0" encoding="windows-1251"?>
<ValCurs ID="R01235" DateTimeFormat="dd.MM.yyyy">
  <Record Date="01.04.2026" Id="R01235">
    <Nominal>1</Nominal>
    <Value>81,2504</Value>
    <VunitRate>81,2504</VunitRate>
  </Record>
  <Record Date="02.04.2026" Id="R01235">
    <Nominal>1</Nominal>
    <Value>80,6234</Value>
    <VunitRate>80,6234</VunitRate>
  </Record>
  <Record Date="03.04.2026" Id="R01235">
    <Nominal>1</Nominal>
    <Value>80,5000</Value>
    <VunitRate>80,5000</VunitRate>
  </Record>
</ValCurs>
```



