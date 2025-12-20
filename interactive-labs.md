---
layout: page
title: "SentinelGuard Advanced SOC Station"
permalink: /labs/station/
---

<style>
    /* BASE DARK THEME */
    html, body, main { background-color: #010409 !important; color: #c9d1d9 !important; margin: 0 !important; padding: 0 !important; width: 100%; max-width: 100% !important; }
    #desktop-environment {
        background: radial-gradient(circle, #0d1117 0%, #010409 100%);
        height: 88vh; position: relative; overflow: hidden; border: 2px solid #30363d; margin: 15px; border-radius: 12px; display: flex; flex-direction: column; font-family: 'JetBrains Mono', monospace;
    }

    /* WINDOWS & EDR TERMINAL */
    .window {
        position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
        width: 95%; height: 92%; background: #0d1117; border: 1px solid #30363d; border-radius: 10px; box-shadow: 0 30px 60px rgba(0,0,0,0.8); display: flex; flex-direction: column; z-index: 100;
    }
    .edr-terminal { background: #000; color: #3fb950; padding: 15px; font-family: 'Courier New', monospace; height: 150px; overflow-y: auto; border: 1px solid #30363d; margin-top: 10px; border-radius: 4px; }

    /* MODALS */
    .modal-overlay { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); z-index: 500; display: flex; justify-content: center; align-items: center; }
    .custom-modal { background: #161b22; border: 1px solid #58a6ff; padding: 30px; border-radius: 12px; text-align: center; max-width: 550px; border-top: 5px solid #58a6ff; }

    /* UI ELEMENTS */
    .playbook-step { background: rgba(88, 166, 255, 0.05); border: 1px solid #30363d; padding: 20px; margin-bottom: 20px; border-radius: 8px; }
    .btn-action { background: #238636; border: none; color: white; padding: 12px 24px; border-radius: 6px; cursor: pointer; font-weight: bold; }
    .input-field { width: 100%; background: #0d1117; border: 1px solid #30363d; color: #58a6ff; padding: 10px; margin-top: 5px; border-radius: 5px; outline: none; }
    .hidden { display: none !important; }
    .correct-ans { border-color: #238636 !important; }
    .wrong-ans { border-color: #f85149 !important; }
</style>

<div id="desktop-environment">
    <div id="system-modal" class="modal-overlay hidden">
        <div class="custom-modal">
            <h2 id="modal-title" style="color: #58a6ff;">SYSTEM ALERT</h2>
            <div id="modal-body" style="margin: 20px 0; font-size: 14px;"></div>
            <button onclick="closeModal()" class="btn-action">CLOSE</button>
        </div>
    </div>

    <div class="desktop-icons" style="padding: 25px;">
        <div onclick="openWindow('auth-window')" style="width:100px; text-align:center; cursor:pointer;">
            <img src="https://cdn-icons-png.flaticon.com/512/3233/3233514.png" width="50">
            <span style="display:block; font-size:11px; margin-top:5px;">Login.sys</span>
        </div>
        <div id="range-icon" class="hidden" onclick="openWindow('selection-window')" style="width:100px; text-align:center; cursor:pointer; margin-top:30px;">
            <img src="https://cdn-icons-png.flaticon.com/512/906/906343.png" width="50">
            <span style="display:block; font-size:11px; margin-top:5px;">CyberRange.exe</span>
        </div>
    </div>

    <div id="lab-window" class="window hidden">
        <div style="display: flex; flex: 1; overflow: hidden; padding: 20px; gap: 20px;">
            <div style="flex: 3; display: flex; flex-direction: column;">
                <div style="background:#161b22; padding:8px; font-size:10px; border:1px solid #30363d;">RAW SIEM TELEMETRY</div>
                <div id="log-screen" style="flex:1; background:#010409; overflow-y:scroll; padding:15px; font-size:11px; color:#8b949e; line-height:1.8; border:1px solid #30363d;"></div>
            </div>
            <div style="flex: 1.2; background:#161b22; padding:20px; border-radius:8px; border:1px solid #30363d; overflow-y:auto;">
                <h4 style="color:#f85149; margin-top:0;">Investigation Questions</h4>
                <div id="q-area"></div>
                <button onclick="checkLabAnswers()" class="btn-action" style="width: 100%; margin-top: 20px;">VALIDATE EVIDENCE</button>
            </div>
        </div>
    </div>

    <div id="playbook-window" class="window hidden">
        <div style="padding: 40px; overflow-y: auto;">
            <h2 style="color: #3fb950;">ACTION CENTER: INCIDENT RESPONSE</h2>
            <div id="playbook-content">
                </div>
        </div>
    </div>

</div>



<script>
    const labData = {
        easy: {
            name: "OP-01: Windows Brute Force",
            questions: [
                {q: "Attacker IP?", a: "185.22.155.10"},
                {q: "How many failed attempts before success?", a: "12"},
                {q: "What is the suspicious EventID for failure?", a: "4625"}
            ],
            generateLogs: () => {
                let l = []; for(let i=0; i<50; i++) l.push(`[${new Date().toLocaleTimeString()}] INFO Audit: EventID=4624 source=10.0.0.${i} status=Success`);
                for(let i=0; i<12; i++) l.push(`[14:40:05] WARN Audit: EventID=4625 source=185.22.155.10 user=admin msg="Failure"`);
                l.push(`[14:41:00] CRIT Audit: EventID=4624 source=185.22.155.10 user=admin msg="Success after Brute Force"`);
                return l.reverse();
            }
        },
        medium: {
            name: "OP-02: Web SQLi & Exfiltration",
            questions: [
                {q: "Attacker IP?", a: "45.155.205.233"},
                {q: "Which specific SQL keyword was used?", a: "UNION"},
                {q: "What automation tool is detected?", a: "sqlmap"}
            ],
            generateLogs: () => {
                let l = []; for(let i=0; i<60; i++) l.push(`[12:10:${i}] INFO Apache: 192.168.1.${i} GET / HTTP/1.1 200`);
                l.push(`[12:15:22] WARN Apache: 45.155.205.233 "GET /search.php?id=1' UNION SELECT * FROM users" 200 agent="sqlmap/1.5"`);
                return l.reverse();
            }
        }
    };

    let userAlias = ""; let currentKey = "";

    function openWindow(id) { document.getElementById(id).classList.remove('hidden'); }
    function closeWindow(id) { document.getElementById(id).classList.add('hidden'); }
    function openModal(title, body) { document.getElementById('modal-title').innerText = title; document.getElementById('modal-body').innerHTML = body; document.getElementById('system-modal').classList.remove('hidden'); }
    function closeModal() { document.getElementById('system-modal').classList.add('hidden'); }

    function loginAnalyst() {
        userAlias = document.getElementById('alias-input').value.trim();
        if(!userAlias) return;
        document.getElementById('range-icon').classList.remove('hidden');
        closeWindow('auth-window');
        openModal("ACCESS GRANTED", `Welcome Analyst ${userAlias}. Cyber Range is now active.`);
    }

    // Lab Başlatma ve Seçim Fonksiyonları buraya gelecek...
    // (Önceki kodlardaki seçim ekranı mantığı aynı kalacak)

    function startLab(key) {
        currentKey = key;
        const lab = labData[key];
        closeWindow('selection-window');
        openWindow('lab-window');
        document.getElementById('log-screen').innerHTML = lab.generateLogs().map(l => `<div style="border-bottom:1px solid #161b22; padding:3px 0;">${l}</div>`).join('');
        document.getElementById('q-area').innerHTML = lab.questions.map((q, i) => `
            <div style="margin-bottom:15px;">
                <label style="font-size:11px; color:#8b949e;">${q.q}</label>
                <input type="text" id="ans-${i}" class="input-field">
            </div>
        `).join('');
    }

    function checkLabAnswers() {
        const lab = labData[currentKey]; let correct = 0;
        lab.questions.forEach((q, i) => {
            const el = document.getElementById(`ans-${i}`);
            if(el.value.trim().toLowerCase() === q.a.toLowerCase()) { correct++; el.classList.add('correct-ans'); }
            else { el.classList.add('wrong-ans'); }
        });
        if(correct === lab.questions.length) {
            openModal("VERIFIED", "Evidence correct. Proceeding to EDR & Escalation center.");
            setTimeout(() => { closeWindow('lab-window'); openWindow('playbook-window'); renderAdvancedPlaybook(); }, 2000);
        } else { openModal("ERROR", "Artifacts do not match telemetry."); }
    }

    function renderAdvancedPlaybook() {
        const area = document.getElementById('playbook-content');
        area.innerHTML = `
            <div class="playbook-step">
                <h4>1. EDR Remote Console (Password Reset)</h4>
                <p>Connect to the compromised machine via EDR and reset the 'admin' password.</p>
                <div class="edr-terminal" id="edr-term">
                    <div>[SYS] EDR Connection Established to SRV-PROD...</div>
                    <div>[SYS] Type 'reset admin --new [password]' to proceed.</div>
                    <div id="edr-history"></div>
                    <div style="display:flex;"><span>$</span><input type="text" id="edr-input" style="background:none; border:none; color:#3fb950; outline:none; width:100%;" onkeydown="handleEDR(event)"></div>
                </div>
            </div>

            <div class="playbook-step">
                <h4>2. L2 Escalation Report</h4>
                <label>Classification:</label>
                <select id="escalate-type" class="input-field" style="margin-bottom:15px;">
                    <option value="">-- Select --</option>
                    <option value="tp">True Positive (Confirmed Attack)</option>
                    <option value="fp">False Positive (False Alarm)</option>
                </select>
                <label>Analyst Note for L2:</label>
                <textarea id="analyst-note" class="input-field" placeholder="Describe your findings..."></textarea>
                <button onclick="finalEscalate()" class="btn-action" style="margin-top:20px; width:100%;">ESCALATE TO L2</button>
            </div>
        `;
    }

    function handleEDR(e) {
        if(e.key === "Enter") {
            const cmd = e.target.value;
            const history = document.getElementById('edr-history');
            if(cmd.startsWith("reset admin --new")) {
                history.innerHTML += `<div>$ ${cmd}</div><div style="color:#58a6ff;">[SUCCESS] Password for user 'admin' changed successfully. Session killed.</div>`;
            } else {
                history.innerHTML += `<div>$ ${cmd}</div><div style="color:#f85149;">[ERROR] Unknown command.</div>`;
            }
            e.target.value = "";
        }
    }

    function finalEscalate() {
        const type = document.getElementById('escalate-type').value;
        const note = document.getElementById('analyst-note').value;
        if(!type || !note) { alert("Please complete the report before escalation."); return; }
        
        openModal("MISSION COMPLETE", `Vaka L2 birimine başarıyla devredildi.<br><b>Sınıflandırma:</b> ${type.toUpperCase()}<br><b>Not:</b> ${note}`);
        setTimeout(() => { location.reload(); }, 4000);
    }

    // (Eksik kalan renderSelection fonksiyonu eklenecektir...)
    function renderSelection() {
        const container = document.getElementById('machine-list');
        // ... (Seçim listesi oluşturma kodu)
    }
</script>
