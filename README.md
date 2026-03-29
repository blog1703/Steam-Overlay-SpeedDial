**Language:** [Русский](README.md) | [English](README_EN.md)

# Steam-Overlay-SpeedDial
Кастомная стартовая страница для браузера Steam Overlay | Custom start page for Steam Overlay browser  Плитки сайтов, радио с глобальным управлением, поиск Google, погода, Telegram, Discord, YouTube Music, Yandex Music, VK. Экспорт/импорт закладок, RU/EN

## 🛠️ Установка

### 1️⃣ Сохраните файл

Скачайте или скопируйте файл `dashboard.html` и сохраните его в удобном месте.  
_Пример:_ создайте папку `SteamOverlayDashboard` в «Моих документах»: C:\Users\ИМЯ_ПОЛЬЗОВАТЕЛЯ\Documents\SteamOverlayDashboard\dashboard.html

### 2️⃣ Скопируйте путь к файлу

Нажмите правой кнопкой мыши на `dashboard.html` → выберите **«Копировать как путь»**.  
У вас получится строка вроде: C:\Users\PC\Documents\SteamOverlayDashboard\dashboard.html

### 3️⃣ Закройте Steam

> ⚠️ **Важно:** Steam должен быть **полностью закрыт**, иначе изменения в файле настроек не сохранятся.

### 4️⃣ Откройте файл настроек Steam

Перейдите по пути: C:\Program Files (x86)\Steam\userdata\XXXXXXXX\config\localconfig.vdf

- `XXXXXXXX` — ваша папка с цифрами (Steam ID)
- Откройте файл **Блокнотом** или **Notepad++**

### 5️⃣ Добавьте строку с домашней страницей

Найдите: "GameOverlayHomePage" строка кода где нужно сделать замену `"https://www.google.com/"` вставьте `"file:/C:/Users/ИМЯ_ПОЛЬЗОВАТЕЛЯ/Documents/SteamOverlayDashboard/dashboard.html"`

![строчка кода где нужно сделать замену](https://raw.githubusercontent.com/blog1703/Steam-Overlay-Dashboard/refs/heads/main/images/screenshot.jpg)

*строка кода где нужно сделать замену*

🔑 **Главный секрет:**  
В начале пути используйте **один слеш** — `file:/`, а не три `(file:///)`).  
Steam Overlay блокирует `file:///`, но **не блокирует** `file:/`.

### 6️⃣ Сохраните файл и закройте редактор

### 7️⃣ Запустите Steam и проверьте

- Запустите любую игру с поддержкой оверлея (Shift+Tab)
- Откройте браузер в оверлее — вы увидите свою новую домашнюю страницу
- Если страница не открылась автоматически, нажмите на иконку домика 🏠

### Вот так будет выглядеть Домашняя страница

