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
    <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body, html { margin: 0; padding: 0; height: 100%; overflow: hidden; background: #000; font-family: 'JetBrains Mono', monospace; }

        /* FAZ 1: ALARM */
        #alarm-state {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            z-index: 999; animation: redAlert 0.4s infinite alternate;
        }

        @keyframes redAlert {
            0% { background: rgba(255, 0, 0, 0.05); }
            100% { background: rgba(255, 0, 0, 0.3); }
        }

        .breach-title {
            font-size: 3rem; color: #ff0000; text-shadow: 0 0 20px #ff0000;
            text-align: center; font-weight: 900; letter-spacing: 5px;
        }

        .fake-terminal {
            width: 85%; max-width: 800px; background: rgba(0,0,0,0.9);
            border: 1px solid #ff0000; padding: 25px; margin-top: 30px;
            height: 250px; overflow: hidden; box-shadow: 0 0 50px rgba(255,0,0,0.2);
        }
        .log-entry { color: #ff4444; font-size: 0.85rem; margin-bottom: 8px; }

        /* FAZ 2: JOKE (Başlangıçta gizli) */
        #joke-state {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: #050608; flex-direction: column; align-items: center; justify-content: center;
            z-index: 1000; color: #00f2ff;
        }

        .joke-media {
            border: 2px solid #00f2ff; box-shadow: 0 0 30px #00f2ff;
            border-radius: 8px; max-width: 400px; width: 90%;
        }

        .joke-text { margin-top: 35px; font-size: 1.8rem; text-align: center; font-weight: bold; }
        .joke-subtext { color: #8b949e; font-size: 1rem; margin-top: 15px; }

        .go-home-btn {
            margin-top: 45px; padding: 18px 50px;
            border: 1px solid #00f2ff; color: #00f2ff; text-decoration: none;
            font-weight: bold; transition: 0.3s; background: rgba(0, 242, 255, 0.05);
            letter-spacing: 2px;
        }
        .go-home-btn:hover { background: #00f2ff; color: #000; box-shadow: 0 0 40px #00f2ff; }

    </style>
</head>
<body>

    <div id="alarm-state">
        <h1 class="breach-title">⚠️ HONEYPOT_TRIGGERED ⚠️</h1>
        <div class="fake-terminal" id="terminal-logs"></div>
    </div>

    <div id="joke-state">
        <img src="https://media.tenor.com/7123pXv4Q3AAAAAC/jurassic-park-dennis-nedry.gif" class="joke-media" alt="Magic Word">
        
        <h2 class="joke-text">Access Denied, Operator.</h2>
        <p class="joke-subtext">"You didn't say the magic word!"</p>
        
        <a href="/" class="go-home-btn">cd /home/emirhan</a>
    </div>

    <script>
        const terminalLogs = document.getElementById('terminal-logs');
        const alarmState = document.getElementById('alarm-state');
        const jokeState = document.getElementById('joke-state');

        // Rastgele IP Üretici (Seni ifşalamaz)
        const getRandomIP = () => Array(4).fill(0).map(() => Math.floor(Math.random() * 255)).join('.');

        const logs = [
            `[ALERT] Intrusion attempt detected from IP: ${getRandomIP()}`,
            "[SOC] Initiating Honeypot protocols...",
            `[TRACE] Route identified: node-${Math.floor(Math.random()*999)}.cyber-backbone.net`,
            "[INFO] Deploying countermeasures to target segment...",
            "[SYSTEM] Capturing browser artifacts for analysis...",
            "[WARN] Unauthorized request to /admin blocked by WAF.",
            "[INFO] Notifying Lead Analyst: Emirhan Gungoroglu...",
            "[STATUS] Target session locked. Preparing joke deployment..."
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
                    setTimeout(revealJoke, 2000);
                }
            }, 700);
        }

        function revealJoke() {
            alarmState.style.display = 'none';
            jokeState.style.display = 'flex';
        }

        window.onload = startAlarm;
    </script>

</body>
</html>
