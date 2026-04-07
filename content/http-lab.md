# Отчёт по лабораторной работе: Протокол HTTP
## Цель работы
Изучить основы протокола HTTP, освоить отправку GET и POST запросов различными инструментами (netcat, cURL, Postman).
## Используемые инструменты
| Инструмент | Назначение |
|------------|------------|
| **netcat (nc)** | Ручное формирование HTTP-запросов в CLI |
| **cURL** | Автоматизированные запросы в CLI |
| **Postman** | Работа с API в графическом интерфейсе |
---
## 1. GET запрос через netcat
**Команда:**

nc [httpbin.org](https://httpbin.org/) 80  
GET /get HTTP/1.1  
Host: [httpbin.org](https://httpbin.org/)

### ответ сервера: 

[![[Pasted image 20260407161751.png|508]]](https:///images/http-lab/get-nc.png)

---

## 2. POST запрос через netcat

**Команда:**

```
nc httpbin.org 80
POST /post HTTP/1.1
Host: httpbin.org
Content-Type: application/json
Content-Length: 27
{"name": "John", "age": 30}
```


**Ответ сервера:**
[![[Pasted image 20260407161826.png|426]]](https:///images/http-lab/post-nc.png)

---

## 3. GET запрос через cURL

**Команда:**

bash

curl http://httpbin.org/get

**Ответ сервера:**
[![[Pasted image 20260407161857.png|465]]](https:///images/http-lab/curl-get.png)

---
## 4. POST запрос через cURL

**Команда:**

bash

curl -X POST -H "Content-Type: application/json" -d '{"name":"John","age":30}' http://httpbin.org/post

**Ответ сервера:**
[![[Pasted image 20260407161955.png|551]]](https:///images/http-lab/curl-post.png)

---
## 5. Запрос к API Банка России через Postman

### Выбранная валюта

**Доллар США** (код валюты: `R01235`)

### Период

01.04.2026 – 07.04.2026

### URL запроса
https://www.cbr.ru/scripts/XML_dynamic.asp?date_req1=01/04/2026&date_req2=07/04/2026&VAL_NM_RQ=R01235

### Параметры запроса

|Параметр|Значение|Описание|
|---|---|---|
|`date_req1`|01/04/2026|Начальная дата|
|`date_req2`|07/04/2026|Конечная дата|
|`VAL_NM_RQ`|R01235|Код валюты (доллар США)|

### Результат в Postman
[![[Pasted image 20260407162435.png]]](https:///images/http-lab/postman-cbr.png)
### Ответ сервера (XML, фрагмент)

xml

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

### Анализ полученных данных

|Дата|Курс USD (руб.)|
|---|---|
|01.04.2026|81,2504|
|02.04.2026|80,6234|
|03.04.2026|80,5000|

**Наблюдение:** Курс доллара за указанный период снизился с 81,25 до 80,50 рублей.
