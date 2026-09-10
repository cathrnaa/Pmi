<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Math Realm: Petualangan Aljabar SMP</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        * {
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: #030712;
            color: #f9fafb;
            overflow: hidden;
        }
        .font-pixel {
            font-family: 'Press Start 2P', cursive;
        }
        .glass-panel {
            background: rgba(15, 23, 42, 0.92);
            backdrop-filter: blur(16px);
            border: 1px solid rgba(56, 189, 248, 0.25);
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.8);
        }
        .glow-cyan {
            box-shadow: 0 0 25px rgba(6, 182, 212, 0.5), inset 0 0 10px rgba(56, 189, 248, 0.3);
        }
        .canvas-container {
            box-shadow: 0 0 40px rgba(14, 165, 233, 0.25), 0 20px 50px rgba(0, 0, 0, 0.9);
        }
        /* Touch Control Buttons */
        .btn-touch {
            background: rgba(15, 23, 42, 0.85);
            border: 2px solid rgba(56, 189, 248, 0.4);
            backdrop-filter: blur(12px);
            transition: all 0.1s ease;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
        }
        .btn-touch:active {
            background: rgba(14, 165, 233, 0.7);
            transform: scale(0.92);
            box-shadow: 0 0 20px rgba(56, 189, 248, 0.8);
        }
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-thumb {
            background: #38bdf8;
            border-radius: 4px;
        }
    </style>
