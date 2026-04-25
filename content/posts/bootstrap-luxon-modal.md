---
title: "Bootstrap 5 + Luxon Modal — Этап 2"
date: 2026-04-25
draft: false
tags: ["bootstrap", "luxon", "docker", "modal"]
---

## Задание

Интеграция Bootstrap 5 в приложение с Luxon. Создать страницу с тремя колонками (2-8-2), красной кнопкой «Показать время» и модальным окном, отображающим дату и время через Luxon.

## Код приложения
```
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap + Luxon Modal</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #ffffff;
        }
        .btn-show-time {
            width: 100%;
            font-size: 2rem;
            font-weight: bold;
            padding: 1rem;
        }
    </style>
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

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/luxon@3.6.1/build/global/luxon.min.js"></script>
<script>
    const modalTime = document.getElementById('modal-time');
    const timeModal = document.getElementById('timeModal');
    let timerId;

    timeModal.addEventListener('show.bs.modal', function () {
        const updateTime = () => {
            modalTime.textContent = luxon.DateTime
                .local()
                .setLocale('ru')
                .toFormat('dd.LL.y HH:mm:ss');
        };
        updateTime();
        timerId = setInterval(updateTime, 1000);
    });

    timeModal.addEventListener('hidden.bs.modal', function () {
        clearInterval(timerId);
    });
</script>
</body>
</html>
```

## Внешний вид с открытым окном

![Приложение с модальным окном](https://raw.githubusercontent.com/fairwix/my-new-portfolio/main/static/images/luxon.png)

## Последовательность выполненных действий

1. **Создан HTML-файл** с сеткой Bootstrap 5: три колонки в соотношении 2-8-2, боковые колонки пустые, центральная содержит кнопку.
2. **Добавлена красная кнопка** `btn btn-danger` с текстом «Показать время», растянута на всю ширину центральной колонки.
3. **Настроено модальное окно** Bootstrap с тремя секциями:
   - **Заголовок** — имя и фамилия (Юлия Смирнова) и крестик `btn-close` для закрытия.
   - **Тело** — дата и время через Luxon в русской локали (`dd.LL.y HH:mm:ss`), обновляются каждую секунду.
   - **Футер** — кнопка «Закрыть» в правом нижнем углу.
4. **Подключены CDN**: Bootstrap 5 (CSS + JS), Luxon.
5. **Написан JavaScript**:
   - При открытии окна (`show.bs.modal`) запускается интервал — часы обновляются каждую секунду.
   - При закрытии (`hidden.bs.modal`) интервал очищается — ресурсы не расходуются.
6. **Закрытие окна** реализовано двумя способами: крестик в заголовке и кнопка «Закрыть» в футере.
7. **Приложение запущено в Docker**:
   - Базовый образ: `nginx:alpine`
   - Команда сборки: `docker build -t bootstrap-luxon-modal .`
   - Команда запуска: `docker run -p 8080:80 bootstrap-luxon-modal`
8. **Сделаны скриншоты** кода и работающего приложения, размещены на портфолио.

## Dockerfile

```dockerfile
FROM nginx:alpine
COPY bootstrap-luxon-modal.html /usr/share/nginx/html/index.html
EXPOSE 80
```
Команды Docker
```
docker build -t bootstrap-luxon-modal .
docker run -p 8080:80 bootstrap-luxon-modal
CMD ["nginx", "-g", "daemon off;"]
```
