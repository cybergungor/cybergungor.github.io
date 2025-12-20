---
layout: page
title: "SentinelGuard Workstation"
permalink: /labs/station/
---

<style>
    /* SAYFA SIFIRLAMA - BEYAZ ALANLARI YOK ETME */
    html, body, main { 
        background-color: #010409 !important; 
        color: #c9d1d9 !important; 
        margin: 0 !important; 
        padding: 0 !important;
        width: 100%;
        max-width: 100% !important;
    }

    /* MASAÜSTÜ TASARIMI */
    #desktop-environment {
        background: radial-gradient(circle, #0d1117 0%, #010409 100%);
        height: 90vh;
        position: relative;
        overflow: hidden;
        border: 2px solid #30363d;
        margin: 10px;
        border-radius: 8px;
        display: flex;
        flex-direction: column;
    }

    .desktop-icons {
        padding: 20px;
        display: flex;
        flex-direction: column;
        gap: 30px;
        flex: 1;
    }

    .icon {
        width: 80px;
        text-align: center;
        cursor: pointer;
        transition: 0.2s;
        border-radius: 5px;
        padding: 10px;
    }

    .icon:hover { background: rgba(88, 166, 255, 0.1); }
    .icon img { width: 45px; margin-bottom: 5px; }
    .icon span { font-size: 11px; display: block; text-shadow: 1px 1px 2px #000; }

    /* TASKBAR */
    #taskbar {
        background: rgba(22, 27, 34, 0.95);
        height: 45px;
        border-top: 1px solid #30363d;
        display: flex;
        align-items: center;
        padding: 0 15px;
        gap: 15px;
    }

    /* PENCERE SİSTEMİ */
    .window {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        width: 90%;
        height: 85%;
        background: #0d1117;
        border: 1px solid #30363d;
        border-radius: 8px;
        box-shadow: 0 20px 50px rgba(0,0,0,0.7);
        display: flex;
        flex-direction: column;
        z-index: 100;
        animation: windowOpen 0.3s ease-out;
    }

    @keyframes windowOpen { from { transform: translate(-50%, -45%) scale(0.95); opacity: 0; } }

    .window-header {
        background: #161b22;
        padding: 10px 15px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        border-bottom: 1px solid #30363d;
        border-radius: 8px 8px 0 0;
    }

    .close-btn { color: #f85149; cursor: pointer; font-weight: bold; }

    /* LAB İÇERİK */
    .lab-content { display: flex; flex: 1; overflow: hidden; padding: 15px; gap: 20px; }
    .log-panel { flex: 2; background: #010409; border-radius: 5px; display: flex; flex-direction: column; }
    #log-screen { flex: 1; overflow-y: scroll; padding: 10px; font-size: 11px; font-family: 'JetBrains Mono'; line-height: 1.6; color: #8b949e; }
    .question-panel { flex: 1; background: #161b22; padding: 15px; border-radius: 5px; overflow-y: auto; }

    /* DİĞER STİLLER */
    .hidden { display: none !important; }
    .btn-action { background: #238636; border: none; color: white; padding: 8px 15px; border-radius: 5px; cursor: pointer; font-size: 12px; }
    .input-field { width: 100%; background: #0d1117; border: 1px solid #30363d; color: #58a6ff; padding: 8px; margin-top: 5px; border-radius: 4px; }
</style>

<div id="desktop-environment">
    
    <div class="desktop-icons" id="main-desktop">
        <div class="icon" onclick="openWindow('auth-window')">
            <img src="https://cdn-icons-png.flaticon.com/512/3233/3233514.png" alt="Login">
            <span>Login.sys</span>
        </div>
        
        <div class="icon hidden" id="terminal-icon" onclick="openWindow('selection-window')">
            <img src="https://cdn-icons-png.flaticon.com/512/906/906343.png" alt="Term">
            <span>CyberRange.exe</span>
        </div>
    </div>

    <div id="auth-window" class="window">
        <div class="window-header"><span>System Authentication</span><span class="close-btn" onclick="closeWindow('auth-window')">×</span></div>
        <div style="padding: 40px; text-align: center;">
            <h3 style="color: #58a6ff;">ANALYST CREDENTIALS REQUIRED</h3>
            <input type="text" id="alias-input" class="input-field" style="max-width: 300px;" placeholder="Username...">
            <br><br>
            <button onclick="loginAnalyst()" class="btn-action">AUTHORIZE SESSION</button>
        </div>
    </div>

    <div id="selection-window" class="window hidden">
        <div class="window-header"><span>Mission Control</span><span class="close-btn" onclick="closeWindow('selection-window')">×</span></div>
        <div style="padding: 20px; overflow-y: auto;">
            <h4 id="welcome-text">WELCOME, ANALYST</h4>
            <div id="machine-list" style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 20px;">
                </div>
        </div>
    </div>

    <div id="lab-window" class="window hidden">
        <div class="window-header"><span id="lab-window-title">Investigation in Progress</span><span class="close-btn" onclick="closeWindow('lab-window')">×</span></div>
        <div class="lab-content">
            <div class="log-panel">
                <div style="background: #161b22; padding: 5px 10px; font-size: 10px;">TELEMETRY_LOG_VIEWER</div>
                <div id="log-screen"></div>
            </div>
            <div class="question-panel">
                <h4 style="color:#f85149; margin-top:0;">Case Evidence</h4>
                <div id="q-area"></div>
                <button onclick="checkLabAnswers()" class="btn-action" style="width: 100%; margin-top: 20px;">SUBMIT TO L2</button>
            </div>
        </div>
    </div>

    <div id="taskbar">
        <img src="https://cdn-icons-png.flaticon.com/512/270/270104.png" width="20" style="filter: invert(1);">
        <div style="font-size: 12px; color: #8b949e;" id="clock">12:00 PM</div>
        <div id="active-user" style="margin-left: auto; font-size: 11px; color: #58a6ff;">GUEST_MODE</div>
    </div>
</div>

<script>
    const labData = {
        easy: {
            name: "OP-01: Brute Force",
            questions: [{q: "Attacker IP?", a: "192.168.45.10"}, {q: "Success Code?", a: "200"}],
            generateLogs: () => {
                let l = [];
                for(let i=0; i<150; i++) l.push(`[${new Date().toLocaleTimeString()}] INFO: Valid login from 10.0.0.${i} User: jdoe`);
                l.push(`[04:10:05] WARN: Failed login from 192.168.45.10 User: admin`);
                l.push(`[04:10:10] WARN: Failed login from 192.168.45.10 User: admin`);
                l.push(`[04:10:15] CRIT: Success login from 192.168.45.10 User: admin - CODE: 200`);
                return l;
            }
        },
        medium: {
            name: "OP-02: SQL Injection",
            questions: [{q: "Attacker IP?", a: "45.2.1.99"}, {q: "Tool used?", a: "sqlmap"}, {q: "Table leaked?", a: "customers"}],
            generateLogs: () => {
                let l = [];
                for(let i=0; i<200; i++) l.push(`[${new Date().toLocaleTimeString()}] DEBUG: DB Query executed: SELECT * FROM products WHERE id=${i}`);
                l.push(`[03:22:10] ALERT: SQLi Pattern detected from 45.2.1.99 - Payload: ' UNION SELECT * FROM customers--`);
                l.push(`[03:22:11] INFO: HTTP Response 200 - User-Agent: sqlmap/1.5`);
                return l;
            }
        }
    };

    function loginAnalyst() {
        const name = document.getElementById('alias-input').value.trim();
        if(!name) return;
        localStorage.setItem('analyst', name);
        document.getElementById('active-user').innerText = name.toUpperCase();
        document.getElementById('terminal-icon').classList.remove('hidden');
        closeWindow('auth-window');
        openWindow('selection-window');
        renderSelection();
    }

    function renderSelection() {
        const container = document.getElementById('machine-list');
        container.innerHTML = Object.keys(labData).map(key => `
            <div class="icon" onclick="startLab('${key}')" style="width: auto; background: #161b22; border: 1px solid #30363d;">
                <img src="https://cdn-icons-png.flaticon.com/512/2263/2263304.png">
                <span>${labData[key].name}</span>
            </div>
        `).join('');
    }

    function startLab(key) {
        const lab = labData[key];
        document.getElementById('lab-window-title').innerText = lab.name;
        document.getElementById('log-screen').innerHTML = lab.generateLogs().map(l => `<div>${l}</div>`).join('');
        document.getElementById('q-area').innerHTML = lab.questions.map((q, i) => `
            <div style="margin-bottom:10px;">
                <label style="font-size:11px;">${q.q}</label>
                <input type="text" id="ans-${i}" class="input-field">
            </div>
        `).join('');
        openWindow('lab-window');
    }

    function openWindow(id) { document.getElementById(id).classList.remove('hidden'); }
    function closeWindow(id) { document.getElementById(id).classList.add('hidden'); }

    setInterval(() => { document.getElementById('clock').innerText = new Date().toLocaleTimeString(); }, 1000);
</script>
