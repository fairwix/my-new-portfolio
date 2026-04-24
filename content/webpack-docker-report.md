---
title: "Отчёт: Webpack + Luxon + Docker"
date: 2026-04-24
draft: false
tags: ["webpack", "docker", "luxon", "bootstrap"]
---

## 1. Установка Node.js и создание проекта

Установила Node.js версии 24 через Homebrew, затем создала папку `luxon-webapp` и инициализировала npm-проект:

```bash
mkdir luxon-webapp && cd luxon-webapp
npm init -y
```

Установила необходимые зависимости:

```
npm install luxon
npm install -D webpack webpack-cli serve
```

В файле package.json изменила "type" на "module", чтобы webpack корректно обрабатывал ES-импорты.

Точка входа src/index.js:
```
js
import { DateTime } from 'luxon';

setInterval(() => {
  hh.textContent = DateTime
    .local()
    .setLocale('ru')
    .toFormat('dd.LL.y HH:mm:ss');
}, 1000);
```

Страница index.html с Bootstrap CDN:
```
html
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <title>Luxon + Webpack</title>
  <style> body { font-size: 5rem; } </style>
</head>
<body class="container text-center mt-5">
  <h1 id="hh" class="display-1"></h1>
  <script src="./dist/main.js"></script>
</body>
</html>
```
2. Сборка Webpack

Выполнила сборку:

```
npx webpack
```

Сборка прошла успешно, создан бандл dist/main.js.

![Описание](/images/webpack-build.png)

3. Локальный запуск

Запустила локальный сервер:

```
npx serve .
```
Страница открылась по адресу http://localhost:3000 с крупным отображением даты и времени, стилизованным Bootstrap.

![Локальная страница с часами](/images/local-page.png)

4. Docker-контейнер

Создала Dockerfile на основе образа node:24-alpine:
```
dockerfile
FROM node:24-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npx webpack
EXPOSE 3000
CMD ["npx", "serve", ".", "-l", "3000"]
```

Собрала образ и запустила контейнер:

```
docker build -t luxon-webpack .
docker run -p 3000:3000 luxon-webpack
```
Приложение в контейнере работает идентично локальному запуску.

![Запуск приложения в Docker](/images/docker-run.png)

