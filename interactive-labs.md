---
layout: page
title: "SentinelGuard Ultimate SOC Station"
permalink: /labs/station/
---

<style>
    /* 1. GLOBAL RESET - BEYAZ ALANLARI YOK ETME */
    html, body, main { 
        background-color: #010409 !important; 
        color: #c9d1d9 !important; 
        margin: 0 !important; 
        padding: 0 !important;
        width: 100%;
        height: 100%;
        overflow: hidden;
    }

    /* 2. MASAÜSTÜ ORTAMI */
    #desktop-environment {
        background: radial-gradient(circle, #0d1117 0%, #010409 100%);
        height: 100vh;
        width: 100vw;
        position: relative;
        overflow: hidden;
        font-family: 'JetBrains Mono', monospace;
    }

    /* 3. MASAÜSTÜ İKONLARI */
    .desktop-icons { 
        position: absolute;
        top: 20px;
        left: 20px;
        display: flex; 
        flex-direction: column; 
        gap: 25px; 
        z-index: 10;
    }
    .icon { 
        width: 90px; 
        text-align: center; 
        cursor: pointer; 
        transition: 0.2s; 
        padding: 10px; 
        border-radius: 8px;
    }
    .icon:hover { background: rgba(88, 166, 255, 0.1); }
    .icon img { width: 45px; display: block; margin: 0 auto 5px auto; pointer-events: none; }
    .icon span { font-size: 11px; color: #fff; pointer-events: none; text-shadow: 1px 1px 2px #000; }

    /* 4. PENCERE VE MODAL SİSTEMİ */
    .window {
        position: absolute; 
        top: 45%; 
        left: 50%; 
        transform: translate(-50%, -50%);
        width: 95%; 
        height: 85%; 
        background: #0d1117; 
        border: 1px solid #30363d; 
        border-radius: 10px; 
        box-shadow: 0 30px 60px rgba(0,0,0,0.9); 
        display: flex; 
        flex-direction: column; 
        z-index: 100;
    }
    .window-header { 
        background: #161b22; 
        padding: 10px 20px; 
        display: flex; 
        justify-content: space-between; 
        align-items: center; 
        border-bottom: 1px solid #30363d; 
        border-radius: 10px 10px 0 0; 
    }
    .close-btn { color: #f85149; cursor: pointer; font-size: 22px; }

    .modal-overlay { 
        position: absolute; 
        top: 0; left: 0; 
        width: 100%; height: 100%; 
        background: rgba(0,0,0,0.9); 
        z-index: 500; 
        display: flex; 
        justify-content: center; 
        align-items: center; 
    }
    .custom-modal { 
        background: #161b22; 
        border: 1px solid #58a6ff; 
        padding: 30px; 
        border-radius: 12px; 
        text-align: center; 
        max-width: 500px; 
        border-top: 5px solid #58a6ff; 
    }

    /* 5. GÖREV ÇUBUĞU (TASKBAR - DÜZENLENMİŞ) */
    #taskbar { 
        background: rgba(22, 27, 34, 0.98); 
        height: 45px; 
        border-top: 1px solid #30363d; 
        display: flex; 
        align-items: center; 
        padding: 0 20px; 
        gap: 20px; 
        z-index: 200; 
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
    }

    /* 6. LAB VE LOG PANELİ */
    .lab-content { display: flex; flex: 1; overflow: hidden; padding: 20px; gap: 20px; }
    #log-screen { flex: 1; overflow-y: scroll; padding: 15px; font-size: 11px; color: #8b949e; line-height: 1.8; background: #010409; border: 1px solid #30363d; white-space: pre-wrap; }
    .edr-terminal { background: #000; color: #3fb950; padding: 15px; font-family: 'Courier New', monospace; height: 150px; overflow-y: auto; border: 1px solid #30363d; margin-top: 15px; border-radius: 6px; }

    /* 7. BUTONLAR VE INPUTLAR */
    .btn-action { background: #238636; border: none; color: white; padding: 12px 24px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; }
    .btn-action:hover { background: #2ea043; box-shadow: 0 0 15px rgba(46,160,67,0.4); }
    .input-field { width: 100%; background: #0d1117; border: 1px solid #30363d; color: #58a6ff; padding: 10px; margin-top: 5px; border-radius: 5px; outline: none; }
    .correct-ans { border-color: #238636 !important; background: rgba(35, 134, 54, 0.1) !important; }
    .wrong-ans { border-color: #f85149 !important; background: rgba(248, 81, 73, 0.1) !important; }
    .hidden { display: none !important; }
</style>

<div id="desktop-environment">
    <div id="system-modal" class="modal-overlay hidden">
        <div class="custom-modal">
            <h2 id="modal-title" style="color: #58a6ff;">STATION ALERT</h2>
            <div id="modal-body" style="margin: 20px 0; font-size: 14px; color: #e6edf3;"></div>
            <button onclick="closeModal()" class="btn-action">ACKNOWLEDGE</button>
        </div>
    </div>

    <div class="desktop-icons">
        <div class="icon" onclick="openWindow('auth-window')">
            <img src="https://cdn-icons-png.flaticon.com/512/3233/3233514.png">
            <span>Login.sys</span>
        </div>
        <div id="range-icon" class="icon hidden" onclick="openWindow('selection-window')">
            <img src="https://cdn-icons-png.flaticon.com/512/906/906343.png">
            <span>CyberRange.exe</span>
        </div>
    </div>

    <div id="auth-window" class="window">
        <div class="window-header"><span>Analyst Auth</span><span onclick="closeWindow('auth-window')" class="close-btn">×</span></div>
        <div style="padding: 60px; text-align: center; max-width: 400px; margin: 0 auto;">
            <h3 style="color: #58a6ff;">SECURE LOGIN</h3>
            <input type="text" id="alias-input" class="input-field" placeholder="Analyst Codename...">
            <br><br>
            <button onclick="loginAnalyst()" class="btn-action">INITIALIZE</button>
        </div>
    </div>

    <div id="selection-window" class="window hidden">
        <div class="window-header"><span>Mission Control Center</span><span onclick="closeWindow('selection-window')" class="close-btn">×</span></div>
        <div id="selection-content" style="padding: 30px; overflow-y: auto;">
            <h4 style="color: #58a6ff; border-bottom: 1px solid #30363d; padding-bottom: 10px;">AVAILABLE MISSIONS</h4>
            <div id="machine-list" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; margin-top: 20px;"></div>
        </div>
        <div id="briefing-content" class="hidden" style="padding: 30px; overflow-y: auto;">
            <h2 id="brief-lab-name" style="color: #58a6ff;">BRIEFING</h2>
            <div id="brief-details" style="background: rgba(88,166,255,0.05); border: 1px solid #30363d; padding: 25px; border-radius: 8px; margin: 20px 0;"></div>
            <div style="display: flex; gap: 15px;">
                <button onclick="hideBriefing()" class="btn-action" style="background:#30363d;">BACK</button>
                <button onclick="startInvestigation()" class="btn-action">GO LIVE</button>
            </div>
        </div>
    </div>

    <div id="lab-window" class="window hidden">
        <div class="window-header"><span id="lab-title-text">Investigation</span><span onclick="closeWindow('lab-window')" class="close-btn">×</span></div>
        <div class="lab-content">
            <div style="flex: 2.8; display: flex; flex-direction: column;">
                <div style="background:#161b22; padding:5px 15px; font-size:10px;">SIEM_TELEMETRY_DATA</div>
                <div id="log-screen"></div>
            </div>
            <div style="flex: 1.2; background: #161b22; padding: 20px; border-radius: 6px; border: 1px solid #30363d; overflow-y: auto;">
                <h4 style="color:#f85149; margin-top:0;">Technical Artifacts</h4>
                <div id="q-area"></div>
                <button onclick="checkAnswers()" class="btn-action" style="margin-top: 20px;">SUBMIT EVIDENCE</button>
            </div>
        </div>
    </div>

    <div id="playbook-window" class="window hidden">
        <div class="window-header"><span>Incident Remediation</span><span onclick="closeWindow('playbook-window')" class="close-btn">×</span></div>
        <div style="padding: 40px; overflow-y: auto;">
            <h2 style="color: #3fb950;">ACTION CENTER</h2>
            <div id="playbook-ui"></div>
        </div>
    </div>

    <div id="taskbar">
        <img src="https://cdn-icons-png.flaticon.com/512/270/270104.png" width="22" style="filter: invert(1);">
        <div id="clock" style="font-size: 12px; color: #8b949e; min-width: 80px;">12:00:00</div>
        <div id="active-user" style="margin-left: auto; font-size: 11px; color: #58a6ff;">GUEST@STATION</div>
    </div>
</div>

<script>
    const labs = {
        brute: {
            name: "ADV-01: Windows Brute Force",
            details: "<b>[EN]</b> Identify the attacker IP and the specific EventID for logon failures.<br><b>[TR]</b> Saldırgan IP'sini ve logon hataları için kullanılan EventID'yi bulun.",
            questions: [
                {q: "Attacker IP Address?", a: "185.22.155.10"},
                {q: "Which EventID represents a Failure?", a: "4625"},
                {q: "Was the attack successful? (Yes/No)", a: "yes"}
            ],
            logs: () => {
                let l = []; for(let i=0; i<60; i++) l.push(`[14:38:42] INFO Audit: EventID=4624 source=10.0.0.${i} status=Success user=jdoe`);
                for(let i=0; i<15; i++) l.push(`[14:40:05] WARN Audit: EventID=4625 source=185.22.155.10 status=Failure user=admin`);
                l.push(`[14:41:00] CRIT Audit: EventID=4624 source=185.22.155.10 status=Success user=admin`);
                return l.reverse();
            }
        },
        traversal: {
            name: "ADV-02: Directory Traversal",
            details: "<b>[EN]</b> Find the sensitive file targeted by the path traversal attempt.<br><b>[TR]</b> Path traversal girişimiyle hedeflenen hassas dosyayı bulun.",
            questions: [
                {q: "Attacker IP Address?", a: "192.168.55.12"},
                {q: "Targeted File?", a: "/etc/passwd"},
                {q: "HTTP Status Code for success?", a: "200"}
            ],
            logs: () => {
                let l = []; for(let i=0; i<50; i++) l.push(`[10:10:01] INFO Apache: 192.168.1.${i} GET /index.php 200`);
                l.push(`[10:15:22] WARN Apache: 192.168.55.12 GET /download.php?file=../../../../etc/passwd 200`);
                return l;
            }
        },
        ransom: {
            name: "ADV-03: Ransomware Staging",
            details: "<b>[EN]</b> Detect ransomware encryption process and file extension.<br><b>[TR]</b> Fidye yazılımı şifreleme işlemini ve dosya uzantısını bulun.",
            questions: [
                {q: "Malicious Process?", a: "wannacry.exe"},
                {q: "New File Extension?", a: ".wnry"},
                {q: "CnC Server IP?", a: "203.0.113.42"}
            ],
            logs: () => {
                let l = [];
                l.push(`[08:00:01] CRIT Sysmon: EventID=1 Process="wannacry.exe" Path="C:\\Windows\\Temp"`);
                l.push(`[08:00:15] ALERT FileSystem: Target="Report.pdf.wnry" Status="Encrypted"`);
                l.push(`[08:01:40] WARN Network: Outbound connection to 203.0.113.42:4444 established`);
                return l;
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
        document.getElementById('active-user').innerText = `${userAlias.toUpperCase()}@STATION`;
        closeWindow('auth-window');
        openModal("AUTHORIZATION SUCCESS", `Welcome back, Analyst ${userAlias}. Cyber Range is online.`);
        renderSelection();
    }

    function renderSelection() {
        const container = document.getElementById('machine-list');
        container.innerHTML = Object.keys(labs).map(key => `
            <div onclick="showBriefing('${key}')" style="background:#161b22; border:1px solid #30363d; padding:25px; border-radius:12px; cursor:pointer;">
                <div style="font-weight:bold; color:#fff; font-size:14px;">${labs[key].name}</div>
            </div>
        `).join('');
    }

    function showBriefing(key) {
        currentKey = key;
        document.getElementById('selection-content').classList.add('hidden');
        document.getElementById('briefing-content').classList.remove('hidden');
        document.getElementById('brief-lab-name').innerText = labs[key].name;
        document.getElementById('brief-details').innerHTML = labs[key].details;
    }

    function hideBriefing() {
        document.getElementById('selection-content').classList.remove('hidden');
        document.getElementById('briefing-content').classList.add('hidden');
    }

    function startInvestigation() {
        closeWindow('selection-window');
        openWindow('lab-window');
        const lab = labs[currentKey];
        document.getElementById('lab-title-text').innerText = `INVESTIGATING: ${lab.name}`;
        document.getElementById('log-screen').innerHTML = lab.logs().map(l => `<div style="border-bottom:1px solid #161b22; padding:3px 0;">${l}</div>`).join('');
        document.getElementById('q-area').innerHTML = lab.questions.map((q, i) => `
            <div style="margin-bottom:15px;">
                <label style="font-size:11px; color:#8b949e;">${q.q}</label>
                <input type="text" id="ans-${i}" class="input-field">
            </div>
        `).join('');
    }

    function checkAnswers() {
        const lab = labs[currentKey]; let correct = 0;
        lab.questions.forEach((q, i) => {
            const el = document.getElementById(`ans-${i}`);
            if(el.value.trim().toLowerCase() === q.a.toLowerCase()) { correct++; el.classList.add('correct-ans'); el.classList.remove('wrong-ans'); }
            else { el.classList.add('wrong-ans'); el.classList.remove('correct-ans'); }
        });

        if(correct === lab.questions.length) {
            openModal("VERIFIED", "Evidence correct. Proceeding to Incident Response...");
            setTimeout(() => { closeWindow('lab-window'); openWindow('playbook-window'); renderPlaybook(); }, 2000);
        } else { openModal("MISMATCH", "Evidence artifacts do not match logs."); }
    }

    function renderPlaybook() {
        const ui = document.getElementById('playbook-ui');
        ui.innerHTML = `
            <div class="playbook-step" style="background:rgba(88,166,255,0.05); padding:20px; border-radius:8px;">
                <p>1. EDR Remote Terminal: Containment</p>
                <div class="edr-terminal">
                    <div id="edr-out">[SYS] Connection Ready. Type 'kill --pid 99' or 'reset --user admin'</div>
                    <div style="display:flex;"><span>$</span><input type="text" id="edr-in" style="background:none; border:none; color:#3fb950; outline:none; width:100%;" onkeydown="handleEDR(event)"></div>
                </div>
                <div style="margin-top:20px;">
                    <p>2. Incident Classification</p>
                    <select id="esc-type" class="input-field"><option value="tp">True Positive</option><option value="fp">False Positive</option></select>
                    <textarea id="esc-note" class="input-field" style="height:80px; margin-top:15px;" placeholder="Final report..."></textarea>
                    <button onclick="submitEscalation()" class="btn-action" style="margin-top:15px;">SUBMIT & CLOSE CASE</button>
                </div>
            </div>
        `;
    }

    function handleEDR(e) {
        if(e.key === "Enter") {
            const cmd = e.target.value;
            const out = document.getElementById('edr-out');
            if(cmd.startsWith("kill") || cmd.startsWith("reset")) {
                out.innerHTML += `<div style="color:#58a6ff;">[SUCCESS] Command executed.</div>`;
            } else { out.innerHTML += `<div style="color:#f85149;">[ERROR] Access denied.</div>`; }
            e.target.value = "";
        }
    }

    function submitEscalation() {
        openModal("STATION REPORT", "Case closed. Report sent to L2 Queue. Station standby.");
        setTimeout(() => { location.reload(); }, 4000);
    }

    setInterval(() => { document.getElementById('clock').innerText = new Date().toLocaleTimeString(); }, 1000);
</script>