</head>
<body class="w-screen h-screen flex flex-col justify-between items-center relative bg-slate-950">

    <header class="w-full max-w-5xl px-3 py-2 z-20 flex justify-between items-center glass-panel rounded-b-2xl mt-1">
        <!-- Player Hearts & Gem Score -->
        <div class="flex items-center gap-3">
            <div id="hud-hearts" class="flex items-center gap-1 text-rose-500 text-lg">
                <i class="fa-solid fa-heart"></i>
                <i class="fa-solid fa-heart"></i>
                <i class="fa-solid fa-heart"></i>
            </div>
            <div class="flex items-center gap-1 bg-slate-900/90 px-2.5 py-1 rounded-xl border border-cyan-500/30">
                <span class="text-cyan-400 text-sm">ðŸ’Ž</span>
                <span id="hud-gems-count" class="font-pixel text-[10px] text-cyan-300">0 / 3</span>
            </div>
        </div>

        <!-- Level Name & World Progress -->
        <div class="text-center">
            <p id="hud-level-title" class="font-pixel text-[10px] text-amber-400 tracking-wider">LEVEL 1-1</p>
            <p id="hud-level-subtitle" class="text-[11px] font-semibold text-slate-300 hidden sm:block">Lembah Aljabar (PLSV)</p>
        </div>

        <!-- Gold Coins & Pause/Sound Settings -->
        <div class="flex items-center gap-2">
            <div class="flex items-center gap-1.5 bg-slate-900/90 px-3 py-1 rounded-xl border border-amber-500/30">
                <span class="text-amber-400 text-sm">ðŸª™</span>
                <span id="hud-gold-count" class="font-pixel text-[10px] text-amber-300">0</span>
            </div>
            <button id="btn-sound-toggle" class="p-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-xl border border-slate-700 transition">
                <i class="fa-solid fa-volume-high text-xs"></i>
            </button>
            <button onclick="openShopModal()" class="px-3 py-1 bg-gradient-to-r from-amber-500 to-yellow-600 hover:from-amber-400 hover:to-yellow-500 text-slate-950 rounded-xl font-bold text-xs shadow transition transform active:scale-95 flex items-center gap-1 font-pixel">
                <i class="fa-solid fa-store"></i>
                <span class="hidden sm:inline">TOKO</span>
            </button>
            <button onclick="openWorldMap()" class="px-3 py-1 bg-gradient-to-r from-cyan-500 to-blue-600 hover:from-cyan-400 hover:to-blue-500 text-white rounded-xl font-bold text-xs shadow transition transform active:scale-95 flex items-center gap-1 font-pixel">
                <i class="fa-solid fa-map"></i>
                <span class="hidden sm:inline">PETA</span>
            </button>
        </div>
    </header>

    <main class="relative flex-1 w-full max-w-5xl my-2 flex items-center justify-center px-2">
        <canvas id="gameCanvas" class="w-full h-full max-h-[78vh] object-contain rounded-2xl border-2 border-cyan-500/40 bg-slate-950 canvas-container"></canvas>

        <!-- START OVERLAY -->
        <div id="start-overlay" class="absolute inset-0 z-30 flex flex-col items-center justify-center p-6 text-center glass-panel rounded-2xl m-2 bg-slate-950/95">
            <div class="w-20 h-20 bg-gradient-to-tr from-cyan-500 via-blue-600 to-amber-400 rounded-3xl flex items-center justify-center text-4xl mb-4 shadow-xl glow-cyan animate-bounce">
                ðŸƒâ€â™‚ï¸
            </div>
            <h1 class="font-pixel text-xl sm:text-3xl text-cyan-300 mb-2">MATH REALM</h1>
            <p class="font-pixel text-xs text-amber-400 mb-6">Petualangan Platformer Aljabar SMP</p>
            
            <div class="bg-slate-900/90 p-4 rounded-xl border border-slate-800 text-xs text-slate-300 max-w-md mb-6 text-left space-y-2.5">
                <p>ðŸŽ¯ <strong>Misi Utama:</strong> Jelajahi dunia, kumpulkan 3 Permata Sihir (ðŸ’Ž), dan capai Portal Akhir untuk dapat 3 Bintang â­!</p>
                <p>ðŸ‘¾ <strong>Rintangan & Musuh:</strong> Injak musuh Slime untuk mengalahkannya! Hindari jurang dan duri tajam.</p>
                <p>ðŸ§© <strong>Peti Harta Aljabar:</strong> Buka peti terkunci dengan menyelesaikan soal persamaan & pertidaksamaan aljabar!</p>
            </div>

            <button onclick="startGame()" class="px-8 py-3.5 bg-gradient-to-r from-emerald-500 via-teal-400 to-cyan-500 hover:scale-105 text-slate-950 font-pixel text-xs sm:text-sm rounded-xl shadow-xl transition transform active:scale-95 glow-cyan">
                MULAI PETUALANGAN ðŸš€
            </button>
        </div>

        <!-- MATH QUESTION MODAL -->
        <div id="math-modal" class="hidden absolute inset-0 z-40 flex flex-col items-center justify-center p-4 glass-panel rounded-2xl m-2 bg-slate-950/95">
            <div class="w-full max-w-2xl bg-slate-900 p-5 rounded-2xl border border-cyan-500/40 text-center shadow-2xl flex flex-col md:flex-row gap-4">
                
                <!-- Left Column: Question & Choices -->
                <div class="flex-1 flex flex-col justify-between">
                    <div>
                        <div id="math-type-badge" class="inline-flex items-center gap-1.5 px-3 py-1 bg-cyan-500/20 border border-cyan-500/40 rounded-full text-cyan-300 text-[11px] font-bold mb-3">
                            <i class="fa-solid fa-key"></i> KUNCI PETI MATEMATIKA
                        </div>

                        <!-- Visual Diagram/Hint Box -->
                        <div id="math-visual-box" class="bg-slate-950 p-3 rounded-xl border border-slate-800 mb-3 text-xs text-amber-300 flex items-center justify-center min-h-[50px]">
                        </div>

                        <h3 id="math-question" class="font-bold text-sm sm:text-base text-white mb-4">
                            Soal Matematika...
                        </h3>
                    </div>

                    <!-- Options Grid -->
                    <div id="math-options-grid" class="grid grid-cols-2 gap-2">
                    </div>

                    <!-- Step Feedback Box -->
                    <div id="math-feedback" class="hidden mt-3 p-3 rounded-xl text-xs leading-relaxed text-left"></div>
                </div>

                <!-- Right Column: Interactive Whiteboard / Scratchpad -->
                <div class="w-full md:w-64 bg-slate-950 p-3 rounded-xl border border-slate-800 flex flex-col">
                    <div class="flex justify-between items-center mb-2">
                        <span class="text-[11px] font-bold text-slate-400 flex items-center gap-1">
                            <i class="fa-solid fa-pen-ruler text-cyan-400"></i> Papan Coretan
                        </span>
                        <button onclick="clearScratchpad()" class="text-[10px] px-2 py-0.5 bg-slate-800 hover:bg-slate-700 text-rose-400 rounded">
                            Hapus
                        </button>
                    </div>
                    <canvas id="scratchpadCanvas" class="w-full h-36 bg-slate-900 rounded-lg border border-slate-800 cursor-crosshair touch-none"></canvas>
                    <p class="text-[9px] text-slate-500 mt-1">Gunakan jari / mouse untuk menghitung.</p>
                </div>

            </div>
        </div>

        <!-- WORLD MAP MODAL -->
        <div id="worldmap-modal" class="hidden absolute inset-0 z-40 flex flex-col items-center justify-center p-4 glass-panel rounded-2xl m-2 bg-slate-950/95">
            <div class="w-full max-w-xl bg-slate-900 p-5 rounded-2xl border border-amber-500/40 text-center">
                <div class="flex justify-between items-center mb-4 pb-3 border-b border-slate-800">
                    <h2 class="font-pixel text-xs sm:text-sm text-amber-300 flex items-center gap-2">
                        <i class="fa-solid fa-map-location-dot"></i> PETA DUNIA ALJABAR
                    </h2>
                    <button onclick="closeWorldMap()" class="p-1.5 bg-slate-800 hover:bg-slate-700 text-slate-400 rounded-lg">
                        <i class="fa-solid fa-xmark text-sm"></i>
                    </button>
                </div>

                <!-- Level List Cards Container -->
                <div id="worldmap-levels-grid" class="grid grid-cols-1 sm:grid-cols-3 gap-3 my-4">
                </div>
            </div>
        </div>

        <!-- SHOP MODAL -->
        <div id="shop-modal" class="hidden absolute inset-0 z-40 flex flex-col items-center justify-center p-4 glass-panel rounded-2xl m-2 bg-slate-950/95">
            <div class="w-full max-w-xl bg-slate-900 p-5 rounded-2xl border border-amber-500/40 text-center max-h-[85vh] overflow-y-auto">
                <div class="flex justify-between items-center mb-4 pb-3 border-b border-slate-800">
                    <div class="flex items-center gap-2">
                        <h2 class="font-pixel text-xs sm:text-sm text-amber-300 flex items-center gap-2">
                            <i class="fa-solid fa-store text-amber-400"></i> TOKO SIHIR & SKIN
                        </h2>
                    </div>
                    <div class="flex items-center gap-3">
                        <div class="flex items-center gap-1 bg-slate-950 px-3 py-1 rounded-xl border border-amber-500/30">
                            <span class="text-amber-400 text-xs">ðŸª™</span>
                            <span id="shop-gold-count" class="font-pixel text-[11px] text-amber-300">0</span>
                        </div>
                        <button onclick="closeShopModal()" class="p-1.5 bg-slate-800 hover:bg-slate-700 text-slate-400 rounded-lg">
                            <i class="fa-solid fa-xmark text-sm"></i>
                        </button>
                    </div>
                </div>

                <!-- Items & Powerups Grid -->
                <p class="text-left font-pixel text-[10px] text-cyan-400 mb-2">âš¡ POWER-UP & ITEM SIHIR</p>
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 mb-5 text-left">
                    <div class="bg-slate-950 p-3 rounded-xl border border-slate-800 flex justify-between items-center">
                        <div class="flex items-center gap-3">
                            <div class="text-2xl">â¤ï¸</div>
                            <div>
                                <h4 class="font-bold text-xs text-white">Pulih Nyawa (+1)</h4>
                                <p class="text-[10px] text-slate-400">Tambah 1 jantung nyawa</p>
                            </div>
                        </div>
                        <button onclick="buyHeart()" class="px-3 py-1.5 bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold text-xs rounded-xl font-pixel flex items-center gap-1 shadow">
                            50 ðŸª™
                        </button>
                    </div>

                    <div class="bg-slate-950 p-3 rounded-xl border border-slate-800 flex justify-between items-center">
                        <div class="flex items-center gap-3">
                            <div class="text-2xl">ðŸ›¡ï¸</div>
                            <div>
                                <h4 class="font-bold text-xs text-white">Perisai Sihir</h4>
                                <p class="text-[10px] text-slate-400">Kebal 1x kebal serangan</p>
                            </div>
                        </div>
                        <button onclick="buyShield()" class="px-3 py-1.5 bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold text-xs rounded-xl font-pixel flex items-center gap-1 shadow">
                            100 ðŸª™
                        </button>
                    </div>
                </div>

                <!-- Avatar Skins Grid -->
                <p class="text-left font-pixel text-[10px] text-amber-400 mb-2">ðŸŽ­ KOSTUM & SKIN HERO</p>
                <div id="shop-skins-grid" class="grid grid-cols-2 sm:grid-cols-4 gap-2 text-left">
                </div>
            </div>
        </div>

        <!-- VICTORY MODAL -->
        <div id="victory-modal" class="hidden absolute inset-0 z-50 flex flex-col items-center justify-center p-4 glass-panel rounded-2xl m-2 bg-slate-950/95">
            <div class="w-full max-w-md bg-slate-900 p-6 rounded-2xl border border-amber-500/50 text-center shadow-2xl">
                <div class="text-5xl mb-3 animate-bounce">ðŸ†</div>
                <h2 class="font-pixel text-sm sm:text-base text-amber-300 mb-1">LEVEL SELESAI!</h2>
                <p class="text-xs text-slate-400 mb-4">Kamu berhasil menyelesaikan petualangan!</p>

                <div id="victory-stars" class="text-3xl text-amber-400 mb-4 tracking-widest">
                    â­â­â­
                </div>

                <div class="bg-slate-950 p-3 rounded-xl border border-slate-800 text-xs space-y-2 mb-6">
                    <div class="flex justify-between text-slate-300">
                        <span>Permata Sihir Ditemukan:</span>
                        <span id="vic-gems" class="font-bold text-cyan-300">3 / 3 ðŸ’Ž</span>
                    </div>
                    <div class="flex justify-between text-slate-300">
                        <span>Total Koin Emas:</span>
                        <span id="vic-gold" class="font-bold text-amber-300">+150 ðŸª™</span>
                    </div>
                </div>

                <div class="flex justify-center gap-3">
                    <button onclick="restartCurrentLevel()" class="px-4 py-2.5 bg-slate-800 hover:bg-slate-700 text-slate-300 font-pixel text-[10px] rounded-xl border border-slate-700">
                        ULANG ðŸ”„
                    </button>
                    <button onclick="nextLevelOrMap()" class="px-6 py-2.5 bg-gradient-to-r from-emerald-500 to-teal-400 text-slate-950 font-pixel text-[10px] rounded-xl font-bold shadow glow-cyan">
                        LANJUT ðŸš€
                    </button>
                </div>
            </div>
        </div>
    </main>

    <footer class="w-full max-w-5xl px-4 py-2 z-20 flex justify-between items-center gap-4">
        <!-- Directional Touch Buttons -->
        <div class="flex items-center gap-2">
            <button onmousedown="handleTouch('LEFT', true)" onmouseup="handleTouch('LEFT', false)" ontouchstart="handleTouch('LEFT', true)" ontouchend="handleTouch('LEFT', false)" class="btn-touch w-12 h-12 rounded-2xl flex items-center justify-center text-xl text-white shadow-lg">
                â¬…ï¸
            </button>
            <button onmousedown="handleTouch('RIGHT', true)" onmouseup="handleTouch('RIGHT', false)" ontouchstart="handleTouch('RIGHT', true)" ontouchend="handleTouch('RIGHT', false)" class="btn-touch w-12 h-12 rounded-2xl flex items-center justify-center text-xl text-white shadow-lg">
                âž¡ï¸
            </button>
        </div>

        <div class="hidden sm:flex items-center gap-3 text-[11px] text-slate-400 bg-slate-900/80 px-4 py-2 rounded-xl border border-slate-800 font-semibold">
            <span><kbd class="px-1.5 py-0.5 bg-slate-800 border border-slate-700 rounded text-cyan-300">A / D</kbd> Bergerak</span>
            <span>â€¢</span>
            <span><kbd class="px-1.5 py-0.5 bg-slate-800 border border-slate-700 rounded text-cyan-300">W / Space</kbd> Lompat (2x Double Jump)</span>
        </div>

        <button onmousedown="handleTouch('JUMP', true)" onmouseup="handleTouch('JUMP', false)" ontouchstart="handleTouch('JUMP', true)" ontouchend="handleTouch('JUMP', false)" class="px-6 py-3.5 bg-gradient-to-r from-cyan-500 to-blue-600 active:from-cyan-400 active:to-blue-500 text-white font-pixel text-xs rounded-2xl shadow-lg border border-cyan-400/50 flex items-center gap-1.5 glow-cyan">
            <span>LOMPAT</span> ðŸš€
        </button>
    </footer>

    <script>
        // Web Audio API Sound Generator
        class SoundEngine {
            constructor() {
                this.ctx = null;
                this.muted = false;
            }
            init() {
                if (!this.ctx) {
                    this.ctx = new (window.AudioContext || window.webkitAudioContext)();
                }
            }
            playTone(freq, type='sine', duration=0.15, vol=0.1) {
                if (this.muted || !this.ctx) return;
                try {
                    const osc = this.ctx.createOscillator();
                    const gain = this.ctx.createGain();
                    osc.type = type;
                    osc.frequency.setValueAtTime(freq, this.ctx.currentTime);
                    gain.gain.setValueAtTime(vol, this.ctx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.0001, this.ctx.currentTime + duration);
                    osc.connect(gain);
                    gain.connect(this.ctx.destination);
                    osc.start();
                    osc.stop(this.ctx.currentTime + duration);
                } catch(e){}
            }
            jump() {
                if (this.muted || !this.ctx) return;
                try {
                    const osc = this.ctx.createOscillator();
                    const gain = this.ctx.createGain();
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(150, this.ctx.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(420, this.ctx.currentTime + 0.12);
                    gain.gain.setValueAtTime(0.12, this.ctx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.12);
                    osc.connect(gain);
                    gain.connect(this.ctx.destination);
                    osc.start();
                    osc.stop(this.ctx.currentTime + 0.12);
                } catch(e){}
            }
            coin() {
                this.playTone(987, 'sine', 0.08, 0.1);
                setTimeout(() => this.playTone(1318, 'sine', 0.15, 0.1), 80);
            }
