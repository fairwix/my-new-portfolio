---
title: "Bootstrap 5 + Luxon Vite — Этап 3"
date: 2026-05-08
draft: false
tags: ["bootstrap", "luxon", "vite", "сборка"]
---

## Задание

Интеграция Bootstrap 5 в приложение с Luxon с использованием сборщика Vite. Сборка бандла на хосте, минимизация размера, отказ от CDN.

## Используемые технологии

- **Vite** — сборщик проекта
- **Bootstrap 5** — npm-пакет (SCSS + JS)
- **Luxon** — npm-пакет (дата и время)
- **Sass** — препроцессор для стилей

## Код приложения

### index.html

```html
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Bootstrap + Luxon Vite Stage 3</title>
</head>
<body>
  <div class="container-fluid mt-4">
    <div class="row">
      <div class="col-2"></div>
      <div class="col-8 d-flex align-items-center justify-content-center">
        <button type="button" class="btn btn-danger btn-show-time" data-bs-toggle="modal" data-bs-target="#timeModal">
          Показать время
        </button>
      </div>
      <div class="col-2"></div>
    </div>
  </div>

  <div class="modal fade" id="timeModal" tabindex="-1" aria-labelledby="timeModalLabel" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="timeModalLabel">Юлия Смирнова</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Закрыть"></button>
        </div>
        <div class="modal-body text-center">
          <h1 id="modal-time" class="display-4"></h1>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Закрыть</button>
        </div>
      </div>
    </div>
  </div>

  <script type="module" src="/src/main.js"></script>
</body>
</html>
```

### src/style.scss
```
@use "bootstrap/scss/bootstrap" with (
  $enable-grid-classes: true,
  $enable-cssgrid: false
);

body {
  background-color: #ffffff;
}

.btn-show-time {
  width: 100%;
  font-size: 2rem;
  font-weight: bold;
  padding: 1rem;
}
```

### src/main.js

```
import './style.scss';
import 'bootstrap/js/dist/modal';
import { DateTime } from 'luxon';

const modalTime = document.getElementById('modal-time');
const timeModalElement = document.getElementById('timeModal');
let timerId;

if (timeModalElement) {
  timeModalElement.addEventListener('show.bs.modal', function () {
    const updateTime = () => {
      modalTime.textContent = DateTime
        .local()
        .setLocale('ru')
        .toFormat('dd.LL.y HH:mm:ss');
    };
    updateTime();
    timerId = setInterval(updateTime, 1000);
  });

  timeModalElement.addEventListener('hidden.bs.modal', function () {
    clearInterval(timerId);
  });
}
```
#### Внешний вид с открытым окном

![Внешний вид с открытым окном](https://raw.githubusercontent.com/fairwix/my-new-portfolio/main/static/images/bootstrap-luxon-vite-stage3.png)

#### Размер бандла

После сборки npm run build получены следующие размеры:

```
index.html	  1.51 kB	   0.67 kB
CSS	        225.00 kB	  30.59 kB
JS	         93.40 kB	  29.26 kB

```
Команды для сборки (из package.json)
```
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "preview": "vite preview"
}
```

## Последовательность выполненных действий

- Создан проект на Vite: 
```
npm create vite@latest luxon-vite-stage3 -- --template vanilla
```
- Установлены зависимости:
  
Bootstrap, Popper.js, Luxon, Sass

- Файл стилей переименован в .scss, настроен импорт Bootstrap через @use

- Создан index.html с сеткой 2-8-2, красной кнопкой и модальным окном

- В main.js импортированы только нужные компоненты:
```
bootstrap/js/dist/modal (tree-shaking)
```
- Написан скрипт обновления времени через Luxon с русской локалью

- Сборка выполнена командой 
```
npm run build
```

- Измерены размеры бандла

