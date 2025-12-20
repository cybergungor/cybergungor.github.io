---
layout: default
title: SentinelGuard - Hardened Incident Management
permalink: /alerts/
---

<style>
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    .soc-wrapper { display: flex; flex-direction: column; height: calc(100vh - 60px); color: #e6edf3; font-family: 'Inter', sans-serif; }

    /* --- TOAST & MENTOR --- */
    #toast-container { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); z-index: 11000; display: flex; flex-direction: column; align-items: center; gap: 8px; }
    #toast { background: #161b22; border: 1px solid #58a6ff; color: #fff; padding: 12px 25px; border-radius: 8px; font-family: 'JetBrains Mono'; font-size: 0.8rem; box-shadow: 0 10px 30px rgba(0,0,0,0.5); display: none; border-left: 4px solid #58a6ff; }
    #mentor-btn { background: #f85149; color: #fff; font-size: 0.65rem; padding: 5px 12px; border-radius: 20px; cursor: pointer; display: none; font-weight: bold; border: none; animation: pulse 1.5s infinite; }

    /* --- MODALS --- */
    .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(1, 4, 9, 0.98); z-index: 12000; display: none; align-items: center; justify-content: center; backdrop-filter: blur(10px); }
    .modal-box { background: #0d1117; border: 1px solid #30363d; width: 650px; border-radius: 12px; overflow: hidden; }
    .modal-header { background: #161b22; color: #58a6ff; padding: 20px; font-family: 'JetBrains Mono'; font-weight: bold; border-bottom: 1px solid #30363d; text-align: center; }
    .modal-body { padding: 30px; font-size: 0.9rem; color: #8b949e; line-height: 1.6; }

    /* --- QUEUE & TABLE --- */
    .soc-top-bar { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .btn-filter { background: #161b22; border: 1px solid #30363d; color: #8b949e; padding: 8px 18px; border-radius: 6px; font-size: 0.75rem; cursor: pointer; transition: 0.2s; }
    .btn-filter.active { background: #58a6ff; color: #0d1117; font-weight: bold; border-color: #58a6ff; }
    
    .incident-table { width: 100%; border-collapse: collapse; background: #0d1117; }
    .incident-table th { background: #161b22; text-align: left; padding: 15px; font-size: 0.7rem; color: #8b949e; text-transform: uppercase; }
    .incident-table td { padding: 15px; border-bottom: 1px solid #21262d; font-size: 0.85rem; }

    /* --- SIDE PANEL --- */
    .side-panel { position: fixed; top: 0; right: -650px; width: 550px; height: 100vh; background: #0d1117; border-left: 1px solid #30363d; transition: 0.4s cubic-bezier(0.05, 0.7, 0.1, 1); z-index: 9000; display: flex; flex-direction: column; }
    .panel-active { right: 0; box-shadow: -20px 0 80px rgba(0,0,0,0.8); }
    .card { background: #161b22; border: 1px solid #30363d; border-radius: 8px; padding: 15px; margin-bottom: 20px; }
    .artifact-input { width: 100%; background: #010409; border: 1px solid #30363d; color: #e6edf3; padding: 10px; font-size: 0.85rem; border-radius: 4px; margin-top: 5px; }

    .btn-footer { padding: 12px; border-radius: 4px; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; border: none; transition: 0.2s; }
    .badge { padding: 4px 8px; border-radius: 4px; font-size: 0.7rem; font-weight: bold; }
    .badge-tp { background: rgba(248, 81, 73, 0.1); color: #f85149; border: 1px solid #f85149; }
    .badge-fp { background: rgba(88, 166, 255, 0.1); color: #58a6ff; border: 1px solid #58a6ff; }
    .badge-esc { background: rgba(210, 153, 34, 0.1); color: #d29922; border: 1px solid #d29922; }

    @keyframes pulse { 0%, 100% { opacity: 0.6; } 50% { opacity: 1; } }
</style>

<div id="toast-container">
    <div id="toast"></div>
    <button id="mentor-btn" onclick="openMentor()">WHY AM I SEEING THIS ERROR?</button>
</div>

<div id="mentorModal" class="modal-overlay">
    <div class="modal-box">
        <div class="modal-header">ANALYST MENTOR // SYSTEM REASONING</div>
        <div class="modal-body" id="mentorBody"></div>
        <div style="padding:15px; text-align:right;"><button class="btn-filter" style="background:#58a6ff; color:#000" onclick="closeModal('mentorModal')">I UNDERSTAND</button></div>
    </div>
</div>

<div class="soc-wrapper">
    <div class="soc-top-bar">
        <div style="display:flex; gap:10px;">
            <button class="btn-filter active" id="tab-open-btn" onclick="switchTab('open')">ACTIVE QUEUE (<span id="count-open">0</span>)</button>
            <button class="btn-filter" id="tab-closed-btn" onclick="switchTab('closed')">CLOSED ARCHIVE (<span id="count-closed">0</span>)</button>
        </div>
        <div style="font-family:'JetBrains Mono'; font-size:0.7rem; color:#8b949e;">SENTINEL GUARD // MISSION STATUS: <span style="color:#3fb950">AUTHORIZED</span></div>
    </div>

    <div class="queue-container">
        <table class="incident-table">
            <thead>
                <tr><th width="110">Case ID</th><th>Technical Description</th><th width="100">Severity</th><th width="130">Verdict</th><th width="110">Action</th></tr>
            </thead>
            <tbody id="queueBody"></tbody>
        </table>
    </div>
</div>

<div id="sidePanel" class="side-panel">
    <div class="modal-header" style="display:flex; justify-content:space-between; align-items:center;">
        <span id="pID" style="color:#58a6ff;">#SOC-0000</span>
        <span onclick="closePanel()" style="cursor:pointer; font-size:24px;">×</span>
    </div>
    <div style="padding:25px; flex:1; overflow-y:auto;" id="pBody"></div>
    <div style="padding:20px; border-top:1px solid #30363d; display:grid; grid-template-columns:1fr 1fr; gap:10px;" id="pFooter"></div>
</div>

<script>
    const cases = {
        1: { id: "#SOC-5092", title: "Ransomware Pattern Detection", sev: "CRITICAL", status: "open", verdict: "Pending", q: [{l: "Process Name:", a: "tasksche.exe"}, {l: "Target Host IP:", a: "10.20.5.100"}] },
        2: { id: "#SOC-1022", title: "Privileged Account Bruteforce", sev: "HIGH", status: "open", verdict: "Pending", q: [{l: "Target User:", a: "adm_emir"}, {l: "Attacker IP:", a: "192.168.1.150"}] },
        3: { id: "#SOC-883", title: "SQLi Attempt: Web-Prod", sev: "HIGH", status: "open", verdict: "Pending", q: [{l: "Tool Agent:", a: "sqlmap/1.4.7"}, {l: "Target Schema:", a: "information_schema"}] },
        
        // ÖNCEDEN KAPALI VAKALAR
        101: { id: "#SOC-2201", title: "PS Remote Exfiltration", sev: "HIGH", status: "closed", verdict: "True Positive", report: "Confirmed data theft attempt via PowerShell Invoke-WebRequest." },
        102: { id: "#SOC-2188", title: "MFA Push Fatigue", sev: "MEDIUM", status: "closed", verdict: "True Positive", report: "User spammed with 50+ MFA requests. Password reset initiated." },
        103: { id: "#SOC-2015", title: "Internal Port Scan", sev: "LOW", status: "closed", verdict: "False Positive", report: "Authorized Nessus vulnerability scan confirmed by IT." }
    };

    const mentorDocs = {
        art: "<h3>Artifact Mismatch</h3><p>Girdiğiniz kanıtlar sistem loglarıyla eşleşmiyor. <strong>Logs</strong> sekmesine gidip doğru IP veya Process ismini bulmalısınız. 'asdasd' gibi girişler sistem tarafından reddedilir.</p>",
        auth: "<h3>Unauthorized Action</h3><p>Ransomware gibi kritik vakaları L1 analistler kapatamaz. Bu vakayı mutlaka <strong>Escalate</strong> butonuna basarak bir üst birime (L2) aktarmalısınız.</p>",
        logic: "<h3>Logic Conflict</h3><p>Saldırı olduğu kesinleşen bir vakaya 'False Positive' diyemezsiniz. Ayrıca zararsız bulduğunuz bir vakayı üst birime devrederek kaynak israfına yol açamazsınız.</p>"
    };

    let currentTab = 'open';
    let errorContext = "";

    function showToast(msg, err) {
        const t = document.getElementById('toast');
        const m = document.getElementById('mentor-btn');
        t.innerText = msg;
        t.style.display = 'block';
        if(err) { errorContext = err; m.style.display = 'block'; } else { m.style.display = 'none'; }
        setTimeout(() => { t.style.display = 'none'; }, 4000);
    }

    function openMentor() {
        document.getElementById('mentorBody').innerHTML = mentorDocs[errorContext];
        document.getElementById('mentorModal').style.display = 'flex';
    }

    function renderTable() {
        const body = document.getElementById('queueBody');
        body.innerHTML = ""; // Tabloyu temizle
        
        Object.keys(cases).forEach(key => {
            const c = cases[key];
            if(c.status === currentTab) {
                const vCls = c.verdict === 'True Positive' ? 'badge-tp' : (c.verdict === 'False Positive' ? 'badge-fp' : (c.verdict === 'Escalated' ? 'badge-esc' : ''));
                const row = `
                    <tr class="alert-row" onclick="openCase(${key})">
                        <td style="font-family:'JetBrains Mono'; color:#58a6ff;">${c.id}</td>
                        <td><strong>${c.title}</strong></td>
                        <td><span style="color:${c.sev==='CRITICAL'?'#f85149':c.sev==='HIGH'?'#d29922':'#8b949e'}">${c.sev}</span></td>
                        <td><span class="badge ${vCls}">${c.verdict}</span></td>
                        <td><button class="btn-filter" style="width:100%">${c.status==='open'?'ANALYZE':'VIEW'}</button></td>
                    </tr>`;
                body.innerHTML += row;
            }
        });
        updateStats();
    }

    function openCase(k) {
        const c = cases[k];
        const b = document.getElementById('pBody');
        const f = document.getElementById('pFooter');
        document.getElementById('pID').innerText = c.id;
        
        if(c.status === 'open') {
            b.innerHTML = `
                <div class="card"><h4>Forensic Verification</h4>${c.q.map((q,i)=>`<label style="font-size:0.7rem; color:#8b949e;">${q.l}</label><input class="artifact-input" id="ans-${i}" placeholder="Enter value...">`).join('')}</div>
                <div class="card"><h4>Verdict Decision</h4><select class="artifact-input" id="vSel"><option value="True Positive">True Positive</option><option value="False Positive">False Positive</option></select><textarea class="artifact-input" id="aNotes" placeholder="Summary of findings..." style="height:80px;"></textarea></div>`;
            f.innerHTML = `<button class="btn-footer" style="background:#d29922; color:#000" onclick="doEsc(${k})">ESCALATE L2</button><button class="btn-footer" style="background:#238636; color:white;" onclick="doRes(${k})">RESOLVE</button>`;
        } else {
            b.innerHTML = `<div class="card"><h4>Closure Report</h4><p>Verdict: <span class="badge ${c.verdict==='True Positive'?'badge-tp':(c.verdict==='False Positive'?'badge-fp':'badge-esc')}">${c.verdict}</span></p><hr style="border:0; border-top:1px solid #30363d; margin:15px 0;"><p style="font-size:0.85rem;">${c.report || 'Archived incident record.'}</p></div>`;
            f.innerHTML = `<button class="btn-footer" style="background:#30363d; color:white; grid-column:span 2;" onclick="closePanel()">CLOSE</button>`;
        }
        document.getElementById('sidePanel').classList.add('panel-active');
    }

    function checkArtifacts(k) {
        let valid = true;
        cases[k].q.forEach((q,i) => {
            const input = document.getElementById(`ans-${i}`);
            if(input.value.trim().toLowerCase() !== q.a.toLowerCase()) {
                valid = false;
                input.style.borderColor = "#f85149";
            } else {
                input.style.borderColor = "#238636";
            }
        });
        return valid;
    }

    function doRes(k) {
        if(!checkArtifacts(k)) { showToast("ARTIFACT MISMATCH!", "art"); return; }
        if(k === 1) { showToast("AUTHORITY DENIED: Critical alerts must be escalated.", "auth"); return; }
        const v = document.getElementById('vSel').value;
        if(v === "False Positive") { showToast("DECISION ERROR: Attack pattern confirmed.", "logic"); return; }

        cases[k].status = 'closed';
        cases[k].verdict = v;
        cases[k].report = document.getElementById('aNotes').value || "Resolved via IR playbook.";
        showToast("Case Resolved."); renderTable(); closePanel();
    }

    function doEsc(k) {
        // --- SIKI DOĞRULAMA: BURADA RETURN EKLENDİ ---
        if(!checkArtifacts(k)) { 
            showToast("ESC FAILED: Invalid artifacts provided.", "art"); 
            return; // İşlemi durdur
        }

        const v = document.getElementById('vSel').value;
        if(v === "False Positive") { 
            showToast("LOGIC ERROR: Benign cases cannot be escalated.", "logic"); 
            return; // İşlemi durdur
        }

        cases[k].status = 'closed';
        cases[key = k].verdict = "Escalated";
        cases[k].report = "Tier-1 analyst identified risk. Escalated for Tier-2 forensic review. Verdict: " + v;
        showToast("Incident Escalated."); renderTable(); closePanel();
    }

    function switchTab(t) {
        currentTab = t;
        document.getElementById('tab-open-btn').classList.toggle('active', t === 'open');
        document.getElementById('tab-closed-btn').classList.toggle('active', t === 'closed');
        renderTable();
    }

    function updateStats() {
        document.getElementById('count-open').innerText = Object.values(cases).filter(c => c.status === 'open').length;
        document.getElementById('count-closed').innerText = Object.values(cases).filter(c => c.status === 'closed').length;
    }

    function closePanel() { document.getElementById('sidePanel').classList.remove('panel-active'); }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }
    window.onload = () => { renderTable(); };
</script>
