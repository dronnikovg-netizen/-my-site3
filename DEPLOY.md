# Деплой PWA на GitHub Pages

## Что вы получите

- HTTPS-адрес вида `https://username.github.io/daily-planner/`
- Возможность установить приложение на главный экран iPhone (Safari → Поделиться → На экран «Домой»)
- Оффлайн-работу через Service Worker
- Бесплатно и без ограничений на трафик в практических объёмах

## Шаги (≈5 минут)

### 1. Создайте репозиторий

1. Зайдите на https://github.com → New repository
2. Имя: `daily-planner` (или любое другое)
3. Visibility: **Public** (приватные репозитории требуют платный тариф для Pages)
4. **Не** инициализируйте README/.gitignore — у нас уже есть файлы
5. Create repository

### 2. Запушьте файлы

В терминале из папки `Асана/`:

```sh
git init
git add index.html manifest.webmanifest sw.js icon.svg DEPLOY.md
git branch -M main
git remote add origin https://github.com/USERNAME/daily-planner.git
git commit -m "init: daily planner PWA"
git push -u origin main
```

Замените `USERNAME` на свой логин GitHub.

### 3. Включите Pages

1. В репозитории: **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: **main**, folder: **/ (root)**
4. Save

Через 1–2 минуты появится URL вроде `https://USERNAME.github.io/daily-planner/`.

### 4. Установите на iPhone

1. Откройте URL в **Safari** (не Chrome — на iOS только Safari умеет ставить PWA)
2. Тапните «Поделиться» (квадрат со стрелкой) → **«На экран «Домой»»**
3. Подтвердите имя «Ежедневник»
4. Иконка появится на рабочем столе. Запуск открывает приложение в standalone-режиме без адресной строки.

### 5. Обновления

После любого изменения файлов:

```sh
git add -A
git commit -m "update"
git push
```

GitHub Pages обновит сайт через 30–60 секунд. На уже установленном приложении кэш Service Worker'а обновится при следующем открытии (новая версия активируется на следующем запуске).

## Синхронизация между устройствами

В мобильной версии есть таб **«Ещё → Поделиться (iCloud Drive)»**. Это вызывает iOS Share Sheet с JSON-файлом — выберите «Сохранить в Файлы → iCloud Drive».

На другом устройстве: **«Ещё → Импорт из файла»** → выберите файл из iCloud Drive.

Это manual sync, не автоматический. Привычка: делать share раз в неделю на основном устройстве.

## Проверка PWA

Откройте сайт в Chrome desktop → DevTools → Lighthouse → «Progressive Web App». Должны пройти основные пункты: manifest, service worker, HTTPS, viewport, theme color.

## Troubleshooting

**«Service worker registration failed»** — открыли через `file://`. SW работает только по HTTPS или с `localhost`.

**Иконка на iPhone выглядит мутно** — браузер сделал скриншот вместо использования icon.svg. Решение: сгенерировать PNG 180×180 (например, на realfavicongenerator.net) и положить рядом как `apple-touch-icon.png` (Safari подхватит автоматически по соглашению).

**Установленное приложение не обновляется** — закройте его (через App Switcher) и откройте снова. SW проверит обновления при запуске.
