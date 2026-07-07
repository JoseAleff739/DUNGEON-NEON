<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="icon" href="https://public-frontend-cos.metadl.com/mgx/img/favicon_atoms.ico" type="image/x-icon">
    <title>Dungeon Neon Quest</title>
    <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet">
    <style>
        /* ============================================
           DUNGEON NEON QUEST - STYLES
           ============================================ */

        /* --- RESET & BASE --- */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            cursor: crosshair;
            font-family: 'Press Start 2P', monospace;
            overflow: hidden;
            width: 100vw;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: #0a0a1a;
        }

        button {
            cursor: pointer;
        }

        /* --- ANIMATED GRADIENT BACKGROUND --- */
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(
                135deg,
                #0a0a1a 0%,
                #1a0a2e 25%,
                #0a1628 50%,
                #1a0a2e 75%,
                #0a0a1a 100%
            );
            background-size: 400% 400%;
            animation: gradientShift 15s ease infinite;
            z-index: -2;
        }

        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            25% { background-position: 100% 0%; }
            50% { background-position: 100% 100%; }
            75% { background-position: 0% 100%; }
            100% { background-position: 0% 50%; }
        }

        /* --- FLOATING PARTICLES --- */
        .particles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            pointer-events: none;
            overflow: hidden;
        }

        .particle {
            position: absolute;
            border-radius: 50%;
            animation: floatParticle linear infinite;
            opacity: 0;
        }

        @keyframes floatParticle {
            0% {
                transform: translateY(100vh) scale(0);
                opacity: 0;
            }
            10% {
                opacity: 0.8;
            }
            90% {
                opacity: 0.3;
            }
            100% {
                transform: translateY(-10vh) scale(1);
                opacity: 0;
            }
        }

        /* --- GAME WRAPPER --- */
        .game-wrapper {
            position: relative;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 12px;
            padding: 20px;
            z-index: 1;
        }

        /* --- GAME TITLE --- */
        .game-title {
            font-family: 'Press Start 2P', monospace;
            font-size: 28px;
            color: #00d4ff;
            text-shadow:
                0 0 10px #00d4ff,
                0 0 20px #00d4ff,
                0 0 40px #00d4ff,
                0 0 80px #0066ff;
            animation: titleGlow 2s ease-in-out infinite alternate;
            letter-spacing: 4px;
            text-align: center;
            user-select: none;
        }

        .title-icon {
            display: inline-block;
            animation: iconBounce 1.5s ease-in-out infinite;
            font-size: 24px;
        }

        .title-icon:last-child {
            animation-delay: 0.5s;
        }

        @keyframes titleGlow {
            0% {
                text-shadow:
                    0 0 10px #00d4ff,
                    0 0 20px #00d4ff,
                    0 0 40px #00d4ff;
                filter: brightness(1);
            }
            100% {
                text-shadow:
                    0 0 15px #00d4ff,
                    0 0 30px #00d4ff,
                    0 0 60px #00d4ff,
                    0 0 100px #8b5cf6;
                filter: brightness(1.2);
            }
        }

        @keyframes iconBounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-6px); }
        }

        /* --- HUD (Heads-Up Display) --- */
        .hud {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: 800px;
            padding: 10px 20px;
            background: rgba(10, 10, 30, 0.7);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(139, 92, 246, 0.3);
            border-radius: 12px;
            box-shadow:
                0 0 15px rgba(139, 92, 246, 0.2),
                inset 0 0 15px rgba(0, 0, 0, 0.3);
        }

        .hud-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 4px;
        }

        .hud-label {
            font-size: 8px;
            color: #8b5cf6;
            letter-spacing: 2px;
            text-shadow: 0 0 8px rgba(139, 92, 246, 0.5);
        }

        .hud-value {
            font-size: 16px;
            color: #00d4ff;
            text-shadow:
                0 0 8px #00d4ff,
                0 0 16px rgba(0, 212, 255, 0.5);
        }

        .hearts {
            display: flex;
            gap: 4px;
        }

        .heart {
            font-size: 18px;
            animation: heartPulse 1s ease-in-out infinite;
            transition: all 0.3s ease;
        }

        .heart:nth-child(2) { animation-delay: 0.2s; }
        .heart:nth-child(3) { animation-delay: 0.4s; }

        .heart.lost {
            filter: grayscale(1) brightness(0.3);
            animation: none;
            transform: scale(0.8);
        }

        @keyframes heartPulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.15); }
        }

        /* --- CANVAS CONTAINER --- */
        .canvas-container {
            position: relative;
            border-radius: 12px;
            overflow: hidden;
            border: 2px solid transparent;
            background-image: linear-gradient(#0a0a1a, #0a0a1a),
                              linear-gradient(135deg, #8b5cf6, #00d4ff, #00ff88, #8b5cf6);
            background-origin: border-box;
            background-clip: padding-box, border-box;
            box-shadow:
                0 0 20px rgba(139, 92, 246, 0.4),
                0 0 40px rgba(0, 212, 255, 0.2),
                inset 0 0 20px rgba(0, 0, 0, 0.5);
            animation: containerPulse 3s ease-in-out infinite;
            transition: transform 0.05s ease;
        }

        @keyframes containerPulse {
            0%, 100% {
                box-shadow:
                    0 0 20px rgba(139, 92, 246, 0.4),
                    0 0 40px rgba(0, 212, 255, 0.2),
                    inset 0 0 20px rgba(0, 0, 0, 0.5);
            }
            50% {
                box-shadow:
                    0 0 30px rgba(139, 92, 246, 0.6),
                    0 0 60px rgba(0, 212, 255, 0.3),
                    inset 0 0 30px rgba(0, 0, 0, 0.3);
            }
        }

        #gameCanvas {
            display: block;
            background: #0d0d1a;
        }

        /* Screen shake */
        .canvas-container.shake {
            animation: screenShake 0.4s ease-out;
        }

        @keyframes screenShake {
            0% { transform: translate(0, 0); }
            10% { transform: translate(-8px, -5px); }
            20% { transform: translate(8px, 5px); }
            30% { transform: translate(-6px, 3px); }
            40% { transform: translate(6px, -3px); }
            50% { transform: translate(-4px, 2px); }
            60% { transform: translate(4px, -2px); }
            70% { transform: translate(-2px, 1px); }
            80% { transform: translate(2px, -1px); }
            90% { transform: translate(-1px, 0px); }
            100% { transform: translate(0, 0); }
        }

        /* --- OVERLAY SCREENS --- */
        .overlay {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 10;
            animation: fadeIn 0.5s ease;
        }

        .overlay.hidden {
            display: none;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .overlay-content {
            text-align: center;
            padding: 40px 50px;
            border-radius: 20px;
            position: relative;
            z-index: 2;
        }

        /* --- MENU SCREEN --- */
        .menu-content {
            background: rgba(10, 10, 30, 0.95);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(0, 212, 255, 0.4);
            box-shadow:
                0 0 40px rgba(0, 212, 255, 0.3),
                0 0 80px rgba(139, 92, 246, 0.2),
                inset 0 0 40px rgba(0, 0, 0, 0.5);
        }

        .menu-title {
            font-family: 'Press Start 2P', monospace;
            font-size: 24px;
            color: #00d4ff;
            text-shadow:
                0 0 10px #00d4ff,
                0 0 30px #00d4ff,
                0 0 60px #8b5cf6;
            margin-bottom: 16px;
            animation: titleGlow 2s ease-in-out infinite alternate;
        }

        .menu-subtitle {
            font-size: 10px;
            color: #a78bfa;
            margin-bottom: 24px;
            line-height: 1.8;
        }

        .menu-instructions {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 30px;
        }

        .instruction-item {
            font-size: 9px;
            color: #c4b5fd;
            padding: 8px 16px;
            background: rgba(139, 92, 246, 0.1);
            border: 1px solid rgba(139, 92, 246, 0.2);
            border-radius: 8px;
            transition: all 0.3s ease;
        }

        .instruction-item:hover {
            background: rgba(139, 92, 246, 0.2);
            border-color: rgba(139, 92, 246, 0.5);
            transform: translateX(5px);
        }

        /* --- BUTTONS --- */
        .btn {
            font-family: 'Press Start 2P', monospace;
            font-size: 12px;
            padding: 14px 32px;
            border: none;
            border-radius: 10px;
            color: #fff;
            position: relative;
            overflow: hidden;
            transition: all 0.3s ease;
            letter-spacing: 1px;
        }

        .btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            transition: left 0.5s ease;
        }

        .btn:hover::before {
            left: 100%;
        }

        .btn:hover {
            transform: scale(1.08);
            box-shadow:
                0 0 20px rgba(0, 212, 255, 0.5),
                0 0 40px rgba(0, 212, 255, 0.3);
        }

        .btn:active {
            transform: scale(0.95);
        }

        .btn-start {
            background: linear-gradient(135deg, #00d4ff, #8b5cf6);
            background-size: 200% 200%;
            animation: btnGradient 3s ease infinite;
            box-shadow: 0 0 15px rgba(0, 212, 255, 0.4);
        }

        .btn-restart {
            background: linear-gradient(135deg, #ff2244, #ff6644);
            background-size: 200% 200%;
            animation: btnGradient 3s ease infinite;
            box-shadow: 0 0 15px rgba(255, 34, 68, 0.4);
        }

        .btn-victory {
            background: linear-gradient(135deg, #ffd700, #ff8c00);
            background-size: 200% 200%;
            animation: btnGradient 3s ease infinite;
            box-shadow: 0 0 15px rgba(255, 215, 0, 0.4);
            color: #1a0a2e;
        }

        @keyframes btnGradient {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        /* --- GAME OVER SCREEN --- */
        .gameover-content {
            background: rgba(30, 0, 0, 0.95);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255, 34, 68, 0.5);
            box-shadow:
                0 0 40px rgba(255, 34, 68, 0.4),
                0 0 80px rgba(255, 0, 0, 0.2),
                inset 0 0 40px rgba(0, 0, 0, 0.5);
        }

        .gameover-title {
            font-family: 'Press Start 2P', monospace;
            font-size: 28px;
            color: #ff2244;
            text-shadow:
                0 0 10px #ff2244,
                0 0 30px #ff2244,
                0 0 60px #ff0000;
            animation: gameoverFlash 0.8s ease-in-out infinite;
            margin-bottom: 20px;
        }

        @keyframes gameoverFlash {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }

        .gameover-score {
            font-size: 12px;
            color: #ff6666;
            margin-bottom: 24px;
            text-shadow: 0 0 8px rgba(255, 102, 102, 0.5);
        }

        /* --- VICTORY SCREEN --- */
        .victory-content {
            background: rgba(30, 20, 0, 0.95);
            backdrop-filter: blur(20px);
            border: 2px solid rgba(255, 215, 0, 0.5);
            box-shadow:
                0 0 40px rgba(255, 215, 0, 0.4),
                0 0 80px rgba(255, 140, 0, 0.2),
                inset 0 0 40px rgba(0, 0, 0, 0.5);
            overflow: hidden;
        }

        .victory-title {
            font-family: 'Press Start 2P', monospace;
            font-size: 28px;
            color: #ffd700;
            text-shadow:
                0 0 10px #ffd700,
                0 0 30px #ffd700,
                0 0 60px #ff8c00;
            animation: victoryShine 2s ease-in-out infinite;
            margin-bottom: 20px;
        }

        @keyframes victoryShine {
            0%, 100% {
                text-shadow:
                    0 0 10px #ffd700,
                    0 0 30px #ffd700;
                filter: brightness(1);
            }
            50% {
                text-shadow:
                    0 0 20px #ffd700,
                    0 0 50px #ffd700,
                    0 0 80px #ff8c00;
                filter: brightness(1.3);
            }
        }

        .victory-score {
            font-size: 14px;
            color: #ffd700;
            margin-bottom: 12px;
            text-shadow: 0 0 8px rgba(255, 215, 0, 0.5);
        }

        .victory-msg {
            font-size: 10px;
            color: #ffcc66;
            margin-bottom: 24px;
        }

        /* --- CONFETTI --- */
        .confetti-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            overflow: hidden;
            z-index: -1;
        }

        .confetti-piece {
            position: absolute;
            top: -20px;
            width: 10px;
            height: 10px;
            animation: confettiFall linear infinite;
        }

        @keyframes confettiFall {
            0% {
                transform: translateY(-20px) rotate(0deg);
                opacity: 1;
            }
            100% {
                transform: translateY(400px) rotate(720deg);
                opacity: 0;
            }
        }

        /* --- CLICK EFFECTS --- */
        #clickEffects {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 100;
        }

        .click-ring {
            position: absolute;
            width: 30px;
            height: 30px;
            border: 2px solid #00d4ff;
            border-radius: 50%;
            animation: clickRipple 0.6s ease-out forwards;
            pointer-events: none;
        }

        @keyframes clickRipple {
            0% {
                transform: translate(-50%, -50%) scale(0);
                opacity: 1;
            }
            100% {
                transform: translate(-50%, -50%) scale(3);
                opacity: 0;
            }
        }

        .overlay-title {
            font-family: 'Press Start 2P', monospace;
        }

        /* --- RESPONSIVE --- */
        @media (max-width: 860px) {
            .game-wrapper {
                transform: scale(0.7);
                transform-origin: center center;
            }
        }

        @media (max-width: 600px) {
            .game-wrapper {
                transform: scale(0.5);
                transform-origin: center center;
            }
        }
    </style>
</head>
<body>
    <!-- Floating Particles Background -->
    <div class="particles" id="particles"></div>

    <!-- Game Container -->
    <div class="game-wrapper" id="gameWrapper">
        <!-- Title -->
        <h1 class="game-title" id="gameTitle">
            <span class="title-icon">⚔️</span>
            DUNGEON NEON
            <span class="title-icon">🛡️</span>
        </h1>

        <!-- HUD -->
        <div class="hud" id="hud">
            <div class="hud-item">
                <span class="hud-label">SCORE</span>
                <span class="hud-value" id="scoreDisplay">0</span>
            </div>
            <div class="hud-item">
                <span class="hud-label">LIVES</span>
                <div class="hearts" id="heartsDisplay">
                    <span class="heart active">❤️</span>
                    <span class="heart active">❤️</span>
                    <span class="heart active">❤️</span>
                </div>
            </div>
            <div class="hud-item">
                <span class="hud-label">COINS</span>
                <span class="hud-value" id="coinsDisplay">0/5</span>
            </div>
        </div>

        <!-- Canvas Container -->
        <div class="canvas-container" id="canvasContainer">
            <canvas id="gameCanvas" width="800" height="600"></canvas>
        </div>

        <!-- Menu Screen -->
        <div class="overlay" id="menuScreen">
            <div class="overlay-content menu-content">
                <h2 class="overlay-title menu-title">⚔️ DUNGEON NEON ⚔️</h2>
                <p class="menu-subtitle">Colete todas as moedas e encontre a saída!</p>
                <div class="menu-instructions">
                    <div class="instruction-item">🎮 WASD ou Setas para mover</div>
                    <div class="instruction-item">🪙 Colete 5 moedas</div>
                    <div class="instruction-item">🚪 Encontre o portal</div>
                    <div class="instruction-item">👹 Evite os inimigos!</div>
                </div>
                <button class="btn btn-start" id="startBtn" onclick="startGame()">
                    ▶ INICIAR JOGO
                </button>
            </div>
        </div>

        <!-- Game Over Screen -->
        <div class="overlay hidden" id="gameOverScreen">
            <div class="overlay-content gameover-content">
                <h2 class="overlay-title gameover-title">💀 GAME OVER 💀</h2>
                <p class="gameover-score">Pontuação: <span id="finalScore">0</span></p>
                <button class="btn btn-restart" onclick="resetGame()">
                    🔄 JOGAR NOVAMENTE
                </button>
            </div>
        </div>

        <!-- Victory Screen -->
        <div class="overlay hidden" id="victoryScreen">
            <div class="overlay-content victory-content">
                <div class="confetti-container" id="confettiContainer"></div>
                <h2 class="overlay-title victory-title">🏆 VITÓRIA! 🏆</h2>
                <p class="victory-score">Pontuação Final: <span id="victoryScore">0</span></p>
                <p class="victory-msg">Você escapou da dungeon!</p>
                <button class="btn btn-victory" onclick="resetGame()">
                    🔄 JOGAR NOVAMENTE
                </button>
            </div>
        </div>
    </div>

    <!-- Click Effect Container -->
    <div id="clickEffects"></div>

    <script>
        /* ============================================
           DUNGEON NEON QUEST - GAME LOGIC
           ============================================ */

        // ============================================
        // SECTION: Canvas & Context Setup
        // ============================================
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const CANVAS_W = 800;
        const CANVAS_H = 600;

        // ============================================
        // SECTION: Game State Variables
        // ============================================
        let gameRunning = false;
        let score = 0;
        let lives = 3;
        let coinsCollected = 0;
        let portalActive = false;
        let keys = {};
        let animationFrameId = null;
        let damageFlashTimer = 0;
        let damageCooldown = 0;

        // ============================================
        // SECTION: Audio System (Web Audio API)
        // ============================================
        let audioCtx = null;

        function initAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
        }

        function playCoinSound() {
            if (!audioCtx) return;
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            osc.type = 'sine';
            osc.frequency.setValueAtTime(800, audioCtx.currentTime);
            osc.frequency.exponentialRampToValueAtTime(1600, audioCtx.currentTime + 0.1);
            gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.2);
            osc.start(audioCtx.currentTime);
            osc.stop(audioCtx.currentTime + 0.2);
        }

        function playDamageSound() {
            if (!audioCtx) return;
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(400, audioCtx.currentTime);
            osc.frequency.exponentialRampToValueAtTime(100, audioCtx.currentTime + 0.3);
            gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.3);
            osc.start(audioCtx.currentTime);
            osc.stop(audioCtx.currentTime + 0.3);
        }

        function playVictorySound() {
            if (!audioCtx) return;
            const notes = [523, 659, 784, 1047];
            notes.forEach((freq, i) => {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.type = 'sine';
                osc.frequency.setValueAtTime(freq, audioCtx.currentTime + i * 0.15);
                gain.gain.setValueAtTime(0, audioCtx.currentTime + i * 0.15);
                gain.gain.linearRampToValueAtTime(0.2, audioCtx.currentTime + i * 0.15 + 0.05);
                gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + i * 0.15 + 0.5);
                osc.start(audioCtx.currentTime + i * 0.15);
                osc.stop(audioCtx.currentTime + i * 0.15 + 0.5);
            });
        }

        function playGameOverSound() {
            if (!audioCtx) return;
            const notes = [300, 250, 200, 150];
            notes.forEach((freq, i) => {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.type = 'square';
                osc.frequency.setValueAtTime(freq, audioCtx.currentTime + i * 0.2);
                gain.gain.setValueAtTime(0, audioCtx.currentTime + i * 0.2);
                gain.gain.linearRampToValueAtTime(0.2, audioCtx.currentTime + i * 0.2 + 0.05);
                gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + i * 0.2 + 0.4);
                osc.start(audioCtx.currentTime + i * 0.2);
                osc.stop(audioCtx.currentTime + i * 0.2 + 0.4);
            });
        }

        // ============================================
        // SECTION: Particle System
        // ============================================
        let particles = [];

        class Particle {
            constructor(x, y, color, speedX, speedY, size, life) {
                this.x = x;
                this.y = y;
                this.color = color;
                this.speedX = speedX;
                this.speedY = speedY;
                this.size = size;
                this.life = life;
                this.maxLife = life;
            }

            update() {
                this.x += this.speedX;
                this.y += this.speedY;
                this.life--;
                this.speedY += 0.05;
            }

            draw() {
                const alpha = this.life / this.maxLife;
                ctx.save();
                ctx.globalAlpha = alpha;
                ctx.fillStyle = this.color;
                ctx.shadowColor = this.color;
                ctx.shadowBlur = 8;
                ctx.fillRect(this.x - this.size / 2, this.y - this.size / 2, this.size, this.size);
                ctx.restore();
            }
        }

        function spawnParticles(x, y, color, count) {
            for (let i = 0; i < count; i++) {
                const angle = (Math.PI * 2 / count) * i + Math.random() * 0.5;
                const speed = 1 + Math.random() * 3;
                particles.push(new Particle(
                    x, y, color,
                    Math.cos(angle) * speed,
                    Math.sin(angle) * speed,
                    2 + Math.random() * 4,
                    30 + Math.random() * 20
                ));
            }
        }

        // ============================================
        // SECTION: Game Objects
        // ============================================
        const player = {
            x: 50, y: 50,
            width: 28, height: 28,
            speed: 3.5,
            color: '#00d4ff'
        };

        let enemies = [];

        function createEnemies() {
            enemies = [];
            const enemyData = [
                { x: 400, y: 200, dx: 2, dy: 1.5 },
                { x: 600, y: 400, dx: -1.5, dy: 2 },
                { x: 200, y: 350, dx: 1.8, dy: -1.8 }
            ];
            enemyData.forEach(e => {
                enemies.push({
                    x: e.x, y: e.y,
                    width: 26, height: 26,
                    dx: e.dx, dy: e.dy,
                    color: '#ff2244',
                    glowTimer: 0
                });
            });
        }

        let coins = [];

        function createCoins() {
            coins = [];
            const positions = [
                { x: 150, y: 100 },
                { x: 650, y: 80 },
                { x: 400, y: 300 },
                { x: 100, y: 480 },
                { x: 700, y: 500 }
            ];
            positions.forEach(p => {
                coins.push({
                    x: p.x, y: p.y,
                    radius: 12,
                    color: '#ffd700',
                    collected: false,
                    rotationAngle: Math.random() * Math.PI * 2
                });
            });
        }

        const portal = {
            x: 740, y: 530,
            width: 40, height: 50,
            color: '#00ff88',
            pulseTimer: 0
        };

        // ============================================
        // SECTION: Dungeon Background Tiles
        // ============================================
        const TILE_SIZE = 40;
        let dungeonTiles = [];

        function generateDungeonTiles() {
            dungeonTiles = [];
            for (let y = 0; y < CANVAS_H; y += TILE_SIZE) {
                for (let x = 0; x < CANVAS_W; x += TILE_SIZE) {
                    const shade = 12 + Math.random() * 8;
                    const isWall = (x === 0 || y === 0 || x >= CANVAS_W - TILE_SIZE || y >= CANVAS_H - TILE_SIZE);
                    dungeonTiles.push({
                        x, y, shade, isWall,
                        crack: Math.random() > 0.85
                    });
                }
            }
        }

        // ============================================
        // SECTION: Input Handling
        // ============================================
        document.addEventListener('keydown', (e) => {
            keys[e.key] = true;
            if (['ArrowUp', 'ArrowDown', 'ArrowLeft', 'ArrowRight', ' '].includes(e.key)) {
                e.preventDefault();
            }
        });

        document.addEventListener('keyup', (e) => {
            keys[e.key] = false;
        });

        document.addEventListener('click', (e) => {
            const ring = document.createElement('div');
            ring.className = 'click-ring';
            ring.style.left = e.clientX + 'px';
            ring.style.top = e.clientY + 'px';
            document.getElementById('clickEffects').appendChild(ring);
            setTimeout(() => ring.remove(), 600);
        });

        // ============================================
        // SECTION: Collision Detection
        // ============================================
        function rectCollision(a, b) {
            return (
                a.x < b.x + b.width &&
                a.x + a.width > b.x &&
                a.y < b.y + b.height &&
                a.y + a.height > b.y
            );
        }

        function rectCircleCollision(rect, circle) {
            const closestX = Math.max(rect.x, Math.min(circle.x, rect.x + rect.width));
            const closestY = Math.max(rect.y, Math.min(circle.y, rect.y + rect.height));
            const dx = circle.x - closestX;
            const dy = circle.y - closestY;
            return (dx * dx + dy * dy) < (circle.radius * circle.radius);
        }

        function detectCollisions() {
            // Player vs Enemies
            if (damageCooldown <= 0) {
                enemies.forEach(enemy => {
                    if (rectCollision(player, enemy)) {
                        lives--;
                        damageCooldown = 60;
                        damageFlashTimer = 20;
                        playDamageSound();
                        triggerScreenShake();
                        spawnParticles(
                            player.x + player.width / 2,
                            player.y + player.height / 2,
                            '#ff2244', 15
                        );
                        updateHUD();
                        if (lives <= 0) {
                            gameOver();
                        }
                    }
                });
            }

            // Player vs Coins
            coins.forEach(coin => {
                if (!coin.collected && rectCircleCollision(player, coin)) {
                    coin.collected = true;
                    coinsCollected++;
                    score += 10;
                    playCoinSound();
                    spawnParticles(coin.x, coin.y, '#ffd700', 12);
                    updateHUD();
                    if (coinsCollected >= 5) {
                        portalActive = true;
                    }
                }
            });

            // Player vs Portal
            if (portalActive && rectCollision(player, portal)) {
                victory();
            }
        }

        // ============================================
        // SECTION: Screen Shake Effect
        // ============================================
        function triggerScreenShake() {
            const container = document.getElementById('canvasContainer');
            container.classList.remove('shake');
            void container.offsetWidth;
            container.classList.add('shake');
            setTimeout(() => container.classList.remove('shake'), 400);
        }

        // ============================================
        // SECTION: Update Functions
        // ============================================
        function updatePlayer() {
            let dx = 0;
            let dy = 0;
            if (keys['ArrowUp'] || keys['w'] || keys['W']) dy = -player.speed;
            if (keys['ArrowDown'] || keys['s'] || keys['S']) dy = player.speed;
            if (keys['ArrowLeft'] || keys['a'] || keys['A']) dx = -player.speed;
            if (keys['ArrowRight'] || keys['d'] || keys['D']) dx = player.speed;

            if (dx !== 0 && dy !== 0) {
                dx *= 0.707;
                dy *= 0.707;
            }

            player.x += dx;
            player.y += dy;

            const wallThickness = TILE_SIZE;
            player.x = Math.max(wallThickness, Math.min(CANVAS_W - wallThickness - player.width, player.x));
            player.y = Math.max(wallThickness, Math.min(CANVAS_H - wallThickness - player.height, player.y));

            if (damageCooldown > 0) damageCooldown--;
            if (damageFlashTimer > 0) damageFlashTimer--;
        }

        function updateEnemies() {
            const wallThickness = TILE_SIZE;
            enemies.forEach(enemy => {
                enemy.x += enemy.dx;
                enemy.y += enemy.dy;
                enemy.glowTimer += 0.05;

                if (enemy.x <= wallThickness || enemy.x + enemy.width >= CANVAS_W - wallThickness) {
                    enemy.dx *= -1;
                    enemy.x = Math.max(wallThickness, Math.min(CANVAS_W - wallThickness - enemy.width, enemy.x));
                }
                if (enemy.y <= wallThickness || enemy.y + enemy.height >= CANVAS_H - wallThickness) {
                    enemy.dy *= -1;
                    enemy.y = Math.max(wallThickness, Math.min(CANVAS_H - wallThickness - enemy.height, enemy.y));
                }
            });
        }

        function updateCoins() {
            coins.forEach(coin => {
                if (!coin.collected) {
                    coin.rotationAngle += 0.03;
                }
            });
        }

        function updatePortal() {
            if (portalActive) {
                portal.pulseTimer += 0.05;
            }
        }

        function updateParticles() {
            particles = particles.filter(p => p.life > 0);
            particles.forEach(p => p.update());
        }

        function update() {
            updatePlayer();
            updateEnemies();
            updateCoins();
            updatePortal();
            updateParticles();
            detectCollisions();
        }

        // ============================================
        // SECTION: Draw Functions
        // ============================================
        function drawDungeon() {
            dungeonTiles.forEach(tile => {
                if (tile.isWall) {
                    ctx.fillStyle = `rgb(${25 + tile.shade}, ${20 + tile.shade}, ${30 + tile.shade})`;
                    ctx.fillRect(tile.x, tile.y, TILE_SIZE, TILE_SIZE);
                    ctx.strokeStyle = 'rgba(60, 50, 70, 0.5)';
                    ctx.lineWidth = 1;
                    ctx.strokeRect(tile.x, tile.y, TILE_SIZE, TILE_SIZE);
                    ctx.fillStyle = 'rgba(80, 60, 100, 0.15)';
                    ctx.fillRect(tile.x, tile.y, TILE_SIZE, 2);
                } else {
                    ctx.fillStyle = `rgb(${15 + tile.shade}, ${13 + tile.shade}, ${22 + tile.shade})`;
                    ctx.fillRect(tile.x, tile.y, TILE_SIZE, TILE_SIZE);
                    ctx.strokeStyle = 'rgba(40, 35, 55, 0.3)';
                    ctx.lineWidth = 0.5;
                    ctx.strokeRect(tile.x, tile.y, TILE_SIZE, TILE_SIZE);
                    if (tile.crack) {
                        ctx.strokeStyle = 'rgba(50, 40, 60, 0.4)';
                        ctx.lineWidth = 1;
                        ctx.beginPath();
                        ctx.moveTo(tile.x + 10, tile.y + 5);
                        ctx.lineTo(tile.x + 20, tile.y + 20);
                        ctx.lineTo(tile.x + 30, tile.y + 35);
                        ctx.stroke();
                    }
                }
            });
        }

        function drawPlayer() {
            ctx.save();
            if (damageFlashTimer > 0 && Math.floor(damageFlashTimer / 3) % 2 === 0) {
                ctx.globalAlpha = 0.4;
            }
            if (damageCooldown > 0 && Math.floor(damageCooldown / 4) % 2 === 0) {
                ctx.globalAlpha = 0.5;
            }
            ctx.shadowColor = player.color;
            ctx.shadowBlur = 15;
            ctx.fillStyle = player.color;
            ctx.fillRect(player.x, player.y, player.width, player.height);
            ctx.shadowBlur = 0;
            ctx.fillStyle = 'rgba(255, 255, 255, 0.3)';
            ctx.fillRect(player.x + 3, player.y + 3, player.width - 12, player.height - 12);
            ctx.fillStyle = '#0a0a1a';
            ctx.fillRect(player.x + 6, player.y + 8, 4, 5);
            ctx.fillRect(player.x + 18, player.y + 8, 4, 5);
            ctx.restore();
        }

        function drawEnemies() {
            enemies.forEach(enemy => {
                ctx.save();
                const glowIntensity = 12 + Math.sin(enemy.glowTimer) * 6;
                ctx.shadowColor = enemy.color;
                ctx.shadowBlur = glowIntensity;
                ctx.fillStyle = enemy.color;
                ctx.fillRect(enemy.x, enemy.y, enemy.width, enemy.height);
                ctx.shadowBlur = 0;
                ctx.fillStyle = '#0a0a1a';
                ctx.fillRect(enemy.x + 5, enemy.y + 7, 5, 4);
                ctx.fillRect(enemy.x + 16, enemy.y + 7, 5, 4);
                ctx.fillRect(enemy.x + 7, enemy.y + 17, 12, 3);
                ctx.restore();
            });
        }

        function drawCoins() {
            coins.forEach(coin => {
                if (coin.collected) return;
                ctx.save();
                ctx.translate(coin.x, coin.y);
                const scaleX = Math.cos(coin.rotationAngle);
                ctx.scale(scaleX, 1);
                ctx.shadowColor = coin.color;
                ctx.shadowBlur = 12;
                ctx.beginPath();
                ctx.arc(0, 0, coin.radius, 0, Math.PI * 2);
                ctx.fillStyle = coin.color;
                ctx.fill();
                ctx.shadowBlur = 0;
                ctx.beginPath();
                ctx.arc(0, 0, coin.radius - 3, 0, Math.PI * 2);
                ctx.strokeStyle = '#ffaa00';
                ctx.lineWidth = 1.5;
                ctx.stroke();
                ctx.fillStyle = '#cc8800';
                ctx.font = 'bold 12px Arial';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText('$', 0, 1);
                ctx.restore();
            });
        }

        function drawPortal() {
            if (!portalActive) return;
            ctx.save();
            const pulse = Math.sin(portal.pulseTimer) * 0.3 + 1;
            ctx.shadowColor = portal.color;
            ctx.shadowBlur = 20 * pulse;
            ctx.fillStyle = portal.color;
            ctx.fillRect(portal.x, portal.y, portal.width, portal.height);
            ctx.shadowBlur = 0;
            ctx.fillStyle = 'rgba(0, 0, 0, 0.3)';
            ctx.fillRect(portal.x + 5, portal.y + 5, portal.width - 10, portal.height - 10);
            ctx.fillStyle = '#aaffcc';
            ctx.font = '20px Arial';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.fillText('⬆', portal.x + portal.width / 2, portal.y + portal.height / 2);
            ctx.fillStyle = portal.color;
            ctx.shadowColor = portal.color;
            ctx.shadowBlur = 10;
            ctx.font = "bold 10px 'Press Start 2P', monospace";
            ctx.fillText('EXIT', portal.x + portal.width / 2, portal.y - 10);
            ctx.restore();
        }

        function drawParticles() {
            particles.forEach(p => p.draw());
        }

        function drawDamageOverlay() {
            if (damageFlashTimer > 0) {
                ctx.save();
                ctx.fillStyle = `rgba(255, 0, 0, ${damageFlashTimer / 40})`;
                ctx.fillRect(0, 0, CANVAS_W, CANVAS_H);
                ctx.restore();
            }
        }

        function draw() {
            ctx.clearRect(0, 0, CANVAS_W, CANVAS_H);
            drawDungeon();
            drawPortal();
            drawCoins();
            drawEnemies();
            drawPlayer();
            drawParticles();
            drawDamageOverlay();
        }

        // ============================================
        // SECTION: Game Loop
        // ============================================
        function gameLoop() {
            if (!gameRunning) return;
            update();
            draw();
            animationFrameId = requestAnimationFrame(gameLoop);
        }

        // ============================================
        // SECTION: HUD Update
        // ============================================
        function updateHUD() {
            document.getElementById('scoreDisplay').textContent = score;
            document.getElementById('coinsDisplay').textContent = `${coinsCollected}/5`;
            const hearts = document.querySelectorAll('.heart');
            hearts.forEach((heart, i) => {
                if (i < lives) {
                    heart.classList.remove('lost');
                    heart.classList.add('active');
                } else {
                    heart.classList.add('lost');
                    heart.classList.remove('active');
                }
            });
        }

        // ============================================
        // SECTION: Game State Management
        // ============================================
        function startGame() {
            initAudio();
            document.getElementById('menuScreen').classList.add('hidden');
            document.getElementById('hud').style.display = 'flex';

            score = 0;
            lives = 3;
            coinsCollected = 0;
            portalActive = false;
            damageFlashTimer = 0;
            damageCooldown = 0;
            particles = [];
            keys = {};

            player.x = 60;
            player.y = 60;

            createEnemies();
            createCoins();
            generateDungeonTiles();
            updateHUD();

            gameRunning = true;
            gameLoop();
        }

        function gameOver() {
            gameRunning = false;
            if (animationFrameId) cancelAnimationFrame(animationFrameId);
            playGameOverSound();
            document.getElementById('finalScore').textContent = score;
            document.getElementById('gameOverScreen').classList.remove('hidden');
        }

        function victory() {
            gameRunning = false;
            if (animationFrameId) cancelAnimationFrame(animationFrameId);
            playVictorySound();
            document.getElementById('victoryScore').textContent = score;
            document.getElementById('victoryScreen').classList.remove('hidden');
            createConfetti();
        }

        function resetGame() {
            document.getElementById('gameOverScreen').classList.add('hidden');
            document.getElementById('victoryScreen').classList.add('hidden');
            document.getElementById('confettiContainer').innerHTML = '';

            score = 0;
            lives = 3;
            coinsCollected = 0;
            portalActive = false;
            damageFlashTimer = 0;
            damageCooldown = 0;
            particles = [];
            keys = {};

            player.x = 60;
            player.y = 60;

            createEnemies();
            createCoins();
            generateDungeonTiles();
            updateHUD();

            gameRunning = true;
            gameLoop();
        }

        // ============================================
        // SECTION: Confetti Effect
        // ============================================
        function createConfetti() {
            const container = document.getElementById('confettiContainer');
            const colors = ['#ffd700', '#ff6b6b', '#00d4ff', '#00ff88', '#ff44aa', '#8b5cf6', '#ff8c00'];
            for (let i = 0; i < 50; i++) {
                const piece = document.createElement('div');
                piece.className = 'confetti-piece';
                piece.style.left = Math.random() * 100 + '%';
                piece.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                piece.style.animationDuration = (2 + Math.random() * 3) + 's';
                piece.style.animationDelay = Math.random() * 2 + 's';
                piece.style.width = (5 + Math.random() * 10) + 'px';
                piece.style.height = (5 + Math.random() * 10) + 'px';
                piece.style.borderRadius = Math.random() > 0.5 ? '50%' : '0';
                container.appendChild(piece);
            }
        }

        // ============================================
        // SECTION: Background Particles (CSS)
        // ============================================
        function createBackgroundParticles() {
            const container = document.getElementById('particles');
            const colors = ['#8b5cf6', '#00d4ff', '#00ff88', '#ffd700', '#ff44aa'];
            for (let i = 0; i < 30; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                particle.style.left = Math.random() * 100 + '%';
                particle.style.width = (2 + Math.random() * 4) + 'px';
                particle.style.height = particle.style.width;
                particle.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                particle.style.animationDuration = (8 + Math.random() * 15) + 's';
                particle.style.animationDelay = Math.random() * 10 + 's';
                container.appendChild(particle);
            }
        }

        // ============================================
        // SECTION: Initialization
        // ============================================
        document.addEventListener('DOMContentLoaded', function () {
            createBackgroundParticles();
            document.getElementById('hud').style.display = 'none';
            document.getElementById('menuScreen').classList.remove('hidden');
            generateDungeonTiles();
            draw();
        });
    </script>
</body>
</html>
