---
layout: default
title: SentinelGuard - Advanced SOC Operations
permalink: /alerts/
---

<style>
    /* Full Page Layout */
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    .soc-wrapper { display: flex; flex-direction: column; height: calc(100vh - 60px); color: #e6edf3; font-family: 'Inter', sans-serif; }

    /* Top Bar */
    .soc-top-bar { background: #0d1117; padding: 10px 25px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .soc-brand { font-family: 'JetBrains Mono', monospace; font-size: 0.9rem; color: #58a6ff; font-weight: bold; }
    .soc-stats { display: flex; gap: 20px; font-size: 0.75rem; }
    .stat-box { border-left: 1px solid #30363d; padding-left: 15px; }
    .stat-val { color: #3fb950; font-weight: bold; }

    /* Main Table Area */
    .queue-container { flex: 1; padding: 25px; overflow-y: auto; }
    .incident-table { width: 100%; border-collapse: collapse; background: #0d1117; border: 1px solid #30363d; border-radius: 6px; overflow: hidden; }
    .incident-table th { background: #161b22; text-align: left; padding: 12px 15px; font-size: 0.7rem; color: #8b949e; text-transform: uppercase; letter-spacing: 1px; }
    .incident-table td { padding: 15px; border-bottom: 1px solid #21262d; font-size: 0.85rem; }
    .incident-table tr:hover { background: #0d1117; filter: brightness(1.2); cursor: pointer; }

    /* Severity & Status */
    .sev { padding: 3px 8px; border-radius: 3px; font-size: 0.65rem; font-weight: bold; }
    .sev-critical { color: #f85149; border: 1px solid rgba(248, 81, 73, 0.4); background: rgba(248, 81, 73, 0.1); }
    .sev-high { color: #d29922; border: 1px solid rgba(210, 153, 34, 0.4); background: rgba(210, 153, 34, 0.1); }
    .status-open { color: #f85149; font-family: 'JetBrains Mono'; font-size: 0.75rem; }
    .status-resolved { color: #3fb950; font-family: 'JetBrains Mono'; font-size: 0.75rem; }

    /* RIGHT SIDE PANEL (INVESTIGATION CANVAS) */
    .investigation-panel {
        position: fixed; top: 60px; right: -500px; width: 450px; height: calc(100vh - 60px);
        background: #0d1117; border-left: 1px solid #30363d; box-shadow: -10px 0 30px rgba(0,0,0,0.5);
        transition: right 0.4s cubic-bezier(0.05, 0.7, 0.1, 1); z-index: 1000;
        display: flex; flex-direction: column;
    }
    .panel-active { right: 0; }
    .panel-header { padding: 20px; background: #161b22; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; }
    .panel-body { padding: 20px; flex: 1; overflow-y: auto; }
    
    .playbook-card { background: #161b22; border: 1px solid #30363d; border-radius: 6px; padding: 15px; margin-bottom: 20px; }
    .playbook-card h4 { margin: 0 0 10px 0; font-size: 0.75rem; color: #58a6ff; text-transform: uppercase; }
    
    .artifact-input {
        width: 100%; background: #010409; border: 1px solid #30363d; color: #e6edf3;
        padding: 10px; font-family: 'JetBrains Mono', monospace; font-size: 0.8rem; border-radius: 4px; margin-top: 5px;
    }
    .artifact-input:focus { border-color: #58a6ff; outline: none; }

    .btn-group { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; padding: 20px; border-top: 1px solid #30363d; background: #0d1117; }
    .btn-panel { padding: 12px; border-radius: 4px; font-weight: bold; cursor: pointer; font-size: 0.75rem; font-family: 'JetBrains Mono'; border: none; }
    .btn-escalate { background: #d29922; color: #fff; }
    .btn-resolve { background: #238636; color: #fff; }
    .btn-close { background: transparent; color: #8b949e; border: 1px solid #30363d; }

    /* Timeline in Panel */
    .mini-timeline { border-left: 2px solid #30363d; padding-left: 15px; margin-left: 10px; }
    .timeline-item { position: relative; margin-bottom: 15px; font-size: 0.75rem; }
    .timeline-item::before { content: ''; position: absolute; left: -21px; top: 4px; width: 10px; height: 10px; background: #30363d; border-radius: 50%; }
    .active-step::before { background: #58a6ff; box-shadow: 0 0 8px #58a6ff; }
</style>

<div class="soc-wrapper">
    <div class="soc-top-bar">
        <div class="soc-brand">SENTINEL GUARD // INCIDENT RESPONSE TERMINAL</div>
        <div class="soc-stats">
            <div class="stat-box">ACTIVE ALERTS: <span class="stat-val" id="activeCount">3</span></div>
            <div class="stat-box">MTTR: <span class="stat-val">14m 22s</span></div>
            <div class="stat-box">ANALYST: <span style="color:#58a6ff">E_GUNGOROGLU</span></div>
        </div>
    </div>

    <div class="queue-container">
        <table class="incident-table">
            <thead>
                <tr>
                    <th width="120">Alert ID</th>
                    <th>Tactics & Techniques (MITRE)</th>
                    <th width="150">Severity</th>
                    <th width="150">Status</th>
                    <th width="100">Details</th>
                </tr>
            </thead>
            <tbody id="alertQueue">
                <tr onclick="inspectCase(1)">
                    <td class="mono">#SOC-5092</td>
                    <td><strong>Ransomware Activity</strong> <span style="color:#8b949e; font-size:0.7rem; margin-left:10px;">T1486 // Data Encrypted</span></td>
                    <td><span class="sev sev-critical">CRITICAL</span></td>
                    <td><span class="status-open" id="stat-1">● OPEN</span></td>
                    <td><button class="btn-close" style="padding:4px 8px">VIEW</button></td>
                </tr>
                <tr onclick="inspectCase(2)">
                    <td class="mono">#SOC-1022</td>
                    <td><strong>Brute Force Attack</strong> <span style="color:#8b949e; font-size:0.7rem; margin-left:10px;">T1110 // Credential Access</span></td>
                    <td><span class="sev sev-high">HIGH</span></td>
                    <td><span class="status-open" id="stat-2">● OPEN</span></td>
                    <td><button class="btn-close" style="padding:4px 8px">VIEW</button></td>
                </tr>
                <tr onclick="inspectCase(3)">
                    <td class="mono">#SOC-883</td>
                    <td><strong>SQL Injection Attempt</strong> <span style="color:#8b949e; font-size:0.7rem; margin-left:10px;">T1190 // Exploit Public App</span></td>
                    <td><span class="sev sev-high">HIGH</span></td>
                    <td><span class="status-open" id="stat-3">● OPEN</span></td>
                    <td><button class="btn-close" style="padding:4px 8px">VIEW</button></td>
                </tr>
            </tbody>
        </table>
    </div>
</div>

<div id="sidePanel" class="investigation-panel">
    <div class="panel-header">
        <div style="font-family:'JetBrains Mono'; font-size:0.8rem; font-weight:bold;">
            <span style="color:#8b949e">INSPECTING:</span> <span id="panelId">#SOC-0000</span>
        </div>
        <span class="close-btn" style="cursor:pointer" onclick="closePanel()">×</span>
    </div>

    <div class="panel-body">
        <div class="playbook-card">
            <h4>1. Alert Verification</h4>
            <div class="mini-timeline">
                <div class="timeline-item active-step">Validate logs in SIEM (/logs)</div>
                <div class="timeline-item">Extract forensic artifacts</div>
            </div>
        </div>

        <div class="playbook-card">
            <h4>2. Artifact Collection</h4>
            <div id="questionSet">
                </div>
        </div>

        <div class="playbook-card">
            <h4>3. Incident Disposition</h4>
            <label style="font-size:0.7rem; color:#8b949e;">ANALYST NOTES</label>
            <textarea class="artifact-input" style="height:80px; resize:none;" placeholder="Summary of findings for Tier-2..."></textarea>
        </div>
    </div>

    <div class="btn-group">
        <button class="btn-panel btn-escalate" onclick="processAction('Escalated to L2')">ESCALATE TO L2</button>
        <button class="btn-panel btn-resolve" onclick="validateAndResolve()">RESOLVE CASE</button>
        <button class="btn-panel btn-close" style="grid-column: span 2;" onclick="closePanel()">SAVE & EXIT</button>
    </div>
</div>

<script>
    const caseRepo = {
        1: { id: "#SOC-5092", q: [{l: "Malicious Process:", a: "tasksche.exe"}, {l: "Target Host IP:", a: "10.20.5.100"}] },
        2: { id: "#SOC-1022", q: [{l: "Target Account:", a: "adm_emir"}, {l: "Attacker IP:", a: "192.168.1.150"}] },
        3: { id: "#SOC-883", q: [{l: "Attacker User-Agent:", a: "sqlmap/1.4.7"}, {l: "Target DB Schema:", a: "information_schema"}] }
    };

    let activeId = null;

    function inspectCase(id) {
        activeId = id;
        const data = caseRepo[id];
        document.getElementById('panelId').innerText = data.id;
        const qSet = document.getElementById('questionSet');
        qSet.innerHTML = data.q.map((q, i) => `
            <div style="margin-bottom:10px;">
                <label style="font-size:0.7rem; color:#8b949e;">${q.l}</label>
                <input type="text" class="artifact-input" id="ans-${i}" autocomplete="off">
            </div>
        `).join('');
        document.getElementById('sidePanel').classList.add('panel-active');
    }

    function closePanel() { document.getElementById('sidePanel').classList.remove('panel-active'); }

    function validateAndResolve() {
        const data = caseRepo[activeId];
        let valid = true;
        data.q.forEach((q, i) => {
            if(document.getElementById(`ans-${i}`).value.trim().toLowerCase() !== q.a.toLowerCase()) {
                valid = false;
                document.getElementById(`ans-${i}`).style.borderColor = "#f85149";
            } else {
                document.getElementById(`ans-${i}`).style.borderColor = "#238636";
            }
        });

        if(valid) {
            alert("THREAT MITIGATED: Case has been successfully resolved.");
            document.getElementById(`stat-${activeId}`).innerText = "✔ RESOLVED";
            document.getElementById(`stat-${activeId}`).className = "status-resolved";
            closePanel();
        } else {
            alert("VALIDATION ERROR: Forensic artifacts do not match the logs.");
        }
    }

    function processAction(msg) {
        alert("SIMULATION: " + msg);
        closePanel();
    }
</script>
