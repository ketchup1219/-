<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>草抜きリバイバル - EXTREME</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700;900&family=Russo+One&display=swap');
        * { -webkit-tap-highlight-color: transparent; }
        body { font-family: 'Noto Sans JP', sans-serif; touch-action: none; overflow: hidden; background-color: #000105; }
        .font-display { font-family: 'Russo One', 'Noto Sans JP', sans-serif; }

        /* ==================== ガラス質感 (強化版) ==================== */
        .glass-panel {
            background: linear-gradient(160deg, rgba(30, 41, 59, 0.75), rgba(10, 15, 28, 0.75));
            backdrop-filter: blur(18px);
            -webkit-backdrop-filter: blur(18px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 4px 30px rgba(0, 0, 0, 0.6), inset 0 1px 1px rgba(255,255,255,0.15), inset 0 -1px 8px rgba(0,0,0,0.4);
            position: relative;
        }
        .glass-panel::before {
            content: ''; position: absolute; inset: 0; border-radius: inherit; pointer-events: none;
            background: linear-gradient(180deg, rgba(255,255,255,0.12) 0%, transparent 40%);
        }

        .text-glow { text-shadow: 0 0 12px currentColor; }
        .text-outline { -webkit-text-stroke: 1.5px rgba(0,0,0,0.6); paint-order: stroke fill; }

        /* ==================== 質感付きボタン ==================== */
        .btn-tactile {
            position: relative; overflow: hidden;
            box-shadow: 0 4px 0 rgba(0,0,0,0.35), 0 8px 18px rgba(0,0,0,0.45), inset 0 1px 2px rgba(255,255,255,0.35), inset 0 -4px 8px rgba(0,0,0,0.2);
            transition: transform 0.08s ease, box-shadow 0.08s ease;
        }
        .btn-tactile:active {
            transform: translateY(3px) scale(0.97);
            box-shadow: 0 1px 0 rgba(0,0,0,0.35), 0 2px 6px rgba(0,0,0,0.4), inset 0 1px 2px rgba(255,255,255,0.25), inset 0 -2px 4px rgba(0,0,0,0.25);
        }
        .btn-tactile::after {
            content: ''; position: absolute; top: -60%; left: -20%; width: 60%; height: 220%;
            background: linear-gradient(120deg, transparent 20%, rgba(255,255,255,0.35) 45%, transparent 70%);
            transform: rotate(20deg) translateX(-160%);
            animation: btn-sheen 3.2s ease-in-out infinite;
            pointer-events: none;
        }
        @keyframes btn-sheen { 0%, 60% { transform: rotate(20deg) translateX(-160%); } 100% { transform: rotate(20deg) translateX(220%); } }

        @keyframes pulse-glow {
            0%, 100% { box-shadow: 0 0 15px rgba(245, 158, 11, 0.6), inset 0 0 10px rgba(255,255,255,0.1); transform: scale(1); }
            50% { box-shadow: 0 0 40px rgba(245, 158, 11, 1), inset 0 0 15px rgba(255,255,255,0.3); transform: scale(1.08); }
        }
        .skill-ready { animation: pulse-glow 1.1s infinite ease-in-out; }

        @keyframes screen-shake {
            0%, 100% { transform: translate(0, 0); }
            10%, 30%, 50%, 70%, 90% { transform: translate(-12px, 8px); }
            20%, 40%, 60%, 80% { transform: translate(12px, -8px); }
        }
        .shake { animation: screen-shake 0.5s cubic-bezier(.36,.07,.19,.97) both; }

        @keyframes flash-red {
            0% { background-color: rgba(220, 38, 38, 0.85); }
            100% { background-color: transparent; }
        }
        .flash { animation: flash-red 0.8s ease-out; }

        @keyframes flash-white {
            0% { background-color: rgba(255, 255, 255, 0.95); }
            100% { background-color: transparent; }
        }
        .flash-white-anim { animation: flash-white 0.5s ease-out; }

        /* エクストリーム虹色アニメーション */
        @keyframes rainbow-bg {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        .rainbow-button {
            background: linear-gradient(270deg, #ef4444, #f59e0b, #eab308, #10b981, #3b82f6, #6366f1, #d946ef);
            background-size: 200% 200%;
            animation: rainbow-bg 3s ease infinite;
            border: 1px solid rgba(255,255,255,0.5);
        }
        .rainbow-text {
            background: linear-gradient(270deg, #ef4444, #f59e0b, #10b981, #3b82f6, #d946ef);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            background-size: 200% 200%;
            animation: rainbow-bg 3s ease infinite;
        }

        #game-ui-header { height: 130px; }

        /* ==================== 背景演出(星空/オーロラ/蛍) ==================== */
        #bg-stars {
            position: absolute; inset: 0; background-repeat: repeat;
            background-image:
                radial-gradient(1.5px 1.5px at 20% 30%, rgba(255,255,255,0.9), transparent),
                radial-gradient(1px 1px at 70% 65%, rgba(255,255,255,0.7), transparent),
                radial-gradient(1.5px 1.5px at 40% 80%, rgba(255,255,255,0.8), transparent),
                radial-gradient(1px 1px at 85% 20%, rgba(255,255,255,0.6), transparent),
                radial-gradient(1px 1px at 10% 90%, rgba(255,255,255,0.7), transparent),
                radial-gradient(1.5px 1.5px at 55% 15%, rgba(255,255,255,0.8), transparent),
                radial-gradient(1px 1px at 95% 55%, rgba(255,255,255,0.6), transparent),
                radial-gradient(1px 1px at 30% 50%, rgba(255,255,255,0.5), transparent);
            background-size: 200px 200px;
            animation: twinkle 4s ease-in-out infinite alternate;
        }
        @keyframes twinkle { 0% { opacity: 0.4; } 100% { opacity: 1; } }

        .aurora-blob {
            position: absolute; border-radius: 50%; filter: blur(40px); mix-blend-mode: screen;
            animation: drift 18s ease-in-out infinite alternate;
        }
        @keyframes drift {
            0% { transform: translate(0,0) scale(1); }
            50% { transform: translate(30px, -20px) scale(1.15); }
            100% { transform: translate(-20px, 15px) scale(0.95); }
        }

        .firefly {
            position: absolute; border-radius: 50%; background: #fef08a;
            box-shadow: 0 0 6px 2px rgba(253, 224, 71, 0.9), 0 0 14px 5px rgba(253, 224, 71, 0.4);
            animation: firefly-float linear infinite;
        }
        @keyframes firefly-float {
            0% { transform: translate(0,0); opacity: 0; }
            10% { opacity: 0.9; }
            50% { transform: translate(var(--fx, 20px), var(--fy, -60px)); opacity: 1; }
            90% { opacity: 0.7; }
            100% { transform: translate(var(--fx2, -10px), var(--fy2, -140px)); opacity: 0; }
        }

        /* ==================== タイトル画面 ==================== */
        @keyframes title-float { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-8px); } }
        .title-float { animation: title-float 3.5s ease-in-out infinite; }
        @keyframes title-shine-sweep {
            0% { background-position: -200% center; }
            100% { background-position: 200% center; }
        }
        .title-shine {
            background: linear-gradient(100deg, #a7f3d0 20%, #ffffff 35%, #34d399 45%, #a7f3d0 60%);
            background-size: 250% auto;
            -webkit-background-clip: text; background-clip: text; color: transparent;
            animation: title-shine-sweep 3s linear infinite;
        }
        @keyframes ring-spin-pulse {
            0% { transform: rotate(0deg) scale(1); opacity: 0.5; }
            50% { transform: rotate(180deg) scale(1.05); opacity: 1; }
            100% { transform: rotate(360deg) scale(1); opacity: 0.5; }
        }
        .ring-pulse { animation: ring-spin-pulse 4s linear infinite; }

        /* ==================== コンボ表示 ==================== */
        @keyframes combo-pop {
            0% { transform: scale(0.5) rotate(-5deg); opacity: 0; }
            30% { transform: scale(1.25) rotate(3deg); opacity: 1; }
            60% { transform: scale(1) rotate(0deg); opacity: 1; }
            100% { transform: scale(1) rotate(0deg); opacity: 1; }
        }
        .combo-pop { animation: combo-pop 0.25s cubic-bezier(.36,1.5,.5,1) both; }

        /* ==================== ガチャ演出 ==================== */
        .gacha-stage { perspective: 1000px; }
        .gacha-card-3d {
            transform-style: preserve-3d; transition: transform 0.7s cubic-bezier(.2,.9,.3,1.2);
        }
        .gacha-card-3d.flipped { transform: rotateY(180deg); }
        .gacha-face { backface-visibility: hidden; position: absolute; inset: 0; }
        .gacha-face.back { transform: rotateY(180deg); }

        @keyframes ray-rotate { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
        .ray-rotate { animation: ray-rotate 6s linear infinite; }
        @keyframes ray-rotate-rev { from { transform: rotate(360deg); } to { transform: rotate(0deg); } }
        .ray-rotate-rev { animation: ray-rotate-rev 10s linear infinite; }

        @keyframes sparkle-pop {
            0% { transform: scale(0) rotate(0deg); opacity: 1; }
            60% { transform: scale(1.3) rotate(180deg); opacity: 1; }
            100% { transform: scale(0.4) rotate(320deg); opacity: 0; }
        }
        .sparkle-pop-item { animation: sparkle-pop 0.9s ease-out forwards; }

        @keyframes gacha-flash-burst {
            0% { opacity: 0; transform: scale(0.3); }
            15% { opacity: 1; transform: scale(1.4); }
            100% { opacity: 0; transform: scale(2.2); }
        }
        .gacha-flash-burst { animation: gacha-flash-burst 0.6s ease-out forwards; }

        @keyframes shake-anticipation {
            0%, 100% { transform: translateX(0) rotate(0deg); }
            20% { transform: translateX(-6px) rotate(-3deg); }
            40% { transform: translateX(6px) rotate(3deg); }
            60% { transform: translateX(-8px) rotate(-4deg); }
            80% { transform: translateX(8px) rotate(4deg); }
        }
        .shake-anticipation { animation: shake-anticipation 0.5s ease-in-out infinite; }

        @keyframes rise-fade {
            0% { transform: translateY(0) scale(0.8); opacity: 0; }
            20% { opacity: 1; }
            100% { transform: translateY(-40px) scale(1.1); opacity: 0; }
        }

        /* レアリティ別発光カード */
        .rarity-glow-r { box-shadow: 0 0 30px 5px rgba(96,165,250,0.4); }
        .rarity-glow-sr { box-shadow: 0 0 40px 8px rgba(168,85,247,0.55); }
        .rarity-glow-ssr { box-shadow: 0 0 55px 12px rgba(250,204,21,0.65); }
        .rarity-glow-uz { box-shadow: 0 0 65px 15px rgba(244,114,182,0.7); }
        .rarity-glow-og { box-shadow: 0 0 70px 18px rgba(52,211,153,0.75); }
        .rarity-glow-x { box-shadow: 0 0 80px 22px rgba(217,70,239,0.85); }
        .rarity-glow-omega { box-shadow: 0 0 90px 26px rgba(252,211,77,0.9); }

        /* スクロールバー装飾 */
        #modal-content::-webkit-scrollbar { width: 6px; }
        #modal-content::-webkit-scrollbar-thumb { background: rgba(52,211,153,0.4); border-radius: 3px; }

        @keyframes card-frame-shine {
            0% { transform: translateX(-100%) translateY(-100%) rotate(35deg); }
            100% { transform: translateX(100%) translateY(100%) rotate(35deg); }
        }
    </style>
</head>
<body class="text-white select-none w-screen h-screen flex flex-col items-center relative">

    <div id="app-container" class="w-full h-full max-w-md relative flex flex-col bg-slate-900 shadow-2xl overflow-hidden">
        
        <!-- 背景装飾(多層パララックス) -->
        <div class="absolute inset-0 pointer-events-none z-0 overflow-hidden">
            <div class="absolute inset-0 bg-[radial-gradient(ellipse_at_top,#064e3b_0%,#020617_60%,#000105_100%)]"></div>
            <div id="bg-stars" class="absolute inset-0"></div>
            <div class="aurora-blob w-72 h-72 bg-emerald-500/25 top-[-40px] left-[-40px]"></div>
            <div class="aurora-blob w-64 h-64 bg-teal-400/20 top-1/3 right-[-60px]" style="animation-delay:-6s;"></div>
            <div class="aurora-blob w-56 h-56 bg-cyan-300/15 bottom-[-30px] left-1/4" style="animation-delay:-11s;"></div>
            <div id="bg-fireflies" class="absolute inset-0"></div>
            <div class="absolute w-full h-full bg-[url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjAiIGhlaWdodD0iMjAiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGNpcmNsZSBjeD0iMiIgY3k9IjIiIHI9IjAuNSIgZmlsbD0icmdiYSgyNTUsMjU1LDI1NSwwLjA0KSIvPjwvc3ZnPg==')]"></div>
            <div class="absolute inset-x-0 bottom-0 h-40 bg-gradient-to-t from-black/70 to-transparent"></div>
        </div>

        <!-- ==================== 1. タイトル画面 ==================== -->
        <div id="screen-title" class="absolute inset-0 z-50 flex flex-col items-center justify-center p-6 text-center transition-opacity duration-500">
            <div class="space-y-3 mb-16 relative title-float">
                <div class="absolute -inset-16 bg-emerald-500/25 blur-3xl rounded-full"></div>
                <div class="absolute -inset-10 bg-yellow-300/10 blur-2xl rounded-full"></div>
                <span class="text-xs tracking-[0.4em] text-emerald-300 font-black uppercase drop-shadow relative z-10 text-glow">Survival Action</span>
                <div class="relative z-10">
                    <h1 class="font-display text-6xl text-transparent bg-clip-text bg-gradient-to-br from-emerald-300 via-green-400 to-yellow-300 drop-shadow-[0_0_25px_rgba(52,211,153,0.7)] tracking-tighter py-2">
                        草抜き<br>サバイバル
                    </h1>
                    <h1 class="font-display text-6xl title-shine tracking-tighter py-2 absolute inset-0 opacity-70 mix-blend-overlay pointer-events-none">
                        草抜き<br>サバイバル
                    </h1>
                </div>
                <p class="text-xs text-emerald-100 font-bold tracking-widest mt-2 opacity-80 relative z-10">BEAUTIFUL GARDEN OR DEATH</p>
            </div>
            
            <button onclick="playSE('tap');switchScreen('home')" class="btn-tactile relative group overflow-hidden w-64 bg-gradient-to-b from-emerald-400 to-emerald-600 text-slate-950 font-black py-4 rounded-full shadow-[0_0_35px_rgba(16,185,129,0.6)] transition-all text-xl tracking-wider border border-emerald-200/50">
                <span class="relative z-10 font-display">START</span>
            </button>
            <p class="text-[9px] text-slate-500 font-bold mt-6 tracking-widest relative z-10">© KUSANUKI REVIVAL PROJECT</p>
        </div>

        <!-- ==================== 2. ホーム画面 ==================== -->
        <div id="screen-home" class="hidden absolute inset-0 z-40 flex flex-col p-5">
            <div class="glass-panel rounded-2xl p-4 flex justify-between items-center z-10 relative">
                <div class="flex items-center space-x-3">
                    <div class="w-12 h-12 bg-gradient-to-br from-emerald-400 to-teal-600 rounded-full flex items-center justify-center text-2xl shadow-[0_0_15px_rgba(52,211,153,0.4)] border-2 border-emerald-300/50">🧑‍🌾</div>
                    <div>
                        <div class="font-black text-sm text-white">マスターファーマー</div>
                        <div class="text-[10px] text-yellow-400 font-bold mt-0.5 tracking-wider">
                            🌱 <span id="home-coins">300</span> 
                            <span class="mx-0.5 text-slate-600">|</span> 
                            🦴 <span id="home-fossils">3</span>
                            <span class="mx-0.5 text-slate-600">|</span> 
                            <span class="text-fuchsia-400">🔮 <span id="home-cores">0</span></span>
                        </div>
                    </div>
                </div>
                <div class="text-right">
                    <div class="text-[10px] text-slate-400 font-bold">FARM LV</div>
                    <div id="home-farm-level" class="text-3xl font-black text-emerald-400 text-glow">1</div>
                </div>
            </div>

            <div class="flex-1 flex flex-col items-center justify-center w-full z-10 relative space-y-6 mt-4">
                <button onclick="playSE('tap');switchScreen('game')" class="btn-tactile w-56 h-56 rounded-full relative group shadow-[0_0_60px_rgba(16,185,129,0.35)] transition-all flex flex-col items-center justify-center">
                    <div class="absolute inset-0 rounded-full bg-gradient-to-br from-emerald-500/25 via-teal-700/30 to-teal-950/50 backdrop-blur-md border-2 border-emerald-400/40 group-hover:border-emerald-300/90 transition-colors"></div>
                    <div class="absolute inset-2 rounded-full border border-emerald-300/20"></div>
                    <div class="absolute inset-3 rounded-full border-2 border-dashed border-emerald-400/40 ring-pulse"></div>
                    <div class="absolute inset-0 rounded-full" style="background: conic-gradient(from 0deg, transparent, rgba(167,243,208,0.25), transparent 30%); animation: ray-rotate 5s linear infinite;"></div>
                    <div class="relative z-10 flex flex-col items-center">
                        <span class="text-5xl mb-2 filter drop-shadow-[0_0_14px_rgba(255,255,255,0.6)]">🌾</span>
                        <span class="font-display text-2xl text-white tracking-widest text-glow text-outline">草抜き開始</span>
                        <span class="text-[10px] text-emerald-300 mt-2 font-bold tracking-widest bg-emerald-950/80 px-4 py-1.5 rounded-full border border-emerald-500/30">TAP TO SORTIE</span>
                    </div>
                </button>
            </div>

            <!-- エクストリームボタン -->
            <button onclick="playSE('tap');openModal('extreme')" class="btn-tactile rainbow-button w-full py-3.5 rounded-2xl font-black text-white shadow-[0_0_25px_rgba(255,255,255,0.35)] transition z-10 relative mb-3 flex items-center justify-center space-x-2 border border-white/40">
                <span class="text-xl drop-shadow-md">✨</span>
                <span class="tracking-widest text-lg drop-shadow-md font-display">EXTREME</span>
                <span class="text-xl drop-shadow-md">✨</span>
            </button>

            <!-- メニュー4種 -->
            <div class="glass-panel rounded-2xl p-2 grid grid-cols-4 gap-2 z-10 relative mb-4">
                <button onclick="playSE('tap');openModal('mission')" class="py-3 flex flex-col items-center rounded-xl hover:bg-white/10 active:scale-90 transition-all">
                    <span class="text-xl mb-1 drop-shadow">📜</span><span class="text-[9px] font-bold text-slate-300">ミッション</span>
                </button>
                <button onclick="playSE('tap');openModal('collection')" class="py-3 flex flex-col items-center rounded-xl hover:bg-white/10 active:scale-90 transition-all">
                    <span class="text-xl mb-1 drop-shadow">📖</span><span class="text-[9px] font-bold text-slate-300">図鑑</span>
                </button>
                <button onclick="playSE('tap');openModal('gacha')" class="py-3 flex flex-col items-center rounded-xl hover:bg-white/10 active:scale-90 transition-all">
                    <span class="text-xl mb-1 drop-shadow">🎁</span><span class="text-[9px] font-bold text-slate-300">ガチャ</span>
                </button>
                <button onclick="playSE('tap');switchScreen('title')" class="py-3 flex flex-col items-center rounded-xl hover:bg-white/10 active:scale-90 transition-all">
                    <span class="text-xl mb-1 drop-shadow">🚪</span><span class="text-[9px] font-bold text-slate-300">タイトル</span>
                </button>
            </div>
        </div>

        <!-- ==================== 3. ゲームプレイ画面 ==================== -->
        <div id="screen-game" class="hidden absolute inset-0 z-30 flex flex-col">
            
            <div id="game-ui-header" class="p-4 z-20 absolute top-0 w-full flex flex-col space-y-3 pointer-events-none">
                <div class="flex justify-between items-start">
                    <div class="glass-panel px-5 py-2.5 rounded-2xl pointer-events-auto border-t-emerald-400/50 relative overflow-hidden">
                        <div class="text-[10px] text-emerald-400 font-bold uppercase tracking-widest mb-0.5">SCORE</div>
                        <div id="game-score" class="font-display text-3xl text-white leading-none tracking-tight text-glow">0</div>
                    </div>
                    <div class="text-right glass-panel px-4 py-2 rounded-2xl border-t-cyan-400/50">
                        <div class="text-[10px] text-cyan-400 font-bold uppercase tracking-widest text-glow mb-0.5">TIME LEFT</div>
                        <div id="game-timer" class="font-display text-3xl text-white leading-none drop-shadow-md">60</div>
                    </div>
                </div>

                <div class="flex items-center space-x-3 w-full pointer-events-auto">
                    <div class="glass-panel p-1.5 rounded-full flex-1 flex items-center space-x-3 pr-4 border-amber-500/40">
                        <button id="skill-btn" class="w-12 h-12 rounded-full bg-slate-800 text-slate-500 font-black text-sm shadow-[inset_0_2px_4px_rgba(0,0,0,0.6)] flex items-center justify-center transition-all duration-300 disabled:opacity-50" disabled>
                            🔥
                        </button>
                        <div class="flex-1 bg-slate-950/80 h-5 rounded-full overflow-hidden relative shadow-[inset_0_2px_5px_rgba(0,0,0,0.8)] border border-slate-700/50">
                            <div id="skill-bar" class="absolute left-0 top-0 h-full bg-gradient-to-r from-amber-600 via-amber-400 to-yellow-200 w-0 transition-all duration-300 ease-out shadow-[0_0_15px_rgba(250,204,21,0.8)]"></div>
                            <div class="absolute inset-0 bg-[linear-gradient(110deg,transparent_30%,rgba(255,255,255,0.35)_50%,transparent_70%)] bg-[length:200%_100%] animate-[btn-sheen_2.5s_linear_infinite]"></div>
                        </div>
                        <div id="skill-text" class="text-[11px] font-black text-amber-400 w-10 text-right tracking-wider">0%</div>
                    </div>
                    <button id="retire-btn" class="glass-panel w-12 h-12 rounded-full flex items-center justify-center text-lg border-rose-500/40 hover:bg-rose-500/20 active:scale-90 transition-all">
                        🏳️
                    </button>
                </div>
            </div>

            <!-- コンボ表示 -->
            <div id="combo-display" class="hidden absolute top-[38%] left-1/2 -translate-x-1/2 z-20 pointer-events-none text-center">
                <div id="combo-count" class="font-display text-6xl text-yellow-300 text-outline drop-shadow-[0_0_20px_rgba(250,204,21,0.8)]">0</div>
                <div class="text-sm font-black text-white tracking-[0.3em] text-outline -mt-2">COMBO</div>
            </div>

            <canvas id="game-canvas" class="w-full h-full absolute inset-0 z-10"></canvas>

            <div id="game-overlay" class="absolute inset-0 z-50 bg-slate-950/95 backdrop-blur-md flex flex-col items-center justify-center p-6 text-center transition-opacity duration-300 hidden overflow-hidden">
                <div id="bg-stars" class="absolute inset-0 opacity-40"></div>
                <h2 id="overlay-title" class="font-display text-5xl text-white mb-2 tracking-tighter drop-shadow-2xl relative z-10"></h2>
                <div id="overlay-desc" class="w-full max-w-sm glass-panel rounded-3xl p-6 my-8 space-y-4 border-t-white/20 relative z-10"></div>
                <button id="overlay-action-btn" class="btn-tactile w-full max-w-xs bg-gradient-to-b from-emerald-400 to-emerald-600 text-slate-950 font-black py-4 rounded-2xl shadow-[0_0_25px_rgba(16,185,129,0.5)] transition text-xl tracking-widest relative z-10 border border-emerald-200/50">
                    CONTINUE
                </button>
            </div>
        </div>

        <!-- ==================== 4. 汎用モーダル ==================== -->
        <div id="modal" class="hidden absolute inset-0 bg-black/80 backdrop-blur-sm z-50 flex flex-col items-center justify-center p-3 opacity-0 transition-opacity duration-200">
            <div class="bg-slate-900 border border-slate-700 w-full max-w-md rounded-3xl flex flex-col max-h-[85vh] shadow-[0_0_30px_rgba(0,0,0,0.8)] overflow-hidden transform scale-95 transition-transform duration-200" id="modal-container">
                <!-- ヘッダー -->
                <div class="p-4 border-b border-slate-800 flex justify-between items-center bg-slate-950/80">
                    <h3 id="modal-title" class="font-black text-sm tracking-widest text-emerald-400">メニュー</h3>
                    <button onclick="closeModal()" class="text-slate-400 hover:text-white font-bold text-[10px] px-3 py-1.5 bg-slate-800 rounded-lg active:scale-95 transition">✕ 閉じる</button>
                </div>
                <!-- 動的コンテンツボディ -->
                <div id="modal-content" class="flex-1 overflow-y-auto p-4 relative bg-slate-900">
                </div>
            </div>
        </div>

    </div>

    <script>
        // --- 状態管理・データ ---
        const RANKS = [
            { name: 'BASIC', maxLevel: 10, color: 'text-slate-300 bg-slate-800/80 border-slate-600' },
            { name: 'N', maxLevel: 20, color: 'text-slate-400 bg-slate-800/50 border-slate-700' },
            { name: 'R', maxLevel: 30, color: 'text-blue-400 bg-blue-950/40 border-blue-800' },
            { name: 'SR', maxLevel: 40, color: 'text-purple-400 bg-purple-950/40 border-purple-800' },
            { name: 'SSR', maxLevel: 50, color: 'text-yellow-400 bg-amber-950/40 border-amber-600' },
            { name: 'UZ', maxLevel: 60, color: 'text-pink-400 bg-pink-950/40 border-pink-700' },
            { name: 'OG', maxLevel: 80, color: 'text-emerald-400 bg-emerald-950/40 border-emerald-600' },
            { name: 'OMEGA', maxLevel: 100, color: 'text-amber-300 bg-yellow-950/50 border-yellow-500 god-glow' },
            { name: 'X', maxLevel: 200, color: 'text-white bg-black border-purple-500 rainbow-text shadow-[0_0_10px_rgba(217,70,239,0.8)]' },
            { name: 'ADMIN', maxLevel: 999, color: 'text-red-500 bg-red-950/60 border-red-600 animate-pulse' }
        ];

        // 変換済みキャラクターリスト
        const CHARACTERS = [
            // OMEGA
            { id: 'kazusan', name: 'ギャンブラーカズキング', rank: 'OMEGA', emoji: '🎰', score: 10000, coin: 5000, nextId: null, rate: 92.8, info: '777', isEvolved: true },
            { id: 'seimarotuyoi', name: '創世神せいまろ', rank: 'OMEGA', emoji: '⚡🐕', score: 12000, coin: 6000, nextId: null, rate: 100, info: '卵抜きで5秒間金75倍/雷霆蒼破', isEvolved: true },
            { id: 'deus_ex', name: 'ゴッドキング', rank: 'OMEGA', emoji: '⭐', score: 15000, coin: 7500, nextId: null, mult: 1200, cost: 0, rate: 100 },

            // OG
            { id: 'sei_god', name: '究極膳神せいまろ', rank: 'OG', emoji: '⚡🐕', score: 8000, coin: 4000, nextId: null, rate: 35, info: '卵抜きで5秒間金75倍/雷霆蒼破', isEvolved: true },
            { id: 'tuyoban', name: 'ギガントバーン', rank: 'OG', emoji: '🏐', score: 7500, coin: 3700, nextId: null, rate: 50, info: '強すぎる', isEvolved: true },
            { id: 'vector_kinguini', name: 'ベクター・キンギーニ', rank: 'OG', emoji: '🌌', score: 8500, coin: 4200, nextId: null, rate: 40, info: '全技から卵排除/月牙天衝', isEvolved: true },
            { id: 'ket_og', name: '不滅のケチャップ', rank: 'OG', emoji: '🍅', score: 9000, coin: 4500, nextId: null, mult: 1000, cost: 0, rate: 25 },

            // X
            { id: 'fusion_zen_kazu', name: 'アルティメット・カズキング・ZEN', rank: 'X', emoji: '☯️', score: 5000, coin: 2500, nextId: null, rate: 20, info: '合体：金20倍/卵抜き', isEvolved: true },
            { id: 'c_rampage_venom', name: 'ランペイジイット(ヴェノム)', rank: 'UZ', emoji: '☣️', score: 4000, coin: 2000, nextId: null, rate: 15, info: '進化：金15倍', isEvolved: true },
            { id: 'ket_x', name: '究極ケチャップ', rank: 'X', emoji: '⑨', score: 3000, coin: 1500, nextId: null, mult: 100, cost: 0, rate: 30 },
            { id: 'sei_cook', name: 'キングオブクッキングせいまろ', rank: 'X', emoji: '🍳', score: 3000, coin: 1500, nextId: null, mult: 100, cost: 0, rate: 30 },
            { id: 'sei_dragon', name: '紅蓮犬・せいまろ', rank: 'X', emoji: '🐉', score: 3000, coin: 1500, nextId: null, mult: 100, cost: 0, rate: 25 },
            { id: 'ket_royal_k', name: 'アルティメット・カズケチャ', rank: 'X', emoji: '🎮', score: 3000, coin: 1500, nextId: null, mult: 100, cost: 0, rate: 28 },
            { id: 'zen_omelette', name: '緋色のオムレツZEN', rank: 'X', emoji: '🥚', score: 2800, coin: 1400, nextId: null, rate: 23, info: '卵抜きで倍率半減' },
            { id: 'ham_pork', name: '緋色のロイヤル・ポーク', rank: 'X', emoji: '🐷', score: 2800, coin: 1400, nextId: null, rate: 25, info: '卵で5秒間倍率UP' },
            { id: 'ven_kazu', name: 'ランペイジカズキング（ヴェノム）', rank: 'X', emoji: '👑', score: 3000, coin: 1500, nextId: null, mult: 100, cost: 0, rate: 20 },
            { id: 'ven_seimaro', name: 'ランペイジせいまろ', rank: 'X', emoji: '☣️', score: 3000, coin: 1500, nextId: null, mult: 100, cost: 0, rate: 22 },
            { id: 'ven_ket', name: 'ブラッドケチャップ（ヴェノム）', rank: 'X', emoji: '🩸', score: 3000, coin: 1500, nextId: null, mult: 100, cost: 0, rate: 20 },

            // UZ
            { id: 'c_scarlet_ham_uz', name: '緋色の金権覇王：ハム', rank: 'UZ', emoji: '🐹', score: 3500, coin: 1750, nextId: null, rate: 15, isEvolved: true },
            { id: 'ev_kazu_ultimate', name: 'ゴッドネスカズキング', rank: 'UZ', emoji: '🌌', score: 3500, coin: 1750, nextId: null, rate: 15, isEvolved: true },
            { id: 'huhto2', name: 'BIGhuh翔', rank: 'UZ', emoji: '🐈', score: 3500, coin: 1750, nextId: null, rate: 15, isEvolved: true },
            { id: 'taka1', name: 'デカタカキング', rank: 'UZ', emoji: '㌀', score: 3500, coin: 1750, nextId: null, rate: 15 },
            { id: 'c1_ultimate', name: 'マキシマムZEN', rank: 'UZ', emoji: '👹', score: 3500, coin: 1750, nextId: null, rate: 15, isEvolved: true },
            { id: 'c13_ultimate', name: '大泥棒ぜん', rank: 'UZ', emoji: '💰', score: 3000, coin: 1500, nextId: null, rate: 10, isEvolved: true },
            { id: 'c7_ultimate', name: '精霊ぜん', rank: 'UZ', emoji: '👼', score: 3000, coin: 1500, nextId: null, rate: 10, isEvolved: true },
            { id: 'c2_ultimate', name: '億万長者ぜん', rank: 'UZ', emoji: '🏢', score: 3000, coin: 1500, nextId: null, rate: 10, isEvolved: true },
            { id: 'c_seimaro_uz', name: 'ゴッドネスせいまろ', rank: 'UZ', emoji: '⚡🐕', score: 3500, coin: 1750, nextId: null, rate: 15, isEvolved: true },
            { id: 'ket_uz', name: '伝説のケチャップ', rank: 'UZ', emoji: '✨', score: 3500, coin: 1750, nextId: null, rate: 15 },

            // SSR
            { id: 'c_scarlet_ham', name: '緋色・ハム', rank: 'SSR', emoji: '🐹', score: 1500, coin: 750, nextId: null, rate: 10 },
            { id: 'ev_kazu', name: 'カズキングタイジュウフエターネキンギーニ', rank: 'SSR', emoji: '👑', score: 1500, coin: 750, nextId: null, rate: 10 },
            { id: 'hahto', name: 'huh翔', rank: 'SSR', emoji: '🐈', score: 1500, coin: 750, nextId: null, rate: 10 },
            { id: 'c_seimaro', name: 'せいまろ', rank: 'SSR', emoji: '🐕‍🦺', score: 1500, coin: 750, nextId: null, rate: 10 },
            { id: 'c1', name: '覚醒ぜん', rank: 'SSR', emoji: '🔥', score: 1500, coin: 750, nextId: null, rate: 10 },
            { id: 'taka', name: 'タカキング', rank: 'SSR', emoji: '㌀', score: 1500, coin: 750, nextId: null, rate: 10 },
            { id: 'c13', name: '泥棒ぜん', rank: 'SSR', emoji: '👤', score: 1200, coin: 600, nextId: null, rate: 5 },
            { id: 'c7', name: '幽霊ぜん', rank: 'SSR', emoji: '👻', score: 1200, coin: 600, nextId: null, rate: 5 },
            { id: 'c2', name: '大富豪ぜん', rank: 'SSR', emoji: '💎', score: 1200, coin: 600, nextId: null, rate: 5 },
            { id: 'c_dragon', name: '龍神ぜん', rank: 'SSR', emoji: '🐉', score: 1200, coin: 600, nextId: null, rate: 5 },
            { id: 'c_mech', name: '機甲ぜんTYPE-Z', rank: 'SSR', emoji: '🤖', score: 1200, coin: 600, nextId: null, rate: 5 },
            { id: 'c_dark', name: 'ダークネスぜん', rank: 'SSR', emoji: '🌑', score: 1200, coin: 600, nextId: null, rate: 5 },
            { id: 'c_rampage', name: 'ランペイジイット', rank: 'SSR', emoji: '🦖', score: 1500, coin: 750, nextId: null, rate: 10 },
            { id: 'ket_ssr', name: '覚醒ケチャップ', rank: 'SSR', emoji: '🔥', score: 1500, coin: 750, nextId: null, mult: 15, cost: 0, rate: 10 },

            // SR (動物・フルーツ・職業シリーズなど)
            { id: 'ze1', name: 'ウシぜん', rank: 'SR', emoji: '🐄', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze2', name: 'イノシシイット', rank: 'SR', emoji: '🐗', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze3', name: 'ひつじハム', rank: 'SR', emoji: '🐏', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze4', name: 'ヤギカズ', rank: 'SR', emoji: '🐐', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze5', name: 'ラクダぜん', rank: 'SR', emoji: '🐫', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze6_1', name: 'キリンカズ', rank: 'SR', emoji: '🦒', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze6_2', name: 'カバぜん', rank: 'SR', emoji: '🦛', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze7', name: 'ビーバーぜん', rank: 'SR', emoji: '🦫', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze8', name: 'ハリネズミぜん', rank: 'SR', emoji: '🦔', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze9', name: 'クマぜん', rank: 'SR', emoji: '🐻', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze10', name: 'シロクマぜん', rank: 'SR', emoji: '🐻‍❄️', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze11', name: 'パンダぜん', rank: 'SR', emoji: '🐼', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze12', name: 'カニぜん', rank: 'SR', emoji: '🦀', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze13', name: 'カエルぜん', rank: 'SR', emoji: '🐸', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze14', name: 'ぶどうカズ', rank: 'SR', emoji: '🍇', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze15', name: 'スイカせいまろ', rank: 'SR', emoji: '🍉', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze16', name: 'ココナッツイット', rank: 'SR', emoji: '🥥', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze17', name: 'キウイhuh翔', rank: 'SR', emoji: '🥝', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze18', name: 'ブルーベリーぜん', rank: 'SR', emoji: '🫐', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze19', name: 'チェリーぜん', rank: 'SR', emoji: '🍒', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze20', name: 'ピーチハム', rank: 'SR', emoji: '🍑', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze21', name: 'りんごぜん', rank: 'SR', emoji: '🍎', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze22', name: 'マンゴーぜん', rank: 'SR', emoji: '🥭', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze23', name: 'バナナカズ', rank: 'SR', emoji: '🍌', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze24', name: 'みかんぜん', rank: 'SR', emoji: '🍊', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze25', name: 'ペッパーせいまろ', rank: 'SR', emoji: '🌶️', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze26', name: 'キノコカズ', rank: 'SR', emoji: '🍄', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze27', name: 'チョコせいまろ', rank: 'SR', emoji: '🍫', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze28', name: 'ケーキせいまろ', rank: 'SR', emoji: '🍰', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze29', name: 'クッキーせいまろ', rank: 'SR', emoji: '🍪', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze30', name: 'ドーナツせいまろ', rank: 'SR', emoji: '🍩', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze31', name: 'アイスぜん', rank: 'SR', emoji: '🍦', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze32', name: 'アメカズ', rank: 'SR', emoji: '🍬', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze33', name: 'パズルぜん', rank: 'SR', emoji: '🧩', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze34', name: 'スペードイット', rank: 'SR', emoji: '♠', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze35', name: 'ハートぜん', rank: 'SR', emoji: '♥', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze36', name: 'ダイヤせいまろ', rank: 'SR', emoji: '♦', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze37', name: 'クラブカズ', rank: 'SR', emoji: '♣', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze38', name: 'ウッドぜん', rank: 'SR', emoji: '🪵', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze39', name: 'ストーンイット', rank: 'SR', emoji: '🪨', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze40', name: 'ポリスぜん', rank: 'SR', emoji: '🚔', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze41', name: 'ファイヤーぜん', rank: 'SR', emoji: '🚒', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze42', name: '救急ぜん', rank: 'SR', emoji: '🚑', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze43', name: '爆走ぜん', rank: 'SR', emoji: '🏍️', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze44', name: 'ローラーぜん', rank: 'SR', emoji: '🛼', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze45', name: 'チャリぜん', rank: 'SR', emoji: '🚲', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze46', name: 'サングラスぜん', rank: 'SR', emoji: '🕶️', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze47', name: 'メガネカズ', rank: 'SR', emoji: '👓', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze48', name: '歌い手ぜん', rank: 'SR', emoji: '🎤', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze49', name: 'ハッカーぜん', rank: 'SR', emoji: '⌨', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'ze50', name: 'バッドイット', rank: 'SR', emoji: '🗑️', score: 100, coin: 50, nextId: null, rate: 0.1 },
            { id: 'c9', name: '黄金ぜん', rank: 'SR', emoji: '✨', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'c3', name: '社長ぜん', rank: 'SR', emoji: '💼', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'c4', name: '地主ぜん', rank: 'SR', emoji: '🏰', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'c5', name: '投資家ぜん', rank: 'SR', emoji: '📈', score: 500, coin: 250, nextId: null, rate: 3 },
            { id: 'sr_shodo', name: '書道家ぜん', rank: 'SR', emoji: '🖌️', score: 500, coin: 250, nextId: null, rate: 3.0 },
            { id: 'sr_adv', name: '冒険家ぜん', rank: 'SR', emoji: '🤠', score: 500, coin: 250, nextId: null, rate: 3.0 },
            { id: 'sr_guitar', name: 'ギタリストぜん', rank: 'SR', emoji: '🎸', score: 500, coin: 250, nextId: null, rate: 3.0 },
            { id: 'ket_sr_normal', name: '普通のケチャップ', rank: 'SR', emoji: '👤', score: 500, coin: 250, nextId: null, mult: 5, cost: 0, rate: 3 },

            // R
            { id: 'c6', name: '商人ぜん', rank: 'R', emoji: '⚖️', score: 200, coin: 100, nextId: null, rate: 1.5 },
            { id: 'c8', name: '侍ぜん', rank: 'R', emoji: '⚔️', score: 200, coin: 100, nextId: null, rate: 1.5 },
            { id: 'r_farm', name: '農家ぜん', rank: 'R', emoji: '👨‍🌾', score: 200, coin: 100, nextId: null, rate: 1.5 },
            { id: 'r_cheer', name: '応援団ぜん', rank: 'R', emoji: '📣', score: 200, coin: 100, nextId: null, rate: 1.5 },
            { id: 'r_book', name: '読書家ぜん', rank: 'R', emoji: '📖', score: 200, coin: 100, nextId: null, rate: 1.5 },
            { id: 'c14', name: '料理人ぜん', rank: 'R', emoji: '🍳', score: 200, coin: 100, nextId: null, rate: 1.5 },

            // BASIC
            { id: 'ket_r', name: 'ただのケチャップ', rank: 'BASIC', emoji: '👤', score: 100, coin: 50, nextId: null, mult: 1.0, cost: 500, rate: 1 },
            { id: 'c20', name: '普通のぜん', rank: 'BASIC', emoji: '👤', score: 50, coin: 25, nextId: null, owned: true, rate: 1 },
            // ご提示いただいた乗組員データを指定された形式に変換したリストです
            { id: 'crew_none', name: 'ひとりぼっち', rank: 'N', emoji: '🧑‍🦲', score: 10, coin: 5, effect: 'none', skillName: 'がむしゃらアタック', desc: '乗組員なし。\n【SP】画面の敵を一掃！' },
            { id: 'crew_tom', name: 'トム', rank: 'R', emoji: '🧑‍✈️', score: 200, coin: 100, effect: 'speedUp', skillName: 'トムズ・ダッシュ', desc: 'スピードが少しアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_tom', name: 'トム', rank: 'R', emoji: '🧑‍✈️', score: 200, coin: 100, effect: 'speedUp', skillName: 'トムズ・ダッシュ', desc: 'スピードが少しアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_alex', name: 'アレックス', rank: 'R', emoji: '🧑‍🔧', score: 200, coin: 100, effect: 'scoreUp', skillName: 'アレックスサーチ', desc: 'スコアが少しアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_steve', name: 'スティーブ', rank: 'R', emoji: '🧑‍🌾', score: 200, coin: 100, effect: 'powerUp', skillName: 'スティーブパンチ', desc: 'パワーが少しアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_bob', name: 'ボブ', rank: 'R', emoji: '🧑‍🍳', score: 200, coin: 100, effect: 'speedUp', skillName: 'ボブズウェーブ', desc: 'スピードが少しアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_mark', name: 'マーク', rank: 'R', emoji: '🧑‍🎨', score: 200, coin: 100, effect: 'scoreUp', skillName: 'マークトレジャー', desc: 'スコアが少しアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_speed', name: 'ダッシュマン', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: '音速ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_john', name: 'ジョン', rank: 'SR', emoji: '👮', score: 500, coin: 250, effect: 'powerUp', skillName: 'ジョンズキヤノン', desc: 'パワーが中アップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_jack', name: 'ジャック', rank: 'SR', emoji: '🕵️', score: 500, coin: 250, effect: 'speedUp', skillName: 'ジャックストーム', desc: 'スピードが中アップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_leon', name: 'レオン', rank: 'SR', emoji: '💂', score: 500, coin: 250, effect: 'scoreUp', skillName: 'レオンハンター', desc: 'スコアが中アップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_luke', name: 'ルーク', rank: 'SR', emoji: '🤵', score: 500, coin: 250, effect: 'doubleShot', skillName: 'ルークツイン', desc: 'たまに弾が2列に！？\n【SP】画面の敵を一掃！' },
    { id: 'crew_bill', name: 'ビル', rank: 'SR', emoji: '👲', score: 500, coin: 250, effect: 'powerUp', skillName: 'ビルズアーマー', desc: '弾サイズ中アップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_score', name: 'ナビゲーター', rank: 'SR', emoji: '🗺️', score: 500, coin: 250, effect: 'scoreUp', skillName: 'お宝ディスカバリー', desc: '敵を追い払ったポイント2倍！\n【SP】画面の敵を一掃！' },
    { id: 'crew_1', name: 'セナ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_2', name: 'リク', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: '小さい林', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_3', name: 'ソラ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: '燃えろー', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_4', name: 'リト', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_5', name: 'ソウタ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_6', name: 'ハル', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: '桜ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_7', name: 'ナツ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: '流星ストライク', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_8', name: 'アキ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: '落ち葉ストリーム', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_9', name: 'フユ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: '雪風吹', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_10', name: 'サクナ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'メイドアタック', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_11', name: 'アオイ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: '蝶の舞', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_12', name: 'アカイ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'スナイパー', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_13', name: 'アサヒ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: '怪盗ブースト', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_14', name: 'ユウリ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'ドライフラワー', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_15', name: 'レン', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_16', name: 'レオ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'ドッグストライク', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_17', name: 'アキラ', rank: 'R', emoji: '👹', score: 200, coin: 100, effect: 'speedUp', skillName: '黒鬼の祭り', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_18', name: 'ライ', rank: 'R', emoji: '👹', score: 200, coin: 100, effect: 'speedUp', skillName: '黒鬼の祭り', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_19', name: 'ノイ', rank: 'R', emoji: '👹', score: 200, coin: 100, effect: 'speedUp', skillName: '黒鬼の祭り', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_20', name: 'フミオ', rank: 'R', emoji: '👓', score: 200, coin: 100, effect: 'speedUp', skillName: '増税ビーム', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_21', name: 'シンイチ', rank: 'R', emoji: '⚽', score: 200, coin: 100, effect: 'speedUp', skillName: 'バァロー', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_22', name: 'ミッキ', rank: 'R', emoji: '🐭', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハハハハハハハハ', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_23', name: 'アカリ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_24', name: 'ユカリ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_25', name: 'クオン', rank: 'R', emoji: '🔫', score: 200, coin: 100, effect: 'speedUp', skillName: '結婚しようよぉぉぉぉぉぉ!!!!!', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_26', name: 'タクミ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_27', name: 'タクマ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'すべての辻褄があいます！', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_28', name: 'フブキ', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'コンコンキーツネ！', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_29', name: 'いちごラビット', rank: 'R', emoji: '🏃', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_30', name: 'パインラビット', rank: 'R', emoji: '🍍', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_31', name: 'メロンラビット', rank: 'R', emoji: '🍈', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_32', name: 'キウイラビット', rank: 'R', emoji: '🥝', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_33', name: 'みかんラビット', rank: 'R', emoji: '🍊', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_34', name: 'ぶどうラビット', rank: 'R', emoji: '🍇', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_35', name: 'スイカラビット', rank: 'R', emoji: '🍉', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_36', name: 'レモンラビット', rank: 'R', emoji: '🍋', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_37', name: 'ライムラビット', rank: 'R', emoji: '🍋‍🟩', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_38', name: 'バナナラビット', rank: 'R', emoji: '🍌', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_39', name: 'マンゴーラビット', rank: 'R', emoji: '🥭', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_40', name: 'リンゴラビット', rank: 'R', emoji: '🍎', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_41', name: 'ナシラビット', rank: 'R', emoji: '🍐', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_42', name: 'ももラビット', rank: 'R', emoji: '🍑', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_43', name: 'チェリーラビット', rank: 'R', emoji: '🍒', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_44', name: 'ブルーベリーラビット', rank: 'R', emoji: '🫐', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_45', name: 'ココナッツラビット', rank: 'R', emoji: '🥥', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_46', name: 'ピーマンラビット', rank: 'R', emoji: '🫑', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_47', name: 'アボカドラビット', rank: 'R', emoji: '🥑', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_48', name: 'ナスラビット', rank: 'R', emoji: '🍆', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_49', name: 'ポテトラビット', rank: 'R', emoji: '🥔', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_50', name: 'ニンジンラビット', rank: 'R', emoji: '🥕', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_51', name: 'コーンラビット', rank: 'R', emoji: '🌽', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_52', name: 'ペッパーラビット', rank: 'R', emoji: '🌶️', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_53', name: 'キュウリラビット', rank: 'R', emoji: '🥒', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_54', name: 'ブロッコリーラビット', rank: 'R', emoji: '🥦', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_55', name: 'たまねぎラビット', rank: 'R', emoji: '🧅', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_56', name: 'まめラビット', rank: 'R', emoji: '🫘', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_57', name: 'ピーナッツラビット', rank: 'R', emoji: '🥜', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_58', name: 'くりラビット', rank: 'R', emoji: '🌰', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_59', name: 'キノコラビット', rank: 'R', emoji: '🍄‍', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_60', name: 'タカシ', rank: 'SR', emoji: '🏃', score: 500, coin: 250, effect: 'speedUp', skillName: 'タカシブレイク', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_61', name: 'グリス', rank: 'SR', emoji: '🏃', score: 500, coin: 250, effect: 'speedUp', skillName: '進化を燃やしてぶっ潰す', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_62', name: 'ローグ', rank: 'SR', emoji: '🏃', score: 500, coin: 250, effect: 'speedUp', skillName: '大義のための犠牲となれ', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_63', name: 'シンジ', rank: 'SR', emoji: '🏃', score: 500, coin: 250, effect: 'speedUp', skillName: 'オンドゥル', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_64', name: 'ショウヘイ', rank: 'SR', emoji: '🏃', score: 500, coin: 250, effect: 'speedUp', skillName: 'オオタニサーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_65', name: 'ボーンラビット', rank: 'R', emoji: '💀', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_66', name: 'デビルラビット', rank: 'R', emoji: '😈', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_67', name: 'ピエロラビット', rank: 'R', emoji: '🤡', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_68', name: 'オニラビット', rank: 'R', emoji: '👹', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_69', name: 'テングラビット', rank: 'R', emoji: '👺', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_70', name: 'ゴーストラビット', rank: 'R', emoji: '👻', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_71', name: 'エイリアンラビット', rank: 'R', emoji: '👽', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_72', name: 'ロボラビット', rank: 'R', emoji: '🤖', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_73', name: 'ルビーラビット', rank: 'R', emoji: '💎', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_74', name: 'エメラルドラビット', rank: 'R', emoji: '💎', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_75', name: 'サファイアラビット', rank: 'R', emoji: '💎', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_76', name: 'ゴールドラビット', rank: 'R', emoji: '💎', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_77', name: 'パールラビット', rank: 'R', emoji: '💎', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_78', name: 'ダイヤラビット', rank: 'R', emoji: '💎', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_79', name: 'アメジストラビット', rank: 'R', emoji: '💎', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_80', name: 'モアイラビット', rank: 'R', emoji: '🗿', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_81', name: 'アイスラビット', rank: 'R', emoji: '🍦', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_82', name: 'ベビーラビット', rank: 'R', emoji: '🍼', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_83', name: 'ダンゴラビット', rank: 'R', emoji: '🍡', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_84', name: 'ナルトラビット', rank: 'R', emoji: '🍥', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_85', name: 'オデンラビット', rank: 'R', emoji: '🍢', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_86', name: 'カレーラビット', rank: 'R', emoji: '🍛', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_87', name: 'タコスラビット', rank: 'R', emoji: '🌮', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_88', name: 'チーズラビット', rank: 'R', emoji: '🧀', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_89', name: 'エンジェルラビット', rank: 'R', emoji: '👼', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_90', name: 'ゾンビラビット', rank: 'R', emoji: '🧟', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_91', name: 'ヴァンパイアラビット', rank: 'R', emoji: '🧛', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_92', name: 'ドクターラビット', rank: 'R', emoji: '🧑‍⚕️', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_93', name: 'ティーチャーラビット', rank: 'R', emoji: '🧑‍🏫', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_94', name: 'ジャッジマン', rank: 'R', emoji: '🧑‍⚖️', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_95', name: 'ポリスラビット', rank: 'R', emoji: '👮', score: 200, coin: 100, effect: 'speedUp', skillName: 'ハリケーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_96', name: 'ソウ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_97', name: 'レン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_98', name: 'ヒロ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_99', name: 'カオキ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_100', name: 'ユウキ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_101', name: 'タクミ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_102', name: 'コウキ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_103', name: 'ダイキ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_104', name: 'ナオ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_105', name: 'ケント', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_106', name: 'カズマサ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_107', name: 'ショウ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_108', name: 'リョウ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_109', name: 'トモヤ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_110', name: 'ハヤト', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_111', name: 'ユウタ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_112', name: 'ケンタ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_113', name: 'タツヤ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_114', name: 'ソウタ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_115', name: 'リク', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_116', name: 'カズマ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_117', name: 'シュン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_118', name: 'カイジ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_119', name: 'コウヘイ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_120', name: 'ダイスケ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_121', name: 'シン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_122', name: 'スバル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_123', name: 'ケンンジ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_124', name: 'ナオキ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_125', name: 'マサト', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_126', name: 'カナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_127', name: 'マユ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_128', name: 'アスカ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_129', name: 'リナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_130', name: 'アオイ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_131', name: 'ミサキ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_132', name: 'ユイ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_133', name: 'ヒナタ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_134', name: 'ナナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_135', name: 'マイ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_136', name: 'サクラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_137', name: 'ハルカ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_138', name: 'リコ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_139', name: 'ユウカ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_140', name: 'ミユ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_141', name: 'トモカ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_142', name: 'シミ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_143', name: 'エリ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_144', name: 'カリン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_145', name: 'サヤ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_146', name: 'レイ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_147', name: 'ジン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_148', name: 'ソウマ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_149', name: 'イッキ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_150', name: 'セナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_151', name: 'ルカ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_152', name: 'イブキ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_153', name: 'タイガ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_154', name: 'カナメ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_155', name: 'ユウイ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_156', name: 'ミナト', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_157', name: 'アユム', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_158', name: 'カナデ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_159', name: 'シオン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_160', name: 'レオ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_161', name: 'ソラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_162', name: 'イオリ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_163', name: 'ナルミ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_164', name: 'タスク', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_165', name: 'テル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_166', name: 'ツバサ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_167', name: 'カズマサ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_168', name: 'ヨシキ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_169', name: 'ノブ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_170', name: 'テツヤ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_171', name: 'セイヤ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_172', name: 'リョウタ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_173', name: 'コウジ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_174', name: 'ヒロキ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_175', name: 'タカシ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_176', name: 'チハヤ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_177', name: 'サトシ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: '10万ボルト', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_178', name: 'マナブ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_179', name: 'アツシ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_180', name: 'ヤス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_181', name: 'コウタ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_182', name: 'ユウジ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_183', name: 'ノリ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_184', name: 'イサム', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_185', name: 'タカヒロ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_186', name: 'カツミ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_187', name: 'ススム', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_188', name: 'アキラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_189', name: 'モトイ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_190', name: 'トモキ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_191', name: 'キンジ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_192', name: 'ルーカス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_193', name: 'オリバー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_194', name: 'リアム', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_195', name: 'ノア', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_196', name: 'イーサン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_197', name: 'メイソン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_198', name: 'ローガン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_199', name: 'レオ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_200', name: 'ジョン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_201', name: 'ポール', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_202', name: 'マーク', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_203', name: 'デヴィッド', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_204', name: 'トム', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_205', name: 'サム', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_206', name: 'ベン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_207', name: 'ダン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_208', name: 'ニック', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_209', name: 'ジャック', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_210', name: 'エリック', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_211', name: 'アダム', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_212', name: 'カール', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_213', name: 'オスカー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_214', name: 'フェリックス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_215', name: 'マックス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_216', name: 'ルイス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_217', name: 'ヘンリー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_218', name: 'アーサー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_219', name: 'チャールズ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_220', name: 'ジョージ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'おさるの', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_221', name: 'エドワード', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_222', name: 'アルフレッド', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_223', name: 'フィリップ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'サイクロン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_224', name: 'リチャード', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_225', name: 'ロバート', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_226', name: 'ウィリアム', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_227', name: 'ジェームズ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_228', name: 'マイケル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_229', name: 'ダニエル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_230', name: 'マシュー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_231', name: 'アンドリュー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_232', name: 'ジョセフ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_233', name: 'クリス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_234', name: 'ライアン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_235', name: 'ケーシー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_236', name: 'エマ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_237', name: 'オリヴィア', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_238', name: 'ソフィア', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_239', name: 'アヴァ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_240', name: 'イザベラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_241', name: 'ミア', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_242', name: 'シャーロット', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_243', name: 'アリア', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_244', name: 'エラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_245', name: 'クロエ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'バックバックバクーン', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_246', name: 'レイチェル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_247', name: 'サラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_248', name: 'ローラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_249', name: 'ニコル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_250', name: 'ハンナ', rank: 'SR', emoji: '👤', score: 500, coin: 250, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_251', name: 'リリー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_252', name: 'アリス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_253', name: 'ルーシー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_254', name: 'ジュリア', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_255', name: 'エミリー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_256', name: 'ゾーイ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_257', name: 'ステラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_258', name: 'ルナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_259', name: 'オーロラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_260', name: 'イヴ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_261', name: 'マヤ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_262', name: 'ノラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_263', name: 'クララ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_264', name: 'エレーナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_265', name: 'アンナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_266', name: 'ローザ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_267', name: 'モニカ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_268', name: 'ディアナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_269', name: 'ニーナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_270', name: 'ラウラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_271', name: 'マルコ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_272', name: 'マテオ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_273', name: 'ディエゴ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_274', name: 'ルカ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_275', name: 'レオン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_276', name: 'アントニオ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_277', name: 'マリオ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_278', name: 'カルロス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_279', name: 'フアン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_280', name: 'ホセ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_281', name: 'アンドレス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_282', name: 'セバスチャン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_283', name: 'ガブリエル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_284', name: 'ジュリアン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_285', name: 'アドリアン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_286', name: 'ゼノン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_287', name: 'ルーナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_288', name: 'カイン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_289', name: 'シルフィ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_290', name: 'アルディス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_291', name: 'エレノア', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_292', name: 'ガルム', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_293', name: 'ヴァルツ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_294', name: 'イシュタル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_295', name: 'ジルベール', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_296', name: 'セレス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_297', name: 'ディアブロ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_298', name: 'ベルゼ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_299', name: 'フェンリル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_300', name: 'アルテミス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_301', name: 'ノワール', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_302', name: 'ブレイズ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_303', name: 'クラリス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_304', name: 'フォルテ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_305', name: 'ソル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_306', name: 'ルシアン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_307', name: 'ヴェイン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_308', name: 'リリス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_309', name: 'アスラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_310', name: 'ヴェルダン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_311', name: 'ギルガ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_312', name: 'オディール', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_313', name: 'シルフ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_314', name: 'ウンディーネ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_315', name: 'イフリート', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_316', name: 'ノーム', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_317', name: 'ヴァルキリー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_318', name: 'オルフェ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_319', name: 'エルディン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_320', name: 'セフィラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_321', name: 'アストレイ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_322', name: 'レヴィア', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_323', name: 'バハムト', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_324', name: 'ルシフェ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_325', name: 'アザゼル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_326', name: 'ベルフェ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_327', name: 'マモン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_328', name: 'アスモデ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_329', name: 'ベヒモス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_330', name: 'グリフォン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_331', name: 'ペガサス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_332', name: 'キマイラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_333', name: 'セルケト', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_334', name: 'アヌビス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_335', name: 'ホルス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_336', name: 'オシリス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_337', name: 'イシス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_338', name: 'ラー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_339', name: 'トト', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_340', name: 'バステト', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_341', name: 'ソベク', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_342', name: 'セクメト', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_343', name: 'アペプ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_344', name: 'ハトホル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_345', name: 'メジェド', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_346', name: 'オーディン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_347', name: 'トール', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_348', name: 'ロキ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_349', name: 'フレイヤ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_350', name: 'ヘイムダル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_351', name: 'バルドル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_352', name: 'チール', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_353', name: 'ヘル', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_354', name: 'ジーク', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_355', name: 'ブリュン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_356', name: 'クーフー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_357', name: 'スカサハ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_358', name: 'メドゥーサ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_359', name: 'アタランテ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_360', name: 'ヘラクレス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_361', name: 'アキレウス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_362', name: 'オデュッセ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_363', name: 'ペルセウス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_364', name: 'テセウス', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_365', name: 'イアソン', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_366', name: 'メデイア', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_367', name: 'カルナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_368', name: 'アルジュナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_369', name: 'ラーマ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_370', name: 'ラヴァナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_371', name: 'インドラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_372', name: 'ヴリトラ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_373', name: 'ヴァルナ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_374', name: 'アグニ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_375', name: 'スーリヤ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_376', name: 'ヤマ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_377', name: 'ガネーシャ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_378', name: 'シヴァ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
    { id: 'crew_379', name: 'ヴィシュヌ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
            { id: 'crew_379', name: 'ヴィシュヌ', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' },
            { id: 'crew_380', name: 'ブラフマー', rank: 'R', emoji: '👤', score: 200, coin: 100, effect: 'speedUp', skillName: 'すごい', desc: 'スピードアップ！\n【SP】画面の敵を一掃！' }
        ];

        let state = {
            screen: 'title',
            score: 0,
            grassCoins: 300,
            fossils: 3,
            cores: 10, // 初期所有草核
            level: 1,
            pulledCount: 0,
            timeLeft: 60,
            skillGauge: 0,
            isPlaying: false,
            stats: { normal: 0, gold: 0, rainbow: 0 }
        };

        let collection = { 'c20': { count: 1, level: 1 } };
        
        const MISSIONS = {
            daily: [
                { id: 'd1', title: '草を合計20本抜く', current: 0, target: 20, reward: 5, claimed: false },
                { id: 'd2', title: 'ガチャを1回引く', current: 0, target: 1, reward: 10, claimed: false }
            ],
            normal: [
                { id: 'n1', title: '農園Lvを2にする', current: 1, target: 2, reward: 15, claimed: false },
                { id: 'n2', title: '必殺技を1回発動', current: 0, target: 1, reward: 20, claimed: false }
            ],
            event: [
                { id: 'e1', title: '虹の草を1本抜く', current: 0, target: 1, reward: 50, claimed: false }
            ]
        };

        // --- DOM Elements ---
        const ui = {
            screenTitle: document.getElementById('screen-title'),
            screenHome: document.getElementById('screen-home'),
            screenGame: document.getElementById('screen-game'),
            homeCoins: document.getElementById('home-coins'),
            homeFossils: document.getElementById('home-fossils'),
            homeCores: document.getElementById('home-cores'),
            homeFarmLevel: document.getElementById('home-farm-level'),
            gameScore: document.getElementById('game-score'),
            gameTimer: document.getElementById('game-timer'),
            skillBar: document.getElementById('skill-bar'),
            skillBtn: document.getElementById('skill-btn'),
            skillText: document.getElementById('skill-text'),
            retireBtn: document.getElementById('retire-btn'),
            canvas: document.getElementById('game-canvas'),
            appContainer: document.getElementById('app-container'),
            gameOverlay: document.getElementById('game-overlay'),
            overlayTitle: document.getElementById('overlay-title'),
            overlayDesc: document.getElementById('overlay-desc'),
            overlayActionBtn: document.getElementById('overlay-action-btn'),
            modal: document.getElementById('modal'),
            modalContainer: document.getElementById('modal-container'),
            modalTitle: document.getElementById('modal-title'),
            modalContent: document.getElementById('modal-content'),
            comboDisplay: document.getElementById('combo-display'),
            comboCount: document.getElementById('combo-count'),
            bgFireflies: document.getElementById('bg-fireflies')
        };

        // ==================== 蛍(背景装飾)生成 ====================
        (function spawnFireflies() {
            if (!ui.bgFireflies) return;
            for (let i = 0; i < 12; i++) {
                const f = document.createElement('div');
                const size = Math.random() * 3 + 2;
                f.className = 'firefly';
                f.style.width = `${size}px`; f.style.height = `${size}px`;
                f.style.left = `${Math.random() * 100}%`;
                f.style.top = `${40 + Math.random() * 55}%`;
                f.style.setProperty('--fx', `${(Math.random()-0.5)*80}px`);
                f.style.setProperty('--fy', `${-60 - Math.random()*60}px`);
                f.style.setProperty('--fx2', `${(Math.random()-0.5)*100}px`);
                f.style.setProperty('--fy2', `${-140 - Math.random()*80}px`);
                f.style.animationDuration = `${6 + Math.random() * 6}s`;
                f.style.animationDelay = `${Math.random() * 8}s`;
                ui.bgFireflies.appendChild(f);
            }
        })();

        // ==================== 軽量サウンドエンジン (Web Audio 合成) ====================
        let audioCtx = null;
        function getAudioCtx() {
            if (!audioCtx) {
                try { audioCtx = new (window.AudioContext || window.webkitAudioContext)(); } catch(e) { return null; }
            }
            if (audioCtx.state === 'suspended') audioCtx.resume();
            return audioCtx;
        }
        window.playSE = function(type) {
            const ac = getAudioCtx();
            if (!ac) return;
            const now = ac.currentTime;
            const master = ac.createGain();
            master.connect(ac.destination);

            function tone(freq, start, dur, type='sine', vol=0.2, glideTo=null) {
                const osc = ac.createOscillator();
                const gain = ac.createGain();
                osc.type = type; osc.frequency.setValueAtTime(freq, now + start);
                if (glideTo) osc.frequency.exponentialRampToValueAtTime(glideTo, now + start + dur);
                gain.gain.setValueAtTime(vol, now + start);
                gain.gain.exponentialRampToValueAtTime(0.001, now + start + dur);
                osc.connect(gain); gain.connect(master);
                osc.start(now + start); osc.stop(now + start + dur + 0.02);
            }

            switch(type) {
                case 'tap': tone(880, 0, 0.06, 'sine', 0.12); break;
                case 'pull_normal': tone(660, 0, 0.09, 'triangle', 0.16, 900); break;
                case 'pull_gold': tone(880, 0, 0.08, 'triangle', 0.18, 1400); tone(1320, 0.04, 0.12, 'sine', 0.12, 1760); break;
                case 'pull_rainbow':
                    tone(660, 0, 0.1, 'sawtooth', 0.1, 1320);
                    tone(990, 0.05, 0.12, 'sine', 0.14, 1980);
                    tone(1320, 0.1, 0.15, 'sine', 0.12, 2200);
                    break;
                case 'combo': tone(500 + Math.random()*300, 0, 0.07, 'square', 0.08); break;
                case 'skill': 
                    tone(220, 0, 0.3, 'sawtooth', 0.2, 880);
                    tone(440, 0.05, 0.3, 'triangle', 0.18, 1100);
                    break;
                case 'levelup':
                    tone(523, 0, 0.12, 'triangle', 0.18);
                    tone(659, 0.1, 0.12, 'triangle', 0.18);
                    tone(783, 0.2, 0.2, 'triangle', 0.2);
                    break;
                case 'death':
                    tone(180, 0, 0.4, 'sawtooth', 0.25, 40);
                    break;
                case 'gacha_spin': tone(440, 0, 0.5, 'sine', 0.08, 220); break;
                case 'gacha_reveal':
                    tone(660, 0, 0.15, 'triangle', 0.2);
                    tone(880, 0.1, 0.15, 'triangle', 0.2);
                    tone(1320, 0.2, 0.25, 'sine', 0.22);
                    break;
                case 'gacha_rare':
                    tone(440, 0, 0.15, 'sawtooth', 0.15);
                    tone(660, 0.1, 0.15, 'sawtooth', 0.16);
                    tone(880, 0.2, 0.15, 'sawtooth', 0.17);
                    tone(1320, 0.3, 0.35, 'sine', 0.25);
                    break;
            }
        };

        // --- 画面切り替え ---
        window.switchScreen = function(target) {
            state.screen = target;
            ui.screenTitle.classList.add('hidden');
            ui.screenHome.classList.add('hidden');
            ui.screenGame.classList.add('hidden');
            
            if (target === 'title') {
                ui.screenTitle.classList.remove('hidden');
            } else if (target === 'home') {
                updateHomeUI();
                ui.screenHome.classList.remove('hidden');
            } else if (target === 'game') {
                ui.screenGame.classList.remove('hidden');
                startGame();
            }
        };

        function updateHomeUI() {
            ui.homeCoins.textContent = state.grassCoins;
            ui.homeFossils.textContent = state.fossils;
            ui.homeCores.textContent = state.cores;
            ui.homeFarmLevel.textContent = state.level;
        }

        // ==================== モーダルシステム ====================
        window.openModal = function(type) {
            ui.modal.classList.remove('hidden');
            setTimeout(() => {
                ui.modal.classList.remove('opacity-0');
                ui.modalContainer.classList.remove('scale-95');
                ui.modalContainer.classList.add('scale-100');
            }, 10);

            if(type === 'mission') renderMission('daily');
            if(type === 'collection') renderCollection('character');
            if(type === 'gacha') renderGacha();
            if(type === 'extreme') renderExtreme('rankup');
        };

        window.closeModal = function() {
            ui.modal.classList.add('opacity-0');
            ui.modalContainer.classList.remove('scale-100');
            ui.modalContainer.classList.add('scale-95');
            setTimeout(() => {
                ui.modal.classList.add('hidden');
            }, 200);
        };

        const activeTabClass = "flex-1 py-2 rounded-xl font-bold text-[10px] bg-emerald-600 text-white shadow-inner";
        const inactiveTabClass = "flex-1 py-2 rounded-xl font-bold text-[10px] bg-slate-800 text-slate-400 hover:bg-slate-700 transition";

        // --- ミッション ---
        window.renderMission = function(tab) {
            ui.modalTitle.textContent = "📜 ミッション";
            let html = `
                <div class="flex space-x-2 mb-4 bg-slate-950 p-1 rounded-2xl border border-slate-800">
                    <button onclick="renderMission('daily')" class="${tab==='daily'?activeTabClass:inactiveTabClass}">デイリー</button>
                    <button onclick="renderMission('normal')" class="${tab==='normal'?activeTabClass:inactiveTabClass}">ノーマル</button>
                    <button onclick="renderMission('event')" class="${tab==='event'?activeTabClass:inactiveTabClass}">イベント</button>
                </div>
                <div class="space-y-3 pb-4">
            `;
            
            MISSIONS[tab].forEach(m => {
                const canClaim = m.current >= m.target && !m.claimed;
                const progressPct = Math.min(100, (m.current / m.target) * 100);
                html += `
                    <div class="bg-slate-950/50 border border-slate-700/50 p-3 rounded-2xl flex items-center justify-between ${canClaim ? 'ring-1 ring-fuchsia-500/50 shadow-[0_0_15px_rgba(217,70,239,0.15)]' : ''}">
                        <div class="flex-1 mr-3">
                            <div class="font-bold text-xs text-white mb-1 drop-shadow">${m.title}</div>
                            <div class="h-1.5 bg-slate-800 rounded-full overflow-hidden mb-1">
                                <div class="h-full bg-gradient-to-r from-emerald-500 to-emerald-300 transition-all" style="width:${progressPct}%"></div>
                            </div>
                            <div class="text-[10px] text-slate-400 font-bold tracking-wider">進捗: <span class="${m.current>=m.target?'text-emerald-400':'text-slate-300'}">${m.current}</span> / ${m.target}</div>
                            <div class="text-[10px] text-fuchsia-300 mt-1 font-black">報酬: 🔮 ${m.reward} 草核</div>
                        </div>
                        <button onclick="claimMission('${tab}', '${m.id}')" class="btn-tactile px-4 py-2 rounded-xl text-xs font-black shadow transition ${m.claimed ? 'bg-slate-800 text-slate-600' : canClaim ? 'bg-gradient-to-b from-fuchsia-400 to-purple-600 text-white' : 'bg-slate-800 border border-slate-700 text-slate-500'} disabled:opacity-50" ${!canClaim || m.claimed ? 'disabled' : ''}>
                            ${m.claimed ? '受取済' : '受取'}
                        </button>
                    </div>
                `;
            });
            html += `</div>`;
            ui.modalContent.innerHTML = html;
        };

        window.claimMission = function(tab, id) {
            const m = MISSIONS[tab].find(x => x.id === id);
            if(m && m.current >= m.target && !m.claimed) {
                m.claimed = true;
                state.cores += m.reward;
                playSE('gacha_reveal');
                updateHomeUI();
                renderMission(tab);
                alert(`🎉 報酬「🔮 ${m.reward} 草核」を受け取りました！`);
            }
        };

        function updateMissionProgress(type, id, amount, isAbsolute = false) {
            const m = MISSIONS[type].find(x => x.id === id);
            if(m && !m.claimed) {
                if(isAbsolute) m.current = Math.max(m.current, amount);
                else m.current += amount;
                m.current = Math.min(m.current, m.target);
            }
        }

        // --- 図鑑 ---
        window.renderCollection = function(tab) {
            ui.modalTitle.textContent = "📖 図鑑";
            let html = `
                <div class="flex space-x-2 mb-4 bg-slate-950 p-1 rounded-2xl border border-slate-800">
                    <button onclick="renderCollection('character')" class="${tab==='character'?activeTabClass:inactiveTabClass}">キャラ</button>
                    <button onclick="renderCollection('bgm')" class="${tab==='bgm'?activeTabClass:inactiveTabClass}">BGM</button>
                    <button onclick="renderCollection('bg')" class="${tab==='bg'?activeTabClass:inactiveTabClass}">背景</button>
                </div>
                <div class="space-y-3 pb-4">
            `;
            
            if(tab === 'character') {
                CHARACTERS.forEach(c => {
                    const has = collection[c.id] ? collection[c.id].count : 0;
                    const rankObj = RANKS.find(r => r.name === c.rank) || { color: 'text-slate-400 bg-slate-800 border-slate-600' };
                    const glowClass = has > 0 ? getRarityGlowClass(c.rank) : '';
                    html += `
                        <div class="bg-slate-950 border border-slate-800 p-3 rounded-2xl flex items-center space-x-4 transition-all ${has > 0 ? glowClass : 'opacity-40 filter grayscale'}">
                            <div class="text-4xl drop-shadow-md flex items-center justify-center w-14">${c.emoji}</div>
                            <div class="flex-1">
                                <div class="flex items-center space-x-2 mb-1">
                                    <span class="${rankObj.color} text-[9px] px-1.5 py-0.5 rounded font-black">${c.rank}</span>
                                    <span class="font-bold text-sm text-white">${has > 0 ? c.name : '？？？'}</span>
                                </div>
                                <div class="text-[10px] text-slate-400 font-bold">所持: ${has} 体</div>
                                ${c.info ? `<div class="text-[9px] text-yellow-300 mt-1">${c.info}</div>` : ''}
                            </div>
                        </div>
                    `;
                });
            } else if(tab === 'bgm') {
                html += `<div class="text-center text-slate-500 text-xs py-10 font-bold">BGMは現在「デフォルト」のみ選択可能です。</div>`;
            } else if(tab === 'bg') {
                html += `<div class="text-center text-slate-500 text-xs py-10 font-bold">背景は現在「夜の農園」のみ選択可能です。</div>`;
            }
            html += `</div>`;
            ui.modalContent.innerHTML = html;
        };

        // --- ガチャ ---
        // レアリティ別の演出レベル定義
        const RARITY_TIER = {
            BASIC: 0, N: 0, R: 1, SR: 2, SSR: 3, UZ: 4, OG: 5, X: 6, OMEGA: 7, ADMIN: 7
        };
        function getRarityGlowClass(rank) {
            const map = { R: 'rarity-glow-r', SR: 'rarity-glow-sr', SSR: 'rarity-glow-ssr', UZ: 'rarity-glow-uz', OG: 'rarity-glow-og', X: 'rarity-glow-x', OMEGA: 'rarity-glow-omega', ADMIN: 'rarity-glow-omega' };
            return map[rank] || '';
        }
        function getRarityRayColor(rank) {
            const map = { R: '#60a5fa', SR: '#a855f7', SSR: '#facc15', UZ: '#f472b6', OG: '#34d399', X: '#d946ef', OMEGA: '#fcd34d', ADMIN: '#ef4444' };
            return map[rank] || '#a3e635';
        }

        window.renderGacha = function() {
            ui.modalTitle.textContent = "🎁 ガチャ";
            const cost = 10;
            ui.modalContent.innerHTML = `
                <div class="flex flex-col items-center py-6 space-y-8">
                    <div class="text-center">
                        <p class="text-[10px] font-bold text-slate-400 mb-1 tracking-widest">所持リソース</p>
                        <p class="text-2xl font-black text-fuchsia-400 drop-shadow">🔮 <span id="gacha-core-count">${state.cores}</span> <span class="text-xs">草核</span></p>
                    </div>
                    
                    <div class="gacha-stage relative w-64 h-64 flex items-center justify-center">
                        <div id="gacha-ray-bg" class="absolute inset-0 flex items-center justify-center opacity-0 transition-opacity duration-300 pointer-events-none">
                            <div class="ray-rotate w-full h-full" style="background: conic-gradient(from 0deg, transparent 0deg, rgba(217,70,239,0.35) 20deg, transparent 40deg, transparent 160deg, rgba(217,70,239,0.35) 180deg, transparent 200deg);"></div>
                        </div>
                        <div id="gacha-capsule" class="relative w-48 h-48 rounded-full flex flex-col items-center justify-center shadow-[inset_0_-10px_20px_rgba(0,0,0,0.4),0_0_25px_rgba(217,70,239,0.3)] overflow-hidden transition-all border-4 border-fuchsia-700/60" style="background: radial-gradient(circle at 35% 30%, #f0abfc 0%, #c026d3 45%, #581c87 100%);">
                            <div class="absolute inset-0 bg-[linear-gradient(180deg,rgba(255,255,255,0.35)_0%,transparent_45%)]"></div>
                            <span class="text-6xl drop-shadow-lg relative z-10">🥚</span>
                            <span class="text-[10px] text-fuchsia-100 font-black mt-2 tracking-widest relative z-10 bg-black/25 px-3 py-1 rounded-full">TAP TO SUMMON</span>
                        </div>
                        <div id="gacha-flash" class="absolute inset-0 rounded-full bg-white pointer-events-none opacity-0"></div>
                        <div id="gacha-sparkle-layer" class="absolute inset-0 pointer-events-none"></div>
                    </div>
                    
                    <button id="gacha-pull-btn" onclick="pullGachaAction()" class="btn-tactile w-full max-w-xs bg-gradient-to-b from-fuchsia-500 to-purple-700 text-white font-black py-4 rounded-2xl shadow-[0_0_25px_rgba(192,38,211,0.5)] transition text-lg tracking-widest flex items-center justify-center space-x-2 border border-fuchsia-300/40">
                        <span class="font-display">召喚する</span>
                        <span class="text-xs bg-black/30 px-2 py-1 rounded-lg">🔮 ${cost}</span>
                    </button>
                </div>
            `;
        };

        window.pullGachaAction = function() {
            const cost = 10;
            if(state.cores < cost) {
                alert('草核(🔮)が足りません！ミッションをクリアして集めましょう。');
                return;
            }
            const btn = document.getElementById('gacha-pull-btn');
            const capsule = document.getElementById('gacha-capsule');
            const flash = document.getElementById('gacha-flash');
            const rayBg = document.getElementById('gacha-ray-bg');
            const sparkleLayer = document.getElementById('gacha-sparkle-layer');
            if (!capsule || btn.disabled) return;
            btn.disabled = true; btn.classList.add('opacity-50');

            state.cores -= cost;
            updateHomeUI();
            const coreCountEl = document.getElementById('gacha-core-count');
            if (coreCountEl) coreCountEl.textContent = state.cores;
            updateMissionProgress('daily', 'd2', 1);
            
            // レアリティ抽選
            const rRand = Math.random() * 100;
            let selectedRank = 'BASIC';
            if (rRand < 2) selectedRank = 'SSR';
            else if (rRand < 15) selectedRank = 'SR';
            else if (rRand < 40) selectedRank = 'R';
            else selectedRank = 'BASIC';

            const pool = CHARACTERS.filter(c => c.rank === selectedRank && c.rank !== 'ADMIN' && c.rank !== 'X');
            const pulledChar = pool.length > 0 ? pool[Math.floor(Math.random() * pool.length)] : CHARACTERS[CHARACTERS.length - 1];

            if (!collection[pulledChar.id]) collection[pulledChar.id] = { count: 0, level: 1 };
            collection[pulledChar.id].count++;

            const rankObj = RANKS.find(r => r.name === pulledChar.rank) || { color: 'text-slate-400 bg-slate-800' };
            const tier = RARITY_TIER[pulledChar.rank] || 0;
            const rayColor = getRarityRayColor(pulledChar.rank);
            const glowClass = getRarityGlowClass(pulledChar.rank);

            // Step1: 予感演出(振動+SE)
            playSE('gacha_spin');
            capsule.classList.add('shake-anticipation');
            rayBg.style.opacity = '1';

            const shakeDuration = tier >= 3 ? 1100 : 650;
            setTimeout(() => {
                capsule.classList.remove('shake-anticipation');

                // Step2: 光爆発
                flash.classList.add('gacha-flash-burst');
                playSE(tier >= 3 ? 'gacha_rare' : 'gacha_reveal');
                if (tier >= 3) { ui.appContainer.classList.add('flash-white-anim'); setTimeout(()=>ui.appContainer.classList.remove('flash-white-anim'), 500); }

                // スパークル粒子をDOMで生成
                const sparkleCount = 8 + tier * 4;
                for (let i = 0; i < sparkleCount; i++) {
                    const sp = document.createElement('div');
                    sp.textContent = ['✨','⭐','💫'][Math.floor(Math.random()*3)];
                    sp.className = 'sparkle-pop-item absolute text-xl';
                    const angle = (Math.PI * 2 / sparkleCount) * i;
                    const dist = 60 + Math.random() * 60;
                    sp.style.left = `calc(50% + ${Math.cos(angle)*dist}px)`;
                    sp.style.top = `calc(50% + ${Math.sin(angle)*dist}px)`;
                    sp.style.animationDelay = `${Math.random()*0.15}s`;
                    sparkleLayer.appendChild(sp);
                    setTimeout(() => sp.remove(), 1200);
                }

                // Step3: カード登場
                capsule.innerHTML = `
                    <div class="absolute inset-0 rounded-full ${glowClass}"></div>
                    <div class="relative z-10 flex flex-col items-center justify-center w-full h-full bg-slate-950/90 rounded-full animate-[combo-pop_0.4s_cubic-bezier(.36,1.5,.5,1)_both]">
                        <span class="${rankObj.color} text-[11px] px-3 py-0.5 rounded-full font-black mb-3 shadow border">${pulledChar.rank}</span>
                        <span class="text-7xl drop-shadow-2xl mb-2" style="filter: drop-shadow(0 0 15px ${rayColor});">${pulledChar.emoji}</span>
                        <span class="font-bold text-xs text-white tracking-wider text-center px-4">${pulledChar.name}</span>
                    </div>
                `;
                capsule.style.background = 'transparent';
                capsule.classList.remove('border-fuchsia-700/60');
                capsule.classList.add('border-transparent');

                setTimeout(() => { rayBg.style.opacity = '0'; }, 1800);
                setTimeout(() => { flash.classList.remove('gacha-flash-burst'); }, 700);

                btn.disabled = false; btn.classList.remove('opacity-50');
            }, shakeDuration);
        };

        // --- エクストリーム (進化/強化/フュージョン) ---
        window.renderExtreme = function(tab) {
            ui.modalTitle.textContent = "✨ EXTREME";
            const activeTabRainbow = "flex-1 py-2 rounded-xl font-bold text-[10px] rainbow-button text-white shadow-lg";
            let html = `
                <div class="flex space-x-2 mb-4 bg-slate-950 p-1 rounded-2xl border border-slate-800">
                    <button onclick="renderExtreme('rankup')" class="${tab==='rankup'?activeTabRainbow:inactiveTabClass}">強化/進化</button>
                    <button onclick="renderExtreme('fusion')" class="${tab==='fusion'?activeTabRainbow:inactiveTabClass}">フュージョン</button>
                </div>
                <div class="space-y-3 pb-4">
            `;
            
            if(tab === 'rankup') {
                html += `<p class="text-[10px] text-center text-slate-400 font-bold tracking-widest mb-4">草コイン(🌱)で強化 / 化石(🦴)と同名キャラで進化！</p>`;
                let count = 0;
                CHARACTERS.forEach(c => {
                    const data = collection[c.id] || { count: 0, level: 1 };
                    if(data.count === 0) return; 
                    
                    count++;
                    const rankObj = RANKS.find(r => r.name === c.rank) || { maxLevel: 20, color: 'text-slate-400 bg-slate-800' };
                    const cost = data.level * 50;
                    const isMax = data.level >= rankObj.maxLevel;
                    const canUpgrade = !isMax && state.grassCoins >= cost;
                    
                    const nextChar = c.nextId ? CHARACTERS.find(nc => nc.id === c.nextId) : null;
                    const canEvolve = isMax && nextChar && data.count >= 2 && state.fossils >= 1;
                    
                    html += `
                        <div class="bg-slate-900 border border-slate-700/50 p-3 rounded-2xl flex items-center justify-between">
                            <div class="flex items-center space-x-3">
                                <span class="text-3xl">${c.emoji}</span>
                                <div>
                                    <div class="font-bold text-xs text-white mb-0.5">${c.name} <span class="${rankObj.color} text-[8px] px-1 rounded">${c.rank}</span></div>
                                    <div class="text-[10px] text-emerald-400 font-bold">Lv.${data.level} <span class="text-slate-500">/ MAX ${rankObj.maxLevel}</span></div>
                                    <div class="text-[9px] text-slate-400">所持: ${data.count}体</div>
                                </div>
                            </div>
                            <div class="flex flex-col space-y-1">
                                ${!isMax ? `
                                    <button class="px-3 py-1.5 rounded-xl font-black text-[10px] shadow transition ${canUpgrade ? 'bg-emerald-500 text-slate-900 active:scale-95' : 'bg-slate-800 border border-slate-700 text-slate-600'}" onclick="extremeRankup('${c.id}')" ${!canUpgrade ? 'disabled' : ''}>
                                        強化 (🌱${cost})
                                    </button>
                                ` : ''}
                                ${nextChar ? `
                                    <button class="px-3 py-1.5 rounded-xl font-black text-[10px] shadow transition ${canEvolve ? 'bg-purple-600 text-white active:scale-95' : 'bg-slate-800 border border-slate-700 text-slate-600'}" onclick="executeEvolution('${c.id}')" ${!canEvolve ? 'disabled' : ''}>
                                        進化 (🦴1/2体)
                                    </button>
                                ` : ''}
                            </div>
                        </div>
                    `;
                });
                if(count === 0) html += `<div class="text-center text-slate-500 text-xs py-10 font-bold">所持しているキャラがいません。ガチャを引こう！</div>`;
            } else if(tab === 'fusion') {
                // 合体(フュージョン)選択画面
                html += `
                    <div class="text-center space-y-4 py-2 px-2">
                        <div>
                            <h4 class="text-base text-fuchsia-300 font-black tracking-widest drop-shadow">アルティメットフュージョン</h4>
                            <p class="text-[10px] text-slate-400 mt-1 leading-relaxed">2体の特殊キャラを合体させて【Xランク】を生成します。</p>
                        </div>
                        
                        <div class="bg-slate-950 border border-slate-800 p-3 rounded-2xl space-y-3">
                            <div class="text-left font-bold text-xs text-yellow-400 flex items-center justify-between">
                                <span>アルティメット・カズキング・ZEN</span>
                                <span class="rainbow-text text-[10px]">Xランク</span>
                            </div>
                            <div class="flex items-center justify-center space-x-2 text-xs">
                                <span class="bg-slate-900 px-2 py-1 rounded border border-slate-700">👑 カズキングタイジュウ</span>
                                <span>＋</span>
                                <span class="bg-slate-900 px-2 py-1 rounded border border-slate-700">🔥 覚醒ぜん</span>
                            </div>
                            <button onclick="executeSpecificFusion('ev_kazu', 'c1', 'fusion_zen_kazu')" class="rainbow-button w-full py-2 rounded-xl font-black text-xs text-white shadow">
                                合体実行
                            </button>
                        </div>
                    </div>
                `;
            }
            
            html += `</div>`;
            ui.modalContent.innerHTML = html;
        };

        window.extremeRankup = function(id) {
            const char = CHARACTERS.find(c => c.id === id);
            const rankObj = RANKS.find(r => r.name === char.rank) || { maxLevel: 20 };
            const data = collection[id];
            const cost = data.level * 50;
            if(state.grassCoins >= cost && data.level < rankObj.maxLevel) {
                state.grassCoins -= cost;
                data.level++;
                playSE('tap');
                updateHomeUI();
                renderExtreme('rankup');
            }
        };

        window.executeEvolution = function(baseId) {
            const baseChar = CHARACTERS.find(c => c.id === baseId);
            const nextChar = CHARACTERS.find(c => c.id === baseChar.nextId);
            const baseData = collection[baseId];
            if(baseData && baseData.count >= 2 && state.fossils >= 1) {
                baseData.count -= 2;
                state.fossils -= 1;
                if(!collection[nextChar.id]) collection[nextChar.id] = { count: 0, level: 1 };
                collection[nextChar.id].count++;
                playSE('gacha_rare');
                updateHomeUI();
                renderExtreme('rankup');
                alert(`🎉 進化成功！【${nextChar.name}】を獲得！`);
            }
        };

        window.executeSpecificFusion = function(id1, id2, targetId) {
            const c1 = collection[id1] ? collection[id1].count : 0;
            const c2 = collection[id2] ? collection[id2].count : 0;
            if(c1 >= 1 && c2 >= 1) {
                collection[id1].count--;
                collection[id2].count--;
                if(!collection[targetId]) collection[targetId] = { count: 0, level: 1 };
                collection[targetId].count++;
                playSE('gacha_rare');
                
                const targetChar = CHARACTERS.find(c => c.id === targetId);
                ui.modalContent.innerHTML = `
                    <div class="flex flex-col items-center justify-center py-10 space-y-6 h-full">
                        <h3 class="font-display text-2xl text-fuchsia-400 tracking-widest text-glow">FUSION SUCCESS!!</h3>
                        <div class="text-7xl drop-shadow-[0_0_30px_rgba(217,70,239,0.8)] animate-bounce">${targetChar.emoji}</div>
                        <div class="text-center space-y-1">
                            <div class="text-white font-black text-xl">${targetChar.name}</div>
                            <div class="rainbow-text font-black text-sm">RANK X 誕生！</div>
                        </div>
                        <button onclick="renderExtreme('fusion')" class="bg-slate-800 hover:bg-slate-700 px-6 py-2.5 rounded-xl text-xs font-bold mt-4 shadow active:scale-95 transition">戻る</button>
                    </div>
                `;
            } else {
                alert('必要なキャラクター(各1体)が揃っていません。');
            }
        };


        // ==================== ゲームエンジン (Canvas 2D) ====================
        const ctx = ui.canvas.getContext('2d');
        let dpr = window.devicePixelRatio || 1;
        let cw = 0, ch = 0;
        let lastTime = 0;
        let gameLoopId;
        
        const CONFIG = {
            grassProb: { normal: 0.949, gold: 0.05, rainbow: 0.001 },
            skillGainPerPull: 4,
            maxEntities: 30,
            baseEggSpeed: 90,
            baseSpawnRate: 0.8,
            uiSafeAreaHeight: 140
        };

        let grasses = [], eggs = [], particles = [], ripples = [], floatTexts = [], ambientParticles = [], shockwaves = [], slashes = [], timeSinceLastSpawn = 0;
        let comboCount = 0, comboTimer = 0;
        const COMBO_WINDOW = 1.4;

        function resizeCanvas() {
            const rect = ui.appContainer.getBoundingClientRect();
            cw = rect.width; ch = rect.height;
            ui.canvas.width = cw * dpr; ui.canvas.height = ch * dpr;
            ctx.scale(dpr, dpr);
            
            ambientParticles = [];
            for(let i=0; i<30; i++) ambientParticles.push(new AmbientParticle());
        }
        window.addEventListener('resize', resizeCanvas);

        class AmbientParticle {
            constructor() { this.reset(); this.y = Math.random() * ch; }
            reset() {
                this.x = Math.random() * cw; this.y = ch + 10;
                this.size = Math.random() * 2.5 + 0.5;
                this.vy = -(Math.random() * 20 + 10);
                this.vx = (Math.random() - 0.5) * 10;
                this.wobbleSpeed = Math.random() * 2 + 1;
                this.wobbleOffset = Math.random() * Math.PI * 2;
                this.alpha = Math.random() * 0.5 + 0.15;
                this.glow = Math.random() < 0.3;
            }
            update(dt, time) {
                this.y += this.vy * dt;
                this.x += Math.sin(time * this.wobbleSpeed + this.wobbleOffset) * 20 * dt;
                if (this.y < -10) this.reset();
            }
            draw(ctx) {
                ctx.save();
                if (this.glow) { ctx.shadowBlur = 8; ctx.shadowColor = 'rgba(167,243,208,0.8)'; }
                ctx.fillStyle = `rgba(167, 243, 208, ${this.alpha})`;
                ctx.beginPath(); ctx.arc(this.x, this.y, this.size, 0, Math.PI*2); ctx.fill();
                ctx.restore();
            }
        }

        // 地面の衝撃波リング(タップ着地演出)
        class Shockwave {
            constructor(x, y, color) {
                this.x = x; this.y = y; this.color = color; this.life = 1.0; this.radius = 5;
            }
            update(dt) { this.radius += 320 * dt; this.life -= dt * 3.2; }
            draw(ctx) {
                const a = Math.max(0, this.life);
                ctx.save();
                ctx.strokeStyle = this.color; ctx.globalAlpha = a * 0.8; ctx.lineWidth = 4 * a;
                ctx.beginPath(); ctx.ellipse(this.x, this.y, this.radius, this.radius * 0.4, 0, 0, Math.PI*2); ctx.stroke();
                ctx.restore();
            }
        }

        // 斬撃エフェクト(草を抜いた瞬間の切れ味線)
        class Slash {
            constructor(x, y, color) {
                this.x = x; this.y = y; this.color = color; this.life = 1.0;
                this.angle = Math.random() * Math.PI - Math.PI/2;
                this.len = 55 + Math.random() * 25;
            }
            update(dt) { this.life -= dt * 5; }
            draw(ctx) {
                const a = Math.max(0, this.life);
                ctx.save();
                ctx.translate(this.x, this.y); ctx.rotate(this.angle);
                ctx.globalAlpha = a;
                const grad = ctx.createLinearGradient(-this.len/2, 0, this.len/2, 0);
                grad.addColorStop(0, 'rgba(255,255,255,0)');
                grad.addColorStop(0.5, '#ffffff');
                grad.addColorStop(1, 'rgba(255,255,255,0)');
                ctx.strokeStyle = grad; ctx.lineWidth = 4 * a; ctx.lineCap = 'round';
                ctx.shadowBlur = 12; ctx.shadowColor = this.color;
                ctx.beginPath(); ctx.moveTo(-this.len/2, 0); ctx.lineTo(this.len/2, 0); ctx.stroke();
                ctx.restore();
            }
        }

        class Grass {
            constructor(x, y) {
                this.x = x; this.y = y; this.radius = 28; this.growth = 0;
                const r = Math.random();
                if (r < CONFIG.grassProb.rainbow) this.type = 'rainbow';
                else if (r < CONFIG.grassProb.rainbow + CONFIG.grassProb.gold) this.type = 'gold';
                else this.type = 'normal';
                
                this.swayOffset = Math.random() * Math.PI * 2;
                this.swaySpeed = Math.random() * 1.5 + 2;
                this.tilt = (Math.random() - 0.5) * 0.3;
                this.leafScaleVariance = 0.85 + Math.random() * 0.3;
                this.bobOffset = Math.random() * Math.PI * 2;
                if (this.type === 'normal') { this.score = 10; this.coin = 5; }
                else if (this.type === 'gold') { this.score = 80; this.coin = 35; }
                else { this.score = 300; this.coin = 100; }
            }
            update(dt) {
                if (this.growth < 1) this.growth = Math.min(1, this.growth + dt * 2.8);
                if (this.type === 'gold' && Math.random() < 0.04 && this.growth >= 1) {
                    particles.push(new Particle(this.x + (Math.random()-0.5)*24, this.y - 20 - Math.random()*20, '#fef08a', true));
                }
                if (this.type === 'rainbow' && Math.random() < 0.15 && this.growth >= 1) {
                    particles.push(new Particle(this.x + (Math.random()-0.5)*24, this.y - Math.random()*35, '#fff', true));
                }
            }
            draw(ctx, time) {
                // 地面の落ち影(質感アップの要)
                const groundScale = 1 - Math.pow(1 - this.growth, 3);
                ctx.save();
                ctx.globalAlpha = 0.35 * groundScale;
                ctx.fillStyle = '#000';
                ctx.beginPath(); ctx.ellipse(this.x + 4, this.y + 6, this.radius * 0.55 * groundScale, this.radius * 0.2 * groundScale, 0, 0, Math.PI*2); ctx.fill();
                ctx.restore();

                ctx.save(); ctx.translate(this.x, this.y);
                const easeGrowth = 1 - Math.pow(1 - this.growth, 3);
                const bob = Math.sin(time * 3 + this.bobOffset) * 1.5 * easeGrowth;
                ctx.translate(0, bob);
                ctx.scale(easeGrowth * this.leafScaleVariance, easeGrowth * this.leafScaleVariance);
                ctx.rotate(this.tilt + Math.sin(time * this.swaySpeed + this.swayOffset) * 0.14);

                if (this.type === 'normal') {
                    this.drawLeaves(ctx, '#064e3b', '#059669', '#6ee7b7', time);
                } else if (this.type === 'gold') {
                    ctx.shadowBlur = 24; ctx.shadowColor = 'rgba(250, 204, 21, 0.75)';
                    this.drawLeaves(ctx, '#78350f', '#d97706', '#fef08a', time);
                    // 輝く粒
                    ctx.shadowBlur = 0;
                    ctx.globalCompositeOperation = 'lighter';
                    const sparkPulse = (Math.sin(time*7)+1)/2;
                    ctx.fillStyle = `rgba(255,255,255,${0.5 + sparkPulse*0.5})`;
                    ctx.beginPath(); ctx.arc(Math.cos(time*6)*13, Math.sin(time*6)*16 - 18, 2.2, 0, Math.PI*2); ctx.fill();
                    ctx.beginPath(); ctx.arc(Math.cos(time*4+2)*10, Math.sin(time*4+2)*12 - 25, 1.6, 0, Math.PI*2); ctx.fill();
                    ctx.globalCompositeOperation = 'source-over';
                } else if (this.type === 'rainbow') {
                    const hue = (time * 140) % 360;
                    ctx.shadowBlur = 30; ctx.shadowColor = `hsl(${hue}, 100%, 62%)`;
                    this.drawLeaves(ctx, `hsl(${(hue+200)%360}, 70%, 28%)`, `hsl(${(hue+40)%360}, 90%, 45%)`, `hsl(${hue}, 100%, 78%)`, time);
                    ctx.shadowBlur = 0;
                    ctx.globalCompositeOperation = 'lighter';
                    ctx.fillStyle = `hsla(${hue}, 100%, 65%, 0.35)`;
                    ctx.beginPath(); ctx.ellipse(0, -16, 22, 32, 0, 0, Math.PI*2); ctx.fill();
                    // 虹の光輪(オーラリング)
                    for (let i = 0; i < 3; i++) {
                        const rh = (hue + i * 60) % 360;
                        ctx.strokeStyle = `hsla(${rh}, 100%, 70%, 0.5)`;
                        ctx.lineWidth = 1.4;
                        ctx.beginPath(); ctx.arc(0, -14, 24 + i*5 + Math.sin(time*3+i)*2, 0, Math.PI*2); ctx.stroke();
                    }
                    ctx.globalCompositeOperation = 'source-over';
                }
                ctx.restore();
            }
            drawLeaves(ctx, baseColor, midColor, tipColor, time) {
                // 3枚の葉に多層グラデーション + ハイライト線で質感UP
                const drawLeaf = (curveMul, flip) => {
                    const grad = ctx.createLinearGradient(0, 0, flip * 14, -42);
                    grad.addColorStop(0, baseColor); grad.addColorStop(0.55, midColor); grad.addColorStop(1, tipColor);
                    ctx.fillStyle = grad;
                    ctx.beginPath();
                    ctx.moveTo(0, 2);
                    ctx.bezierCurveTo(flip*-15*curveMul, -15, flip*-10*curveMul, -35, flip*2, -46);
                    ctx.bezierCurveTo(flip*12*curveMul, -35, flip*15*curveMul, -15, 0, 2);
                    ctx.closePath(); ctx.fill();
                    // ハイライト(光沢)
                    ctx.save();
                    ctx.globalAlpha = 0.35;
                    ctx.fillStyle = 'rgba(255,255,255,0.6)';
                    ctx.beginPath();
                    ctx.moveTo(flip*2, -10);
                    ctx.bezierCurveTo(flip*-4*curveMul, -20, flip*-2*curveMul, -32, flip*2, -40);
                    ctx.bezierCurveTo(flip*5*curveMul, -32, flip*6*curveMul, -20, flip*2, -10);
                    ctx.closePath(); ctx.fill();
                    ctx.restore();
                    // 葉脈
                    ctx.strokeStyle = 'rgba(0,0,0,0.2)'; ctx.lineWidth = 0.8;
                    ctx.beginPath(); ctx.moveTo(flip*1, 0); ctx.quadraticCurveTo(flip*1, -25, flip*2, -44); ctx.stroke();
                };
                // 中央の葉(奥)
                drawLeaf(0.55, 1);
                // 左右の葉(手前)
                ctx.save(); drawLeaf(1.6, -1); ctx.restore();
                ctx.save(); drawLeaf(1.6, 1); ctx.restore();
            }
        }

        class Egg {
            constructor(x, y, level) {
                this.x = x; this.y = y; this.radius = 22;
                const speed = CONFIG.baseEggSpeed + (level * 6);
                const angle = Math.random() * Math.PI * 2;
                this.vx = Math.cos(angle) * speed; this.vy = Math.sin(angle) * speed;
                this.pulseOff = Math.random() * 10; this.trail = [];
                this.squash = 1; this.stretch = 1; this.angle = angle;
                this.spikeRot = Math.random() * Math.PI * 2;
            }
            update(dt) {
                this.trail.unshift({x: this.x, y: this.y});
                if (this.trail.length > 12) this.trail.pop();

                this.x += this.vx * dt; this.y += this.vy * dt;
                const margin = this.radius;
                const topBound = CONFIG.uiSafeAreaHeight + margin; 
                const bottomBound = ch - margin - 10;
                
                if (this.x < margin) { this.x = margin; this.vx *= -1; }
                else if (this.x > cw - margin) { this.x = cw - margin; this.vx *= -1; }
                if (this.y < topBound) { this.y = topBound; this.vy *= -1; }
                else if (this.y > bottomBound) { this.y = bottomBound; this.vy *= -1; }

                this.vx += Math.sin(Date.now()*0.005 + this.pulseOff) * 60 * dt;
                this.vy += Math.cos(Date.now()*0.004 + this.pulseOff) * 60 * dt;
                
                const speed = Math.hypot(this.vx, this.vy);
                const targetSpeed = CONFIG.baseEggSpeed + (state.level * 6);
                this.vx = (this.vx / speed) * targetSpeed; this.vy = (this.vy / speed) * targetSpeed;
                this.angle = Math.atan2(this.vy, this.vx);
                
                const velocityFactor = speed / 150;
                this.stretch = 1 + velocityFactor * 0.3; this.squash = 1 - velocityFactor * 0.15;
                this.spikeRot += dt * 1.5;
            }
            draw(ctx, time) {
                // 危険を示す軌跡(赤いグラデーショントレイル)
                if (this.trail.length > 1) {
                    for (let i = 1; i < this.trail.length; i++) {
                        const a = (1 - i / this.trail.length) * 0.35;
                        ctx.beginPath();
                        ctx.moveTo(this.trail[i-1].x, this.trail[i-1].y);
                        ctx.lineTo(this.trail[i].x, this.trail[i].y);
                        ctx.strokeStyle = `rgba(239, 68, 68, ${a})`;
                        ctx.lineWidth = this.radius * 1.3 * (1 - i / this.trail.length);
                        ctx.lineCap = 'round'; ctx.stroke();
                    }
                }

                // 地面の落ち影
                ctx.save();
                ctx.globalAlpha = 0.4; ctx.fillStyle = '#000';
                ctx.beginPath(); ctx.ellipse(this.x, this.y + this.radius*0.9, this.radius*0.7, this.radius*0.25, 0, 0, Math.PI*2); ctx.fill();
                ctx.restore();

                ctx.save(); ctx.translate(this.x, this.y);

                // 外側の脈動する警告オーラ(危険な質感)
                const pulse = Math.sin(time * 7 + this.pulseOff) * 0.2 + 0.9;
                const auraGrad = ctx.createRadialGradient(0, 0, this.radius*0.6, 0, 0, this.radius * 2.1 * pulse);
                auraGrad.addColorStop(0, 'rgba(239, 68, 68, 0.35)');
                auraGrad.addColorStop(1, 'rgba(239, 68, 68, 0)');
                ctx.fillStyle = auraGrad;
                ctx.beginPath(); ctx.arc(0, 0, this.radius * 2.1 * pulse, 0, Math.PI*2); ctx.fill();

                ctx.rotate(this.angle + Math.PI/2); ctx.scale(this.squash, this.stretch);

                // トゲトゲの触手/棘(不気味さ演出)
                ctx.save();
                ctx.rotate(this.spikeRot);
                for (let i = 0; i < 6; i++) {
                    const a = (Math.PI * 2 / 6) * i;
                    const spikeLen = this.radius * 0.55 * (0.8 + Math.sin(time*5+i)*0.2);
                    ctx.save(); ctx.rotate(a);
                    ctx.beginPath();
                    ctx.moveTo(0, -this.radius*0.85);
                    ctx.lineTo(-4, -this.radius*0.85 - spikeLen);
                    ctx.lineTo(4, -this.radius*0.85 - spikeLen);
                    ctx.closePath();
                    ctx.fillStyle = '#7f1d1d'; ctx.fill();
                    ctx.restore();
                }
                ctx.restore();

                // 本体(妖しい光沢の卵殻)
                ctx.beginPath(); ctx.ellipse(0, 0, this.radius, this.radius * 1.25, 0, 0, Math.PI*2);
                const grad = ctx.createRadialGradient(-this.radius*0.3, -this.radius*0.6, 2, 0, 0, this.radius*1.3);
                grad.addColorStop(0, '#fef3c7'); grad.addColorStop(0.35, '#f59e0b'); grad.addColorStop(0.7, '#b91c1c'); grad.addColorStop(1, '#450a0a');
                ctx.fillStyle = grad; ctx.shadowBlur = 22; ctx.shadowColor = '#dc2626'; ctx.fill();

                // ひび割れ模様(不気味な質感 - 発光する亀裂)
                const crackGlow = (Math.sin(time*8+this.pulseOff)+1)/2;
                ctx.strokeStyle = `rgba(254, 240, 138, ${0.5 + crackGlow*0.5})`; ctx.lineWidth = 1.8;
                ctx.shadowBlur = 8; ctx.shadowColor = '#fde047';
                ctx.beginPath(); ctx.moveTo(-8, 12); ctx.quadraticCurveTo(-15, 0, -5, -8); ctx.lineTo(2,-14);
                ctx.moveTo(5, 10); ctx.quadraticCurveTo(12, 0, 2, -10);
                ctx.moveTo(0, 15); ctx.lineTo(-2, 5); ctx.lineTo(3,0);
                ctx.stroke();

                // ハイライト(質感の光沢)
                ctx.shadowBlur = 0;
                ctx.fillStyle = 'rgba(255,255,255,0.4)';
                ctx.beginPath(); ctx.ellipse(-this.radius*0.35, -this.radius*0.55, this.radius*0.28, this.radius*0.4, -0.4, 0, Math.PI*2); ctx.fill();

                // 邪悪な目玉のような発光点(2つ)
                const eyeGlow = 0.6 + crackGlow * 0.4;
                ctx.globalCompositeOperation = 'lighter';
                ctx.fillStyle = `rgba(255, 60, 60, ${eyeGlow})`;
                ctx.beginPath(); ctx.arc(-6, -2, 2.2, 0, Math.PI*2); ctx.fill();
                ctx.beginPath(); ctx.arc(6, -2, 2.2, 0, Math.PI*2); ctx.fill();
                ctx.globalCompositeOperation = 'source-over';

                ctx.restore();
            }
        }

        class Particle {
            constructor(x, y, color, isAmbientSparkle = false, isStar = false) {
                this.x = x; this.y = y;
                const a = Math.random() * Math.PI * 2;
                const v = isAmbientSparkle ? Math.random() * 30 + 10 : Math.random() * 220 + 60;
                this.vx = Math.cos(a) * v; this.vy = Math.sin(a) * v - (isAmbientSparkle ? 20 : 170);
                this.life = 1.0; this.decay = isAmbientSparkle ? Math.random() * 1 + 0.5 : Math.random() * 1.5 + 0.8;
                this.color = color; this.size = isAmbientSparkle ? Math.random() * 2 + 1 : Math.random() * 5 + 2;
                this.isAmbientSparkle = isAmbientSparkle;
                this.isStar = isStar;
                this.rot = Math.random() * Math.PI * 2; this.rotSpeed = (Math.random()-0.5) * 8;
            }
            update(dt) {
                if (!this.isAmbientSparkle) this.vy += 600 * dt;
                this.x += this.vx * dt; this.y += this.vy * dt; this.life -= dt * this.decay;
                this.rot += this.rotSpeed * dt;
            }
            draw(ctx) {
                ctx.save();
                ctx.globalAlpha = Math.max(0, this.life); ctx.fillStyle = this.color;
                ctx.shadowBlur = this.isStar ? 8 : 0; ctx.shadowColor = this.color;
                if (this.isStar) {
                    ctx.translate(this.x, this.y); ctx.rotate(this.rot);
                    drawStarPath(ctx, this.size * 1.8);
                    ctx.fill();
                } else {
                    ctx.beginPath(); ctx.arc(this.x, this.y, this.size, 0, Math.PI*2); ctx.fill();
                }
                ctx.restore();
            }
        }

        function drawStarPath(ctx, r) {
            ctx.beginPath();
            for (let i = 0; i < 5; i++) {
                const a1 = (Math.PI * 2 / 5) * i - Math.PI/2;
                const a2 = a1 + Math.PI / 5;
                ctx.lineTo(Math.cos(a1) * r, Math.sin(a1) * r);
                ctx.lineTo(Math.cos(a2) * r * 0.45, Math.sin(a2) * r * 0.45);
            }
            ctx.closePath();
        }

        class Ripple {
            constructor(x, y, color, maxWidth = 3) { this.x = x; this.y = y; this.radius = 0; this.life = 1.0; this.color = color; this.maxWidth = maxWidth; }
            update(dt) { this.radius += 180 * dt; this.life -= dt * 2.2; }
            draw(ctx) {
                ctx.save();
                ctx.globalAlpha = Math.max(0, this.life); ctx.strokeStyle = this.color; ctx.lineWidth = this.maxWidth * this.life;
                ctx.shadowBlur = 10; ctx.shadowColor = this.color;
                ctx.beginPath(); ctx.arc(this.x, this.y, this.radius, 0, Math.PI*2); ctx.stroke();
                ctx.restore();
            }
        }

        class FloatText {
            constructor(x, y, text, color, isLarge = false) {
                this.x = x; this.y = y; this.text = text; this.color = color;
                this.life = 1.0; this.vy = -65; this.scale = 0.3; this.isLarge = isLarge;
                this.wobble = Math.random() * Math.PI * 2;
            }
            update(dt) {
                this.y += this.vy * dt; this.vy *= (1 - dt * 0.6);
                this.life -= dt * 1.1;
                if (this.scale < 1) this.scale += dt * 7;
            }
            draw(ctx) {
                ctx.save(); ctx.globalAlpha = Math.max(0, this.life); ctx.translate(this.x, this.y);
                const s = Math.min(1, this.scale) * (this.scale > 1 ? 1 : (0.85 + Math.sin(this.wobble)*0.05));
                ctx.scale(s, s);
                ctx.font = `900 ${this.isLarge ? '34px' : '23px'} "Noto Sans JP"`; ctx.textAlign = 'center';
                ctx.lineWidth = 4; ctx.strokeStyle = 'rgba(0,0,0,0.65)'; ctx.lineJoin = 'round';
                ctx.strokeText(this.text, 0, 0);
                const grad = ctx.createLinearGradient(0, -14, 0, 14);
                grad.addColorStop(0, '#ffffff'); grad.addColorStop(0.4, this.color); grad.addColorStop(1, this.color);
                ctx.fillStyle = grad;
                ctx.shadowBlur = 10; ctx.shadowColor = this.color;
                ctx.fillText(this.text, 0, 0);
                ctx.restore();
            }
        }

        function startGame() {
            resizeCanvas();
            grasses = []; eggs = []; particles = []; ripples = []; floatTexts = []; shockwaves = []; slashes = [];
            comboCount = 0; comboTimer = 0;
            ui.comboDisplay.classList.add('hidden');
            
            state.score = 0; state.weedPulledCount = 0; state.timeLeft = 60; state.skillGauge = 0;
            state.isPlaying = true; state.stats = { normal: 0, gold: 0, rainbow: 0 };
            
            updateGameUI();
            
            for(let i=0; i<3; i++) spawnGrass();
            for(let i=0; i<2; i++) spawnEgg();

            lastTime = performance.now();
            cancelAnimationFrame(gameLoopId);
            gameLoopId = requestAnimationFrame(gameLoop);
            
            if (state.timerId) clearInterval(state.timerId);
            state.timerId = setInterval(() => {
                if(!state.isPlaying) return;
                state.timeLeft--;
                ui.gameTimer.textContent = state.timeLeft;
                if(state.timeLeft <= 0) gameOver("TIME UP", false);
            }, 1000);
        }

        function gameLoop(currentTime) {
            if (!state.isPlaying) return;
            const dt = Math.min((currentTime - lastTime) / 1000, 0.1);
            lastTime = currentTime;
            const timeSec = currentTime / 1000;

            updateEngine(dt, timeSec);
            drawEngine(timeSec);

            gameLoopId = requestAnimationFrame(gameLoop);
        }

        function updateEngine(dt, timeSec) {
            timeSinceLastSpawn += dt;
            const spawnRate = Math.max(0.2, CONFIG.baseSpawnRate - (state.level * 0.05));
            if (timeSinceLastSpawn > spawnRate) {
                timeSinceLastSpawn = 0;
                if (grasses.length + eggs.length < CONFIG.maxEntities) {
                    if (Math.random() < 0.12) spawnEgg(); else spawnGrass();
                }
            }

            // コンボタイマー減衰
            if (comboCount > 0) {
                comboTimer -= dt;
                if (comboTimer <= 0) { comboCount = 0; ui.comboDisplay.classList.add('hidden'); }
            }

            ambientParticles.forEach(p => p.update(dt, timeSec));
            grasses.forEach(g => g.update(dt));
            eggs.forEach(e => e.update(dt));
            
            particles.forEach(p => p.update(dt)); particles = particles.filter(p => p.life > 0);
            ripples.forEach(r => r.update(dt)); ripples = ripples.filter(r => r.life > 0);
            floatTexts.forEach(f => f.update(dt)); floatTexts = floatTexts.filter(f => f.life > 0);
            shockwaves.forEach(s => s.update(dt)); shockwaves = shockwaves.filter(s => s.life > 0);
            slashes.forEach(s => s.update(dt)); slashes = slashes.filter(s => s.life > 0);
        }

        function drawEngine(time) {
            ctx.clearRect(0, 0, cw, ch);

            // 地面にほのかなビネット(奥行き感)
            const vgn = ctx.createRadialGradient(cw/2, ch/2, ch*0.2, cw/2, ch/2, ch*0.75);
            vgn.addColorStop(0, 'rgba(0,0,0,0)'); vgn.addColorStop(1, 'rgba(0,0,0,0.35)');
            ctx.fillStyle = vgn; ctx.fillRect(0,0,cw,ch);

            ambientParticles.forEach(p => p.draw(ctx));

            grasses.sort((a,b) => a.y - b.y); grasses.forEach(g => g.draw(ctx, time));
            eggs.sort((a,b) => a.y - b.y); eggs.forEach(e => e.draw(ctx, time));
            
            shockwaves.forEach(s => s.draw(ctx));
            ripples.forEach(r => r.draw(ctx));
            slashes.forEach(s => s.draw(ctx));
            particles.forEach(p => p.draw(ctx));
            floatTexts.forEach(f => f.draw(ctx));
        }

        function getValidSpawnPos() {
            const marginX = 40;
            const minY = CONFIG.uiSafeAreaHeight + 20; 
            const maxY = ch - 40;
            const x = marginX + Math.random() * (cw - marginX*2);
            const y = minY + Math.random() * (maxY - minY);
            return {x, y};
        }

        function spawnGrass() { const pos = getValidSpawnPos(); grasses.push(new Grass(pos.x, pos.y)); }
        function spawnEgg() { const pos = getValidSpawnPos(); eggs.push(new Egg(pos.x, pos.y, state.level)); }

        ui.canvas.addEventListener('pointerdown', (e) => {
            if (!state.isPlaying) return;
            const rect = ui.canvas.getBoundingClientRect();
            checkTap(e.clientX - rect.left, e.clientY - rect.top);
        });

        function checkTap(px, py) {
            for (let i = eggs.length - 1; i >= 0; i--) {
                const egg = eggs[i];
                if (Math.hypot(egg.x - px, egg.y - py) < egg.radius * 1.6) {
                    createExplosion(egg.x, egg.y, '#ef4444', 30);
                    ripples.push(new Ripple(egg.x, egg.y, '#ef4444', 5));
                    shockwaves.push(new Shockwave(egg.x, egg.y, '#ef4444'));
                    playSE('death');
                    gameOver("FATAL ERROR", true);
                    return;
                }
            }

            let hitIndex = -1;
            for (let i = grasses.length - 1; i >= 0; i--) {
                const g = grasses[i];
                if (Math.hypot(g.x - px, (g.y - 25) - py) < g.radius * 1.8) { hitIndex = i; break; }
            }

            if (hitIndex !== -1) {
                handleGrassPull(grasses.splice(hitIndex, 1)[0]);
                spawnGrass();
            }
        }

        function handleGrassPull(g) {
            let color = '#34d399', textColor = '#6ee7b7', se = 'pull_normal', particleCount = 16;
            if (g.type === 'gold') { color = '#fde047'; textColor = '#fef08a'; se = 'pull_gold'; particleCount = 26; }
            else if (g.type === 'rainbow') { color = '#22d3ee'; textColor = '#67e8f9'; se = 'pull_rainbow'; particleCount = 40; }
            
            createExplosion(g.x, g.y, color, particleCount, g.type !== 'normal');
            ripples.push(new Ripple(g.x, g.y - 25, color, g.type === 'normal' ? 3 : 5));
            shockwaves.push(new Shockwave(g.x, g.y + 5, color));
            slashes.push(new Slash(g.x, g.y - 25, color));
            playSE(se);

            // コンボ更新
            comboCount++; comboTimer = COMBO_WINDOW;
            ui.comboDisplay.classList.remove('hidden');
            ui.comboCount.textContent = comboCount;
            ui.comboCount.classList.remove('combo-pop'); void ui.comboCount.offsetWidth; ui.comboCount.classList.add('combo-pop');
            if (comboCount >= 3) playSE('combo');
            const comboMult = 1 + Math.min(comboCount * 0.05, 1.0); // 最大+100%

            const gainedScore = Math.round(g.score * comboMult);
            floatTexts.push(new FloatText(g.x, g.y - 40, `+${gainedScore}`, textColor, g.type !== 'normal'));
            if (comboCount >= 5 && comboCount % 5 === 0) {
                floatTexts.push(new FloatText(g.x, g.y - 70, `${comboCount} COMBO!`, '#fde047', true));
            }

            state.score += gainedScore; state.grassCoins += g.coin; state.stats[g.type]++; state.weedPulledCount++;
            state.skillGauge = Math.min(100, state.skillGauge + CONFIG.skillGainPerPull);

            // ミッション進捗
            updateMissionProgress('daily', 'd1', 1);
            if(g.type === 'rainbow') updateMissionProgress('event', 'e1', 1);

            updateGameUI();
            checkLevelUp();
        }

        function createExplosion(x, y, color, count = 20, withStars = false) {
            const r = parseInt(color.slice(1,3), 16), g = parseInt(color.slice(3,5), 16), b = parseInt(color.slice(5,7), 16);
            for(let i=0; i<count; i++) {
                const isStar = withStars && i % 4 === 0;
                particles.push(new Particle(x, y, `rgb(${Math.min(255,Math.max(0,r+Math.random()*40-20))}, ${Math.min(255,Math.max(0,g+Math.random()*40-20))}, ${Math.min(255,Math.max(0,b+Math.random()*40-20))})`, false, isStar));
            }
            // 白フラッシュ小粒(質感アップ)
            for(let i=0; i<Math.floor(count*0.3); i++) {
                particles.push(new Particle(x, y, '#ffffff'));
            }
        }

        function updateGameUI() {
            ui.gameScore.textContent = state.score;
            ui.skillBar.style.width = `${state.skillGauge}%`;
            
            if (state.skillGauge >= 100) {
                ui.skillBtn.disabled = false;
                ui.skillBtn.classList.add('skill-ready'); ui.skillBtn.classList.replace('bg-slate-800', 'bg-amber-500'); ui.skillBtn.classList.replace('text-slate-500', 'text-slate-950');
                ui.skillText.textContent = "MAX!!"; ui.skillText.classList.add('animate-pulse', 'text-yellow-300', 'scale-110');
            } else {
                ui.skillBtn.disabled = true;
                ui.skillBtn.classList.remove('skill-ready'); ui.skillBtn.classList.replace('bg-amber-500', 'bg-slate-800'); ui.skillBtn.classList.replace('text-slate-950', 'text-slate-500');
                ui.skillText.textContent = `${state.skillGauge}%`; ui.skillText.classList.remove('animate-pulse', 'text-yellow-300', 'scale-110');
            }
        }

        function checkLevelUp() {
            if (state.weedPulledCount >= state.level * 6) {
                state.level++;
                updateMissionProgress('normal', 'n1', state.level, true);
                state.timeLeft += 30; ui.gameTimer.textContent = state.timeLeft;
                floatTexts.push(new FloatText(cw/2, ch*0.4, `LEVEL ${state.level}!! +30sec`, '#a7f3d0', true));
                ripples.push(new Ripple(cw/2, ch/2, '#10b981', 6));
                ripples.push(new Ripple(cw/2, ch/2, '#6ee7b7', 4));
                createExplosion(cw/2, ch/2, '#34d399', 24, true);
                playSE('levelup');
                ui.appContainer.classList.add('flash-white-anim'); setTimeout(()=>ui.appContainer.classList.remove('flash-white-anim'), 500);
            }
        }

        ui.skillBtn.addEventListener('click', () => {
            if (state.skillGauge < 100 || !state.isPlaying) return;
            state.skillGauge = 0; state.fossils++; state.timeLeft += 10;
            
            updateMissionProgress('normal', 'n2', 1);

            playSE('skill');
            ui.appContainer.classList.add('shake', 'flash'); setTimeout(()=>ui.appContainer.classList.remove('shake', 'flash'), 800);
            floatTexts.push(new FloatText(cw/2, ch/2 - 50, "🔥🔥 一掃完了 🔥🔥", '#fde047', true));
            floatTexts.push(new FloatText(cw/2, ch/2, "+10sec & 化石GET!", '#fcd34d'));
            for (let i = 0; i < 4; i++) {
                ripples.push(new Ripple(cw/2, ch/2, i % 2 === 0 ? '#fbbf24' : '#fde047', 6));
            }
            shockwaves.push(new Shockwave(cw/2, ch/2, '#fbbf24'));

            grasses.forEach((g, i) => {
                state.score += g.score * 2; state.grassCoins += g.coin; state.stats[g.type]++;
                setTimeout(() => { createExplosion(g.x, g.y, '#fbbf24', 22, true); }, i * 15);
            });
            grasses = [];
            updateGameUI();
        });

        ui.retireBtn.addEventListener('click', () => { if(!state.isPlaying) return; gameOver("RETIRE", false); });

        function gameOver(title, isDead) {
            state.isPlaying = false; clearInterval(state.timerId);
            ui.comboDisplay.classList.add('hidden'); comboCount = 0;
            if (isDead) { ui.appContainer.classList.add('shake', 'flash'); setTimeout(() => ui.appContainer.classList.remove('shake', 'flash'), 1000); }

            const earnedCoins = Math.floor(state.score / 10);
            state.grassCoins += earnedCoins;

            ui.overlayTitle.textContent = title;
            ui.overlayTitle.className = `font-display text-5xl mb-2 tracking-tighter drop-shadow-2xl relative z-10 ${isDead ? 'text-rose-500 text-glow' : 'text-emerald-400 text-glow'}`;

            ui.overlayDesc.innerHTML = `
                <div class="flex justify-between items-end border-b border-slate-700 pb-3">
                    <span class="text-slate-400 text-sm font-bold tracking-widest">FINAL SCORE</span>
                    <span class="font-display text-white text-4xl text-glow">${state.score}</span>
                </div>
                <div class="grid grid-cols-2 gap-3 text-left pt-3">
                    <div class="bg-slate-900 p-3 rounded-xl border border-slate-700/50 shadow-inner">
                        <span class="block text-[10px] text-slate-500 font-bold mb-1">REACHED LV</span>
                        <span class="font-black text-emerald-400 text-xl">Lv.${state.level}</span>
                    </div>
                    <div class="bg-slate-900 p-3 rounded-xl border border-slate-700/50 shadow-inner">
                        <span class="block text-[10px] text-slate-500 font-bold mb-1">FOSSIL BONUS</span>
                        <span class="font-black text-amber-400 text-xl">🦴+${Math.max(0, state.fossils-3)}</span>
                    </div>
                    <div class="col-span-2 flex justify-between bg-slate-900 p-3 rounded-xl border border-slate-700/50 shadow-inner">
                        <div class="text-center"><span class="block text-[10px] text-slate-500 font-bold">NORMAL</span><span class="font-black text-emerald-400 text-lg">${state.stats.normal}</span></div>
                        <div class="text-center"><span class="block text-[10px] text-slate-500 font-bold">GOLD</span><span class="font-black text-yellow-400 text-lg">${state.stats.gold}</span></div>
                        <div class="text-center"><span class="block text-[10px] text-slate-500 font-bold">RAINBOW</span><span class="font-black text-cyan-400 text-lg">${state.stats.rainbow}</span></div>
                    </div>
                </div>
                <div class="bg-gradient-to-r from-emerald-900/40 to-teal-900/40 border border-emerald-500/30 p-4 rounded-2xl mt-5 shadow-lg relative overflow-hidden">
                    <span class="block text-[10px] text-emerald-300 font-bold tracking-widest mb-1">COIN REWARD</span>
                    <span class="text-3xl font-black text-yellow-300 drop-shadow-md">+ ${earnedCoins} 🌱</span>
                </div>
            `;
            
            ui.overlayActionBtn.textContent = "RETURN TO HOME";
            ui.overlayActionBtn.onclick = () => { playSE('tap'); ui.gameOverlay.classList.add('hidden'); switchScreen('home'); };
            ui.gameOverlay.classList.remove('hidden');
        }

        // 初期化処理
        resizeCanvas();
    </script>
<script type="module" src="https://static.cloudflareinsights.com/beacon.min.js/v3d52b47920f24c319d37e2661827c42b1787588026925" integrity="sha512-d9sL6GJLXn6fInD1+TVXhTcQOsmxeHfmHAvwGDIxp5TO+uo1fiWW7mHomMj4MLRlCsJDTqXzWLHJFFlPCEIj/A==" data-cf-beacon='{"version":"2024.11.0","token":"4edd5f8ec12a48cfa682ab8261b80a79"}' crossorigin="anonymous"></script>
</body>
</html>
