## Практическая работа 1.1: Развёртывание сайта на Hugo

### ✅ Выполнено:
1. Установлен Hugo 0.157.0 через Homebrew на macOS
2. Создан проект: `hugo new site my-new-portfolio`
3. Добавлена тема Ananke через git submodule
4. Настроены конфиги в config/_default/
5. Созданы страницы контента:
   - /about — Обо мне
   - /contact — Контакты
   - / — Главная страница
6. Протестирована локальная сборка: hugo server
7. Исходники размещены в репозитории: fairwix/my-new-portfolio
8. Настроен автоматический деплой через GitHub Actions

### 🔗 Ссылки:
- Репозиторий с исходниками: https://github.com/fairwix/my-new-portfolio
- Развёрнутый сайт: https://fairwix.github.io/my-new-portfolio/

### 📄 GitHub Actions скрипт:
Файл .github/workflows/hugo.yml собирает сайт с подмодулями и публикует 
результат в GitHub Pages. Ключевая настройка: submodules: recursive.
