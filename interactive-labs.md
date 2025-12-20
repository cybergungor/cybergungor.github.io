---
layout: page
title: "SentinelGuard Cyber Range"
permalink: /labs/station/
---

<style>
    #soc-station {
        background: #0d1117;
        color: #c9d1d9;
        font-family: 'JetBrains Mono', monospace;
        border: 1px solid #30363d;
        border-radius: 12px;
        padding: 30px;
        min-height: 700px;
    }

    .screen { animation: fadeIn 0.4s ease-in-out; }
    @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    .hidden { display: none !important; }

    /* CARD DESIGN */
    .lab-card {
        background: #161b22;
        border: 1px solid #30363d;
        padding: 20px;
        border-radius: 10px;
        margin-bottom: 15px;
        transition: 0.3s;
        cursor: pointer;
    }
    .lab-card:hover { border-color: #58a6ff; transform: translateY(-3px); }

    .difficulty { padding: 3px 8px; border-radius: 4px; font-size: 11px; font-weight: bold; text-transform: uppercase; }
    .easy { background: rgba(63, 185, 80, 0.1); color: #3fb950; }
    .medium { background: rgba(210, 153, 34, 0.1); color: #d29922; }
    .hard { background: rgba(248, 81, 73, 0.1); color: #f85149; }

    /* INPUT & BUTTONS */
    .analyst-input { width: 100%; background: #010409; border: 1px solid #30363d; color: #58a6ff; padding: 12px; border-radius: 6px; margin-top: 10px; outline: none; }
    .btn-main { background: #238636; color: white; border: none; padding: 12px 25px; border-radius: 6px; cursor: pointer; font-weight: bold; }
    .btn-main:hover { background: #2ea043; box-shadow: 0 0 15px rgba(46,160,67,0.4); }

    /* LOG VIEWER */
    #log-viewer { height: 400px; overflow-y: auto; background: #010409; padding: 15px; border: 1px solid #30363d; font-size: 12px; border-radius: 5px; }

    /* BRIEFING BOX */
    .briefing-box {
        background: rgba(88, 166, 255, 0.05);
        border: 1px solid #58a6ff;
        padding: 20px;
        border-radius: 8px;
        margin-bottom: 20px;
    }
    .brief-item { margin-bottom: 10px; font-size: 0.85rem; }
    .brief-title { color: #58a6ff; font-weight: bold; margin-right: 5px; }
</style>

<div id="soc-station">

    <div id="view-auth" class="screen">
        <h2 style="color: #58a6ff; text-align: center;">ANALYST TERMINAL ACCESS</h2>
        <div style="max-width: 400px; margin: 40px auto; text-align: center;">
            <p style="font-size: 0.9rem; color: #8b949e;">Initialize your session to access the Cyber Range.</p>
            <input type="text" id="alias-input" class="analyst-input" placeholder="Enter Alias...">
            <br><br>
            <button onclick="initStation()" class="btn-main">INITIALIZE SESSION</button>
        </div>
    </div>

    <div id="view-selection" class="screen hidden">
        <div style="display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #30363d; padding-bottom: 15px;">
            <h3 id="welcome-msg">STATION: ONLINE</h3>
            <div id="global-stats" style="font-size: 0.8rem; color: #58a6ff;">RANK: <span id="user-rank">TRAINEE</span></div>
        </div>
        <p style="margin: 20px 0; color: #8b949e;">Select a scenario to start technical briefing:</p>
        <div id="lab-list"></div>
    </div>

    <div id="view-briefing" class="screen hidden">
        <h2 id="brief-title" style="color: #58a6ff;">MISSION BRIEFING</h2>
        <div id="brief-content" class="briefing-box"></div>
        <div style="display: flex; gap: 15px;">
            <button onclick="backToMenu()" class="btn-main" style="background: #30363d;">BACK</button>
            <button onclick="goLive()" class="btn-main">GO LIVE (START LAB)</button>
        </div>
    </div>

    <div id="view-lab" class="screen hidden">
        <div style="display:flex; justify-content:space-between; margin-bottom: 20px;">
            <h3 id="lab-title" style="color: #d29922;">OPERATION</h3>
            <div id="mission-status" style="font-size: 0.7rem; color: #3fb950; animation: pulse 2s infinite;">● LIVE INVESTIGATION</div>
        </div>
        
        <div style="display: flex; gap: 25px;">
            <div style="flex: 2;">
                <h4 style="color: #58a6ff;">📑 LOG VAULT</h4>
                <div id="log-viewer"></div>
            </div>
            
            <div style="flex: 1; background: #161b22; padding: 20px; border-radius: 8px; border: 1px solid #30363d;">
                <h4 style="color: #f85149;">EVIDENCE COLLECTION</h4>
                <div id="question-area" style="font-size: 0.8rem;"></div>
                <button onclick="evaluateMission()" class="btn-main" style="width: 100%; margin-top: 20px;">SUBMIT TO L2</button>
            </div>
        </div>
    </div>

    <div id="view-result" class="screen hidden" style="text-align: center; padding: 50px;">
        <h2 id="result-status">ANALYSIS COMPLETE</h2>
        <p id="result-msg" style="margin: 20px 0;"></p>
        <div id="badge-display" style="padding: 30px; border: 2px solid #58a6ff; border-radius: 15px; display: inline-block; min-width: 300px;">
            <div style="font-size: 0.7rem; color: #8b949e;">OFFICIAL ANALYST CREDENTIAL</div>
            <h2 id="badge-name" style="margin: 10px 0;">NAME</h2>
            <div id="badge-rank" style="font-size: 1.2rem; font-weight: bold; color: #d29922;">RANK</div>
        </div>
        <br><br>
        <button onclick="backToMenu()" class="btn-main">NEXT MISSION</button>
    </div>

</div>

<script>
    const labs = {
        easy: {
            title: "OP-01: Brute Force Attacks",
            difficulty: "easy",
            briefing: `
                <div class="brief-item"><span class="brief-title">Nedir?</span> Brute Force (Kaba Kuvvet), saldırganın binlerce şifre kombinasyonunu deneyerek bir hesaba sızma girişimidir.</div>
                <div class="brief-item"><span class="brief-title">Nelere Bakmalı?</span> Kısa süre içinde aynı IP'den gelen çok sayıda <b>401 Unauthorized</b> hatası en büyük kanıttır.</div>
                <div class="brief-item"><span class="brief-title">Dikkat:</span> Saldırı başarılı olduysa, seri hatalardan sonra bir tane <b>200 OK</b> kodu görülür. Bu, şifrenin bulunduğunu gösterir.</div>
            `,
            logs: [
                "2025-12-20 04:10:01 185.22.155.10 GET /login - 401 (Fail) User: admin",
                "2025-12-20 04:10:05 185.22.155.10 GET /login - 401 (Fail) User: admin",
                "2025-12-20 04:10:15 185.22.155.10 GET /login - 200 (Success) User: admin",
                "2025-12-20 04:11:00 185.22.155.10 GET /dashboard - 200 User: admin"
            ],
            questions: [
                {q: "1. Saldırgan IP adresi nedir?", a: "185.22.155.10"},
                {q: "2. Hedeflenen kullanıcı adı nedir?", a: "admin"},
                {q: "3. Saldırı başarılı oldu mu? (Evet/Hayır)", a: "evet"}
            ]
        },
        medium: {
            title: "OP-02: SQL Injection & Exfiltration",
            difficulty: "medium",
            briefing: `
                <div class="brief-item"><span class="brief-title">Nedir?</span> Saldırganın URL parametrelerine SQL komutları enjekte ederek veritabanını okumasıdır.</div>
                <div class="brief-item"><span class="brief-title">Nelere Bakmalı?</span> URL içindeki <b>UNION, SELECT, --, ' </b> gibi karakterleri arayın.</div>
                <div class="brief-item"><span class="brief-title">Dikkat:</span> "User-Agent" kısmına bakın; saldırgan <b>sqlmap</b> gibi otomatik bir araç kullanıyor olabilir.</div>
            `,
            logs: [
                "2025-12-20 03:00:10 45.155.205.233 GET /search?id=1' - 200 agent: sqlmap",
                "2025-12-20 03:02:45 45.155.205.233 GET /search?id=1 UNION SELECT credit_card FROM payments - 200",
                "2025-12-20 03:05:00 45.155.205.233 GET /config.php - 403"
            ],
            questions: [
                {q: "1. Saldırgan IP adresi nedir?", a: "45.155.205.233"},
                {q: "2. Saldırgan hangi aracı kullandı?", a: "sqlmap"},
                {q: "3. Hangi veritabanı tablosu hedeflendi?", a: "payments"}
            ]
        },
        hard: {
            title: "OP-03: Post-Exploitation & Lateral Movement",
            difficulty: "hard",
            briefing: `
                <div class="brief-item"><span class="brief-title">Nedir?</span> Sisteme sızan saldırganın yetki yükseltip ağda yayılmasıdır.</div>
                <div class="brief-item"><span class="brief-title">Nelere Bakmalı?</span> PowerShell'in <b>Bypass</b> modunda çalıştırılması ve <b>Mimikatz</b> gibi şifre çalan araçların izlerini arayın.</div>
                <div class="brief-item"><span class="brief-title">Dikkat:</span> Standart dışı portlara (Örn: 4444) yapılan ağ bağlantılarını kontrol edin.</div>
            `,
            logs: [
                "2025-12-20 01:20:00 [SYSTEM] powershell.exe -ExecutionPolicy Bypass -File calc.ps1",
                "2025-12-20 01:21:45 [NETWORK] Connection from 10.0.0.5 to 192.168.100.55 Port: 4444",
                "2025-12-20 01:22:10 [SECURITY] Process: mimikatz.exe executed from Temp folder"
            ],
            questions: [
                {q: "1. Yerel ağdaki saldırgan IP adresi?", a: "10.0.0.5"},
                {q: "2. Çalıştırılan zararlı script adı?", a: "calc.ps1"},
                {q: "3. Şifre çalmak için hangi araç kullanıldı?", a: "mimikatz"}
            ]
        }
    };

    let userAlias = "";
    let selectedKey = "";
    let totalScore = parseInt(localStorage.getItem('total_soc_score')) || 0;

    function initStation() {
        userAlias = document.getElementById('alias-input').value.trim();
        if(!userAlias) return;
        localStorage.setItem('analyst_alias', userAlias);
        document.getElementById('view-auth').classList.add('hidden');
        document.getElementById('view-selection').classList.remove('hidden');
        renderLabList();
        updateRank();
    }

    function renderLabList() {
        const list = document.getElementById('lab-list');
        list.innerHTML = Object.keys(labs).map(key => `
            <div class="lab-card" onclick="showBriefing('${key}')">
                <div style="display:flex; justify-content:space-between;">
                    <strong style="color: #fff;">${labs[key].title}</strong>
                    <span class="difficulty ${labs[key].difficulty}">${labs[key].difficulty}</span>
                </div>
            </div>
        `).join('');
    }

    function showBriefing(key) {
        selectedKey = key;
        document.getElementById('view-selection').classList.add('hidden');
        document.getElementById('view-briefing').classList.remove('hidden');
        document.getElementById('brief-title').innerText = labs[key].title;
        document.getElementById('brief-content').innerHTML = labs[key].briefing;
    }

    function goLive() {
        document.getElementById('view-briefing').classList.add('hidden');
        document.getElementById('view-lab').classList.remove('hidden');
        const lab = labs[selectedKey];
        document.getElementById('lab-title').innerText = lab.title;
        document.getElementById('log-viewer').innerHTML = lab.logs.map(l => `<div style="border-bottom: 1px solid #21262d; padding: 5px 0;">${l}</div>`).join('');
        document.getElementById('question-area').innerHTML = lab.questions.map((q, i) => `
            <div style="margin-bottom:15px;">
                <label>${q.q}</label>
                <input type="text" id="ans-${i}" class="analyst-input" placeholder="Evidence here...">
            </div>
        `).join('');
    }

    function evaluateMission() {
        const lab = labs[selectedKey];
        let correct = 0;
        lab.questions.forEach((q, i) => {
            if(document.getElementById(`ans-${i}`).value.trim().toLowerCase() === q.a.toLowerCase()) correct++;
        });

        totalScore += correct;
        localStorage.setItem('total_soc_score', totalScore);
        
        document.getElementById('view-lab').classList.add('hidden');
        document.getElementById('view-result').classList.remove('hidden');
        document.getElementById('badge-name').innerText = userAlias.toUpperCase();
        document.getElementById('result-msg').innerText = `Tebrikler Analist! ${lab.questions.length} sorudan ${correct} tanesini doğru yanıtladın.`;
        updateRank();
    }

    function updateRank() {
        let rank = "TRAINEE";
        if(totalScore >= 3) rank = "JUNIOR ANALYST (L1)";
        if(totalScore >= 6) rank = "SENIOR ANALYST (L2)";
        if(totalScore >= 9) rank = "EXPERT ANALYST (SOC LEAD)";
        document.getElementById('user-rank').innerText = rank;
        if(document.getElementById('badge-rank')) document.getElementById('badge-rank').innerText = rank;
    }

    function backToMenu() {
        document.querySelectorAll('.screen').forEach(s => s.classList.add('hidden'));
        document.getElementById('view-selection').classList.remove('hidden');
    }
</script>
