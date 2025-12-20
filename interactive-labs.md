---
layout: page
title: "SentinelGuard Advanced SOC Station"
permalink: /labs/station/
---

<style>
    /* GLOBAL RESET */
    html, body, main { background-color: #010409 !important; color: #c9d1d9 !important; margin: 0 !important; padding: 0 !important; width: 100%; height: 100%; overflow: hidden; }
    #desktop-environment {
        background: radial-gradient(circle, #0d1117 0%, #010409 100%);
        height: 100vh; width: 100vw; position: relative; overflow: hidden; font-family: 'JetBrains Mono', monospace; z-index: 1;
    }

    /* SEARCH BAR STYLE */
    .filter-container {
        background: #161b22; padding: 8px 15px; display: flex; align-items: center; border-bottom: 1px solid #30363d; gap: 10px;
    }
    .search-input {
        background: #0d1117; border: 1px solid #30363d; color: #3fb950; padding: 5px 10px; border-radius: 4px; font-size: 11px; width: 250px; outline: none;
    }
    .search-input:focus { border-color: #58a6ff; }

    /* TASKBAR */
    #taskbar { background: rgba(22, 27, 34, 0.98); height: 40px; border-top: 1px solid #30363d; display: flex; align-items: center; padding: 0 15px; gap: 20px; z-index: 999; position: absolute; bottom: 0; left: 0; width: 100%; }

    /* WINDOW SYSTEM */
    .window {
        position: absolute; top: 48%; left: 50%; transform: translate(-50%, -50%);
        width: 96%; height: 85%; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; box-shadow: 0 30px 60px rgba(0,0,0,0.9); display: flex; flex-direction: column; z-index: 100;
    }
    .window-header { background: #161b22; padding: 10px 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #30363d; }
    
    .lab-content { display: flex; flex: 1; overflow: hidden; padding: 15px; gap: 15px; }
    #log-screen { flex: 1; overflow-y: scroll; padding: 15px; font-size: 11px; color: #8b949e; line-height: 1.8; background: #010409; border: 1px solid #30363d; white-space: pre-wrap; }
    .log-line { border-bottom: 1px solid #161b22; padding: 2px 0; }
    
    .btn-action { background: #238636; border: none; color: white; padding: 12px 20px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; }
    .input-field { width: 100%; background: #0d1117; border: 1px solid #30363d; color: #58a6ff; padding: 10px; margin-top: 5px; border-radius: 5px; outline: none; }
    .hidden { display: none !important; }
    .correct-ans { border-color: #238636 !important; background: rgba(35, 134, 54, 0.1) !important; }
    .wrong-ans { border-color: #f85149 !important; background: rgba(248, 81, 73, 0.1) !important; }
    
    .modal-overlay { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); z-index: 1000; display: flex; justify-content: center; align-items: center; }
    .edr-terminal { background: #000; color: #3fb950; padding: 10px; font-family: 'Courier New', monospace; height: 110px; overflow-y: auto; border: 1px solid #30363d; margin-top: 10px; border-radius: 4px; font-size: 11px; }
</style>

<div id="desktop-environment">
    <div id="system-modal" class="modal-overlay hidden">
        <div style="background: #161b22; border: 1px solid #58a6ff; padding: 30px; border-radius: 12px; text-align: center; max-width: 500px; border-top: 5px solid #58a6ff;">
            <h2 id="modal-title" style="color: #58a6ff; margin-top:0;">STATION ALERT</h2>
            <div id="modal-body" style="margin: 20px 0; font-size: 13px; color: #e6edf3;"></div>
            <button onclick="closeModal()" class="btn-action">OK</button>
        </div>
    </div>

    <div style="position: absolute; top: 20px; left: 20px; display: flex; flex-direction: column; gap: 25px;">
        <div onclick="openWindow('auth-window')" style="text-align:center; cursor:pointer;">
            <img src="https://cdn-icons-png.flaticon.com/512/3233/3233514.png" width="45">
            <span style="display:block; font-size:10px; color:#fff; margin-top:5px;">Login.sys</span>
        </div>
        <div id="range-icon" class="hidden" onclick="openWindow('selection-window')" style="text-align:center; cursor:pointer;">
            <img src="https://cdn-icons-png.flaticon.com/512/906/906343.png" width="45">
            <span style="display:block; font-size:10px; color:#fff; margin-top:5px;">CyberRange.exe</span>
        </div>
    </div>

    <div id="auth-window" class="window">
        <div style="padding: 60px; text-align: center; max-width: 400px; margin: 0 auto;">
            <h3 style="color: #58a6ff;">ANALYST LOGIN</h3>
            <input type="text" id="alias-input" class="input-field" placeholder="Codename...">
            <br><br>
            <button onclick="loginAnalyst()" class="btn-action">INITIALIZE</button>
        </div>
    </div>

    <div id="selection-window" class="window hidden">
        <div style="padding: 25px; overflow-y: auto;">
            <h4 style="color: #58a6ff; border-bottom: 1px solid #30363d; padding-bottom: 10px;">MISSION CONTROL</h4>
            <div id="machine-list" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 15px; margin-top: 20px;"></div>
        </div>
        <div id="briefing-content" class="hidden" style="padding: 25px; overflow-y: auto;">
            <h2 id="brief-lab-name" style="color: #58a6ff;">BRIEFING</h2>
            <div id="brief-details" style="background: rgba(88,166,255,0.05); border: 1px solid #30363d; padding: 20px; border-radius: 8px; margin: 20px 0; font-size: 13px;"></div>
            <div style="display: flex; gap: 15px;">
                <button onclick="hideBriefing()" class="btn-action" style="background:#30363d; width: 150px;">BACK</button>
                <button onclick="startInvestigation()" class="btn-action">START INVESTIGATION</button>
            </div>
        </div>
    </div>

    <div id="lab-window" class="window hidden">
        <div class="window-header"><span id="lab-title-text">Investigation Unit</span><span onclick="closeWindow('lab-window')" style="cursor:pointer;">×</span></div>
        
        <div class="filter-container">
            <span style="font-size: 10px; color: #8b949e;">FILTER:</span>
            <input type="text" id="log-search-input" class="search-input" placeholder="Search keyword (e.g. admin, UNION, 4625)..." onkeyup="filterLogs()">
            <span id="filter-status" style="font-size: 9px; color: #58a6ff; margin-left: auto;">SHOWING ALL LOGS</span>
        </div>

        <div class="lab-content">
            <div style="flex: 1.5; display: flex; flex-direction: column;">
                <div id="log-screen"></div>
            </div>
            <div style="flex: 0.6; background: #161b22; padding: 15px; border-radius: 6px; border: 1px solid #30363d; overflow-y: auto;">
                <h4 style="color:#f85149; margin-top:0;">Checklist</h4>
                <div id="q-area"></div>
                <button onclick="checkAnswers()" class="btn-action" style="margin-top: 15px;">VALIDATE</button>
            </div>
        </div>
    </div>

    <div id="playbook-window" class="window hidden">
        <div style="padding: 30px; overflow-y: auto;">
            <h2 style="color: #3fb950; margin-top: 0;">REMEDIATION & ESCALATION</h2>
            <div id="playbook-ui"></div>
        </div>
    </div>

    <div id="taskbar">
        <div id="clock" style="font-size: 11px; color: #8b949e;">00:00:00</div>
        <div id="active-user" style="margin-left: auto; font-size: 10px; color: #58a6ff;">GUEST@STATION</div>
    </div>
</div>

<script>
    const labs = {
        brute: {
            name: "ADV-01: Windows Brute Force",
            details: "Detect unauthorized access attempts. Search for EventID 4625/4624.",
            questions: [{q: "Attacker IP?", a: "141.10.22.5"}, {q: "Compromised User?", a: "admin"}],
            logs: () => {
                let l = [];
                for(let i=0; i<150; i++) l.push(`[14:20:${i}] INFO Auth: EventID=4624 source=10.10.1.${i} user=jdoe status=Success`);
                l.push(`[14:40:01] WARN Auth: EventID=4625 source=141.10.22.5 user=admin status=Failure`);
                l.push(`[14:40:05] WARN Auth: EventID=4625 source=141.10.22.5 user=admin status=Failure`);
                l.push(`[14:45:01] CRIT Auth: EventID=4624 source=141.10.22.5 user=admin status=Success`);
                return l;
            }
        },
        traversal: {
            name: "ADV-02: Directory Traversal",
            details: "Identify attempts to bypass directory restrictions via web.",
            questions: [{q: "Attacker IP?", a: "192.168.55.12"}, {q: "Sensitive File?", a: "/etc/passwd"}],
            logs: () => {
                let l = [];
                for(let i=0; i<100; i++) l.push(`[10:01:${i}] INFO Web: 192.168.1.${i} GET /assets/img.png 200`);
                l.push(`[10:15:22] WARN Web: 192.168.55.12 GET /download.php?file=../../../../etc/passwd 200`);
                return l;
            }
        }
    };

    let userAlias = ""; let currentKey = ""; let activeLogs = [];

    function openWindow(id) { document.querySelectorAll('.window').forEach(w => w.classList.add('hidden')); document.getElementById(id).classList.remove('hidden'); }
    function closeWindow(id) { document.getElementById(id).classList.add('hidden'); }
    function openModal(title, body) { document.getElementById('modal-title').innerText = title; document.getElementById('modal-body').innerHTML = body; document.getElementById('system-modal').classList.remove('hidden'); }
    function closeModal() { document.getElementById('system-modal').classList.add('hidden'); }

    function loginAnalyst() {
        userAlias = document.getElementById('alias-input').value.trim(); if(!userAlias) return;
        document.getElementById('range-icon').classList.remove('hidden');
        document.getElementById('active-user').innerText = `${userAlias.toUpperCase()}@STATION`;
        closeWindow('auth-window'); openWindow('selection-window'); renderSelection();
    }

    function renderSelection() {
        const container = document.getElementById('machine-list');
        container.innerHTML = Object.keys(labs).map(key => `<div onclick="showBriefing('${key}')" style="background:#161b22; border:1px solid #30363d; padding:20px; border-radius:10px; cursor:pointer; font-weight:bold;">${labs[key].name}</div>`).join('');
    }

    function showBriefing(key) {
        currentKey = key; document.getElementById('selection-content').classList.add('hidden');
        document.getElementById('briefing-content').classList.remove('hidden');
        document.getElementById('brief-details').innerHTML = labs[key].details;
    }

    function hideBriefing() { document.getElementById('selection-content').classList.remove('hidden'); document.getElementById('briefing-content').classList.add('hidden'); }

    function startInvestigation() {
        closeWindow('selection-window'); openWindow('lab-window');
        const lab = labs[currentKey];
        activeLogs = lab.logs();
        displayLogs(activeLogs); // Initial display
        document.getElementById('q-area').innerHTML = lab.questions.map((q, i) => `<div style="margin-bottom:10px;"><label style="font-size:10px; color:#8b949e;">${q.q}</label><input type="text" id="ans-${i}" class="input-field"></div>`).join('');
    }

    // NEW LOG DISPLAY FUNCTION
    function displayLogs(logsToDisplay) {
        const screen = document.getElementById('log-screen');
        screen.innerHTML = logsToDisplay.map(l => `<div class="log-line">${l}</div>`).join('');
        document.getElementById('filter-status').innerText = `SHOWING ${logsToDisplay.length} LOGS`;
    }

    // NEW FILTER LOGIC
    function filterLogs() {
        const searchTerm = document.getElementById('log-search-input').value.toLowerCase();
        const filtered = activeLogs.filter(log => log.toLowerCase().includes(searchTerm));
        displayLogs(filtered);
    }

    function checkAnswers() {
        const lab = labs[currentKey]; let correct = 0;
        lab.questions.forEach((q, i) => {
            const el = document.getElementById(`ans-${i}`);
            if(el.value.trim().toLowerCase() === q.a.toLowerCase()) { correct++; el.classList.add('correct-ans'); } else { el.classList.add('wrong-ans'); }
        });
        if(correct === lab.questions.length) { openModal("VERIFIED", "Proceeding to Playbook..."); setTimeout(() => { closeWindow('lab-window'); openWindow('playbook-window'); renderPlaybook(); }, 1500); }
    }

    function renderPlaybook() {
        document.getElementById('playbook-ui').innerHTML = `
            <div style="background:rgba(88,166,255,0.05); padding:20px; border-radius:8px;">
                <select id="case-decision" class="input-field" onchange="updatePlaybookView()">
                    <option value="tp">True Positive (Confirmed Attack)</option>
                    <option value="fp">False Positive (Benign Activity)</option>
                </select>
                <div id="tp-area" style="margin-top:20px;">
                    <div class="edr-terminal"><div id="edr-out">[CONNECTED]</div><input type="text" id="edr-in" onkeydown="handleEDR(event)" style="background:none; border:none; color:#3fb950; outline:none; width:100%;"></div>
                    <button onclick="finalize('tp')" class="btn-action" style="margin-top:15px; background:#d29922;">ESCALATE TO L2</button>
                </div>
                <div id="fp-area" class="hidden" style="margin-top:20px;">
                    <button onclick="finalize('fp')" class="btn-action" style="margin-top:15px;">CLOSE CASE (FP)</button>
                </div>
                <textarea id="analyst-note" class="input-field" style="height:60px; margin-top:15px;" placeholder="Analyst notes..."></textarea>
            </div>`;
    }

    function updatePlaybookView() {
        const dec = document.getElementById('case-decision').value;
        if(dec === 'fp') { document.getElementById('tp-area').classList.add('hidden'); document.getElementById('fp-area').classList.remove('hidden'); }
        else { document.getElementById('tp-area').classList.remove('hidden'); document.getElementById('fp-area').classList.add('hidden'); }
    }

    function handleEDR(e) { if(e.key === "Enter") { document.getElementById('edr-out').innerHTML += `<div>$ ${e.target.value}</div><div style="color:#3fb950;">[OK] Executed.</div>`; e.target.value = ""; } }
    function finalize(type) { if(!document.getElementById('analyst-note').value) { alert("Note required."); return; } openModal("DONE", type==='tp'?"Escalated.":"Closed."); setTimeout(()=>location.reload(), 3000); }
    setInterval(() => { document.getElementById('clock').innerText = new Date().toLocaleTimeString(); }, 1000);
</script>
