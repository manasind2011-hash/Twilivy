<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scarlet Vault</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@500;600&family=Montserrat:wght@300;400;500&display=swap');

        :root {
            --audio-glow: 0;
            --audio-scale: 0;
            --warp-intensity: 1;
            --theme-grad-1: #1c0616;
            --theme-grad-2: #3b0a29;
            --theme-grad-3: #610f41;
            --theme-grad-4: #120320;
        }

        * { box-sizing: border-box; }

        body {
            background: linear-gradient(125deg, var(--theme-grad-1), var(--theme-grad-2), var(--theme-grad-3), var(--theme-grad-4));
            background-size: 400% 400%;
            animation: auroraShift 14s ease infinite;
            color: #EDF2F4;
            font-family: 'Montserrat', sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            overflow: hidden;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            user-select: none;
            -webkit-user-select: none;
            cursor: crosshair;
        }

        @keyframes auroraShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        .cursor-light {
            position: fixed;
            width: 500px; height: 500px;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(255, 77, 148, 0.25) 0%, rgba(255, 20, 147, 0.08) 40%, transparent 70%);
            pointer-events: none;
            transform: translate(-50%, -50%);
            z-index: 2;
            transition: width 0.3s, height 0.3s;
        }

        .cursor-sparkle {
            position: fixed;
            width: 6px; height: 6px;
            background: #ffffff;
            border-radius: 50%;
            box-shadow: 0 0 10px #ff4d94, 0 0 20px #ffb6c1;
            pointer-events: none;
            animation: fadeSparkle 0.8s ease-out forwards;
            z-index: 999;
            transform: translate(-50%, -50%);
        }

        @keyframes fadeSparkle {
            0% { transform: translate(-50%, -50%) scale(1); opacity: 1; }
            100% { transform: translate(-50%, -50%) scale(0.2) translateY(-25px); opacity: 0; }
        }

        .click-heart {
            position: fixed;
            font-size: 1.2rem;
            pointer-events: none;
            animation: floatUpHeart 1.2s ease-out forwards;
            z-index: 99999;
            transform: translate(-50%, -50%);
        }

        @keyframes floatUpHeart {
            0% { transform: translate(-50%, -50%) scale(0.5); opacity: 1; }
            100% { transform: translate(-50%, -120px) scale(1.4); opacity: 0; }
        }

        .vault-title {
            position: fixed;
            top: 35px;
            left: 50%;
            transform: translateX(-50%);
            font-family: 'Cinzel', serif;
            font-size: 1.2rem;
            letter-spacing: 14px;
            color: #ffffff;
            text-transform: uppercase;
            z-index: 15;
            text-shadow: 0 0 30px rgba(255, 105, 180, 0.9), 0 0 10px rgba(255, 255, 255, 0.6);
            cursor: pointer;
            opacity: 0;
            transition: opacity 2s ease, transform 0.3s ease;
        }
        .vault-title:hover { transform: translateX(-50%) scale(1.05); }
        .vault-title.visible { opacity: 1; }

        .moon-quote-panel {
            position: fixed; top: 290px; left: 5%; width: 290px; max-height: 280px;
            background: rgba(35, 10, 30, 0.9); backdrop-filter: blur(25px);
            border: 2px solid rgba(255, 105, 180, 0.85); border-radius: 20px;
            padding: 22px 18px; z-index: 25;
            box-shadow: 0 25px 60px rgba(0,0,0,0.9), 0 0 45px rgba(255, 77, 148, 0.5);
            text-align: left; opacity: 0; transform: translateY(20px);
            transition: opacity 1.5s ease 0.6s, transform 0.3s cubic-bezier(0.16,1,0.3,1);
            cursor: pointer; animation: quoteShine 4s ease-in-out infinite alternate;
            display: flex; flex-direction: column; justify-content: space-between;
        }
        .moon-quote-panel.visible { opacity: 1; transform: translateY(0); }
        .moon-quote-panel:hover { box-shadow: 0 30px 75px rgba(0,0,0,0.95), 0 0 65px rgba(255,105,180,0.9); transform: translateY(-3px) scale(1.02); }
        @keyframes quoteShine {
            0% { box-shadow: 0 25px 60px rgba(0,0,0,0.9), 0 0 30px rgba(255,77,148,0.4); }
            100% { box-shadow: 0 25px 60px rgba(0,0,0,0.9), 0 0 60px rgba(255,105,180,0.85), inset 0 0 25px rgba(255,182,193,0.4); }
        }
        .moon-quote-content-wrapper { overflow-y: auto; max-height: 180px; padding-right: 4px; }
        .moon-quote-text { font-family: 'Cinzel', serif; font-size: 0.75rem; letter-spacing: 1.5px; color: #ffb6c1; line-height: 1.7; text-shadow: 0 0 12px rgba(255,182,193,0.7); transition: opacity 0.2s ease; }
        .moon-quote-author { display: block; margin-top: 10px; font-size: 0.62rem; letter-spacing: 2px; color: rgba(255,255,255,0.85); text-align: right; font-style: italic; }
        .quote-hint { display: block; margin-top: 12px; padding-top: 8px; border-top: 1px solid rgba(255,105,180,0.25); font-size: 0.52rem; letter-spacing: 2.5px; color: rgba(255,105,180,0.8); text-align: center; text-transform: uppercase; }

        .upload-memory-panel {
            position: fixed; bottom: 35px; left: 35px; width: 180px;
            background: rgba(30, 10, 24, 0.85); backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 105, 180, 0.4); border-radius: 14px;
            padding: 12px; z-index: 25; opacity: 0; transition: opacity 1.5s ease 0.6s;
            display: flex; flex-direction: column; gap: 8px;
        }
        .upload-memory-panel.visible { opacity: 1; }
        .upload-btn {
            background: rgba(255, 105, 180, 0.2); border: 1px solid rgba(255, 105, 180, 0.6);
            color: #fff; padding: 8px 10px; font-size: 0.62rem; letter-spacing: 1.5px;
            border-radius: 20px; cursor: pointer; text-transform: uppercase;
            font-family: 'Cinzel', serif; width: 100%; transition: all 0.3s;
        }
        .upload-btn:hover { background: rgba(255, 77, 148, 0.4); border-color: #ff4d94; box-shadow: 0 0 15px rgba(255,77,148,0.6); transform: translateY(-1px); }
        .upload-btn.song-btn { background: rgba(255, 182, 193, 0.12); border-color: rgba(255,182,193,0.45); }
        .song-status { font-size: 0.5rem; letter-spacing: 1px; color: rgba(255,182,193,0.55); text-align: center; line-height: 1.4; }

        .side-note-panel {
            position: fixed; top: 100px; right: 35px; width: 290px; height: 0px;
            background: rgba(30, 10, 24, 0.85); backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 105, 180, 0.4); border-radius: 18px;
            z-index: 25; box-shadow: 0 25px 60px rgba(0,0,0,0.8);
            display: flex; flex-direction: column; gap: 12px; opacity: 0; overflow: hidden;
            transition: opacity 1s ease 0.5s, height 0.8s cubic-bezier(0.16,1,0.3,1) 0.5s, padding 0.8s cubic-bezier(0.16,1,0.3,1) 0.5s;
        }
        .side-note-panel.visible { opacity: 1; height: 600px; padding: 18px; }
        .note-header { font-family: 'Cinzel', serif; font-size: 0.75rem; letter-spacing: 3px; color: #ff99cc; display: flex; justify-content: space-between; }
        .note-saved-status { font-size: 0.6rem; color: rgba(255,255,255,0.5); letter-spacing: 1px; }
        .note-title-input { background: rgba(255,255,255,0.05); border: 1px solid rgba(255,105,180,0.3); color: #ffffff; padding: 8px 12px; border-radius: 8px; font-family: 'Cinzel', serif; outline: none; width: 100%; }
        .note-textarea { background: rgba(255,255,255,0.05); border: 1px solid rgba(255,105,180,0.3); color: #EDF2F4; padding: 10px 12px; border-radius: 8px; outline: none; width: 100%; flex: 1; resize: none; font-family: 'Montserrat', sans-serif; }

        .preloader {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: #1c0616;
            z-index: 99999; display: flex; flex-direction: column; align-items: center; justify-content: center;
            transition: opacity 1.5s ease, visibility 1.5s;
        }
        .preloader-text { font-family: 'Cinzel', serif; color: rgba(255,182,193,0.85); font-size: 0.85rem; letter-spacing: 8px; margin-bottom: 25px; text-transform: uppercase; }
        .preloader-bar-container { width: 220px; height: 2px; background: rgba(255,255,255,0.1); position: relative; border-radius: 2px; overflow: hidden; }
        .preloader-bar { position: absolute; top: 0; left: 0; height: 100%; background: linear-gradient(90deg, #ff4d94, #ffb6c1); width: 0%; transition: width 0.3s ease; box-shadow: 0 0 15px rgba(255,77,148,0.9); }
        .preloader.hidden { opacity: 0; visibility: hidden; }

        .start-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(28, 6, 22, 0.96); backdrop-filter: blur(25px);
            z-index: 9998; display: flex; flex-direction: column; align-items: center; justify-content: center;
            transition: opacity 1.2s ease, visibility 1.2s;
        }
        .start-overlay.hidden { opacity: 0; visibility: hidden; pointer-events: none; }
        .pass-input {
            background: rgba(255,255,255,0.05); border: 1px solid rgba(255,105,180,0.4); color: #ffffff;
            padding: 16px 28px; font-size: 1rem; letter-spacing: 8px; border-radius: 40px; text-align: center;
            outline: none; font-family: 'Cinzel', serif; margin-bottom: 20px; width: 280px;
        }
        .error-msg { color: #ff6b81; font-size: 0.75rem; letter-spacing: 3px; margin-top: 15px; text-transform: uppercase; opacity: 0; transition: opacity 0.3s; }
        .error-msg.visible { opacity: 1; }
        .enter-btn {
            background: rgba(255,105,180,0.2); border: 1px solid rgba(255,105,180,0.6); color: #fff;
            padding: 14px 36px; font-size: 0.8rem; letter-spacing: 8px; border-radius: 40px; cursor: pointer;
            text-transform: uppercase; font-family: 'Cinzel', serif; transition: all 0.3s;
        }
        .enter-btn:hover { background: rgba(255,77,148,0.4); box-shadow: 0 0 25px rgba(255,77,148,0.6); transform: translateY(-2px); }

        .environment-wrapper {
            position: fixed; inset: 0; z-index: 1; pointer-events: none;
            transition: transform 0.4s ease-out;
        }
        .stars {
            position: absolute; inset: 0;
            background-image:
                radial-gradient(1.5px 1.5px at 20px 30px, white, transparent),
                radial-gradient(1.5px 1.5px at 40px 70px, rgba(255,255,255,0.8), transparent),
                radial-gradient(1px 1px at 90px 40px, white, transparent),
                radial-gradient(1.5px 1.5px at 160px 120px, rgba(255,255,255,0.9), transparent),
                radial-gradient(1px 1px at 230px 80px, white, transparent),
                radial-gradient(1.5px 1.5px at 300px 150px, rgba(255,255,255,0.7), transparent);
            background-size: 350px 200px;
            opacity: 0.6;
            animation: twinkle 8s ease-in-out infinite alternate;
        }
        @keyframes twinkle { 0% { opacity: 0.4; } 100% { opacity: 0.75; } }

        /* Fixed upper-right sky — outside parallax so it stays put */
        .celestial-moon {
            position: fixed;
            top: 72px;
            right: 48px;
            width: 110px;
            height: 110px;
            border-radius: 50%;
            background: radial-gradient(circle at 32% 32%, #fff8fb, #ffd0e0 35%, #ff8fb8 70%, #e85a9a 100%);
            box-shadow:
                0 0 40px rgba(255,182,193,0.7),
                0 0 90px rgba(255,105,180,0.55),
                0 0 160px rgba(255,77,148,0.35),
                inset -12px -8px 30px rgba(180,40,100,0.25);
            cursor: pointer;
            pointer-events: all;
            transition: box-shadow 0.15s ease;
            z-index: 20;
        }
        .celestial-moon::after {
            content: '';
            position: absolute; width: 14px; height: 14px; border-radius: 50%;
            background: rgba(255,200,210,0.35);
            top: 28%; left: 22%;
            box-shadow: 22px 18px 0 -2px rgba(255,180,200,0.25), 40px 8px 0 -4px rgba(255,160,190,0.2);
        }
        @media (max-width: 768px) {
            .celestial-moon { top: 64px; right: 16px; width: 72px; height: 72px; }
        }

        .scene {
            position: relative; width: 100%; height: 100vh;
            perspective: 1200px; z-index: 5;
            opacity: 0; transform: scale(0.95);
            transition: opacity 1.5s ease, transform 1.5s ease;
        }
        .scene.visible { opacity: 1; transform: scale(1); }
        .scene:active { cursor: grabbing; }

        .dna-world {
            position: absolute; left: 50%; top: 50%;
            width: 1000px; height: 1000px;
            margin-left: -500px; margin-top: -500px;
            transform-style: preserve-3d;
            transform: rotateX(-5deg) rotateY(0deg);
        }

        .dna-svg {
            position: absolute; left: 0; top: 0;
            width: 1000px; height: 1000px;
            overflow: visible; pointer-events: none;
        }
        .dna-line {
            stroke: rgba(255,105,180,0.35);
            stroke-width: 1.5;
            filter: drop-shadow(0 0 4px rgba(255,77,148,0.5));
        }

        .photo-card {
            position: absolute; left: 50%; top: 50%;
            width: 110px; height: 140px;
            margin-left: -55px; margin-top: -70px;
            border-radius: 12px; overflow: hidden;
            border: 2px solid rgba(255,105,180,0.5);
            box-shadow: 0 10px 40px rgba(0,0,0,0.6), 0 0 20px rgba(255,77,148,0.3);
            transform-style: preserve-3d;
            transition: box-shadow 0.3s, border-color 0.3s;
            cursor: pointer;
            background: #1a0a14;
        }
        .photo-card img {
            width: 100%; height: 100%; object-fit: cover;
            display: block;
        }
        .photo-card .glare {
            position: absolute; inset: 0;
            background: linear-gradient(135deg, rgba(255,255,255,0.15) 0%, transparent 50%);
            pointer-events: none;
        }
        .photo-card.focused {
            border-color: #ff4d94;
            box-shadow: 0 15px 50px rgba(0,0,0,0.8), 0 0 40px rgba(255,77,148,0.7);
            z-index: 100 !important;
        }

        .lightbox {
            position: fixed; inset: 0; z-index: 20000;
            background: rgba(12, 4, 10, 0.94); backdrop-filter: blur(20px);
            display: flex; align-items: center; justify-content: center;
            opacity: 0; pointer-events: none; transition: opacity 0.4s;
        }
        .lightbox.active { opacity: 1; pointer-events: all; }
        .lightbox-wrapper {
            display: flex; width: 90%; max-width: 980px; max-height: 85vh;
            background: rgba(30,10,24,0.95); border-radius: 24px; overflow: hidden;
            border: 1px solid rgba(255,105,180,0.4);
            box-shadow: 0 35px 100px rgba(0,0,0,0.95); transform: scale(0.92); transition: transform 0.4s;
        }
        .lightbox.active .lightbox-wrapper { transform: scale(1); }
        .lightbox-img-box { flex: 1.2; background: #000; display: flex; align-items: center; justify-content: center; padding: 20px; position: relative; }
        .lightbox-img-box img { max-width: 100%; max-height: 75vh; border-radius: 10px; object-fit: contain; }
        .photo-stamp { position: absolute; font-size: 3rem; pointer-events: none; animation: stampAnim 0.8s ease-out forwards; z-index: 100; transform: translate(-50%, -50%) scale(0.3); }
        @keyframes stampAnim {
            0% { transform: translate(-50%, -50%) scale(0.3) rotate(-20deg); opacity: 0; }
            50% { transform: translate(-50%, -50%) scale(1.3) rotate(10deg); opacity: 1; }
            100% { transform: translate(-50%, -50%) scale(1) rotate(0deg); opacity: 0; }
        }
        .lightbox-info-box { flex: 1; padding: 36px 28px; display: flex; flex-direction: column; text-align: left; background: rgba(30,10,24,0.92); gap: 14px; }
        .photo-note-title { font-family: 'Cinzel', serif; font-size: 0.9rem; letter-spacing: 4px; color: #ff99cc; text-transform: uppercase; margin-bottom: 4px; }
        .photo-note-hint { font-size: 0.6rem; letter-spacing: 1.5px; color: rgba(255,182,193,0.55); margin-bottom: 8px; }
        .photo-note-textarea {
            flex: 1; min-height: 160px; width: 100%; resize: none; outline: none;
            background: rgba(255,255,255,0.04); border: 1px solid rgba(255,105,180,0.28);
            color: #EDF2F4; padding: 14px 16px; border-radius: 12px;
            font-family: 'Montserrat', sans-serif; font-size: 0.85rem; line-height: 1.65;
        }
        .photo-note-textarea:focus { border-color: rgba(255,105,180,0.7); box-shadow: 0 0 18px rgba(255,77,148,0.25); }
        .photo-note-status { font-size: 0.58rem; letter-spacing: 2px; color: rgba(255,182,193,0.5); text-transform: uppercase; }
        .lightbox-actions { display: flex; gap: 12px; margin-top: 8px; }
        .action-btn { background: rgba(255,255,255,0.08); border: 1px solid rgba(255,105,180,0.3); color: #fff; padding: 10px 18px; font-size: 0.72rem; letter-spacing: 2px; border-radius: 20px; cursor: pointer; transition: all 0.3s; text-transform: uppercase; flex: 1; font-family: 'Cinzel', serif; }
        .action-btn:hover { background: rgba(255,77,148,0.3); border-color: #ff4d94; }
        .action-btn.danger { border-color: rgba(255,107,129,0.5); color: #ffb3bc; }
        .action-btn.danger:hover { background: rgba(255,107,129,0.25); }
        .lightbox-close { position: absolute; top: 35px; right: 45px; color: rgba(255,255,255,0.6); font-size: 0.75rem; letter-spacing: 4px; cursor: pointer; z-index: 99999; }
        .lightbox-close:hover { color: #FFFFFF; }

        .surprise-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(18,6,14,0.96); backdrop-filter: blur(30px);
            z-index: 30000; display: flex; justify-content: center; align-items: center; opacity: 0; pointer-events: none; transition: opacity 1.5s;
        }
        .surprise-overlay.active { opacity: 1; pointer-events: all; }
        .surprise-card { background: rgba(255,255,255,0.05); border: 1px solid rgba(255,105,180,0.5); padding: 48px 56px; border-radius: 24px; max-width: 560px; box-shadow: 0 35px 120px rgba(0,0,0,0.95); }
        .surprise-text { font-family: 'Cinzel', serif; font-size: 1.25rem; line-height: 2; letter-spacing: 3px; color: #FFFFFF; margin-bottom: 28px; }
        .surprise-enter-btn { background: rgba(255,105,180,0.2); border: 1px solid rgba(255,105,180,0.65); color: #fff; padding: 14px 32px; font-size: 0.75rem; letter-spacing: 3px; border-radius: 30px; cursor: pointer; text-transform: uppercase; font-family: 'Cinzel', serif; transition: all 0.3s; }
        .surprise-enter-btn:hover { background: rgba(255,77,148,0.45); transform: translateY(-2px); }

        .secret-toast {
            position: fixed; bottom: 40px; left: 50%; transform: translateX(-50%) translateY(50px);
            background: rgba(30,10,24,0.95); border: 1px solid rgba(255,105,180,0.8); color: #ff99cc;
            padding: 14px 28px; border-radius: 30px; font-family: 'Cinzel', serif; font-size: 0.8rem; letter-spacing: 3px;
            z-index: 40000; opacity: 0; transition: all 0.5s; pointer-events: none;
        }
        .secret-toast.visible { opacity: 1; transform: translateX(-50%) translateY(0); }

        .dust-wrapper { position: absolute; }
        .dust-particle {
            width: 3px; height: 3px; background: rgba(255,182,193,0.7); border-radius: 50%;
            animation: floatDust linear infinite;
            box-shadow: 0 0 6px rgba(255,105,180,0.6);
        }
        @keyframes floatDust {
            0% { transform: translateY(0) scale(1); opacity: 0; }
            10% { opacity: 0.8; }
            90% { opacity: 0.4; }
            100% { transform: translateY(-100vh) scale(0.3); opacity: 0; }
        }

        .shooting-star {
            position: absolute; width: 80px; height: 2px;
            background: linear-gradient(90deg, rgba(255,255,255,0.9), transparent);
            border-radius: 2px; opacity: 0;
            animation: shoot 1.2s ease-out forwards;
        }
        @keyframes shoot {
            0% { opacity: 1; transform: translateX(0) translateY(0); }
            100% { opacity: 0; transform: translateX(200px) translateY(80px); }
        }

        @media(max-width: 768px) {
            .lightbox-wrapper { flex-direction: column; max-height: 85vh; overflow-y: auto; }
            .lightbox-img-box { max-height: 40vh; }
            .side-note-panel, .upload-memory-panel, .moon-quote-panel { display: none; }
        }
    </style>
</head>
<body>

    <div class="cursor-light" id="cursorLight"></div>
    <div class="vault-title" id="vaultTitle">Scarlet Vault</div>

    <div class="moon-quote-panel" id="moonQuotePanel" title="Click to cycle quotes">
        <div class="moon-quote-content-wrapper">
            <div class="moon-quote-text" id="moonQuoteTextContent">
                “Don’t compare her to sunshine and roses when she’s clearly orchids and moonlight.”
                <span class="moon-quote-author">— Melody Lee</span>
            </div>
        </div>
        <span class="quote-hint">[ Click to Cycle ]</span>
    </div>

    <div class="upload-memory-panel" id="uploadMemoryPanel">
        <input type="file" id="memoryFileInput" accept="image/*" style="display: none;">
        <button class="upload-btn" id="uploadTriggerBtn">Add Photo</button>
    </div>

    <div class="secret-toast" id="secretToast">a quiet secret, kept for you</div>

    <div class="side-note-panel" id="sideNotePanel">
        <div class="note-header">
            <span>Secret Journal</span>
            <span class="note-saved-status" id="saveStatus">Saved</span>
        </div>
        <input type="text" id="noteTitle" class="note-title-input" placeholder="Title..." autocomplete="off">
        <textarea id="noteBody" class="note-textarea" placeholder="whatever is sitting heavy or soft in your chest — leave it here..."></textarea>
    </div>

    <div class="preloader" id="preloader">
        <div class="preloader-text">gathering memories</div>
        <div class="preloader-bar-container">
            <div class="preloader-bar" id="preloaderBar"></div>
        </div>
    </div>

    <div class="start-overlay" id="startOverlay">
        <div id="passStep" style="display: flex; flex-direction: column; align-items: center;">
            <div style="font-family: 'Cinzel', serif; font-size: 0.85rem; letter-spacing: 6px; color: rgba(255,255,255,0.7); margin-bottom: 22px; text-transform: uppercase;">a word only we know</div>
            <input type="password" id="passInput" class="pass-input" placeholder="••••••" autocomplete="off">
            <button class="enter-btn" id="passBtn">Open</button>
            <div class="error-msg" id="errorMsg">not that one</div>
        </div>
        <div id="enterStep" style="display: none; flex-direction: column; align-items: center;">
            <button class="enter-btn" id="enterBtn">come in</button>
        </div>
    </div>

    <!-- Keep lover.mp3 next to this HTML file -->
    <audio id="bgMusic" src="lover.mp3" loop preload="auto"></audio>

    <div class="celestial-moon" id="celestialMoon" title="tap me"></div>

    <div class="environment-wrapper" id="environment">
        <div class="stars"></div>
        <div class="ambient-orb" style="top: 20%; left: 15%; width: 350px; height: 350px; position: absolute; border-radius: 50%; background: radial-gradient(circle, rgba(255,105,180,0.25) 0%, transparent 70%); filter: blur(15px);"></div>
        <div class="ambient-orb" style="bottom: 10%; right: 18%; width: 400px; height: 400px; position: absolute; border-radius: 50%; background: radial-gradient(circle, rgba(255,105,180,0.25) 0%, transparent 70%); filter: blur(15px);"></div>
        <div id="shootingStarContainer"></div>
        <div id="dustContainer"></div>
    </div>

    <div class="scene" id="scene">
        <div class="dna-world" id="dnaWorld">
            <svg class="dna-svg" id="dnaSvg"></svg>
        </div>
    </div>

    <div class="lightbox" id="lightbox">
        <div class="lightbox-close" id="lightboxClose">Close [X]</div>
        <div class="lightbox-wrapper">
            <div class="lightbox-img-box" id="lightboxImgBox">
                <img id="lightboxImg" src="" alt="Memory">
            </div>
            <div class="lightbox-info-box">
                <div class="photo-note-title">A note for this memory</div>
                <div class="photo-note-hint">write whatever you feel — it stays with this photo</div>
                <textarea id="photoNoteInput" class="photo-note-textarea" placeholder="Something only this frame would understand..."></textarea>
                <div class="photo-note-status" id="photoNoteStatus">unsaved</div>
                <div class="lightbox-actions">
                    <button class="action-btn" id="nextBtn">Next memory</button>
                    <button class="action-btn danger" id="removePhotoBtn">Remove</button>
                </div>
            </div>
        </div>
    </div>

    <div class="surprise-overlay" id="surpriseOverlay">
        <div class="surprise-card">
            <div class="surprise-text" id="surpriseText">built this little universe for you.<br>welcome home.</div>
            <button class="surprise-enter-btn" id="surpriseEnterBtn">step inside</button>
        </div>
    </div>

    <script>
        // ========== CONFIG ==========
        const VAULT_PASSCODE = 'black';
        const AUTO_DISMISS_PRELOADER_MS = 3000;
        // ============================

        // No preloaded photos — start empty; add your own
        const defaultImageFiles = [];

        let storedCustomPhotos = JSON.parse(localStorage.getItem('scarlet_custom_photos') || '[]');
        let imageFiles = defaultImageFiles.concat(storedCustomPhotos);

        const interactiveQuotes = [
            { text: "“Don’t compare her to sunshine and roses when she’s clearly orchids and moonlight.”", author: "— Melody Lee" },
            { text: "“Tell me the story of how the sun loved the moon so much he died every night to let her breathe.”", author: "— Hanako Ishii" },
            { text: "“You are my sun, my moon, and all my stars.”", author: "— E.E. Cummings" },
            { text: "“The moon is a friend for the lonesome to talk to.”", author: "— Carl Sandburg" },
            { text: "“Moonlight drowns out all but the brightest stars.”", author: "— J.R.R. Tolkien" },
            { text: "“We ran as if to meet the moon.”", author: "— Robert Frost" },
            { text: "“There is something beautiful and calm about the moon, like a promise.”", author: "— Shannon A. Thompson" },
            { text: "“Three things cannot be long hidden: the sun, the moon, and the truth.”", author: "— Buddha" }
        ];

        let currentQuoteIdx = 0;
        const moonQuotePanel = document.getElementById('moonQuotePanel');
        const moonQuoteTextContent = document.getElementById('moonQuoteTextContent');

        moonQuotePanel.addEventListener('click', (e) => {
            const sparkle = document.createElement('div');
            sparkle.className = 'cursor-sparkle';
            sparkle.style.left = e.clientX + 'px';
            sparkle.style.top = e.clientY + 'px';
            document.body.appendChild(sparkle);
            setTimeout(() => sparkle.remove(), 800);

            currentQuoteIdx = (currentQuoteIdx + 1) % interactiveQuotes.length;
            moonQuoteTextContent.style.opacity = '0';
            setTimeout(() => {
                moonQuoteTextContent.innerHTML = `${interactiveQuotes[currentQuoteIdx].text}<span class="moon-quote-author">${interactiveQuotes[currentQuoteIdx].author}</span>`;
                moonQuoteTextContent.style.opacity = '1';
            }, 200);
        });

        const dnaWorld = document.getElementById('dnaWorld');
        const dnaSvg = document.getElementById('dnaSvg');
        const cardsData = [];
        let currentIndex = 0;

        let photoNotes = JSON.parse(localStorage.getItem('scarlet_photo_notes') || '{}');
        function noteKey(src) {
            return (src || '').slice(0, 120);
        }

        const themes = [
            { c1: '#1c0616', c2: '#3b0a29', c3: '#610f41', c4: '#120320' },
            { c1: '#0a0d20', c2: '#1b103b', c3: '#381661', c4: '#060212' },
            { c1: '#041c1c', c2: '#0b3b38', c3: '#106155', c4: '#021012' }
        ];
        let currentThemeIndex = 0;
        document.getElementById('vaultTitle').addEventListener('click', () => {
            currentThemeIndex = (currentThemeIndex + 1) % themes.length;
            const t = themes[currentThemeIndex];
            document.documentElement.style.setProperty('--theme-grad-1', t.c1);
            document.documentElement.style.setProperty('--theme-grad-2', t.c2);
            document.documentElement.style.setProperty('--theme-grad-3', t.c3);
            document.documentElement.style.setProperty('--theme-grad-4', t.c4);
        });

        const uploadTriggerBtn = document.getElementById('uploadTriggerBtn');
        const memoryFileInput = document.getElementById('memoryFileInput');
        const bgMusic = document.getElementById('bgMusic');

        uploadTriggerBtn.addEventListener('click', () => memoryFileInput.click());

        memoryFileInput.addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(event) {
                const newSrc = event.target.result;
                storedCustomPhotos.push(newSrc);
                localStorage.setItem('scarlet_custom_photos', JSON.stringify(storedCustomPhotos));
                addNewPhotoCard(newSrc);
            };
            reader.readAsDataURL(file);
            e.target.value = '';
        });

        function rebuildDnaLines() {
            dnaSvg.innerHTML = '';
            for (let i = 0; i < cardsData.length; i += 2) {
                if (i + 1 < cardsData.length) {
                    const line = document.createElementNS('http://www.w3.org/2000/svg', 'line');
                    line.setAttribute('class', 'dna-line');
                    line.setAttribute('data-i', i);
                    line.setAttribute('data-j', i + 1);
                    dnaSvg.appendChild(line);
                }
            }
        }

        function layoutCards() {
            const total = Math.max(cardsData.length, 1);
            cardsData.forEach((c, idx) => {
                const t = (idx / total) * Math.PI * 3.5;
                const strand = idx % 2;
                const radius = 220;
                c.angleOffset = t + (strand * Math.PI);
                c.hx = Math.cos(c.angleOffset) * radius;
                c.hz = Math.sin(c.angleOffset) * radius;
                c.hy = (idx - total / 2) * 38;
            });
            rebuildDnaLines();
        }

        function addNewPhotoCard(src) {
            const index = cardsData.length;
            const card = document.createElement('div');
            card.classList.add('photo-card');
            const glare = document.createElement('div');
            glare.classList.add('glare');
            const imageEl = document.createElement('img');
            imageEl.src = src;

            card.appendChild(imageEl);
            card.appendChild(glare);
            dnaWorld.appendChild(card);

            cardsData.push({ el: card, glareEl: glare, hx: 0, hy: 0, hz: 0, angleOffset: 0, src });

            card.addEventListener('mouseenter', () => card.classList.add('focused'));
            card.addEventListener('mouseleave', () => card.classList.remove('focused'));
            card.addEventListener('click', (e) => {
                e.stopPropagation();
                if (Math.abs(velX) > 0.5 || Math.abs(velY) > 0.5) return;
                openLightbox(cardsData.length - 1);
            });

            layoutCards();
        }

        function removePhotoAt(index) {
            if (index < 0 || index >= cardsData.length) return;
            const data = cardsData[index];
            data.el.remove();
            cardsData.splice(index, 1);

            // Also remove from custom photos storage if present
            const customIdx = storedCustomPhotos.indexOf(data.src);
            if (customIdx !== -1) {
                storedCustomPhotos.splice(customIdx, 1);
                localStorage.setItem('scarlet_custom_photos', JSON.stringify(storedCustomPhotos));
            }
            const key = noteKey(data.src);
            if (photoNotes[key]) {
                delete photoNotes[key];
                localStorage.setItem('scarlet_photo_notes', JSON.stringify(photoNotes));
            }

            layoutCards();
            if (!cardsData.length) {
                closeLightbox();
                return;
            }
            if (lightboxOpen) {
                currentIndex = Math.min(index, cardsData.length - 1);
                openLightbox(currentIndex);
            }
        }

        const noteTitleInput = document.getElementById('noteTitle');
        const noteBodyInput = document.getElementById('noteBody');
        const saveStatus = document.getElementById('saveStatus');

        if (localStorage.getItem('scarletNoteTitle')) noteTitleInput.value = localStorage.getItem('scarletNoteTitle');
        if (localStorage.getItem('scarletNoteBody')) noteBodyInput.value = localStorage.getItem('scarletNoteBody');

        let saveTimeout;
        function handleNoteInput() {
            localStorage.setItem('scarletNoteTitle', noteTitleInput.value);
            localStorage.setItem('scarletNoteBody', noteBodyInput.value);
            saveStatus.innerText = 'Saving…';
            clearTimeout(saveTimeout);
            saveTimeout = setTimeout(() => { saveStatus.innerText = 'Saved'; }, 600);
        }
        noteTitleInput.addEventListener('input', handleNoteInput);
        noteBodyInput.addEventListener('input', handleNoteInput);

        const celestialMoon = document.getElementById('celestialMoon');
        const secretToast = document.getElementById('secretToast');
        let moonClickCount = 0, moonTimer = null;

        celestialMoon.addEventListener('click', () => {
            moonClickCount++;
            celestialMoon.style.transition = 'transform 0.2s ease';
            celestialMoon.style.transform = `scale(0.88) rotate(${moonClickCount * 20}deg)`;
            setTimeout(() => {
                celestialMoon.style.transform = '';
            }, 220);

            if (moonClickCount === 5) {
                secretToast.classList.add('visible');
                setTimeout(() => secretToast.classList.remove('visible'), 4000);
                moonClickCount = 0;
            }
            clearTimeout(moonTimer);
            moonTimer = setTimeout(() => { moonClickCount = 0; }, 1500);
        });

        const cursorLight = document.getElementById('cursorLight');
        let lastSparkleTime = 0;

        document.addEventListener('mousemove', (e) => {
            cursorLight.style.left = e.clientX + 'px';
            cursorLight.style.top = e.clientY + 'px';
            const now = Date.now();
            if (now - lastSparkleTime > 60) {
                lastSparkleTime = now;
                const sparkle = document.createElement('div');
                sparkle.className = 'cursor-sparkle';
                sparkle.style.left = e.clientX + (Math.random() * 20 - 10) + 'px';
                sparkle.style.top = e.clientY + (Math.random() * 20 - 10) + 'px';
                document.body.appendChild(sparkle);
                setTimeout(() => sparkle.remove(), 800);
            }
        });

        document.addEventListener('click', (e) => {
            if (e.target.closest('.lightbox') || e.target.closest('button') || e.target.closest('input') || e.target.closest('textarea')) return;
            const heart = document.createElement('div');
            heart.className = 'click-heart';
            heart.innerHTML = '💕';
            heart.style.left = e.clientX + 'px';
            heart.style.top = e.clientY + 'px';
            document.body.appendChild(heart);
            setTimeout(() => heart.remove(), 1200);
        });

        const shootingStarContainer = document.getElementById('shootingStarContainer');
        function spawnShootingStar() {
            const star = document.createElement('div');
            star.className = 'shooting-star';
            star.style.left = Math.random() * 70 + '%';
            star.style.top = Math.random() * 40 + '%';
            shootingStarContainer.appendChild(star);
            setTimeout(() => star.remove(), 1300);
        }
        setInterval(spawnShootingStar, 4000);

        const dustContainer = document.getElementById('dustContainer');
        for (let i = 0; i < 100; i++) {
            let wrapper = document.createElement('div');
            wrapper.className = 'dust-wrapper';
            wrapper.style.left = Math.random() * 100 + '%';
            wrapper.style.top = Math.random() * 100 + '%';
            let particle = document.createElement('div');
            particle.className = 'dust-particle';
            particle.style.animationDuration = (8 + Math.random() * 12) + 's';
            particle.style.animationDelay = (Math.random() * 10) + 's';
            wrapper.appendChild(particle);
            dustContainer.appendChild(wrapper);
        }

        let audioCtx, analyser, dataArray, audioInitialized = false;

        function initAudio() {
            if (audioInitialized) return;
            try {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                analyser = audioCtx.createAnalyser();
                analyser.fftSize = 256;
                dataArray = new Uint8Array(analyser.frequencyBinCount);
                const source = audioCtx.createMediaElementSource(bgMusic);
                source.connect(analyser);
                analyser.connect(audioCtx.destination);
                audioInitialized = true;
            } catch (e) {
                console.log('Audio analyser unavailable.', e);
            }
        }

        let loadedCount = 0;
        const totalPhotos = Math.max(imageFiles.length, 1);
        const preloaderBar = document.getElementById('preloaderBar');
        let preloaderFinished = false;

        function dismissPreloader() {
            if (preloaderFinished) return;
            preloaderFinished = true;
            document.getElementById('preloader').classList.add('hidden');
            document.getElementById('vaultTitle').classList.add('visible');
            document.getElementById('sideNotePanel').classList.add('visible');
            document.getElementById('moonQuotePanel').classList.add('visible');
            document.getElementById('uploadMemoryPanel').classList.add('visible');
        }

        setTimeout(dismissPreloader, AUTO_DISMISS_PRELOADER_MS);

        if (imageFiles.length === 0) {
            preloaderBar.style.width = '100%';
            setTimeout(dismissPreloader, 400);
        } else {
            Promise.all(imageFiles.map((src, index) => {
                return new Promise(resolve => {
                    const img = new Image();
                    img.src = src;
                    const handleImageComplete = () => {
                        loadedCount++;
                        preloaderBar.style.width = (loadedCount / totalPhotos) * 100 + '%';
                        addNewPhotoCard(src);
                        // Fix click index after batch load
                        const cardIndex = cardsData.length - 1;
                        const card = cardsData[cardIndex].el;
                        card.onclick = null;
                        card.addEventListener('click', (e) => {
                            e.stopPropagation();
                            if (Math.abs(velX) > 0.5 || Math.abs(velY) > 0.5) return;
                            openLightbox(cardIndex);
                        });
                        resolve();
                    };
                    img.onload = handleImageComplete;
                    img.onerror = handleImageComplete;
                });
            })).then(() => {
                layoutCards();
                setTimeout(dismissPreloader, 400);
            });
        }

        const passInput = document.getElementById('passInput');
        const passBtn = document.getElementById('passBtn');
        const passStep = document.getElementById('passStep');
        const enterStep = document.getElementById('enterStep');
        const errorMsg = document.getElementById('errorMsg');

        function verifyPasscode() {
            if (passInput.value.trim().toUpperCase() === VAULT_PASSCODE.toUpperCase()) {
                errorMsg.classList.remove('visible');
                passStep.style.opacity = '0';
                setTimeout(() => {
                    passStep.style.display = 'none';
                    enterStep.style.display = 'flex';
                    enterStep.style.opacity = '0';
                    setTimeout(() => { enterStep.style.opacity = '1'; }, 50);
                }, 400);
            } else {
                errorMsg.classList.add('visible');
                passInput.style.borderColor = '#ff6b81';
                setTimeout(() => { passInput.style.borderColor = 'rgba(255,105,180,0.4)'; }, 1200);
            }
        }

        passBtn.addEventListener('click', verifyPasscode);
        passInput.addEventListener('keypress', (e) => { if (e.key === 'Enter') verifyPasscode(); });
        passInput.addEventListener('input', () => {
            errorMsg.classList.remove('visible');
            passInput.style.borderColor = 'rgba(255,105,180,0.4)';
        });

        document.getElementById('enterBtn').addEventListener('click', () => {
            document.getElementById('startOverlay').classList.add('hidden');
            document.getElementById('scene').classList.add('visible');
            initAudio();
            if (audioCtx && audioCtx.state === 'suspended') {
                audioCtx.resume().catch(() => {});
            }
            try {
                bgMusic.volume = 0.55;
                bgMusic.play().catch(err => console.log('Audio play restricted:', err));
            } catch (e) {}

            if (!sessionStorage.getItem('welcomed')) {
                sessionStorage.setItem('welcomed', 'true');
                setTimeout(() => { document.getElementById('surpriseOverlay').classList.add('active'); }, 700);
            }
        });

        document.getElementById('surpriseEnterBtn').addEventListener('click', () => {
            document.getElementById('surpriseOverlay').classList.remove('active');
        });

        const lightbox = document.getElementById('lightbox');
        const lightboxImg = document.getElementById('lightboxImg');
        const lightboxImgBox = document.getElementById('lightboxImgBox');
        const lightboxClose = document.getElementById('lightboxClose');
        const nextBtn = document.getElementById('nextBtn');
        const removePhotoBtn = document.getElementById('removePhotoBtn');
        const photoNoteInput = document.getElementById('photoNoteInput');
        const photoNoteStatus = document.getElementById('photoNoteStatus');
        let lightboxOpen = false;
        let noteSaveTimer = null;

        lightboxImg.addEventListener('dblclick', (e) => {
            const rect = lightboxImgBox.getBoundingClientRect();
            const stamp = document.createElement('div');
            stamp.className = 'photo-stamp';
            stamp.innerHTML = '💖';
            stamp.style.left = (e.clientX - rect.left) + 'px';
            stamp.style.top = (e.clientY - rect.top) + 'px';
            lightboxImgBox.appendChild(stamp);
            setTimeout(() => stamp.remove(), 800);
        });

        function openLightbox(index) {
            if (!cardsData.length) return;
            currentIndex = index;
            const data = cardsData[index];
            lightboxImg.src = data.src;

            const key = noteKey(data.src);
            photoNoteInput.value = photoNotes[key] || '';
            photoNoteStatus.textContent = photoNotes[key] ? 'saved with this memory' : 'write something if you want';

            lightbox.classList.add('active');
            lightboxOpen = true;
        }

        function closeLightbox() {
            lightbox.classList.remove('active');
            setTimeout(() => { lightboxImg.src = ''; }, 400);
            lightboxOpen = false;
        }

        photoNoteInput.addEventListener('input', () => {
            photoNoteStatus.textContent = 'saving…';
            clearTimeout(noteSaveTimer);
            noteSaveTimer = setTimeout(() => {
                const data = cardsData[currentIndex];
                if (!data) return;
                const key = noteKey(data.src);
                const text = photoNoteInput.value;
                if (text.trim()) photoNotes[key] = text;
                else delete photoNotes[key];
                localStorage.setItem('scarlet_photo_notes', JSON.stringify(photoNotes));
                photoNoteStatus.textContent = 'saved with this memory';
            }, 400);
        });

        lightboxClose.addEventListener('click', closeLightbox);
        lightbox.addEventListener('click', (e) => { if (e.target === lightbox) closeLightbox(); });
        nextBtn.addEventListener('click', () => {
            if (!cardsData.length) return;
            currentIndex = (currentIndex + 1) % cardsData.length;
            openLightbox(currentIndex);
        });
        removePhotoBtn.addEventListener('click', () => {
            removePhotoAt(currentIndex);
        });

        const environment = document.getElementById('environment');
        const scene = document.getElementById('scene');

        document.addEventListener('mousemove', (e) => {
            if (lightboxOpen) return;
            const x = (e.clientX / window.innerWidth - 0.5) * 2;
            const y = (e.clientY / window.innerHeight - 0.5) * 2;
            environment.style.transform = `translate(${x * -18}px, ${y * -18}px)`;
            scene.style.transform = `rotateX(${y * -4}deg) rotateY(${x * 4}deg)`;
        });

        let isDragging = false, previousX = 0, previousY = 0;
        let rotX = -5, rotY = 0, velX = 0.45, velY = 0;
        const friction = 0.94;

        // Beat-driven helix state (Lover ~ BPM  ~92–96 mid-tempo)
        let beatPulse = 0;
        let smoothBass = 0;
        let smoothMid = 0;
        let lastBeatTime = 0;
        let beatEnergy = 0;

        function render() {
            let bass = 0, mid = 0, high = 0, overall = 0;

            if (audioInitialized && !bgMusic.paused && dataArray) {
                try {
                    analyser.getByteFrequencyData(dataArray);
                    const n = dataArray.length;
                    // Low bins = kick / bass pulse
                    let bassSum = 0, midSum = 0, highSum = 0;
                    const bassEnd = Math.floor(n * 0.08);
                    const midEnd = Math.floor(n * 0.35);
                    for (let i = 0; i < n; i++) {
                        const v = dataArray[i];
                        overall += v;
                        if (i < bassEnd) bassSum += v;
                        else if (i < midEnd) midSum += v;
                        else highSum += v;
                    }
                    bass = bassSum / Math.max(1, bassEnd) / 255;
                    mid = midSum / Math.max(1, midEnd - bassEnd) / 255;
                    high = highSum / Math.max(1, n - midEnd) / 255;
                    overall = overall / n / 255;

                    // Simple onset / beat detection on bass
                    const now = performance.now();
                    if (bass > 0.55 && bass > smoothBass * 1.18 && now - lastBeatTime > 280) {
                        lastBeatTime = now;
                        beatPulse = 1;
                        beatEnergy = 1;
                        // Nudge spin so helix "hits" with the beat
                        if (!isDragging) velX += 0.12 + bass * 0.25;
                    }

                    smoothBass += (bass - smoothBass) * 0.22;
                    smoothMid += (mid - smoothMid) * 0.18;
                    beatPulse *= 0.88;
                    beatEnergy *= 0.92;

                    document.documentElement.style.setProperty('--audio-glow', String(smoothBass));
                    document.documentElement.style.setProperty('--audio-scale', String(overall));

                    // Moon glow follows the bass (position stays fixed in the sky)
                    const moonGlow = 50 + smoothBass * 90 + beatPulse * 50;
                    celestialMoon.style.boxShadow =
                        `0 0 ${moonGlow * 0.55}px rgba(255,182,193,0.75),` +
                        `0 0 ${moonGlow}px rgba(255,105,180,0.55),` +
                        `0 0 ${moonGlow * 1.7}px rgba(255,77,148,0.35),` +
                        `inset -12px -8px 30px rgba(180,40,100,0.25)`;
                } catch (e) {}
            }

            if (!isDragging && !lightboxOpen) {
                // Idle spin speed follows song energy
                const targetIdle = 0.28 + smoothMid * 0.55 + beatEnergy * 0.2;
                velX *= friction;
                velY *= friction;
                if (Math.abs(velX) < 0.04 && Math.abs(velY) < 0.04) velX = targetIdle;
            }
            if (rotX > 45) { rotX = 45; velY = 0; }
            if (rotX < -45) { rotX = -45; velY = 0; }

            rotY += velX;
            rotX -= velY;

            let currentSpeed = Math.hypot(velX, velY);
            let targetWarp = 1 + Math.min(currentSpeed * 0.4 + smoothBass * 2, 6);
            let currentWarp = parseFloat(getComputedStyle(document.documentElement).getPropertyValue('--warp-intensity')) || 1;
            document.documentElement.style.setProperty('--warp-intensity', currentWarp + (targetWarp - currentWarp) * 0.1);

            dnaWorld.style.transform = `rotateX(${rotX}deg) rotateY(${rotY}deg)`;

            // Helix expands on bass hits, contracts between
            const audioScaleFactor = 1 + smoothBass * 0.22 + beatPulse * 0.14;
            const cardPulse = 1 + beatPulse * 0.06 + smoothMid * 0.04;

            const radX = rotX * Math.PI / 180;
            const radY = rotY * Math.PI / 180;
            const projectedCoords = [];

            cardsData.forEach((c, idx) => {
                let x1 = c.hx * audioScaleFactor;
                let y1 = c.hy * audioScaleFactor;
                let z1 = c.hz * audioScaleFactor;

                let y2 = y1 * Math.cos(radX) - z1 * Math.sin(radX);
                let z2 = y1 * Math.sin(radX) + z1 * Math.cos(radX);
                let x2 = x1;

                let x3 = x2 * Math.cos(radY) + z2 * Math.sin(radY);
                let z3 = -x2 * Math.sin(radY) + z2 * Math.cos(radY);

                projectedCoords[idx] = { x: 500 + x3, y: 500 + y2, z: z3 };
                c.el.style.zIndex = Math.floor(z3 + 300);

                const cardRotY = (-c.angleOffset - (rotY * Math.PI / 180)) * 180 / Math.PI + 90;
                c.el.style.transform =
                    `translate3d(${x1}px, ${y1}px, ${z1}px) rotateY(${cardRotY}deg) scale(${cardPulse})`;
                c.el.style.boxShadow =
                    `0 10px 40px rgba(0,0,0,0.6), 0 0 ${18 + smoothBass * 40 + beatPulse * 30}px rgba(255,77,148,${0.25 + smoothBass * 0.45})`;
            });

            const lines = dnaSvg.querySelectorAll('line');
            lines.forEach(line => {
                const i = line.getAttribute('data-i');
                const j = line.getAttribute('data-j');
                const p1 = projectedCoords[i];
                const p2 = projectedCoords[j];
                if (p1 && p2) {
                    line.setAttribute('x1', p1.x);
                    line.setAttribute('y1', p1.y);
                    line.setAttribute('x2', p2.x);
                    line.setAttribute('y2', p2.y);
                    const avgZ = (p1.z + p2.z) / 2;
                    const baseOp = Math.max(0.15, Math.min(0.7, (avgZ + 250) / 500));
                    line.style.opacity = Math.min(1, baseOp + smoothBass * 0.35 + beatPulse * 0.25);
                    line.setAttribute('stroke-width', String(1.2 + smoothBass * 2.2 + beatPulse * 1.5));
                }
            });

            requestAnimationFrame(render);
        }
        render();

        scene.addEventListener('mousedown', (e) => {
            if (lightboxOpen) return;
            isDragging = true; velX = 0; velY = 0;
            previousX = e.clientX; previousY = e.clientY;
        });
        document.addEventListener('mouseup', () => { isDragging = false; });
        document.addEventListener('mousemove', (e) => {
            if (!isDragging) return;
            const deltaX = e.clientX - previousX;
            const deltaY = e.clientY - previousY;
            previousX = e.clientX; previousY = e.clientY;
            velX = deltaX * 0.6; velY = deltaY * 0.6;
        });

        scene.addEventListener('touchstart', (e) => {
            if (lightboxOpen) return;
            isDragging = true; velX = 0; velY = 0;
            previousX = e.touches[0].clientX; previousY = e.touches[0].clientY;
        }, { passive: true });
        document.addEventListener('touchend', () => { isDragging = false; });
        document.addEventListener('touchmove', (e) => {
            if (!isDragging) return;
            const deltaX = e.touches[0].clientX - previousX;
            const deltaY = e.touches[0].clientY - previousY;
            previousX = e.touches[0].clientX; previousY = e.touches[0].clientY;
            velX = deltaX * 0.6; velY = deltaY * 0.6;
        }, { passive: true });
    </script>
</body>
</html>
