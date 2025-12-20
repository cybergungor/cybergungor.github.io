---
layout: default
title: SentinelGuard - Incident Management Dashboard
permalink: /alerts/
---

<style>
    /* Full Page UI & Cyber Theme */
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    .soc-wrapper { display: flex; flex-direction: column; height: calc(100vh - 60px); color: #e6edf3; font-family: 'Inter', sans-serif; }

    /* --- MODERNIZED DISCLAIMER MODAL --- */
    .disclaimer-modal { 
        position: fixed; top: 0; left: 0; width: 100%; height: 100%; 
        background: rgba(1, 4, 9, 0.94); z-index: 10000; 
        display: none; align-items: center; justify-content: center; backdrop-filter: blur(20px); 
    }
    .disclaimer-box { 
        background: #0d1117; border: 1px solid #30363d; width: 800px; 
        border-radius: 16px; overflow: hidden; box-shadow: 0 0 100px rgba(88, 166, 255, 0.1); 
        animation: fadeIn 0.5s ease-out;
    }
    .disclaimer-header { 
        background: linear-gradient(90deg, #161b22, #0d1117); color: #58a6ff; 
        padding: 25px; font-family: 'JetBrains Mono'; font-weight: bold; text-align: center; 
        border-bottom: 1px solid #30363d; font-size: 1.1rem; letter-spacing: 2px;
    }
    .disclaimer-body { padding: 40px; display: flex; gap: 40px; line-height: 1.6; }
    .lang-section { flex: 1; }
    .lang-section h3 { font-size: 0.85rem; color: #58a6ff; margin-bottom: 15px; font-family: 'JetBrains Mono'; border-left: 3px solid #58a6ff; padding-left: 10px; }
    .lang-section p { font-size: 0.9rem; color: #8b949e; margin-bottom: 10px; }
    .v-divider { width: 1px; background: #30363d; }
    .btn-ack { 
        width: 100%; padding: 20px; background: #238636; color: #fff; border: none; 
        font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; transition: 0.3s;
        letter-spacing: 1px;
    }
    .btn-ack:hover { background: #2ea043; box-shadow: 0 0 20px rgba(46, 160, 67, 0.4); }

    /* --- FILTER BAR --- */
    .soc-top-bar { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .btn-filter { background: #161b22; border: 1px solid #30363d; color: #8b949e; padding: 8px 18px; border-radius: 6px; font-size: 0.75rem; cursor: pointer; font-family: 'JetBrains Mono'; transition: 0.2s; margin-right: 8px; }
    .btn-filter.active { background: #58a6ff; color: #0d1117; border-color: #58a6ff; font-weight: bold; }

    /* --- TABLE --- */
    .queue-container { flex: 1; padding: 25px; overflow-y: auto; }
    .incident-table { width: 100%; border-collapse: collapse; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; }
    .incident-table th { background: #161b22; text-align: left; padding: 15px; font-size: 0.7rem; color: #8b949e; text-transform: uppercase; letter-spacing: 1px; }
    .incident-table td { padding: 18px 15px; border-bottom: 1px solid #21262d; font-size: 0.85rem; }
    .alert-row:hover { background: #121d2f; cursor: pointer; }

    /* --- SIDE PANEL --- */
    .side-panel { position: fixed; top: 0; right: -650px; width: 600px; height: 100vh; background: #0d1117; border-left: 1px solid #30363d; transition: 0.4s cubic-bezier(0.05, 0.7, 0.1, 1); z-index: 9000; display: flex; flex-direction: column; }
    .panel-active { right: 0; box-shadow: -20px 0 80px rgba(0,0,0,0.8); }
    .panel-header { padding: 25px; background: #161b22; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .panel-body { padding: 30px; flex: 1; overflow-y: auto; }
    
    .card { background: #161b22; border: 1px solid #30363d; border-radius: 12px; padding: 20px; margin-bottom: 25px; }
    .card h4 { margin: 0 0 15px 0; font-size: 0.75rem; color: #58a6ff; text-transform: uppercase; border-bottom: 1px solid #30363d; padding-bottom: 8px; }
    
    .artifact-input { width: 100%; background: #010409; border: 1px solid #30363d; color: #e6edf3; padding: 12px; font-family: 'JetBrains Mono'; font-size: 0.85rem; border-radius: 6px; margin-bottom: 15px; }
    .threat-info { font-size: 0.85rem; color: #8b949e; line-height: 1.6; background: rgba(88, 166, 255, 0.03); padding: 15px; border-left: 4px solid #58a6ff; margin-bottom: 15px; }

    /* Verdict Badges */
    .badge { padding: 4px 10px; border-radius: 4px; font-size: 0.7rem; font-weight: bold; text-transform: uppercase; }
    .badge-tp { background: rgba(248, 81, 73, 0.1); color: #f85149; border: 1px solid #f85149; }
    .badge-fp { background: rgba(88, 166, 255, 0.1); color: #58a6ff; border: 1px solid #58a6ff; }
    .badge-pending { background: #30363d; color: #8b949e; }

    .panel-footer { padding: 25px; background: #0d1117; border-top: 1px solid #30363d; display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
    .btn-footer { padding: 14px; border-radius: 6px; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; font-size: 0.8rem; border: none; transition: 0.2s; }
    .btn-res { background: #238636; color: white; }
    .btn-esc { background: #d29922; color: #0d1117; }
    .btn-res:hover { filter: brightness(1.2); }

    @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
</style>



<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">SENTINEL GUARD // COMMAND & CONTROL</div>
        <div class="disclaimer-body">
            <div class="lang-section">
                <h3>[TR] SİSTEM ERİŞİMİ</h3>
                <p>Bu panel, siber olay müdahale döngüsünü yönetmek için tasarlanmıştır. Alarmları triyajlayın, kanıtları doğrulayın ve profesyonel raporlar oluşturun.</p>
                <p style="font-size: 0.8rem; color: #d29922;">*Analiz Notu: Tüm veriler gerçeğe uygun SIEM loglarıyla entegredir.</p>
            </div>
            <div class="v-divider"></div>
            <div class="lang-section">
                <h3>[EN] SYSTEM ACCESS</h3>
                <p>Designed for end-to-end incident lifecycle management. Triage active alerts, verify artifacts, and generate deep-forensic IR reports.</p>
                <p style="font-size: 0.8rem; color: #d29922;">*Analyst Note: Live data is cross-referenced with simulated SIEM telemetry.</p>
            </div>
        </div>
        <button class="btn-ack" onclick="closeModal('welcomeModal')">AUTHORIZE SESSION</button>
    </div>
</div>

<div class="soc-wrapper">
    <div class="soc-top-bar">
        <div class="filter-group">
            <button class="btn-filter active" id="tab-open" onclick="switchTab('open')">ACTIVE QUEUE (<span id="count-open">3</span>)</button>
            <button class="btn-filter" id="tab-closed" onclick="switchTab('closed')">CLOSED ARCHIVE (<span id="count-closed">5</span>)</button>
        </div>
        <div style="font-family:'JetBrains Mono'; font-size:0.75rem; color:#8b949e;">ANALYST STATUS: <span style="color:#3fb950">● AUTHORIZED</span></div>
    </div>

    <div class="queue-container">
        <table class="incident-table">
            <thead>
                <tr>
                    <th width="120">Case ID</th>
                    <th>Tactical Description</th>
                    <th width="120">Severity</th>
                    <th width="140">Verdict</th>
                    <th width="120">Action</th>
                </tr>
            </thead>
            <tbody id="queueBody"></tbody>
        </table>
    </div>
</div>

<div id="sidePanel" class="side-panel">
    <div class="panel-header">
        <div style="font-family:'JetBrains Mono'; font-size:0.85rem; font-weight:bold;">
            <span id="headerMode">ANALYSIS</span>: <span id="panelId" style="color:#58a6ff;">#SOC-0000</span>
        </div>
        <span style="cursor:pointer; font-size:1.8rem; color:#8b949e;" onclick="closePanel()">×</span>
    </div>
    <div class="panel-body" id="panelBody"></div>
    <div class="panel-footer" id="panelFooter"></div>
</div>

<script>
    const cases = {
        1: { id: "#SOC-5092", title: "Ransomware Activity Detected", sev: "CRITICAL", status: "open", verdict: "Pending", root_cause: "Sysmon EventID 11 (FileCreate) flagged high-frequency encryption behavior in Finance-SRV-01. Extension: .locky.", q: [{l: "Malicious Process:", a: "tasksche.exe"}, {l: "Targeted Subnet:", a: "10.20.5.100"}], report: "" },
        2: { id: "#SOC-1022", title: "Domain Admin Brute Force", sev: "HIGH", status: "open", verdict: "Pending", root_cause: "EventID 4625 spike on DC-01. 50+ failed logons against 'adm_emir' account within 60 seconds.", q: [{l: "Target User:", a: "adm_emir"}, {l: "Attacker IP:", a: "192.168.1.150"}], report: "" },
        3: { id: "#SOC-883", title: "SQLi Exploit Attempt", sev: "HIGH", status: "open", verdict: "Pending", root_cause: "WAF block: UNION SELECT pattern matched in URI parameters. Database schema 'information_schema' targeted.", q: [{l: "Tool Agent:", a: "sqlmap/1.4.7"}, {l: "Schema Name:", a: "information_schema"}], report: "" },
        
        // --- PRE-CLOSED ARCHIVE CASES ---
        101: { id: "#SOC-2201", title: "Unauthorized PowerShell Download", sev: "HIGH", status: "closed", verdict: "True Positive", root_cause: "IEX (Invoke-Expression) used to download remote script from suspicious IP 45.33.2.11.", report: "Incident confirmed as a Red Team exercise. The analyst identified the encoded PowerShell command and isolated the workstation.", q:[] },
        102: { id: "#SOC-2188", title: "MFA Fatigue Attack", sev: "MEDIUM", status: "closed", verdict: "True Positive", root_cause: "User reported receiving 20+ MFA push notifications in 5 minutes.", report: "Account was compromised via password spraying. Attacker attempted MFA bypass. Password reset and session revocation completed.", q:[] },
        103: { id: "#SOC-2150", title: "Large ICMP Data Exfiltration", sev: "HIGH", status: "closed", verdict: "True Positive", root_cause: "Outbound ICMP packets with 1024-byte payloads detected by IDS.", report: "Confirmed data exfiltration via ICMP tunneling. Firewall rules updated to block non-standard ICMP traffic.", q:[] },
        104: { id: "#SOC-2090", title: "Dev Server Port Scan", sev: "LOW", status: "closed", verdict: "False Positive", root_cause: "Internal security scanner (Nessus) started a scheduled vulnerability scan.", report: "Activity was cross-referenced with the security maintenance schedule. No malicious intent found.", q:[] },
        105: { id: "#SOC-2012", title: "Anomalous Admin Login", sev: "MEDIUM", status: "closed", verdict: "False Positive", root_cause: "Admin logged in from a new VPN location at 3 AM.", report: "Confirmed with the admin that he was working late during a planned emergency maintenance. Legitimate activity.", q:[] }
    };

    let activeTab = 'open';

    function renderTable() {
        const body = document.getElementById('queueBody');
        body.innerHTML = "";
        Object.entries(cases).forEach(([key, c]) => {
            if(c.status !== activeTab) return;
            const verdictClass = c.verdict === 'True Positive' ? 'badge-tp' : (c.verdict === 'False Positive' ? 'badge-fp' : 'badge-pending');
            body.innerHTML += `
                <tr class="alert-row" onclick="openIncident(${key})">
                    <td style="font-family:'JetBrains Mono'; color:#58a6ff;">${c.id}</td>
                    <td><strong>${c.title}</strong></td>
                    <td><span style="color:${c.sev==='CRITICAL'?'#f85149':c.sev==='HIGH'?'#d29922':'#8b949e'}">${c.sev}</span></td>
                    <td><span class="badge ${verdictClass}">${c.verdict}</span></td>
                    <td><button class="btn-filter" style="width:100%">${c.status==='open'?'ANALYZE':'REPORT'}</button></td>
                </tr>
            `;
        });
        updateCounts();
    }

    function openIncident(key) {
        const c = cases[key];
        const body = document.getElementById('panelBody');
        const footer = document.getElementById('panelFooter');
        document.getElementById('panelId').innerText = c.id;

        if(c.status === 'open') {
            document.getElementById('headerMode').innerText = "FORENSIC ANALYSIS";
            body.innerHTML = `
                <div class="card">
                    <h4>Alert Root Cause</h4>
                    <div class="threat-info">${c.root_cause}</div>
                </div>
                <div class="card">
                    <h4>Artifact Verification</h4>
                    ${c.q.map((q, i) => `<label style="font-size:0.7rem; color:#8b949e; display:block; margin-bottom:5px;">${q.l}</label><input type="text" class="artifact-input" id="ans-${i}" placeholder="Extract from /logs..." autocomplete="off">`).join('')}
                </div>
                <div class="card">
                    <h4>Official Verdict</h4>
                    <select class="artifact-input" id="verdictSelect">
                        <option value="True Positive">True Positive (Confirmed Malicious)</option>
                        <option value="False Positive">False Positive (Benign / Testing)</option>
                    </select>
                    <label style="font-size:0.7rem; color:#8b949e;">Resolution Summary</label>
                    <textarea class="artifact-input" id="analystNotes" style="height:100px; resize:none;" placeholder="Summary of analysis and remediation..."></textarea>
                </div>
            `;
            footer.innerHTML = `
                <button class="btn-footer btn-esc" onclick="escalateL2(${key})">ESCALATE L2</button>
                <button class="btn-footer btn-res" onclick="resolveIncident(${key})">RESOLVE & CLOSE</button>
            `;
        } else {
            document.getElementById('headerMode').innerText = "ARCHIVED CASE REPORT";
            body.innerHTML = `
                <div class="card" style="border-top: 4px solid ${c.verdict==='True Positive'?'#f85149':'#58a6ff'}">
                    <h3 style="margin-top:0;">Incident Closure Report</h3>
                    <p style="font-size:0.85rem; color:#8b949e;"><strong>Verdict:</strong> <span class="badge ${c.verdict==='True Positive'?'badge-tp':'badge-fp'}">${c.verdict}</span></p>
                    <hr style="border:0; border-top:1px solid #30363d; margin:20px 0;">
                    <div style="font-size:0.9rem; line-height:1.7;">
                        <strong>Executive Summary:</strong><br>${c.report}<br><br>
                        <strong>Root Cause:</strong><br>${c.root_cause}
                    </div>
                </div>
            `;
            footer.innerHTML = `<button class="btn-footer btn-res" style="grid-column: span 2; background:#30363d;" onclick="closePanel()">CLOSE REPORT</button>`;
        }
        document.getElementById('sidePanel').classList.add('panel-active');
    }

    function resolveIncident(key) {
        const c = cases[key];
        let allCorrect = true;
        c.q.forEach((q, i) => {
            if(document.getElementById(`ans-${i}`).value.trim().toLowerCase() !== q.a.toLowerCase()) allCorrect = false;
        });

        if(!allCorrect) {
            alert("VALIDATION FAILED: The artifacts provided do not match forensic telemetry. Investigation incomplete.");
            return;
        }

        const notes = document.getElementById('analystNotes').value;
        const verdict = document.getElementById('verdictSelect').value;
        
        cases[key].status = 'closed';
        cases[key].verdict = verdict;
        cases[key].report = notes || "Resolved based on standard incident response playbook protocols.";
        
        renderTable();
        closePanel();
    }

    function switchTab(tab) {
        activeTab = tab;
        document.getElementById('tab-open').classList.toggle('active', tab==='open');
        document.getElementById('tab-closed').classList.toggle('active', tab==='closed');
        renderTable();
    }

    function updateCounts() {
        const o = Object.values(cases).filter(c => c.status === 'open').length;
        const cl = Object.values(cases).filter(c => c.status === 'closed').length;
        document.getElementById('count-open').innerText = o;
        document.getElementById('count-closed').innerText = cl;
    }

    function escalateL2(key) { alert("SİMÜLASYON: Olay " + cases[key].id + " derinlemesine analiz için Kıdemli Analiste (L2) devredilmiştir."); closePanel(); }
    function closePanel() { document.getElementById('sidePanel').classList.remove('panel-active'); }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }

    window.onload = () => {
        document.getElementById('welcomeModal').style.display = 'flex';
        renderTable();
    };
</script>
