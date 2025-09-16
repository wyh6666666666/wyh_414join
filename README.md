# wyh_414join
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>折觥策略跳棋：青铜器上的智慧对决</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        bronze: '#CD7F32',
                        darkBronze: '#8B4513',
                        lightBronze: '#D2B48C',
                        inscription: '#E67E22',
                        player1: '#34495E',
                        player2: '#E74C3C'
                    },
                    fontFamily: {
                        ancient: ['"Noto Serif SC"', 'serif']
                    }
                }
            }
        }
    </script>
    <style type="text/tailwindcss">
        @layer utilities {
            .piece-shadow {
                filter: drop-shadow(0 4px 3px rgb(0 0 0 / 0.3));
            }
            .board-pattern {
                background-image: url("data:image/svg+xml,%3Csvg width='100' height='100' viewBox='0 0 100 100' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M11 18c3.866 0 7-3.134 7-7s-3.134-7-7-7-7 3.134-7 7 3.134 7 7 7zm48 25c3.866 0 7-3.134 7-7s-3.134-7-7-7-7 3.134-7 7 3.134 7 7 7zm-43-7c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zm63 31c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zM34 90c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zm56-76c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zM12 86c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm28-65c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm23-11c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm-6 60c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm29 22c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zM32 63c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm57-13c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm-9-21c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM60 91c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM35 41c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM12 60c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2z' fill='%23d2b48c' fill-opacity='0.1' fill-rule='evenodd'/%3E%3C/svg%3E");
            }
            .inscription-mark {
                position: relative;
            }
            .inscription-mark::after {
                content: "铭文";
                position: absolute;
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%);
                font-size: 8px;
                color: rgba(230, 126, 34, 0.7);
                pointer-events: none;
            }
            .resource-bronze {
                background-color: rgba(205, 127, 50, 0.2);
            }
            .resource-land {
                background-color: rgba(46, 204, 113, 0.2);
            }
            .resource-slave {
                background-color: rgba(155, 89, 182, 0.2);
            }
            .piece-zuoce {
                border: 2px solid gold;
            }
        }
    </style>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;700&display=swap');
        
        .hex-board {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            grid-template-rows: repeat(7, 1fr);
            gap: 2px;
        }
        
        .hex-cell {
            position: relative;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        
        .hex-cell::before {
            content: '';
            position: absolute;
            width: 80%;
            height: 80%;
            background-color: inherit;
            transform: rotate(45deg);
            z-index: -1;
        }
        
        .hex-cell:hover {
            background-color: rgba(230, 126, 34, 0.2);
        }
        
        .piece {
            width: 70%;
            height: 70%;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: transform 0.3s ease;
            z-index: 1;
        }
        
        .piece:hover {
            transform: scale(1.1);
        }
        
        .piece-zuoce::after {
            content: "册";
            color: gold;
            font-weight: bold;
        }
        
        .selected {
            outline: 2px solid #f39c12;
            outline-offset: 2px;
        }
        
        .valid-move::after {
            content: '';
            position: absolute;
            width: 30%;
            height: 30%;
            background-color: rgba(46, 204, 113, 0.5);
            border-radius: 50%;
            z-index: 0;
        }
        
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }
        
        .turn-indicator {
            animation: pulse 2s infinite;
        }
        
        .card {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen font-ancient text-gray-800">
    <div class="container mx-auto px-4 py-8 max-w-6xl">
        <!-- 游戏标题 -->
        <header class="text-center mb-8">
            <h1 class="text-[clamp(1.8rem,5vw,2.8rem)] font-bold text-darkBronze mb-2">折觥策略跳棋</h1>
            <p class="text-gray-600 italic">青铜器上的智慧对决</p>
            <div class="mt-4 flex justify-center">
                <div class="h-1 w-32 bg-bronze rounded-full"></div>
            </div>
        </header>
        
        <!-- 游戏区域 -->
        <div class="flex flex-col lg:flex-row gap-8">
            <!-- 左侧：棋盘 -->
            <div class="lg:w-2/3">
                <div class="bg-white rounded-xl shadow-lg p-4 md:p-6 board-pattern">
                    <div class="relative mx-auto" style="width: min(90vw, 500px); height: min(90vw, 500px);">
                        <!-- 棋盘 -->
                        <div id="game-board" class="hex-board w-full h-full bg-lightBronze/30 rounded-lg p-2">
                            <!-- 棋盘格子将由JS动态生成 -->
                        </div>
                        
                        <!-- 胜利信息 -->
                        <div id="winner-modal" class="hidden absolute inset-0 bg-black/70 rounded-lg flex flex-col items-center justify-center text-white p-4">
                            <h2 id="winner-text" class="text-2xl md:text-3xl font-bold mb-6 text-center"></h2>
                            <button id="restart-btn" class="bg-bronze hover:bg-darkBronze text-white font-bold py-2 px-6 rounded-full transition-colors">
                                重新开始游戏
                            </button>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- 右侧：游戏信息和控制面板 -->
            <div class="lg:w-1/3 flex flex-col gap-6">
                <!-- 玩家信息 -->
                <div class="bg-white rounded-xl shadow-md p-5">
                    <h2 class="text-xl font-bold text-bronze mb-4 border-b border-lightBronze pb-2">游戏状态</h2>
                    
                    <div class="mb-4">
                        <div class="flex items-center justify-between mb-2">
                            <div class="flex items-center">
                                <div class="w-5 h-5 rounded-full bg-player1 mr-2"></div>
                                <span>玩家一</span>
                            </div>
                            <span id="player1-pieces" class="text-sm bg-gray-100 px-2 py-1 rounded">剩余棋子: 10</span>
                        </div>
                        
                        <div class="flex items-center justify-between">
                            <div class="flex items-center">
                                <div class="w-5 h-5 rounded-full bg-player2 mr-2"></div>
                                <span>玩家二</span>
                            </div>
                            <span id="player2-pieces" class="text-sm bg-gray-100 px-2 py-1 rounded">剩余棋子: 10</span>
                        </div>
                    </div>
                    
                    <div class="bg-gray-50 p-3 rounded-lg">
                        <p class="text-sm text-gray-600 mb-1">当前回合</p>
                        <div id="current-player" class="flex items-center">
                            <div class="w-6 h-6 rounded-full bg-player1 turn-indicator mr-2"></div>
                            <span class="font-medium">玩家一</span>
                        </div>
                    </div>
                </div>
                
                <!-- 资源收集 -->
                <div class="bg-white rounded-xl shadow-md p-5">
                    <h2 class="text-xl font-bold text-bronze mb-4 border-b border-lightBronze pb-2">资源收集</h2>
                    
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <h3 class="text-sm font-medium text-gray-600 mb-2">玩家一</h3>
                            <div class="space-y-2">
                                <div class="flex items-center text-sm">
                                    <i class="fa fa-cube text-bronze mr-2"></i>
                                    <span>青铜: <span id="player1-bronze">0</span></span>
                                </div>
                                <div class="flex items-center text-sm">
                                    <i class="fa fa-tree text-green-600 mr-2"></i>
                                    <span>土地: <span id="player1-land">0</span></span>
                                </div>
                                <div class="flex items-center text-sm">
                                    <i class="fa fa-users text-purple-600 mr-2"></i>
                                    <span>奴隶: <span id="player1-slave">0</span></span>
                                </div>
                            </div>
                        </div>
                        
                        <div>
                            <h3 class="text-sm font-medium text-gray-600 mb-2">玩家二</h3>
                            <div class="space-y-2">
                                <div class="flex items-center text-sm">
                                    <i class="fa fa-cube text-bronze mr-2"></i>
                                    <span>青铜: <span id="player2-bronze">0</span></span>
                                </div>
                                <div class="flex items-center text-sm">
                                    <i class="fa fa-tree text-green-600 mr-2"></i>
                                    <span>土地: <span id="player2-land">0</span></span>
                                </div>
                                <div class="flex items-center text-sm">
                                    <i class="fa fa-users text-purple-600 mr-2"></i>
                                    <span>奴隶: <span id="player2-slave">0</span></span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- 铭文卡片 -->
                <div class="bg-white rounded-xl shadow-md p-5">
                    <h2 class="text-xl font-bold text-bronze mb-4 border-b border-lightBronze pb-2">铭文收集</h2>
                    
                    <div id="inscription-cards" class="grid grid-cols-2 gap-2">
                        <!-- 铭文卡片将由JS动态生成 -->
                    </div>
                    
                    <div class="mt-4 text-xs text-gray-500 italic">
                        收集全部铭文关键词可获得胜利加成
                    </div>
                </div>
                
                <!-- 游戏规则简介 -->
                <div class="bg-white rounded-xl shadow-md p-5">
                    <h2 class="text-xl font-bold text-bronze mb-4 border-b border-lightBronze pb-2">游戏规则</h2>
                    
                    <ul class="text-sm space-y-2 text-gray-700">
                        <li>• 将所有棋子移动到对方底线</li>
                        <li>• 或收集全部铭文关键词获胜</li>
                        <li>• "作册折"棋子有特殊移动能力</li>
                        <li>• 特殊区域可收集资源</li>
                        <li>• 点击棋子选择，再点击目标位置移动</li>
                    </ul>
                </div>
            </div>
        </div>
        
        <!-- 页脚 -->
        <footer class="mt-12 text-center text-gray-500 text-sm">
            <p>《折觥策略跳棋》© 2023 - 基于西周青铜器"折觥"设计的策略游戏</p>
        </footer>
    </div>

    <script>
        // 游戏状态
        const gameState = {
            // 棋盘状态: null(空), 'p1'(玩家1), 'p2'(玩家2), 'p1z'(玩家1的作册折), 'p2z'(玩家2的作册折)
            board: Array(7).fill().map(() => Array(7).fill(null)),
            currentPlayer: 'p1', // 'p1' 或 'p2'
            selectedPiece: null, // {row, col}
            validMoves: [],
            resources: {
                p1: { bronze: 0, land: 0, slave: 0 },
                p2: { bronze: 0, land: 0, slave: 0 }
            },
            inscriptions: {
                p1: [],
                p2: []
            },
            inscriptionDeck: ['昭王', '作册折', '土地', '青铜', '奴隶', '父亲乙']
        };
        
        // 特殊区域配置
        const specialAreas = {
            // 铭文区
            inscription: [
                {row: 2, col: 3}, {row: 3, col: 2}, {row: 3, col: 3}, 
                {row: 3, col: 4}, {row: 4, col: 3}
            ],
            // 青铜铸造区
            bronze: [
                {row: 1, col: 2}, {row: 1, col: 4}, 
                {row: 5, col: 2}, {row: 5, col: 4}
            ],
            // 土地赏赐区
            land: [
                {row: 2, col: 1}, {row: 2, col: 5}, 
                {row: 4, col: 1}, {row: 4, col: 5}
            ],
            // 奴隶区
            slave: [
                {row: 0, col: 3}, {row: 6, col: 3}
            ]
        };
        
        // 初始化棋盘
        function initializeBoard() {
            const boardElement = document.getElementById('game-board');
            boardElement.innerHTML = '';
            
            // 创建棋盘格子
            for (let row = 0; row < 7; row++) {
                for (let col = 0; col < 7; col++) {
                    const cell = document.createElement('div');
                    cell.className = 'hex-cell bg-white/60';
                    cell.dataset.row = row;
                    cell.dataset.col = col;
                    
                    // 标记特殊区域
                    if (specialAreas.inscription.some(area => area.row === row && area.col === col)) {
                        cell.classList.add('inscription-mark');
                    }
                    if (specialAreas.bronze.some(area => area.row === row && area.col === col)) {
                        cell.classList.add('resource-bronze');
                    }
                    if (specialAreas.land.some(area => area.row === row && area.col === col)) {
                        cell.classList.add('resource-land');
                    }
                    if (specialAreas.slave.some(area => area.row === row && area.col === col)) {
                        cell.classList.add('resource-slave');
                    }
                    
                    // 添加点击事件
                    cell.addEventListener('click', () => handleCellClick(row, col));
                    
                    boardElement.appendChild(cell);
                }
            }
            
            // 初始化棋子位置
            // 玩家1棋子 (顶部)
            gameState.board[0][2] = 'p1';
            gameState.board[0][3] = 'p1';
            gameState.board[0][4] = 'p1';
            gameState.board[1][1] = 'p1';
            gameState.board[1][2] = 'p1z'; // 作册折
            gameState.board[1][3] = 'p1';
            gameState.board[1][4] = 'p1';
            gameState.board[1][5] = 'p1';
            gameState.board[2][0] = 'p1';
            gameState.board[2][1] = 'p1';
            
            // 玩家2棋子 (底部)
            gameState.board[6][2] = 'p2';
            gameState.board[6][3] = 'p2';
            gameState.board[6][4] = 'p2';
            gameState.board[5][1] = 'p2';
            gameState.board[5][2] = 'p2';
            gameState.board[5][3] = 'p2z'; // 作册折
            gameState.board[5][4] = 'p2';
            gameState.board[5][5] = 'p2';
            gameState.board[4][5] = 'p2';
            gameState.board[4][6] = 'p2';
            
            // 初始化铭文卡片
            initializeInscriptionCards();
            
            // 渲染棋盘
            renderBoard();
        }
        
        // 初始化铭文卡片
        function initializeInscriptionCards() {
            const cardsContainer = document.getElementById('inscription-cards');
            cardsContainer.innerHTML = '';
            
            // 打乱铭文牌组
            gameState.inscriptionDeck = shuffleArray([...gameState.inscriptionDeck]);
            
            // 创建铭文卡片
            gameState.inscriptionDeck.forEach((text, index) => {
                const card = document.createElement('div');
                card.className = 'card bg-lightBronze/20 rounded-lg p-2 text-center text-xs md:text-sm';
                card.textContent = text;
                card.dataset.index = index;
                cardsContainer.appendChild(card);
            });
        }
        
        // 洗牌函数
        function shuffleArray(array) {
            const newArray = [...array];
            for (let i = newArray.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [newArray[i], newArray[j]] = [newArray[j], newArray[i]];
            }
            return newArray;
        }
        
        // 渲染棋盘
        function renderBoard() {
            // 清除所有选中状态和有效移动标记
            document.querySelectorAll('.selected').forEach(el => el.classList.remove('selected'));
            document.querySelectorAll('.valid-move').forEach(el => el.classList.remove('valid-move'));
            
            // 渲染棋子
            document.querySelectorAll('.hex-cell').forEach(cell => {
                const row = parseInt(cell.dataset.row);
                const col = parseInt(cell.dataset.col);
                const piece = gameState.board[row][col];
                
                // 清除现有棋子
                while (cell.firstChild) {
                    cell.removeChild(cell.firstChild);
                }
                
                // 添加棋子
                if (piece) {
                    const isZuoce = piece.endsWith('z');
                    const player = piece.startsWith('p1') ? 'p1' : 'p2';
                    
                    const pieceElement = document.createElement('div');
                    pieceElement.className = `piece piece-shadow ${
                        player === 'p1' ? 'bg-player1' : 'bg-player2'
                    } ${isZuoce ? 'piece-zuoce' : ''}`;
                    
                    cell.appendChild(pieceElement);
                }
                
                // 标记选中的棋子
                if (gameState.selectedPiece && 
                    gameState.selectedPiece.row === row && 
                    gameState.selectedPiece.col === col) {
                    cell.classList.add('selected');
                }
                
                // 标记有效移动
                if (gameState.validMoves.some(move => move.row === row && move.col === col)) {
                    cell.classList.add('valid-move');
                }
            });
            
            // 更新玩家信息
            updatePlayerInfo();
            
            // 更新回合显示
            const currentPlayerEl = document.getElementById('current-player');
            currentPlayerEl.innerHTML = `
                <div class="w-6 h-6 rounded-full ${gameState.currentPlayer === 'p1' ? 'bg-player1' : 'bg-player2'} turn-indicator mr-2"></div>
                <span class="font-medium">${gameState.currentPlayer === 'p1' ? '玩家一' : '玩家二'}</span>
            `;
            
            // 更新资源显示
            updateResourcesDisplay();
        }
        
        // 更新玩家信息
        function updatePlayerInfo() {
            // 计算剩余棋子数量
            let p1Count = 0, p2Count = 0;
            gameState.board.forEach(row => {
                row.forEach(cell => {
                    if (cell === 'p1' || cell === 'p1z') p1Count++;
                    if (cell === 'p2' || cell === 'p2z') p2Count++;
                });
            });
            
            document.getElementById('player1-pieces').textContent = `剩余棋子: ${p1Count}`;
            document.getElementById('player2-pieces').textContent = `剩余棋子: ${p2Count}`;
        }
        
        // 更新资源显示
        function updateResourcesDisplay() {
            // 玩家一资源
            document.getElementById('player1-bronze').textContent = gameState.resources.p1.bronze;
            document.getElementById('player1-land').textContent = gameState.resources.p1.land;
            document.getElementById('player1-slave').textContent = gameState.resources.p1.slave;
            
            // 玩家二资源
            document.getElementById('player2-bronze').textContent = gameState.resources.p2.bronze;
            document.getElementById('player2-land').textContent = gameState.resources.p2.land;
            document.getElementById('player2-slave').textContent = gameState.resources.p2.slave;
            
            // 更新铭文收集显示
            document.querySelectorAll('#inscription-cards .card').forEach((card, index) => {
                const text = gameState.inscriptionDeck[index];
                if (gameState.inscriptions.p1.includes(text)) {
                    card.classList.add('bg-player1/20');
                    card.classList.remove('bg-lightBronze/20');
                } else if (gameState.inscriptions.p2.includes(text)) {
                    card.classList.add('bg-player2/20');
                    card.classList.remove('bg-lightBronze/20');
                } else {
                    card.classList.remove('bg-player1/20', 'bg-player2/20');
                    card.classList.add('bg-lightBronze/20');
                }
            });
        }
        
        // 处理格子点击
        function handleCellClick(row, col) {
            const piece = gameState.board[row][col];
            
            // 如果点击了当前玩家的棋子，则选中它
            if (piece && piece.startsWith(gameState.currentPlayer)) {
                gameState.selectedPiece = { row, col };
                gameState.validMoves = getValidMoves(row, col);
                renderBoard();
                return;
            }
            
            // 如果已经选中棋子，并且点击了有效移动位置
            if (gameState.selectedPiece && 
                gameState.validMoves.some(move => move.row === row && move.col === col)) {
                
                const fromRow = gameState.selectedPiece.row;
                const fromCol = gameState.selectedPiece.col;
                const movingPiece = gameState.board[fromRow][fromCol];
                
                // 执行移动
                gameState.board[row][col] = movingPiece;
                gameState.board[fromRow][fromCol] = null;
                
                // 检查是否跳过了对方棋子（吃子）
                const isJump = Math.abs(row - fromRow) > 1 || Math.abs(col - fromCol) > 1;
                if (isJump) {
                    handleJumpCapture(fromRow, fromCol, row, col);
                }
                
                // 检查是否进入特殊区域并获取资源
                handleSpecialAreaRewards(row, col);
                
                // 检查胜利条件
                if (checkWin()) {
                    showWinner();
                    return;
                }
                
                // 切换玩家
                gameState.currentPlayer = gameState.currentPlayer === 'p1' ? 'p2' : 'p1';
                
                // 重置选择状态
                gameState.selectedPiece = null;
                gameState.validMoves = [];
                
                renderBoard();
            }
        }
        
        // 处理跳跃吃子
        function handleJumpCapture(fromRow, fromCol, toRow, toCol) {
            // 计算被跳过的棋子位置
            const capturedRow = Math.floor((fromRow + toRow) / 2);
            const capturedCol = Math.floor((fromCol + toCol) / 2);
            
            // 检查是否有被跳过的敌方棋子
            const capturedPiece = gameState.board[capturedRow][capturedCol];
            if (capturedPiece && !capturedPiece.startsWith(gameState.currentPlayer)) {
                // 移除被吃的棋子
                gameState.board[capturedRow][capturedCol] = null;
                
                // 获得青铜资源
                gameState.resources[gameState.currentPlayer].bronze += 1;
            }
        }
        
        // 处理特殊区域奖励
        function handleSpecialAreaRewards(row, col) {
            // 检查是否进入铭文区
            if (specialAreas.inscription.some(area => area.row === row && area.col === col)) {
                // 随机获得一个未收集的铭文
                const availableInscriptions = gameState.inscriptionDeck.filter(
                    text => !gameState.inscriptions[gameState.currentPlayer].includes(text)
                );
                
                if (availableInscriptions.length > 0) {
                    const randomIndex = Math.floor(Math.random() * availableInscriptions.length);
                    const newInscription = availableInscriptions[randomIndex];
                    
                    // 添加到玩家的铭文收集
                    if (!gameState.inscriptions[gameState.currentPlayer].includes(newInscription)) {
                        gameState.inscriptions[gameState.currentPlayer].push(newInscription);
                    }
                }
            }
            
            // 检查是否进入青铜铸造区
            if (specialAreas.bronze.some(area => area.row === row && area.col === col)) {
                gameState.resources[gameState.currentPlayer].bronze += 1;
            }
            
            // 检查是否进入土地赏赐区
            if (specialAreas.land.some(area => area.row === row && area.col === col)) {
                gameState.resources[gameState.currentPlayer].land += 1;
            }
            
            // 检查是否进入奴隶区
            if (specialAreas.slave.some(area => area.row === row && area.col === col)) {
                gameState.resources[gameState.currentPlayer].slave += 1;
            }
        }
        
        // 获取有效移动
        function getValidMoves(row, col) {
            const moves = [];
            const piece = gameState.board[row][col];
            const isZuoce = piece.endsWith('z'); // 是否为作册折棋子
            
            // 移动方向: 玩家1向下，玩家2向上
            const directions = piece.startsWith('p1') ? 
                [[1, -1], [1, 0], [1, 1], [0, -1], [0, 1]] : 
                [[-1, -1], [-1, 0], [-1, 1], [0, -1], [0, 1]];
            
            // 作册折可以向任何方向移动
            const allDirections = [[-1, -1], [-1, 0], [-1, 1], 
                                 [0, -1],          [0, 1],
                                 [1, -1],  [1, 0], [1, 1]];
            
            const possibleDirections = isZuoce ? allDirections : directions;
            
            // 检查基本移动
            possibleDirections.forEach(([dr, dc]) => {
                const newRow = row + dr;
                const newCol = col + dc;
                
                // 检查是否在棋盘范围内
                if (newRow >= 0 && newRow < 7 && newCol >= 0 && newCol < 7) {
                    // 检查目标位置是否为空
                    if (gameState.board[newRow][newCol] === null) {
                        moves.push({ row: newRow, col: newCol });
                    } 
                    // 检查是否可以跳跃
                    else if (!gameState.board[newRow][newCol].startsWith(gameState.currentPlayer)) {
                        // 跳跃后的位置
                        const jumpRow = newRow + dr;
                        const jumpCol = newCol + dc;
                        
                        if (jumpRow >= 0 && jumpRow < 7 && jumpCol >= 0 && jumpCol < 7 &&
                            gameState.board[jumpRow][jumpCol] === null) {
                            moves.push({ row: jumpRow, col: jumpCol });
                        }
                    }
                }
            });
            
            return moves;
        }
        
        // 检查胜利条件
        function checkWin() {
            // 条件1: 收集所有铭文
            if (gameState.inscriptions[gameState.currentPlayer].length === gameState.inscriptionDeck.length) {
                return true;
            }
            
            // 条件2: 所有棋子到达对方底线
            const targetRows = gameState.currentPlayer === 'p1' ? [6] : [0];
            let allReached = true;
            let hasPieces = false;
            
            gameState.board.forEach((row, rowIdx) => {
                row.forEach((cell, colIdx) => {
                    if (cell && cell.startsWith(gameState.currentPlayer)) {
                        hasPieces = true;
                        if (!targetRows.includes(rowIdx)) {
                            allReached = false;
                        }
                    }
                });
            });
            
            if (hasPieces && allReached) {
                return true;
            }
            
            // 条件3: 对方没有棋子了
            let opponentHasPieces = false;
            const opponent = gameState.currentPlayer === 'p1' ? 'p2' : 'p1';
            
            gameState.board.forEach(row => {
                row.forEach(cell => {
                    if (cell && cell.startsWith(opponent)) {
                        opponentHasPieces = true;
                    }
                });
            });
            
            return !opponentHasPieces;
        }
        
        // 显示胜利者
        function showWinner() {
            const winnerText = document.getElementById('winner-text');
            winnerText.textContent = `${gameState.currentPlayer === 'p1' ? '玩家一' : '玩家二'}获胜！`;
            document.getElementById('winner-modal').classList.remove('hidden');
        }
        
        // 重新开始游戏
        document.getElementById('restart-btn').addEventListener('click', () => {
            // 重置游戏状态
            gameState.board = Array(7).fill().map(() => Array(7).fill(null));
            gameState.currentPlayer = 'p1';
            gameState.selectedPiece = null;
            gameState.validMoves = [];
            gameState.resources = {
                p1: { bronze: 0, land: 0, slave: 0 },
                p2: { bronze: 0, land: 0, slave: 0 }
            };
            gameState.inscriptions = {
                p1: [],
                p2: []
            };
            
            // 隐藏胜利模态框
            document.getElementById('winner-modal').classList.add('hidden');
            
            // 重新初始化游戏
            initializeBoard();
        });
        
        // 初始化游戏
        initializeBoard();
    </script>
</body>
</html>
