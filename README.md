# Neon-dash
first game
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Neon Dash: Cyber Jumper</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@500;700&display=swap');
        
        * {
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }

        body {
            font-family: 'Rajdhani', sans-serif;
            background-color: #05050d;
            color: #ffffff;
            overflow: hidden;
        }

        .font-orbitron {
            font-family: 'Orbitron', sans-serif;
        }

        .neon-glow-cyan {
            text-shadow: 0 0 10px rgba(0, 243, 255, 0.8), 0 0 20px rgba(0, 243, 255, 0.4);
        }

        .neon-glow-magenta {
            text-shadow: 0 0 10px rgba(255, 0, 85, 0.8), 0 0 20px rgba(255, 0, 85, 0.4);
        }

        .neon-box-cyan {
            box-shadow: 0 0 15px rgba(0, 243, 255, 0.5), inset 0 0 15px rgba(0, 243, 255, 0.3);
            border: 1px solid rgba(0, 243, 255, 0.8);
        }

        .neon-box-magenta {
            box-shadow: 0 0 15px rgba(255, 0, 85, 0.5), inset 0 0 15px rgba(255, 0, 85, 0.3);
            border: 1px solid rgba(255, 0, 85, 0.8);
        }

        .btn-action {
            background: linear-gradient(135deg, rgba(0,243,255,0.2) 0%, rgba(255,0,85,0.2) 100%);
            transition: all 0.2s ease;
        }

        .btn-action:active {
            transform: scale(0.95);
        }

        canvas {
            display: block;
            touch-action: none;
        }
    </style>
