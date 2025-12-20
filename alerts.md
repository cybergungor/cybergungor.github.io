---
layout: default
title: SentinelGuard - Advanced Incident Management
permalink: /alerts/
---

<style>
    /* Full Page UI */
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    .soc-wrapper { display: flex; flex-direction: column; height: calc(100vh - 60px); color: #e6edf3; font-family: 'Inter', sans-serif; }

    /* --- DISCLAIMER --- */
    .disclaimer-modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(1, 4, 9, 0.96); z-index: 10000; display: none; align-items: center; justify-content: center; backdrop-filter: blur(15px); }
    .disclaimer-box { background: #0d1117; border: 1px solid #30363d; width: 750px; border-radius: 12px; overflow: hidden; animation: slideUp 0.4s ease-out; }
    .disclaimer-header { background: #161b22; color: #58a6ff; padding: 20px; font-family: 'JetBrains Mono'; font-weight: bold; text-align: center; border-bottom: 1px solid #30363d; }
    .btn-ack { width: 100%; padding: 18px; background: #238636; color: #fff; border: none; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; transition: 0.2s; }

    /* --- TOP BAR & FILTER --- */
    .soc-top-bar { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .btn-filter { background: #161b22; border: 1px solid #30363d; color: #8b949e; padding: 8px 18px; border-radius: 4px; font-size: 0.75rem; cursor: pointer; font-family: 'JetBrains Mono'; transition: 0.2s; margin-right: 5px; }
    .btn-filter.active { background: #58a6ff; color: #0d1117; border-color: #58a6ff; font-weight: bold; }

    /* --- TABLE --- */
    .queue-container { flex: 1; padding: 25px; overflow-y: auto; }
    .incident-table { width: 100%; border-collapse: collapse; background: #0d1117; border: 1px solid #30363d; border-radius: 6px; }
    .incident-table th { background: #161b22; text-align: left; padding: 12px 15px; font-size: 0.7rem; color: #8b949e; text-transform: uppercase; letter-spacing: 1px; }
    .incident-table td { padding: 15px; border-bottom: 1px solid #21262d; font-size: 0.85rem; }
    .alert-row:hover { background: #121d2f; cursor: pointer; }

    /* --- SIDE PANEL --- */
    .side-panel { position: fixed; top: 0; right: -600px; width: 550px; height: 100vh; background: #0d1117; border-left: 1px solid #30363d; transition: 0.4s; z-index: 9000; display: flex; flex-direction: column; box-shadow: -20px 0 50px rgba(0,0,0,0.7); }
    .panel-active { right: 0; }
    .panel-header { padding: 20px; background: #161b22; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .panel-body { padding: 25px; flex: 1; overflow-y: auto; }
    
    .card { background: #161b22; border: 1px solid #30363d; border-radius: 8px; padding: 15px; margin-bottom: 20px; }
    .card h4 { margin: 0 0 10px 0; font-size: 0.7rem; color: #58a6ff; text-transform: uppercase; border-bottom: 1px solid #30363d; padding-bottom: 5px; margin-bottom: 10px; }
    
    .artifact-input { width: 100%; background: #010409; border: 1px solid #30363d; color: #e6edf3; padding: 10px; font-family: 'JetBrains Mono'; font-size: 0.8rem; border-radius: 4px; margin-bottom: 10px; }
    .threat-info { font-size: 0.8rem; color: #8b949e; line-height: 1.5; background: rgba(88, 166, 255, 0.05); padding: 12px; border-left: 3px solid #58a6ff; margin-bottom: 10px; }

    /* Verdict Badges */
    .badge { padding: 3px 8px; border-radius: 3px; font-size: 0.7rem; font-weight: bold; }
    .badge-tp { background: rgba(248, 81, 73, 0.1); color: #f85149; border: 1px solid #f85149; }
    .badge-fp { background: rgba(88, 166, 255, 0.1); color: #58a6ff; border: 1px solid #58a6ff; }
    .badge-none { background: #30363d; color: #8b949e; }

    /* Footer Buttons */
    .panel-footer { padding: 20px; background: #0d1117; border-top: 1px solid #30363d; display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
    .btn-footer { padding: 12px; border-radius: 4px; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; font-size: 0.75rem; border: none; }
    .btn-res { background: #238636; color: white; }
    .btn-esc { background: #d29922; color: #0d1117; }
    .btn-res:hover { background: #2ea043; }

    @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
</style>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">SENTINEL GUARD // MISSION PROTOCOL</div>
        <div class="disclaimer-body">
            <div style="flex:1;">
                <h3>[TR] PROFESYONEL ANALİZ</h3>
                <p>Alarmları inceleyin, TP/FP kararı verin ve L2 yükseltme (Escalation) süreçlerini yönetin. Kapalı vakalar arşivde raporlanır.</p>
            </div>
            <div style="width:1px; background:#30363d; margin:0 20px;"></div>
            <div style="flex:1;">
                <h3>[EN] INCIDENT RESPONSE</h3>
                <p>Analyze alerts, determine verdict, and manage L2 escalation. All closed incidents are archived with full forensic reports.</p>
            </div>
        </div>
        <button class="btn-ack" onclick="closeModal('welcomeModal')">ACCESS MONITORING QUEUE</button>
    </div>
</div>

<div class="soc-wrapper">
    <div class="soc-top-bar">
        <div class="filter-group">
            <button class="btn-filter active" id="tab-open" onclick="switchTab('open')">OPEN ALERTS (<span id="count-open">3</span>)</button>
            <button class="btn-filter" id="tab-closed" onclick="switchTab('closed')">CLOSED ARCHIVE (<span id="count-closed">0</span>)</button>
        </div>
        <div style="font-family:'JetBrains Mono'; font-size:0.75rem; color:#8b949e;">ANALYST_ID: <span style="color:#58a6ff">E_GUNGOROGLU</span></div>
    </div>

    <div class="queue-container">
        <table class="incident-table">
            <thead>
                <tr>
                    <th width="120">Alert ID</th>
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
        <div style="font-family:'JetBrains Mono'; font-size:0.8rem; font-weight:bold;">
            <span id="headerMode">INVESTIGATION</span>: <span id="panelId" style="color:#58a6ff;">#SOC-0000</span>
        </div>
        <span style="cursor:pointer; font-size:1.5rem;" onclick="closePanel()">×</span>
    </div>
    <div class="panel-body" id="panelBody"></div>
    <div class="panel-footer" id="panelFooter"></div>
</div>

<script>
    const cases = {
        1: { id: "#SOC-5092", title: "Ransomware Pattern Detected", sev: "CRITICAL", status: "open", verdict: "Pending", root_cause: "Sysmon ID 11 detected massive file renames in Finance-SRV. Potential ransomware encryption in progress.", q: [{l: "Process Name:", a: "tasksche.exe"}, {l: "Target IP:", a: "10.20.5.100"}], report: "" },
        2: { id: "#SOC-1022", title: "Privileged Account Bruteforce", sev: "HIGH", status: "open", verdict: "Pending", root_cause: "High volume of Event ID 4625 (Failed Logon) against 'adm_emir' from internal IP 192.168.1.150.", q: [{l: "Target User:", a: "adm_emir"}, {l: "Attacker IP:", a: "192.168.1.150"}], report: "" },
        3: { id: "#SOC-883", title: "SQL Injection Exploit Attempt", sev: "HIGH", status: "open", verdict: "Pending", root_cause: "HTTP GET request with UNION SELECT signature detected. Targeted 'information_schema' to dump DB structure.", q: [{l: "Tool String:", a: "sqlmap/1.4.7"}, {l: "Target Schema:", a: "information_schema"}], report: "" }
    };

    let activeTab = 'open';

    function renderTable() {
        const body = document.getElementById('queueBody');
        body.innerHTML = "";
        Object.entries(cases).forEach(([key, c]) => {
            if(c.status !== activeTab) return;
            const verdictClass = c.verdict === 'True Positive' ? 'badge-tp' : (c.verdict === 'False Positive' ? 'badge-fp' : 'badge-none');
            body.innerHTML += `
                <tr class="alert-row" onclick="openIncident(${key})">
                    <td class="mono" style="color:#58a6ff;">${c.id}</td>
                    <td><strong>${c.title}</strong></td>
                    <td><span style="color:${c.sev==='CRITICAL'?'#f85149':'#d29922'}">${c.sev}</span></td>
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
            document.getElementById('headerMode').innerText = "ANALYSIS";
            body.innerHTML = `
                <div class="card">
                    <h4>Threat Context / Trigger</h4>
                    <div class="threat-info">${c.root_cause}</div>
                    <p style="font-size:0.75rem; color:#8b949e;">*Hint: Identify artifacts in /logs to proceed.</p>
                </div>
                <div class="card">
                    <h4>Step 1: Artifact Collection</h4>
                    ${c.q.map((q, i) => `<label class="artifact-label" style="font-size:0.7rem; color:#8b949e;">${q.l}</label><input type="text" class="artifact-input" id="ans-${i}" autocomplete="off">`).join('')}
                </div>
                <div class="card">
                    <h4>Step 2: Analyst Verdict</h4>
                    <select class="artifact-input" id="verdictSelect">
                        <option value="True Positive">True Positive (Confirmed Malicious)</option>
                        <option value="False Positive">False Positive (Benign/Test)</option>
                    </select>
                    <label style="font-size:0.7rem; color:#8b949e;">Investigation Summary</label>
                    <textarea class="artifact-input" id="analystNotes" style="height:80px; resize:none;" placeholder="Detail the root cause and mitigation..."></textarea>
                </div>
            `;
            footer.innerHTML = `
                <button class="btn-footer btn-esc" onclick="escalateL2(${key})">ESCALATE L2</button>
                <button class="btn-footer btn-res" onclick="resolveIncident(${key})">RESOLVE INCIDENT</button>
            `;
        } else {
            document.getElementById('headerMode').innerText = "CLOSED REPORT";
            body.innerHTML = `
                <div class="card" style="border-left: 4px solid ${c.verdict==='True Positive'?'#f85149':'#58a6ff'}">
                    <h3 style="margin-top:0;">Incident Closure Report</h3>
                    <p style="font-size:0.8rem; color:#8b949e;"><strong>Verdict:</strong> ${c.verdict}</p>
                    <hr style="border:0; border-top:1px solid #30363d; margin:15px 0;">
                    <div style="font-size:0.85rem; line-height:1.6;">
                        <strong>Executive Summary:</strong><br>${c.report}<br><br>
                        <strong>Root Cause:</strong><br>${c.root_cause}
                    </div>
                </div>
            `;
            footer.innerHTML = `<button class="btn-footer btn-res" style="grid-column: span 2; background:#30363d;" onclick="closePanel()">CLOSE VIEW</button>`;
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
            alert("TELEMETRY ERROR: Provided artifacts do not match forensic logs. Verification failed.");
            return;
        }

        const notes = document.getElementById('analystNotes').value;
        const verdict = document.getElementById('verdictSelect').value;
        
        cases[key].status = 'closed';
        cases[key].verdict = verdict;
        cases[key].report = notes || "Resolved based on playbook standard mitigation procedures.";
        
        renderTable();
        closePanel();
    }

    function escalateL2(key) {
        alert("ACTION: Incident " + cases[key].id + " has been escalated to Tier-2 SOC Team for deep forensic analysis.");
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

    function closePanel() { document.getElementById('sidePanel').classList.remove('panel-active'); }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }

    window.onload = () => {
        document.getElementById('welcomeModal').style.display = 'flex';
        renderTable();
    };
</script>