![Домашняя страница в браузере стим](https://raw.githubusercontent.com/blog1703/Steam-Overlay-SpeedDial/refs/heads/main/images/view_speedial.gif)

*Домашняя страница в браузере стим*

---

## 🎨 Кастомизация

Файл `dashboard.html` можно редактировать как угодно:

- ➕ Добавлять свои сайты в плитки
- 🎵 Менять радиостанции
- 🎨 Изменять цвета, шрифты, анимации
- 🌐 Добавлять новые иконки сервисов

Всё находится внутри одного файла — изменять его очень просто, можно с помощью Chat GPT, DeepSeek.

## ➕ Добавление новых иконок быстрых ссылок

В панели иконок (Telegram, Discord, YouTube Music, Yandex Music, VK) можно легко добавить любой сервис.

### Шаблон для добавления

```html
<a href="https://link-to-service" target="_blank" class="pure-icon" title="Name">
    <img src="https://img.icons8.com/?size=100&id=ID_ICONS&format=png&color=000000" alt="Name">
</a>
```

---
## 📄 Полный код проекта
<details>
  <summary>📄 Нажмите, чтобы показать код dashboard.html</summary>
  
  
  ```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Steam Overlay Dashboard</title>
    <!-- Elfsight Weather Widget -->
    <script src="https://apps.elfsight.com/p/platform.js" defer></script>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1e1f2c 0%, #13141f 100%);
            min-height: 100vh;
            padding: 30px;
            color: #e0e0e0;
        }
        .container { 
            max-width: 1400px; 
            margin: 0 auto;
            display: flex;
            flex-direction: column;
            min-height: calc(100vh - 60px);
        }
        
        /* ========== ВЕРХНЯЯ ПАНЕЛЬ: поиск + погода ========== */
        .top-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 20px;
            margin-bottom: 15px;
            background: rgba(0,0,0,0.3);
            backdrop-filter: blur(10px);
            padding: 15px 25px;
            border-radius: 20px;
            border: 1px solid rgba(102, 192, 244, 0.2);
        }
        
        .left-area {
            flex: 2;
            min-width: 250px;
        }
        
        .search-form {
            display: flex;
            gap: 10px;
            align-items: center;
            background: rgba(0,0,0,0.5);
            border-radius: 50px;
            padding: 5px 15px;
            border: 1px solid rgba(102, 192, 244, 0.3);
            transition: all 0.3s;
            margin-bottom: 12px;
        }
        
        .search-form:focus-within {
            border-color: #66c0f4;
            box-shadow: 0 0 10px rgba(102, 192, 244, 0.3);
        }
        
        .search-icon {
            font-size: 18px;
            color: #66c0f4;
        }
        
        .search-input {
            flex: 1;
            background: transparent;
            border: none;
            padding: 12px 5px;
            color: white;
            font-size: 16px;
            outline: none;
        }
        
        .search-input::placeholder {
            color: rgba(255,255,255,0.5);
        }
        
        .search-btn {
            background: #66c0f4;
            border: none;
            color: #1a1a2e;
            padding: 8px 20px;
            border-radius: 50px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.2s;
        }
        
        .search-btn:hover {
            background: #2a6f8f;
            color: white;
            transform: scale(1.02);
        }
        
        .lang-switch {
            position: absolute;
            top: 20px;
            right: 30px;
            background: rgba(42, 71, 94, 0.6);
            border: 1px solid rgba(102, 192, 244, 0.3);
            border-radius: 30px;
            padding: 5px 12px;
            cursor: pointer;
            font-size: 12px;
            font-weight: 500;
            transition: all 0.2s;
            backdrop-filter: blur(4px);
            z-index: 100;
        }
        
        .lang-switch:hover {
            background: #66c0f4;
            color: #1a1a2e;
            border-color: #66c0f4;
        }
        
        .icon-row {
            display: flex;
            gap: 15px;
            align-items: center;
            margin-top: 5px;
            flex-wrap: wrap;
        }
        
        .pure-icon {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            width: 80px;
            height: 80px;
            text-decoration: none;
            transition: all 0.3s ease;
            background: none;
            border: none;
        }

        .pure-icon:hover {
            transform: scale(1.15);
            filter: drop-shadow(0 0 10px rgba(0, 136, 204, 0.5));
        }

        .pure-icon img {
            width: 100%;
            height: 100%;
            object-fit: contain;
            display: block;
        }
        
        .weather-widget {
            min-width: 280px;
            background: rgba(0,0,0,0.3);
            border-radius: 16px;
            overflow: hidden;
        }
        
        .tiles-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 170px));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .tile {
            background: rgba(42, 71, 94, 0.4);
            backdrop-filter: blur(10px);
            border-radius: 12px;
            padding: 20px 10px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 1px solid rgba(102, 192, 244, 0.2);
            position: relative;
        }
        
        .tile:hover {
            transform: translateY(-8px);
            background: rgba(102, 192, 244, 0.3);
            border-color: #66c0f4;
            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
        }
        
        .tile img {
            width: 56px;
            height: 56px;
            border-radius: 12px;
            margin-bottom: 12px;
        }
        
        .tile .title {
            font-size: 14px;
            font-weight: 500;
            margin-bottom: 5px;
        }
        
        .delete-tile {
            position: absolute;
            top: 8px;
            right: 10px;
            background: rgba(0,0,0,0.7);
            width: 22px;
            height: 22px;
            border-radius: 50%;
            font-size: 14px;
            line-height: 20px;
            text-align: center;
            cursor: pointer;
            opacity: 0;
            transition: opacity 0.2s;
            color: #ff8a8a;
        }
        
        .tile:hover .delete-tile {
            opacity: 1;
        }
        
        .add-form {
            background: rgba(0,0,0,0.4);
            border-radius: 12px;
            padding: 20px;
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
            margin-bottom: 30px;
        }
        
        .add-form input {
            flex: 1;
            min-width: 200px;
            padding: 12px;
            background: rgba(0,0,0,0.5);
            border: 1px solid rgba(255,255,255,0.2);
            border-radius: 8px;
            color: white;
            font-size: 14px;
        }
        
        .add-form input:focus {
            outline: none;
            border-color: #66c0f4;
        }
        
        .add-form button {
            padding: 12px 30px;
            background: #66c0f4;
            border: none;
            border-radius: 20px;
            color: #1a1a2e;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        
        .add-form button:hover {
            background: #2a6f8f;
            color: white;
            transform: scale(1.02);
            box-shadow: 0 0 10px rgba(102, 192, 244, 0.4);
        }
        
        .radio-section {
            margin-bottom: 20px;
        }
        
        .radio-top-row {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }
        
        .radio-player-box {
            flex: 3;
            background: rgba(0,0,0,0.3);
            border-radius: 16px;
            padding: 12px 20px;
            border: 1px solid rgba(102, 192, 244, 0.2);
            display: flex;
            align-items: center;
            gap: 15px;
            flex-wrap: wrap;
        }
        
        .current-station {
            font-size: 13px;
            color: #66c0f4;
            min-width: 180px;
        }
        
        #radio-player {
            flex: 2;
            min-width: 200px;
            height: 36px;
            border-radius: 20px;
        }
        
        #radio-player::-webkit-media-controls-panel {
            background-color: #2a475e;
        }
        
        .add-radio-btn {
            background: #2a475e;
            border: none;
            color: white;
            padding: 8px 20px;
            border-radius: 20px;
            cursor: pointer;
            font-size: 13px;
            font-weight: 500;
            transition: all 0.2s;
            white-space: nowrap;
        }
        
        .add-radio-btn:hover {
            background: #66c0f4;
            transform: scale(1.02);
        }
        
        .radio-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            margin-bottom: 15px;
        }
        
        .radio-card {
            background: rgba(42, 71, 94, 0.4);
            backdrop-filter: blur(10px);
            border-radius: 12px;
            padding: 12px 18px;
            display: flex;
            align-items: center;
            gap: 12px;
            border: 1px solid rgba(102, 192, 244, 0.2);
            transition: all 0.2s;
            cursor: pointer;
            position: relative;
        }
        
        .radio-card:hover {
            background: rgba(102, 192, 244, 0.2);
            border-color: #66c0f4;
        }
        
        .radio-card.active {
            background: rgba(102, 192, 244, 0.3);
            border-color: #66c0f4;
            box-shadow: 0 0 10px rgba(102, 192, 244, 0.3);
        }
        
        .radio-icon {
            width: 32px;
            height: 32px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .radio-icon img {
            width: 28px;
            height: 28px;
            object-fit: contain;
        }
        
        .radio-info {
            flex: 1;
        }
        
        .radio-name {
            font-size: 14px;
            font-weight: 500;
        }
        
        .radio-url {
            font-size: 10px;
            opacity: 0.6;
            max-width: 200px;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }
        
        .delete-radio {
            position: absolute;
            top: 8px;
            right: 10px;
            background: rgba(0,0,0,0.7);
            width: 22px;
            height: 22px;
            border-radius: 50%;
            font-size: 14px;
            line-height: 20px;
            text-align: center;
            cursor: pointer;
            opacity: 0;
            transition: opacity 0.2s;
            color: #ff8a8a;
        }
        
        .radio-card:hover .delete-radio {
            opacity: 1;
        }
        
        .radio-add-form {
            background: rgba(0,0,0,0.5);
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 15px;
            display: none;
            flex-wrap: wrap;
            gap: 10px;
            align-items: center;
        }
        
        .radio-add-form input {
            flex: 1;
            padding: 10px;
            background: rgba(0,0,0,0.5);
            border: 1px solid rgba(255,255,255,0.2);
            border-radius: 8px;
            color: white;
            font-size: 14px;
        }
        
        .radio-add-form button {
            padding: 10px 20px;
            background: #66c0f4;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
        }
        
        .confirm-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.7);
            backdrop-filter: blur(4px);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2000;
        }
        
        .confirm-dialog {
            background: #1e1f2c;
            border-radius: 20px;
            padding: 25px 30px;
            max-width: 350px;
            width: 90%;
            text-align: center;
            border: 1px solid #66c0f4;
            box-shadow: 0 0 30px rgba(102,192,244,0.3);
        }
        
        .confirm-dialog p {
            margin-bottom: 20px;
            font-size: 16px;
            color: #e0e0e0;
        }
        
        .confirm-dialog .station-name {
            color: #66c0f4;
            font-weight: bold;
        }
        
        .confirm-buttons {
            display: flex;
            gap: 15px;
            justify-content: center;
        }
        
        .confirm-btn {
            padding: 8px 25px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            font-weight: bold;
            transition: all 0.2s;
        }
        
        .confirm-yes {
            background: #8b3c3c;
            border: none;
            color: white;
        }
        
        .confirm-yes:hover {
            background: #ac4c4c;
            transform: scale(1.02);
        }
        
        .confirm-no {
            background: #2a475e;
            border: none;
            color: white;
        }
        
        .confirm-no:hover {
            background: #66c0f4;
            transform: scale(1.02);
        }
        
        .controls-footer {
            display: flex;
            justify-content: center;
            gap: 15px;
            flex-wrap: wrap;
            margin-top: 20px;
            margin-bottom: 10px;
        }
        
        .control-btn {
            background: #2a475e;
            border: none;
            color: white;
            padding: 8px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.2s;
        }
        
        .control-btn:hover {
            background: #66c0f4;
            transform: translateY(-2px);
        }
        
        .control-btn.danger {
            background: #8b3c3c;
        }
        
        .control-btn.danger:hover {
            background: #ac4c4c;
        }
        
        /* ========== ГЛОБАЛЬНАЯ ПЕРЕНОСИМАЯ КНОПКА РАДИО ========== */
        .radio-float-btn {
            position: fixed;
            z-index: 1000;
            cursor: grab;
            width: 80px;
            height: 80px;
            background: none;
            border: none;
            transition: all 0.1s ease;
            user-select: none;
        }
        
        .radio-float-btn:active {
            cursor: grabbing;
        }

        .float-icon {
            width: 100%;
            height: 100%;
            object-fit: contain;
            display: block;
            transition: all 0.2s ease;
            filter: drop-shadow(0 0 0px rgba(92, 124, 250, 0));
            pointer-events: none;
        }

        .radio-float-btn:hover .float-icon {
            transform: scale(1.05);
            filter: drop-shadow(0 0 12px rgba(92, 124, 250, 0.6));
        }

        .float-icon.pulsing {
            animation: radioPulse 1.2s infinite ease-out;
        }

        @keyframes radioPulse {
            0% {
                filter: drop-shadow(0 0 0px rgba(92, 124, 250, 0));
                transform: scale(1);
            }
            50% {
                filter: drop-shadow(0 0 16px rgba(92, 124, 250, 0.8));
                transform: scale(1.05);
            }
            100% {
                filter: drop-shadow(0 0 0px rgba(92, 124, 250, 0));
                transform: scale(1);
            }
        }
        
        @media (max-width: 768px) {
            body { padding: 15px; }
            .tiles-grid { grid-template-columns: repeat(auto-fill, minmax(120px, 140px)); gap: 12px; }
            .tile img { width: 44px; height: 44px; }
            .top-bar { flex-direction: column; }
            .weather-widget { width: 100%; }
            .pure-icon { width: 60px; height: 60px; }
            .radio-float-btn { width: 60px; height: 60px; }
            .radio-top-row { flex-direction: column; align-items: stretch; }
            .radio-player-box { flex-direction: column; align-items: stretch; }
            .current-station { text-align: center; }
            .add-radio-btn { text-align: center; }
            .lang-switch { top: 10px; right: 15px; font-size: 10px; }
        }
        
        .toast {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #4c9a6e;
            padding: 10px 20px;
            border-radius: 8px;
            animation: slideIn 0.3s;
            z-index: 1000;
        }
        
        @keyframes slideIn {
            from { transform: translateX(100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }
    </style>
</head>
<body>
    <!-- Плавающая переносимая кнопка радио -->
    <div id="radioFloatBtn" class="radio-float-btn" title="Перетащите мышкой | Нажмите для Play/Pause">
        <img id="floatIcon" class="float-icon" src="https://img.icons8.com/?size=100&id=ZfzQA6FzEiPP&format=png&color=5C7CFA" alt="Play/Pause">
    </div>
    
    <div class="lang-switch" id="langSwitch">EN</div>
    
    <div class="container">
        <div class="top-bar">
            <div class="left-area">
                <form class="search-form" id="searchForm">
                    <span class="search-icon">🔍</span>
                    <input type="text" class="search-input" id="searchInput" placeholder="Поиск в Google...">
                    <button type="submit" class="search-btn">Найти</button>
                </form>
                <div class="icon-row">
                    <a href="https://web.telegram.org/k/" target="_blank" class="pure-icon" title="Telegram">
                        <img src="https://img.icons8.com/?size=100&id=lUktdBVdL4Kb&format=png&color=FF005A" alt="TG">
                    </a>
                    <a href="https://discord.com/app" target="_blank" class="pure-icon" title="Discord">
                        <img src="https://img.icons8.com/?size=100&id=30888&format=png&color=FF005A" alt="Discord">
                    </a>
                    <a href="https://music.youtube.com/" target="_blank" class="pure-icon" title="YouTube Music">
                        <img src="https://img.icons8.com/?size=100&id=Mw6P3tmWMfOB&format=png&color=FF005A" alt="YouTube Music">
                    </a>
                    <a href="https://music.yandex.ru/" target="_blank" class="pure-icon" title="Yandex Music">
                        <img src="https://img.icons8.com/?size=100&id=DGHVPGM9dSp4&format=png&color=FF005A" alt="Yandex Music">
                    </a>
                    <a href="https://vk.com/" target="_blank" class="pure-icon" title="VK">
                        <img src="https://img.icons8.com/?size=100&id=60454&format=png&color=FF005A" alt="VK">
                    </a>
                </div>
            </div>
            <div class="weather-widget">
                <div class="elfsight-app-80b5fcbd-xxxx-xxxx-a654-xxxxxxxxxxxx" data-elfsight-app-lazy></div>
            </div>
        </div>
        
        <div id="tilesContainer" class="tiles-grid"></div>
        
        <div class="add-form">
            <input type="text" id="siteTitle" placeholder="Название сайта (например, YouTube)">
            <input type="text" id="siteUrl" placeholder="Ссылка (https://youtube.com)">
            <button id="addBtn">➕ Добавить плитку</button>
        </div>
        
        <div class="radio-section">
            <div class="radio-top-row">
                <div class="radio-player-box">
                    <div class="current-station" id="currentStationName">🎵 Выберите радио</div>
                    <audio id="radio-player" controls></audio>
                </div>
                <button class="add-radio-btn" id="showAddRadioBtn">➕ Добавить радио</button>
            </div>
            
            <div class="radio-add-form" id="radioAddForm">
                <input type="text" id="radioName" placeholder="Название радио (например, Record)">
                <input type="text" id="radioUrl" placeholder="Ссылка на поток (https://...mp3, .aac)">
                <button id="saveRadioBtn">💾 Сохранить</button>
                <button id="cancelRadioBtn" style="background:#8b3c3c;">❌ Отмена</button>
            </div>
            
            <div id="radioList" class="radio-grid"></div>
        </div>
        
        <div class="controls-footer">
            <button id="exportBtn" class="control-btn">📤 Экспорт закладок</button>
            <button id="importBtn" class="control-btn">📥 Импорт закладок</button>
            <button id="resetBtn" class="control-btn danger">🗑️ Сбросить всё</button>
        </div>
    </div>

    <script>
        // ========== КЭШИРОВАННЫЕ DOM-ЭЛЕМЕНТЫ ==========
        const dom = {
            tilesContainer: document.getElementById('tilesContainer'),
            radioList: document.getElementById('radioList'),
            radioPlayer: document.getElementById('radio-player'),
            currentStationName: document.getElementById('currentStationName'),
            searchInput: document.getElementById('searchInput'),
            searchForm: document.getElementById('searchForm'),
            siteTitle: document.getElementById('siteTitle'),
            siteUrl: document.getElementById('siteUrl'),
            addBtn: document.getElementById('addBtn'),
            exportBtn: document.getElementById('exportBtn'),
            importBtn: document.getElementById('importBtn'),
            resetBtn: document.getElementById('resetBtn'),
            showAddRadioBtn: document.getElementById('showAddRadioBtn'),
            radioAddForm: document.getElementById('radioAddForm'),
            radioName: document.getElementById('radioName'),
            radioUrl: document.getElementById('radioUrl'),
            saveRadioBtn: document.getElementById('saveRadioBtn'),
            cancelRadioBtn: document.getElementById('cancelRadioBtn'),
            langSwitch: document.getElementById('langSwitch'),
            floatBtn: document.getElementById('radioFloatBtn'),
            floatIcon: document.getElementById('floatIcon')
        };
        
        // ========== СОСТОЯНИЕ ==========
        const state = {
            bookmarks: [],
            radioStations: [],
            currentRadio: null,
            currentLang: 'ru',
            isDragging: false,
            dragStartX: 0,
            dragStartY: 0,
            startLeft: 0,
            startTop: 0
        };
        
        // ========== ПЕРЕВОДЫ ==========
        const translations = {
            ru: {
                searchPlaceholder: "Поиск в Google...",
                searchBtn: "Найти",
                addSitePlaceholder: "Название сайта (например, YouTube)",
                addUrlPlaceholder: "Ссылка (https://youtube.com)",
                addSiteBtn: "➕ Добавить плитку",
                addRadioBtn: "➕ Добавить радио",
                radioNamePlaceholder: "Название радио (например, Record)",
                radioUrlPlaceholder: "Ссылка на поток (https://...mp3, .aac)",
                saveRadio: "💾 Сохранить",
                cancelRadio: "❌ Отмена",
                exportBtn: "📤 Экспорт закладок",
                importBtn: "📥 Импорт закладок",
                resetBtn: "🗑️ Сбросить всё",
                noBookmarks: "Нет закладок. Добавьте первую плитку!",
                noRadio: "Нет радиостанций. Нажмите \"+ Добавить радио\"",
                currentStation: "🎵 Сейчас играет: ",
                selectRadio: "🎵 Выберите радио",
                deleteConfirm: "Удалить радиостанцию",
                deleteYes: "Да, удалить",
                deleteNo: "Отмена",
                stationDeleted: "Радиостанция удалена",
                fillFields: "Заполните название и ссылку",
                added: "✓ Плитка добавлена",
                invalidUrl: "Некорректная ссылка",
                exportDone: "Экспорт выполнен",
                importSuccess: "Импорт успешен!",
                importError: "Ошибка при импорте",
                resetConfirm: "Удалить все закладки?",
                resetDone: "Все закладки удалены",
                radioAdded: "✓ Радио добавлено"
            },
            en: {
                searchPlaceholder: "Search Google...",
                searchBtn: "Search",
                addSitePlaceholder: "Site name (e.g., YouTube)",
                addUrlPlaceholder: "URL (https://youtube.com)",
                addSiteBtn: "➕ Add tile",
                addRadioBtn: "➕ Add radio",
                radioNamePlaceholder: "Radio name (e.g., Record)",
                radioUrlPlaceholder: "Stream URL (https://...mp3, .aac)",
                saveRadio: "💾 Save",
                cancelRadio: "❌ Cancel",
                exportBtn: "📤 Export bookmarks",
                importBtn: "📥 Import bookmarks",
                resetBtn: "🗑️ Reset all",
                noBookmarks: "No bookmarks. Add your first tile!",
                noRadio: "No radio stations. Click \"+ Add radio\"",
                currentStation: "🎵 Now playing: ",
                selectRadio: "🎵 Select radio",
                deleteConfirm: "Delete radio station",
                deleteYes: "Yes, delete",
                deleteNo: "Cancel",
                stationDeleted: "Radio station deleted",
                fillFields: "Fill in name and URL",
                added: "✓ Tile added",
                invalidUrl: "Invalid URL",
                exportDone: "Export completed",
                importSuccess: "Import successful!",
                importError: "Import error",
                resetConfirm: "Delete all bookmarks?",
                resetDone: "All bookmarks deleted",
                radioAdded: "✓ Radio added"
            }
        };
        
        function t(key) { return translations[state.currentLang][key] || key; }
        
        // ========== TOAST С ОГРАНИЧЕНИЕМ ==========
        function showToast(msg) {
            const existing = document.querySelectorAll('.toast');
            if (existing.length > 3) existing[0].remove();
            
            const toast = document.createElement('div');
            toast.className = 'toast';
            toast.textContent = msg;
            document.body.appendChild(toast);
            setTimeout(() => toast.remove(), 2000);
        }
        
        // ========== РАДИО ==========
        function loadRadio() {
            const saved = localStorage.getItem('steam_radio');
            if (saved) state.radioStations = JSON.parse(saved);
            if (!state.radioStations || state.radioStations.length === 0) {
                state.radioStations = [
                    { id: crypto.randomUUID(), name: "Radio Record", url: "https://radiorecord.hostingradio.ru/rr_main96.aacp" },
                    { id: crypto.randomUUID(), name: "Европа-Плюс", url: "http://ep128.hostingradio.ru:8030/ep128" },
                    { id: crypto.randomUUID(), name: "Black Rap", url: "https://radiorecord.hostingradio.ru/yo96.aacp" },
                    { id: crypto.randomUUID(), name: "Chill-Out", url: "https://radiorecord.hostingradio.ru/chil96.aacp" },
                    { id: crypto.randomUUID(), name: "Hypnotic", url: "https://radiorecord.hostingradio.ru/hypno96.aacp" }
                ];
                saveRadio();
            }
            renderRadio();
        }
        
        function saveRadio() { localStorage.setItem('steam_radio', JSON.stringify(state.radioStations)); }
        
        function playRadio(station) {
            state.currentRadio = station;
            
            // Очищаем старый поток
            dom.radioPlayer.pause();
            dom.radioPlayer.src = '';
            dom.radioPlayer.load();
            
            dom.radioPlayer.src = station.url;
            dom.currentStationName.textContent = t('currentStation') + station.name;
            dom.radioPlayer.play().catch(e => console.log('Play manually'));
            renderRadio();
            updateFloatButton();
        }
        
        function confirmDeleteRadio(station) {
            const overlay = document.createElement('div');
            overlay.className = 'confirm-overlay';
            overlay.innerHTML = `
                <div class="confirm-dialog">
                    <p>${t('deleteConfirm')} <span class="station-name">${escapeHtml(station.name)}</span>?</p>
                    <div class="confirm-buttons">
                        <button class="confirm-btn confirm-yes" id="confirmYes">${t('deleteYes')}</button>
                        <button class="confirm-btn confirm-no" id="confirmNo">${t('deleteNo')}</button>
                    </div>
                </div>
            `;
            document.body.appendChild(overlay);
            
            const yesBtn = document.getElementById('confirmYes');
            const noBtn = document.getElementById('confirmNo');
            
            const handleYes = () => {
                state.radioStations = state.radioStations.filter(s => s.id !== station.id);
                saveRadio();
                if (state.currentRadio && state.currentRadio.id === station.id) {
                    state.currentRadio = null;
                    dom.radioPlayer.pause();
                    dom.radioPlayer.src = '';
                    dom.radioPlayer.load();
                    dom.currentStationName.textContent = t('selectRadio');
                    updateFloatButton();
                }
                renderRadio();
                showToast(t('stationDeleted'));
                overlay.remove();
            };
            const handleNo = () => overlay.remove();
            
            yesBtn.addEventListener('click', handleYes, { once: true });
            noBtn.addEventListener('click', handleNo, { once: true });
            overlay.addEventListener('click', (e) => { if (e.target === overlay) overlay.remove(); });
        }
        
        function addRadio(name, url) {
            if (!name || !url) { showToast(t('fillFields')); return false; }
            if (!url.startsWith('http')) url = 'https://' + url;
            state.radioStations.push({ id: crypto.randomUUID(), name: name, url: url });
            saveRadio();
            renderRadio();
            return true;
        }
        
        function renderRadio() {
            if (!dom.radioList) return;
            const fragment = document.createDocumentFragment();
            
            state.radioStations.forEach(station => {
                const card = document.createElement('div');
                card.className = 'radio-card';
                if (state.currentRadio && state.currentRadio.id === station.id) card.classList.add('active');
                card.onclick = () => playRadio(station);
                card.innerHTML = `
                    <div class="radio-icon"><img src="https://img.icons8.com/?size=100&id=9js2jhDqLr76&format=png&color=000000" alt="radio"></div>
                    <div class="radio-info">
                        <div class="radio-name">${escapeHtml(station.name)}</div>
                        <div class="radio-url">${escapeHtml(station.url.substring(0, 50))}${station.url.length > 50 ? '...' : ''}</div>
                    </div>
                    <div class="delete-radio">✕</div>
                `;
                card.querySelector('.delete-radio').onclick = (e) => { e.stopPropagation(); confirmDeleteRadio(station); };
                fragment.appendChild(card);
            });
            
            dom.radioList.innerHTML = '';
            dom.radioList.appendChild(fragment);
            
            if (state.radioStations.length === 0) {
                dom.radioList.innerHTML = `<div style="padding:20px; text-align:center; opacity:0.6;">${t('noRadio')}</div>`;
            }
        }
        
        // ========== ЗАКЛАДКИ ==========
        function loadData() {
            const saved = localStorage.getItem('steam_dashboard');
            state.bookmarks = saved ? JSON.parse(saved) : [
                { title: "YouTube", url: "https://youtube.com" },
                { title: "GitHub", url: "https://github.com" },
                { title: "Steam Community", url: "https://steamcommunity.com" },
                { title: "Twitch", url: "https://twitch.tv" },
                { title: "Reddit", url: "https://reddit.com" }
            ];
            saveData();
            render();
        }
        
        function saveData() { localStorage.setItem('steam_dashboard', JSON.stringify(state.bookmarks)); }
        
        function getFavicon(url) {
            try { return `https://www.google.com/s2/favicons?domain=${new URL(url).hostname}&sz=64`; } 
            catch(e) { return 'https://www.google.com/s2/favicons?domain=steampowered.com'; }
        }
        
        function render() {
            if (!dom.tilesContainer) return;
            const fragment = document.createDocumentFragment();
            
            state.bookmarks.forEach((item, i) => {
                const tile = document.createElement('div');
                tile.className = 'tile';
                tile.dataset.index = i;
                tile.innerHTML = `
                    <div class="delete-tile">✕</div>
                    <img src="${getFavicon(item.url)}" onerror="this.src='https://www.google.com/s2/favicons?domain=steampowered.com'">
                    <div class="title">${escapeHtml(item.title)}</div>
                `;
                fragment.appendChild(tile);
            });
            
            dom.tilesContainer.innerHTML = '';
            dom.tilesContainer.appendChild(fragment);
            
            if (state.bookmarks.length === 0) {
                dom.tilesContainer.innerHTML = `<div style="text-align:center; padding:50px;">${t('noBookmarks')}</div>`;
            }
        }
        
        // Делегирование событий для плиток
        dom.tilesContainer.addEventListener('click', (e) => {
            const deleteBtn = e.target.closest('.delete-tile');
            const tile = e.target.closest('.tile');
            if (!tile) return;
            
            if (deleteBtn) {
                const index = parseInt(tile.dataset.index);
                if (!isNaN(index)) {
                    state.bookmarks.splice(index, 1);
                    saveData();
                    render();
                    showToast('Плитка удалена');
                }
            } else {
                const index = parseInt(tile.dataset.index);
                if (!isNaN(index) && state.bookmarks[index]) {
                    window.location.href = state.bookmarks[index].url;
                }
            }
        });
        
        function addBookmark() {
            let title = dom.siteTitle.value.trim();
            let url = dom.siteUrl.value.trim();
            if (!title || !url) { showToast(t('fillFields')); return; }
            if (!url.startsWith('http')) url = 'https://' + url;
            try {
                new URL(url);
                state.bookmarks.push({ title, url });
                saveData();
                render();
                dom.siteTitle.value = '';
                dom.siteUrl.value = '';
                showToast(t('added'));
            } catch(e) { showToast(t('invalidUrl')); }
        }
        
        function exportBookmarks() {
            const a = document.createElement('a');
            a.href = URL.createObjectURL(new Blob([JSON.stringify(state.bookmarks, null, 2)], {type:'application/json'}));
            a.download = `steam_bookmarks_${new Date().toISOString().slice(0,10)}.json`;
            a.click();
            URL.revokeObjectURL(a.href);
            showToast(t('exportDone'));
        }
        
        function importBookmarks() {
            const input = document.createElement('input');
            input.type = 'file';
            input.accept = 'application/json';
            input.onchange = (e) => {
                const file = e.target.files[0];
                const reader = new FileReader();
                reader.onload = (ev) => {
                    try {
                        const imported = JSON.parse(ev.target.result);
                        if (Array.isArray(imported)) {
                            state.bookmarks = imported;
                            saveData();
                            render();
                            showToast(t('importSuccess'));
                        }
                    } catch(err) { showToast(t('importError')); }
                    reader.onload = null;
                };
                reader.readAsText(file);
            };
            input.click();
        }
        
        function resetAll() { if (confirm(t('resetConfirm'))) { state.bookmarks = []; saveData(); render(); showToast(t('resetDone')); } }
        
        // ========== ПОИСК ==========
        function setupSearch() {
            dom.searchForm.onsubmit = (e) => {
                e.preventDefault();
                const q = dom.searchInput.value.trim();
                if (q) window.open(`https://www.google.com/search?q=${encodeURIComponent(q)}`, '_blank');
            };
        }
        
        // ========== ЯЗЫК ==========
        function updateLanguage() {
            dom.searchInput.placeholder = t('searchPlaceholder');
            document.querySelector('.search-btn').textContent = t('searchBtn');
            dom.siteTitle.placeholder = t('addSitePlaceholder');
            dom.siteUrl.placeholder = t('addUrlPlaceholder');
            dom.addBtn.textContent = t('addSiteBtn');
            dom.showAddRadioBtn.textContent = t('addRadioBtn');
            dom.radioName.placeholder = t('radioNamePlaceholder');
            dom.radioUrl.placeholder = t('radioUrlPlaceholder');
            dom.saveRadioBtn.textContent = t('saveRadio');
            dom.cancelRadioBtn.textContent = t('cancelRadio');
            dom.exportBtn.textContent = t('exportBtn');
            dom.importBtn.textContent = t('importBtn');
            dom.resetBtn.textContent = t('resetBtn');
            dom.langSwitch.textContent = state.currentLang === 'ru' ? 'EN' : 'RU';
            
            if (state.bookmarks.length === 0) {
                if (dom.tilesContainer && dom.tilesContainer.innerHTML.includes('Нет закладок')) {
                    dom.tilesContainer.innerHTML = `<div style="text-align:center; padding:50px;">${t('noBookmarks')}</div>`;
                }
            }
            if (state.radioStations.length === 0) {
                if (dom.radioList && dom.radioList.innerHTML.includes('Нет радиостанций')) {
                    dom.radioList.innerHTML = `<div style="padding:20px; text-align:center; opacity:0.6;">${t('noRadio')}</div>`;
                }
            }
            if (state.currentRadio) {
                dom.currentStationName.textContent = t('currentStation') + state.currentRadio.name;
            } else {
                dom.currentStationName.textContent = t('selectRadio');
            }
        }
        
        function switchLanguage() {
            state.currentLang = state.currentLang === 'ru' ? 'en' : 'ru';
            localStorage.setItem('steam_language', state.currentLang);
            updateLanguage();
        }
        
        function loadLanguage() {
            const saved = localStorage.getItem('steam_language');
            if (saved && (saved === 'ru' || saved === 'en')) state.currentLang = saved;
            updateLanguage();
        }
        
        // ========== ПЕРЕНОСИМАЯ КНОПКА РАДИО ==========
        const PLAY_ICON = "https://img.icons8.com/?size=100&id=ZfzQA6FzEiPP&format=png&color=5C7CFA";
        const PAUSE_ICON = "https://img.icons8.com/?size=100&id=EEH2MmtvmpiW&format=png&color=5C7CFA";
        
        function saveButtonPosition(left, top) {
            localStorage.setItem('radio_btn_left', left);
            localStorage.setItem('radio_btn_top', top);
        }
        
        function loadButtonPosition() {
            const savedLeft = localStorage.getItem('radio_btn_left');
            const savedTop = localStorage.getItem('radio_btn_top');
            if (savedLeft && savedTop) {
                dom.floatBtn.style.left = savedLeft;
                dom.floatBtn.style.top = savedTop;
            } else {
                dom.floatBtn.style.left = '20px';
                dom.floatBtn.style.top = '20px';
            }
        }
        
        function onMouseDown(e) {
            if (e.button !== 0) return;
            e.preventDefault();
            state.isDragging = true;
            state.dragStartX = e.clientX;
            state.dragStartY = e.clientY;
            state.startLeft = dom.floatBtn.offsetLeft;
            state.startTop = dom.floatBtn.offsetTop;
            dom.floatBtn.style.cursor = 'grabbing';
            dom.floatBtn.style.transition = 'none';
        }
        
        function onMouseMove(e) {
            if (!state.isDragging) return;
            e.preventDefault();
            const dx = e.clientX - state.dragStartX;
            const dy = e.clientY - state.dragStartY;
            let newLeft = state.startLeft + dx;
            let newTop = state.startTop + dy;
            
            const maxLeft = window.innerWidth - dom.floatBtn.offsetWidth;
            const maxTop = window.innerHeight - dom.floatBtn.offsetHeight;
            newLeft = Math.max(0, Math.min(maxLeft, newLeft));
            newTop = Math.max(0, Math.min(maxTop, newTop));
            
            dom.floatBtn.style.left = newLeft + 'px';
            dom.floatBtn.style.top = newTop + 'px';
        }
        
        function onMouseUp(e) {
            if (!state.isDragging) return;
            state.isDragging = false;
            dom.floatBtn.style.cursor = 'grab';
            dom.floatBtn.style.transition = '';
            saveButtonPosition(dom.floatBtn.style.left, dom.floatBtn.style.top);
        }
        
        function updateFloatButton() {
            if (!dom.radioPlayer.paused && dom.radioPlayer.src && !dom.radioPlayer.ended) {
                dom.floatIcon.src = PAUSE_ICON;
                dom.floatIcon.classList.add('pulsing');
                dom.floatBtn.title = 'Пауза | Перетащите мышкой';
            } else {
                dom.floatIcon.src = PLAY_ICON;
                dom.floatIcon.classList.remove('pulsing');
                dom.floatBtn.title = 'Включить радио | Перетащите мышкой';
            }
        }
        
        function toggleRadio(e) {
            if (state.isDragging) return;
            e.stopPropagation();
            
            if (dom.radioPlayer.paused || !dom.radioPlayer.src || dom.radioPlayer.ended) {
                if (state.currentRadio) {
                    playRadio(state.currentRadio);
                } else if (state.radioStations.length > 0) {
                    playRadio(state.radioStations[0]);
                } else {
                    showToast(t('noRadio'));
                    return;
                }
            } else {
                dom.radioPlayer.pause();
            }
            updateFloatButton();
        }
        
        function initRadioForm() {
            dom.showAddRadioBtn.onclick = () => {
                dom.radioAddForm.style.display = 'flex';
                dom.radioName.value = '';
                dom.radioUrl.value = '';
            };
            dom.cancelRadioBtn.onclick = () => { dom.radioAddForm.style.display = 'none'; };
            dom.saveRadioBtn.onclick = () => {
                if (addRadio(dom.radioName.value.trim(), dom.radioUrl.value.trim())) {
                    dom.radioAddForm.style.display = 'none';
                    showToast(t('radioAdded'));
                }
            };
            dom.radioName.onkeypress = dom.radioUrl.onkeypress = (e) => { if (e.key === 'Enter') dom.saveRadioBtn.click(); };
        }
        
        function escapeHtml(str) { if (!str) return ''; return str.replace(/[&<>]/g, m => ({'&':'&amp;','<':'&lt;','>':'&gt;'}[m])); }
        
        // ========== ИНИЦИАЛИЗАЦИЯ ==========
        function init() {
            dom.addBtn.onclick = addBookmark;
            dom.exportBtn.onclick = exportBookmarks;
            dom.importBtn.onclick = importBookmarks;
            dom.resetBtn.onclick = resetAll;
            dom.siteUrl.onkeypress = (e) => { if (e.key === 'Enter') addBookmark(); };
            dom.langSwitch.onclick = switchLanguage;
            
            // Drag & Drop события
            dom.floatBtn.addEventListener('mousedown', onMouseDown);
            window.addEventListener('mousemove', onMouseMove);
            window.addEventListener('mouseup', onMouseUp);
            
            // Очистка перед закрытием
            window.addEventListener('beforeunload', () => {
                window.removeEventListener('mousemove', onMouseMove);
                window.removeEventListener('mouseup', onMouseUp);
            });
            
            dom.radioPlayer.addEventListener('play', updateFloatButton);
            dom.radioPlayer.addEventListener('pause', updateFloatButton);
            dom.radioPlayer.addEventListener('ended', updateFloatButton);
            dom.floatBtn.addEventListener('click', toggleRadio);
            
            loadLanguage();
            loadData();
            loadRadio();
            setupSearch();
            initRadioForm();
            loadButtonPosition();
            dom.floatBtn.style.cursor = 'grab';
            updateFloatButton();
        }
        
        init();
    </script>
</body>
</html>
```
</details>

## 💡 Авторская заметка

Файл можно настраивать под себя: менять иконки, добавлять свои виджеты, менять цветовую схему.  
Если вы захотите поделиться своим вариантом или улучшить проект — добро пожаловать в открытый исходный код.

**Приятного использования!** 🎮

