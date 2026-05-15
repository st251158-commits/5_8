<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>テトリス風ゲーム</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&family=Noto+Sans+JP:wght@400;700&display=swap');
        
        body {
            font-family: 'Noto Sans JP', sans-serif;
            background-color: #111827; /* Tailwind gray-900 */
            color: white;
            touch-action: none; /* スマホでのスワイプによるスクロールを防止 */
        }

        .game-title {
            font-family: 'Press Start 2P', 'Noto Sans JP', cursive;
            text-shadow: 2px 2px 0px #ef4444, -2px -2px 0px #3b82f6;
        }

        canvas {
            display: block;
            background-color: #000;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
            border: 2px solid #374151;
            border-radius: 4px;
        }

        /* スマホ用ボタンのスタイル */
        .control-btn {
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }
        
        .control-btn:active {
            transform: scale(0.95);
            background-color: #4b5563;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col items-center justify-center p-4">

    <div class="max-w-4xl w-full flex flex-col items-center gap-6">
        
        <!-- ヘッダー -->
        <div class="text-center">
            <h1 class="text-3xl md:text-4xl font-bold game-title mb-2 tracking-wider">TETRIS CLONE</h1>
            <p class="text-gray-400 text-sm">矢印キーで移動・回転、スペースで一気に落下</p>
        </div>

        <!-- メインゲームエリア -->
        <div class="flex flex-col md:flex-row gap-6 items-start justify-center w-full">
            
            <!-- キャンバス（ゲーム画面） -->
            <div class="relative group">
                <canvas id="tetris" width="300" height="600" class="w-full max-w-[300px] h-auto aspect-[1/2]"></canvas>
                
                <!-- オーバーレイ (スタート、一時停止、ゲームオーバー) -->
                <div id="game-overlay" class="absolute inset-0 bg-black/80 flex flex-col items-center justify-center rounded backdrop-blur-sm z-10 transition-opacity">
                    <h2 id="overlay-title" class="text-2xl font-bold mb-4 text-white">準備完了</h2>
                    <p id="overlay-score" class="text-lg text-yellow-400 mb-6 hidden">最終スコア: <span id="final-score">0</span></p>
                    <button id="start-btn" class="bg-blue-600 hover:bg-blue-500 text-white font-bold py-3 px-8 rounded-full shadow-lg shadow-blue-500/50 transition-all transform hover:scale-105 active:scale-95">
                        スタート
                    </button>
                </div>
            </div>

            <!-- サイドパネル (スコア、次のブロック) -->
            <div class="flex flex-row md:flex-col gap-4 w-full md:w-48 justify-center">
                <!-- 次のブロック表示 -->
                <div class="bg-gray-800 p-4 rounded-xl border border-gray-700 flex flex-col items-center">
                    <h3 class="text-gray-400 text-sm font-bold mb-2 uppercase tracking-wide">Next</h3>
                    <canvas id="next-piece" width="120" height="120" class="bg-transparent border-none shadow-none w-24 h-24"></canvas>
                </div>

                <!-- スコアボード -->
                <div class="bg-gray-800 p-4 rounded-xl border border-gray-700 w-full flex-1 flex flex-col gap-3">
                    <div>
                        <h3 class="text-gray-400 text-xs font-bold uppercase tracking-wide">Score</h3>
                        <p id="score" class="text-2xl font-mono font-bold text-yellow-400">0</p>
                    </div>
                    <div>
                        <h3 class="text-gray-400 text-xs font-bold uppercase tracking-wide">Level</h3>
                        <p id="level" class="text-xl font-mono font-bold text-white">1</p>
                    </div>
                    <div>
                        <h3 class="text-gray-400 text-xs font-bold uppercase tracking-wide">Lines</h3>
                        <p id="lines" class="text-xl font-mono font-bold text-white">0</p>
                    </div>
                </div>
                
                <!-- コントロールボタン (PC用) -->
                <button id="pause-btn" class="hidden md:block bg-gray-700 hover:bg-gray-600 text-white font-bold py-2 px-4 rounded transition-colors w-full">
                    一時停止 (P)
                </button>
            </div>
        </div>

        <!-- モバイル用コントロールパネル (画面幅が小さい時のみ表示) -->
        <div class="md:hidden grid grid-cols-3 gap-2 w-full max-w-[300px] mt-2">
            <button id="btn-left" class="control-btn bg-gray-700 p-4 rounded-lg flex items-center justify-center active:bg-gray-600">
                <svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
            </button>
            <button id="btn-rotate" class="control-btn bg-blue-600 p-4 rounded-lg flex items-center justify-center active:bg-blue-500">
                <svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path></svg>
            </button>
            <button id="btn-right" class="control-btn bg-gray-700 p-4 rounded-lg flex items-center justify-center active:bg-gray-600">
                <svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3"></path></svg>
            </button>
            <button id="btn-drop" class="control-btn bg-red-600 p-4 rounded-lg flex items-center justify-center active:bg-red-500 col-span-1">
                <svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3"></path></svg>
            </button>
            <button id="btn-down" class="control-btn bg-gray-700 p-4 rounded-lg flex items-center justify-center active:bg-gray-600 col-span-2 text-sm font-bold">
                ソフトドロップ
            </button>
        </div>

    </div>

    <script>
        // --- 設定と定数 ---
        const COLS = 10;
        const ROWS = 20;
        const BLOCK_SIZE = 30; // 300px / 10cols
        const NEXT_BLOCK_SIZE = 24; // 次のブロックプレビュー用のサイズ

        const canvas = document.getElementById('tetris');
        const ctx = canvas.getContext('2d');
        const nextCanvas = document.getElementById('next-piece');
        const nextCtx = nextCanvas.getContext('2d');

        // ブロックの色 (0は空)
        const COLORS = [
            null,
            '#06b6d4', // I - シアン
            '#3b82f6', // J - ブルー
            '#f97316', // L - オレンジ
            '#eab308', // O - イエロー
            '#22c55e', // S - グリーン
            '#a855f7', // T - パープル
            '#ef4444'  // Z - レッド
        ];

        // テトロミノの形状
        const SHAPES = [
            [], // 0は空用
            [[0, 0, 0, 0], [1, 1, 1, 1], [0, 0, 0, 0], [0, 0, 0, 0]], // I
            [[2, 0, 0], [2, 2, 2], [0, 0, 0]], // J
            [[0, 0, 3], [3, 3, 3], [0, 0, 0]], // L
            [[4, 4], [4, 4]], // O
            [[0, 5, 5], [5, 5, 0], [0, 0, 0]], // S
            [[0, 6, 0], [6, 6, 6], [0, 0, 0]], // T
            [[7, 7, 0], [0, 7, 7], [0, 0, 0]]  // Z
        ];

        // --- ゲームのステート ---
        let board = [];
        let score = 0;
        let lines = 0;
        let level = 1;
        let isGameOver = false;
        let isPaused = false;
        let isPlaying = false;
        let animationId = null;

        // タイマー用
        let dropCounter = 0;
        let dropInterval = 1000; // 1秒ごとに落下
        let lastTime = 0;

        // ピースオブジェクト
        let player = {
            pos: {x: 0, y: 0},
            matrix: null,
        };
        let nextPlayerMatrix = null;

        // --- UI要素 ---
        const overlay = document.getElementById('game-overlay');
        const overlayTitle = document.getElementById('overlay-title');
        const startBtn = document.getElementById('start-btn');
        const overlayScore = document.getElementById('overlay-score');
        const finalScoreSpan = document.getElementById('final-score');
        const scoreEl = document.getElementById('score');
        const levelEl = document.getElementById('level');
        const linesEl = document.getElementById('lines');
        const pauseBtn = document.getElementById('pause-btn');

        // --- コアロジック ---

        // ボードの初期化
        function createMatrix(w, h) {
            const matrix = [];
            while (h--) {
                matrix.push(new Array(w).fill(0));
            }
            return matrix;
        }

        // ランダムなピースの生成
        function createPiece(typeIndex) {
            if (typeIndex === undefined) {
                // 1~7のランダムなインデックス
                typeIndex = Math.floor(Math.random() * 7) + 1;
            }
            return SHAPES[typeIndex];
        }

        // 衝突判定
        function collide(board, player) {
            const m = player.matrix;
            const o = player.pos;
            for (let y = 0; y < m.length; ++y) {
                for (let x = 0; x < m[y].length; ++x) {
                    if (m[y][x] !== 0 &&
                       (board[y + o.y] && board[y + o.y][x + o.x]) !== 0) {
                        return true;
                    }
                }
            }
            return false;
        }

        // ピースをボードに固定する
        function merge(board, player) {
            player.matrix.forEach((row, y) => {
                row.forEach((value, x) => {
                    if (value !== 0) {
                        board[y + player.pos.y][x + player.pos.x] = value;
                    }
                });
            });
        }

        // 行の消去とスコア計算
        function sweep() {
            let rowCount = 0;
            outer: for (let y = board.length - 1; y >= 0; --y) {
                for (let x = 0; x < board[y].length; ++x) {
                    if (board[y][x] === 0) {
                        continue outer; // 空のマスがあれば次の行へ
                    }
                }
                // 行が揃っている場合
                const row = board.splice(y, 1)[0].fill(0); // 行を取り出して0で埋める
                board.unshift(row); // 一番上に追加
                ++y; // ずれた分インデックスを調整
                rowCount++;
            }

            if (rowCount > 0) {
                // テトリスの一般的なスコアリング
                const baseScores = [0, 40, 100, 300, 1200];
                score += baseScores[rowCount] * level;
                lines += rowCount;
                level = Math.floor(lines / 10) + 1;
                // レベルアップで速度上昇（最小50ms）
                dropInterval = Math.max(50, 1000 - (level - 1) * 100); 
                updateScoreBoard();
            }
        }

        // --- プレイヤーアクション ---

        // 落下
        function playerDrop() {
            player.pos.y++;
            if (collide(board, player)) {
                player.pos.y--;
                merge(board, player);
                playerReset();
                sweep();
            }
            dropCounter = 0; // 手動で落としたらタイマーリセット
        }

        // 一番下まで一気に落下（ハードドロップ）
        function playerHardDrop() {
            while (!collide(board, player)) {
                player.pos.y++;
            }
            player.pos.y--;
            merge(board, player);
            playerReset();
            sweep();
            dropCounter = 0;
        }

        // 左右移動
        function playerMove(dir) {
            player.pos.x += dir;
            if (collide(board, player)) {
                player.pos.x -= dir; // 衝突したら元に戻す
            }
        }

        // 行列の回転 (時計回り・反時計回り)
        function rotateMatrix(matrix, dir) {
            // 転置
            for (let y = 0; y < matrix.length; ++y) {
                for (let x = 0; x < y; ++x) {
                    [
                        matrix[x][y],
                        matrix[y][x]
                    ] = [
                        matrix[y][x],
                        matrix[x][y]
                    ];
                }
            }
            // 反転
            if (dir > 0) {
                matrix.forEach(row => row.reverse());
            } else {
                matrix.reverse();
            }
        }

        // ピースの回転と壁蹴り(簡易版)
        function playerRotate(dir) {
            const pos = player.pos.x;
            let offset = 1;
            rotateMatrix(player.matrix, dir);
            
            // 回転した結果、壁や他のブロックにめり込んだ場合の補正（壁蹴り）
            while (collide(board, player)) {
                player.pos.x += offset;
                offset = -(offset + (offset > 0 ? 1 : -1));
                
                // 補正してもダメなら回転をキャンセルして戻る
                if (offset > player.matrix[0].length) {
                    rotateMatrix(player.matrix, -dir);
                    player.pos.x = pos;
                    return;
                }
            }
        }

        // 新しいピースのセットアップ
        function playerReset() {
            if (!nextPlayerMatrix) {
                nextPlayerMatrix = createPiece();
            }
            player.matrix = nextPlayerMatrix;
            nextPlayerMatrix = createPiece();
            
            // 中央上部に配置
            player.pos.y = 0;
            player.pos.x = Math.floor(COLS / 2) - Math.floor(player.matrix[0].length / 2);

            // 出現直後に衝突する場合はゲームオーバー
            if (collide(board, player)) {
                gameOver();
            }
            
            drawNextPiece();
        }

        // --- 描画ロジック ---

        // ブロックを描画するヘルパー
        function drawMatrix(matrix, offset, ctxToDraw, blockSize, isGhost = false) {
            matrix.forEach((row, y) => {
                row.forEach((value, x) => {
                    if (value !== 0) {
                        const color = COLORS[value];
                        
                        if (isGhost) {
                            ctxToDraw.fillStyle = color + '40'; // 透明度を追加
                            ctxToDraw.strokeStyle = color + '80';
                        } else {
                            // メインカラー
                            ctxToDraw.fillStyle = color;
                            ctxToDraw.strokeStyle = '#000'; // 黒い縁取り
                        }

                        // 3Dっぽく見えるようにハイライトとシャドウを追加
                        const bx = (x + offset.x) * blockSize;
                        const by = (y + offset.y) * blockSize;
                        
                        ctxToDraw.fillRect(bx, by, blockSize, blockSize);
                        
                        if (!isGhost) {
                            // 内側の明るい線
                            ctxToDraw.fillStyle = 'rgba(255,255,255,0.3)';
                            ctxToDraw.fillRect(bx, by, blockSize, 4);
                            ctxToDraw.fillRect(bx, by, 4, blockSize);
                            // 内側の暗い線
                            ctxToDraw.fillStyle = 'rgba(0,0,0,0.3)';
                            ctxToDraw.fillRect(bx, by + blockSize - 4, blockSize, 4);
                            ctxToDraw.fillRect(bx + blockSize - 4, by, 4, blockSize);
                        }

                        ctxToDraw.strokeRect(bx, by, blockSize, blockSize);
                    }
                });
            });
        }

        // メイン描画ループ
        function draw() {
            // 背景クリア
            ctx.fillStyle = '#000';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // グリッド線を描画
            ctx.strokeStyle = '#222';
            ctx.lineWidth = 1;
            for (let x = 0; x <= COLS; x++) {
                ctx.beginPath();
                ctx.moveTo(x * BLOCK_SIZE, 0);
                ctx.lineTo(x * BLOCK_SIZE, canvas.height);
                ctx.stroke();
            }
            for (let y = 0; y <= ROWS; y++) {
                ctx.beginPath();
                ctx.moveTo(0, y * BLOCK_SIZE);
                ctx.lineTo(canvas.width, y * BLOCK_SIZE);
                ctx.stroke();
            }

            // ボードのブロックを描画
            drawMatrix(board, {x: 0, y: 0}, ctx, BLOCK_SIZE);

            if (isPlaying && !isGameOver) {
                // ゴースト（落下位置の予測）を描画
                const ghost = {
                    matrix: player.matrix,
                    pos: { x: player.pos.x, y: player.pos.y }
                };
                while (!collide(board, ghost)) {
                    ghost.pos.y++;
                }
                ghost.pos.y--;
                drawMatrix(ghost.matrix, ghost.pos, ctx, BLOCK_SIZE, true);

                // プレイヤーのブロックを描画
                drawMatrix(player.matrix, player.pos, ctx, BLOCK_SIZE);
            }
        }

        // 次のブロックを描画
        function drawNextPiece() {
            nextCtx.clearRect(0, 0, nextCanvas.width, nextCanvas.height);
            if (!nextPlayerMatrix) return;

            // 中央に配置するためのオフセット計算
            const offsetX = (nextCanvas.width / NEXT_BLOCK_SIZE - nextPlayerMatrix[0].length) / 2;
            const offsetY = (nextCanvas.height / NEXT_BLOCK_SIZE - nextPlayerMatrix.length) / 2;

            drawMatrix(nextPlayerMatrix, {x: offsetX, y: offsetY}, nextCtx, NEXT_BLOCK_SIZE);
        }

        // UIの更新
        function updateScoreBoard() {
            scoreEl.innerText = score;
            levelEl.innerText = level;
            linesEl.innerText = lines;
        }

        // --- ゲームループ ---
        function update(time = 0) {
            if (isPaused || isGameOver || !isPlaying) return;

            const deltaTime = time - lastTime;
            lastTime = time;

            dropCounter += deltaTime;
            if (dropCounter > dropInterval) {
                playerDrop();
            }

            draw();
            animationId = requestAnimationFrame(update);
        }

        // --- ゲーム状態管理 ---
        function startGame() {
            board = createMatrix(COLS, ROWS);
            score = 0;
            lines = 0;
            level = 1;
            dropInterval = 1000;
            isGameOver = false;
            isPaused = false;
            isPlaying = true;
            nextPlayerMatrix = null;
            
            updateScoreBoard();
            playerReset();
            
            overlay.classList.add('hidden');
            pauseBtn.innerText = "一時停止 (P)";
            
            lastTime = performance.now();
            if (animationId) cancelAnimationFrame(animationId);
            update(lastTime);
        }

        function gameOver() {
            isGameOver = true;
            isPlaying = false;
            if (animationId) cancelAnimationFrame(animationId);
            
            overlayTitle.innerText = "ゲームオーバー";
            overlayTitle.classList.replace('text-white', 'text-red-500');
            finalScoreSpan.innerText = score;
            overlayScore.classList.remove('hidden');
            startBtn.innerText = "もう一度プレイ";
            overlay.classList.remove('hidden');
        }

        function togglePause() {
            if (!isPlaying || isGameOver) return;
            
            isPaused = !isPaused;
            if (isPaused) {
                if (animationId) cancelAnimationFrame(animationId);
                overlayTitle.innerText = "一時停止中";
                overlayTitle.classList.replace('text-red-500', 'text-white');
                overlayScore.classList.add('hidden');
                startBtn.innerText = "再開する";
                overlay.classList.remove('hidden');
                pauseBtn.innerText = "再開 (P)";
            } else {
                overlay.classList.add('hidden');
                pauseBtn.innerText = "一時停止 (P)";
                lastTime = performance.now();
                update(lastTime);
            }
        }

        // --- イベントリスナー ---

        // キーボード操作
        document.addEventListener('keydown', event => {
            if (!isPlaying || isGameOver) {
                // EnterかSpaceでスタート
                if (event.key === 'Enter' || event.code === 'Space') {
                    if (isPaused) {
                        togglePause();
                    } else if (overlay.classList.contains('hidden') === false) {
                        startGame();
                    }
                }
                return;
            }

            switch (event.code) {
                case 'ArrowLeft':
                    if (!isPaused) playerMove(-1);
                    break;
                case 'ArrowRight':
                    if (!isPaused) playerMove(1);
                    break;
                case 'ArrowDown':
                    if (!isPaused) playerDrop();
                    break;
                case 'ArrowUp':
                    if (!isPaused) playerRotate(1);
                    break;
                case 'Space':
                    if (!isPaused) playerHardDrop();
                    event.preventDefault(); // スクロール防止
                    break;
                case 'KeyP':
                case 'Escape':
                    togglePause();
                    break;
            }
            if (!isPaused) draw(); // 操作後の即時描画
        });

        // ボタンクリックイベント (UI)
        startBtn.addEventListener('click', () => {
            if (isPaused) {
                togglePause();
            } else {
                startGame();
            }
        });
        pauseBtn.addEventListener('click', togglePause);

        // --- モバイル用タッチ/マウスクリック操作 ---
        function addControlListener(elementId, action, continuous = false) {
            const btn = document.getElementById(elementId);
            let intervalId = null;

            const startAction = (e) => {
                e.preventDefault(); // デフォルトアクションの防止（ズーム等）
                if (isPaused || isGameOver || !isPlaying) return;
                action();
                draw();
                if (continuous) {
                    // 長押し対応（最初は少し遅らせて、その後高速リピート）
                    setTimeout(() => {
                        if (intervalId === "pending") {
                            intervalId = setInterval(() => {
                                action();
                                draw();
                            }, 50); // リピート速度
                        }
                    }, 200); // 長押し判定のディレイ
                    intervalId = "pending";
                }
            };

            const endAction = (e) => {
                e.preventDefault();
                if (intervalId !== null && intervalId !== "pending") {
                    clearInterval(intervalId);
                }
                intervalId = null;
            };

            // タッチとマウス両方に対応
            btn.addEventListener('touchstart', startAction, {passive: false});
            btn.addEventListener('touchend', endAction, {passive: false});
            btn.addEventListener('touchcancel', endAction, {passive: false});
            
            btn.addEventListener('mousedown', startAction);
            btn.addEventListener('mouseup', endAction);
            btn.addEventListener('mouseleave', endAction);
        }

        // コントロールの紐付け
        addControlListener('btn-left', () => playerMove(-1), true);
        addControlListener('btn-right', () => playerMove(1), true);
        addControlListener('btn-down', () => playerDrop(), true);
        addControlListener('btn-rotate', () => playerRotate(1), false);
        addControlListener('btn-drop', () => playerHardDrop(), false);

        // 初期描画
        draw();
        drawNextPiece();

    </script>
</body>
</html>
