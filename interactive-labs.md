---
layout: page
title: "SentinelGuard Professional Workstation"
permalink: /labs/station/
---

<style>
    /* RESET & BASE */
    html, body, main { background-color: #010409 !important; color: #c9d1d9 !important; margin: 0 !important; padding: 0 !important; width: 100%; max-width: 100% !important; }
    #desktop-environment {
        background: radial-gradient(circle, #0d1117 0%, #010409 100%);
        height: 85vh; position: relative; overflow: hidden; border: 2px solid #30363d; margin: 15px; border-radius: 12px; display: flex; flex-direction: column; font-family: 'JetBrains Mono', monospace;
    }

    /* WINDOWS & MODALS */
    .window {
        position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
        width: 95%; height: 92%; background: #0d1117; border: 1px solid #30363d; border-radius: 10px; box-shadow: 0 30px 60px rgba(0,0,0,0.8); display: flex; flex-direction: column; z-index: 100; animation: windowOpen 0.3s ease-out;
    }
    @keyframes windowOpen { from { transform: translate(-50%, -48%) scale(0.9); opacity: 0; } }
    .modal-overlay { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.85); z-index: 300; display: flex; justify-content: center; align-items: center; }
    .custom-modal { background: #161b22; border: 1px solid #58a6ff; padding: 35px; border-radius: 12px; text-align: center; max-width: 500px; border-top: 5px solid #58a6ff; }

    /* LAB & LOG PANEL */
    .lab-content { display: flex; flex: 1; overflow: hidden; padding: 20px; gap: 20px; }
    .log-panel { flex: 3; background: #010409; border: 1px solid #30363d; border-radius: 6px; display: flex; flex-direction: column; }
    #log-screen { flex: 1; overflow-y: scroll; padding: 15px; font-size: 11px; color: #8b949e; line-height: 1.8; }
    
    /* INTERACTIVE PLAYBOOK */
    .playbook-step { background: rgba(88, 166, 255, 0.05); border: 1px solid #30363d; border-left: 4px solid #3fb950; padding: 20px; margin-bottom: 20px; border-radius: 0 8px 8px 0; }
    .step-action-btn { background: #21262d; border: 1px solid #30363d; color: #58a6ff; padding: 8px 15px; border-radius: 4px; cursor: pointer; font-size: 11px; margin-top: 10px; transition: 0.2s; }
    .step-action-btn:hover { background: #30363d; color: #fff; }

    /* BUTTONS & INPUTS */
    .btn-action { background: #238636; border: none; color: white; padding: 14px 28px; border-radius: 6px; cursor: pointer; font-weight: bold; font-family: inherit; }
    .btn-action:hover { background: #2ea043; box-shadow: 0 0 20px rgba(46, 160, 67, 0.4); }
    .input-field { width: 100%; background: #0d1117; border: 1px solid #30363d; color: #58a6ff; padding: 12px; margin-top: 10px; border-radius: 6px; outline: none; }
    .correct-ans { border-color: #238636 !important; background: rgba(35, 134, 54, 0.1) !important; }
    .wrong-ans { border-color: #f85149 !important; background: rgba(248, 81, 73, 0.1) !important; }
    .hidden { display: none !important; }
</style>

<div id="desktop-environment">
    <div id="system-modal" class="modal-overlay hidden">
        <div class="custom-modal">
            <h2 id="modal-title" style="color: #58a6ff;">SYSTEM ALERT</h2>
            <p id="modal-body" style="margin: 25px 0; font-size: 14px; color: #e6edf3;"></p>
            <button onclick="closeModal()" class="btn-action">ACKNOWLEDGE</button>
        </div>
    </div>

    <div class="desktop-icons" style="padding: 25px;">
        <div onclick="openWindow('auth-window')" style="width:100px; text-align:center; cursor:pointer; margin-bottom: 30px;">
            <img src="https://cdn-icons-png.flaticon.com/512/3233/3233514.png" width="55">
            <span style="font-size:11px; margin-top:8px; display:block;">Login.sys</span>
        </div>
        <div id="range-icon" class="hidden" onclick="openWindow('selection-window')" style="width:100px; text-align:center; cursor:pointer;">
            <img src="https://cdn-icons-png.flaticon.com/512/906/906343.png" width="55">
            <span style="font-size:11px; margin-top:8px; display:block;">CyberRange.exe</span>
        </div>
    </div>

    <div id="auth-window" class="window">
        <div style="padding: 80px; text-align: center; max-width: 450px; margin: 0 auto;">
            <h3 style="color: #58a6ff;">ANALYST AUTHENTICATION</h3>
            <input type="text" id="alias-input" class="input-field" placeholder="Codename / Kod Adı...">
            <br><br>
            <button onclick="loginAnalyst()" class="btn-action" style="width: 100%;">START SESSION</button>
        </div>
    </div>

    <div id="selection-window" class="window hidden">
        <div id="selection-content" style="padding: 40px; overflow-y: auto;">
            <h4 style="color: #58a6ff; border-bottom: 1px solid #30363d; padding-bottom: 10px;">MISSION CONTROL</h4>
            <div id="machine-list" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 20px; margin-top: 30px;"></div>
        </div>

        <div id="briefing-content" class="hidden" style="padding: 40px; overflow-y: auto; height: 100%;">
            <h2 id="brief-lab-name" style="color: #58a6ff;">BRIEFING</h2>
            <div id="brief-details-area" style="margin: 25px 0;"></div>
            <div style="display: flex; gap: 15px;">
                <button onclick="hideBriefing()" class="btn-action" style="background:#30363d;">CANCEL</button>
                <button onclick="startLabAfterBrief()" class="btn-action" style="flex: 1;">ENTER TELEMETRY VAULT</button>
            </div>
        </div>
    </div>

    <div id="lab-window" class="window hidden">
        <div class="lab-content">
            <div class="log-panel">
                <div style="background:#161b22; padding:8px 15px; font-size:10px; display:flex; justify-content:space-between;">
                    <span style="color: #3fb950;">[LIVE] SIEM CONSOLE</span>
                    <span id="log-count">0 EVENTS</span>
                </div>
                <div id="log-screen"></div>
            </div>
            <div class="question-panel">
                <h4 style="color:#f85149; margin-top:0;">Investigation Checklist</h4>
                <div id="q-area"></div>
                <button onclick="checkLabAnswers()" class="btn-action" style="width: 100%; margin-top: 25px;">SUBMIT ANALYSIS</button>
            </div>
        </div>
    </div>

    <div id="playbook-window" class="window hidden">
        <div style="padding: 40px; overflow-y: auto;">
            <div style="text-align:center;">
                <h2 style="color: #3fb950;">VERIFICATION COMPLETE</h2>
                <p style="color: #8b949e;">L1 Analysis finished. Execute Incident Response Actions / IR Adımlarını Uygulayın:</p>
            </div>
            <hr style="border: 0.5px solid #30363d; margin: 30px 0;">
            <div id="playbook-actions"></div>
            <div style="text-align: center; margin-top: 30px;">
                <button onclick="closePlaybook()" class="btn-action" style="background:#30363d; width: auto;">CLOSE INCIDENT</button>
            </div>
        </div>
    </div>

</div>



<script>
    const labData = {
        easy: {
            name: "OP-01: Brute Force Detection",
            briefing: `<div style="background:rgba(88,166,255,0.05); padding:15px; border-radius:8px; border:1px solid #30363d;">
                <p style="color:#58a6ff; font-weight:bold; margin-bottom:5px;">[EN] MISSION:</p><p style="font-size:12px; margin-bottom:15px;">A critical server is under brute-force pressure. Find the attacker IP and verify success. / Kritik bir sunucu saldırı altında. IP'yi bulun ve başarısını kontrol edin.</p>
                <p style="color:#3fb950; font-weight:bold; margin-bottom:5px;">[TR] ANALİZ REHBERİ:</p><p style="font-size:12px;">Windows EventID <b>4625</b> (Hata) ve <b>4624</b> (Başarı) loglarına odaklanın.</p>
            </div>`,
            playbookActions: [
                { en: "Reset compromised account password.", tr: "Ele geçirilen hesabın şifresini sıfırla.", btn: "GO / SIFIRLA", type: "password" },
                { en: "Escalate incident to L2 (Tier 2) Analyst.", tr: "Vakayı L2 Analistine devret.", btn: "ESCALATE / DEVRET", type: "escalate" }
            ],
            questions: [{q: "Attacker IP?", a: "185.22.155.10"}, {q: "Success? (Yes/No)", a: "yes"}],
            generateLogs: () => {
                let l = []; for(let i=0; i<120; i++) l.push(`[${new Date().toLocaleTimeString()}] INFO Audit: EventID=4624 source=10.0.1.${i} status=Success msg="Normal logon"`);
                for(let i=0; i<25; i++) l.push(`[${new Date().toLocaleTimeString()}] WARN Audit: EventID=4625 source=185.22.155.10 user=admin status=Failure msg="Bad password"`);
                l.push(`[${new Date().toLocaleTimeString()}] CRIT Audit: EventID=4624 source=185.22.155.10 user=admin status=Success msg="Logon after failure"`);
                return l.reverse();
            }
        },
        medium: {
            name: "OP-02: SQL Injection & Exfiltration",
            briefing: `<div style="background:rgba(88,166,255,0.05); padding:15px; border-radius:8px; border:1px solid #30363d;">
                <p style="color:#58a6ff; font-weight:bold; margin-bottom:5px;">[EN] MISSION:</p><p style="font-size:12px; margin-bottom:15px;">Detect SQLi patterns in web logs and identify stolen table. / Web loglarındaki SQLi örüntülerini ve çalınan tabloyu bulun.</p>
                <p style="color:#3fb950; font-weight:bold; margin-bottom:5px;">[TR] ANALİZ REHBERİ:</p><p style="font-size:12px;">Apache loglarında <b>UNION SELECT</b> ve <b>sqlmap</b> izlerini arayın.</p>
            </div>`,
            playbookActions: [
                { en: "Block Attacker IP on Palo Alto Firewall.", tr: "Saldırgan IP'sini Firewall'da engelle.", btn: "BLOCK IP / ENGELLE", type: "block" },
                { en: "Isolate Web Server from Network.", tr: "Web Sunucusunu ağdan izole et.", btn: "ISOLATE / İZOLE ET", type: "isolate" }
            ],
            questions: [{q: "Attacker IP?", a: "45.155.205.233"}, {q: "Table Name?", a: "payments"}],
            generateLogs: () => {
                let l = []; for(let i=0; i<100; i++) l.push(`[${new Date().toLocaleTimeString()}] INFO Apache: 192.168.1.${i} - GET /index.php 200`);
                l.push(`[12:15:00] WARN Apache: 45.155.205.233 - "GET /search?id=1' UNION SELECT * FROM payments" 200 "sqlmap/1.5"`);
                return l.reverse();
            }
        }
    };

    let userAlias = ""; let currentLabKey = "";

    function openModal(title, body) {
        document.getElementById('modal-title').innerText = title;
        document.getElementById('modal-body').innerText = body;
        document.getElementById('system-modal').classList.remove('hidden');
    }
    function closeModal() { document.getElementById('system-modal').classList.add('hidden'); }

    function loginAnalyst() {
        userAlias = document.getElementById('alias-input').value.trim(); if(!userAlias) return;
        localStorage.setItem('analyst_alias', userAlias);
        document.getElementById('range-icon').classList.remove('hidden');
        closeWindow('auth-window'); openWindow('selection-window'); renderSelection();
        openModal("TERMINAL AUTHORIZED", `Welcome, Analyst ${userAlias}. All systems initialized.`);
    }

    function renderSelection() {
        const container = document.getElementById('machine-list');
        container.innerHTML = Object.keys(labData).map(key => `
            <div onclick="showBriefing('${key}')" style="background:#161b22; border:1px solid #30363d; padding:25px; border-radius:10px; cursor:pointer; transition:0.3s;">
                <div style="font-weight:bold; color:#fff;">${labData[key].name}</div>
            </div>
        `).join('');
    }

    function showBriefing(key) {
        currentLabKey = key; document.getElementById('selection-content').classList.add('hidden');
        document.getElementById('briefing-content').classList.remove('hidden');
        document.getElementById('brief-details-area').innerHTML = labData[key].briefing;
    }

    function hideBriefing() { document.getElementById('selection-content').classList.remove('hidden'); document.getElementById('briefing-content').classList.add('hidden'); }

    function startLabAfterBrief() {
        closeWindow('selection-window'); openWindow('lab-window');
        const lab = labData[currentLabKey];
        document.getElementById('log-screen').innerHTML = lab.generateLogs().map(l => `<div style="border-bottom:1px solid #161b22; padding:3px 0;">${l}</div>`).join('');
        document.getElementById('log-count').innerText = `${lab.generateLogs().length} EVENTS LOADED`;
        document.getElementById('q-area').innerHTML = lab.questions.map((q, i) => `<div style="margin-bottom:15px;"><label style="font-size:11px; color:#8b949e;">${q.q}</label><input type="text" id="ans-${i}" class="input-field"></div>`).join('');
    }

    function checkLabAnswers() {
        const lab = labData[currentLabKey]; let correct = 0;
        lab.questions.forEach((q, i) => {
            const el = document.getElementById(`ans-${i}`);
            if(el.value.trim().toLowerCase() === q.a.toLowerCase()) { correct++; el.classList.add('correct-ans'); } else { el.classList.add('wrong-ans'); }
        });

        if(correct === lab.questions.length) {
            openModal("VERIFICATION SUCCESS", "Evidence matches. Forwarding to Incident Response Action Center...");
            setTimeout(() => { closeWindow('lab-window'); openWindow('playbook-window'); renderPlaybook(); }, 2000);
        } else {
            openModal("EVIDENCE MISMATCH", `${lab.questions.length - correct} artifacts could not be verified. Check logs for precision.`);
        }
    }

    function renderPlaybook() {
        const lab = labData[currentLabKey];
        const area = document.getElementById('playbook-actions');
        area.innerHTML = lab.playbookActions.map(action => `
            <div class="playbook-step">
                <div style="font-size:13px; color:#fff;"><b>[EN]</b> ${action.en}</div>
                <div style="font-size:13px; color:#8b949e; margin-top:5px;"><b>[TR]</b> ${action.tr}</div>
                <button class="step-action-btn" onclick="executePlaybookAction('${action.type}')">${action.btn}</button>
            </div>
        `).join('');
    }

    function executePlaybookAction(type) {
        let msg = "";
        if(type === "password") msg = "SYSTEM: User 'admin' password has been successfully reset. User forced to logout.";
        if(type === "escalate") msg = `SYSTEM: Case transferred to L2 Shift Lead. Notification sent.`;
        if(type === "block") msg = "FIREWALL: Attacker IP has been permanently blacklisted on all zones.";
        if(type === "isolate") msg = "NETWORK: VLAN 100 isolated. Connection to web-srv-01 severed.";
        
        openModal("ACTION EXECUTED", msg);
    }

    function closePlaybook() { closeWindow('playbook-window'); openWindow('selection-window'); hideBriefing(); }
    function openWindow(id) { document.getElementById(id).classList.remove('hidden'); }
    function closeWindow(id) { document.getElementById(id).classList.add('hidden'); }
</script>
