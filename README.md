## Практическая работа 1.1: Развёртывание сайта на Hugo

### Что сделано:
1. Установлен и настроен генератор статических сайтов Hugo версии 0.157.0 на macOS через Homebrew
2. Создан новый проект Hugo: `hugo new site my-new-portfolio`
3. Добавлена тема оформления Ananke через git submodule для возможности дальнейшего обновления
4. Созданы страницы контента:
   - Главная страница с приветствием
   - Страница "Обо мне" с информацией
   - Страница "Контакты" с контактными данными
5. Настроена конфигурация сайта (hugo.toml) с правильным baseURL для GitHub Pages
6. Исходный код размещен в репозитории GitHub
7. Настроен автоматический деплой на GitHub Pages с помощью GitHub Actions
8. Проверена работоспособность сайта локально и на удаленном хостинге

### Ссылки:
- Репозиторий с исходниками: https://github.com/fairwix/my-new-portfolio
- Развёрнутый сайт: https://fairwix.github.io/my-new-portfolio/

### GitHub Actions скрипт (`.github/workflows/hugo.yml`):
```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive  # Важно для загрузки темы
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: '0.157.0'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
        ```
        Описание скрипта:

Скрипт автоматически собирает и публикует сайт при каждом пуше в ветку main:

Checkout - клонирует репозиторий вместе с submodule (темой)
Setup Hugo - устанавливает Hugo extended версии для поддержки SASS/SCSS
Build - собирает статические файлы в папку public
Upload artifact - загружает собранные файлы как артефакт
Deploy - публикует артефакт на GitHub Pages
