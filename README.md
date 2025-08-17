<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Эпичное Приключение 2.0</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            touch-action: manipulation;
            image-rendering: pixelated;
            image-rendering: -moz-crisp-edges;
            image-rendering: crisp-edges;
        }
        
        body {
            overflow: hidden;
            background-color: #222;
            font-family: 'Arial', sans-serif;
            color: white;
            position: fixed;
            width: 100%;
            height: 100%;
        }
        
        #gameCanvas {
            display: block;
            width: 100%;
            height: 100%;
            background-color: #1a1a2e;
        }
        
        #uiContainer {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }
        
        #healthBar {
            position: absolute;
            top: 10px;
            left: 10px;
            width: 200px;
            height: 20px;
            background-color: #333;
            border: 2px solid #555;
            border-radius: 5px;
            overflow: hidden;
        }
        
        #healthFill {
            height: 100%;
            width: 100%;
            background: linear-gradient(to right, #ff0000, #ff3300);
            transition: width 0.3s;
        }
        
        #inventoryBtn {
            position: absolute;
            bottom: 20px;
            right: 20px;
            width: 60px;
            height: 60px;
            background-color: rgba(100, 100, 255, 0.7);
            border: 2px solid #aaa;
            border-radius: 50%;
            pointer-events: auto;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            color: white;
        }
        
        #joystickArea {
            position: absolute;
            bottom: 20px;
            left: 20px;
            width: 120px;
            height: 120px;
            pointer-events: auto;
        }
        
        #joystick {
            width: 50px;
            height: 50px;
            background-color: rgba(255, 255, 255, 0.5);
            border-radius: 50%;
            position: absolute;
            left: 35px;
            top: 35px;
        }
        
        #attackBtn {
            position: absolute;
            bottom: 20px;
            right: 100px;
            width: 60px;
            height: 60px;
            background-color: rgba(255, 100, 100, 0.7);
            border: 2px solid #aaa;
            border-radius: 50%;
            pointer-events: auto;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            color: white;
        }
        
        #inventory {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 90%;
            height: 70%;
            background-color: rgba(0, 0, 0, 0.9);
            border: 3px solid #555;
            border-radius: 10px;
            display: none;
            pointer-events: auto;
            padding: 15px;
            flex-direction: column;
            z-index: 10;
        }
        
        #inventoryTitle {
            text-align: center;
            margin-bottom: 15px;
            font-size: 24px;
            color: gold;
        }
        
        #inventoryClose {
            position: absolute;
            top: 10px;
            right: 10px;
            width: 30px;
            height: 30px;
            background-color: #f00;
            border: none;
            border-radius: 50%;
            color: white;
            font-weight: bold;
        }
        
        #inventorySlots {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            flex-grow: 1;
            overflow-y: auto;
        }
        
        .inventorySlot {
            width: 100%;
            aspect-ratio: 1;
            background-color: rgba(100, 100, 100, 0.5);
            border: 2px solid #777;
            border-radius: 5px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            position: relative;
        }
        
        .itemCount {
            position: absolute;
            bottom: 2px;
            right: 2px;
            font-size: 12px;
            background-color: rgba(0, 0, 0, 0.7);
            border-radius: 3px;
            padding: 0 3px;
        }
        
        #crafting {
            margin-top: 15px;
            background-color: rgba(50, 50, 50, 0.7);
            padding: 10px;
            border-radius: 5px;
        }
        
        #craftingTitle {
            margin-bottom: 10px;
            color: #aaf;
        }
        
        #craftingRecipes {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
        }
        
        .recipe {
            background-color: rgba(70, 70, 100, 0.5);
            padding: 8px;
            border-radius: 5px;
            font-size: 14px;
        }
        
        .craftBtn {
            background-color: #4CAF50;
            border: none;
            color: white;
            padding: 3px 6px;
            border-radius: 3px;
            margin-top: 5px;
            font-size: 12px;
        }
        
        #questLog {
            position: absolute;
            top: 50px;
            right: 10px;
            width: 200px;
            background-color: rgba(0, 0, 0, 0.7);
            border: 2px solid #555;
            border-radius: 5px;
            padding: 10px;
            pointer-events: auto;
        }
        
        #questTitle {
            color: gold;
            margin-bottom: 5px;
            font-size: 16px;
        }
        
        #questText {
            font-size: 14px;
        }
        
        #bossHealth {
            position: absolute;
            top: 40px;
            left: 10px;
            width: 200px;
            height: 15px;
            background-color: #333;
            border: 2px solid #800;
            border-radius: 5px;
            overflow: hidden;
            display: none;
        }
        
        #bossHealthFill {
            height: 100%;
            width: 100%;
            background: linear-gradient(to right, #800, #f00);
        }
        
        #bossName {
            position: absolute;
            top: 15px;
            left: 10px;
            color: #f00;
            font-weight: bold;
            display: none;
        }
        
        #achievementPopup {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(0, 0, 0, 0.9);
            border: 3px solid gold;
            border-radius: 10px;
            padding: 20px;
            text-align: center;
            display: none;
            pointer-events: none;
            z-index: 100;
        }
        
        #achievementTitle {
            color: gold;
            font-size: 20px;
            margin-bottom: 10px;
        }
        
        #achievementDesc {
            font-size: 16px;
        }
        
        #resourcesPanel {
            position: absolute;
            top: 10px;
            right: 10px;
            background-color: rgba(0, 0, 0, 0.7);
            border: 2px solid #555;
            border-radius: 5px;
            padding: 5px 10px;
            display: flex;
            flex-direction: column;
            gap: 5px;
        }
        
        .resource {
            display: flex;
            align-items: center;
            gap: 5px;
            font-size: 14px;
        }
        
        .resourceIcon {
            width: 16px;
            height: 16px;
            background-color: #aaa;
            border-radius: 3px;
        }
        
        #dayNightIndicator {
            position: absolute;
            top: 10px;
            left: 220px;
            width: 100px;
            height: 30px;
            background-color: rgba(0, 0, 0, 0.7);
            border: 2px solid #555;
            border-radius: 5px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 14px;
        }
        
        #miniMap {
            position: absolute;
            bottom: 150px;
            right: 10px;
            width: 100px;
            height: 100px;
            background-color: rgba(0, 0, 0, 0.5);
            border: 2px solid #555;
            border-radius: 5px;
            overflow: hidden;
        }
        
        #miniMapView {
            position: absolute;
            width: 100%;
            height: 100%;
        }
        
        #merchantBtn {
            position: absolute;
            bottom: 20px;
            right: 180px;
            width: 60px;
            height: 60px;
            background-color: rgba(255, 215, 0, 0.7);
            border: 2px solid #aaa;
            border-radius: 50%;
            pointer-events: auto;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            color: white;
        }
        
        #merchantUI {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 80%;
            height: 60%;
            background-color: rgba(0, 0, 0, 0.9);
            border: 3px solid gold;
            border-radius: 10px;
            display: none;
            pointer-events: auto;
            padding: 15px;
            flex-direction: column;
            z-index: 10;
        }
        
        #merchantTitle {
            text-align: center;
            margin-bottom: 15px;
            font-size: 24px;
            color: gold;
        }
        
        #merchantClose {
            position: absolute;
            top: 10px;
            right: 10px;
            width: 30px;
            height: 30px;
            background-color: #f00;
            border: none;
            border-radius: 50%;
            color: white;
            font-weight: bold;
        }
        
        #merchantItems {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            flex-grow: 1;
            overflow-y: auto;
        }
        
        .merchantItem {
            background-color: rgba(70, 70, 0, 0.5);
            padding: 10px;
            border-radius: 5px;
            text-align: center;
        }
        
        .buyBtn {
            background-color: #4CAF50;
            border: none;
            color: white;
            padding: 5px 10px;
            border-radius: 3px;
            margin-top: 5px;
            font-size: 14px;
        }
        
        #buildBtn {
            position: absolute;
            bottom: 80px;
            right: 20px;
            width: 60px;
            height: 60px;
            background-color: rgba(100, 255, 100, 0.7);
            border: 2px solid #aaa;
            border-radius: 50%;
            pointer-events: auto;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            color: white;
        }
        
        #buildUI {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 80%;
            height: 60%;
            background-color: rgba(0, 0, 0, 0.9);
            border: 3px solid #4CAF50;
            border-radius: 10px;
            display: none;
            pointer-events: auto;
            padding: 15px;
            flex-direction: column;
            z-index: 10;
        }
        
        #buildTitle {
            text-align: center;
            margin-bottom: 15px;
            font-size: 24px;
            color: #4CAF50;
        }
        
        #buildClose {
            position: absolute;
            top: 10px;
            right: 10px;
            width: 30px;
            height: 30px;
            background-color: #f00;
            border: none;
            border-radius: 50%;
            color: white;
            font-weight: bold;
        }
        
        #buildItems {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            flex-grow: 1;
            overflow-y: auto;
        }
        
        .buildItem {
            background-color: rgba(70, 100, 70, 0.5);
            padding: 10px;
            border-radius: 5px;
            text-align: center;
        }
        
        .buildBtn {
            background-color: #4CAF50;
            border: none;
            color: white;
            padding: 5px 10px;
            border-radius: 3px;
            margin-top: 5px;
            font-size: 14px;
        }
        
        #darkOverlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            display: none;
            pointer-events: none;
            z-index: 5;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas"></canvas>
    <div id="darkOverlay"></div>
    
    <div id="uiContainer">
        <div id="healthBar">
            <div id="healthFill"></div>
        </div>
        
        <div id="bossName">БОСС</div>
        <div id="bossHealth">
            <div id="bossHealthFill"></div>
        </div>
        
        <div id="dayNightIndicator">День</div>
        
        <div id="joystickArea">
            <div id="joystick"></div>
        </div>
        
        <div id="attackBtn">⚔️</div>
        <div id="buildBtn">🏗️</div>
        <div id="merchantBtn">💰</div>
        <div id="inventoryBtn">🎒</div>
        
        <div id="questLog">
            <div id="questTitle">Квест:</div>
            <div id="questText">Найдите 5 дерева</div>
        </div>
        
        <div id="resourcesPanel">
            <div class="resource">
                <div class="resourceIcon" style="background-color: #8B4513;"></div>
                <span id="woodCount">0</span>
            </div>
            <div class="resource">
                <div class="resourceIcon" style="background-color: #808080;"></div>
                <span id="stoneCount">0</span>
            </div>
            <div class="resource">
                <div class="resourceIcon" style="background-color: #FFD700;"></div>
                <span id="goldCount">0</span>
            </div>
            <div class="resource">
                <div class="resourceIcon" style="background-color: #A9A9A9;"></div>
                <span id="ironCount">0</span>
            </div>
            <div class="resource">
                <div class="resourceIcon" style="background-color: #FF6347;"></div>
                <span id="coalCount">0</span>
            </div>
            <div class="resource">
                <div class="resourceIcon" style="background-color: #FF0000;"></div>
                <span id="rubyCount">0</span>
            </div>
            <div class="resource">
                <div class="resourceIcon" style="background-color: #0000FF;"></div>
                <span id="sapphireCount">0</span>
            </div>
            <div class="resource">
                <div class="resourceIcon" style="background-color: #00FF00;"></div>
                <span id="emeraldCount">0</span>
            </div>
            <div class="resource">
                <div class="resourceIcon" style="background-color: #FFFFFF;"></div>
                <span id="moneyCount">100</span>
            </div>
        </div>
        
        <div id="miniMap">
            <canvas id="miniMapView"></canvas>
        </div>
        
        <div id="inventory">
            <div id="inventoryTitle">Инвентарь</div>
            <button id="inventoryClose">X</button>
            <div id="inventorySlots">
                <!-- Слоты будут добавляться через JS -->
            </div>
            <div id="crafting">
                <div id="craftingTitle">Крафт</div>
                <div id="craftingRecipes">
                    <!-- Рецепты будут добавляться через JS -->
                </div>
            </div>
        </div>
        
        <div id="merchantUI">
            <div id="merchantTitle">Торговец</div>
            <button id="merchantClose">X</button>
            <div id="merchantItems">
                <!-- Товары будут добавляться через JS -->
            </div>
        </div>
        
        <div id="buildUI">
            <div id="buildTitle">Строительство</div>
            <button id="buildClose">X</button>
            <div id="buildItems">
                <!-- Строения будут добавляться через JS -->
            </div>
        </div>
        
        <div id="achievementPopup">
            <div id="achievementTitle">Достижение разблокировано!</div>
            <div id="achievementDesc">Первое дерево</div>
        </div>
    </div>
    
    <script>
        // Инициализация игры
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const miniMapCanvas = document.getElementById('miniMapView');
        const miniMapCtx = miniMapCanvas.getContext('2d');
        
        // Установка размеров canvas
        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            miniMapCanvas.width = 100;
            miniMapCanvas.height = 100;
        }
        
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();
        
        // Состояние игры
        const gameState = {
            player: {
                x: canvas.width / 2,
                y: canvas.height / 2,
                speed: 3,
                health: 100,
                maxHealth: 100,
                inventory: Array(30).fill(null),
                resources: {
                    wood: 0,
                    stone: 0,
                    gold: 0,
                    iron: 0,
                    coal: 0,
                    ruby: 0,
                    sapphire: 0,
                    emerald: 0,
                    money: 100
                },
                attackCooldown: 0,
                direction: { x: 0, y: 1 },
                buildingMode: false,
                selectedBuilding: null
            },
            world: {
                tiles: [],
                size: 200,
                tileSize: 64,
                offset: { x: 0, y: 0 },
                time: 0,
                dayLength: 5 * 60 * 1000, // 5 минут день
                nightLength: 2 * 60 * 1000, // 2 минуты ночь
                isDay: true
            },
            entities: [],
            buildings: [],
            particles: [],
            merchant: {
                x: 2000,
                y: 2000,
                items: [
                    { type: "wood", price: 10, amount: 5 },
                    { type: "stone", price: 15, amount: 5 },
                    { type: "iron", price: 30, amount: 3 },
                    { type: "coal", price: 25, amount: 3 },
                    { type: "health_potion", price: 50, amount: 1 }
                ]
            },
            joystick: {
                active: false,
                x: 0,
                y: 0,
                startX: 0,
                startY: 0,
                moveX: 0,
                moveY: 0
            },
            quests: {
                current: 0,
                list: [
                    { title: "Сбор ресурсов", description: "Соберите 5 дерева", target: "wood", amount: 5, reward: "axe" },
                    { title: "Каменные блоки", description: "Соберите 10 камня", target: "stone", amount: 10, reward: "pickaxe" },
                    { title: "Драгоценности", description: "Соберите 3 золота", target: "gold", amount: 3, reward: "sword" },
                    { title: "Металлургия", description: "Соберите 5 железа", target: "iron", amount: 5, reward: "iron_sword" },
                    { title: "Финальный босс", description: "Победите короля монстров", target: "boss", amount: 1, reward: "victory" }
                ]
            },
            achievements: [
                { id: "first_wood", title: "Первое дерево", description: "Собрать 1 дерево", unlocked: false },
                { id: "first_stone", title: "Первый камень", description: "Собрать 1 камень", unlocked: false },
                { id: "first_gold", title: "Первое золото", description: "Собрать 1 золото", unlocked: false },
                { id: "first_iron", title: "Первое железо", description: "Собрать 1 железо", unlocked: false },
                { id: "first_craft", title: "Первый крафт", description: "Создать первый предмет", unlocked: false },
                { id: "first_monster", title: "Первый монстр", description: "Победить первого монстра", unlocked: false },
                { id: "first_night", title: "Первая ночь", description: "Пережить первую ночь", unlocked: false },
                { id: "first_wall", title: "Первая стена", description: "Построить первую стену", unlocked: false },
                { id: "boss_kill", title: "Победитель", description: "Победить босса", unlocked: false }
            ],
            craftingRecipes: [
                { result: "axe", ingredients: [{ type: "wood", amount: 3 }, { type: "stone", amount: 2 }] },
                { result: "pickaxe", ingredients: [{ type: "wood", amount: 2 }, { type: "stone", amount: 4 }] },
                { result: "sword", ingredients: [{ type: "wood", amount: 2 }, { type: "stone", amount: 3 }, { type: "gold", amount: 1 }] },
                { result: "iron_sword", ingredients: [{ type: "wood", amount: 2 }, { type: "iron", amount: 3 }, { type: "coal", amount: 1 }] },
                { result: "health_potion", ingredients: [{ type: "wood", amount: 1 }, { type: "stone", amount: 1 }, { type: "gold", amount: 1 }] },
                { result: "wall", ingredients: [{ type: "stone", amount: 5 }] },
                { result: "iron_wall", ingredients: [{ type: "stone", amount: 3 }, { type: "iron", amount: 2 }] },
                { result: "torch", ingredients: [{ type: "wood", amount: 1 }, { type: "coal", amount: 1 }] }
            ],
            buildables: [
                { type: "wall", name: "Каменная стена", icon: "🧱", cost: [{ type: "stone", amount: 5 }], health: 100 },
                { type: "iron_wall", name: "Железная стена", icon: "🔩", cost: [{ type: "stone", amount: 3 }, { type: "iron", amount: 2 }], health: 200 },
                { type: "torch", name: "Факел", icon: "🔥", cost: [{ type: "wood", amount: 1 }, { type: "coal", amount: 1 }], health: 50, light: true }
            ],
            items: {
                wood: { name: "Дерево", icon: "🪵", type: "resource" },
                stone: { name: "Камень", icon: "🪨", type: "resource" },
                gold: { name: "Золото", icon: "💰", type: "resource" },
                iron: { name: "Железо", icon: "⛓️", type: "resource" },
                coal: { name: "Уголь", icon: "⚫", type: "resource" },
                ruby: { name: "Рубин", icon: "🔴", type: "resource" },
                sapphire: { name: "Сапфир", icon: "🔵", type: "resource" },
                emerald: { name: "Изумруд", icon: "🟢", type: "resource" },
                money: { name: "Деньги", icon: "💵", type: "resource" },
                axe: { name: "Топор", icon: "🪓", type: "tool", damage: 10 },
                pickaxe: { name: "Кирка", icon: "⛏️", type: "tool", damage: 15 },
                sword: { name: "Меч", icon: "⚔️", type: "weapon", damage: 25 },
                iron_sword: { name: "Железный меч", icon: "🗡️", type: "weapon", damage: 35 },
                health_potion: { name: "Зелье здоровья", icon: "🧪", type: "consumable", health: 30 },
                wall: { name: "Каменная стена", icon: "🧱", type: "building", health: 100 },
                iron_wall: { name: "Железная стена", icon: "🔩", type: "building", health: 200 },
                torch: { name: "Факел", icon: "🔥", type: "building", health: 50, light: true }
            },
            monsters: [
                { name: "Гоблин", health: 30, damage: 5, speed: 1.5, size: 40, color: "#4CAF50", exp: 10, spawnRate: 0.01, nightSpawnRate: 0.02 },
                { name: "Скелет", health: 50, damage: 8, speed: 1.2, size: 45, color: "#F5F5F5", exp: 15, spawnRate: 0.007, nightSpawnRate: 0.015 },
                { name: "Орк", health: 80, damage: 12, speed: 1, size: 50, color: "#8BC34A", exp: 20, spawnRate: 0.005, nightSpawnRate: 0.01 },
                { name: "Зомби", health: 60, damage: 10, speed: 0.8, size: 50, color: "#795548", exp: 15, spawnRate: 0, nightSpawnRate: 0.03 },
                { name: "Король монстров", health: 300, damage: 20, speed: 1.5, size: 80, color: "#F44336", exp: 100, spawnRate: 0.001, nightSpawnRate: 0, isBoss: true }
            ],
            lastTime: 0,
            gameTime: 0,
            camera: {
                x: 0,
                y: 0
            }
        };
        
        // Генерация мира
        function generateWorld() {
            gameState.world.tiles = [];
            for (let y = 0; y < gameState.world.size; y++) {
                gameState.world.tiles[y] = [];
                for (let x = 0; x < gameState.world.size; x++) {
                    const noise = Math.random();
                    if (noise < 0.6) {
                        gameState.world.tiles[y][x] = { type: "grass", resource: null };
                    } else if (noise < 0.8) {
                        gameState.world.tiles[y][x] = { type: "sand", resource: null };
                    } else {
                        gameState.world.tiles[y][x] = { type: "water", resource: null };
                    }
                    
                    // Добавление ресурсов
                    if (Math.random() < 0.05 && gameState.world.tiles[y][x].type !== "water") {
                        const resourceNoise = Math.random();
                        if (resourceNoise < 0.5) {
                            gameState.world.tiles[y][x].resource = { type: "wood", amount: 3 };
                        } else if (resourceNoise < 0.75) {
                            gameState.world.tiles[y][x].resource = { type: "stone", amount: 2 };
                        } else if (resourceNoise < 0.85) {
                            gameState.world.tiles[y][x].resource = { type: "gold", amount: 1 };
                        } else if (resourceNoise < 0.92) {
                            gameState.world.tiles[y][x].resource = { type: "iron", amount: 1 };
                        } else if (resourceNoise < 0.97) {
                            gameState.world.tiles[y][x].resource = { type: "coal", amount: 1 };
                        } else {
                            const gemNoise = Math.random();
                            if (gemNoise < 0.33) {
                                gameState.world.tiles[y][x].resource = { type: "ruby", amount: 1 };
                            } else if (gemNoise < 0.66) {
                                gameState.world.tiles[y][x].resource = { type: "sapphire", amount: 1 };
                            } else {
                                gameState.world.tiles[y][x].resource = { type: "emerald", amount: 1 };
                            }
                        }
                    }
                }
            }
        }
        
        // Инициализация UI
        function initUI() {
            // Заполнение инвентаря
            const inventorySlots = document.getElementById('inventorySlots');
            inventorySlots.innerHTML = '';
            for (let i = 0; i < gameState.player.inventory.length; i++) {
                const slot = document.createElement('div');
                slot.className = 'inventorySlot';
                slot.dataset.index = i;
                slot.addEventListener('click', () => useItem(i));
                inventorySlots.appendChild(slot);
            }
            
            // Заполнение рецептов крафта
            const craftingRecipes = document.getElementById('craftingRecipes');
            craftingRecipes.innerHTML = '';
            gameState.craftingRecipes.forEach(recipe => {
                const recipeElement = document.createElement('div');
                recipeElement.className = 'recipe';
                
                let ingredientsText = '';
                recipe.ingredients.forEach(ing => {
                    ingredientsText += `${gameState.items[ing.type].icon} x${ing.amount} `;
                });
                
                recipeElement.innerHTML = `
                    <div>${gameState.items[recipe.result].icon} ${gameState.items[recipe.result].name}</div>
                    <div>${ingredientsText}</div>
                    <button class="craftBtn" data-recipe="${recipe.result}">Создать</button>
                `;
                
                craftingRecipes.appendChild(recipeElement);
            });
            
            // Заполнение товаров торговца
            const merchantItems = document.getElementById('merchantItems');
            merchantItems.innerHTML = '';
            gameState.merchant.items.forEach(item => {
                const itemElement = document.createElement('div');
                itemElement.className = 'merchantItem';
                
                itemElement.innerHTML = `
                    <div>${gameState.items[item.type].icon} ${gameState.items[item.type].name}</div>
                    <div>${item.amount} шт.</div>
                    <div>Цена: ${item.price} 💵</div>
                    <button class="buyBtn" data-type="${item.type}" data-price="${item.price}" data-amount="${item.amount}">Купить</button>
                `;
                
                merchantItems.appendChild(itemElement);
            });
            
            // Заполнение строений
            const buildItems = document.getElementById('buildItems');
            buildItems.innerHTML = '';
            gameState.buildables.forEach(item => {
                const itemElement = document.createElement('div');
                itemElement.className = 'buildItem';
                
                let costText = '';
                item.cost.forEach(c => {
                    costText += `${gameState.items[c.type].icon} x${c.amount} `;
                });
                
                itemElement.innerHTML = `
                    <div>${item.icon} ${item.name}</div>
                    <div>${costText}</div>
                    <button class="buildBtn" data-type="${item.type}">Построить</button>
                `;
                
                buildItems.appendChild(itemElement);
            });
            
            // Обработчики кнопок
            document.getElementById('inventoryBtn').addEventListener('click', toggleInventory);
            document.getElementById('inventoryClose').addEventListener('click', toggleInventory);
            document.getElementById('merchantBtn').addEventListener('click', toggleMerchant);
            document.getElementById('merchantClose').addEventListener('click', toggleMerchant);
            document.getElementById('buildBtn').addEventListener('click', toggleBuild);
            document.getElementById('buildClose').addEventListener('click', toggleBuild);
            
            document.querySelectorAll('.craftBtn').forEach(btn => {
                btn.addEventListener('click', function() {
                    craftItem(this.dataset.recipe);
                });
            });
            
            document.querySelectorAll('.buyBtn').forEach(btn => {
                btn.addEventListener('click', function() {
                    buyItem(this.dataset.type, parseInt(this.dataset.price), parseInt(this.dataset.amount));
                });
            });
            
            document.querySelectorAll('.buildBtn').forEach(btn => {
                btn.addEventListener('click', function() {
                    selectBuilding(this.dataset.type);
                });
            });
            
            // Джойстик
            const joystickArea = document.getElementById('joystickArea');
            const joystick = document.getElementById('joystick');
            
            joystickArea.addEventListener('touchstart', handleJoystickStart);
            joystickArea.addEventListener('touchmove', handleJoystickMove);
            joystickArea.addEventListener('touchend', handleJoystickEnd);
            
            // Кнопка атаки
            document.getElementById('attackBtn').addEventListener('touchstart', () => {
                gameState.player.attackCooldown = 20;
                attack();
            });
            
            // Обновление квестов
            updateQuestUI();
        }
        
        // Джойстик управление
        function handleJoystickStart(e) {
            const touch = e.touches[0];
            const rect = e.target.getBoundingClientRect();
            
            gameState.joystick.active = true;
            gameState.joystick.startX = rect.left + rect.width / 2;
            gameState.joystick.startY = rect.top + rect.height / 2;
            gameState.joystick.x = touch.clientX;
            gameState.joystick.y = touch.clientY;
            
            updateJoystickPosition();
        }
        
        function handleJoystickMove(e) {
            if (!gameState.joystick.active) return;
            
            const touch = e.touches[0];
            gameState.joystick.x = touch.clientX;
            gameState.joystick.y = touch.clientY;
            
            updateJoystickPosition();
        }
        
        function handleJoystickEnd() {
            gameState.joystick.active = false;
            gameState.joystick.moveX = 0;
            gameState.joystick.moveY = 0;
            
            const joystick = document.getElementById('joystick');
            joystick.style.transform = 'translate(0, 0)';
        }
        
        function updateJoystickPosition() {
            const joystick = document.getElementById('joystick');
            const rect = document.getElementById('joystickArea').getBoundingClientRect();
            const centerX = rect.left + rect.width / 2;
            const centerY = rect.top + rect.height / 2;
            
            const deltaX = gameState.joystick.x - centerX;
            const deltaY = gameState.joystick.y - centerY;
            const distance = Math.sqrt(deltaX * deltaX + deltaY * deltaY);
            const maxDistance = rect.width / 3;
            
            if (distance > maxDistance) {
                const angle = Math.atan2(deltaY, deltaX);
                gameState.joystick.x = centerX + Math.cos(angle) * maxDistance;
                gameState.joystick.y = centerY + Math.sin(angle) * maxDistance;
            }
            
            const moveX = (gameState.joystick.x - centerX) / maxDistance;
            const moveY = (gameState.joystick.y - centerY) / maxDistance;
            
            gameState.joystick.moveX = moveX;
            gameState.joystick.moveY = moveY;
            
            // Обновление направления игрока
            if (Math.abs(moveX) > 0.1 || Math.abs(moveY) > 0.1) {
                gameState.player.direction = { x: moveX, y: moveY };
            }
            
            joystick.style.transform = `translate(${gameState.joystick.x - centerX}px, ${gameState.joystick.y - centerY}px)`;
        }
        
        // Инвентарь
        function toggleInventory() {
            const inventory = document.getElementById('inventory');
            if (inventory.style.display === 'flex') {
                inventory.style.display = 'none';
            } else {
                inventory.style.display = 'flex';
                updateInventoryUI();
            }
        }
        
        function updateInventoryUI() {
            const slots = document.querySelectorAll('.inventorySlot');
            gameState.player.inventory.forEach((item, index) => {
                const slot = slots[index];
                slot.innerHTML = '';
                
                if (item) {
                    const itemIcon = document.createElement('div');
                    itemIcon.textContent = gameState.items[item.type].icon;
                    slot.appendChild(itemIcon);
                    
                    const itemCount = document.createElement('div');
                    itemCount.className = 'itemCount';
                    itemCount.textContent = item.amount;
                    slot.appendChild(itemCount);
                }
            });
        }
        
        function useItem(slotIndex) {
            const item = gameState.player.inventory[slotIndex];
            if (!item) return;
            
            const itemData = gameState.items[item.type];
            
            if (itemData.type === "consumable") {
                gameState.player.health = Math.min(gameState.player.maxHealth, gameState.player.health + itemData.health);
                updateHealthUI();
                
                // Уменьшаем количество или удаляем предмет
                if (item.amount > 1) {
                    item.amount--;
                } else {
                    gameState.player.inventory[slotIndex] = null;
                }
                
                updateInventoryUI();
            }
        }
        
        // Торговец
        function toggleMerchant() {
            const merchantUI = document.getElementById('merchantUI');
            if (merchantUI.style.display === 'flex') {
                merchantUI.style.display = 'none';
            } else {
                merchantUI.style.display = 'flex';
            }
        }
        
        function buyItem(itemType, price, amount) {
            if (gameState.player.resources.money < price) {
                showMessage("Недостаточно денег!");
                return;
            }
            
            gameState.player.resources.money -= price;
            
            // Добавляем ресурс
            if (gameState.items[itemType].type === "resource") {
                gameState.player.resources[itemType] += amount;
                updateResourcesUI();
            } else {
                // Добавляем предмет в инвентарь
                let added = false;
                for (let i = 0; i < gameState.player.inventory.length; i++) {
                    if (gameState.player.inventory[i] && gameState.player.inventory[i].type === itemType) {
                        gameState.player.inventory[i].amount += amount;
                        added = true;
                        break;
                    }
                }
                
                if (!added) {
                    for (let i = 0; i < gameState.player.inventory.length; i++) {
                        if (!gameState.player.inventory[i]) {
                            gameState.player.inventory[i] = { type: itemType, amount: amount };
                            added = true;
                            break;
                        }
                    }
                }
                
                if (!added) {
                    showMessage("Инвентарь полон!");
                    gameState.player.resources.money += price;
                    return;
                }
            }
            
            showMessage(`Куплено: ${gameState.items[itemType].name} x${amount}`);
            updateInventoryUI();
            updateResourcesUI();
        }
        
        // Строительство
        function toggleBuild() {
            const buildUI = document.getElementById('buildUI');
            if (buildUI.style.display === 'flex') {
                buildUI.style.display = 'none';
                gameState.player.buildingMode = false;
                gameState.player.selectedBuilding = null;
            } else {
                buildUI.style.display = 'flex';
            }
        }
        
        function selectBuilding(buildingType) {
            const building = gameState.buildables.find(b => b.type === buildingType);
            if (!building) return;
            
            // Проверка ресурсов
            for (const cost of building.cost) {
                if (gameState.player.resources[cost.type] < cost.amount) {
                    showMessage(`Не хватает ${gameState.items[cost.type].name}`);
                    return;
                }
            }
            
            gameState.player.buildingMode = true;
            gameState.player.selectedBuilding = building;
            document.getElementById('buildUI').style.display = 'none';
            showMessage(`Выбрано: ${building.name}. Коснитесь места для постройки`);
        }
        
        function placeBuilding(x, y) {
            if (!gameState.player.buildingMode || !gameState.player.selectedBuilding) return;
            
            const building = gameState.player.selectedBuilding;
            
            // Проверка, можно ли строить здесь
            const tileX = Math.floor(x / gameState.world.tileSize);
            const tileY = Math.floor(y / gameState.world.tileSize);
            
            if (tileX < 0 || tileX >= gameState.world.size || tileY < 0 || tileY >= gameState.world.size) {
                showMessage("Нельзя строить за пределами карты!");
                return;
            }
            
            if (gameState.world.tiles[tileY][tileX].type === "water") {
                showMessage("Нельзя строить на воде!");
                return;
            }
            
            // Проверка, нет ли здесь уже постройки
            for (const b of gameState.buildings) {
                const bTileX = Math.floor(b.x / gameState.world.tileSize);
                const bTileY = Math.floor(b.y / gameState.world.tileSize);
                
                if (bTileX === tileX && bTileY === tileY) {
                    showMessage("Здесь уже есть постройка!");
                    return;
                }
            }
            
            // Использование ресурсов
            for (const cost of building.cost) {
                gameState.player.resources[cost.type] -= cost.amount;
            }
            
            // Создание постройки
            gameState.buildings.push({
                type: building.type,
                x: tileX * gameState.world.tileSize + gameState.world.tileSize / 2,
                y: tileY * gameState.world.tileSize + gameState.world.tileSize / 2,
                health: building.health,
                maxHealth: building.health,
                light: building.light || false
            });
            
            updateResourcesUI();
            showMessage(`Построено: ${building.name}`);
            
            // Проверка достижения
            if (!gameState.achievements.find(a => a.id === "first_wall").unlocked) {
                unlockAchievement("first_wall");
            }
            
            gameState.player.buildingMode = false;
            gameState.player.selectedBuilding = null;
        }
        
        // Крафт
        function craftItem(itemType) {
            const recipe = gameState.craftingRecipes.find(r => r.result === itemType);
            if (!recipe) return;
            
            // Проверка ингредиентов
            for (const ing of recipe.ingredients) {
                if (gameState.player.resources[ing.type] < ing.amount) {
                    showMessage(`Не хватает ${gameState.items[ing.type].name}`);
                    return;
                }
            }
            
            // Использование ингредиентов
            for (const ing of recipe.ingredients) {
                gameState.player.resources[ing.type] -= ing.amount;
            }
            
            // Добавление предмета в инвентарь
            let added = false;
            for (let i = 0; i < gameState.player.inventory.length; i++) {
                if (gameState.player.inventory[i] && gameState.player.inventory[i].type === itemType) {
                    gameState.player.inventory[i].amount++;
                    added = true;
                    break;
                }
            }
            
            if (!added) {
                for (let i = 0; i < gameState.player.inventory.length; i++) {
                    if (!gameState.player.inventory[i]) {
                        gameState.player.inventory[i] = { type: itemType, amount: 1 };
                        added = true;
                        break;
                    }
                }
            }
            
            if (added) {
                showMessage(`Создано: ${gameState.items[itemType].name}`);
                updateInventoryUI();
                updateResourcesUI();
                
                // Проверка достижения
                if (!gameState.achievements.find(a => a.id === "first_craft").unlocked) {
                    unlockAchievement("first_craft");
                }
            } else {
                showMessage("Инвентарь полон!");
            }
        }
        
        // Атака
        function attack() {
            const weapon = gameState.player.inventory.find(item => item && 
                (gameState.items[item.type].type === "weapon" || gameState.items[item.type].type === "tool"));
            
            let damage = 5; // Базовая атака
            if (weapon) {
                damage = gameState.items[weapon.type].damage;
            }
            
            // Проверка попадания по монстрам
            for (let i = gameState.entities.length - 1; i >= 0; i--) {
                const entity = gameState.entities[i];
                if (entity.type === "monster") {
                    const dx = entity.x - gameState.player.x;
                    const dy = entity.y - gameState.player.y;
                    const distance = Math.sqrt(dx * dx + dy * dy);
                    
                    if (distance < 80) { // Радиус атаки
                        entity.health -= damage;
                        
                        // Эффект попадания
                        for (let j = 0; j < 5; j++) {
                            gameState.particles.push({
                                x: entity.x,
                                y: entity.y,
                                vx: Math.random() * 4 - 2,
                                vy: Math.random() * 4 - 2,
                                color: "#FF0000",
                                life: 30
                            });
                        }
                        
                        if (entity.health <= 0) {
                            // Убийство монстра
                            if (entity.isBoss) {
                                completeQuest();
                                unlockAchievement("boss_kill");
                            } else if (!gameState.achievements.find(a => a.id === "first_monster").unlocked) {
                                unlockAchievement("first_monster");
                            }
                            
                            // Шанс выпадения ресурсов
                            if (Math.random() < 0.3) {
                                const dropType = ["wood", "stone", "gold", "iron", "coal"][Math.floor(Math.random() * 5)];
                                addResource(dropType, 1);
                            }
                            
                            gameState.entities.splice(i, 1);
                        }
                    }
                }
            }
            
            // Проверка попадания по стенам (для зомби)
            if (gameState.world.isDay) return;
            
            for (let i = gameState.buildings.length - 1; i >= 0; i--) {
                const building = gameState.buildings[i];
                const dx = building.x - gameState.player.x;
                const dy = building.y - gameState.player.y;
                const distance = Math.sqrt(dx * dx + dy * dy);
                
                if (distance < 80) { // Радиус атаки
                    // Восстановление стен
                    building.health = Math.min(building.maxHealth, building.health + 5);
                }
            }
        }
        
        // Квесты
        function updateQuestUI() {
            const currentQuest = gameState.quests.list[gameState.quests.current];
            document.getElementById('questTitle').textContent = `Квест: ${currentQuest.title}`;
            document.getElementById('questText').textContent = currentQuest.description;
        }
        
        function checkQuestProgress(resourceType) {
            const currentQuest = gameState.quests.list[gameState.quests.current];
            
            if (currentQuest.target === resourceType) {
                const resourceCount = gameState.player.resources[resourceType];
                if (resourceCount >= currentQuest.amount) {
                    completeQuest();
                }
            } else if (currentQuest.target === "boss") {
                // Проверка убийства босса
                const bossExists = gameState.entities.some(e => e.type === "monster" && e.isBoss);
                if (!bossExists) {
                    completeQuest();
                }
            }
        }
        
        function completeQuest() {
            const currentQuest = gameState.quests.list[gameState.quests.current];
            
            // Награда
            if (currentQuest.reward === "victory") {
                showMessage("ПОБЕДА! Вы победили короля монстров!");
            } else if (currentQuest.reward) {
                let added = false;
                for (let i = 0; i < gameState.player.inventory.length; i++) {
                    if (!gameState.player.inventory[i]) {
                        gameState.player.inventory[i] = { type: currentQuest.reward, amount: 1 };
                        added = true;
                        break;
                    }
                }
                
                if (added) {
                    showMessage(`Награда: ${gameState.items[currentQuest.reward].name}`);
                    updateInventoryUI();
                }
            }
            
            // Переход к следующему квесту
            if (gameState.quests.current < gameState.quests.list.length - 1) {
                gameState.quests.current++;
                updateQuestUI();
            }
        }
        
        // Достижения
        function unlockAchievement(achievementId) {
            const achievement = gameState.achievements.find(a => a.id === achievementId);
            if (!achievement || achievement.unlocked) return;
            
            achievement.unlocked = true;
            showAchievement(achievement.title, achievement.description);
        }
        
        function showAchievement(title, description) {
            const popup = document.getElementById('achievementPopup');
            document.getElementById('achievementTitle').textContent = title;
            document.getElementById('achievementDesc').textContent = description;
            
            popup.style.display = 'block';
            setTimeout(() => {
                popup.style.display = 'none';
            }, 3000);
        }
        
        // Ресурсы
        function addResource(type, amount = 1) {
            gameState.player.resources[type] += amount;
            updateResourcesUI();
            
            // Проверка достижений
            if (type === "wood" && !gameState.achievements.find(a => a.id === "first_wood").unlocked) {
                unlockAchievement("first_wood");
            } else if (type === "stone" && !gameState.achievements.find(a => a.id === "first_stone").unlocked) {
                unlockAchievement("first_stone");
            } else if (type === "gold" && !gameState.achievements.find(a => a.id === "first_gold").unlocked) {
                unlockAchievement("first_gold");
            } else if (type === "iron" && !gameState.achievements.find(a => a.id === "first_iron").unlocked) {
                unlockAchievement("first_iron");
            }
            
            // Проверка квеста
            checkQuestProgress(type);
        }
        
        function updateResourcesUI() {
            document.getElementById('woodCount').textContent = gameState.player.resources.wood;
            document.getElementById('stoneCount').textContent = gameState.player.resources.stone;
            document.getElementById('goldCount').textContent = gameState.player.resources.gold;
            document.getElementById('ironCount').textContent = gameState.player.resources.iron;
            document.getElementById('coalCount').textContent = gameState.player.resources.coal;
            document.getElementById('rubyCount').textContent = gameState.player.resources.ruby;
            document.getElementById('sapphireCount').textContent = gameState.player.resources.sapphire;
            document.getElementById('emeraldCount').textContent = gameState.player.resources.emerald;
            document.getElementById('moneyCount').textContent = gameState.player.resources.money;
        }
        
        // Здоровье
        function updateHealthUI() {
            const healthPercent = (gameState.player.health / gameState.player.maxHealth) * 100;
            document.getElementById('healthFill').style.width = `${healthPercent}%`;
        }
        
        // День/ночь
        function updateDayNight() {
            gameState.world.time += 16; // Примерно 60 FPS
            
            const dayNightElement = document.getElementById('dayNightIndicator');
            const darkOverlay = document.getElementById('darkOverlay');
            
            if (gameState.world.isDay) {
                // День
                const dayProgress = gameState.world.time / gameState.world.dayLength;
                const timeLeft = Math.ceil((gameState.world.dayLength - gameState.world.time) / 1000);
                dayNightElement.textContent = `День (${timeLeft}с)`;
                dayNightElement.style.color = "#FFFF00";
                darkOverlay.style.display = 'none';
                
                if (gameState.world.time >= gameState.world.dayLength) {
                    gameState.world.time = 0;
                    gameState.world.isDay = false;
                    showMessage("Наступает ночь! Зомби атакуют!");
                    
                    // Проверка достижения
                    if (!gameState.achievements.find(a => a.id === "first_night").unlocked) {
                        unlockAchievement("first_night");
                    }
                }
            } else {
                // Ночь
                const nightProgress = gameState.world.time / gameState.world.nightLength;
                const timeLeft = Math.ceil((gameState.world.nightLength - gameState.world.time) / 1000);
                dayNightElement.textContent = `Ночь (${timeLeft}с)`;
                dayNightElement.style.color = "#FFFFFF";
                darkOverlay.style.display = 'block';
                
                if (gameState.world.time >= gameState.world.nightLength) {
                    gameState.world.time = 0;
                    gameState.world.isDay = true;
                    showMessage("Наступает день! Монстры отступают.");
                }
            }
        }
        
        // Мини-карта
        function updateMiniMap() {
            miniMapCtx.clearRect(0, 0, miniMapCanvas.width, miniMapCanvas.height);
            
            // Масштаб для отображения всего мира на мини-карте
            const scale = Math.min(
                miniMapCanvas.width / (gameState.world.size * gameState.world.tileSize),
                miniMapCanvas.height / (gameState.world.size * gameState.world.tileSize)
            );
            
            // Центрирование на игроке
            const offsetX = (miniMapCanvas.width - gameState.world.size * gameState.world.tileSize * scale) / 2;
            const offsetY = (miniMapCanvas.height - gameState.world.size * gameState.world.tileSize * scale) / 2;
            
            // Отрисовка мира
            for (let y = 0; y < gameState.world.size; y += 2) {
                for (let x = 0; x < gameState.world.size; x += 2) {
                    const tile = gameState.world.tiles[y][x];
                    
                    switch (tile.type) {
                        case "grass":
                            miniMapCtx.fillStyle = "#4CAF50";
                            break;
                        case "sand":
                            miniMapCtx.fillStyle = "#F4A460";
                            break;
                        case "water":
                            miniMapCtx.fillStyle = "#1E90FF";
                            break;
                    }
                    
                    miniMapCtx.fillRect(
                        offsetX + x * gameState.world.tileSize * scale,
                        offsetY + y * gameState.world.tileSize * scale,
                        gameState.world.tileSize * scale * 2,
                        gameState.world.tileSize * scale * 2
                    );
                }
            }
            
            // Отрисовка построек
            miniMapCtx.fillStyle = "#888";
            gameState.buildings.forEach(building => {
                const x = offsetX + (building.x - gameState.world.tileSize / 2) * scale;
                const y = offsetY + (building.y - gameState.world.tileSize / 2) * scale;
                
                miniMapCtx.fillRect(
                    x,
                    y,
                    gameState.world.tileSize * scale,
                    gameState.world.tileSize * scale
                );
            });
            
            // Отрисовка торговца
            miniMapCtx.fillStyle = "gold";
            miniMapCtx.beginPath();
            miniMapCtx.arc(
                offsetX + gameState.merchant.x * scale,
                offsetY + gameState.merchant.y * scale,
                3 * scale,
                0,
                Math.PI * 2
            );
            miniMapCtx.fill();
            
            // Отрисовка игрока
            miniMapCtx.fillStyle = "#3498db";
            miniMapCtx.beginPath();
            miniMapCtx.arc(
                offsetX + gameState.player.x * scale,
                offsetY + gameState.player.y * scale,
                3 * scale,
                0,
                Math.PI * 2
            );
            miniMapCtx.fill();
            
            // Отрисовка монстров
            miniMapCtx.fillStyle = "#f00";
            gameState.entities.forEach(entity => {
                if (entity.type === "monster") {
                    miniMapCtx.beginPath();
                    miniMapCtx.arc(
                        offsetX + entity.x * scale,
                        offsetY + entity.y * scale,
                        2 * scale,
                        0,
                        Math.PI * 2
                    );
                    miniMapCtx.fill();
                }
            });
        }
        
        // Сообщения
        function showMessage(text) {
            // Можно добавить систему сообщений
            console.log(text);
        }
        
        // Генерация монстров
        function spawnMonsters() {
            // Шанс появления монстра
            gameState.monsters.forEach(monster => {
                const spawnRate = gameState.world.isDay ? monster.spawnRate : monster.nightSpawnRate;
                
                if (Math.random() < spawnRate * (1 + gameState.gameTime / 100000)) {
                    const angle = Math.random() * Math.PI * 2;
                    const distance = 500 + Math.random() * 300;
                    
                    const x = gameState.player.x + Math.cos(angle) * distance;
                    const y = gameState.player.y + Math.sin(angle) * distance;
                    
                    // Проверка, чтобы монстры не спавнились в воде
                    const tileX = Math.floor(x / gameState.world.tileSize);
                    const tileY = Math.floor(y / gameState.world.tileSize);
                    
                    if (tileX >= 0 && tileX < gameState.world.size && 
                        tileY >= 0 && tileY < gameState.world.size && 
                        gameState.world.tiles[tileY][tileX].type !== "water") {
                        
                        const newMonster = {
                            type: "monster",
                            x: x,
                            y: y,
                            health: monster.health,
                            maxHealth: monster.health,
                            damage: monster.damage,
                            speed: monster.speed,
                            size: monster.size,
                            color: monster.color,
                            name: monster.name,
                            isBoss: monster.isBoss || false,
                            isZombie: monster.name === "Зомби"
                        };
                        
                        gameState.entities.push(newMonster);
                    }
                }
            });
        }
        
        // Игровой цикл
        function gameLoop(timestamp) {
            const deltaTime = timestamp - gameState.lastTime;
            gameState.lastTime = timestamp;
            gameState.gameTime += deltaTime;
            
            // Обновление дня и ночи
            updateDayNight();
            
            // Обновление игрока
            if (gameState.joystick.active) {
                gameState.player.x += gameState.joystick.moveX * gameState.player.speed;
                gameState.player.y += gameState.joystick.moveY * gameState.player.speed;
            }
            
            // Ограничение перемещения игрока
            const halfTile = gameState.world.tileSize / 2;
            gameState.player.x = Math.max(halfTile, Math.min(gameState.player.x, gameState.world.size * gameState.world.tileSize - halfTile));
            gameState.player.y = Math.max(halfTile, Math.min(gameState.player.y, gameState.world.size * gameState.world.tileSize - halfTile));
            
            // Обновление камеры
            gameState.camera.x = gameState.player.x - canvas.width / 2;
            gameState.camera.y = gameState.player.y - canvas.height / 2;
            
            // Обновление монстров
            gameState.entities.forEach(entity => {
                if (entity.type === "monster") {
                    // Движение к игроку или к стенам (для зомби ночью)
                    let targetX = gameState.player.x;
                    let targetY = gameState.player.y;
                    
                    if (!gameState.world.isDay && entity.isZombie && gameState.buildings.length > 0) {
                        // Зомби ночью атакуют стены
                        let closestBuilding = null;
                        let closestDistance = Infinity;
                        
                        gameState.buildings.forEach(building => {
                            const dx = building.x - entity.x;
                            const dy = building.y - entity.y;
                            const distance = Math.sqrt(dx * dx + dy * dy);
                            
                            if (distance < closestDistance) {
                                closestDistance = distance;
                                closestBuilding = building;
                            }
                        });
                        
                        if (closestBuilding && closestDistance < 300) {
                            targetX = closestBuilding.x;
                            targetY = closestBuilding.y;
                        }
                    }
                    
                    const dx = targetX - entity.x;
                    const dy = targetY - entity.y;
                    const distance = Math.sqrt(dx * dx + dy * dy);
                    
                    if (distance > 50) { // Не подходить слишком близко
                        entity.x += (dx / distance) * entity.speed;
                        entity.y += (dy / distance) * entity.speed;
                    }
                    
                    // Атака игрока или стен
                    if (distance < 60) {
                        if (Math.random() < 0.02) {
                            if (distance < 60 && targetX === gameState.player.x && targetY === gameState.player.y) {
                                // Атака игрока
                                gameState.player.health -= entity.damage;
                                updateHealthUI();
                                
                                // Эффект попадания
                                for (let i = 0; i < 5; i++) {
                                    gameState.particles.push({
                                        x: gameState.player.x,
                                        y: gameState.player.y,
                                        vx: Math.random() * 4 - 2,
                                        vy: Math.random() * 4 - 2,
                                        color: "#FF0000",
                                        life: 30
                                    });
                                }
                                
                                if (gameState.player.health <= 0) {
                                    // Игрок умер
                                    gameState.player.health = gameState.player.maxHealth;
                                    gameState.player.x = canvas.width / 2;
                                    gameState.player.y = canvas.height / 2;
                                    updateHealthUI();
                                    showMessage("Вы умерли! Возрождение...");
                                }
                            } else {
                                // Атака стен (для зомби)
                                for (let i = gameState.buildings.length - 1; i >= 0; i--) {
                                    const building = gameState.buildings[i];
                                    const bdx = building.x - entity.x;
                                    const bdy = building.y - entity.y;
                                    const bdistance = Math.sqrt(bdx * bdx + bdy * bdy);
                                    
                                    if (bdistance < 60) {
                                        building.health -= entity.damage;
                                        
                                        if (building.health <= 0) {
                                            gameState.buildings.splice(i, 1);
                                            showMessage("Стена разрушена!");
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
            });
            
            // Обновление частиц
            for (let i = gameState.particles.length - 1; i >= 0; i--) {
                const p = gameState.particles[i];
                p.x += p.vx;
                p.y += p.vy;
                p.life--;
                
                if (p.life <= 0) {
                    gameState.particles.splice(i, 1);
                }
            }
            
            // Спавн монстров
            if (Math.random() < 0.01) {
                spawnMonsters();
            }
            
            // Обновление мини-карты
            updateMiniMap();
            
            // Отрисовка
            render();
            
            // Перезарядка атаки
            if (gameState.player.attackCooldown > 0) {
                gameState.player.attackCooldown--;
            }
            
            requestAnimationFrame(gameLoop);
        }
        
        // Отрисовка
        function render() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // Отрисовка мира
            const startX = Math.max(0, Math.floor(gameState.camera.x / gameState.world.tileSize));
            const startY = Math.max(0, Math.floor(gameState.camera.y / gameState.world.tileSize));
            const endX = Math.min(gameState.world.size, Math.ceil((gameState.camera.x + canvas.width) / gameState.world.tileSize));
            const endY = Math.min(gameState.world.size, Math.ceil((gameState.camera.y + canvas.height) / gameState.world.tileSize));
            
            for (let y = startY; y < endY; y++) {
                for (let x = startX; x < endX; x++) {
                    const tile = gameState.world.tiles[y][x];
                    const screenX = x * gameState.world.tileSize - gameState.camera.x;
                    const screenY = y * gameState.world.tileSize - gameState.camera.y;
                    
                    // Отрисовка земли
                    switch (tile.type) {
                        case "grass":
                            ctx.fillStyle = "#4CAF50";
                            break;
                        case "sand":
                            ctx.fillStyle = "#F4A460";
                            break;
                        case "water":
                            ctx.fillStyle = "#1E90FF";
                            break;
                    }
                    
                    ctx.fillRect(screenX, screenY, gameState.world.tileSize, gameState.world.tileSize);
                    
                    // Отрисовка ресурсов
                    if (tile.resource) {
                        let resourceColor, resourceChar;
                        switch (tile.resource.type) {
                            case "wood":
                                resourceColor = "#8B4513";
                                resourceChar = "🌲";
                                break;
                            case "stone":
                                resourceColor = "#808080";
                                resourceChar = "🪨";
                                break;
                            case "gold":
                                resourceColor = "#FFD700";
                                resourceChar = "💰";
                                break;
                            case "iron":
                                resourceColor = "#A9A9A9";
                                resourceChar = "⛓️";
                                break;
                            case "coal":
                                resourceColor = "#000000";
                                resourceChar = "⚫";
                                break;
                            case "ruby":
                                resourceColor = "#FF0000";
                                resourceChar = "🔴";
                                break;
                            case "sapphire":
                                resourceColor = "#0000FF";
                                resourceChar = "🔵";
                                break;
                            case "emerald":
                                resourceColor = "#00FF00";
                                resourceChar = "🟢";
                                break;
                        }
                        
                        ctx.font = `${gameState.world.tileSize}px Arial`;
                        ctx.textAlign = "center";
                        ctx.textBaseline = "middle";
                        ctx.fillStyle = resourceColor;
                        ctx.fillText(resourceChar, screenX + gameState.world.tileSize / 2, screenY + gameState.world.tileSize / 2);
                    }
                }
            }
            
            // Отрисовка построек
            gameState.buildings.forEach(building => {
                const screenX = building.x - gameState.camera.x;
                const screenY = building.y - gameState.camera.y;
                
                let buildingColor, buildingChar;
                switch (building.type) {
                    case "wall":
                        buildingColor = "#888";
                        buildingChar = "🧱";
                        break;
                    case "iron_wall":
                        buildingColor = "#555";
                        buildingChar = "🔩";
                        break;
                    case "torch":
                        buildingColor = "#FFA500";
                        buildingChar = "🔥";
                        break;
                }
                
                ctx.font = `${gameState.world.tileSize}px Arial`;
                ctx.textAlign = "center";
                ctx.textBaseline = "middle";
                ctx.fillStyle = buildingColor;
                ctx.fillText(buildingChar, screenX, screenY);
                
                // Полоска здоровья
                if (building.health < building.maxHealth) {
                    const healthWidth = 40;
                    const healthHeight = 5;
                    const healthX = screenX - healthWidth / 2;
                    const healthY = screenY - gameState.world.tileSize / 2 - 5;
                    
                    ctx.fillStyle = "#333";
                    ctx.fillRect(healthX, healthY, healthWidth, healthHeight);
                    
                    ctx.fillStyle = "#0F0";
                    ctx.fillRect(healthX, healthY, healthWidth * (building.health / building.maxHealth), healthHeight);
                }
            });
            
            // Отрисовка торговца
            const merchantScreenX = gameState.merchant.x - gameState.camera.x;
            const merchantScreenY = gameState.merchant.y - gameState.camera.y;
            
            if (merchantScreenX > -50 && merchantScreenX < canvas.width + 50 &&
                merchantScreenY > -50 && merchantScreenY < canvas.height + 50) {
                
                ctx.fillStyle = "gold";
                ctx.beginPath();
                ctx.arc(merchantScreenX, merchantScreenY, 25, 0, Math.PI * 2);
                ctx.fill();
                
                ctx.font = "16px Arial";
                ctx.textAlign = "center";
                ctx.fillStyle = "#000";
                ctx.fillText("💰", merchantScreenX, merchantScreenY + 5);
            }
            
            // Отрисовка сущностей
            gameState.entities.forEach(entity => {
                const screenX = entity.x - gameState.camera.x;
                const screenY = entity.y - gameState.camera.y;
                
                if (entity.type === "monster") {
                    // Тело монстра
                    ctx.fillStyle = entity.color;
                    ctx.beginPath();
                    ctx.arc(screenX, screenY, entity.size / 2, 0, Math.PI * 2);
                    ctx.fill();
                    
                    // Глаза для зомби ночью
                    if (entity.isZombie && !gameState.world.isDay) {
                        ctx.fillStyle = "#F00";
                        ctx.beginPath();
                        ctx.arc(screenX - 10, screenY - 5, 3, 0, Math.PI * 2);
                        ctx.arc(screenX + 10, screenY - 5, 3, 0, Math.PI * 2);
                        ctx.fill();
                    }
                    
                    // Полоска здоровья
                    if (entity.health < entity.maxHealth) {
                        const healthWidth = 40;
                        const healthHeight = 5;
                        const healthX = screenX - healthWidth / 2;
                        const healthY = screenY - entity.size / 2 - 10;
                        
                        ctx.fillStyle = "#333";
                        ctx.fillRect(healthX, healthY, healthWidth, healthHeight);
                        
                        ctx.fillStyle = "#F00";
                        ctx.fillRect(healthX, healthY, healthWidth * (entity.health / entity.maxHealth), healthHeight);
                    }
                    
                    // Имя монстра
                    if (entity.isBoss) {
                        ctx.font = "bold 16px Arial";
                        ctx.fillStyle = "#F00";
                        ctx.textAlign = "center";
                        ctx.fillText(entity.name, screenX, screenY - entity.size / 2 - 25);
                    }
                }
            });
            
            // Отрисовка игрока
            const playerScreenX = gameState.player.x - gameState.camera.x;
            const playerScreenY = gameState.player.y - gameState.camera.y;
            
            ctx.fillStyle = "#3498db";
            ctx.beginPath();
            ctx.arc(playerScreenX, playerScreenY, 20, 0, Math.PI * 2);
            ctx.fill();
            
            // Отрисовка направления взгляда игрока
            ctx.strokeStyle = "#fff";
            ctx.lineWidth = 3;
            ctx.beginPath();
            ctx.moveTo(playerScreenX, playerScreenY);
            ctx.lineTo(
                playerScreenX + gameState.player.direction.x * 30,
                playerScreenY + gameState.player.direction.y * 30
            );
            ctx.stroke();
            
            // Отрисовка атаки
            if (gameState.player.attackCooldown > 0) {
                ctx.strokeStyle = "rgba(255, 255, 0, 0.7)";
                ctx.lineWidth = 3;
                ctx.beginPath();
                ctx.arc(
                    playerScreenX, 
                    playerScreenY, 
                    80, 
                    Math.atan2(gameState.player.direction.y, gameState.player.direction.x) - Math.PI/4,
                    Math.atan2(gameState.player.direction.y, gameState.player.direction.x) + Math.PI/4
                );
                ctx.stroke();
            }
            
            // Отрисовка выбранного здания
            if (gameState.player.buildingMode && gameState.player.selectedBuilding) {
                const tileX = Math.floor((gameState.player.x + gameState.player.direction.x * 50) / gameState.world.tileSize);
                const tileY = Math.floor((gameState.player.y + gameState.player.direction.y * 50) / gameState.world.tileSize);
                
                const screenX = tileX * gameState.world.tileSize - gameState.camera.x + gameState.world.tileSize / 2;
                const screenY = tileY * gameState.world.tileSize - gameState.camera.y + gameState.world.tileSize / 2;
                
                ctx.fillStyle = "rgba(0, 255, 0, 0.3)";
                ctx.fillRect(
                    tileX * gameState.world.tileSize - gameState.camera.x,
                    tileY * gameState.world.tileSize - gameState.camera.y,
                    gameState.world.tileSize,
                    gameState.world.tileSize
                );
                
                ctx.font = `${gameState.world.tileSize}px Arial`;
                ctx.textAlign = "center";
                ctx.textBaseline = "middle";
                ctx.fillStyle = "rgba(0, 255, 0, 0.7)";
                ctx.fillText(gameState.player.selectedBuilding.icon, screenX, screenY);
            }
            
            // Отрисовка частиц
            gameState.particles.forEach(p => {
                const screenX = p.x - gameState.camera.x;
                const screenY = p.y - gameState.camera.y;
                
                ctx.fillStyle = p.color;
                ctx.beginPath();
                ctx.arc(screenX, screenY, 2, 0, Math.PI * 2);
                ctx.fill();
            });
            
            // Отрисовка UI поверх всего
            renderUI();
        }
        
        function renderUI() {
            // Босс UI
            const boss = gameState.entities.find(e => e.type === "monster" && e.isBoss);
            if (boss) {
                document.getElementById('bossName').style.display = 'block';
                document.getElementById('bossHealth').style.display = 'block';
                document.getElementById('bossHealthFill').style.width = `${(boss.health / boss.maxHealth) * 100}%`;
            } else {
                document.getElementById('bossName').style.display = 'none';
                document.getElementById('bossHealth').style.display = 'none';
            }
        }
        
        // Обработка касаний для сбора ресурсов и строительства
        canvas.addEventListener('touchstart', handleTouch, { passive: false });
        
        function handleTouch(e) {
            e.preventDefault();
            
            if (document.getElementById('inventory').style.display === 'flex' || 
                document.getElementById('merchantUI').style.display === 'flex' ||
                document.getElementById('buildUI').style.display === 'flex') {
                return;
            }
            
            const touch = e.touches[0];
            const rect = canvas.getBoundingClientRect();
            const x = touch.clientX - rect.left + gameState.camera.x;
            const y = touch.clientY - rect.top + gameState.camera.y;
            
            // Проверка расстояния до игрока
            const dx = x - gameState.player.x;
            const dy = y - gameState.player.y;
            const distance = Math.sqrt(dx * dx + dy * dy);
            
            if (distance > 100) {
                // Если в режиме строительства, попытаться построить
                if (gameState.player.buildingMode) {
                    placeBuilding(x, y);
                }
                return;
            }
            
            // Проверка, есть ли ресурс в этой клетке
            const tileX = Math.floor(x / gameState.world.tileSize);
            const tileY = Math.floor(y / gameState.world.tileSize);
            
            if (tileX >= 0 && tileX < gameState.world.size && 
                tileY >= 0 && tileY < gameState.world.size) {
                
                const tile = gameState.world.tiles[tileY][tileX];
                if (tile.resource) {
                    // Сбор ресурса
                    addResource(tile.resource.type, 1);
                    tile.resource.amount--;
                    
                    // Эффект сбора
                    for (let i = 0; i < 5; i++) {
                        gameState.particles.push({
                            x: (tileX + 0.5) * gameState.world.tileSize,
                            y: (tileY + 0.5) * gameState.world.tileSize,
                            vx: Math.random() * 4 - 2,
                            vy: Math.random() * 4 - 2,
                            color: tile.resource.type === "wood" ? "#8B4513" : 
                                  tile.resource.type === "stone" ? "#808080" : 
                                  tile.resource.type === "gold" ? "#FFD700" :
                                  tile.resource.type === "iron" ? "#A9A9A9" :
                                  tile.resource.type === "coal" ? "#000000" :
                                  tile.resource.type === "ruby" ? "#FF0000" :
                                  tile.resource.type === "sapphire" ? "#0000FF" : "#00FF00",
                            life: 30
                        });
                    }
                    
                    // Удаление ресурса, если он закончился
                    if (tile.resource.amount <= 0) {
                        tile.resource = null;
                    }
                }
            }
        }
        
        // Запуск игры
        generateWorld();
        initUI();
        updateHealthUI();
        updateResourcesUI();
        requestAnimationFrame(gameLoop);
    </script>
</body>
</html>
