---
layout: page
title: "SentinelGuard Professional Workstation"
permalink: /labs/station/
---

<style>
    /* FULL DARK RESET */
    html, body, main { background-color: #010409 !important; color: #c9d1d9 !important; margin: 0 !important; padding: 0 !important; width: 100%; max-width: 100% !important; }

    #desktop-environment {
        background: radial-gradient(circle, #0d1117 0%, #010409 100%);
        height: 85vh; position: relative; overflow: hidden; border: 2px solid #30363d; margin: 15px; border-radius: 12px; display: flex; flex-direction: column; font-family: 'JetBrains Mono', monospace;
    }

    /* WINDOWS & MODALS */
    .window {
        position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
        width: 95%; height: 90%; background: #0d1117; border: 1px solid #30363d; border-radius: 10px; box-shadow: 0 30px 60px rgba(0,0,0,0.8); display: flex; flex-direction: column; z-index: 100; animation: windowOpen 0.3s ease-out;
    }
    @keyframes windowOpen { from { transform: translate(-50%, -48%) scale(0.9); opacity: 0; } }

    .modal-overlay {
        position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 200; display: flex; justify-content: center; align-items: center;
    }
    .custom-modal {
        background: #161b22; border: 1px solid #58a6ff; padding: 30px; border-radius: 12px; text-align: center; max-width: 500px;
    }

    /* LAB & PLAYBOOK CONTENT */
    .lab-content { display: flex; flex: 1; overflow: hidden; padding: 20px; gap: 20px; }
    .log-panel { flex: 2.5; background: #010409; border: 1px solid #30363d; border-radius: 6px; display: flex; flex-direction: column; }
    #log-screen { flex: 1; overflow-y: scroll; padding: 15px; font-size: 11px; color: #8b949e; line-height: 1.8; }
    
    .playbook-step { background: rgba(88, 166, 255, 0.05); border-left: 4px solid #58a6ff; padding: 15px; margin-bottom: 10px; font-size: 0.8rem; }
    .lang-label { font-size: 10px; color: #58a6ff; font-weight: bold; margin-bottom: 2px; text-transform: uppercase; }

    /* BUTTONS */
    .btn-action { background: #238636; border: none; color: white; padding: 12px 20px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; }
    .btn-action:hover { background: #2ea043; }
    .input-field { width: 100%; background: #0d1117; border: 1px solid #30363d; color: #58a6ff; padding: 10px; margin-top: 5px; border-radius: 5px; outline: none; }
    .correct-ans { border-color: #238636 !important; background: rgba(35, 134, 54, 0.1) !important; }
    .wrong-ans { border-color: #f85149 !important; background: rgba(248, 81, 73, 0.1) !important; }
    .hidden { display: none !important; }
</style>

<div id="desktop-environment">
    <div class="desktop-icons" style="padding: 20px;">
        <div onclick="openWindow('auth-window')" style="width:80px; text-align:center; cursor:pointer;">
            <img src="https://cdn-icons-png.flaticon.com/512/3233/3233514.png" width="45">
            <span style="display:block; font-size:11px;">Login.sys</span>
        </div>
        <div id="range-icon" class="hidden" onclick="openWindow('selection-window')" style="width:80px; text-align:center; cursor:pointer; margin-top:30px;">
            <img src="https://cdn-icons-png.flaticon.com/512/906/906343.png" width="45">
            <span style="display:block; font-size:11px;">CyberRange.exe</span>
        </div>
    </div>

    <div id="system-modal" class="modal-overlay hidden">
        <div class="custom-modal">
            <h2 id="modal-title" style="color: #58a6ff;">SYSTEM MESSAGE</h2>
            <p id="modal-body" style="margin: 20px 0; font-size: 14px;"></p>
            <button onclick="closeModal()" class="btn-action">ACKNOWLEDGE</button>
        </div>
    </div>

    <div id="auth-window" class="window">
        <div style="padding: 60px; text-align: center; max-width: 400px; margin: 0 auto;">
            <h3 style="color: #58a6ff;">AUTHENTICATION</h3>
            <p style="font-size: 11px;">Kod Adınızı Girin / Enter Analyst Alias</p>
            <input type="text" id="alias-input" class="input-field" placeholder="Username...">
            <br><br>
            <button onclick="loginAnalyst()" class="btn-action">INITIALIZE SESSION</button>
        </div>
    </div>

    <div id="selection-window" class="window hidden">
        <div id="selection-content" style="padding: 30px; overflow-y: auto;">
            <h4 style="color: #58a6ff;">ACTIVE OPERATIONS</h4>
            <div id="machine-list" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; margin-top: 20px;"></div>
        </div>
        <div id="briefing-content" class="hidden" style="padding: 30px; overflow-y: auto;">
            <h3 id="brief-lab-name" style="color: #58a6ff;">MISSION</h3>
            <div id="brief-details"></div>
            <div style="display: flex; gap: 15px;">
                <button onclick="hideBriefing()" class="btn-action" style="background:#30363d;">BACK</button>
                <button onclick="startLabAfterBrief()" class="btn-action">GO LIVE / BAŞLAT</button>
            </div>
        </div>
    </div>

    <div id="lab-window" class="window hidden">
        <div class="lab-content">
            <div class="log-panel">
                <div id="log-screen"></div>
            </div>
            <div class="question-panel">
                <h4 style="color:#f85149; margin-top:0;">Investigation Checklist</h4>
                <div id="q-area"></div>
                <button onclick="checkLabAnswers()" class="btn-action" style="margin-top: 20px;">SUBMIT EVIDENCE</button>
            </div>
        </div>
    </div>

    <div id="playbook-window" class="window hidden">
        <div style="padding: 30px; overflow-y: auto;">
            <h2 style="color: #3fb950;">🎉 MISSION ACCOMPLISHED / GÖREV TAMAMLANDI</h2>
            <p style="color: #8b949e;">Evidence verified. Follow the Incident Response Playbook below:</p>
            <hr style="border: 0.5px solid #30363d; margin: 20px 0;">
            <div id="playbook-steps"></div>
            <button onclick="closePlaybook()" class="btn-action" style="margin-top:20px;">FINISH / BİTİR</button>
        </div>
    </div>

</div>

<script>
    const labData = {
        easy: {
            name: "OP-01: Brute Force Attack",
            details: `
                <div class="lang-label">English</div><p style="font-size:0.8rem;">A brute-force attack involves repeatedly trying different passwords until the correct one is found.</p>
                <div class="lang-label">Türkçe</div><p style="font-size:0.8rem;">Brute Force, bir saldırganın doğru şifreyi bulana kadar farklı şifreleri denemesidir.</p>
            `,
            playbook: `
                <div class="playbook-step"><b>Step 1:</b> Change the compromised user's password immediately. <br> <b>Adım 1:</b> Ele geçirilen kullanıcının şifresini derhal değiştirin.</div>
                <div class="playbook-step"><b>Step 2:</b> Implement Account Lockout Policies. <br> <b>Adım 2:</b> Hesap Kilitleme Politikalarını devreye alın.</div>
            `,
            questions: [{q: "Attacker IP?", a: "185.22.155.10"}, {q: "Target User?", a: "admin"}],
            generateLogs: () => {
                let l = []; for(let i=0; i<10; i++) l.push(`[${new Date().toLocaleTimeString()}] INFO Audit: EventID=4625 source=185.22.155.10 user=admin status=Failure`);
                l.push(`[${new Date().toLocaleTimeString()}] INFO Audit: EventID=4624 source=185.22.155.10 user=admin status=Success`);
                return l.reverse();
            }
        },
        medium: {
            name: "OP-02: SQL Injection",
            details: `
                <div class="lang-label">English</div><p style="font-size:0.8rem;">SQL Injection targets web application databases via URL parameters.</p>
                <div class="lang-label">Türkçe</div><p style="font-size:0.8rem;">SQL Injection, URL parametreleri üzerinden veritabanını hedefleyen bir saldırıdır.</p>
            `,
            playbook: `
                <div class="playbook-step"><b>Step 1:</b> Patch the vulnerable parameter (id). <br> <b>Adım 1:</b> Güvenlik açığı olan parametreyi (id) yamalayın.</div>
                <div class="playbook-step"><b>Step 2:</b> Reset all database credentials. <br> <b>Adım 2:</b> Tüm veritabanı yetki bilgilerini sıfırlayın.</div>
            `,
            questions: [{q: "Attacker IP?", a: "45.155.205.233"}, {q: "Table Name?", a: "payments"}],
            generateLogs: () => {
                let l = []; l.push(`[${new Date().toLocaleTimeString()}] INFO Apache: 45.155.205.233 - "GET /search.php?id=1' UNION SELECT * FROM payments" 200`);
                return l;
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
        document.getElementById('range-icon').classList.remove('hidden');
        closeWindow('auth-window'); openWindow('selection-window'); renderSelection();
        openModal("ACCESS GRANTED", `Welcome back, Analyst ${userAlias}.`);
    }

    function renderSelection() {
        const container = document.getElementById('machine-list');
        container.innerHTML = Object.keys(labData).map(key => `
            <div onclick="showBriefing('${key}')" style="background:#161b22; border:1px solid #30363d; padding:20px; border-radius:10px; cursor:pointer;">
                <div style="font-weight:bold; color:#fff;">${labData[key].name}</div>
            </div>
        `).join('');
    }

    function showBriefing(key) {
        currentLabKey = key; document.getElementById('selection-content').classList.add('hidden');
        document.getElementById('briefing-content').classList.remove('hidden');
        document.getElementById('brief-details').innerHTML = labData[key].details;
    }

    function hideBriefing() { document.getElementById('selection-content').classList.remove('hidden'); document.getElementById('briefing-content').classList.add('hidden'); }

    function startLabAfterBrief() {
        closeWindow('selection-window'); openWindow('lab-window');
        const lab = labData[currentLabKey];
        document.getElementById('log-screen').innerHTML = lab.generateLogs().map(l => `<div style="margin-bottom:4px; border-bottom:1px solid #161b22;">${l}</div>`).join('');
        document.getElementById('q-area').innerHTML = lab.questions.map((q, i) => `<div style="margin-bottom:15px;"><label style="font-size:11px; color:#8b949e;">${q.q}</label><input type="text" id="ans-${i}" class="input-field"></div>`).join('');
    }

    function checkLabAnswers() {
        const lab = labData[currentLabKey]; let correct = 0;
        lab.questions.forEach((q, i) => {
            const el = document.getElementById(`ans-${i}`);
            if(el.value.trim().toLowerCase() === q.a.toLowerCase()) { correct++; el.classList.add('correct-ans'); } else { el.classList.add('wrong-ans'); }
        });
        if(correct === lab.questions.length) {
            openModal("VERIFICATION SUCCESS", "Investigation complete. Moving to Incident Response Playbook...");
            setTimeout(() => { closeWindow('lab-window'); openWindow('playbook-window'); document.getElementById('playbook-steps').innerHTML = lab.playbook; }, 2000);
        } else {
            openModal("EVIDENCE MISMATCH", "Some artifacts are incorrect. Check logs again.");
        }
    }

    function closePlaybook() { closeWindow('playbook-window'); openWindow('selection-window'); hideBriefing(); }
    function openWindow(id) { document.getElementById(id).classList.remove('hidden'); }
    function closeWindow(id) { document.getElementById(id).classList.add('hidden'); }
</script>
