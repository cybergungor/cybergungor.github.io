---
layout: null
permalink: /admin/
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>⚠️ SECURITY BREACH DETECTED ⚠️</title>
    <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        body, html { margin: 0; padding: 0; height: 100%; overflow: hidden; background: #000; font-family: 'JetBrains Mono', monospace; }

        /* --- FAZ 1: KIRMIZI ALARM (Terminal) --- */
        #alarm-state {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            z-index: 999; background: rgba(20, 0, 0, 0.95);
        }
        .breach-title {
            font-size: 3rem; color: #ff0000; text-shadow: 0 0 20px #ff0000, 0 0 40px #ff0000;
            text-align: center; font-weight: 900; letter-spacing: 5px;
            animation: pulseRed 0.5s infinite alternate;
        }
        .fake-terminal {
            width: 85%; max-width: 800px; background: rgba(0,0,0,0.95);
            border: 2px solid #ff0000; padding: 25px; margin-top: 30px;
            height: 300px; overflow: hidden; box-shadow: 0 0 50px rgba(255,0,0,0.3);
        }
        .log-entry { color: #ff4444; font-size: 0.9rem; margin-bottom: 8px; }

        /* --- FAZ 2: ERIŞIM REDDEDİLDİ (Glitch Ekranı) --- */
        #joke-state {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: #050608; flex-direction: column; align-items: center; justify-content: center;
            z-index: 1000; color: #fff;
            /* Arka plana hafif kırmızı alarm ışığı */
            box-shadow: inset 0 0 200px rgba(255, 0, 0, 0.2);
        }

        /* Glitch Başlık */
        .access-denied-title {
            font-size: 5rem; font-weight: 900; color: #ff0000;
            text-shadow: 2px 2px 0px #00f2ff, -2px -2px 0px #ff0000;
            animation: glitchTitle 1s infinite linear alternate-reverse;
            text-align: center; line-height: 1; margin-bottom: 30px;
        }
        @keyframes glitchTitle {
            0% { text-shadow: 4px 4px 0px #00f2ff, -4px -4px 0px #ff0000; transform: skewX(0deg); }
            20% { text-shadow: 2px -2px 0px #00f2ff, -2px 2px 0px #ff0000; transform: skewX(-5deg); }
            40% { text-shadow: -2px 2px 0px #00f2ff, 2px -2px 0px #ff0000; transform: skewX(5deg); }
            100% { text-shadow: 2px 2px 0px #00f2ff, -2px -2px 0px #ff0000; transform: skewX(0deg); }
        }

        /* GIF Konteyner (Artık boş görünmeyecek) */
        .joke-media-container {
            border: 3px solid #ff0000; box-shadow: 0 0 50px rgba(255,0,0,0.5);
            border-radius: 4px; overflow: hidden; width: 90%; max-width: 500px;
            position: relative; background: #000;
        }
        /* GIF'in üzerine gelen tarama çizgileri */
        .joke-media-container::after {
            content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: repeating-linear-gradient(0deg, rgba(0,0,0,0.3) 0px, rgba(0,0,0,0.3) 2px, transparent 2px, transparent 4px);
            pointer-events: none;
        }
        .joke-media { width: 100%; display: block; opacity: 0.9; }

        .joke-subtext { color: #ff4444; font-size: 1.2rem; margin-top: 25px; font-weight: bold; letter-spacing: 2px; }

        .go-home-btn {
            margin-top: 45px; padding: 20px 60px;
            border: 2px solid #ff0000; color: #ff0000; text-decoration: none;
            font-weight: 800; transition: 0.3s; background: rgba(255, 0, 0, 0.1);
            letter-spacing: 3px; font-size: 1rem;
        }
        .go-home-btn:hover { background: #ff0000; color: #000; box-shadow: 0 0 50px #ff0000; }

        @media (max-width: 768px) { .access-denied-title { font-size: 3rem; } }
    </style>
</head>
<body>

    <div id="alarm-state">
        <h1 class="breach-title">⚠️ SECURITY_BREACH ⚠️</h1>
        <div class="fake-terminal" id="terminal-logs"></div>
    </div>

    <div id="joke-state">
        <h1 class="access-denied-title">ACCESS DENIED</h1>
        
        <div class="joke-media-container">
            <img src="https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExYjZ5Ymx4YmZ5YmZ5YmZ5YmZ5YmZ5YmZ5YmZ5YmZ5YmZ5YmZ5YmZ5YSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/fbHk008g9FYOI/giphy.gif" class="joke-media" alt="Magic Word GIF">
        </div>
        
        <p class="joke-subtext">"YOU DIDN'T SAY THE MAGIC WORD!"</p>
        
        <a href="/" class="go-home-btn">CD /HOME</a>
    </div>

    <script>
        const terminalLogs = document.getElementById('terminal-logs');
        const alarmState = document.getElementById('alarm-state');
        const jokeState = document.getElementById('joke-state');

        const getRandomIP = () => Array(4).fill(0).map(() => Math.floor(Math.random() * 255)).join('.');

        const logs = [
            `[CRITICAL] UNAUTHORIZED ACCESS attempt from: ${getRandomIP()}`,
            "[SOC] ACTIVATING LAYER-7 DEFENSE PROTOCOLS...",
            `[TRACE] HOP 1: node-${Math.floor(Math.random()*999)}.isp.net`,
            `[TRACE] HOP 2: gateway-${Math.floor(Math.random()*999)}.secure-cloud.com`,
            "[INFO] User fingerprinting initiating...",
            "[WARN] Admin credentials not found. Flagging session as HOSTILE.",
            "[SYSTEM] Locking down terminal and deploying countermeasures...",
            "[ALERT] PREPARING NEURAL SHOCK... (Just Kidding)"
        ];

        let logIndex = 0;

        function startAlarm() {
            const logInterval = setInterval(() => {
                if (logIndex < logs.length) {
                    const newLine = document.createElement('div');
                    newLine.className = 'log-entry';
                    newLine.innerText = `> ${logs[logIndex]}`;
                    terminalLogs.appendChild(newLine);
                    terminalLogs.scrollTop = terminalLogs.scrollHeight;
                    logIndex++;
                } else {
                    clearInterval(logInterval);
                    setTimeout(revealJoke, 1500);
                }
            }, 600);
        }

        function revealJoke() {
            alarmState.style.display = 'none';
            jokeState.style.display = 'flex';
        }

        window.onload = startAlarm;
    </script>

</body>
</html>
