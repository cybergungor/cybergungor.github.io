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
        /* --- GLOBAL RESET --- */
        body, html { margin: 0; padding: 0; height: 100%; overflow: hidden; background: #000; font-family: 'JetBrains Mono', monospace; }

        /* --- FAZ 1: KIRMIZI ALARM MODU --- */
        #alarm-state {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            z-index: 999;
            /* Kırmızı yanıp sönme efekti */
            animation: redAlert 0.5s infinite alternate;
        }

        @keyframes redAlert {
            0% { background: rgba(255, 0, 0, 0.1); box-shadow: inset 0 0 100px rgba(255,0,0,0.5); }
            100% { background: rgba(255, 0, 0, 0.4); box-shadow: inset 0 0 200px rgba(255,0,0,0.9); }
        }

        .breach-title {
            font-size: 4rem; color: #ff0000; text-shadow: 0 0 20px #ff0000;
            text-align: center; font-weight: 900; letter-spacing: 5px;
            animation: glitch 0.2s infinite;
        }

        /* Sahte Terminal */
        .fake-terminal {
            width: 80%; max-width: 700px; background: rgba(0,0,0,0.8);
            border: 2px solid #ff0000; padding: 20px; margin-top: 30px;
            height: 200px; overflow: hidden; text-align: left;
        }
        .log-entry { color: #ff0000; font-size: 0.9rem; margin-bottom: 5px; }

        /* --- FAZ 2: ŞAKA MODU (Başlangıçta gizli) --- */
        #joke-state {
            display: none; /* JS ile açılacak */
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: #08090a; /* Normal site rengi */
            flex-direction: column; align-items: center; justify-content: center;
            z-index: 1000; color: #00f2ff;
        }

        .joke-gif {
            border: 3px solid #00f2ff; box-shadow: 0 0 30px #00f2ff;
            border-radius: 10px; max-width: 90%;
        }

        .joke-text { margin-top: 30px; font-size: 1.5rem; text-align: center; }
        .joke-subtext { color: #8b949e; font-size: 1rem; margin-top: 10px; }

        .go-home-btn {
            margin-top: 40px; padding: 15px 40px;
            border: 2px solid #00f2ff; color: #00f2ff; text-decoration: none;
            font-weight: bold; transition: 0.3s; background: rgba(0, 242, 255, 0.1);
        }
        .go-home-btn:hover { background: #00f2ff; color: #000; box-shadow: 0 0 30px #00f2ff; }

    </style>
</head>
<body>

    <div id="alarm-state">
        <h1 class="breach-title">⚠️ HONEYPOT TRIGGERED! ⚠️</h1>
        <div class="fake-terminal" id="terminal-logs">
            </div>
    </div>

    <div id="joke-state">
        <img src="https://media.giphy.com/media/fbHk008g9FYOI/giphy.gif" alt="Ah ah ah, you didn't say the magic word" class="joke-gif">
        
        <h2 class="joke-text">Nice try, script kiddie. 😎</h2>
        <p class="joke-subtext">Burada admin paneli yok. Sadece ben ve loglarım var.</p>
        
        <a href="/" class="go-home-btn">CD /HOME</a>
    </div>

    <audio id="siren-sound" src="https://www.soundjay.com/mechanical/sounds/smoke-detector-1.mp3" preload="auto"></audio>

    <script>
        const terminalLogs = document.getElementById('terminal-logs');
        const alarmState = document.getElementById('alarm-state');
        const jokeState = document.getElementById('joke-state');
        const siren = document.getElementById('siren-sound');

        // Sahte loglar dizisi
        const logs = [
            "[ALERT] Unauthorized access attempt detected at /admin directory.",
            "[SOC] HONEYPOT MECHANISM ACTIVATED.",
            "[TRACE] Initiating reverse IP lookup...",
            "[TRACE] Tracing route hops... 192.168.1.1 -> 10.0.0.5 -> ...",
            "[GEO] Triangulating user location...",
            "[ALERT] User identified as curious visitor.",
            "[ACTION] Deploying countermeasures...",
            "[SYSTEM] Deleting System32... (just kidding)",
            "[SOC] Notifying Emirhan Gungoroglu of capture event."
        ];

        let logIndex = 0;

        // 1. Adım: Alarmı başlat ve logları akıt
        function startAlarm() {
            // Tarayıcı politikaları bazen otomatik sesi engeller ama deneriz.
            try { siren.play(); siren.loop = true; } catch(e) { console.log("Audio blocked"); }
            
            const logInterval = setInterval(() => {
                if (logIndex < logs.length) {
                    const newLine = document.createElement('div');
                    newLine.className = 'log-entry';
                    // Her satırın başına zaman damgası ekle
                    newLine.innerText = `[${new Date().toLocaleTimeString()}] ${logs[logIndex]}`;
                    terminalLogs.appendChild(newLine);
                    terminalLogs.scrollTop = terminalLogs.scrollHeight; // Otomatik aşağı kaydır
                    logIndex++;
                } else {
                    clearInterval(logInterval);
                    // Loglar bitince şaka moduna geç (toplam 5-6 saniye sürer)
                    setTimeout(revealJoke, 1500);
                }
            }, 600); // Her 600ms'de bir yeni log satırı
        }

        // 2. Adım: Şakayı göster
        function revealJoke() {
            siren.pause(); // Sesi durdur
            alarmState.style.display = 'none'; // Kırmızı ekranı gizle
            jokeState.style.display = 'flex';  // Şaka ekranını aç
        }

        // Sayfa yüklenince her şeyi başlat
        window.onload = startAlarm;
    </script>

</body>
</html>
