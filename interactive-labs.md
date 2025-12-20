---
layout: page
title: "SentinelGuard Expert Cyber Range"
permalink: /labs/station/
---

<style>
    /* CSS AYARLARI - BEYAZ ALANLARI TAMAMEN YOK EDER */
    html, body, main { background-color: #010409 !important; color: #c9d1d9 !important; margin: 0 !important; padding: 0 !important; width: 100%; height: 100%; }
    #desktop-environment {
        background: radial-gradient(circle, #0d1117 0%, #010409 100%);
        height: 90vh; position: relative; overflow: hidden; border: 1px solid #30363d; margin: 10px; border-radius: 12px; display: flex; flex-direction: column; font-family: 'JetBrains Mono', monospace;
    }

    /* PENCERELER VE MODALLAR */
    .window {
        position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
        width: 96%; height: 92%; background: #0d1117; border: 1px solid #30363d; border-radius: 10px; box-shadow: 0 30px 60px rgba(0,0,0,0.9); display: flex; flex-direction: column; z-index: 100;
    }
    .modal-overlay { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); z-index: 500; display: flex; justify-content: center; align-items: center; }
    .custom-modal { background: #161b22; border: 1px solid #58a6ff; padding: 30px; border-radius: 12px; text-align: center; max-width: 500px; border-top: 5px solid #58a6ff; }

    /* LAB EKRANI */
    #log-screen { flex: 1; overflow-y: scroll; padding: 15px; font-size: 11px; color: #8b949e; line-height: 1.8; background: #010409; white-space: pre-wrap; }
    .edr-terminal { background: #000; color: #3fb950; padding: 15px; font-family: 'Courier New', monospace; height: 180px; overflow-y: auto; border: 1px solid #30363d; margin-top: 15px; border-radius: 6px; }

    /* BUTONLAR VE INPUTLAR */
    .btn-action { background: #238636; border: none; color: white; padding: 12px 24px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; }
    .input-field { width: 100%; background: #0d1117; border: 1px solid #30363d; color: #58a6ff; padding: 10px; margin-top: 5px; border-radius: 5px; outline: none; font-size: 12px; }
    .correct-ans { border-color: #238636 !important; background: rgba(35, 134, 54, 0.1) !important; }
    .wrong-ans { border-color: #f85149 !important; background: rgba(248, 81, 73, 0.1) !important; }
    .hidden { display: none !important; }
</style>

<div id="desktop-environment">
    <div id="system-modal" class="modal-overlay hidden">
        <div class="custom-modal">
            <h2 id="modal-title" style="color: #58a6ff;">STATION MESSAGE</h2>
            <div id="modal-body" style="margin: 20px 0; font-size: 14px; color: #e6edf3;"></div>
            <button onclick="closeModal()" class="btn-action">ACKNOWLEDGE</button>
        </div>
    </div>

    <div class="desktop-icons" style="padding: 25px;">
        <div class="icon" onclick="openWindow('auth-window')" style="width:100px; text-align:center; cursor:pointer;">
            <img src="https://cdn-icons-png.flaticon.com/512/3233/3233514.png" width="50">
            <span style="display:block; font-size:11px; margin-top:5px;">Login.sys</span>
        </div>
        <div id="range-icon" class="icon hidden" onclick="openWindow('selection-window')" style="width:100px; text-align:center; cursor:pointer; margin-top:30px;">
            <img src="https://cdn-icons-png.flaticon.com/512/906/906343.png" width="50">
            <span style="display:block; font-size:11px; margin-top:5px;">CyberRange.exe</span>
        </div>
    </div>

    <div id="auth-window" class="window">
        <div style="padding: 80px; text-align: center; max-width: 400px; margin: 0 auto;">
            <h3 style="color: #58a6ff;">ANALYST AUTHENTICATION</h3>
            <input type="text" id="alias-input" class="input-field" placeholder="Enter Alias...">
            <br><br>
            <button onclick="loginAnalyst()" class="btn-action">INITIALIZE</button>
        </div>
    </div>

    <div id="selection-window" class="window hidden">
        <div style="padding: 30px; overflow-y: auto;">
            <h4 style="color: #58a6ff; border-bottom: 1px solid #30363d; padding-bottom: 10px;">MISSION CONTROL</h4>
            <div id="machine-list" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; margin-top: 20px;"></div>
        </div>
        <div id="briefing-content" class="hidden" style="padding: 30px; overflow-y: auto;">
            <h2 id="brief-lab-name" style="color: #58a6ff;">BRIEFING</h2>
            <div id="brief-details" style="background: rgba(88,166,255,0.05); border: 1px solid #30363d; padding: 20px; border-radius: 8px; margin: 20px 0;"></div>
            <div style="display: flex; gap: 15px;">
                <button onclick="hideBriefing()" class="btn-action" style="background:#30363d;">BACK</button>
                <button onclick="startInvestigation()" class="btn-action">START INVESTIGATION</button>
            </div>
        </div>
    </div>

    <div id="lab-window" class="window hidden">
        <div class="lab-content">
            <div style="flex: 2.8; display: flex; flex-direction: column;">
                <div style="background:#161b22; padding:8px 15px; font-size:10px; border-bottom: 1px solid #30363d;">RAW SIEM TELEMETRY (MONOCHROME_MODE)</div>
                <div id="log-screen"></div>
            </div>
            <div style="flex: 1.2; background: #161b22; padding: 20px; border-radius: 6px; border: 1px solid #30363d; overflow-y: auto;">
                <h4 style="color:#f85149; margin-top:0;">Investigation Questions</h4>
                <div id="q-area"></div>
                <button onclick="checkAnswers()" class="btn-action" style="margin-top: 20px;">VALIDATE EVIDENCE</button>
            </div>
        </div>
    </div>

    <div id="playbook-window" class="window hidden">
        <div style="padding: 40px; overflow-y: auto;">
            <h2 style="color: #3fb950;">INCIDENT RESPONSE CENTER</h2>
            <div id="playbook-ui"></div>
        </div>
    </div>
</div>

<script>
    const labs = {
        brute: {
            name: "ADV-01: Stealthy Brute Force",
            details: "<b>[EN]</b> Low-and-slow brute force detection. Identify the specific tool and compromised time.<br><b>[TR]</b> Yavaş ve sinsi brute force tespiti. Kullanılan aracı ve sızma vaktini bulun.",
            questions: [
                {q: "Attacker IP Address?", a: "141.10.22.5"},
                {q: "Which account was compromised?", a: "svc_web"},
                {q: "MITRE Technique ID? (e.g. T1110)", a: "T1110"},
                {q: "Exact timestamp of SUCCESS logon?", a: "15:42:01"},
                {q: "What is the suspicious EventID?", a: "4624"}
            ],
            logs: () => {
                let l = []; for(let i=0; i<80; i++) l.push(`[${15}:${i<10?'0'+i:i}:12] INFO Audit: EventID=4624 source=10.10.1.${i} user=jdoe status=Success`);
                for(let i=10; i<40; i+=5) l.push(`[15:${i}:05] WARN Audit: EventID=4625 source=141.10.22.5 user=svc_web status=Failure msg="Bad Password"`);
                l.push(`[15:42:01] CRIT Audit: EventID=4624 source=141.10.22.5 user=svc_web status=Success msg="Logon type 3"`);
                return l.reverse();
            }
        },
        traversal: {
            name: "ADV-02: Directory Traversal",
            details: "<b>[EN]</b> Identify path traversal attempt and the file the attacker tried to read.<br><b>[TR]</b> Path traversal girişimini ve saldırganın okumaya çalıştığı dosyayı bulun.",
            questions: [
                {q: "Attacker IP Address?", a: "192.168.55.12"},
                {q: "Which file was targeted? (e.g. /etc/passwd)", a: "/etc/passwd"},
                {q: "What is the HTTP Status Code for the attempt?", a: "200"},
                {q: "What parameter was vulnerable?", a: "file"},
                {q: "What encoding was used? (URL/Hex/None)", a: "URL"}
            ],
            logs: () => {
                let l = []; for(let i=0; i<60; i++) l.push(`[10:10:${i}] INFO Apache: 192.168.1.${i} GET /assets/img.png 200`);
                l.push(`[10:15:22] WARN Apache: 192.168.55.12 GET /download.php?file=../../../../etc/passwd HTTP/1.1 200`);
                l.push(`[10:15:25] WARN Apache: 192.168.55.12 GET /download.php?file=%2e%2e%2f%2e%2e%2fetc%2fshadow 403`);
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
        userAlias = document.getElementById('alias-input').value.trim(); if(!userAlias) return;
        document.getElementById('range-icon').classList.remove('hidden');
        closeWindow('auth-window');
        openModal("TERMINAL INITIALIZED", `Analyst ${userAlias}, station is online.`);
        renderSelection();
    }

    function renderSelection() {
        const container = document.getElementById('machine-list');
        container.innerHTML = Object.keys(labs).map(key => `
            <div onclick="showBriefing('${key}')" style="background:#161b22; border:1px solid #30363d; padding:25px; border-radius:12px; cursor:pointer;">
                <div style="font-weight:bold; color:#fff;">${labs[key].name}</div>
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
        document.getElementById('log-screen').innerHTML = lab.logs().map(l => `<div style="border-bottom:1px solid #161b22; padding:3px 0;">${l}</div>`).join('');
        document.getElementById('q-area').innerHTML = lab.questions.map((q, i) => `
            <div style="margin-bottom:15px;">
                <label style="font-size:11px; color:#8b949e; font-weight:bold;">${q.q}</label>
                <input type="text" id="ans-${i}" class="input-field" placeholder="Analyze logs...">
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
            openModal("VERIFIED", "Evidence correct. Proceeding to Remediation...");
            setTimeout(() => { closeWindow('lab-window'); openWindow('playbook-window'); renderPlaybook(); }, 2000);
        } else { openModal("MISMATCH", `${lab.questions.length - correct} artifacts do not match telemetry.`); }
    }

    function renderPlaybook() {
        const area = document.getElementById('playbook-ui');
        area.innerHTML = `
            <div style="background:rgba(88,166,255,0.05); padding:20px; border-radius:8px;">
                <p>1. EDR Remote Terminal: Execute Containment</p>
                <div class="edr-terminal">
                    <div id="edr-out">[SYS] Connection Ready. Use 'kill --pid [id]' or 'reset --user [name]'</div>
                    <div style="display:flex;"><span>$</span><input type="text" id="edr-in" style="background:none; border:none; color:#3fb950; outline:none; width:100%;" onkeydown="handleEDR(event)"></div>
                </div>
                <div style="margin-top:20px;">
                    <p>2. Final Incident Classification</p>
                    <select id="esc-type" class="input-field"><option value="tp">True Positive (Confirmed Attack)</option><option value="fp">False Positive (Benign Activity)</option></select>
                    <textarea id="esc-note" class="input-field" style="height:80px; margin-top:15px;" placeholder="Final Analyst Note..."></textarea>
                    <button onclick="submitEscalation()" class="btn-action" style="margin-top:15px;">CLOSE CASE</button>
                </div>
            </div>
        `;
    }

    function handleEDR(e) {
        if(e.key === "Enter") {
            const cmd = e.target.value;
            const out = document.getElementById('edr-out');
            if(cmd.startsWith("reset") || cmd.startsWith("kill")) {
                out.innerHTML += `<div style="color:#58a6ff;">[SUCCESS] Action executed on host. Telemetry updated.</div>`;
            } else { out.innerHTML += `<div style="color:#f85149;">[ERROR] Command rejected. Access denied.</div>`; }
            e.target.value = "";
        }
    }

    function submitEscalation() {
        openModal("STATION REPORT", "Case closed. Report sent to L2. Station Standby.");
        setTimeout(() => { location.reload(); }, 4000);
    }
</script>
