---
layout: page
title: "SentinelGuard Workstation"
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
        display: flex; flex-direction: column;
        font-family: 'JetBrains Mono', monospace;
    }

    /* DESKTOP UI */
    .desktop-icons { padding: 30px; display: flex; flex-direction: column; gap: 40px; flex: 1; }
    .icon {
        width: 90px; text-align: center; cursor: pointer; transition: 0.2s;
        border-radius: 8px; padding: 10px; z-index: 10;
    }
    .icon:hover { background: rgba(88, 166, 255, 0.15); transform: scale(1.05); }
    .icon img { width: 50px; margin-bottom: 8px; }
    .icon span { font-size: 11px; display: block; color: #e6edf3; }

    /* WINDOW SYSTEM */
    .window {
        position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
        width: 95%; height: 90%; background: #0d1117; border: 1px solid #30363d;
        border-radius: 10px; box-shadow: 0 30px 60px rgba(0,0,0,0.8);
        display: flex; flex-direction: column; z-index: 100;
        animation: windowOpen 0.3s ease-out;
    }
    @keyframes windowOpen { from { transform: translate(-50%, -48%) scale(0.9); opacity: 0; } }

    .window-header {
        background: #161b22; padding: 12px 20px; display: flex;
        justify-content: space-between; align-items: center;
        border-bottom: 1px solid #30363d; border-radius: 10px 10px 0 0;
    }

    /* CONTENT BOXES */
    .briefing-box {
        background: rgba(88, 166, 255, 0.05); border: 1px solid #58a6ff;
        padding: 20px; border-radius: 8px; margin-bottom: 20px;
    }
    .brief-section { margin-bottom: 15px; border-bottom: 1px solid rgba(88, 166, 255, 0.1); padding-bottom: 10px; }
    .brief-title { color: #58a6ff; font-weight: bold; font-size: 0.9rem; }
    .brief-text { font-size: 0.8rem; color: #8b949e; margin-top: 5px; line-height: 1.4; }

    .lab-content { display: flex; flex: 1; overflow: hidden; padding: 20px; gap: 20px; }
    .log-panel { flex: 2.5; background: #010409; border: 1px solid #30363d; border-radius: 6px; display: flex; flex-direction: column; }
    #log-screen { flex: 1; overflow-y: scroll; padding: 15px; font-size: 11px; color: #8b949e; line-height: 1.8; }
    
    /* INPUTS & BUTTONS */
    .btn-action { background: #238636; border: none; color: white; padding: 12px 20px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; }
    .input-field { width: 100%; background: #0d1117; border: 1px solid #30363d; color: #58a6ff; padding: 10px; margin-top: 5px; border-radius: 5px; outline: none; }
    .correct-ans { border-color: #238636 !important; background: rgba(35, 134, 54, 0.1) !important; }
    .wrong-ans { border-color: #f85149 !important; background: rgba(248, 81, 73, 0.1) !important; }

    #taskbar { background: rgba(22, 27, 34, 0.98); height: 50px; border-top: 1px solid #30363d; display: flex; align-items: center; padding: 0 20px; gap: 20px; }
    .hidden { display: none !important; }
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
        <div class="window-header"><span>Authentication Required</span><span class="close-btn" onclick="closeWindow('auth-window')" style="cursor:pointer">×</span></div>
        <div style="padding: 60px; text-align: center; max-width: 400px; margin: 0 auto;">
            <h3 style="color: #58a6ff;">STATION LOGIN</h3>
            <input type="text" id="alias-input" class="input-field" placeholder="Analyst Name...">
            <br><br>
            <button onclick="loginAnalyst()" class="btn-action">INITIALIZE</button>
        </div>
    </div>

    <div id="selection-window" class="window hidden">
        <div class="window-header"><span>Mission Control</span><span class="close-btn" onclick="closeWindow('selection-window')" style="cursor:pointer">×</span></div>
        <div id="selection-content" style="padding: 30px; overflow-y: auto;">
            <h4 style="color: #58a6ff;">AVAILABLE MISSIONS</h4>
            <div id="machine-list" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 20px; margin-top: 20px;"></div>
        </div>
        <div id="briefing-content" class="hidden" style="padding: 30px; overflow-y: auto;">
            <h3 id="brief-lab-name" style="color: #58a6ff;">MISSION NAME</h3>
            <div class="briefing-box" id="brief-details"></div>
            <div style="display: flex; gap: 15px;">
                <button onclick="hideBriefing()" class="btn-action" style="background:#30363d;">BACK</button>
                <button onclick="startLabAfterBrief()" class="btn-action">GO LIVE (ACCESS TELEMETRY)</button>
            </div>
        </div>
    </div>

    <div id="lab-window" class="window hidden">
        <div class="window-header"><span id="lab-window-title">Forensic Workstation</span><span class="close-btn" onclick="closeWindow('lab-window')" style="cursor:pointer">×</span></div>
        <div class="lab-content">
            <div class="log-panel">
                <div style="background: #161b22; padding: 8px 15px; font-size: 10px;">RAW SIEM TELEMETRY (MONO_COLOR_MODE)</div>
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
        <div id="active-user" style="margin-left:auto; font-size: 11px; color: #58a6ff;">GUEST@SENTINEL</div>
    </div>
</div>

<script>
    const labData = {
        easy: {
            name: "OP-01: Brute Force Attack",
            details: `
                <div class="brief-section"><div class="brief-title">Saldırı Nedir?</div><div class="brief-text">Brute Force, bir saldırganın deneme-yanılma yoluyla kullanıcı şifrelerini tahmin etmeye çalıştığı bir kaba kuvvet saldırısıdır.</div></div>
                <div class="brief-section"><div class="brief-title">Nasıl Yapılır?</div><div class="brief-text">Saldırganlar otomatik araçlar kullanarak binlerce şifre kombinasyonunu saniyeler içinde dener.</div></div>
                <div class="brief-section"><div class="brief-title">Nasıl Tespit Edilir?</div><div class="brief-text">Loglarda aynı kullanıcı adına veya IP'sine gelen çok sayıda <b>4625 (Logon Failure)</b> Event ID'si aranır. Seri hatalardan sonra gelen bir <b>4624 (Logon Success)</b> ihlalin başarılı olduğunu gösterir.</div></div>
            `,
            questions: [{q: "Saldırgan IP?", a: "185.22.155.10"}, {q: "Hedef Kullanıcı?", a: "admin"}, {q: "Başarılı mı? (Evet/Hayır)", a: "evet"}],
            generateLogs: () => {
                let l = []; for(let i=0; i<80; i++) l.push(`[${new Date().toLocaleTimeString()}] INFO Audit: EventID=4624 source=10.0.0.${i} target=SRV-PROD user=jdoe status=Success msg="Logon successful"`);
                for(let i=0; i<12; i++) l.push(`[${new Date().toLocaleTimeString()}] INFO Audit: EventID=4625 source=185.22.155.10 target=SRV-PROD user=admin status=Failure msg="Logon failure"`);
                l.push(`[${new Date().toLocaleTimeString()}] INFO Audit: EventID=4624 source=185.22.155.10 target=SRV-PROD user=admin status=Success msg="Account logon success after multiple failures"`);
                return l.reverse();
            }
        },
        medium: {
            name: "OP-02: SQL Injection & Exfiltration",
            details: `
                <div class="brief-section"><div class="brief-title">Saldırı Nedir?</div><div class="brief-text">SQL Injection, web uygulamalarındaki formlar veya URL parametreleri üzerinden veritabanına doğrudan komut gönderilmesidir.</div></div>
                <div class="brief-section"><div class="brief-title">Nasıl Yapılır?</div><div class="brief-text">Saldırgan, giriş alanlarına 'UNION SELECT' gibi ifadeler yazarak veritabanındaki gizli tabloları listeler.</div></div>
                <div class="brief-section"><div class="brief-title">Nasıl Tespit Edilir?</div><div class="brief-text">Web sunucusu loglarında (Apache/Nginx) URL içinde geçen SQL anahtar kelimeleri (SELECT, FROM, information_schema) ve şüpheli User-Agent (sqlmap vb.) bilgileri aranır.</div></div>
            `,
            questions: [{q: "Saldırgan IP?", a: "45.155.205.233"}, {q: "User-Agent (Araç)?", a: "sqlmap"}, {q: "Sızdırılan Tablo?", a: "payments"}],
            generateLogs: () => {
                let l = []; for(let i=0; i<100; i++) l.push(`[${new Date().toLocaleTimeString()}] INFO Apache: 192.168.1.${i} - "GET /products.php?id=${i} HTTP/1.1" 200 4520`);
                l.push(`[${new Date().toLocaleTimeString()}] INFO Apache: 45.155.205.233 - "GET /search.php?id=1' UNION SELECT 1,table_name FROM information_schema.tables WHERE table_schema='db' HTTP/1.1" 200 560 "sqlmap/1.5.12"`);
                l.push(`[${new Date().toLocaleTimeString()}] INFO DB-Audit: Query="SELECT * FROM payments" user="web_user" source="45.155.205.233" status=SUCCESS`);
                return l.reverse();
            }
        }
    };

    let userAlias = ""; let currentLabKey = ""; let totalScore = parseInt(localStorage.getItem('total_soc_score')) || 0;

    function loginAnalyst() {
        const input = document.getElementById('alias-input').value.trim(); if(!input) return;
        userAlias = input; localStorage.setItem('analyst_alias', userAlias);
        document.getElementById('active-user').innerText = `${userAlias.toUpperCase()}@SENTINEL`;
        document.getElementById('range-icon').classList.remove('hidden');
        closeWindow('auth-window'); openWindow('selection-window'); renderSelection();
    }

    function renderSelection() {
        const container = document.getElementById('machine-list');
        container.innerHTML = Object.keys(labData).map(key => `
            <div class="icon" onclick="showBriefing('${key}')" style="width: 100%; background: #161b22; border: 1px solid #30363d; display:flex; align-items:center; gap:15px; padding:15px; border-radius:10px;">
                <img src="https://cdn-icons-png.flaticon.com/512/2263/2263304.png" style="width:35px;">
                <div style="text-align:left;"><div style="font-size:12px; color:#fff; font-weight:bold;">${labData[key].name}</div></div>
            </div>
        `).join('');
    }

    function showBriefing(key) {
        currentLabKey = key;
        document.getElementById('selection-content').classList.add('hidden');
        document.getElementById('briefing-content').classList.remove('hidden');
        document.getElementById('brief-lab-name').innerText = labData[key].name;
        document.getElementById('brief-details').innerHTML = labData[key].details;
    }

    function hideBriefing() {
        document.getElementById('selection-content').classList.remove('hidden');
        document.getElementById('briefing-content').classList.add('hidden');
    }

    function startLabAfterBrief() {
        closeWindow('selection-window'); openWindow('lab-window');
        const lab = labData[currentLabKey];
        document.getElementById('lab-window-title').innerText = lab.name;
        document.getElementById('log-screen').innerHTML = lab.generateLogs().map(l => `<div style="margin-bottom:4px; border-bottom:1px solid #161b22;">${l}</div>`).join('');
        document.getElementById('q-area').innerHTML = lab.questions.map((q, i) => `<div style="margin-bottom:15px;"><label style="font-size:11px; color:#8b949e;">${q.q}</label><input type="text" id="ans-${i}" class="input-field"></div>`).join('');
    }

    function checkLabAnswers() {
        const lab = labData[currentLabKey]; let correct = 0;
        lab.questions.forEach((q, i) => {
            const el = document.getElementById(`ans-${i}`);
            if(el.value.trim().toLowerCase() === q.a.toLowerCase()) { correct++; el.classList.add('correct-ans'); } else { el.classList.add('wrong-ans'); }
        });
        if(correct === lab.questions.length) { alert("🎯 CASE RESOLVED!"); totalScore += 100; localStorage.setItem('total_soc_score', totalScore); closeWindow('lab-window'); } else { alert("⚠️ MISSING EVIDENCE!"); }
    }

    function openWindow(id) { document.getElementById(id).classList.remove('hidden'); }
    function closeWindow(id) { document.getElementById(id).classList.add('hidden'); }
    setInterval(() => { document.getElementById('clock').innerText = new Date().toLocaleTimeString(); }, 1000);
</script>
