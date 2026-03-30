**Language:** [Russian](README.md) | [English](README_EN.md)

# Steam-Overlay-SpeedDial
Custom Start Page for Steam Overlay Browser | Features site tiles, radio with global controls, Google search, weather, Telegram, Discord, YouTube Music, Yandex Music, VK. Supports bookmark import/export and RU/EN languages.

[Demo site Home Page Overlay](https://blog1703.github.io/Steam-Overlay-SpeedDial/demo.html)

## 🛠️ Installation

### 1️⃣ Save the file

Download or copy the `dashboard.html` file and save it somewhere convenient.  
_Example:_ create a folder named `SteamOverlayDashboard` inside your Documents folder: C:\Users\USERNAME\Documents\SteamOverlayDashboard\dashboard.html

### 2️⃣ Copy the file path

Right-click on `dashboard.html` → select **‘Copy as path’**.  
You should get a string like this: C:\Users\PC\Documents\SteamOverlayDashboard\dashboard.html

### 3️⃣ Close Steam

> ⚠️ **Important:** Steam must be **fully closed**, otherwise changes to the settings file won’t be saved.

### 4️⃣ Open the Steam settings file

Navigate to the path: C:\Program Files (x86)\Steam\userdata\XXXXXXXX\config\localconfig.vdf

- `XXXXXXXX` — this is your folder named with your Steam ID numbers
- Open the file using **Notepad** or **Notepad++**

### 5️⃣ Add a line with your homepage URL

Locate the code line containing "GameOverlayHomePage" and replace `"https://www.google.com/"` with `"file:/C:/Users/USERNAME/Documents/SteamOverlayDashboard/dashboard.html"`

![code line where you need to make the replacement](https://raw.githubusercontent.com/blog1703/Steam-Overlay-SpeedDial/refs/heads/main/images/final.gif)

*code line where you need to make the replacement*

🔑 **Key tip:**  
At the start of the path, use **a single slash** — `file:/`, not three `(file:///)`).  
Steam Overlay blocks `file:///`, but **does not block** `file:/`.

### 6️⃣ Save the file and close the editor

### 7️⃣ Launch Steam and verify

- Start any game with overlay support (Shift+Tab)
- Open the browser in the overlay — you will see your new Home page
- If the page doesn’t open automatically, click the home icon 🏠

### Here’s how the Home page will look

![Home page in Steam browser](https://raw.githubusercontent.com/blog1703/Steam-Overlay-Dashboard/refs/heads/main/images/dashboard1.jpg)

*Steam browser home page*

---

## 🎨 Customization

The file `dashboard.html` can be edited as you wish:

- ➕ Add your websites to tiles
- 🎵 Change radio stations
- 🎨 Change colors, fonts, animations
- 🌐  Add new service icons

Everything is located inside one file - it is very easy to change it, you can use Chat GPT, DeepSeek.

## ➕ Adding new quick link icons

You can easily add any service to the icon bar (Telegram, Discord, YouTube Music, Yandex Music, VK).

### Template to add

```html
<a href="https://link-to-service" target="_blank" class="pure-icon" title="Name">
    <img src="https://img.icons8.com/?size=100&id=ID_ICONS&format=png&color=000000" alt="Name">
</a>
```

---
## 📄 Full project code
<details>
  <summary>📄 Click to show dashboard.html</summary>
  
  
  ```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Steam Overlay Dashboard</title>
    <style>
        /* ============================================ */
        /* ========== 1. ГЛОБАЛЬНЫЕ СТИЛИ ========== */
        /* ============================================ */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1e1f2c 0%, #13141f 100%);
            min-height: 100vh;
            padding: 30px;
            color: #e0e0e0;
            transition: background 0.3s ease;
        }
        .container { 
            max-width: 1400px; 
            margin: 0 auto;
            display: flex;
            flex-direction: column;
            min-height: calc(100vh - 60px);
        }
        
        /* ============================================ */
        /* ========== 2. ТЕМЫ ОФОРМЛЕНИЯ ========== */
        /* ============================================ */
        body[data-theme="default"] {
            background: linear-gradient(135deg, #1e1f2c 0%, #13141f 100%);
            color: #e0e0e0;
        }
        body[data-theme="default"] .current-station { color: #66c0f4; }
        body[data-theme="default"] .search-btn { background: #66c0f4; color: #1a1a2e; }
        body[data-theme="default"] .icon-circle { background: rgba(42, 71, 94, 0.6); }
        body[data-theme="default"] .play-btn { background: #66c0f4; }
        body[data-theme="default"] .volume-slider { accent-color: #66c0f4; }
        body[data-theme="default"] .station-status { color: #66c0f4; }
        
        body[data-theme="minimalism"] {
            background: #f5f5f5;
            color: #1a1a1a;
        }
        body[data-theme="minimalism"] .current-station { color: #333; }
        body[data-theme="minimalism"] .tile,
        body[data-theme="minimalism"] .radio-card,
        body[data-theme="minimalism"] .add-form,
        body[data-theme="minimalism"] .top-bar {
            background: rgba(255,255,255,0.8);
            backdrop-filter: blur(4px);
            border-color: #ddd;
            color: #333;
        }
        body[data-theme="minimalism"] .search-input {
            color: #333;
            background: rgba(0,0,0,0.05);
        }
        body[data-theme="minimalism"] .search-input::placeholder { color: #999; }
        body[data-theme="minimalism"] .search-btn { background: #e0e0e0; color: #333; }
        body[data-theme="minimalism"] .icon-circle { background: rgba(200,200,200,0.8); color: #333; }
        body[data-theme="minimalism"] .theme-dot { border-color: #999; }
        body[data-theme="minimalism"] .play-btn { background: #ff6b6b; }
        body[data-theme="minimalism"] .volume-slider { accent-color: #ff6b6b; }
        body[data-theme="minimalism"] .station-status { color: #ff6b6b; }
        
        body[data-theme="amber"] {
            background: #2a1a0a;
            color: #ffb347;
        }
        body[data-theme="amber"] .current-station { color: #ffb347; }
        body[data-theme="amber"] .tile,
        body[data-theme="amber"] .radio-card,
        body[data-theme="amber"] .add-form,
        body[data-theme="amber"] .top-bar {
            background: rgba(50, 30, 10, 0.6);
            border-color: #ffb347;
            color: #ffb347;
        }
        body[data-theme="amber"] .search-input {
            background: rgba(0,0,0,0.5);
            border-color: #ffb347;
            color: #ffb347;
        }
        body[data-theme="amber"] .search-input::placeholder { color: #ffb34780; }
        body[data-theme="amber"] .search-btn { background: #3a2a1a; border: 1px solid #ffb347; color: #ffb347; }
        body[data-theme="amber"] .icon-circle { background: rgba(50, 30, 10, 0.8); color: #ffb347; border-color: #ffb347; }
        body[data-theme="amber"] .theme-dot { border-color: #ffb347; }
        body[data-theme="amber"] .play-btn { background: #ffb347; }
        body[data-theme="amber"] .play-btn .icon { fill: #2a1a0a; }
        body[data-theme="amber"] .volume-slider { accent-color: #ffb347; }
        body[data-theme="amber"] .station-status { color: #ffb347; }
        
        body[data-theme="pink"] {
            background: linear-gradient(135deg, #4a2a3a 0%, #6a3a4a 100%);
            color: #ffe0f0;
        }
        body[data-theme="pink"] .current-station { color: #ff99cc; }
        body[data-theme="pink"] .tile,
        body[data-theme="pink"] .radio-card,
        body[data-theme="pink"] .add-form,
        body[data-theme="pink"] .top-bar {
            background: rgba(255, 200, 220, 0.2);
            backdrop-filter: blur(4px);
            border-color: rgba(255, 255, 255, 0.4);
            color: #ffe0f0;
        }
        body[data-theme="pink"] .search-input {
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.3);
            color: #ffe0f0;
        }
        body[data-theme="pink"] .search-input::placeholder { color: #ffc0e0; }
        body[data-theme="pink"] .search-btn { background: #ff99cc; color: #4a2a3a; border: 1px solid rgba(255, 255, 255, 0.5); }
        body[data-theme="pink"] .icon-circle { background: rgba(255, 200, 220, 0.3); color: #ff99cc; border-color: rgba(255, 255, 255, 0.5); }
        body[data-theme="pink"] .play-btn { background: #ff99cc; }
        body[data-theme="pink"] .play-btn .icon { fill: #4a2a3a; }
        body[data-theme="pink"] .volume-slider { accent-color: #ff99cc; }
        body[data-theme="pink"] .station-status { color: #ff99cc; }
        
        /* ============================================ */
        /* ========== 3. ВЕРХНЯЯ ПАНЕЛЬ ========== */
        /* ============================================ */
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
        
        .theme-dots {
            display: flex;
            gap: 8px;
            align-items: center;
            margin-bottom: 12px;
        }
        .theme-dot {
            width: 15px;
            height: 15px;
            border-radius: 50%;
            cursor: pointer;
            transition: all 0.2s ease;
            border: 1px solid rgba(255,255,255,0.3);
        }
        .theme-dot:hover {
            transform: scale(1.2);
            box-shadow: 0 0 6px rgba(102, 192, 244, 0.6);
        }
        .theme-dot.active {
            border: 2px solid white;
            transform: scale(1.15);
            box-shadow: 0 0 8px rgba(255,215,0,0.8);
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
        .search-icon { font-size: 18px; color: #66c0f4; }
        .search-input {
            flex: 1;
            background: transparent;
            border: none;
            padding: 12px 5px;
            color: white;
            font-size: 16px;
            outline: none;
        }
        .search-input::placeholder { color: rgba(255,255,255,0.5); }
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
            position: relative;
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
        
        .add-icon-circle {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            width: 80px;
            height: 80px;
            background: rgba(42, 71, 94, 0.3);
            border: 2px dashed rgba(102, 192, 244, 0.6);
            border-radius: 50%;
            cursor: pointer;
            transition: all 0.3s ease;
            text-decoration: none;
        }
        .add-icon-circle svg {
            width: 48px;
            height: 48px;
            stroke: #66c0f4;
            stroke-width: 2;
            stroke-linecap: round;
        }
        .add-icon-circle:hover {
            background: rgba(102, 192, 244, 0.2);
            border-color: #66c0f4;
            transform: scale(1.05);
        }
        .add-icon-circle:hover svg { stroke: #ffffff; }
        
        .right-area {
            display: flex;
            flex-direction: column;
            align-items: flex-end;
            gap: 8px;
        }
        .right-icons {
            display: flex;
            gap: 12px;
            align-items: center;
        }
        .icon-circle {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            background: rgba(42, 71, 94, 0.6);
            border: 1px solid rgba(102, 192, 244, 0.3);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 18px;
            transition: all 0.2s ease;
            backdrop-filter: blur(4px);
        }
        .icon-circle:hover {
            background: #66c0f4;
            color: #1a1a2e;
            transform: scale(1.05);
        }
        
        .lang-switch-btn {
            background: rgba(42, 71, 94, 0.6);
            border: 1px solid rgba(102, 192, 244, 0.3);
            border-radius: 30px;
            padding: 5px 12px;
            cursor: pointer;
            font-size: 12px;
            font-weight: 500;
            transition: all 0.2s;
            backdrop-filter: blur(4px);
        }
        .lang-switch-btn:hover {
            background: #66c0f4;
            color: #1a1a2e;
            border-color: #66c0f4;
        }
        
        /* Статичный блок погоды */
        .weather-static {
            min-width: 280px;
            background: rgba(0,0,0,0.3);
            border-radius: 16px;
            padding: 12px 20px;
            text-align: center;
            backdrop-filter: blur(4px);
            border: 1px solid rgba(102, 192, 244, 0.2);
        }
        .weather-static .temp {
            font-size: 28px;
            font-weight: bold;
            color: #66c0f4;
        }
        .weather-static .city {
            font-size: 12px;
            opacity: 0.7;
        }
        
        /* ============================================ */
        /* ========== 4. ПЛИТКИ ========== */
        /* ============================================ */
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
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            opacity: 0;
            transition: opacity 0.2s;
        }
        .delete-tile svg {
            width: 14px;
            height: 14px;
            stroke: #ff8a8a;
            stroke-width: 2;
            stroke-linecap: round;
        }
        .tile:hover .delete-tile { opacity: 1; }
        
        .add-tile-btn {
            background: rgba(42, 71, 94, 0.3);
            border: 2px dashed rgba(102, 192, 244, 0.6);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
            min-height: 140px;
        }
        .add-tile-btn svg {
            width: 48px;
            height: 48px;
            stroke: #66c0f4;
            stroke-width: 2;
            stroke-linecap: round;
        }
        .add-tile-btn:hover {
            background: rgba(102, 192, 244, 0.2);
            border-color: #66c0f4;
            transform: translateY(-5px);
        }
        .add-tile-btn:hover svg { stroke: #ffffff; }
        
        /* ============================================ */
        /* ========== 5. РАДИО ПЛЕЕР ========== */
        /* ============================================ */
        .radio-section { margin-bottom: 20px; }
        .radio-player-box {
            background: #000000;
            border-radius: 60px;
            padding: 12px 24px;
            border: 2px solid #ffcc00;
            display: flex;
            align-items: center;
            gap: 20px;
            flex-wrap: wrap;
            transition: all 0.3s ease;
            width: 100%;
            box-shadow: 0 2px 10px rgba(0,0,0,0.3);
        }
        .radio-player-box.playing {
            animation: pulse-glow 2s infinite ease-in-out;
        }
        @keyframes pulse-glow {
            0% { box-shadow: 0 0 0 0 rgba(255,204,0,0.4); border-color: #ffcc00; }
            70% { box-shadow: 0 0 0 6px rgba(255,204,0,0); border-color: #ffdd44; }
            100% { box-shadow: 0 0 0 0 rgba(255,204,0,0); border-color: #ffcc00; }
        }
        
        .play-btn {
            width: 48px;
            height: 48px;
            background: #66c0f4;
            border: none;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s ease;
            flex-shrink: 0;
        }
        .play-btn:hover {
            transform: scale(1.05);
            filter: brightness(1.1);
        }
        .play-btn .icon {
            width: 24px;
            height: 24px;
            fill: #1a1a2e;
        }
        
        .station-info {
            flex: 1;
            min-width: 150px;
        }
        .station-name {
            font-size: 14px;
            font-weight: bold;
            display: block;
            margin-bottom: 4px;
            color: #ffffff;
        }
        .station-status {
            font-size: 11px;
            text-transform: uppercase;
            letter-spacing: 1px;
            color: #ffcc00;
        }
        
        .volume-slider {
            width: 120px;
            cursor: pointer;
            height: 4px;
            border-radius: 5px;
            flex-shrink: 0;
            background: #333;
        }
        .volume-slider::-webkit-slider-thumb {
            background: #ffcc00;
            border-radius: 50%;
            width: 12px;
            height: 12px;
            cursor: pointer;
        }
        
        .radio-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            margin-top: 15px;
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
            transform: translateY(-2px);
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
        .radio-info { flex: 1; }
        .radio-name { font-size: 14px; font-weight: 500; }
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
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            opacity: 0;
            transition: opacity 0.2s;
        }
        .delete-radio svg {
            width: 14px;
            height: 14px;
            stroke: #ff8a8a;
            stroke-width: 2;
            stroke-linecap: round;
        }
        .radio-card:hover .delete-radio { opacity: 1; }
        
        .add-radio-card {
            background: rgba(42, 71, 94, 0.3);
            border: 2px dashed rgba(102, 192, 244, 0.6);
            border-radius: 12px;
            padding: 12px 18px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
            min-width: 140px;
        }
        .add-radio-card svg {
            width: 32px;
            height: 32px;
            stroke: #66c0f4;
            stroke-width: 2;
            stroke-linecap: round;
        }
        .add-radio-card:hover {
            background: rgba(102, 192, 244, 0.2);
            border-color: #66c0f4;
            transform: translateY(-2px);
        }
        .add-radio-card:hover svg { stroke: #ffffff; }
        
        /* ============================================ */
        /* ========== 6. ПЕРЕНОСИМАЯ КНОПКА РАДИО ========== */
        /* ============================================ */
        .radio-float-btn {
            position: fixed;
            z-index: 1000;
            cursor: grab;
            width: 80px;
            height: 80px;
            background: none;
            border: none;
            transition: all 0.3s ease;
            user-select: none;
            filter: drop-shadow(0 0 12px rgba(0,0,0,0.3));
            animation: float 3s ease-in-out infinite;
        }
        .radio-float-btn:active { cursor: grabbing; }
        @keyframes float {
            0% { transform: translateY(0px); }
            50% { transform: translateY(-8px); }
            100% { transform: translateY(0px); }
        }
        .float-icon {
            width: 100%;
            height: 100%;
            object-fit: contain;
            display: block;
            transition: all 0.3s ease;
            pointer-events: none;
            filter: drop-shadow(0 0 8px currentColor);
        }
        .radio-float-btn:hover .float-icon {
            transform: scale(1.08);
            filter: drop-shadow(0 0 20px currentColor);
        }
        .radio-float-btn:hover { animation: none; transform: translateY(-5px); }
        .float-icon.pulsing { animation: radioPulse 5.0s infinite ease-in-out; }
        @keyframes radioPulse {
            0% { filter: drop-shadow(0 0 0px currentColor); transform: scale(1); }
            50% { filter: drop-shadow(0 0 16px currentColor); transform: scale(1.03); }
            100% { filter: drop-shadow(0 0 0px currentColor); transform: scale(1); }
        }
        
        body[data-theme="default"] .radio-float-btn { color: #66c0f4; }
        body[data-theme="minimalism"] .radio-float-btn { color: #ff6b6b; }
        body[data-theme="amber"] .radio-float-btn { color: #ffaa33; }
        body[data-theme="pink"] .radio-float-btn { color: #ff99cc; }
        
        /* ============================================ */
        /* ========== 7. НАПОМИНАНИЕ ========== */
        /* ============================================ */
        .export-reminder {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #2a2a2a;
            border-left: 4px solid #ffcc00;
            padding: 12px 20px;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.3);
            z-index: 1001;
            animation: slideInReminder 0.3s ease;
            backdrop-filter: blur(8px);
            max-width: 350px;
            font-size: 13px;
        }
        @keyframes slideInReminder {
            from { transform: translateX(100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }
        .reminder-content {
            display: flex;
            align-items: flex-start;
            gap: 12px;
        }
        .reminder-icon {
            font-size: 20px;
            color: #ffcc00;
            flex-shrink: 0;
        }
        .reminder-text {
            flex: 1;
            line-height: 1.4;
        }
        .reminder-text strong {
            color: #ffcc00;
        }
        .reminder-close {
            cursor: pointer;
            font-size: 16px;
            opacity: 0.6;
            transition: opacity 0.2s;
            flex-shrink: 0;
        }
        .reminder-close:hover {
            opacity: 1;
        }
        .reminder-hint {
            font-size: 11px;
            margin-top: 6px;
            opacity: 0.7;
        }
        
        /* ============================================ */
        /* ========== 8. МОДАЛЬНЫЕ ОКНА ========== */
        /* ============================================ */
        .settings-modal, .confirm-overlay, .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            backdrop-filter: blur(8px);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2000;
        }
        
        .settings-dialog, .confirm-dialog, .modal-dialog {
            background: #1e1f2c;
            border-radius: 20px;
            padding: 25px;
            max-width: 450px;
            width: 90%;
            text-align: center;
            border: 1px solid #66c0f4;
            box-shadow: 0 0 30px rgba(102,192,244,0.3);
        }
        body[data-theme="minimalism"] .settings-dialog,
        body[data-theme="minimalism"] .confirm-dialog,
        body[data-theme="minimalism"] .modal-dialog {
            background: #e8eef2;
            color: #1a1a2e;
        }
        body[data-theme="amber"] .settings-dialog,
        body[data-theme="amber"] .confirm-dialog,
        body[data-theme="amber"] .modal-dialog {
            background: #3a2a1a;
            border-color: #ffb347;
        }
        body[data-theme="pink"] .settings-dialog,
        body[data-theme="pink"] .confirm-dialog,
        body[data-theme="pink"] .modal-dialog {
            background: #4a2a3a;
            border-color: #ff99cc;
        }
        .settings-dialog h3, .confirm-dialog p, .modal-dialog h3 {
            margin-bottom: 20px;
            color: #66c0f4;
        }
        body[data-theme="amber"] .settings-dialog h3,
        body[data-theme="amber"] .modal-dialog h3 { color: #ffb347; }
        body[data-theme="pink"] .settings-dialog h3,
        body[data-theme="pink"] .modal-dialog h3 { color: #ff99cc; }
        .confirm-dialog .station-name { color: #66c0f4; font-weight: bold; }
        
        .settings-buttons, .modal-buttons, .confirm-buttons {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        .modal-buttons, .confirm-buttons { flex-direction: row; justify-content: center; }
        .settings-btn, .modal-buttons button, .confirm-btn {
            padding: 12px;
            background: #2a475e;
            border: none;
            border-radius: 10px;
            color: white;
            cursor: pointer;
            font-size: 14px;
            font-weight: 500;
            transition: all 0.2s;
        }
        .settings-btn:hover, .modal-buttons button:hover, .confirm-btn:hover {
            background: #66c0f4;
            transform: translateX(5px);
        }
        .settings-btn.danger, .modal-cancel, .confirm-yes {
            background: #8b3c3c;
        }
        .settings-btn.danger:hover, .modal-cancel:hover, .confirm-yes:hover {
            background: #ac4c4c;
            transform: scale(1.02);
        }
        .modal-save:hover, .confirm-no:hover {
            transform: scale(1.02);
        }
        .close-settings {
            margin-top: 15px;
            padding: 10px;
            background: transparent;
            border: 1px solid #66c0f4;
            border-radius: 10px;
            color: #66c0f4;
            cursor: pointer;
            width: 100%;
        }
        body[data-theme="pink"] .close-settings {
            border-color: #ff99cc;
            color: #ff99cc;
        }
        .modal-dialog input {
            width: 100%;
            padding: 12px;
            margin-bottom: 15px;
            background: rgba(0,0,0,0.5);
            border: 1px solid rgba(255,255,255,0.2);
            border-radius: 12px;
            color: white;
            font-size: 14px;
        }
        .modal-dialog input:focus { outline: none; border-color: #66c0f4; }
        
        /* ============================================ */
        /* ========== 9. НИЖНИЙ КОЛОНТИТУЛ ========== */
        /* ============================================ */
        .github-link {
            text-align: center;
            margin-top: 30px;
            padding: 12px 20px;
            background: rgba(0,0,0,0.3);
            backdrop-filter: blur(8px);
            border-radius: 40px;
            border: 1px solid rgba(102, 192, 244, 0.3);
            transition: all 0.2s ease;
            display: inline-block;
            width: auto;
            margin-left: auto;
            margin-right: auto;
        }
        .github-link:hover {
            border-color: #66c0f4;
            background: rgba(102, 192, 244, 0.1);
            transform: translateY(-2px);
        }
        .github-link a {
            display: inline-flex;
            align-items: center;
            gap: 10px;
            color: #66c0f4;
            text-decoration: none;
            font-size: 14px;
            font-weight: 500;
        }
        .github-link a:hover { text-decoration: underline; }
        .github-link svg {
            width: 20px;
            height: 20px;
            fill: #66c0f4;
            transition: fill 0.2s;
        }
        .github-link:hover svg { fill: #ffffff; }
        body[data-theme="minimalism"] .github-link a { color: #333; }
        body[data-theme="minimalism"] .github-link svg { fill: #333; }
        body[data-theme="amber"] .github-link a { color: #ffb347; }
        body[data-theme="amber"] .github-link svg { fill: #ffb347; }
        body[data-theme="pink"] .github-link a { color: #ff99cc; }
        body[data-theme="pink"] .github-link svg { fill: #ff99cc; }
        
        /* ============================================ */
        /* ========== 10. УВЕДОМЛЕНИЯ ========== */
        /* ============================================ */
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
        
        /* ============================================ */
        /* ========== 11. АДАПТИВНОСТЬ ========== */
        /* ============================================ */
        @media (max-width: 768px) {
            body { padding: 15px; }
            .tiles-grid { grid-template-columns: repeat(auto-fill, minmax(120px, 140px)); gap: 12px; }
            .tile img { width: 44px; height: 44px; }
            .add-tile-btn { min-height: 120px; }
            .add-tile-btn svg { width: 36px; height: 36px; }
            .top-bar { flex-direction: column; }
            .pure-icon, .add-icon-circle { width: 60px; height: 60px; }
            .add-icon-circle svg { width: 36px; height: 36px; }
            .radio-float-btn { width: 60px; height: 60px; }
            .radio-player-box { border-radius: 30px; padding: 12px 16px; gap: 12px; }
            .station-info { text-align: center; min-width: 120px; }
            .volume-slider { width: 100px; }
            .right-area { align-items: flex-start; width: 100%; }
            .right-icons { justify-content: flex-start; }
            .add-radio-card { min-width: 100px; }
            .export-reminder { max-width: 280px; padding: 10px 15px; }
            .delete-tile, .delete-radio { width: 18px; height: 18px; }
            .delete-tile svg, .delete-radio svg { width: 10px; height: 10px; }
        }
    </style>
</head>
<body data-theme="default">
    <div id="radioFloatBtn" class="radio-float-btn" title="Перетащите мышкой | Нажмите для Play/Pause">
        <img id="floatIcon" class="float-icon" src="https://img.icons8.com/?size=100&id=ZfzQA6FzEiPP&format=png&color=66C0F4" alt="Play/Pause">
    </div>
    
    <div class="container">
        <div class="top-bar">
            <div class="left-area">
                <div class="theme-dots">
                    <div class="theme-dot active" data-theme="default" style="background: #66c0f4;"></div>
                    <div class="theme-dot" data-theme="minimalism" style="background: #ccc;"></div>
                    <div class="theme-dot" data-theme="amber" style="background: #ffb347;"></div>
                    <div class="theme-dot" data-theme="pink" style="background: #ff99cc;"></div>
                </div>
                <form class="search-form" id="searchForm">
                    <span class="search-icon">🔍</span>
                    <input type="text" class="search-input" id="searchInput" placeholder="Поиск в Google...">
                    <button type="submit" class="search-btn">Найти</button>
                </form>
                <div class="icon-row" id="quickLinksContainer"></div>
            </div>
            
            <div class="right-area">
                <div class="right-icons">
                    <div class="lang-switch-btn" id="langSwitch">EN</div>
                    <div class="icon-circle" id="settingsIcon" title="Настройки">⚙️</div>
                </div>
                <div class="weather-static" id="weatherStatic">
                    <div class="temp" id="weatherTemp">--°C</div>
                    <div id="weatherDesc">Загрузка...</div>
                    <div class="city" id="weatherCity">Санкт-Петербург</div>
                </div>
            </div>
        </div>
        
        <div id="tilesContainer" class="tiles-grid"></div>
        
        <div class="radio-section">
            <div class="radio-player-box" id="playerContainer">
                <button class="play-btn" id="customPlayBtn">
                    <svg id="playIcon" class="icon" viewBox="0 0 24 24">
                        <path d="M8 5v14l11-7z"/>
                    </svg>
                </button>
                <div class="station-info">
                    <span class="station-name" id="currentStationDisplay">Выберите радио</span>
                    <span class="station-status" id="statusText">Off-line</span>
                </div>
                <input type="range" id="volumeControl" class="volume-slider" min="0" max="1" step="0.05" value="0.5">
            </div>
            
            <div id="radioList" class="radio-grid"></div>
        </div>
        
        <div class="github-link">
            <a href="https://github.com/blog1703/Steam-Overlay-SpeedDial" target="_blank">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="20" height="20">
                    <path fill="currentColor" d="M12 0C5.37 0 0 5.37 0 12c0 5.3 3.438 9.8 8.205 11.387.6.113.82-.26.82-.58 0-.287-.01-1.05-.015-2.06-3.338.726-4.042-1.61-4.042-1.61-.546-1.387-1.333-1.756-1.333-1.756-1.09-.745.082-.73.082-.73 1.205.085 1.838 1.237 1.838 1.237 1.07 1.834 2.807 1.304 3.492.997.108-.775.418-1.305.762-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.4 3-.405 1.02.005 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 21.795 24 17.295 24 12c0-6.63-5.37-12-12-12z"/>
                </svg>
                Исходный код на GitHub
            </a>
        </div>
    </div>

    <script>
        // ============================================================
        // ========== 1. ПОГОДА (ручной выбор города) ==========
        // ============================================================
        let weatherCity = localStorage.getItem('steam_weather_city') || "Санкт-Петербург";
        
        async function fetchWeather() {
            try {
                const url = `https://wttr.in/${encodeURIComponent(weatherCity)}?format=%t+%c+%C&lang=ru`;
                const response = await fetch(url);
                const data = await response.text();
                const tempMatch = data.match(/(-?\d+°C)/);
                const descMatch = data.match(/[+-]?\d+°C\s+(.+)/);
                
                if (tempMatch) document.getElementById('weatherTemp').textContent = tempMatch[0];
                if (descMatch) document.getElementById('weatherDesc').textContent = descMatch[1];
                document.getElementById('weatherCity').textContent = weatherCity;
            } catch(e) {
                console.error('Weather error:', e);
                document.getElementById('weatherDesc').textContent = 'Ошибка загрузки';
            }
        }
        
        // ============================================================
        // ========== 2. DOM ЭЛЕМЕНТЫ ==========
        // ============================================================
        const dom = {
            tilesContainer: document.getElementById('tilesContainer'),
            radioList: document.getElementById('radioList'),
            radioPlayer: null,
            currentStationDisplay: document.getElementById('currentStationDisplay'),
            statusText: document.getElementById('statusText'),
            customPlayBtn: document.getElementById('customPlayBtn'),
            playIcon: document.getElementById('playIcon'),
            volumeControl: document.getElementById('volumeControl'),
            playerContainer: document.getElementById('playerContainer'),
            searchInput: document.getElementById('searchInput'),
            searchForm: document.getElementById('searchForm'),
            langSwitch: document.getElementById('langSwitch'),
            settingsIcon: document.getElementById('settingsIcon'),
            floatBtn: document.getElementById('radioFloatBtn'),
            floatIcon: document.getElementById('floatIcon'),
            quickLinksContainer: document.getElementById('quickLinksContainer')
        };
        
        const audioElement = new Audio();
        dom.radioPlayer = audioElement;
        
        // ============================================================
        // ========== 3. СОСТОЯНИЕ ==========
        // ============================================================
        const state = {
            bookmarks: [],
            quickLinks: [],
            radioStations: [],
            currentRadio: null,
            currentLang: 'ru',
            currentTheme: 'default',
            isDragging: false,
            dragStartX: 0,
            dragStartY: 0,
            startLeft: 0,
            startTop: 0,
            isPlaying: false,
            remindersEnabled: true,
            lastReminderShown: 0
        };
        
        let reminderTimeout = null;
        
        // ============================================================
        // ========== 4. ПЕРЕВОДЫ ==========
        // ============================================================
        const translations = {
            ru: {
                searchPlaceholder: "Поиск в Google...", searchBtn: "Найти",
                noBookmarks: "Нет закладок. Добавьте первую плитку!",
                noRadio: "Нет радиостанций. Нажмите \"+\" чтобы добавить",
                selectRadio: "Выберите радио",
                deleteConfirm: "Удалить радиостанцию", deleteYes: "Да, удалить", deleteNo: "Отмена",
                stationDeleted: "Радиостанция удалена", fillFields: "Заполните название и ссылку",
                added: "✓ Плитка добавлена", invalidUrl: "Некорректная ссылка",
                exportDone: "Экспорт выполнен", importSuccess: "Импорт успешен!", importError: "Ошибка при импорте",
                resetConfirm: "Удалить все закладки?", resetDone: "Все закладки удалены",
                radioAdded: "✓ Радио добавлено",
                addQuickLinkTitle: "Добавить ярлык", addQuickLinkName: "Название", addQuickLinkUrl: "Ссылка на сервис (https://...)",
                addQuickLinkIcon: "Прямая ссылка на иконку PNG", addQuickLinkSave: "Добавить", addQuickLinkCancel: "Отмена",
                quickLinkAdded: "✓ Ярлык добавлен", quickLinkDelete: "Ярлык удален",
                addTileTitle: "Добавить плитку", addTileName: "Название сайта", addTileUrl: "Ссылка (https://...)",
                addTileSave: "Добавить", addTileCancel: "Отмена",
                exportFull: "📦 Полный экспорт (все данные)", importFull: "📦 Полный импорт (все данные)",
                addRadioTitle: "Добавить радиостанцию", addRadioName: "Название радио", addRadioUrl: "Ссылка на поток",
                addRadioSave: "Добавить", addRadioCancel: "Отмена",
                onAir: "On Air", paused: "Paused", offline: "Off-line",
                remindersToggle: "🔔 Показывать напоминания о экспорте",
                reminderTitle: "⚠️ Сохраните настройки!", reminderText: "Экспортируйте, чтобы не потерять: закладки, ярлыки и радио",
                reminderHint: "⚙️ Настройки → 📦 Полный экспорт", reminderClose: "Закрыть",
                weatherCityLabel: "🌍 Город для погоды",
                saveCity: "💾 Сохранить город"
            },
            en: {
                searchPlaceholder: "Search Google...", searchBtn: "Search",
                noBookmarks: "No bookmarks. Add your first tile!",
                noRadio: "No radio stations. Click \"+\" to add",
                selectRadio: "Select radio",
                deleteConfirm: "Delete radio station", deleteYes: "Yes, delete", deleteNo: "Cancel",
                stationDeleted: "Radio station deleted", fillFields: "Fill in name and URL",
                added: "✓ Tile added", invalidUrl: "Invalid URL",
                exportDone: "Export completed", importSuccess: "Import successful!", importError: "Import error",
                resetConfirm: "Delete all bookmarks?", resetDone: "All bookmarks deleted",
                radioAdded: "✓ Radio added",
                addQuickLinkTitle: "Add shortcut", addQuickLinkName: "Name", addQuickLinkUrl: "URL (https://...)",
                addQuickLinkIcon: "Direct link to PNG icon", addQuickLinkSave: "Add", addQuickLinkCancel: "Cancel",
                quickLinkAdded: "✓ Shortcut added", quickLinkDelete: "Shortcut deleted",
                addTileTitle: "Add tile", addTileName: "Site name", addTileUrl: "URL (https://...)",
                addTileSave: "Add", addTileCancel: "Cancel",
                exportFull: "📦 Full export (all data)", importFull: "📦 Full import (all data)",
                addRadioTitle: "Add radio station", addRadioName: "Radio name", addRadioUrl: "Stream URL",
                addRadioSave: "Add", addRadioCancel: "Cancel",
                onAir: "On Air", paused: "Paused", offline: "Off-line",
                remindersToggle: "🔔 Show export reminders",
                reminderTitle: "⚠️ Save your settings!", reminderText: "Export to keep: bookmarks, shortcuts & radio",
                reminderHint: "⚙️ Settings → 📦 Full export", reminderClose: "Close",
                weatherCityLabel: "🌍 Weather city",
                saveCity: "💾 Save city"
            }
        };
        
        function t(key) { return translations[state.currentLang][key] || key; }
        
        function showToast(msg) {
            const existing = document.querySelectorAll('.toast');
            if (existing.length > 3) existing[0]?.remove();
            const toast = document.createElement('div');
            toast.className = 'toast';
            toast.textContent = msg;
            document.body.appendChild(toast);
            setTimeout(() => toast.remove(), 2000);
        }
        
        // ============================================================
        // ========== 5. НАПОМИНАНИЕ ==========
        // ============================================================
        function showExportReminder() {
            if (!state.remindersEnabled) return;
            if (reminderTimeout) return;
            
            const now = Date.now();
            if (now - state.lastReminderShown < 30000) return;
            state.lastReminderShown = now;
            
            const existing = document.querySelector('.export-reminder');
            if (existing) existing.remove();
            
            const reminder = document.createElement('div');
            reminder.className = 'export-reminder';
            reminder.innerHTML = `
                <div class="reminder-content">
                    <div class="reminder-icon">⚠️</div>
                    <div class="reminder-text">
                        <strong>${t('reminderTitle')}</strong><br>
                        ${t('reminderText')}<br>
                        <div class="reminder-hint">${t('reminderHint')}</div>
                    </div>
                    <div class="reminder-close" title="${t('reminderClose')}">✕</div>
                </div>
            `;
            document.body.appendChild(reminder);
            
            reminder.querySelector('.reminder-close').onclick = () => {
                reminder.remove();
                if (reminderTimeout) clearTimeout(reminderTimeout);
                reminderTimeout = null;
            };
            
            reminderTimeout = setTimeout(() => {
                if (reminder && reminder.parentNode) reminder.remove();
                reminderTimeout = null;
            }, 8000);
        }
        
        // ============================================================
        // ========== 6. ТЕМЫ ==========
        // ============================================================
        function setTheme(theme) {
            state.currentTheme = theme;
            document.body.setAttribute('data-theme', theme);
            localStorage.setItem('dashboard_theme', theme);
            document.querySelectorAll('.theme-dot').forEach(dot => {
                if (dot.dataset.theme === theme) dot.classList.add('active');
                else dot.classList.remove('active');
            });
            updateRadioIconColor();
        }
        
        function updateRadioIconColor() {
            const colors = { default: '66C0F4', minimalism: 'FF6B6B', amber: 'FFAA33', pink: 'FF99CC' };
            const color = colors[state.currentTheme] || '66C0F4';
            if (state.isPlaying) {
                dom.floatIcon.src = `https://img.icons8.com/?size=100&id=EEH2MmtvmpiW&format=png&color=${color}`;
                dom.floatIcon.classList.add('pulsing');
            } else {
                dom.floatIcon.src = `https://img.icons8.com/?size=100&id=ZfzQA6FzEiPP&format=png&color=${color}`;
                dom.floatIcon.classList.remove('pulsing');
            }
        }
        
        function loadTheme() {
            const saved = localStorage.getItem('dashboard_theme');
            if (saved && (saved === 'default' || saved === 'minimalism' || saved === 'amber' || saved === 'pink')) {
                setTheme(saved);
            } else {
                setTheme('default');
            }
        }
        
        // ============================================================
        // ========== 7. НАСТРОЙКИ ==========
        // ============================================================
        function exportFullData() {
            const exportData = {
                version: "1.2",
                exportDate: new Date().toISOString(),
                bookmarks: state.bookmarks,
                quickLinks: state.quickLinks,
                radioStations: state.radioStations,
                theme: state.currentTheme,
                language: state.currentLang,
                remindersEnabled: state.remindersEnabled,
                weatherCity: weatherCity
            };
            const a = document.createElement('a');
            a.href = URL.createObjectURL(new Blob([JSON.stringify(exportData, null, 2)], {type:'application/json'}));
            a.download = `steam_dashboard_backup_${new Date().toISOString().slice(0,19).replace(/:/g, '-')}.json`;
            a.click();
            URL.revokeObjectURL(a.href);
            showToast(t('exportDone'));
        }
        
        function importFullData() {
            const input = document.createElement('input');
            input.type = 'file';
            input.accept = 'application/json';
            input.onchange = (e) => {
                const file = e.target.files[0];
                const reader = new FileReader();
                reader.onload = (ev) => {
                    try {
                        const imported = JSON.parse(ev.target.result);
                        if (imported.bookmarks) { state.bookmarks = imported.bookmarks; saveData(); render(); }
                        if (imported.quickLinks) { state.quickLinks = imported.quickLinks; saveQuickLinks(); renderQuickLinks(); }
                        if (imported.radioStations) { state.radioStations = imported.radioStations; saveRadio(); renderRadio(); }
                        if (imported.theme) setTheme(imported.theme);
                        if (imported.language) { state.currentLang = imported.language; localStorage.setItem('steam_language', state.currentLang); updateLanguage(); }
                        if (typeof imported.remindersEnabled === 'boolean') { state.remindersEnabled = imported.remindersEnabled; localStorage.setItem('steam_reminders', state.remindersEnabled); }
                        if (imported.weatherCity) { 
                            weatherCity = imported.weatherCity; 
                            localStorage.setItem('steam_weather_city', weatherCity);
                            fetchWeather();
                        }
                        showToast(t('importSuccess'));
                    } catch(err) { showToast(t('importError')); }
                };
                reader.readAsText(file);
            };
            input.click();
        }
        
        function resetAllData() {
            if (confirm(t('resetConfirm'))) {
                state.bookmarks = [];
                state.quickLinks = [];
                state.radioStations = [];
                state.currentRadio = null;
                setTheme('default');
                weatherCity = "Санкт-Петербург";
                localStorage.setItem('steam_weather_city', weatherCity);
                fetchWeather();
                saveData(); saveQuickLinks(); saveRadio();
                render(); renderQuickLinks(); renderRadio();
                audioElement.pause();
                audioElement.src = '';
                audioElement.load();
                state.isPlaying = false;
                state.currentRadio = null;
                dom.currentStationDisplay.textContent = t('selectRadio');
                dom.statusText.textContent = t('offline');
                dom.playerContainer.classList.remove('playing');
                dom.playIcon.innerHTML = '<path d="M8 5v14l11-7z"/>';
                updateRadioIconColor();
                showToast(t('resetDone'));
            }
        }
        
        function showSettings() {
            const modal = document.createElement('div');
            modal.className = 'settings-modal';
            modal.innerHTML = `
                <div class="settings-dialog">
                    <h3>⚙️ ${state.currentLang === 'ru' ? 'Настройки' : 'Settings'}</h3>
                    <div class="settings-buttons">
                        <button class="settings-btn" id="settingsExportBtn" style="padding:12px;margin:5px;background:#2a475e;border:none;border-radius:10px;color:white;cursor:pointer;">📦 ${t('exportFull')}</button>
                        <button class="settings-btn" id="settingsImportBtn" style="padding:12px;margin:5px;background:#2a475e;border:none;border-radius:10px;color:white;cursor:pointer;">📦 ${t('importFull')}</button>
                        <button class="settings-btn danger" id="settingsResetBtn" style="padding:12px;margin:5px;background:#8b3c3c;border:none;border-radius:10px;color:white;cursor:pointer;">🗑️ ${state.currentLang === 'ru' ? 'Сбросить всё' : 'Reset all'}</button>
                        <div style="display:flex;align-items:center;justify-content:space-between;margin-top:10px;padding:8px 12px;background:rgba(102,192,244,0.1);border-radius:10px;">
                            <span>${t('remindersToggle')}</span>
                            <input type="checkbox" id="remindersToggleCheckbox" ${state.remindersEnabled ? 'checked' : ''} style="width:20px;height:20px;cursor:pointer;">
                        </div>
                        <div style="margin-top:15px;padding:8px 12px;background:rgba(102,192,244,0.1);border-radius:10px;">
                            <div style="margin-bottom:8px;">${t('weatherCityLabel')}</div>
                            <input type="text" id="weatherCityInput" value="${weatherCity}" style="width:100%;padding:8px;border-radius:8px;background:rgba(0,0,0,0.5);border:1px solid rgba(255,255,255,0.2);color:white;margin-bottom:8px;">
                            <button id="saveCityBtn" style="padding:8px 16px;background:#66c0f4;border:none;border-radius:8px;cursor:pointer;color:#1a1a2e;">${t('saveCity')}</button>
                        </div>
                    </div>
                    <button class="close-settings" id="closeSettingsBtn" style="margin-top:15px;padding:10px;background:transparent;border:1px solid #66c0f4;border-radius:10px;color:#66c0f4;cursor:pointer;">${state.currentLang === 'ru' ? 'Закрыть' : 'Close'}</button>
                </div>
            `;
            document.body.appendChild(modal);
            
            const toggleCheckbox = modal.querySelector('#remindersToggleCheckbox');
            toggleCheckbox.onchange = (e) => {
                state.remindersEnabled = e.target.checked;
                localStorage.setItem('steam_reminders', state.remindersEnabled);
            };
            
            const cityInput = modal.querySelector('#weatherCityInput');
            const saveCityBtn = modal.querySelector('#saveCityBtn');
            saveCityBtn.onclick = () => {
                const newCity = cityInput.value.trim();
                if (newCity) {
                    weatherCity = newCity;
                    localStorage.setItem('steam_weather_city', weatherCity);
                    fetchWeather();
                    showToast(`Город изменён на ${weatherCity}`);
                }
            };
            
            modal.querySelector('#settingsExportBtn').onclick = () => { exportFullData(); modal.remove(); };
            modal.querySelector('#settingsImportBtn').onclick = () => { importFullData(); modal.remove(); };
            modal.querySelector('#settingsResetBtn').onclick = () => { resetAllData(); modal.remove(); };
            modal.querySelector('#closeSettingsBtn').onclick = () => modal.remove();
            modal.onclick = (e) => { if (e.target === modal) modal.remove(); };
        }
        
        // ============================================================
        // ========== 8. ЯРЛЫКИ ==========
        // ============================================================
        function loadQuickLinks() {
            const saved = localStorage.getItem('steam_quicklinks');
            if (saved) state.quickLinks = JSON.parse(saved);
            if (!state.quickLinks || state.quickLinks.length === 0) {
                state.quickLinks = [
                    { id: crypto.randomUUID(), name: "Telegram", url: "https://web.telegram.org/k/", icon: "https://img.icons8.com/?size=100&id=lUktdBVdL4Kb&format=png&color=FF005A" },
                    { id: crypto.randomUUID(), name: "Discord", url: "https://discord.com/app", icon: "https://img.icons8.com/?size=100&id=30888&format=png&color=FF005A" },
                    { id: crypto.randomUUID(), name: "YouTube Music", url: "https://music.youtube.com/", icon: "https://img.icons8.com/?size=100&id=Mw6P3tmWMfOB&format=png&color=FF005A" },
                    { id: crypto.randomUUID(), name: "Yandex Music", url: "https://music.yandex.ru/", icon: "https://img.icons8.com/?size=100&id=DGHVPGM9dSp4&format=png&color=FF005A" },
                    { id: crypto.randomUUID(), name: "VK", url: "https://vk.com/", icon: "https://img.icons8.com/?size=100&id=60454&format=png&color=FF005A" }
                ];
                saveQuickLinks();
            }
            renderQuickLinks();
        }
        
        function saveQuickLinks() { localStorage.setItem('steam_quicklinks', JSON.stringify(state.quickLinks)); }
        
        function addQuickLink(name, url, iconUrl) {
            if (!name || !url) { showToast(t('fillFields')); return false; }
            if (!url.startsWith('http')) url = 'https://' + url;
            try {
                new URL(url);
                state.quickLinks.push({ id: crypto.randomUUID(), name: name, url: url, icon: iconUrl || 'https://img.icons8.com/?size=100&id=9js2jhDqLr76&format=png&color=FF005A' });
                saveQuickLinks();
                renderQuickLinks();
                showExportReminder();
                return true;
            } catch(e) { showToast(t('invalidUrl')); return false; }
        }
        
        function deleteQuickLink(id) {
            state.quickLinks = state.quickLinks.filter(link => link.id !== id);
            saveQuickLinks();
            renderQuickLinks();
            showToast(t('quickLinkDelete'));
        }
        
        function renderQuickLinks() {
            if (!dom.quickLinksContainer) return;
            const fragment = document.createDocumentFragment();
            state.quickLinks.forEach((link, idx) => {
                const wrapper = document.createElement('div');
                wrapper.style.position = 'relative';
                wrapper.style.display = 'inline-flex';
                const a = document.createElement('a');
                a.href = link.url;
                a.target = '_blank';
                a.className = 'pure-icon';
                a.title = link.name;
                const img = document.createElement('img');
                img.src = link.icon;
                img.alt = link.name;
                img.onerror = () => img.src = 'https://img.icons8.com/?size=100&id=9js2jhDqLr76&format=png&color=FF005A';
                a.appendChild(img);
                
                const deleteBtn = document.createElement('div');
                deleteBtn.className = 'delete-tile';
                deleteBtn.style.position = 'absolute';
                deleteBtn.style.top = '4px';
                deleteBtn.style.right = '4px';
                deleteBtn.style.width = '22px';
                deleteBtn.style.height = '22px';
                deleteBtn.style.background = 'rgba(0,0,0,0.7)';
                deleteBtn.style.borderRadius = '50%';
                deleteBtn.style.cursor = 'pointer';
                deleteBtn.style.opacity = '0';
                deleteBtn.style.transition = 'opacity 0.2s';
                deleteBtn.style.display = 'flex';
                deleteBtn.style.alignItems = 'center';
                deleteBtn.style.justifyContent = 'center';
                
                const svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
                svg.setAttribute('viewBox', '0 0 24 24');
                svg.setAttribute('fill', 'none');
                svg.style.width = '14px';
                svg.style.height = '14px';
                const path = document.createElementNS('http://www.w3.org/2000/svg', 'path');
                path.setAttribute('d', 'M18 6L6 18M6 6L18 18');
                path.setAttribute('stroke', '#ff8a8a');
                path.setAttribute('stroke-width', '2');
                path.setAttribute('stroke-linecap', 'round');
                svg.appendChild(path);
                deleteBtn.appendChild(svg);
                
                deleteBtn.onclick = (e) => { e.preventDefault(); e.stopPropagation(); deleteQuickLink(link.id); };
                wrapper.appendChild(a);
                wrapper.appendChild(deleteBtn);
                wrapper.onmouseenter = () => { deleteBtn.style.opacity = '1'; };
                wrapper.onmouseleave = () => { deleteBtn.style.opacity = '0'; };
                fragment.appendChild(wrapper);
            });
            const addBtn = document.createElement('div');
            addBtn.className = 'add-icon-circle';
            const svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
            svg.setAttribute('viewBox', '0 0 24 24');
            svg.setAttribute('fill', 'none');
            const path = document.createElementNS('http://www.w3.org/2000/svg', 'path');
            path.setAttribute('d', 'M12 4V20M20 12H4');
            path.setAttribute('stroke', 'currentColor');
            path.setAttribute('stroke-width', '2');
            svg.appendChild(path);
            addBtn.appendChild(svg);
            addBtn.title = t('addQuickLinkTitle');
            addBtn.onclick = () => { showAddQuickLinkModal(); };
            fragment.appendChild(addBtn);
            dom.quickLinksContainer.innerHTML = '';
            dom.quickLinksContainer.appendChild(fragment);
        }
        
        function showAddQuickLinkModal() {
            const modal = document.createElement('div');
            modal.className = 'modal-overlay';
            modal.innerHTML = `
                <div class="modal-dialog">
                    <h3>${t('addQuickLinkTitle')}</h3>
                    <input type="text" id="modalQuickName" placeholder="${t('addQuickLinkName')}" autocomplete="off" style="width:100%;padding:12px;margin-bottom:15px;background:rgba(0,0,0,0.5);border:1px solid rgba(255,255,255,0.2);border-radius:12px;color:white;">
                    <input type="text" id="modalQuickUrl" placeholder="${t('addQuickLinkUrl')}" autocomplete="off" style="width:100%;padding:12px;margin-bottom:15px;background:rgba(0,0,0,0.5);border:1px solid rgba(255,255,255,0.2);border-radius:12px;color:white;">
                    <input type="text" id="modalQuickIcon" placeholder="${t('addQuickLinkIcon')}" autocomplete="off" style="width:100%;padding:12px;margin-bottom:15px;background:rgba(0,0,0,0.5);border:1px solid rgba(255,255,255,0.2);border-radius:12px;color:white;">
                    <div class="modal-buttons" style="display:flex;gap:15px;justify-content:center;">
                        <button id="modalSaveBtn" class="modal-save" style="padding:12px 25px;background:#66c0f4;border:none;border-radius:30px;cursor:pointer;">${t('addQuickLinkSave')}</button>
                        <button id="modalCancelBtn" class="modal-cancel" style="padding:12px 25px;background:#8b3c3c;border:none;border-radius:30px;cursor:pointer;">${t('addQuickLinkCancel')}</button>
                    </div>
                </div>
            `;
            document.body.appendChild(modal);
            const nameInput = modal.querySelector('#modalQuickName');
            const urlInput = modal.querySelector('#modalQuickUrl');
            const iconInput = modal.querySelector('#modalQuickIcon');
            const saveBtn = modal.querySelector('#modalSaveBtn');
            const cancelBtn = modal.querySelector('#modalCancelBtn');
            const closeModal = () => modal.remove();
            saveBtn.onclick = () => {
                const name = nameInput.value.trim();
                const url = urlInput.value.trim();
                const icon = iconInput.value.trim();
                if (addQuickLink(name, url, icon)) {
                    closeModal();
                    showToast(t('quickLinkAdded'));
                }
            };
            cancelBtn.onclick = closeModal;
            modal.onclick = (e) => { if (e.target === modal) closeModal(); };
        }
        
        // ============================================================
        // ========== 9. РАДИО ==========
        // ============================================================
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
        
        function stopRadio() {
            state.isPlaying = false;
            state.currentRadio = null;
            audioElement.pause();
            audioElement.src = '';
            audioElement.load();
            dom.currentStationDisplay.textContent = t('selectRadio');
            dom.statusText.textContent = t('offline');
            dom.playerContainer.classList.remove('playing');
            dom.playIcon.innerHTML = '<path d="M8 5v14l11-7z"/>';
            updateRadioIconColor();
        }
        
        function playRadio(station) {
            state.currentRadio = station;
            state.isPlaying = true;
            audioElement.src = station.url;
            dom.currentStationDisplay.textContent = station.name;
            dom.statusText.textContent = t('onAir');
            dom.playerContainer.classList.add('playing');
            dom.playIcon.innerHTML = '<path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z"/>';
            audioElement.play().catch(e => {
                state.isPlaying = false;
                dom.statusText.textContent = t('offline');
                dom.playerContainer.classList.remove('playing');
                dom.playIcon.innerHTML = '<path d="M8 5v14l11-7z"/>';
            });
            renderRadio();
            updateRadioIconColor();
        }
        
        function togglePlayPause() {
            if (!state.currentRadio && state.radioStations.length > 0) {
                playRadio(state.radioStations[0]);
                return;
            }
            if (state.isPlaying) {
                audioElement.pause();
                state.isPlaying = false;
                dom.statusText.textContent = t('paused');
                dom.playerContainer.classList.remove('playing');
                dom.playIcon.innerHTML = '<path d="M8 5v14l11-7z"/>';
                updateRadioIconColor();
            } else {
                if (state.currentRadio) {
                    audioElement.play().then(() => {
                        state.isPlaying = true;
                        dom.statusText.textContent = t('onAir');
                        dom.playerContainer.classList.add('playing');
                        dom.playIcon.innerHTML = '<path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z"/>';
                        updateRadioIconColor();
                    }).catch(e => { if (state.currentRadio) playRadio(state.currentRadio); });
                } else if (state.radioStations.length > 0) {
                    playRadio(state.radioStations[0]);
                } else {
                    showToast(t('noRadio'));
                }
            }
        }
        
        function confirmDeleteRadio(station) {
            const overlay = document.createElement('div');
            overlay.className = 'confirm-overlay';
            overlay.innerHTML = `
                <div class="confirm-dialog">
                    <p>${t('deleteConfirm')} <span class="station-name">${escapeHtml(station.name)}</span>?</p>
                    <div class="confirm-buttons" style="display:flex;gap:15px;justify-content:center;">
                        <button class="confirm-btn confirm-yes" id="confirmYes" style="padding:8px 25px;background:#8b3c3c;border:none;border-radius:8px;color:white;cursor:pointer;">${t('deleteYes')}</button>
                        <button class="confirm-btn confirm-no" id="confirmNo" style="padding:8px 25px;background:#2a475e;border:none;border-radius:8px;color:white;cursor:pointer;">${t('deleteNo')}</button>
                    </div>
                </div>
            `;
            document.body.appendChild(overlay);
            const yesBtn = document.getElementById('confirmYes');
            const noBtn = document.getElementById('confirmNo');
            const handleYes = () => {
                state.radioStations = state.radioStations.filter(s => s.id !== station.id);
                saveRadio();
                if (state.currentRadio && state.currentRadio.id === station.id) stopRadio();
                renderRadio();
                showToast(t('stationDeleted'));
                overlay.remove();
            };
            const handleNo = () => overlay.remove();
            yesBtn?.addEventListener('click', handleYes, { once: true });
            noBtn?.addEventListener('click', handleNo, { once: true });
            overlay.addEventListener('click', (e) => { if (e.target === overlay) overlay.remove(); });
        }
        
        function addRadio(name, url) {
            if (!name || !url) { showToast(t('fillFields')); return false; }
            if (!url.startsWith('http')) url = 'https://' + url;
            state.radioStations.push({ id: crypto.randomUUID(), name: name, url: url });
            saveRadio();
            renderRadio();
            showExportReminder();
            return true;
        }
        
        function showAddRadioModal() {
            const modal = document.createElement('div');
            modal.className = 'modal-overlay';
            modal.innerHTML = `
                <div class="modal-dialog">
                    <h3>${t('addRadioTitle')}</h3>
                    <input type="text" id="modalRadioName" placeholder="${t('addRadioName')}" autocomplete="off" style="width:100%;padding:12px;margin-bottom:15px;background:rgba(0,0,0,0.5);border:1px solid rgba(255,255,255,0.2);border-radius:12px;color:white;">
                    <input type="text" id="modalRadioUrl" placeholder="${t('addRadioUrl')}" autocomplete="off" style="width:100%;padding:12px;margin-bottom:15px;background:rgba(0,0,0,0.5);border:1px solid rgba(255,255,255,0.2);border-radius:12px;color:white;">
                    <div class="modal-buttons" style="display:flex;gap:15px;justify-content:center;">
                        <button id="modalSaveBtn" class="modal-save" style="padding:12px 25px;background:#66c0f4;border:none;border-radius:30px;cursor:pointer;">${t('addRadioSave')}</button>
                        <button id="modalCancelBtn" class="modal-cancel" style="padding:12px 25px;background:#8b3c3c;border:none;border-radius:30px;cursor:pointer;">${t('addRadioCancel')}</button>
                    </div>
                </div>
            `;
            document.body.appendChild(modal);
            const nameInput = modal.querySelector('#modalRadioName');
            const urlInput = modal.querySelector('#modalRadioUrl');
            const saveBtn = modal.querySelector('#modalSaveBtn');
            const cancelBtn = modal.querySelector('#modalCancelBtn');
            const closeModal = () => modal.remove();
            saveBtn.onclick = () => {
                const name = nameInput.value.trim();
                const url = urlInput.value.trim();
                if (addRadio(name, url)) { closeModal(); showToast(t('radioAdded')); }
            };
            cancelBtn.onclick = closeModal;
            modal.onclick = (e) => { if (e.target === modal) closeModal(); };
        }
        
        function renderRadio() {
            if (!dom.radioList) return;
            const fragment = document.createDocumentFragment();
            state.radioStations.forEach((station, idx) => {
                const card = document.createElement('div');
                card.className = 'radio-card';
                if (state.currentRadio && state.currentRadio.id === station.id) card.classList.add('active');
                card.onclick = () => playRadio(station);
                card.innerHTML = `
                    <div class="radio-icon" style="width:32px;height:32px;"><img src="https://img.icons8.com/?size=100&id=9js2jhDqLr76&format=png&color=000000" style="width:28px;height:28px;"></div>
                    <div class="radio-info" style="flex:1;"><div class="radio-name" style="font-size:14px;font-weight:500;">${escapeHtml(station.name)}</div><div class="radio-url" style="font-size:10px;opacity:0.6;">${escapeHtml(station.url.substring(0, 50))}${station.url.length > 50 ? '...' : ''}</div></div>
                    <div class="delete-radio"></div>
                `;
                const deleteBtn = card.querySelector('.delete-radio');
                const svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
                svg.setAttribute('viewBox', '0 0 24 24');
                svg.setAttribute('fill', 'none');
                svg.style.width = '14px';
                svg.style.height = '14px';
                const path = document.createElementNS('http://www.w3.org/2000/svg', 'path');
                path.setAttribute('d', 'M18 6L6 18M6 6L18 18');
                path.setAttribute('stroke', '#ff8a8a');
                path.setAttribute('stroke-width', '2');
                path.setAttribute('stroke-linecap', 'round');
                svg.appendChild(path);
                deleteBtn.appendChild(svg);
                deleteBtn.onclick = (e) => { e.stopPropagation(); confirmDeleteRadio(station); };
                fragment.appendChild(card);
            });
            const addCard = document.createElement('div');
            addCard.className = 'add-radio-card';
            addCard.onclick = () => showAddRadioModal();
            const svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
            svg.setAttribute('viewBox', '0 0 24 24');
            svg.setAttribute('fill', 'none');
            const path = document.createElementNS('http://www.w3.org/2000/svg', 'path');
            path.setAttribute('d', 'M12 4V20M20 12H4');
            path.setAttribute('stroke', 'currentColor');
            path.setAttribute('stroke-width', '2');
            svg.appendChild(path);
            addCard.appendChild(svg);
            fragment.appendChild(addCard);
            dom.radioList.innerHTML = '';
            dom.radioList.appendChild(fragment);
            if (state.radioStations.length === 0) {
                dom.radioList.innerHTML = `<div style="padding:20px;text-align:center;opacity:0.6;">${t('noRadio')}</div>`;
                dom.radioList.appendChild(addCard);
            }
        }
        
        // ============================================================
        // ========== 10. ЗАКЛАДКИ ==========
        // ============================================================
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
            state.bookmarks.forEach((item, idx) => {
                const tile = document.createElement('div');
                tile.className = 'tile';
                tile.dataset.index = idx;
                tile.addEventListener('click', (e) => { if (!e.target.closest('.delete-tile')) window.location.href = item.url; });
                const deleteBtn = document.createElement('div');
                deleteBtn.className = 'delete-tile';
                const svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
                svg.setAttribute('viewBox', '0 0 24 24');
                svg.setAttribute('fill', 'none');
                svg.style.width = '14px';
                svg.style.height = '14px';
                const path = document.createElementNS('http://www.w3.org/2000/svg', 'path');
                path.setAttribute('d', 'M18 6L6 18M6 6L18 18');
                path.setAttribute('stroke', '#ff8a8a');
                path.setAttribute('stroke-width', '2');
                svg.appendChild(path);
                deleteBtn.appendChild(svg);
                const img = document.createElement('img');
                img.src = getFavicon(item.url);
                img.onerror = () => img.src = 'https://www.google.com/s2/favicons?domain=steampowered.com';
                const titleDiv = document.createElement('div');
                titleDiv.className = 'title';
                titleDiv.textContent = escapeHtml(item.title);
                tile.appendChild(deleteBtn);
                tile.appendChild(img);
                tile.appendChild(titleDiv);
                fragment.appendChild(tile);
            });
            const addTile = document.createElement('div');
            addTile.className = 'add-tile-btn';
            const svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
            svg.setAttribute('viewBox', '0 0 24 24');
            svg.setAttribute('fill', 'none');
            const path = document.createElementNS('http://www.w3.org/2000/svg', 'path');
            path.setAttribute('d', 'M12 4V20M20 12H4');
            path.setAttribute('stroke', 'currentColor');
            path.setAttribute('stroke-width', '2');
            svg.appendChild(path);
            addTile.appendChild(svg);
            addTile.onclick = () => showAddTileModal();
            fragment.appendChild(addTile);
            dom.tilesContainer.innerHTML = '';
            dom.tilesContainer.appendChild(fragment);
            if (state.bookmarks.length === 0 && fragment.children.length === 1) {
                dom.tilesContainer.innerHTML = `<div style="text-align:center;padding:50px;">${t('noBookmarks')}</div>`;
                dom.tilesContainer.appendChild(addTile);
            }
        }
        
        function showAddTileModal() {
            const modal = document.createElement('div');
            modal.className = 'modal-overlay';
            modal.innerHTML = `
                <div class="modal-dialog">
                    <h3>${t('addTileTitle')}</h3>
                    <input type="text" id="modalTileName" placeholder="${t('addTileName')}" autocomplete="off" style="width:100%;padding:12px;margin-bottom:15px;background:rgba(0,0,0,0.5);border:1px solid rgba(255,255,255,0.2);border-radius:12px;color:white;">
                    <input type="text" id="modalTileUrl" placeholder="${t('addTileUrl')}" autocomplete="off" style="width:100%;padding:12px;margin-bottom:15px;background:rgba(0,0,0,0.5);border:1px solid rgba(255,255,255,0.2);border-radius:12px;color:white;">
                    <div class="modal-buttons" style="display:flex;gap:15px;justify-content:center;">
                        <button id="modalSaveBtn" class="modal-save" style="padding:12px 25px;background:#66c0f4;border:none;border-radius:30px;cursor:pointer;">${t('addTileSave')}</button>
                        <button id="modalCancelBtn" class="modal-cancel" style="padding:12px 25px;background:#8b3c3c;border:none;border-radius:30px;cursor:pointer;">${t('addTileCancel')}</button>
                    </div>
                </div>
            `;
            document.body.appendChild(modal);
            const nameInput = modal.querySelector('#modalTileName');
            const urlInput = modal.querySelector('#modalTileUrl');
            const saveBtn = modal.querySelector('#modalSaveBtn');
            const cancelBtn = modal.querySelector('#modalCancelBtn');
            const closeModal = () => modal.remove();
            saveBtn.onclick = () => {
                let title = nameInput.value.trim();
                let url = urlInput.value.trim();
                if (!title || !url) { showToast(t('fillFields')); return; }
                if (!url.startsWith('http')) url = 'https://' + url;
                try {
                    new URL(url);
                    state.bookmarks.push({ title, url });
                    saveData();
                    render();
                    showToast(t('added'));
                    showExportReminder();
                    closeModal();
                } catch(e) { showToast(t('invalidUrl')); }
            };
            cancelBtn.onclick = closeModal;
            modal.onclick = (e) => { if (e.target === modal) closeModal(); };
        }
        
        function deleteBookmark(index) {
            state.bookmarks.splice(index, 1);
            saveData();
            render();
            showToast('Плитка удалена');
        }
        
        dom.tilesContainer.addEventListener('click', (e) => {
            const deleteBtn = e.target.closest('.delete-tile');
            const tile = e.target.closest('.tile');
            if (!tile) return;
            if (deleteBtn) {
                const index = parseInt(tile.dataset.index);
                if (!isNaN(index)) deleteBookmark(index);
            }
        });
        
        // ============================================================
        // ========== 11. ПОИСК ==========
        // ============================================================
        function setupSearch() {
            dom.searchForm.onsubmit = (e) => {
                e.preventDefault();
                const q = dom.searchInput.value.trim();
                if (q) window.open(`https://www.google.com/search?q=${encodeURIComponent(q)}`, '_blank');
            };
        }
        
        // ============================================================
        // ========== 12. ЯЗЫК ==========
        // ============================================================
        function updateLanguage() {
            dom.searchInput.placeholder = t('searchPlaceholder');
            document.querySelector('.search-btn').textContent = t('searchBtn');
            dom.langSwitch.textContent = state.currentLang === 'ru' ? 'EN' : 'RU';
            if (state.currentRadio) {
                dom.currentStationDisplay.textContent = state.currentRadio.name;
                dom.statusText.textContent = state.isPlaying ? t('onAir') : t('paused');
            } else {
                dom.currentStationDisplay.textContent = t('selectRadio');
                dom.statusText.textContent = t('offline');
            }
        }
        
        function switchLanguage() {
            state.currentLang = state.currentLang === 'ru' ? 'en' : 'ru';
            localStorage.setItem('steam_language', state.currentLang);
            updateLanguage();
        }
        
        function loadLanguage() {
            const saved = localStorage.getItem('steam_language');
            if (saved === 'ru' || saved === 'en') state.currentLang = saved;
            updateLanguage();
        }
        
        // ============================================================
        // ========== 13. ПЕРЕНОСИМАЯ КНОПКА ==========
        // ============================================================
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
        
        function toggleRadioFromFloat(e) {
            if (state.isDragging) return;
            e.stopPropagation();
            togglePlayPause();
        }
        
        // ============================================================
        // ========== 14. ОЧИСТКА ПРИ ЗАКРЫТИИ ==========
        // ============================================================
        window.addEventListener('beforeunload', () => {
            if (audioElement) {
                audioElement.pause();
                audioElement.src = '';
                audioElement.load();
            }
            if (reminderTimeout) {
                clearTimeout(reminderTimeout);
                reminderTimeout = null;
            }
        });
        
        // ============================================================
        // ========== 15. ВСПОМОГАТЕЛЬНЫЕ ==========
        // ============================================================
        function escapeHtml(str) { if (!str) return ''; return str.replace(/[&<>]/g, m => ({'&':'&amp;','<':'&lt;','>':'&gt;'}[m])); }
        
        // ============================================================
        // ========== 16. ИНИЦИАЛИЗАЦИЯ ==========
        // ============================================================
        function init() {
            const savedReminders = localStorage.getItem('steam_reminders');
            if (savedReminders !== null) state.remindersEnabled = savedReminders === 'true';
            
            dom.langSwitch.onclick = switchLanguage;
            dom.settingsIcon.onclick = showSettings;
            dom.customPlayBtn.onclick = togglePlayPause;
            dom.volumeControl.addEventListener('input', (e) => { audioElement.volume = e.target.value; });
            document.querySelectorAll('.theme-dot').forEach(btn => { btn.onclick = () => setTheme(btn.dataset.theme); });
            
            dom.floatBtn.addEventListener('mousedown', onMouseDown);
            window.addEventListener('mousemove', onMouseMove);
            window.addEventListener('mouseup', onMouseUp);
            dom.floatBtn.addEventListener('click', toggleRadioFromFloat);
            
            audioElement.addEventListener('play', () => { state.isPlaying = true; updateRadioIconColor(); });
            audioElement.addEventListener('pause', () => { state.isPlaying = false; updateRadioIconColor(); });
            audioElement.addEventListener('ended', () => { state.isPlaying = false; updateRadioIconColor(); });
            
            loadLanguage();
            loadTheme();
            loadData();
            loadRadio();
            loadQuickLinks();
            setupSearch();
            loadButtonPosition();
            updateRadioIconColor();
            audioElement.volume = 0.5;
            fetchWeather();
            setInterval(fetchWeather, 600000);
        }
        
        init();
    </script>
</body>
</html>
```
</details>

## Author's note

The file is customizable: change icons, add your own widgets, and change the color scheme.
If you'd like to share your own version or improve the project, you're welcome to open source it.

**Enjoy using it!** 🎮

