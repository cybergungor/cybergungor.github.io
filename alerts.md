---
layout: default
title: SentinelGuard - Incident Management Dashboard
permalink: /alerts/
---

<style>
    /* Full Page & Theme Overrides */
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    .soc-wrapper { display: flex; flex-direction: column; height: calc(100vh - 60px); color: #e6edf3; font-family: 'Inter', sans-serif; }

    /* --- INITIAL DISCLAIMER MODAL --- */
    .disclaimer-modal {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background: rgba(1, 4, 9, 0.96); z-index: 10000;
        display: none; align-items: center; justify-content: center; backdrop-filter: blur(15px);
    }
    .disclaimer-box { 
        background: #0d1117; border: 1px solid #30363d; width: 750px; 
        border-radius: 12px; overflow: hidden; box-shadow: 0 25px 50px rgba(0,0,0,0.5);
        animation: slideUp 0.4s ease-out;
    }
    .disclaimer-header { background: #161b22; color: #58a6ff; padding: 20px; font-family: 'JetBrains Mono'; font-weight: bold; text-align: center; border-bottom: 1px solid #30363d; }
    .disclaimer-body { padding: 30px; display: flex; gap: 30px; line-height: 1.6; }
    .v-divider { width: 1px; background: #30363d; }
    .lang-section h3 { font-size: 0.8rem; color: #58a6ff; margin-bottom: 12px; font-family: 'JetBrains Mono'; border-bottom: 1px solid #30363d; padding-bottom: 5px; }
    .lang-section p { font-size: 0.85rem; color: #8b949e; }
    .btn-ack { width: 100%; padding: 18px; background: #238636; color: #fff; border: none; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; transition: 0.2s; }
    .btn-ack:hover { background: #2ea043; }

    /* --- TOP NAV & FILTER BAR --- */
    .soc-top-bar { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .filter-group { display: flex; gap: 10px; align-items: center; }
    .btn-filter { 
        background: #161b22; border: 1px solid #30363d; color: #8b949e; 
        padding: 6px 15px; border-radius: 4px; font-size: 0.75rem; cursor: pointer; 
        font-family: 'JetBrains Mono'; transition: 0.2s;
    }
    .btn-filter.active { background: #58a6ff; color: #0d1117; border-color: #58a6ff; font-weight: bold; }
    .btn-filter:hover:not(.active) { border-color: #58a6ff; color: #58a6ff; }

    /* --- TABLE DESIGN --- */
    .queue-container { flex: 1; padding: 25px; overflow-y: auto; }
    .incident-table { width: 100%; border-collapse: collapse; background: #0d1117; border: 1px solid #30363d; border-radius: 6px; overflow: hidden; }
    .incident-table th { background: #161b22; text-align: left; padding: 12px 15px; font-size: 0.7rem; color: #8b949e; text-transform: uppercase; letter-spacing: 1px; }
    .incident-table td { padding: 15px; border-bottom: 1px solid #21262d; font-size: 0.85rem; }
    .incident-table tr:hover { background: #121d2f; cursor: pointer; }

    /* Severity & Status Badges */
    .sev { padding: 3px 8px; border-radius: 3px; font-size: 0.65rem; font-weight: bold; border: 1px solid transparent; }
    .sev-critical { color: #f85149; border-color: rgba(248, 81, 73, 0.4); background: rgba(248, 81, 73, 0.1); }
    .sev-high { color: #d29922; border-color: rgba(210, 153, 34, 0.4); background: rgba(210, 153, 34, 0.1); }
    .status-badge { font-family: 'JetBrains Mono'; font-size: 0.7rem; font-weight: bold; }
    .status-open { color: #f85149; }
    .status-closed { color: #3fb950; }

    /* --- INVESTIGATION SIDE PANEL --- */
    .investigation-panel {
        position: fixed; top: 0; right: -500px; width: 480px; height: 100vh;
        background: #0d1117; border-left: 1px solid #30363d; box-shadow: -15px 0 40px rgba(0,0,0,0.6);
        transition: right 0.4s cubic-bezier(0.05, 0.7, 0.1, 1); z-index: 9000;
        display: flex; flex-direction: column;
    }
    .panel-active { right: 0; }
    .panel-header { padding: 20px; background: #161b22; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .panel-body { padding: 25px; flex: 1; overflow-y: auto; }
    .playbook-card { background: #161b22; border: 1px solid #30363d; border-radius: 8px; padding: 20px; margin-bottom: 25px; }
    .playbook-card h4 { margin: 0 0 15px 0; font-size: 0.75rem; color: #58a6ff; text-transform: uppercase; letter-spacing: 1px; border-bottom: 1px solid #30363d; padding-bottom: 8px; }
    
    .artifact-label { display: block; font-size: 0.7rem; color: #8b949e; margin-bottom: 5px; text-transform: uppercase; }
    .artifact-input {
        width: 100%; background: #010409; border: 1px solid #30363d; color: #e6edf3;
        padding: 12px; font-family: 'JetBrains Mono', monospace; font-size: 0.8rem; border-radius: 4px; margin-bottom: 15px;
    }
    .artifact-input:focus { border-color: #58a6ff; outline: none; box-shadow: 0 0 8px rgba(88, 166, 255, 0.2); }

    .panel-footer { padding: 20px; background: #0d1117; border-top: 1px solid #30363d; display: flex; gap: 10px; }
    .btn-panel { flex: 1; padding: 12px; border-radius: 4px; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; font-size: 0.75rem; border: none; transition: 0.2s; }
    .btn-escalate { background: #d29922; color: #0d1117; }
    .btn-resolve { background: #238636; color: #fff; }
    .btn-resolve:hover { background: #2ea043; }
    .btn-escalate:hover { background: #e3a92b; }

    @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
</style>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">INCIDENT RESPONSE TERMINAL // MISSION BRIEFING</div>
        <div class="disclaimer-body">
            <div class="lang-section">
                <h3>[TR] VAKA YÖNETİMİ</h3>
                <p>Aktif alarmları analiz etmek için <strong>Analyze</strong> butonuna basın. Delilleri toplamak için <strong>Logs</strong> sayfasını kullanın.</p>
                <p style="font-size:0.75rem; color:#d29922;">*İşveren Notu: Bu panel vaka döngüsünü (Triage, Analysis, Resolution) simüle eder.</p>
            </div>
            <div class="v-divider"></div>
            <div class="lang-section">
                <h3>[EN] INCIDENT LIFECYCLE</h3>
                <p>Monitor live alerts, perform triage, and extract forensic evidence. Use the <strong>Logs</strong> tab to verify artifacts.</p>
                <p style="font-size:0.75rem; color:#d29922;">*Recruiter Note: This dashboard demonstrates full incident lifecycle management skills.</p>
            </div>
        </div>
        <button class="btn-ack" onclick="closeModal('welcomeModal')">ACCESS MONITORING QUEUE</button>
    </div>
</div>

<div class="soc-wrapper">
    <div class="soc-top-bar">
        <div class="filter-group">
            <button class="btn-filter active" id="filter-all" onclick="filterQueue('all')">ALL ALERTS</button>
            <button class="btn-filter" id="filter-open" onclick="filterQueue('open')">OPEN (3)</button>
            <button class="btn-filter" id="filter-closed" onclick="filterQueue('closed')">CLOSED (0)</button>
        </div>
        <div style="font-family:'JetBrains Mono'; font-size:0.75rem; color:#8b949e;">
            ANALYST_STATUS: <span style="color:#3fb950">● ONLINE</span>
        </div>
    </div>

    <div class="queue-container">
        <table class="incident-table">
            <thead>
                <tr>
                    <th width="120">ID</th>
                    <th>Tactics & Techniques</th>
                    <th width="150">Severity</th>
                    <th width="150">Status</th>
                    <th width="120">Action</th>
                </tr>
            </thead>
            <tbody id="alertQueueBody">
                <tr class="alert-row open" id="row-1" onclick="inspectCase(1)">
                    <td class="mono">#SOC-5092</td>
                    <td><strong>Host-Based Ransomware Activity</strong> <span style="color:#8b949e; font-size:0.7rem;">(T1486) // Financial_Srv_01</span></td>
                    <td><span class="sev sev-critical">CRITICAL</span></td>
                    <td class="status-badge status-open" id="stat-1">● OPEN</td>
                    <td><button class="btn-filter" style="width:100%">ANALYZE</button></td>
                </tr>
                <tr class="alert-row open" id="row-2" onclick="inspectCase(2)">
                    <td class="mono">#SOC-1022</td>
                    <td><strong>Privileged Account Bruteforce</strong> <span style="color:#8b949e; font-size:0.7rem;">(T1110) // Domain_Controller</span></td>
                    <td><span class="sev sev-high">HIGH</span></td>
                    <td class="status-badge status-open" id="stat-2">● OPEN</td>
                    <td><button class="btn-filter" style="width:100%">ANALYZE</button></td>
                </tr>
                <tr class="alert-row open" id="row-3" onclick="inspectCase(3)">
                    <td class="mono">#SOC-883</td>
                    <td><strong>Advanced SQL Injection Attempt</strong> <span style="color:#8b949e; font-size:0.7rem;">(T1190) // Web_Prod_Server</span></td>
                    <td><span class="sev sev-high">HIGH</span></td>
                    <td class="status-badge status-open" id="stat-3">● OPEN</td>
                    <td><button class="btn-filter" style="width:100%">ANALYZE</button></td>
                </tr>
            </tbody>
        </table>
    </div>
</div>

<div id="sidePanel" class="investigation-panel">
    <div class="panel-header">
        <div style="font-family:'JetBrains Mono'; font-size:0.85rem; font-weight:bold;">
            INVESTIGATION: <span id="panelId" style="color:#58a6ff;">#SOC-0000</span>
        </div>
        <span style="cursor:pointer; font-size:1.5rem;" onclick="closePanel()">×</span>
    </div>

    <div class="panel-body">
        <div class="playbook-card">
            <h4>Playbook: Artifact Verification</h4>
            <p style="font-size:0.75rem; color:#8b949e; margin-bottom:15px;">Cross-reference telemetry in the <a href="/logs/" target="_blank" style="color:#58a6ff;">SIEM Log Analytics</a> panel to verify the following indicators.</p>
            <div id="artifactFields">
                </div>
        </div>

        <div class="playbook-card">
            <h4>Disposition & Notes</h4>
            <label class="artifact-label">Executive Summary</label>
            <textarea class="artifact-input" style="height:100px; resize:none;" placeholder="Describe the threat, impact, and your mitigation steps..."></textarea>
        </div>
    </div>

    <div class="panel-footer">
        <button class="btn-panel btn-escalate" onclick="processAction('Escalated to SOC Tier-2')">ESCALATE L2</button>
        <button class="btn-panel btn-resolve" onclick="validateCase()">RESOLVE CASE</button>
    </div>
</div>

<script>
    const caseData = {
        1: { id: "#SOC-5092", q: [{l: "Malicious Process Name:", a: "tasksche.exe"}, {l: "Target Destination IP:", a: "10.20.5.100"}], status: 'open' },
        2: { id: "#SOC-1022", q: [{l: "Target Username:", a: "adm_emir"}, {l: "Attacker Source IP:", a: "192.168.1.150"}], status: 'open' },
        3: { id: "#SOC-883", q: [{l: "SQLi Tool Agent:", a: "sqlmap/1.4.7"}, {l: "Database Targeted:", a: "information_schema"}], status: 'open' }
    };

    let activeCaseId = null;

    function inspectCase(id) {
        if(caseData[id].status === 'closed') return;
        activeCaseId = id;
        const data = caseData[id];
        document.getElementById('panelId').innerText = data.id;
        const container = document.getElementById('artifactFields');
        container.innerHTML = data.q.map((item, i) => `
            <label class="artifact-label">${item.l}</label>
            <input type="text" class="artifact-input" id="ans-${i}" placeholder="Extract from logs..." autocomplete="off">
        `).join('');
        document.getElementById('sidePanel').classList.add('panel-active');
    }

    function validateCase() {
        const data = caseData[activeCaseId];
        let isValid = true;
        data.q.forEach((item, i) => {
            if(document.getElementById(`ans-${i}`).value.trim().toLowerCase() !== item.a.toLowerCase()) {
                isValid = false;
                document.getElementById(`ans-${i}`).style.borderColor = "#f85149";
            } else {
                document.getElementById(`ans-${i}`).style.borderColor = "#238636";
            }
        });

        if(isValid) {
            caseData[activeCaseId].status = 'closed';
            document.getElementById(`row-${activeCaseId}`).classList.remove('open');
            document.getElementById(`row-${activeCaseId}`).classList.add('closed');
            document.getElementById(`stat-${activeCaseId}`).innerText = "✔ CLOSED";
            document.getElementById(`stat-${activeCaseId}`).className = "status-badge status-closed";
            updateFilterCounts();
            closePanel();
        } else {
            alert("Validation Failed: The artifacts provided do not match the forensic logs.");
        }
    }

    function filterQueue(type) {
        document.querySelectorAll('.btn-filter').forEach(btn => btn.classList.remove('active'));
        document.getElementById(`filter-${type}`).classList.add('active');
        
        const rows = document.querySelectorAll('.alert-row');
        rows.forEach(row => {
            if(type === 'all') row.style.display = 'table-row';
            else if(type === 'open') row.style.display = row.classList.contains('open') ? 'table-row' : 'none';
            else if(type === 'closed') row.style.display = row.classList.contains('closed') ? 'table-row' : 'none';
        });
    }

    function updateFilterCounts() {
        const open = Object.values(caseData).filter(c => c.status === 'open').length;
        const closed = Object.values(caseData).filter(c => c.status === 'closed').length;
        document.getElementById('filter-open').innerText = `OPEN (${open})`;
        document.getElementById('filter-closed').innerText = `CLOSED (${closed})`;
    }

    function closePanel() { document.getElementById('sidePanel').classList.remove('panel-active'); }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }
    function processAction(m) { alert("Incident Lifecycle Action: " + m); closePanel(); }

    window.onload = () => {
        document.getElementById('welcomeModal').style.display = 'flex';
        updateFilterCounts();
    };
</script>
