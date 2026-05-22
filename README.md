# Отчет по лабораторной работе №4.1: Создание веб-сайта с использованием GitHub Pages и GitHub Actions

## Цель работы
Изучение процесса публикации статического веб-сайта на хостинге GitHub Pages с использованием автоматизированного пайплайна (CI/CD) на базе GitHub Actions.

---

## Ссылка на работающий веб-сайт
**[(https://andiosknight.github.io/)](https://andiosknight.github.io/)**

---

## Ход выполнения работы

### Шаг 1. Создание персонального репозитория

1. На GitHub создан новый публичный репозиторий.
2. Имя репозитория строго соответствует формату: `<имя_пользователя>.github.io` для автоматической привязки к домену.

### Шаг 2. Настройка источника деплоя в Settings

1. В настройках (вкладка **Settings**) созданного репозитория выбран раздел **Pages**.
2. Под заголовком **Build and deployment** в выпадающем списке **Source** вместо стандартного *Deploy from a branch* был выбран **GitHub Actions**.
3. Создание собственного скрипта**create your own**.

### Шаг 3. Конфигурация скрипта деплоя (`deploy.yml`)

   Был создлан файл конфигурации `deploy.yml` с готовым скриптом автоматического развертывания:
   ```yaml
   name: Deploy static content to Pages 

   on: 
     push: 
       branches: ['main'] 
     workflow_dispatch: 

   permissions: 
     contents: read 
     pages: write 
     id-token: write 

   concurrency: 
     group: 'pages' 
     cancel-in-progress: false 

   jobs: 
     deploy: 
       environment: 
         name: github-pages 
         url: ${{ steps.deployment.outputs.page_url }} 
       runs-on: ubuntu-latest 
       steps: 
         - name: Checkout 
           uses: actions/checkout@v4 
         - name: Setup Pages 
           uses: actions/configure-pages@v5 
         - name: Upload artifact 
           uses: actions/upload-pages-artifact@v3 
           with: 
             path: '.' 
         - name: Deploy to GitHub Pages 
           id: deployment 
           uses: actions/deploy-pages@v4 
   ```
   Выполнен коммит изменений в ветку `main`.
### Шаг 4. Создание файла статического сайта `index.html`

   В корне репозитория **Add file** -> **Create new file**.
   Создан файл `index.html` с предоставленным кодом разметки страницы:
   ```html
   <!DOCTYPE html> 
   <html lang="en"> 
   <head> 
       <meta charset="UTF-8"> 
       <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
       <title>GitHub Deployment Workflow</title> 
   </head> 
   <body> 
       <p>Hello, GitHub Actions!</p> 
   </body> 
   </html>
   ```
  Выполнен коммит изменений в репозиторий.
### Шаг 5. Мониторинг Pipeline и проверка работы сайта

   Вкладка **Actions**
   Рабочий процесс `Deploy static content to Pages` запустился автоматически после пуша и отработал корректно
   <img width="1331" height="490" alt="Снимок экрана 2026-05-22 183728" src="https://github.com/user-attachments/assets/be648674-e05b-4326-a334-3f3ff271c42c" />
   <img width="724" height="370" alt="Снимок экрана 2026-05-22 183831" src="https://github.com/user-attachments/assets/035417d8-8144-4923-ab18-4c4e35e6b530" />


   Открыт веб-браузер, осуществлен переход по адресу [](https://andiosknight.github.io/)`. На странице успешно отобразился текст `Hello, GitHub Actions!`.
   <img width="2560" height="1392" alt="изображение" src="https://github.com/user-attachments/assets/c1f97b1d-5dfb-456b-a7d8-54daf7dc742b" />


---

## Вывод
В ходе лабораторной работы были изучены продвинутые инструменты автоматизации GitHub Actions. Настроен полноценный CI/CD процесс: при любом изменении кода в ветке `main` триггер автоматически запускает пайплайн, который собирает артефакты проекта и разворачивает их на хостинге GitHub Pages. Получен рабочий персональный сайт на уникальном домене.
