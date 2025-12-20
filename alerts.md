---
layout: default
title: SentinelGuard - Pro Incident Response
permalink: /alerts/
---

<style>
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    .soc-wrapper { display: flex; flex-direction: column; height: calc(100vh - 60px); color: #e6edf3; font-family: 'Inter', sans-serif; }

    /* --- TOAST & MENTOR --- */
    #toast-container { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); z-index: 11000; display: flex; flex-direction: column; align-items: center; gap: 8px; }
    #toast { background: #161b22; border: 1px solid #58a6ff; color: #fff; padding: 12px 25px; border-radius: 8px; font-family: 'JetBrains Mono'; font-size: 0.8rem; box-shadow: 0 10px 30px rgba(0,0,0,0.5); display: none; }
    #mentor-btn { background: #f85149; color: #fff; font-size: 0.65rem; padding: 5px 12px; border-radius: 20px; cursor: pointer; display: none; font-weight: bold; border: none; animation: pulse 1.5s infinite; }

    /* --- MODALS --- */
    .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(1, 4, 9, 0.98); z-index: 12000; display: none; align-items: center; justify-content: center; backdrop-filter: blur(10px); }
    .modal-box { background: #0d1117; border: 1px solid #30363d; width: 650px; border-radius: 12px; overflow: hidden; }
    .modal-header { background: #161b22; color: #58a6ff; padding: 20px; font-family: 'JetBrains Mono'; font-weight: bold; border-bottom: 1px solid #30363d; text-align: center; }
    .modal-body { padding: 30px; font-size: 0.9rem; color: #8b949e; line-height: 1.6; }

    /* --- QUEUE & TABLE --- */
    .soc-top-bar { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .btn-filter { background: #161b22; border: 1px solid #30363d; color: #8b949e; padding: 8px 18px; border-radius: 6px; font-size: 0.75rem; cursor: pointer; transition: 0.2s; }
    .btn-filter.active { background: #58a6ff; color: #0d1117; font-weight: bold; }
    
    .incident-table { width: 100%; border-collapse: collapse; background: #0d1117; }
    .incident-table th { background: #161b22; text-align: left; padding: 15px; font-size: 0.7rem; color: #8b949e; text-transform: uppercase; }
    .incident-table td { padding: 15px; border-bottom: 1px solid #21262d; font-size: 0.85rem; }

    /* --- SIDE PANEL --- */
    .side-panel { position: fixed; top: 0; right: -650px; width: 550px; height: 100vh; background: #0d1117; border-left: 1px solid #30363d; transition: 0.4s cubic-bezier(0.05, 0.7, 0.1, 1); z-index: 9000; display: flex; flex-direction: column; }
    .panel-active { right: 0; box-shadow: -20px 0 80px rgba(0,0,0,0.8); }
    .card { background: #161b22; border: 1px solid #30363d; border-radius: 8px; padding: 15px; margin-bottom: 20px; }
    .artifact-input { width: 100%; background: #010409; border: 1px solid #30363d; color: #e6edf3; padding: 10px; font-size: 0.85rem; border-radius: 4px; margin-top: 5px; }

    .btn-footer { padding: 12px; border-radius: 4px; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; border: none; }
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
        <div class="modal-header">ANALYST MENTOR // ROOT CAUSE</div>
        <div class="modal-body" id="mentorBody"></div>
        <div style="padding:15px; text-align:right;"><button class="btn-filter" onclick="closeModal('mentorModal')">I UNDERSTAND</button></div>
    </div>
</div>

<div class="soc-wrapper">
    <div class="soc-top-bar">
        <div style="display:flex; gap:10px;">
            <button class="btn-filter active" id="tab-open" onclick="switchTab('open')">OPEN (<span id="count-open">3</span>)</button>
            <button class="btn-filter" id="tab-closed" onclick="switchTab('closed')">CLOSED (<span id="count-closed">5</span>)</button>
        </div>
        <div style="font-family:'JetBrains Mono'; font-size:0.7rem; color:#8b949e;">ANALYST: L1_EMIR</div>
    </div>

    <div class="queue-container">
        <table class="incident-table">
            <thead>
                <tr><th width="100">Case ID</th><th>Description</th><th width="100">Severity</th><th width="130">Verdict</th><th width="100">Action</th></tr>
            </thead>
            <tbody id="queueBody"></tbody>
        </table>
    </div>
</div>

<div id="sidePanel" class="side-panel">
    <div class="modal-header" style="display:flex; justify-content:space-between; align-items:center;">
        <span id="pID">#SOC-0000</span>
        <span onclick="closePanel()" style="cursor:pointer">×</span>
    </div>
    <div style="padding:20px; flex:1; overflow-y:auto;" id="pBody"></div>
    <div style="padding:20px; border-top:1px solid #30363d; display:grid; grid-template-columns:1fr 1fr; gap:10px;" id="pFooter"></div>
</div>

<script>
    const cases = {
        1: { id: "#SOC-5092", title: "Ransomware Pattern", sev: "CRITICAL", status: "open", verdict: "Pending", q: [{l: "Process:", a: "tasksche.exe"}, {l: "Target IP:", a: "10.20.5.100"}] },
        2: { id: "#SOC-1022", title: "Admin Brute Force", sev: "HIGH", status: "open", verdict: "Pending", q: [{l: "User:", a: "adm_emir"}, {l: "Attacker IP:", a: "192.168.1.150"}] },
        3: { id: "#SOC-883", title: "SQLi Exploit Attempt", sev: "HIGH", status: "open", verdict: "Pending", q: [{l: "Agent:", a: "sqlmap/1.4.7"}, {l: "Schema:", a: "information_schema"}] },
        
        101: { id: "#SOC-2201", title: "PS Exfiltration", sev: "HIGH", status: "closed", verdict: "True Positive", report: "Red Team exercise confirmed." },
        102: { id: "#SOC-2188", title: "MFA Fatigue", sev: "MEDIUM", status: "closed", verdict: "True Positive", report: "Spraying attack mitigated." }
    };

    const mentorData = {
        art: "<h3>Validation Failed</h3><p>Kanıtlar (artifacts) loglardaki verilerle uyuşmuyor. Lütfen /logs sayfasındaki tam değerleri girdiğinizden emin olun.</p>",
        auth: "<h3>Authority Error</h3><p>Ransomware gibi kritik vakalar L1 tarafından kapatılamaz, mutlaka L2'ye (Escalate) gönderilmelidir.</p>",
        logic: "<h3>Logic Conflict</h3><p>'False Positive' olarak işaretlediğiniz bir vakayı üst birime (Escalate) gönderemezsiniz.</p>"
    };

    let activeTab = 'open';
    let lastErr = "";

    function showToast(m, err) {
        const t = document.getElementById('toast');
        t.innerText = m; t.style.display = 'block';
        if(err) { lastErr = err; document.getElementById('mentor-btn').style.display = 'block'; }
        setTimeout(() => { t.style.display = 'none'; }, 4000);
    }

    function openMentor() {
        document.getElementById('mentorBody').innerHTML = mentorData[lastErr];
        document.getElementById('mentorModal').style.display = 'flex';
    }

    function renderTable() {
        const b = document.getElementById('queueBody'); b.innerHTML = "";
        Object.entries(cases).forEach(([k, c]) => {
            if(c.status !== activeTab) return;
            let vCls = c.verdict === 'True Positive' ? 'badge-tp' : (c.verdict === 'False Positive' ? 'badge-fp' : (c.verdict === 'Escalated' ? 'badge-esc' : ''));
            b.innerHTML += `<tr class="alert-row" onclick="openCase(${k})"><td>${c.id}</td><td><strong>${c.title}</strong></td><td>${c.sev}</td><td><span class="badge ${vCls}">${c.verdict}</span></td><td><button class="btn-filter" style="width:100%">${c.status==='open'?'ANALYZE':'VIEW'}</button></td></tr>`;
        });
        updateCounts();
    }

    function openCase(k) {
        const c = cases[k]; document.getElementById('pID').innerText = c.id;
        const b = document.getElementById('pBody'); const f = document.getElementById('pFooter');
        
        if(c.status === 'open') {
            b.innerHTML = `<div class="card"><h4>Verification</h4>${c.q.map((q,i)=>`<label style="font-size:0.7rem;">${q.l}</label><input class="artifact-input" id="ans-${i}" placeholder="Value from logs...">`).join('')}</div><div class="card"><h4>Verdict</h4><select class="artifact-input" id="vSel"><option value="True Positive">True Positive</option><option value="False Positive">False Positive</option></select><textarea class="artifact-input" id="aNotes" placeholder="Analyst notes..." style="height:80px;"></textarea></div>`;
            f.innerHTML = `<button class="btn-footer" style="background:#d29922" onclick="doEsc(${k})">ESCALATE L2</button><button class="btn-footer" style="background:#238636; color:white;" onclick="doRes(${k})">RESOLVE</button>`;
        } else {
            b.innerHTML = `<div class="card"><h4>Closure Report</h4><p>Verdict: ${c.verdict}</p><hr style="border:0; border-top:1px solid #30363d; margin:10px 0;"><p>${c.report || 'No data.'}</p></div>`;
            f.innerHTML = `<button class="btn-footer" style="background:#30363d; color:white; grid-column:span 2;" onclick="closePanel()">CLOSE</button>`;
        }
        document.getElementById('sidePanel').classList.add('panel-active');
    }

    function validate(k) {
        let ok = true;
        cases[k].q.forEach((q,i) => { if(document.getElementById(`ans-${i}`).value.trim().toLowerCase() !== q.a.toLowerCase()) ok = false; });
        return ok;
    }

    function doRes(k) {
        if(!validate(k)) { showToast("ARTIFACT MISMATCH!", "art"); return; }
        if(k === 1) { showToast("CRITICAL ALERT: MUST ESCALATE!", "auth"); return; }
        if(document.getElementById('vSel').value === "False Positive") { showToast("VERDICT ERROR: Pattern is Malicious.", "logic"); return; }

        cases[k].status = 'closed'; cases[k].verdict = document.getElementById('vSel').value;
        cases[k].report = document.getElementById('aNotes').value;
        showToast("Case Resolved."); renderTable(); closePanel();
    }

    function doEsc(k) {
        // ESKALE ETSE BİLE ARTIFACT DOĞRU OLMALI (ASDASD YAZIP GEÇEMEZ)
        if(!validate(k)) { showToast("ARTIFACT MISMATCH!", "art"); return; }
        if(document.getElementById('vSel').value === "False Positive") { showToast("LOGIC ERROR: Benign cases cannot be escalated.", "logic"); return; }

        cases[k].status = 'closed'; cases[k].verdict = "Escalated";
        cases[k].report = "Escalated for Tier-2 forensic review.";
        showToast("Escalated to L2."); renderTable(); closePanel();
    }

    function switchTab(t) { activeTab = t; document.getElementById('tab-open').classList.toggle('active', t==='open'); document.getElementById('tab-closed').classList.toggle('active', t==='closed'); renderTable(); }
    function updateCounts() { document.getElementById('count-open').innerText = Object.values(cases).filter(c=>c.status==='open').length; document.getElementById('count-closed').innerText = Object.values(cases).filter(c=>c.status==='closed').length; }
    function closePanel() { document.getElementById('sidePanel').classList.remove('panel-active'); }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }
    window.onload = () => { renderTable(); };
</script>
