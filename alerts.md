---
layout: default
title: SentinelGuard - Pro Incident Management
permalink: /alerts/
---

<style>
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    .soc-wrapper { display: flex; flex-direction: column; height: calc(100vh - 60px); color: #e6edf3; font-family: 'Inter', sans-serif; }

    /* --- INITIAL DISCLAIMER MODAL --- */
    .disclaimer-modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(1, 4, 9, 0.96); z-index: 10000; display: none; align-items: center; justify-content: center; backdrop-filter: blur(15px); }
    .disclaimer-box { background: #0d1117; border: 1px solid #30363d; width: 750px; border-radius: 12px; overflow: hidden; animation: slideUp 0.4s ease-out; }
    .disclaimer-header { background: #161b22; color: #58a6ff; padding: 20px; font-family: 'JetBrains Mono'; font-weight: bold; text-align: center; border-bottom: 1px solid #30363d; }
    .disclaimer-body { padding: 30px; display: flex; gap: 30px; line-height: 1.6; }
    .v-divider { width: 1px; background: #30363d; }
    .lang-section h3 { font-size: 0.8rem; color: #58a6ff; margin-bottom: 12px; font-family: 'JetBrains Mono'; }
    .btn-ack { width: 100%; padding: 18px; background: #238636; color: #fff; border: none; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; transition: 0.2s; }

    /* --- TOAST & MENTOR --- */
    #toast-container { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); z-index: 11000; display: flex; flex-direction: column; align-items: center; gap: 8px; }
    #toast { background: #161b22; border: 1px solid #58a6ff; color: #fff; padding: 12px 25px; border-radius: 8px; font-family: 'JetBrains Mono'; font-size: 0.8rem; box-shadow: 0 10px 30px rgba(0,0,0,0.5); display: none; border-left: 4px solid #58a6ff; text-align: center; }
    #mentor-btn { background: #f85149; color: #fff; font-size: 0.65rem; padding: 5px 12px; border-radius: 20px; cursor: pointer; display: none; font-weight: bold; border: none; animation: pulse 1.5s infinite; }

    /* --- SIDE PANEL & TABLES --- */
    .soc-top-bar { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .btn-filter { background: #161b22; border: 1px solid #30363d; color: #8b949e; padding: 8px 18px; border-radius: 6px; font-size: 0.75rem; cursor: pointer; transition: 0.2s; }
    .btn-filter.active { background: #58a6ff; color: #0d1117; font-weight: bold; border-color: #58a6ff; }
    .incident-table { width: 100%; border-collapse: collapse; background: #0d1117; }
    .incident-table th { background: #161b22; text-align: left; padding: 15px; font-size: 0.7rem; color: #8b949e; text-transform: uppercase; }
    .incident-table td { padding: 15px; border-bottom: 1px solid #21262d; font-size: 0.85rem; }
    .side-panel { position: fixed; top: 0; right: -650px; width: 550px; height: 100vh; background: #0d1117; border-left: 1px solid #30363d; transition: 0.4s cubic-bezier(0.05, 0.7, 0.1, 1); z-index: 9000; display: flex; flex-direction: column; }
    .panel-active { right: 0; box-shadow: -20px 0 80px rgba(0,0,0,0.8); }
    .card { background: #161b22; border: 1px solid #30363d; border-radius: 8px; padding: 15px; margin-bottom: 20px; }
    .artifact-input { width: 100%; background: #010409; border: 1px solid #30363d; color: #e6edf3; padding: 10px; font-size: 0.85rem; border-radius: 4px; margin-top: 5px; }
    .badge { padding: 4px 8px; border-radius: 4px; font-size: 0.7rem; font-weight: bold; }
    .badge-tp { background: rgba(248, 81, 73, 0.1); color: #f85149; border: 1px solid #f85149; }
    .badge-fp { background: rgba(88, 166, 255, 0.1); color: #58a6ff; border: 1px solid #58a6ff; }
    .badge-esc { background: rgba(210, 153, 34, 0.1); color: #d29922; border: 1px solid #d29922; }

    @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
    @keyframes pulse { 0%, 100% { opacity: 0.6; } 50% { opacity: 1; } }
</style>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">SENTINEL GUARD // MISSION PROTOCOL</div>
        <div class="disclaimer-body">
            <div class="lang-section">
                <h3>[TR] VAKA MÜDAHALE</h3>
                <p>Alarmları analiz edin, TP/FP kararı verin ve L2 yükseltme süreçlerini yönetin. Yanlış kararlar mentor tarafından denetlenir.</p>
            </div>
            <div class="v-divider"></div>
            <div class="lang-section">
                <h3>[EN] INCIDENT RESPONSE</h3>
                <p>Analyze alerts, determine verdict, and manage L2 escalation. Wrong decisions are audited by the system mentor.</p>
            </div>
        </div>
        <button class="btn-ack" onclick="closeModal('welcomeModal')">AUTHORIZE SESSION</button>
    </div>
</div>

<div id="toast-container">
    <div id="toast"></div>
    <button id="mentor-btn" onclick="openMentor()">WHY AM I SEEING THIS ERROR? / NEDEN HATA ALDIM?</button>
</div>

<div id="mentorModal" class="disclaimer-modal" style="z-index: 12000;">
    <div class="disclaimer-box" style="width: 650px;">
        <div class="disclaimer-header">ANALYST MENTOR // ROOT CAUSE</div>
        <div class="disclaimer-body" id="mentorBody" style="color: #8b949e; font-size: 0.9rem;"></div>
        <button class="btn-ack" onclick="closeModal('mentorModal')">I UNDERSTAND / ANLADIM</button>
    </div>
</div>

<div class="soc-wrapper">
    <div class="soc-top-bar">
        <div style="display:flex; gap:10px;">
            <button class="btn-filter active" id="tab-open-btn" onclick="switchTab('open')">ACTIVE QUEUE (<span id="count-open">3</span>)</button>
            <button class="btn-filter" id="tab-closed-btn" onclick="switchTab('closed')">CLOSED ARCHIVE (<span id="count-closed">3</span>)</button>
        </div>
        <div style="font-family:'JetBrains Mono'; font-size:0.7rem; color:#8b949e;">STATUS: <span style="color:#3fb950">AUTHORIZED</span></div>
    </div>

    <div class="queue-container" style="flex:1; overflow-y:auto;">
        <table class="incident-table">
            <thead>
                <tr><th width="110">Case ID</th><th>Description</th><th width="100">Severity</th><th width="130">Verdict</th><th width="110">Action</th></tr>
            </thead>
            <tbody id="queueBody"></tbody>
        </table>
    </div>
</div>

<div id="sidePanel" class="side-panel">
    <div class="disclaimer-header" style="display:flex; justify-content:space-between; align-items:center;">
        <span id="pID">#SOC-0000</span>
        <span onclick="closePanel()" style="cursor:pointer; font-size:24px;">×</span>
    </div>
    <div style="padding:25px; flex:1; overflow-y:auto;" id="pBody"></div>
    <div style="padding:20px; border-top:1px solid #30363d; display:grid; grid-template-columns:1fr 1fr; gap:10px;" id="pFooter"></div>
</div>

<script>
    const cases = {
        1: { id: "#SOC-5092", title: "Ransomware Pattern Detection", sev: "CRITICAL", status: "open", verdict: "Pending", q: [{l: "Process Name:", a: "tasksche.exe"}, {l: "Target Host IP:", a: "10.20.5.100"}] },
        2: { id: "#SOC-1022", title: "Privileged Account Bruteforce", sev: "HIGH", status: "open", verdict: "Pending", q: [{l: "Target Account:", a: "adm_emir"}, {l: "Source IP:", a: "192.168.1.150"}] },
        3: { id: "#SOC-883", title: "SQLi Attempt: Web-Prod", sev: "HIGH", status: "open", verdict: "Pending", q: [{l: "Tool Agent:", a: "sqlmap/1.4.7"}, {l: "Target Schema:", a: "information_schema"}] },
        
        101: { id: "#SOC-2201", title: "PS Remote Exfiltration", sev: "HIGH", status: "closed", verdict: "True Positive", report: "Confirmed data theft attempt." },
        102: { id: "#SOC-2188", title: "MFA Push Fatigue", sev: "MEDIUM", status: "closed", verdict: "True Positive", report: "User spammed with MFA requests." },
        103: { id: "#SOC-2015", title: "Internal Port Scan", sev: "LOW", status: "closed", verdict: "False Positive", report: "Authorized scan confirmed." }
    };

    const mentorDocs = {
        art: "<h3>Artifact Mismatch / Kanıt Hatası</h3><p>[EN] Artifacts do not match SIEM logs. [TR] Girdiğiniz kanıtlar SIEM loglarıyla eşleşmiyor.</p>",
        auth: "<h3>Authority Error / Yetki Hatası</h3><p>[EN] Critical alerts must be escalated to L2. [TR] Kritik alarmlar L2'ye devredilmelidir.</p>",
        logic: "<h3>Logic Conflict / Mantık Hatası</h3><p>[EN] Malicious patterns cannot be True Positive. [TR] Zararlı örüntülere False Positive denilemez.</p>"
    };

    let activeTab = 'open';
    let errorContext = "";

    function showToast(msg, err) {
        const t = document.getElementById('toast');
        const m = document.getElementById('mentor-btn');
        t.innerText = msg; t.style.display = 'block';
        if(err) { errorContext = err; m.style.display = 'block'; } else { m.style.display = 'none'; }
        setTimeout(() => { t.style.display = 'none'; }, 4000);
    }

    function openMentor() {
        document.getElementById('mentorBody').innerHTML = mentorDocs[errorContext];
        document.getElementById('mentorModal').style.display = 'flex';
    }

    function renderTable() {
        const body = document.getElementById('queueBody');
        body.innerHTML = "";
        Object.keys(cases).forEach(key => {
            const c = cases[key];
            if(c.status === activeTab) {
                const vCls = c.verdict === 'True Positive' ? 'badge-tp' : (c.verdict === 'False Positive' ? 'badge-fp' : (c.verdict === 'Escalated' ? 'badge-esc' : ''));
                body.innerHTML += `<tr class="alert-row" onclick="openCase(${key})"><td>${c.id}</td><td><strong>${c.title}</strong></td><td><span style="color:${c.sev==='CRITICAL'?'#f85149':c.sev==='HIGH'?'#d29922':'#8b949e'}">${c.sev}</span></td><td><span class="badge ${vCls}">${c.verdict}</span></td><td><button class="btn-filter" style="width:100%">${c.status==='open'?'ANALYZE':'VIEW'}</button></td></tr>`;
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
            b.innerHTML = `<div class="card"><h4>Verification / Doğrulama</h4>${c.q.map((q,i)=>`<label style="font-size:0.7rem; color:#8b949e;">${q.l}</label><input class="artifact-input" id="ans-${i}">`).join('')}</div><div class="card"><h4>Verdict / Karar</h4><select class="artifact-input" id="vSel"><option value="True Positive">True Positive</option><option value="False Positive">False Positive</option></select></div>`;
            f.innerHTML = `<button class="btn-footer" style="background:#d29922" onclick="doEsc(${k})">ESCALATE L2</button><button class="btn-footer" style="background:#238636; color:white;" onclick="doRes(${k})">RESOLVE</button>`;
        } else {
            b.innerHTML = `<div class="card"><h4>Report / Rapor</h4><p>${c.report}</p></div>`;
            f.innerHTML = `<button class="btn-footer" style="background:#30363d; color:white; grid-column:span 2;" onclick="closePanel()">CLOSE</button>`;
        }
        document.getElementById('sidePanel').classList.add('panel-active');
    }

    function checkValid(k) {
        let ok = true;
        cases[k].q.forEach((q,i) => {
            const inp = document.getElementById(`ans-${i}`);
            if(inp.value.trim().toLowerCase() !== q.a.toLowerCase()) { ok = false; inp.style.borderColor = "#f85149"; } 
            else { inp.style.borderColor = "#238636"; }
        });
        return ok;
    }

    function doRes(k) {
        if(!checkValid(k)) { showToast("ARTIFACT MISMATCH / KANIT HATASI!", "art"); return; }
        if(k == 1) { showToast("AUTHORITY ERROR / YETKİ HATASI!", "auth"); return; }
        const v = document.getElementById('vSel').value;
        if(v === "False Positive") { showToast("LOGIC ERROR / MANTIK HATASI!", "logic"); return; }
        cases[k].status = 'closed'; cases[k].verdict = v;
        cases[k].report = "Resolved / Çözüldü.";
        showToast("Case Resolved / Vaka Kapatıldı."); renderTable(); closePanel();
    }

    function doEsc(k) {
        if(!checkValid(k)) { showToast("ESC FAILED / DOĞRULAMA HATASI!", "art"); return; }
        const v = document.getElementById('vSel').value;
        if(v === "False Positive") { showToast("LOGIC ERROR / MANTIK HATASI!", "logic"); return; }
        cases[k].status = 'closed'; cases[k].verdict = "Escalated";
        showToast("Escalated to L2 / L2'ye Aktarıldı."); renderTable(); closePanel();
    }

    function switchTab(t) { activeTab = t; renderTable(); }
    function updateStats() { 
        document.getElementById('count-open').innerText = Object.values(cases).filter(c => c.status === 'open').length;
        document.getElementById('count-closed').innerText = Object.values(cases).filter(c => c.status === 'closed').length;
    }
    function closePanel() { document.getElementById('sidePanel').classList.remove('panel-active'); }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }
    window.onload = () => { document.getElementById('welcomeModal').style.display = 'flex'; renderTable(); };
</script>
