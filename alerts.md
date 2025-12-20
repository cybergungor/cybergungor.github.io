---
layout: default
title: SentinelGuard - Advanced Incident Management
permalink: /alerts/
---

<style>
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    .soc-wrapper { display: flex; flex-direction: column; height: calc(100vh - 60px); color: #e6edf3; font-family: 'Inter', sans-serif; }

    /* --- TOAST NOTIFICATION --- */
    #toast {
        position: fixed; top: 20px; left: 50%; transform: translateX(-50%);
        background: #161b22; border: 1px solid #58a6ff; color: #fff;
        padding: 12px 25px; border-radius: 8px; font-family: 'JetBrains Mono';
        font-size: 0.85rem; z-index: 11000; box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        display: none; animation: fadeInDown 0.4s ease-out;
    }

    /* --- INITIAL DISCLAIMER --- */
    .disclaimer-modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(1, 4, 9, 0.96); z-index: 10000; display: none; align-items: center; justify-content: center; backdrop-filter: blur(15px); }
    .disclaimer-box { background: #0d1117; border: 1px solid #30363d; width: 750px; border-radius: 12px; overflow: hidden; animation: slideUp 0.4s ease-out; }
    .disclaimer-header { background: #161b22; color: #58a6ff; padding: 20px; font-family: 'JetBrains Mono'; font-weight: bold; text-align: center; border-bottom: 1px solid #30363d; }
    .btn-ack { width: 100%; padding: 18px; background: #238636; color: #fff; border: none; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; transition: 0.2s; }

    /* --- FILTERS --- */
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

    .badge { padding: 4px 10px; border-radius: 4px; font-size: 0.7rem; font-weight: bold; text-transform: uppercase; }
    .badge-tp { background: rgba(248, 81, 73, 0.1); color: #f85149; border: 1px solid #f85149; }
    .badge-fp { background: rgba(88, 166, 255, 0.1); color: #58a6ff; border: 1px solid #58a6ff; }
    .badge-esc { background: rgba(210, 153, 34, 0.1); color: #d29922; border: 1px solid #d29922; }

    .panel-footer { padding: 25px; background: #0d1117; border-top: 1px solid #30363d; display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
    .btn-footer { padding: 14px; border-radius: 6px; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; font-size: 0.8rem; border: none; transition: 0.2s; }
    .btn-res { background: #238636; color: white; }
    .btn-esc { background: #d29922; color: #0d1117; }

    @keyframes fadeInDown { from { opacity: 0; top: 0; } to { opacity: 1; top: 20px; } }
    @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
</style>

<div id="toast"></div>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">SENTINEL GUARD // COMMAND & CONTROL</div>
        <div class="disclaimer-body">
            <div style="flex:1;">
                <h3>[TR] VAKA MÜDAHALE</h3>
                <p>Olay yaşam döngüsünü yönetin. Kritik vakalarda L2 yükseltme zorunludur. Yanlış kararlar (TP/FP hataları) sistem tarafından denetlenir.</p>
            </div>
            <div style="width:1px; background:#30363d; margin:0 20px;"></div>
            <div style="flex:1;">
                <h3>[EN] INCIDENT RESPONSE</h3>
                <p>Manage the incident lifecycle. L2 escalation is mandatory for critical cases. Verdict errors (TP/FP) are monitored by the system.</p>
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
        1: { id: "#SOC-5092", title: "Ransomware Encryption Pattern", sev: "CRITICAL", status: "open", verdict: "Pending", root_cause: "High entropy file modifications detected on Finance-SRV. Critical infrastructure at risk.", q: [{l: "Process Name:", a: "tasksche.exe"}, {l: "Target IP:", a: "10.20.5.100"}], report: "" },
        2: { id: "#SOC-1022", title: "Domain Admin Bruteforce", sev: "HIGH", status: "open", verdict: "Pending", root_cause: "Failed login spike (4625) against 'adm_emir' from non-standard internal IP.", q: [{l: "Target User:", a: "adm_emir"}, {l: "Source IP:", a: "192.168.1.150"}], report: "" },
        3: { id: "#SOC-883", title: "SQLi Attempt: Web Prod", sev: "HIGH", status: "open", verdict: "Pending", root_cause: "UNION SELECT pattern matched in URI. Target: information_schema.", q: [{l: "Tool Agent:", a: "sqlmap/1.4.7"}, {l: "Target Schema:", a: "information_schema"}], report: "" },
        
        // PRE-FILLED ARCHIVE
        101: { id: "#SOC-2201", title: "PowerShell Exfiltration", sev: "HIGH", status: "closed", verdict: "True Positive", report: "Confirmed Red Team exercise. Analyst isolated workstation.", root_cause: "IEX download from 45.33.2.11.", q:[] },
        102: { id: "#SOC-2188", title: "MFA Push Fatigue", sev: "MEDIUM", status: "closed", verdict: "True Positive", report: "Account compromised. Credentials reset.", root_cause: "MFA spamming detected.", q:[] },
        103: { id: "#SOC-2150", title: "ICMP Tunneling", sev: "HIGH", status: "closed", verdict: "True Positive", report: "Tunneling confirmed via deep packet inspection.", root_cause: "Non-standard ICMP data.", q:[] },
        104: { id: "#SOC-2090", title: "Vulnerability Scan", sev: "LOW", status: "closed", verdict: "False Positive", report: "Confirmed scheduled Nessus scan.", root_cause: "Internal security scanner activity.", q:[] },
        105: { id: "#SOC-2012", title: "Late Admin Login", sev: "LOW", status: "closed", verdict: "False Positive", report: "Verified authorized emergency maintenance.", root_cause: "Admin VPN login at 3 AM.", q:[] }
    };

    let activeTab = 'open';

    function showToast(msg, color="#58a6ff") {
        const t = document.getElementById('toast');
        t.innerText = msg;
        t.style.borderColor = color;
        t.style.display = 'block';
        setTimeout(() => { t.style.display = 'none'; }, 3000);
    }

    function renderTable() {
        const body = document.getElementById('queueBody');
        body.innerHTML = "";
        Object.entries(cases).forEach(([key, c]) => {
            if(c.status !== activeTab) return;
            let vClass = "";
            if(c.verdict === 'True Positive') vClass = 'badge-tp';
            else if(c.verdict === 'False Positive') vClass = 'badge-fp';
            else if(c.verdict === 'Escalated') vClass = 'badge-esc';

            body.innerHTML += `
                <tr class="alert-row" onclick="openIncident(${key})">
                    <td style="font-family:'JetBrains Mono'; color:#58a6ff;">${c.id}</td>
                    <td><strong>${c.title}</strong></td>
                    <td><span style="color:${c.sev==='CRITICAL'?'#f85149':c.sev==='HIGH'?'#d29922':'#8b949e'}">${c.sev}</span></td>
                    <td><span class="badge ${vClass}">${c.verdict}</span></td>
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
                    <h4>Threat Intel / Root Cause</h4>
                    <div class="threat-info">${c.root_cause}</div>
                </div>
                <div class="card">
                    <h4>Step 1: Artifact Collection</h4>
                    ${c.q.map((q, i) => `<label style="font-size:0.7rem; color:#8b949e; display:block; margin-bottom:5px;">${q.l}</label><input type="text" class="artifact-input" id="ans-${i}" autocomplete="off">`).join('')}
                </div>
                <div class="card">
                    <h4>Step 2: Analyst Verdict</h4>
                    <select class="artifact-input" id="verdictSelect">
                        <option value="True Positive">True Positive (Confirmed Malicious)</option>
                        <option value="False Positive">False Positive (Benign Activity)</option>
                    </select>
                    <textarea class="artifact-input" id="analystNotes" style="height:100px; resize:none;" placeholder="Official Investigation Summary..."></textarea>
                </div>
            `;
            footer.innerHTML = `
                <button class="btn-footer btn-esc" onclick="escalateL2(${key})">ESCALATE L2</button>
                <button class="btn-footer btn-res" onclick="resolveIncident(${key})">RESOLVE INCIDENT</button>
            `;
        } else {
            document.getElementById('headerMode').innerText = "ARCHIVED CASE REPORT";
            body.innerHTML = `
                <div class="card" style="border-top: 4px solid ${c.verdict==='True Positive'?'#f85149':'#58a6ff'}">
                    <h3 style="margin-top:0;">Closure Report</h3>
                    <p style="font-size:0.85rem;">Verdict: <span class="badge ${c.verdict==='True Positive'?'badge-tp':(c.verdict==='Escalated'?'badge-esc':'badge-fp')}">${c.verdict}</span></p>
                    <hr style="border:0; border-top:1px solid #30363d; margin:15px 0;">
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
        const selectedVerdict = document.getElementById('verdictSelect').value;
        let allCorrect = true;

        // 1. Artifact Check
        c.q.forEach((q, i) => {
            if(document.getElementById(`ans-${i}`).value.trim().toLowerCase() !== q.a.toLowerCase()) allCorrect = false;
        });

        if(!allCorrect) {
            showToast("VERIFICATION FAILED: Artifacts do not match telemetry.", "#f85149");
            return;
        }

        // 2. Logic Check: Critical Ransomware must be Escalated
        if(key === 1) {
            showToast("AUTHORITY DENIED: Critical Ransomware must be ESCALATED to Tier-2!", "#d29922");
            return;
        }

        // 3. Logic Check: Critical Ransomware cannot be False Positive
        if(key === 1 && selectedVerdict === "False Positive") {
            showToast("DECISION ERROR: Cannot flag critical pattern as False Positive!", "#f85149");
            return;
        }

        cases[key].status = 'closed';
        cases[key].verdict = selectedVerdict;
        cases[key].report = document.getElementById('analystNotes').value || "Resolved via playbook protocols.";
        
        showToast("SUCCESS: Case resolved and archived.", "#3fb950");
        renderTable();
        closePanel();
    }

    function escalateL2(key) {
        showToast("ACTION: Case " + cases[key].id + " successfully escalated to Tier-2.", "#3fb950");
        cases[key].status = 'closed';
        cases[key].verdict = "Escalated";
        cases[key].report = "L1 analyst identified high-risk indicators. Escalated to Tier-2 for advanced forensic investigation.";
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
        document.getElementById('count-open').innerText = Object.values(cases).filter(c => c.status === 'open').length;
        document.getElementById('count-closed').innerText = Object.values(cases).filter(c => c.status === 'closed').length;
    }

    function closePanel() { document.getElementById('sidePanel').classList.remove('panel-active'); }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }

    window.onload = () => {
        document.getElementById('welcomeModal').style.display = 'flex';
        renderTable();
    };
</script>
