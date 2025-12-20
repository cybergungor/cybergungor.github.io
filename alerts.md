---
layout: default
title: SentinelGuard - Incident Management Dashboard
permalink: /alerts/
---

<style>
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    .soc-wrapper { display: flex; flex-direction: column; height: calc(100vh - 60px); color: #e6edf3; font-family: 'Inter', sans-serif; }

    /* --- INITIAL DISCLAIMER --- */
    .disclaimer-modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(1, 4, 9, 0.96); z-index: 10000; display: none; align-items: center; justify-content: center; backdrop-filter: blur(15px); }
    .disclaimer-box { background: #0d1117; border: 1px solid #30363d; width: 750px; border-radius: 12px; overflow: hidden; animation: slideUp 0.4s ease-out; }
    .disclaimer-header { background: #161b22; color: #58a6ff; padding: 20px; font-family: 'JetBrains Mono'; font-weight: bold; text-align: center; border-bottom: 1px solid #30363d; }
    .btn-ack { width: 100%; padding: 18px; background: #238636; color: #fff; border: none; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; }

    /* --- FILTER BAR --- */
    .soc-top-bar { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .filter-group { display: flex; gap: 10px; }
    .btn-filter { background: #161b22; border: 1px solid #30363d; color: #8b949e; padding: 6px 15px; border-radius: 4px; font-size: 0.75rem; cursor: pointer; font-family: 'JetBrains Mono'; transition: 0.2s; }
    .btn-filter.active { background: #58a6ff; color: #0d1117; border-color: #58a6ff; font-weight: bold; }

    /* --- TABLE --- */
    .queue-container { flex: 1; padding: 25px; overflow-y: auto; }
    .incident-table { width: 100%; border-collapse: collapse; background: #0d1117; border: 1px solid #30363d; border-radius: 6px; }
    .incident-table th { background: #161b22; text-align: left; padding: 12px 15px; font-size: 0.7rem; color: #8b949e; text-transform: uppercase; letter-spacing: 1px; }
    .incident-table td { padding: 15px; border-bottom: 1px solid #21262d; font-size: 0.85rem; }
    .alert-row:hover { background: #121d2f; cursor: pointer; }

    /* --- PANEL & MODALS --- */
    .side-panel { position: fixed; top: 0; right: -600px; width: 550px; height: 100vh; background: #0d1117; border-left: 1px solid #30363d; transition: 0.4s; z-index: 9000; display: flex; flex-direction: column; box-shadow: -20px 0 50px rgba(0,0,0,0.7); }
    .panel-active { right: 0; }
    .panel-header { padding: 20px; background: #161b22; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; }
    .panel-body { padding: 25px; flex: 1; overflow-y: auto; }
    
    .card { background: #161b22; border: 1px solid #30363d; border-radius: 8px; padding: 15px; margin-bottom: 20px; }
    .card h4 { margin: 0 0 10px 0; font-size: 0.75rem; color: #58a6ff; text-transform: uppercase; border-bottom: 1px solid #30363d; padding-bottom: 5px; }
    
    .artifact-input { width: 100%; background: #010409; border: 1px solid #30363d; color: #e6edf3; padding: 10px; font-family: 'JetBrains Mono'; font-size: 0.8rem; border-radius: 4px; margin-bottom: 10px; }
    .threat-info { font-size: 0.8rem; color: #8b949e; line-height: 1.5; background: rgba(88, 166, 255, 0.05); padding: 12px; border-left: 3px solid #58a6ff; margin-bottom: 15px; }

    /* Report Styles */
    .report-badge { padding: 4px 8px; border-radius: 4px; font-size: 0.7rem; font-weight: bold; margin-bottom: 10px; display: inline-block; }
    .tp { background: rgba(248, 81, 73, 0.1); color: #f85149; border: 1px solid #f85149; }
    .fp { background: rgba(88, 166, 255, 0.1); color: #58a6ff; border: 1px solid #58a6ff; }

    .btn-resolve { background: #238636; color: white; width: 100%; padding: 12px; border: none; border-radius: 4px; font-weight: bold; cursor: pointer; }
    .select-field { background: #010409; color: white; border: 1px solid #30363d; padding: 10px; width: 100%; border-radius: 4px; margin-bottom: 15px; }

    @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
</style>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">SENTINEL GUARD // OPERATIONAL INTERFACE v5.1</div>
        <div class="disclaimer-body">
            <div style="flex:1;">
                <h3>[TR] VAKA ANALİZİ</h3>
                <p>Aktif alarmları inceleyin, delilleri toplayın ve "True Positive" veya "False Positive" kararı vererek resmi raporunuzu oluşturun.</p>
            </div>
            <div style="width:1px; background:#30363d; margin:0 20px;"></div>
            <div style="flex:1;">
                <h3>[EN] INCIDENT LIFECYCLE</h3>
                <p>Execute playbooks, determine verdict (TP/FP), and generate professional IR reports to demonstrate SOC expertise.</p>
            </div>
        </div>
        <button class="btn-ack" onclick="closeModal('welcomeModal')">START SESSION</button>
    </div>
</div>

<div class="soc-wrapper">
    <div class="soc-top-bar">
        <div class="filter-group">
            <button class="btn-filter active" id="filter-open" onclick="switchTab('open')">OPEN ALERTS (<span id="count-open">3</span>)</button>
            <button class="btn-filter" id="filter-closed" onclick="switchTab('closed')">CLOSED ARCHIVE (<span id="count-closed">0</span>)</button>
        </div>
        <div style="font-family:'JetBrains Mono'; font-size:0.75rem; color:#8b949e;">ANALYST: <span style="color:#58a6ff">L1_PRIMARY</span></div>
    </div>

    <div class="queue-container">
        <table class="incident-table">
            <thead>
                <tr>
                    <th width="120">Case ID</th>
                    <th>Tactical Description</th>
                    <th width="120">Severity</th>
                    <th width="120">Verdict</th>
                    <th width="120">Action</th>
                </tr>
            </thead>
            <tbody id="alertQueue"></tbody>
        </table>
    </div>
</div>

<div id="sidePanel" class="side-panel">
    <div class="panel-header">
        <div style="font-family:'JetBrains Mono'; font-size:0.85rem; font-weight:bold;">
            <span id="panelMode">INVESTIGATION</span>: <span id="panelId" style="color:#58a6ff;">#SOC-0000</span>
        </div>
        <span style="cursor:pointer; font-size:1.5rem;" onclick="closePanel()">×</span>
    </div>

    <div class="panel-body" id="panelContent">
        </div>

    <div id="panelFooter" style="padding:20px; border-top:1px solid #30363d; background:#0d1117;">
        </div>
</div>

<script>
    const caseData = {
        1: { id: "#SOC-5092", title: "Ransomware Pattern Detected", sev: "CRITICAL", status: "open", verdict: "-", root_cause: "Triggered by Sysmon Event 11. Large scale file modifications with high entropy extensions (.locky) detected in Finance share.", q: [{l: "Malicious Process:", a: "tasksche.exe"}, {l: "C2 IP Address:", a: "10.20.5.100"}], report: "" },
        2: { id: "#SOC-1022", title: "Domain Admin Bruteforce", sev: "HIGH", status: "open", verdict: "-", root_cause: "Event ID 4625 spike. Multiple failed login attempts detected against 'adm_emir' from a non-standard workstation.", q: [{l: "Target User:", a: "adm_emir"}, {l: "Attacker IP:", a: "192.168.1.150"}], report: "" },
        3: { id: "#SOC-883", title: "SQL Injection Exploit Attempt", sev: "HIGH", status: "open", verdict: "-", root_cause: "WAF signature match for UNION-based SQLi. Payload targets 'information_schema' to extract database structure.", q: [{l: "Tool Detected:", a: "sqlmap/1.4.7"}, {l: "Target Schema:", a: "information_schema"}], report: "" }
    };

    let currentTab = 'open';

    function renderTable() {
        const body = document.getElementById('alertQueue');
        body.innerHTML = "";
        Object.entries(caseData).forEach(([key, c]) => {
            if(c.status !== currentTab) return;
            body.innerHTML += `
                <tr class="alert-row" onclick="viewCase(${key})">
                    <td style="font-family:'JetBrains Mono'; color:#58a6ff;">${c.id}</td>
                    <td><strong>${c.title}</strong></td>
                    <td><span style="color:${c.sev==='CRITICAL'?'#f85149':'#d29922'}">${c.sev}</span></td>
                    <td><span class="report-badge ${c.verdict==='True Positive'?'tp':'fp'}">${c.verdict}</span></td>
                    <td><button class="btn-filter" style="width:100%">${c.status==='open'?'ANALYZE':'VIEW REPORT'}</button></td>
                </tr>
            `;
        });
        updateCounts();
    }

    function viewCase(key) {
        const c = caseData[key];
        const content = document.getElementById('panelContent');
        const footer = document.getElementById('panelFooter');
        document.getElementById('panelId').innerText = c.id;

        if(c.status === 'open') {
            document.getElementById('panelMode').innerText = "ANALYSIS";
            content.innerHTML = `
                <div class="card">
                    <h4>Threat Intel / Root Cause</h4>
                    <div class="threat-info">${c.root_cause}</div>
                </div>
                <div class="card">
                    <h4>Artifact Collection</h4>
                    ${c.q.map((q, i) => `<label style="font-size:0.7rem; color:#8b949e;">${q.l}</label><input type="text" class="artifact-input" id="ans-${i}" autocomplete="off">`).join('')}
                </div>
                <div class="card">
                    <h4>Official Verdict</h4>
                    <select class="select-field" id="verdictSelect">
                        <option value="True Positive">True Positive (Confirmed Threat)</option>
                        <option value="False Positive">False Positive (Benign Activity)</option>
                    </select>
                    <label style="font-size:0.7rem; color:#8b949e;">Final Analyst Notes</label>
                    <textarea class="artifact-input" id="finalNotes" style="height:80px; resize:none;"></textarea>
                </div>
            `;
            footer.innerHTML = `<button class="btn-resolve" onclick="resolveCase(${key})">EXECUTE DISPOSITION & CLOSE CASE</button>`;
        } else {
            document.getElementById('panelMode').innerText = "ARCHIVED REPORT";
            content.innerHTML = `
                <div class="card" style="border-left: 4px solid ${c.verdict==='True Positive'?'#f85149':'#58a6ff'}">
                    <div class="report-badge ${c.verdict==='True Positive'?'tp':'fp'}">${c.verdict}</div>
                    <h3 style="margin:10px 0;">Incident Closing Report</h3>
                    <p style="font-size:0.8rem; color:#8b949e;"><strong>Resolution Date:</strong> ${new Date().toLocaleDateString()}</p>
                    <hr style="border:0; border-top:1px solid #30363d; margin:15px 0;">
                    <div style="font-size:0.85rem; line-height:1.6;">
                        <strong>Executive Summary:</strong><br>
                        ${c.report}<br><br>
                        <strong>Root Cause:</strong><br>${c.root_cause}
                    </div>
                </div>
            `;
            footer.innerHTML = `<button class="btn-filter" style="width:100%" onclick="closePanel()">CLOSE VIEW</button>`;
        }
        document.getElementById('sidePanel').classList.add('panel-active');
    }

    function resolveCase(key) {
        const c = caseData[key];
        let valid = true;
        c.q.forEach((q, i) => {
            if(document.getElementById(`ans-${i}`).value.trim().toLowerCase() !== q.a.toLowerCase()) valid = false;
        });

        if(!valid) {
            alert("Analysis Error: Provided artifacts do not match forensic logs. Re-examine the telemetry.");
            return;
        }

        const notes = document.getElementById('finalNotes').value;
        const verdict = document.getElementById('verdictSelect').value;
        
        caseData[key].status = 'closed';
        caseData[key].verdict = verdict;
        caseData[key].report = notes || "No specific analyst notes provided. Threat mitigated according to standard playbook.";
        
        renderTable();
        closePanel();
    }

    function switchTab(tab) {
        currentTab = tab;
        document.getElementById('filter-open').classList.toggle('active', tab==='open');
        document.getElementById('filter-closed').classList.toggle('active', tab==='closed');
        renderTable();
    }

    function updateCounts() {
        const o = Object.values(caseData).filter(c => c.status === 'open').length;
        const cl = Object.values(caseData).filter(c => c.status === 'closed').length;
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
