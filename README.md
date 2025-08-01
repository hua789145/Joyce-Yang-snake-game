<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>雙人貪吃蛇遊戲</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');
        
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            height: 100vh;
            margin: 0;
            font-family: 'Roboto', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
            padding: 20px;
            box-sizing: border-box;
            overflow-y: auto;
        }
        
        h1 {
            color: white;
            text-align: center;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            font-size: 2.5em;
        }
        
        #main-container {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            width: 100%;
            max-width: 1000px;
            gap: 20px;
        }
        
        #game-container {
            flex-grow: 1;
            max-width: 520px;
            border: 4px solid #3498db;
            box-shadow: 0 0 25px rgba(52, 152, 219, 0.5);
            border-radius: 15px;
            padding: 20px;
            background: #fff;
            text-align: center;
            box-sizing: border-box;
        }
        
        canvas {
            background-color: #2c3e50;
            display: block;
            border: 2px solid #34495e;
            border-radius: 10px;
            box-shadow: inset 0 0 10px rgba(0, 0, 0, 0.3);
            margin: 0 auto;
        }
        
        .player-info-panel {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            background: #fff;
            border-radius: 15px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            width: 200px;
            box-sizing: border-box;
            text-align: center;
        }
        
        .player-avatar {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            border: 4px solid;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2em;
            color: white;
            font-weight: bold;
        }
        
        #player1-avatar { 
            border-color: #f1c40f; 
            background: linear-gradient(135deg, #f1c40f, #f39c12);
        }
        
        #player2-avatar { 
            border-color: #e74c3c; 
            background: linear-gradient(135deg, #e74c3c, #c0392b);
        }

        .player-score {
            font-size: 1.8em;
            font-weight: 700;
            margin-bottom: 15px;
        }
        
        #score-player1 { color: #f1c40f; }
        #score-player2 { color: #e74c3c; }
        
        .player-name {
            font-size: 1.2em;
            font-weight: 700;
            margin-bottom: 15px;
            text-overflow: ellipsis;
            white-space: nowrap;
            overflow: hidden;
            width: 100%;
        }

        .control-panel {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .control-pad {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 5px;
            width: 150px;
            margin-top: 10px;
        }
        
        .control-button {
            background: linear-gradient(135deg, #bdc3c7, #95a5a6);
            color: #2c3e50;
            border: none;
            padding: 10px;
            font-size: 1.2em;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 3px 8px rgba(0, 0, 0, 0.2);
            width: 45px;
            height: 45px;
            display: flex;
            justify-content: center;
            align-items: center;
            user-select: none;
            font-weight: bold;
        }
        
        .control-button:hover {
            background: linear-gradient(135deg, #95a5a6, #7f8c8d);
            transform: translateY(-2px);
            box-shadow: 0 5px 12px rgba(0, 0, 0, 0.3);
        }
        
        .control-button:active {
            transform: translateY(0);
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
        }
        
        .control-button.up { grid-column: 2; }
        .control-button.left { grid-column: 1; grid-row: 2; }
        .control-button.right { grid-column: 3; grid-row: 2; }
        .control-button.down { grid-column: 2; grid-row: 3; }
        
        .game-controls {
            margin: 20px 0;
            display: flex;
            gap: 10px;
            justify-content: center;
            flex-wrap: wrap;
        }
        
        .game-button {
            background: linear-gradient(135deg, #3498db, #2980b9);
            color: white;
            border: none;
            padding: 12px 24px;
            font-size: 1em;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 10px rgba(52, 152, 219, 0.3);
            font-weight: bold;
        }
        
        .game-button:hover {
            background: linear-gradient(135deg, #2980b9, #1f5f8b);
            transform: translateY(-2px);
            box-shadow: 0 6px 15px rgba(52, 152, 219, 0.4);
        }
        
        .game-button:active {
            transform: translateY(0);
        }
        
        .game-button.pause {
            background: linear-gradient(135deg, #f39c12, #e67e22);
        }
        
        .game-button.pause:hover {
            background: linear-gradient(135deg, #e67e22, #d35400);
        }
        
        .game-status {
            font-size: 1.2em;
            font-weight: bold;
            margin: 10px 0;
            color: #2c3e50;
        }
        
        .winner-announcement {
            background: linear-gradient(135deg, #2ecc71, #27ae60);
            color: white;
            padding: 15px;
            border-radius: 10px;
            margin: 10px 0;
            font-size: 1.3em;
            font-weight: bold;
            box-shadow: 0 4px 10px rgba(46, 204, 113, 0.3);
        }
        
        .punishment-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            animation: fadeIn 0.3s ease;
        }
        
        .punishment-content {
            background: linear-gradient(135deg, #e74c3c, #c0392b);
            color: white;
            padding: 40px;
            border-radius: 20px;
            text-align: center;
            max-width: 500px;
            width: 90%;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            animation: slideIn 0.5s ease;
            position: relative;
        }
        
        .punishment-title {
            font-size: 2.5em;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        
        .punishment-text {
            font-size: 1.5em;
            margin-bottom: 30px;
            line-height: 1.4;
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 10px;
            border: 2px solid rgba(255, 255, 255, 0.2);
        }
        
        .punishment-close {
            background: linear-gradient(135deg, #f39c12, #e67e22);
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 1.2em;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 10px rgba(243, 156, 18, 0.3);
            font-weight: bold;
        }
        
        .punishment-close:hover {
            background: linear-gradient(135deg, #e67e22, #d35400);
            transform: translateY(-2px);
            box-shadow: 0 6px 15px rgba(243, 156, 18, 0.4);
        }
        
        .punishment-emoji {
            font-size: 4em;
            margin-bottom: 20px;
            display: block;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @keyframes slideIn {
            from { 
                opacity: 0;
                transform: translateY(-50px) scale(0.8);
            }
            to { 
                opacity: 1;
                transform: translateY(0) scale(1);
            }
        }
        
        @media (max-width: 768px) {
            #main-container {
                flex-direction: column;
                align-items: center;
            }
            
            .player-info-panel {
                width: 100%;
                max-width: 300px;
                margin-bottom: 20px;
            }
            
            h1 {
                font-size: 2em;
            }
            
            .punishment-content {
                padding: 30px 20px;
            }
            
            .punishment-title {
                font-size: 2em;
            }
            
            .punishment-text {
                font-size: 1.2em;
            }
        }
    </style>
</head>
<body>
    <h1>🐍 雙人貪吃蛇大戰 🐍</h1>
    
    <div id="main-container">
        <div class="player-info-panel">
            <div id="player1-avatar" class="player-avatar">1</div>
            <div class="player-name">玩家一</div>
            <div id="score-player1" class="player-score">0</div>
            <div class="control-panel">
                <div style="font-size: 0.9em; margin-bottom: 5px; color: #7f8c8d;">WASD 控制</div>
                <div class="control-pad">
                    <button class="control-button up" data-player="1" data-direction="up">↑</button>
                    <button class="control-button left" data-player="1" data-direction="left">←</button>
                    <button class="control-button right" data-player="1" data-direction="right">→</button>
                    <button class="control-button down" data-player="1" data-direction="down">↓</button>
                </div>
            </div>
        </div>
        
        <div id="game-container">
            <canvas id="gameCanvas" width="480" height="480"></canvas>
            <div class="game-status" id="gameStatus">準備開始遊戲！</div>
            <div class="game-controls">
                <button class="game-button" id="startBtn">開始遊戲</button>
                <button class="game-button pause" id="pauseBtn">暫停</button>
                <button class="game-button" id="resetBtn">重新開始</button>
            </div>
            
            <!-- 食物說明 -->
            <div style="background: #f8f9fa; padding: 15px; border-radius: 10px; margin: 10px 0; font-size: 0.9em;">
                <div style="font-weight: bold; margin-bottom: 8px; color: #2c3e50;">🍎 食物效果說明：</div>
                <div style="display: flex; flex-wrap: wrap; gap: 10px; justify-content: center;">
                    <span>🍎 蘋果 +1分</span>
                    <span>🍯 蜂蜜 +3分</span>
                    <span>⚡ 閃電 +2分 加速</span>
                    <span>🍇 葡萄 +1分 縮小</span>
                    <span>💎 鑽石 +5分</span>
                </div>
            </div>
            
            <div id="winnerDisplay"></div>
        </div>
        
        <div class="player-info-panel">
            <div id="player2-avatar" class="player-avatar">2</div>
            <div class="player-name">玩家二</div>
            <div id="score-player2" class="player-score">0</div>
            <div class="control-panel">
                <div style="font-size: 0.9em; margin-bottom: 5px; color: #7f8c8d;">方向鍵控制</div>
                <div class="control-pad">
                    <button class="control-button up" data-player="2" data-direction="up">↑</button>
                    <button class="control-button left" data-player="2" data-direction="left">←</button>
                    <button class="control-button right" data-player="2" data-direction="right">→</button>
                    <button class="control-button down" data-player="2" data-direction="down">↓</button>
                </div>
            </div>
        </div>
    </div>

    <!-- 懲罰彈窗 -->
    <div id="punishmentModal" class="punishment-modal">
        <div class="punishment-content">
            <span id="punishmentEmoji" class="punishment-emoji">😈</span>
            <div class="punishment-title">懲罰時間！</div>
            <div id="punishmentText" class="punishment-text">
                你輸了！準備接受懲罰吧！
            </div>
            <button class="punishment-close" onclick="closePunishmentModal()">接受懲罰</button>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const startBtn = document.getElementById('startBtn');
        const pauseBtn = document.getElementById('pauseBtn');
        const resetBtn = document.getElementById('resetBtn');
        const gameStatus = document.getElementById('gameStatus');
        const winnerDisplay = document.getElementById('winnerDisplay');
        const scorePlayer1 = document.getElementById('score-player1');
        const scorePlayer2 = document.getElementById('score-player2');

        const gridSize = 20;
        const tileCount = canvas.width / gridSize;

        let gameRunning = false;
        let gamePaused = false;
        let gameLoop;

        // 遊戲狀態
        let player1 = {
            x: 10,
            y: 10,
            dx: 0,
            dy: 0,
            tail: [],
            score: 0,
            color: '#f1c40f'
        };

        let player2 = {
            x: 15,
            y: 15,
            dx: 0,
            dy: 0,
            tail: [],
            score: 0,
            color: '#e74c3c'
        };

        // 食物系統
        let foods = [];
        let obstacles = [];
        
        const foodTypes = [
            { type: 'normal', color: '#2ecc71', points: 1, emoji: '🍎', effect: 'normal' },
            { type: 'golden', color: '#f1c40f', points: 3, emoji: '🍯', effect: 'golden' },
            { type: 'speed', color: '#e67e22', points: 2, emoji: '⚡', effect: 'speed' },
            { type: 'shrink', color: '#9b59b6', points: 1, emoji: '🍇', effect: 'shrink' },
            { type: 'bonus', color: '#1abc9c', points: 5, emoji: '💎', effect: 'bonus' }
        ];

        // 玩家效果狀態
        let playerEffects = {
            player1: { speed: false, speedTimer: 0 },
            player2: { speed: false, speedTimer: 0 }
        };

        // 懲罰系統
        const punishments = [
            {
                emoji: "🤡",
                text: "在下一局遊戲中，你必須用相反的方向鍵操作！（上變下，左變右）"
            },
            {
                emoji: "🐌",
                text: "做10個深蹲！慢慢來，不要偷懶哦～"
            },
            {
                emoji: "🎭",
                text: "用最誇張的表情和聲音說：「我是貪吃蛇菜鳥！」"
            },
            {
                emoji: "🕺",
                text: "跳一段30秒的尬舞！越尷尬越好！"
            },
            {
                emoji: "🎤",
                text: "唱一首兒歌，要有感情地唱完整首！"
            },
            {
                emoji: "🤪",
                text: "用舌頭碰鼻子10次，碰不到就做鬼臉10秒！"
            },
            {
                emoji: "🏃",
                text: "原地跑步1分鐘，要高抬腿哦！"
            },
            {
                emoji: "🎨",
                text: "用非慣用手寫下「我會更努力練習貪吃蛇」"
            },
            {
                emoji: "🤖",
                text: "用機器人的聲音和動作說話30秒！"
            },
            {
                emoji: "🍎",
                text: "下一局遊戲時，每次吃到食物都要說「好吃！」"
            },
            {
                emoji: "😵",
                text: "閉眼轉圈5圈，然後試著走直線到對面！"
            },
            {
                emoji: "🎪",
                text: "表演一個你最拿手的才藝，至少30秒！"
            },
            {
                emoji: "🦘",
                text: "像袋鼠一樣跳躍10次，要有袋鼠的姿勢！"
            },
            {
                emoji: "🎯",
                text: "用眼神「殺死」對手，保持兇狠表情10秒不能笑！"
            },
            {
                emoji: "🧘",
                text: "保持瑜伽姿勢1分鐘，選一個你覺得最難的！"
            }
        ];

        function getRandomPunishment() {
            return punishments[Math.floor(Math.random() * punishments.length)];
        }

        // 生成食物
        function generateFood() {
            // 清空現有食物
            foods = [];
            
            // 生成2-4個食物
            const foodCount = Math.floor(Math.random() * 3) + 2;
            
            for (let i = 0; i < foodCount; i++) {
                let validPosition = false;
                let attempts = 0;
                
                while (!validPosition && attempts < 50) {
                    const x = Math.floor(Math.random() * tileCount);
                    const y = Math.floor(Math.random() * tileCount);
                    
                    validPosition = true;
                    attempts++;
                    
                    // 檢查是否與蛇身重疊
                    if ((x === player1.x && y === player1.y) || (x === player2.x && y === player2.y)) {
                        validPosition = false;
                        continue;
                    }
                    
                    for (let segment of player1.tail) {
                        if (x === segment.x && y === segment.y) {
                            validPosition = false;
                            break;
                        }
                    }
                    
                    if (!validPosition) continue;
                    
                    for (let segment of player2.tail) {
                        if (x === segment.x && y === segment.y) {
                            validPosition = false;
                            break;
                        }
                    }
                    
                    if (!validPosition) continue;
                    
                    // 檢查是否與障礙物重疊
                    for (let obstacle of obstacles) {
                        if (x === obstacle.x && y === obstacle.y) {
                            validPosition = false;
                            break;
                        }
                    }
                    
                    if (!validPosition) continue;
                    
                    // 檢查是否與其他食物重疊
                    for (let food of foods) {
                        if (x === food.x && y === food.y) {
                            validPosition = false;
                            break;
                        }
                    }
                    
                    if (validPosition) {
                        // 選擇食物類型（普通食物機率較高）
                        let foodType;
                        const rand = Math.random();
                        if (rand < 0.5) {
                            foodType = foodTypes[0]; // 普通食物
                        } else if (rand < 0.7) {
                            foodType = foodTypes[1]; // 黃金食物
                        } else if (rand < 0.85) {
                            foodType = foodTypes[2]; // 速度食物
                        } else if (rand < 0.95) {
                            foodType = foodTypes[3]; // 縮小食物
                        } else {
                            foodType = foodTypes[4]; // 獎勵食物
                        }
                        
                        foods.push({
                            x: x,
                            y: y,
                            type: foodType.type,
                            color: foodType.color,
                            points: foodType.points,
                            emoji: foodType.emoji,
                            effect: foodType.effect
                        });
                    }
                }
            }
        }

        // 生成障礙物
        function generateObstacles() {
            obstacles = [];
            
            // 生成5-8個障礙物
            const obstacleCount = Math.floor(Math.random() * 4) + 5;
            
            for (let i = 0; i < obstacleCount; i++) {
                let validPosition = false;
                let attempts = 0;
                
                while (!validPosition && attempts < 50) {
                    const x = Math.floor(Math.random() * tileCount);
                    const y = Math.floor(Math.random() * tileCount);
                    
                    validPosition = true;
                    attempts++;
                    
                    // 避免在玩家起始位置附近生成
                    if ((Math.abs(x - 10) < 3 && Math.abs(y - 10) < 3) ||
                        (Math.abs(x - 15) < 3 && Math.abs(y - 15) < 3)) {
                        validPosition = false;
                        continue;
                    }
                    
                    // 檢查是否與其他障礙物重疊
                    for (let obstacle of obstacles) {
                        if (x === obstacle.x && y === obstacle.y) {
                            validPosition = false;
                            break;
                        }
                    }
                    
                    if (validPosition) {
                        obstacles.push({ x: x, y: y });
                    }
                }
            }
        }

        function showPunishmentModal(loser) {
            const punishment = getRandomPunishment();
            const modal = document.getElementById('punishmentModal');
            const emoji = document.getElementById('punishmentEmoji');
            const text = document.getElementById('punishmentText');
            
            emoji.textContent = punishment.emoji;
            text.textContent = `${loser}輸了！${punishment.text}`;
            
            modal.style.display = 'flex';
            
            // 添加點擊外部關閉功能
            modal.onclick = function(e) {
                if (e.target === modal) {
                    closePunishmentModal();
                }
            };
        }

        function closePunishmentModal() {
            const modal = document.getElementById('punishmentModal');
            modal.style.display = 'none';
        }

        // 繪製遊戲
        function drawGame() {
            // 清除畫布
            ctx.fillStyle = '#2c3e50';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // 繪製網格
            ctx.strokeStyle = '#34495e';
            ctx.lineWidth = 1;
            for (let i = 0; i <= tileCount; i++) {
                ctx.beginPath();
                ctx.moveTo(i * gridSize, 0);
                ctx.lineTo(i * gridSize, canvas.height);
                ctx.stroke();
                
                ctx.beginPath();
                ctx.moveTo(0, i * gridSize);
                ctx.lineTo(canvas.width, i * gridSize);
                ctx.stroke();
            }

            // 繪製障礙物
            ctx.fillStyle = '#95a5a6';
            ctx.shadowColor = '#7f8c8d';
            ctx.shadowBlur = 5;
            for (let obstacle of obstacles) {
                ctx.fillRect(obstacle.x * gridSize + 1, obstacle.y * gridSize + 1, gridSize - 2, gridSize - 2);
                
                // 繪製障礙物紋理
                ctx.fillStyle = '#7f8c8d';
                ctx.fillRect(obstacle.x * gridSize + 3, obstacle.y * gridSize + 3, gridSize - 6, gridSize - 6);
                ctx.fillStyle = '#95a5a6';
            }
            ctx.shadowBlur = 0;

            // 繪製食物
            for (let food of foods) {
                ctx.fillStyle = food.color;
                ctx.shadowColor = food.color;
                ctx.shadowBlur = 8;
                
                // 繪製食物背景
                ctx.fillRect(food.x * gridSize + 2, food.y * gridSize + 2, gridSize - 4, gridSize - 4);
                
                // 繪製食物表情符號
                ctx.shadowBlur = 0;
                ctx.fillStyle = '#fff';
                ctx.font = `${gridSize - 6}px Arial`;
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText(
                    food.emoji, 
                    food.x * gridSize + gridSize / 2, 
                    food.y * gridSize + gridSize / 2
                );
            }

            // 繪製玩家1
            drawSnake(player1, playerEffects.player1.speed);
            
            // 繪製玩家2
            drawSnake(player2, playerEffects.player2.speed);
        }

        function drawSnake(player, hasSpeedEffect) {
            ctx.fillStyle = player.color;
            ctx.shadowColor = player.color;
            ctx.shadowBlur = hasSpeedEffect ? 10 : 5;
            
            // 繪製蛇身
            for (let segment of player.tail) {
                ctx.fillRect(segment.x * gridSize + 1, segment.y * gridSize + 1, gridSize - 2, gridSize - 2);
            }
            
            // 繪製蛇頭（如果有速度效果則閃爍）
            if (hasSpeedEffect) {
                ctx.fillStyle = Math.random() > 0.5 ? player.color : '#fff';
            }
            ctx.fillRect(player.x * gridSize + 1, player.y * gridSize + 1, gridSize - 2, gridSize - 2);
            ctx.shadowBlur = 0;
        }

        function updateGame() {
            if (!gameRunning || gamePaused) return;

            // 更新玩家效果計時器
            updatePlayerEffects();

            // 移動玩家1
            movePlayer(player1, 'player1');
            
            // 移動玩家2
            movePlayer(player2, 'player2');

            // 檢查碰撞
            checkCollisions();

            // 更新分數顯示
            scorePlayer1.textContent = player1.score;
            scorePlayer2.textContent = player2.score;

            drawGame();
        }

        function updatePlayerEffects() {
            // 更新玩家1速度效果
            if (playerEffects.player1.speed) {
                playerEffects.player1.speedTimer--;
                if (playerEffects.player1.speedTimer <= 0) {
                    playerEffects.player1.speed = false;
                }
            }
            
            // 更新玩家2速度效果
            if (playerEffects.player2.speed) {
                playerEffects.player2.speedTimer--;
                if (playerEffects.player2.speedTimer <= 0) {
                    playerEffects.player2.speed = false;
                }
            }
        }

        function movePlayer(player, playerKey) {
            // 添加當前位置到尾巴
            player.tail.push({x: player.x, y: player.y});

            // 移動頭部
            player.x += player.dx;
            player.y += player.dy;

            // 邊界檢查
            if (player.x < 0) player.x = tileCount - 1;
            if (player.x >= tileCount) player.x = 0;
            if (player.y < 0) player.y = tileCount - 1;
            if (player.y >= tileCount) player.y = 0;

            // 檢查是否吃到食物
            let ateFood = false;
            for (let i = foods.length - 1; i >= 0; i--) {
                const food = foods[i];
                if (player.x === food.x && player.y === food.y) {
                    ateFood = true;
                    player.score += food.points;
                    
                    // 應用食物效果
                    applyFoodEffect(player, playerKey, food.effect);
                    
                    // 移除被吃掉的食物
                    foods.splice(i, 1);
                    
                    // 如果所有食物都被吃完，生成新的食物
                    if (foods.length === 0) {
                        generateFood();
                    }
                    break;
                }
            }
            
            if (!ateFood) {
                // 移除尾巴最後一段
                player.tail.shift();
            }
        }

        function applyFoodEffect(player, playerKey, effect) {
            switch (effect) {
                case 'speed':
                    playerEffects[playerKey].speed = true;
                    playerEffects[playerKey].speedTimer = 50; // 持續約5秒
                    break;
                case 'shrink':
                    // 縮小蛇身（移除最後兩段）
                    if (player.tail.length > 2) {
                        player.tail.splice(0, 2);
                    }
                    break;
                case 'golden':
                case 'bonus':
                case 'normal':
                default:
                    // 這些食物只增加分數，沒有特殊效果
                    break;
            }
        }

        function checkCollisions() {
            // 檢查玩家1撞到障礙物
            for (let obstacle of obstacles) {
                if (player1.x === obstacle.x && player1.y === obstacle.y) {
                    endGame('玩家二獲勝！玩家一撞到障礙物了！', '玩家一');
                    return;
                }
            }

            // 檢查玩家2撞到障礙物
            for (let obstacle of obstacles) {
                if (player2.x === obstacle.x && player2.y === obstacle.y) {
                    endGame('玩家一獲勝！玩家二撞到障礙物了！', '玩家二');
                    return;
                }
            }

            // 檢查玩家1自撞
            for (let segment of player1.tail) {
                if (player1.x === segment.x && player1.y === segment.y) {
                    endGame('玩家二獲勝！玩家一撞到自己了！', '玩家一');
                    return;
                }
            }

            // 檢查玩家2自撞
            for (let segment of player2.tail) {
                if (player2.x === segment.x && player2.y === segment.y) {
                    endGame('玩家一獲勝！玩家二撞到自己了！', '玩家二');
                    return;
                }
            }

            // 檢查玩家互撞
            if (player1.x === player2.x && player1.y === player2.y) {
                if (player1.score > player2.score) {
                    endGame('玩家一獲勝！分數較高！', '玩家二');
                } else if (player2.score > player1.score) {
                    endGame('玩家二獲勝！分數較高！', '玩家一');
                } else {
                    endGame('平手！兩位玩家同時碰撞！', null);
                }
                return;
            }

            // 檢查玩家1撞到玩家2身體
            for (let segment of player2.tail) {
                if (player1.x === segment.x && player1.y === segment.y) {
                    endGame('玩家二獲勝！玩家一撞到玩家二！', '玩家一');
                    return;
                }
            }

            // 檢查玩家2撞到玩家1身體
            for (let segment of player1.tail) {
                if (player2.x === segment.x && player2.y === segment.y) {
                    endGame('玩家一獲勝！玩家二撞到玩家一！', '玩家二');
                    return;
                }
            }
        }



        function startGame() {
            if (gameRunning) return;
            
            gameRunning = true;
            gamePaused = false;
            gameStatus.textContent = '遊戲進行中...';
            winnerDisplay.innerHTML = '';
            
            // 生成障礙物和食物
            generateObstacles();
            generateFood();
            
            gameLoop = setInterval(updateGame, 150);
            
            startBtn.textContent = '遊戲中';
            startBtn.disabled = true;
            pauseBtn.disabled = false;
        }

        function pauseGame() {
            if (!gameRunning) return;
            
            gamePaused = !gamePaused;
            
            if (gamePaused) {
                gameStatus.textContent = '遊戲已暫停';
                pauseBtn.textContent = '繼續';
            } else {
                gameStatus.textContent = '遊戲進行中...';
                pauseBtn.textContent = '暫停';
            }
        }

        function resetGame() {
            gameRunning = false;
            gamePaused = false;
            clearInterval(gameLoop);
            
            // 重置玩家狀態
            player1 = {
                x: 10,
                y: 10,
                dx: 0,
                dy: 0,
                tail: [],
                score: 0,
                color: '#f1c40f'
            };
            
            player2 = {
                x: 15,
                y: 15,
                dx: 0,
                dy: 0,
                tail: [],
                score: 0,
                color: '#e74c3c'
            };
            
            // 重置玩家效果
            playerEffects = {
                player1: { speed: false, speedTimer: 0 },
                player2: { speed: false, speedTimer: 0 }
            };
            
            // 清空食物和障礙物
            foods = [];
            obstacles = [];
            
            scorePlayer1.textContent = '0';
            scorePlayer2.textContent = '0';
            gameStatus.textContent = '準備開始遊戲！';
            winnerDisplay.innerHTML = '';
            
            startBtn.textContent = '開始遊戲';
            startBtn.disabled = false;
            pauseBtn.textContent = '暫停';
            pauseBtn.disabled = true;
            
            drawGame();
        }

        function endGame(message, loser) {
            gameRunning = false;
            clearInterval(gameLoop);
            
            gameStatus.textContent = '遊戲結束';
            winnerDisplay.innerHTML = `<div class="winner-announcement">${message}</div>`;
            
            startBtn.textContent = '開始遊戲';
            startBtn.disabled = false;
            pauseBtn.disabled = true;
            
            // 如果有輸家，顯示懲罰彈窗
            if (loser) {
                setTimeout(() => {
                    showPunishmentModal(loser);
                }, 1000); // 延遲1秒顯示懲罰彈窗
            }
        }

        // 鍵盤控制
        document.addEventListener('keydown', (e) => {
            if (!gameRunning || gamePaused) return;
            
            switch(e.key) {
                // 玩家1 (WASD)
                case 'w':
                case 'W':
                    if (player1.dy !== 1) {
                        player1.dx = 0;
                        player1.dy = -1;
                    }
                    break;
                case 's':
                case 'S':
                    if (player1.dy !== -1) {
                        player1.dx = 0;
                        player1.dy = 1;
                    }
                    break;
                case 'a':
                case 'A':
                    if (player1.dx !== 1) {
                        player1.dx = -1;
                        player1.dy = 0;
                    }
                    break;
                case 'd':
                case 'D':
                    if (player1.dx !== -1) {
                        player1.dx = 1;
                        player1.dy = 0;
                    }
                    break;
                
                // 玩家2 (方向鍵)
                case 'ArrowUp':
                    if (player2.dy !== 1) {
                        player2.dx = 0;
                        player2.dy = -1;
                    }
                    break;
                case 'ArrowDown':
                    if (player2.dy !== -1) {
                        player2.dx = 0;
                        player2.dy = 1;
                    }
                    break;
                case 'ArrowLeft':
                    if (player2.dx !== 1) {
                        player2.dx = -1;
                        player2.dy = 0;
                    }
                    break;
                case 'ArrowRight':
                    if (player2.dx !== -1) {
                        player2.dx = 1;
                        player2.dy = 0;
                    }
                    break;
            }
        });

        // 觸控控制
        document.querySelectorAll('.control-button').forEach(button => {
            button.addEventListener('click', () => {
                if (!gameRunning || gamePaused) return;
                
                const player = button.dataset.player === '1' ? player1 : player2;
                const direction = button.dataset.direction;
                
                switch(direction) {
                    case 'up':
                        if (player.dy !== 1) {
                            player.dx = 0;
                            player.dy = -1;
                        }
                        break;
                    case 'down':
                        if (player.dy !== -1) {
                            player.dx = 0;
                            player.dy = 1;
                        }
                        break;
                    case 'left':
                        if (player.dx !== 1) {
                            player.dx = -1;
                            player.dy = 0;
                        }
                        break;
                    case 'right':
                        if (player.dx !== -1) {
                            player.dx = 1;
                            player.dy = 0;
                        }
                        break;
                }
            });
        });

        // 按鈕事件
        startBtn.addEventListener('click', startGame);
        pauseBtn.addEventListener('click', pauseGame);
        resetBtn.addEventListener('click', resetGame);

        // 初始化遊戲
        resetGame();
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9682a9d3e00aa3ac',t:'MTc1NDAyMzI4OS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