</head>
<body class="h-screen w-screen flex flex-col justify-center items-center relative overflow-hidden select-none">

    <!-- Canvas Overlay for UI -->
    <div id="ui-container" class="absolute inset-0 z-10 pointer-events-none flex flex-col justify-between p-4 max-w-md mx-auto">
        <!-- HUD Header -->
        <div class="flex justify-between items-start pt-2 px-2">
            <div>
                <p class="text-xs uppercase tracking-widest text-cyan-400 font-bold">Score</p>
                <h1 id="score-display" class="text-4xl font-black font-orbitron text-cyan-300 neon-glow-cyan">0</h1>
            </div>
            <div class="text-center">
                <p class="text-xs uppercase tracking-widest text-pink-400 font-bold">Multiplier</p>
                <h2 id="multiplier-display" class="text-2xl font-black font-orbitron text-pink-400 neon-glow-magenta">x1</h2>
            </div>
            <div class="text-right">
                <p class="text-xs uppercase tracking-widest text-yellow-400 font-bold">Best</p>
                <h2 id="high-score-display" class="text-2xl font-black font-orbitron text-yellow-300">0</h2>
            </div>
        </div>

        <!-- Combo Bar / Power Indicator -->
        <div class="w-full bg-gray-900/80 rounded-full h-2 mb-auto mt-2 overflow-hidden border border-cyan-500/30">
            <div id="combo-bar" class="bg-gradient-to-r from-cyan-400 to-pink-500 h-full w-0 transition-all duration-100"></div>
        </div>

        <!-- Touch Controls -->
        <div id="touch-controls" class="pointer-events-auto flex justify-between items-center pb-6 px-4 gap-4 opacity-80">
            <button id="left-touch-btn" class="flex-1 py-6 bg-cyan-950/40 border border-cyan-500/40 rounded-2xl flex flex-col items-center justify-center active:bg-cyan-500/30 active:scale-95 transition-all">
                <i class="fa-solid fa-rotate-left text-2xl text-cyan-400 mb-1"></i>
                <span class="text-xs font-bold text-cyan-300 uppercase tracking-wider">SWAP LEFT</span>
            </button>
            <button id="jump-touch-btn" class="flex-1 py-8 bg-pink-950/40 border border-pink-500/40 rounded-2xl flex flex-col items-center justify-center active:bg-pink-500/30 active:scale-95 transition-all">
                <i class="fa-solid fa-angles-up text-3xl text-pink-400 mb-1"></i>
                <span class="text-xs font-bold text-pink-300 uppercase tracking-wider">JUMP / DASH</span>
            </button>
            <button id="right-touch-btn" class="flex-1 py-6 bg-cyan-950/40 border border-cyan-500/40 rounded-2xl flex flex-col items-center justify-center active:bg-cyan-500/30 active:scale-95 transition-all">
                <i class="fa-solid fa-rotate-right text-2xl text-cyan-400 mb-1"></i>
                <span class="text-xs font-bold text-cyan-300 uppercase tracking-wider">SWAP RIGHT</span>
            </button>
        </div>
    </div>

    <!-- Start / Main Menu Screen -->
    <div id="start-menu" class="absolute inset-0 z-20 flex flex-col items-center justify-center bg-black/85 backdrop-blur-md p-6">
        <div class="text-center max-w-xs w-full">
            <div class="mb-2 inline-block px-3 py-1 rounded-full bg-cyan-500/10 border border-cyan-500/30 text-cyan-400 text-xs font-bold tracking-widest uppercase">
                Arcade Survival
            </div>
            <h1 class="text-5xl font-black font-orbitron text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 via-pink-500 to-yellow-400 neon-glow-cyan mb-2">
                NEON DASH
            </h1>
            <p class="text-gray-400 text-sm mb-8 font-medium">Switch orbits, dodge lasers, collect dynamic cyber-orbs!</p>

            <button id="start-btn" class="pointer-events-auto w-full py-4 rounded-xl font-orbitron font-bold text-lg text-white btn-action neon-box-cyan flex items-center justify-center gap-3 mb-4 cursor-pointer">
                <i class="fa-solid fa-play"></i> START RUN
            </button>

            <!-- Instructions -->
            <div class="bg-gray-900/60 rounded-xl p-4 border border-gray-800 text-left text-xs text-gray-300 space-y-2">
                <div class="flex items-center gap-2">
                    <span class="w-6 h-6 rounded bg-cyan-500/20 text-cyan-400 flex items-center justify-center font-bold">←</span>
                    <span>Tap Left/Right or buttons to switch track lanes.</span>
                </div>
                <div class="flex items-center gap-2">
                    <span class="w-6 h-6 rounded bg-pink-500/20 text-pink-400 flex items-center justify-center font-bold">↑</span>
                    <span>Tap Center or Swipe Up to Double Jump / Dash.</span>
                </div>
                <div class="flex items-center gap-2">
                    <span class="w-6 h-6 rounded bg-yellow-500/20 text-yellow-400 flex items-center justify-center font-bold">★</span>
                    <span>Match orb colors for extra combo points!</span>
                </div>
            </div>
        </div>
    </div>

    <!-- Game Over Screen -->
    <div id="game-over-menu" class="hidden absolute inset-0 z-20 flex flex-col items-center justify-center bg-black/90 backdrop-blur-md p-6">
        <div class="text-center max-w-xs w-full">
            <h2 class="text-4xl font-black font-orbitron text-pink-500 neon-glow-magenta mb-1">SYSTEM OVERRIDE</h2>
            <p class="text-gray-400 text-xs uppercase tracking-widest mb-6">Run Terminated</p>

            <div class="bg-gray-900/80 rounded-2xl p-5 border border-pink-500/30 mb-6 space-y-3">
                <div class="flex justify-between items-center">
                    <span class="text-gray-400 text-sm">Final Score</span>
                    <span id="final-score" class="text-2xl font-bold font-orbitron text-cyan-400">0</span>
                </div>
                <div class="flex justify-between items-center">
                    <span class="text-gray-400 text-sm">High Score</span>
                    <span id="final-high-score" class="text-xl font-bold font-orbitron text-yellow-400">0</span>
                </div>
                <div class="flex justify-between items-center pt-2 border-t border-gray-800">
                    <span class="text-gray-400 text-sm">Max Combo</span>
                    <span id="final-combo" class="text-lg font-bold font-orbitron text-pink-400">0x</span>
                </div>
            </div>

            <button id="restart-btn" class="pointer-events-auto w-full py-4 rounded-xl font-orbitron font-bold text-lg text-white btn-action neon-box-magenta flex items-center justify-center gap-3 cursor-pointer">
                <i class="fa-solid fa-rotate-right"></i> TRY AGAIN
            </button>
        </div>
    </div>

    <!-- Main Game Canvas -->
    <canvas id="gameCanvas" class="w-full h-full"></canvas>

    <script>
        class SoundFX {
            constructor() {
                this.ctx = null;
            }

            init() {
                if (!this.ctx) {
                    this.ctx = new (window.AudioContext || window.webkitAudioContext)();
                }
                if (this.ctx.state === 'suspended') {
                    this.ctx.resume();
                }
            }

            playJump() {
                if (!this.ctx) return;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(300, this.ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(800, this.ctx.currentTime + 0.15);
                gain.gain.setValueAtTime(0.3, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.15);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start();
                osc.stop(this.ctx.currentTime + 0.15);
            }

            playCollect() {
                if (!this.ctx) return;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'triangle';
                osc.frequency.setValueAtTime(523.25, this.ctx.currentTime);
                osc.frequency.setValueAtTime(659.25, this.ctx.currentTime + 0.08);
                osc.frequency.setValueAtTime(783.99, this.ctx.currentTime + 0.16);
                gain.gain.setValueAtTime(0.2, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.25);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start();
                osc.stop(this.ctx.currentTime + 0.25);
            }

            playHit() {
                if (!this.ctx) return;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(150, this.ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(30, this.ctx.currentTime + 0.3);
                gain.gain.setValueAtTime(0.4, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.3);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start();
                osc.stop(this.ctx.currentTime + 0.3);
            }

            playSwitch() {
                if (!this.ctx) return;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(400, this.ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(200, this.ctx.currentTime + 0.08);
                gain.gain.setValueAtTime(0.2, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.08);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start();
                osc.stop(this.ctx.currentTime + 0.08);
            }
        }

        const audio = new SoundFX();

        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        let isPlaying = false;
        let score = 0;
        let highScore = localStorage.getItem('neondash_highscore') || 0;
        let multiplier = 1;
        let combo = 0;
        let maxCombo = 0;
        let speed = 6;
        let frameCount = 0;

        const scoreDisplay = document.getElementById('score-display');
        const highScoreDisplay = document.getElementById('high-score-display');
        const multiplierDisplay = document.getElementById('multiplier-display');
        const comboBar = document.getElementById('combo-bar');
        const startMenu = document.getElementById('start-menu');
        const gameOverMenu = document.getElementById('game-over-menu');
        const finalScore = document.getElementById('final-score');
        const finalHighScore = document.getElementById('final-high-score');
        const finalCombo = document.getElementById('final-combo');

        highScoreDisplay.textContent = highScore;

        let width = canvas.width = window.innerWidth;
        let height = canvas.height = window.innerHeight;

        window.addEventListener('resize', () => {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
            setupLanes();
        });

        let lanes = [0, 0, 0];
        function setupLanes() {
            const laneSpacing = Math.min(width * 0.25, 110);
            const center = width / 2;
            lanes = [center - laneSpacing, center, center + laneSpacing];
        }
        setupLanes();

        const player = {
            lane: 1,
            x: lanes[1],
            targetX: lanes[1],
            y: height - 150,
            radius: 18,
            color: '#00f3ff',
            jumpY: 0,
            jumpSpeed: 0,
            gravity: 0.8,
            isJumping: false,
            canDoubleJump: true,
            pulse: 0,

            update() {
                this.x += (this.targetX - this.x) * 0.25;

                if (this.isJumping) {
                    this.jumpY += this.jumpSpeed;
                    this.jumpSpeed += this.gravity;

                    if (this.jumpY >= 0) {
                        this.jumpY = 0;
                        this.jumpSpeed = 0;
                        this.isJumping = false;
                        this.canDoubleJump = true;
                    }
                }

                this.pulse += 0.08;
            },

            draw() {
                const currentY = this.y + this.jumpY;
                const activeRadius = this.radius + Math.sin(this.pulse) * 2;

                ctx.save();
                ctx.shadowBlur = 20;
                ctx.shadowColor = this.color;

                ctx.beginPath();
                ctx.arc(this.x, currentY, activeRadius + 6, 0, Math.PI * 2);
                ctx.strokeStyle = 'rgba(0, 243, 255, 0.5)';
                ctx.lineWidth = 2;
                ctx.stroke();

                ctx.beginPath();
                ctx.arc(this.x, currentY, activeRadius, 0, Math.PI * 2);
                ctx.fillStyle = '#ffffff';
                ctx.fill();

                if (this.jumpY < 0) {
                    ctx.restore();
                    ctx.save();
                    ctx.beginPath();
                    const shadowScale = Math.max(0.2, 1 + this.jumpY / 200);
                    ctx.ellipse(this.x, this.y + 10, this.radius * shadowScale, this.radius * 0.3 * shadowScale, 0, 0, Math.PI * 2);
                    ctx.fillStyle = 'rgba(0, 0, 0, 0.4)';
                    ctx.fill();
                }

                ctx.restore();
            },

            jump() {
                if (!this.isJumping) {
                    this.isJumping = true;
                    this.jumpSpeed = -13;
                    audio.playJump();
                    createParticles(this.x, this.y, '#00f3ff', 12);
                } else if (this.canDoubleJump) {
                    this.canDoubleJump = false;
                    this.jumpSpeed = -11;
                    audio.playJump();
                    createParticles(this.x, this.y + this.jumpY, '#ff0055', 16);
                }
            },

            switchLane(dir) {
                if (dir === 'left' && this.lane > 0) {
                    this.lane--;
                    audio.playSwitch();
                } else if (dir === 'right' && this.lane < 2) {
                    this.lane++;
                    audio.playSwitch();
                }
                this.targetX = lanes[this.lane];
            }
        };

        let particles = [];
        function createParticles(x, y, color, count = 10) {
            for (let i = 0; i < count; i++) {
                particles.push({
                    x, y,
                    vx: (Math.random() - 0.5) * 8,
                    vy: (Math.random() - 0.5) * 8,
                    radius: Math.random() * 4 + 2,
                    color,
                    alpha: 1,
                    decay: Math.random() * 0.03 + 0.02
                });
            }
        }

        function updateParticles() {
            for (let i = particles.length - 1; i >= 0; i--) {
                const p = particles[i];
                p.x += p.vx;
                p.y += p.vy;
                p.alpha -= p.decay;
                if (p.alpha <= 0) particles.splice(i, 1);
            }
        }

        function drawParticles() {
            particles.forEach(p => {
                ctx.save();
                ctx.globalAlpha = p.alpha;
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
                ctx.fillStyle = p.color;
                ctx.shadowBlur = 10;
                ctx.shadowColor = p.color;
                ctx.fill();
                ctx.restore();
            });
        }

        let obstacles = [];
        let collectibles = [];

        class Obstacle {
            constructor() {
                this.lane = Math.floor(Math.random() * 3);
                this.x = lanes[this.lane];
                this.y = -50;
                this.width = 50;
                this.height = 25;
                this.color = '#ff0055';
                this.type = Math.random() > 0.7 ? 'laser' : 'block';
            }

            update() {
                this.y += speed;
            }

            draw() {
                ctx.save();
                ctx.shadowBlur = 15;
                ctx.shadowColor = this.color;

                if (this.type === 'laser') {
                    ctx.fillStyle = '#ff0055';
                    ctx.fillRect(this.x - 35, this.y - 5, 70, 10);
                    ctx.fillStyle = '#ffffff';
                    ctx.fillRect(this.x - 30, this.y - 2, 60, 4);
                } else {
                    ctx.fillStyle = 'rgba(255, 0, 85, 0.2)';
                    ctx.strokeStyle = '#ff0055';
                    ctx.lineWidth = 3;
                    ctx.beginPath();
                    ctx.roundRect(this.x - 25, this.y - 25, 50, 50, 8);
                    ctx.fill();
                    ctx.stroke();
                }
                ctx.restore();
            }
        }

        class Collectible {
            constructor() {
                this.lane = Math.floor(Math.random() * 3);
                this.x = lanes[this.lane];
                this.y = -50;
                this.radius = 12;
                this.color = Math.random() > 0.3 ? '#00f3ff' : '#ffbe0b';
                this.isGolden = this.color === '#ffbe0b';
                this.rotation = 0;
            }

            update() {
                this.y += speed;
                this.rotation += 0.05;
            }

            draw() {
                ctx.save();
                ctx.translate(this.x, this.y);
                ctx.rotate(this.rotation);
                ctx.shadowBlur = 12;
                ctx.shadowColor = this.color;

                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.moveTo(0, -this.radius * 1.3);
                ctx.lineTo(this.radius, 0);
                ctx.lineTo(0, this.radius * 1.3);
                ctx.lineTo(-this.radius, 0);
                ctx.closePath();
                ctx.fill();

                ctx.restore();
            }
        }

        function spawnEntities() {
            if (frameCount % Math.max(25, Math.floor(60 - speed * 3)) === 0) {
                if (Math.random() > 0.3) {
                    obstacles.push(new Obstacle());
                } else {
                    collectibles.push(new Collectible());
                }
            }
        }

        let gridOffset = 0;
        function drawBackground() {
            const bgGrad = ctx.createLinearGradient(0, 0, 0, height);
            bgGrad.addColorStop(0, '#030308');
            bgGrad.addColorStop(1, '#0a0a1a');
            ctx.fillStyle = bgGrad;
            ctx.fillRect(0, 0, width, height);

            ctx.save();
            ctx.strokeStyle = 'rgba(0, 243, 255, 0.08)';
            ctx.lineWidth = 1.5;

            gridOffset = (gridOffset + speed * 1.5) % 40;
            for (let y = gridOffset; y < height; y += 40) {
                ctx.beginPath();
                ctx.moveTo(0, y);
                ctx.lineTo(width, y);
                ctx.stroke();
            }

            lanes.forEach(laneX => {
                ctx.strokeStyle = 'rgba(0, 243, 255, 0.2)';
                ctx.setLineDash([15, 15]);
                ctx.beginPath();
                ctx.moveTo(laneX, 0);
                ctx.lineTo(laneX, height);
                ctx.stroke();
            });

            ctx.restore();
        }

        let touchStartX = 0;
        let touchStartY = 0;

        window.addEventListener('touchstart', (e) => {
            touchStartX = e.touches[0].clientX;
            touchStartY = e.touches[0].clientY;
        }, { passive: true });

        window.addEventListener('touchend', (e) => {
            if (!isPlaying) return;
            const deltaX = e.changedTouches[0].clientX - touchStartX;
            const deltaY = e.changedTouches[0].clientY - touchStartY;

            if (Math.abs(deltaX) > 30 && Math.abs(deltaX) > Math.abs(deltaY)) {
                if (deltaX > 0) player.switchLane('right');
                else player.switchLane('left');
            } else if (deltaY < -30) {
                player.jump();
            }
        }, { passive: true });

        document.getElementById('left-touch-btn').addEventListener('touchstart', (e) => {
            e.preventDefault();
            audio.init();
            if (isPlaying) player.switchLane('left');
        });

        document.getElementById('right-touch-btn').addEventListener('touchstart', (e) => {
            e.preventDefault();
            audio.init();
            if (isPlaying) player.switchLane('right');
        });

        document.getElementById('jump-touch-btn').addEventListener('touchstart', (e) => {
            e.preventDefault();
            audio.init();
            if (isPlaying) player.jump();
        });

        window.addEventListener('keydown', (e) => {
            audio.init();
            if (!isPlaying) return;

            if (e.key === 'ArrowLeft' || e.key === 'a') player.switchLane('left');
            if (e.key === 'ArrowRight' || e.key === 'd') player.switchLane('right');
            if (e.key === 'ArrowUp' || e.key === 'w' || e.key === ' ') player.jump();
        });

        function gameLoop() {
            if (!isPlaying) return;

            frameCount++;
            speed = 6 + Math.floor(score / 150) * 0.5;

            drawBackground();
            spawnEntities();

            // Collectibles logic
            for (let i = collectibles.length - 1; i >= 0; i--) {
                const c = collectibles[i];
                c.update();
                c.draw();

                const playerY = player.y + player.jumpY;
                const dist = Math.hypot(player.x - c.x, playerY - c.y);

                if (dist < player.radius + c.radius) {
                    audio.playCollect();
                    combo++;
                    if (combo > maxCombo) maxCombo = combo;
                    multiplier = 1 + Math.floor(combo / 5) * 0.5;

                    score += (c.isGolden ? 30 : 10) * multiplier;
                    createParticles(c.x, c.y, c.color, 15);

                    scoreDisplay.textContent = Math.floor(score);
                    multiplierDisplay.textContent = `x${multiplier}`;
                    comboBar.style.width = `${Math.min(100, (combo % 5) * 20)}%`;

                    collectibles.splice(i, 1);
                    continue;
                }

                if (c.y > height + 50) collectibles.splice(i, 1);
            }

            // Obstacles logic & Collision
            for (let i = obstacles.length - 1; i >= 0; i--) {
                const ob = obstacles[i];
                ob.update();
                ob.draw();

                const playerY = player.y + player.jumpY;

                let hasHit = false;
                if (ob.lane === player.lane) {
                    if (ob.type === 'laser') {
                        if (Math.abs(ob.y - playerY) < 20 && player.jumpY > -20) {
                            hasHit = true;
                        }
                    } else {
                        if (Math.abs(ob.y - playerY) < 35) {
                            hasHit = true;
                        }
                    }
                }

                if (hasHit) {
                    audio.playHit();
                    createParticles(player.x, playerY, '#ff0055', 30);
                    gameOver();
                    return;
                }

                if (ob.y > height + 50) obstacles.splice(i, 1);
            }

            player.update();
            player.draw();

            updateParticles();
            drawParticles();

            requestAnimationFrame(gameLoop);
        }

        function startGame() {
            audio.init();
            isPlaying = true;
            score = 0;
            combo = 0;
            maxCombo = 0;
            multiplier = 1;
            speed = 6;
            obstacles = [];
            collectibles = [];
            particles = [];
            player.lane = 1;
            player.x = lanes[1];
            player.targetX = lanes[1];

            scoreDisplay.textContent = '0';
            multiplierDisplay.textContent = 'x1';
            comboBar.style.width = '0%';

            startMenu.classList.add('hidden');
            gameOverMenu.classList.add('hidden');

            requestAnimationFrame(gameLoop);
        }

        function gameOver() {
            isPlaying = false;
            if (score > highScore) {
                highScore = Math.floor(score);
                localStorage.setItem('neondash_highscore', highScore);
                highScoreDisplay.textContent = highScore;
            }

            finalScore.textContent = Math.floor(score);
            finalHighScore.textContent = highScore;
            finalCombo.textContent = `${maxCombo}x`;

            gameOverMenu.classList.remove('hidden');
        }

        document.getElementById('start-btn').addEventListener('click', startGame);
        document.getElementById('restart-btn').addEventListener('click', startGame);

        drawBackground();
    </script>
</body>
</html>
```