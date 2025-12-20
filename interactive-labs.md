---
layout: page
title: "SentinelGuard High-Fidelity Workstation"
permalink: /labs/station/
---

<style>
    /* FULL DARK RESET */
    html, body, main { 
        background-color: #010409 !important; 
        color: #c9d1d9 !important; 
        margin: 0 !important; 
        padding: 0 !important;
        width: 100%;
        max-width: 100% !important;
    }

    #desktop-environment {
        background: radial-gradient(circle, #0d1117 0%, #010409 100%);
        height: 85vh;
        position: relative;
        overflow: hidden;
        border: 2px solid #30363d;
        margin: 15px;
        border-radius: 12px;
        display: flex;
        flex-direction: column;
        font-family: 'JetBrains Mono', monospace;
    }

    /* DESKTOP UI */
    .desktop-icons { padding: 30px; display: flex; flex-direction: column; gap: 40px; flex: 1; }
    .icon {
        width: 90px; text-align: center; cursor: pointer; transition: 0.2s;
        border-radius: 8px; padding: 10px; z-index: 10;
    }
    .icon:hover { background: rgba(88, 166, 255, 0.15); transform: scale(1.05); }
    .icon img { width: 50px; margin-bottom: 8px; filter: drop-shadow(0 0 5px rgba(0,0,0,0.5)); }
    .icon span { font-size: 11px; display: block; color: #e6edf3; text-shadow: 1px 1px 3px #000; }

    /* WINDOW SYSTEM */
    .window {
        position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
        width: 95%; height: 90%; background: #0d1117; border: 1px solid #30363d;
        border-radius: 10px; box-shadow: 0 30px 60px rgba(0,0,0,0.8);
        display: flex; flex-direction: column; z-index: 100;
        animation: windowOpen 0.3s cubic-bezier(0.18, 0.89, 0.32, 1.28);
    }
    @keyframes windowOpen { from { transform: translate(-50%, -48%) scale(0.9); opacity: 0; } }

    .window-header {
        background: #161b22; padding: 12px 20px; display: flex;
        justify-content: space-between; align-items: center;
        border-bottom: 1px solid #30363d; border-radius: 10px 10px 0 0;
    }
    .close-btn { color: #f85149; cursor: pointer; font-size: 20px; transition: 0.2s; }

    /* LAB CONTENT */
    .lab-content { display: flex; flex: 1; overflow: hidden; padding: 20px; gap: 20px; }
    .log-panel { flex: 2.5; background: #010409; border: 1px solid #30363d; border-radius: 6px; display: flex; flex-direction: column; }
    #log-screen { flex: 1; overflow-y: scroll; padding: 15px; font-size: 11px; color: #8b949e; line-height: 1.8; }
    .question-panel { flex: 1; background: #161b22; padding: 20px; border-radius: 6px; overflow-y: auto; border: 1px solid #30363d; }

    /* INPUTS & FEEDBACK */
    .correct-ans { border-color: #238636 !important; background: rgba(35, 134, 54, 0.1) !important; }
    .wrong-ans { border-color: #f85149 !important; background: rgba(248, 81, 73, 0.1) !important; }
    .btn-action { background: #238636; border: none; color: white; padding: 12px 20px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; }
    .input-field { width: 100%; background: #0d1117; border: 1px solid #30363d; color: #58a6ff; padding: 10px; margin-top: 5px; border-radius: 5px; outline: none; }

    #taskbar {
        background: rgba(22, 27, 34, 0.98); height: 50px;
        border-top: 1px solid #30363d; display: flex; align-items: center;
        padding: 0 20px; gap: 20px;
    }
    .hidden { display: none !important; }
    .log-entry { margin-bottom: 5px; border-bottom: 1px solid #161b22; padding-bottom: 2px; }
    .hl-warn { color: #d29922; }
    .hl-crit { color: #f85149; font-weight: bold; }
</style>

<div id="desktop-environment">
    <div class="desktop-icons">
        <div class="icon" onclick="openWindow('auth-window')">
            <img src="https://cdn-icons-png.flaticon.com/512/3233/3233514.png">
            <span>Login.sys</span>
        </div>
        <div class="icon hidden" id="range-icon" onclick="openWindow('selection-window')">
            <img src="https://cdn-icons-png.flaticon.com/512/906/906343.png">
            <span>CyberRange.exe</span>
        </div>
    </div>

    <div id="auth-window" class="window">
        <div class="window-header"><span>Terminal Authentication</span><span class="close-btn" onclick="closeWindow('auth-window')">×</span></div>
        <div style="padding: 60px; text-align: center; max-width: 400px; margin: 0 auto;">
            <h3 style="color: #58a6ff;">SECURE LOGIN</h3>
            <input type="text" id="alias-input" class="input-field" placeholder="Analyst Alias...">
            <br><br>
            <button onclick="loginAnalyst()" class="btn-action">INITIALIZE</button>
        </div>
    </div>

    <div id="selection-window" class="window hidden">
        <div class="window-header"><span>Mission Control</span><span class="close-btn" onclick="closeWindow('selection-window')">×</span></div>
        <div style="padding: 30px; overflow-y: auto;">
            <div style="display: flex; justify-content: space-between; border-bottom: 1px solid #30363d; padding-bottom: 15px; margin-bottom: 20px;">
                <h4 id="welcome-text" style="color: #58a6ff; margin: 0;">STATION: ONLINE</h4>
                <div style="color: #d29922; font-size: 12px;">GLOBAL RANK: <span id="user-rank">TRAINEE</span></div>
            </div>
            <div id="machine-list" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 20px;"></div>
        </div>
    </div>

    <div id="lab-window" class="window hidden">
        <div class="window-header"><span id="lab-window-title">Forensic Workstation</span><span class="close-btn" onclick="closeWindow('lab-window')">×</span></div>
        <div class="lab-content">
            <div class="log-panel">
                <div style="background: #161b22; padding: 8px 15px; font-size: 10px; display:flex; justify-content:space-between;">
                    <span>RAW SIEM TELEMETRY</span>
                    <span id="log-count">0 EVENTS</span>
                </div>
                <div id="log-screen"></div>
            </div>
            <div class="question-panel">
                <h4 style="color:#f85149; margin-top:0;">Investigation Checklist</h4>
                <div id="q-area"></div>
                <button onclick="checkLabAnswers()" class="btn-action" style="margin-top: 20px;">VALIDATE EVIDENCE</button>
            </div>
        </div>
    </div>

    <div id="taskbar">
        <img src="https://cdn-icons-png.flaticon.com/512/270/270104.png" width="22" style="filter: invert(1);">
        <div id="clock" style="font-size: 12px; color: #8b949e; min-width: 80px;">00:00:00</div>
        <div style="width: 1px; height: 20px; background: #30363d;"></div>
        <div id="active-user" style="font-size: 11px; color: #58a6ff;">NOT_LOGGED_IN</div>
    </div>
</div>



<script>
    const labData = {
        easy: {
            name: "OP-01: Brute Force Detection",
            questions: [
                {q: "Saldırgan IP adresi nedir?", a: "185.22.155.10"},
                {q: "Hedeflenen kullanıcı adı nedir?", a: "admin"},
                {q: "Saldırı başarılı oldu mu? (Evet/Hayır)", a: "evet"}
            ],
            generateLogs: () => {
                let logs = [];
                const now = new Date();
                // Noise
                for(let i=0; i<80; i++) {
                    let t = new Date(now.getTime() - (i * 3000)).toLocaleTimeString();
                    logs.push(`<div class="log-entry">[${t}] INFO Audit: EventID=4624 source=10.0.0.${i} target=SRV-PROD user=jdoe status=Success msg="Logon successful"</div>`);
                }
                // Attack
                for(let i=0; i<12; i++) {
                    let t = new Date(now.getTime() + (i * 1000)).toLocaleTimeString();
                    logs.push(`<div class="log-entry hl-warn">[${t}] WARN Audit: EventID=4625 source=185.22.155.10 target=SRV-PROD user=admin status=Failure msg="Logon failure: unknown user or bad password"</div>`);
                }
                // Success
                let tSucc = new Date(now.getTime() + 15000).toLocaleTimeString();
                logs.push(`<div class="log-entry hl-crit">[${tSucc}] CRIT Audit: EventID=4624 source=185.22.155.10 target=SRV-PROD user=admin status=Success msg="Account successfully logged on after multiple failures"</div>`);
                return logs.reverse();
            }
        },
        medium: {
            name: "OP-02: SQL Injection & Exfiltration",
            questions: [
                {q: "Saldırgan IP adresi?", a: "45.155.205.233"},
                {q: "Kullanılan araç (User-Agent)?", a: "sqlmap"},
                {q: "Sızdırılan tablo ismi?", a: "payments"}
            ],
            generateLogs: () => {
                let logs = [];
                const now = new Date();
                // Noise
                for(let i=0; i<100; i++) {
                    let t = new Date(now.getTime() - (i * 2000)).toLocaleTimeString();
                    logs.push(`<div class="log-entry">[${t}] INFO Apache: 192.168.1.${i} - "GET /products.php?id=${i} HTTP/1.1" 200 4520 "Mozilla/5.0"</div>`);
                }
                // Scanning
                let tScan = new Date(now.getTime() + 5000).toLocaleTimeString();
                logs.push(`<div class="log-entry hl-warn">[${tScan}] WARN Apache: 45.155.205.233 - "GET /search.php?id=1' AND (SELECT 1 FROM (SELECT COUNT(*),CONCAT(0x7e,DATABASE(),0x7e)x FROM information_schema.tables GROUP BY x)a) HTTP/1.1" 200 1024 "sqlmap/1.5.12#stable"</div>`);
                // Exfil
                let tEx = new Date(now.getTime() + 8000).toLocaleTimeString();
                logs.push(`<div class="log-entry hl-crit">[${tEx}] CRIT Apache: 45.155.205.233 - "GET /search.php?id=1 UNION SELECT 1,2,table_name FROM information_schema.tables WHERE table_schema='db' HTTP/1.1" 200 560 "sqlmap/1.5.12#stable"</div>`);
                logs.push(`<div class="log-entry hl-crit">[${tEx}] CRIT DB-Audit: Query="SELECT * FROM payments" user="web_user" source="45.155.205.233" rows=12500 status=SUCCESS</div>`);
                return logs.reverse();
            }
        }
    };

    let userAlias = "";
    let currentLabKey = "";
    let totalScore = parseInt(localStorage.getItem('total_soc_score')) || 0;

    function loginAnalyst() {
        const input = document.getElementById('alias-input').value.trim();
        if(!input) return;
        userAlias = input;
        localStorage.setItem('analyst_alias', userAlias);
        document.getElementById('active-user').innerText = `${userAlias.toUpperCase()}@SENTINEL`;
        document.getElementById('range-icon').classList.remove('hidden');
        closeWindow('auth-window');
        openWindow('selection-window');
        renderSelection();
        updateRank();
    }

    function renderSelection() {
        const container = document.getElementById('machine-list');
        container.innerHTML = Object.keys(labData).map(key => `
            <div class="icon" onclick="startLab('${key}')" style="width: 100%; background: #161b22; border: 1px solid #30363d; display:flex; align-items:center; gap:15px; padding:15px; border-radius:10px;">
                <img src="https://cdn-icons-png.flaticon.com/512/2263/2263304.png" style="width:35px; margin:0;">
                <div style="text-align:left;">
                    <div style="font-size:12px; color:#fff; font-weight:bold;">${labData[key].name}</div>
                    <div style="font-size:10px; color:#8b949e;">Threat Investigation Case</div>
                </div>
            </div>
        `).join('');
    }

    function startLab(key) {
        currentLabKey = key;
        const lab = labData[key];
        const logs = lab.generateLogs();
        document.getElementById('log-screen').innerHTML = logs.join('');
        document.getElementById('log-count').innerText = `${logs.length} EVENTS LOADED`;
        document.getElementById('q-area').innerHTML = lab.questions.map((q, i) => `
            <div style="margin-bottom:15px;">
                <label style="font-size:11px; color:#8b949e;">${q.q}</label>
                <input type="text" id="ans-${i}" class="input-field" placeholder="Evidence...">
            </div>
        `).join('');
        openWindow('lab-window');
    }

    function checkLabAnswers() {
        const lab = labData[currentLabKey];
        let correctCount = 0;
        lab.questions.forEach((q, i) => {
            const el = document.getElementById(`ans-${i}`);
            if(el.value.trim().toLowerCase() === q.a.toLowerCase()) {
                correctCount++;
                el.classList.add('correct-ans');
            } else {
                el.classList.add('wrong-ans');
            }
        });

        if(correctCount === lab.questions.length) {
            alert("🎯 CASE RESOLVED: Technical evidence verified.");
            totalScore += 100;
            localStorage.setItem('total_soc_score', totalScore);
            updateRank();
            setTimeout(() => { closeWindow('lab-window'); }, 1500);
        } else {
            alert(`⚠️ INVESTIGATION ERROR: ${lab.questions.length - correctCount} artifacts missing.`);
        }
    }

    function updateRank() {
        let rank = "TRAINEE";
        if(totalScore >= 100) rank = "JUNIOR ANALYST (L1)";
        if(totalScore >= 200) rank = "SENIOR ANALYST (L2)";
        if(totalScore >= 300) rank = "EXPERT ANALYST (SOC LEAD)";
        document.getElementById('user-rank').innerText = rank;
    }

    function openWindow(id) { document.getElementById(id).classList.remove('hidden'); }
    function closeWindow(id) { document.getElementById(id).classList.add('hidden'); }
    setInterval(() => { document.getElementById('clock').innerText = new Date().toLocaleTimeString(); }, 1000);

    window.onload = () => {
        const saved = localStorage.getItem('analyst_alias');
        if(saved) {
            userAlias = saved;
            document.getElementById('active-user').innerText = `${saved.toUpperCase()}@SENTINEL`;
            document.getElementById('range-icon').classList.remove('hidden');
            updateRank();
        }
    };
</script>
