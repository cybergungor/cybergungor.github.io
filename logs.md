---
layout: default
title: SentinelGuard - SOC Mentor Dashboard
permalink: /alerts/
---

<style>
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    .soc-wrapper { display: flex; flex-direction: column; height: calc(100vh - 60px); color: #e6edf3; font-family: 'Inter', sans-serif; }

    /* --- ENHANCED TOAST & ERROR GUIDE --- */
    #toast-container {
        position: fixed; top: 20px; left: 50%; transform: translateX(-50%);
        z-index: 11000; display: flex; flex-direction: column; align-items: center; gap: 10px;
    }
    #toast {
        background: #161b22; border: 1px solid #58a6ff; color: #fff;
        padding: 12px 25px; border-radius: 8px; font-family: 'JetBrains Mono';
        font-size: 0.85rem; box-shadow: 0 10px 30px rgba(0,0,0,0.5); display: none;
    }
    #error-guide-trigger {
        background: #f85149; color: #fff; font-size: 0.7rem; padding: 5px 12px;
        border-radius: 20px; cursor: pointer; display: none; font-weight: bold;
        text-transform: uppercase; border: none; animation: pulse 1.5s infinite;
    }

    /* --- KNOWLEDGE BASE MODAL (EĞİTİCİ METİN) --- */
    .kb-modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(1, 4, 9, 0.98); z-index: 12000; display: none; align-items: center; justify-content: center; backdrop-filter: blur(10px); }
    .kb-box { background: #0d1117; border: 1px solid #30363d; width: 700px; border-radius: 12px; overflow: hidden; }
    .kb-header { background: #f85149; color: #fff; padding: 15px; font-family: 'JetBrains Mono'; font-weight: bold; text-align: center; }
    .kb-body { padding: 30px; font-size: 0.9rem; color: #8b949e; line-height: 1.7; max-height: 500px; overflow-y: auto; }
    .kb-footer { padding: 15px; border-top: 1px solid #30363d; text-align: right; }

    /* UI Components (Önceki kodlardan korunanlar) */
    .disclaimer-modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(1, 4, 9, 0.96); z-index: 10000; display: none; align-items: center; justify-content: center; }
    .disclaimer-box { background: #0d1117; border: 1px solid #30363d; width: 750px; border-radius: 12px; overflow: hidden; }
    .btn-ack { width: 100%; padding: 18px; background: #238636; color: #fff; border: none; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; }
    .soc-top-bar { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; }
    .btn-filter { background: #161b22; border: 1px solid #30363d; color: #8b949e; padding: 8px 18px; border-radius: 6px; font-size: 0.75rem; cursor: pointer; }
    .btn-filter.active { background: #58a6ff; color: #0d1117; font-weight: bold; }
    .incident-table { width: 100%; border-collapse: collapse; background: #0d1117; border: 1px solid #30363d; }
    .incident-table th { background: #161b22; text-align: left; padding: 15px; font-size: 0.7rem; color: #8b949e; }
    .incident-table td { padding: 18px 15px; border-bottom: 1px solid #21262d; font-size: 0.85rem; }
    .side-panel { position: fixed; top: 0; right: -650px; width: 600px; height: 100vh; background: #0d1117; border-left: 1px solid #30363d; transition: 0.4s; z-index: 9000; display: flex; flex-direction: column; }
    .panel-active { right: 0; }
    .card { background: #161b22; border: 1px solid #30363d; border-radius: 12px; padding: 20px; margin-bottom: 25px; }
    .artifact-input { width: 100%; background: #010409; border: 1px solid #30363d; color: #e6edf3; padding: 12px; font-size: 0.85rem; border-radius: 6px; }
    .btn-footer { padding: 14px; border-radius: 6px; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; border: none; }
    .badge-tp { background: rgba(248, 81, 73, 0.1); color: #f85149; border: 1px solid #f85149; }
    .badge-fp { background: rgba(88, 166, 255, 0.1); color: #58a6ff; border: 1px solid #58a6ff; }
    .badge-esc { background: rgba(210, 153, 34, 0.1); color: #d29922; border: 1px solid #d29922; }

    @keyframes pulse { 0% { opacity: 0.7; } 50% { opacity: 1; } 100% { opacity: 0.7; } }
</style>

<div id="kbModal" class="kb-modal">
    <div class="kb-box">
        <div class="kb-header">ANALYST MENTOR // ROOT CAUSE ERROR ANALYSIS</div>
        <div class="kb-body" id="kbContent"></div>
        <div class="kb-footer">
            <button class="btn-ack" style="width:auto; padding:10px 25px;" onclick="closeModal('kbModal')">I UNDERSTAND</button>
        </div>
    </div>
</div>

<div id="toast-container">
    <div id="toast"></div>
    <button id="error-guide-trigger" onclick="openMentorGuide()">Click to see why (Nedenini görmek için tıklayın)</button>
</div>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">SENTINEL GUARD // COMMAND & CONTROL</div>
        <div class="disclaimer-body">
            <div style="flex:1;">
                <h3>[TR] MENTOR SİSTEMİ</h3>
                <p>Analiz sırasında hatalı bir karar verirseniz sistem sizi durduracak ve teknik bir rehber sunacaktır.</p>
            </div>
            <div style="width:1px; background:#30363d; margin:0 20px;"></div>
            <div style="flex:1;">
                <h3>[EN] MENTORING SYSTEM</h3>
                <p>If you make a wrong investigative decision, the system will provide a technical guide to correct your workflow.</p>
            </div>
        </div>
        <button class="btn-ack" onclick="closeModal('welcomeModal')">START SESSION</button>
    </div>
</div>

<div class="soc-wrapper">
    <div class="soc-top-bar">
        <div class="filter-group">
            <button class="btn-filter active" id="tab-open" onclick="switchTab('open')">OPEN ALERTS (<span id="count-open">3</span>)</button>
            <button class="btn-filter" id="tab-closed" onclick="switchTab('closed')">CLOSED ARCHIVE (<span id="count-closed">5</span>)</button>
        </div>
    </div>
    <div class="queue-container">
        <table class="incident-table">
            <thead>
                <tr><th width="120">Case ID</th><th>Tactical Description</th><th width="120">Severity</th><th width="140">Verdict</th><th width="120">Action</th></tr>
            </thead>
            <tbody id="queueBody"></tbody>
        </table>
    </div>
</div>

<div id="sidePanel" class="side-panel">
    <div class="panel-header">
        <div style="font-family:'JetBrains Mono'; font-size:0.85rem; font-weight:bold;">ANALYSIS: <span id="panelId" style="color:#58a6ff;">#SOC-0000</span></div>
        <span style="cursor:pointer; font-size:1.8rem; color:#8b949e;" onclick="closePanel()">×</span>
    </div>
    <div class="panel-body" id="panelBody"></div>
    <div class="panel-footer" id="panelFooter"></div>
</div>

<script>
    const cases = {
        1: { id: "#SOC-5092", title: "Ransomware Activity", sev: "CRITICAL", status: "open", verdict: "Pending", root_cause: "High entropy file modifications detected. Extension: .locky", q: [{l: "Process:", a: "tasksche.exe"}, {l: "Target IP:", a: "10.20.5.100"}] },
        2: { id: "#SOC-1022", title: "Domain Admin Bruteforce", sev: "HIGH", status: "open", verdict: "Pending", root_cause: "Failed logon spike against 'adm_emir' from 192.168.1.150", q: [{l: "User:", a: "adm_emir"}, {l: "Attacker IP:", a: "192.168.1.150"}] },
        3: { id: "#SOC-883", title: "SQLi Attempt", sev: "HIGH", status: "open", verdict: "Pending", root_cause: "UNION SELECT pattern matched in URI parameters.", q: [{l: "Agent:", a: "sqlmap/1.4.7"}, {l: "Schema:", a: "information_schema"}] }
    };

    // --- MENTORING DATA (HATA METİNLERİ) ---
    const mentorGuide = {
        artifactError: "<h3>Forensic Verification Failed</h3><p>[TR] Girdiğiniz kanıtlar SIEM loglarıyla eşleşmiyor. Bir analist her zaman loglardaki veriyi tam (copy-paste) olarak kullanmalıdır.</p><p>[EN] Artifact mismatch. An analyst must always use the exact data from SIEM logs. Check the /logs page again.</p>",
        authorityError: "<h3>Escalation Mandatory (Critical Risk)</h3><p>[TR] <strong>Neden Kapatamazsınız?</strong> Bu bir Ransomware vakasıdır. L1 analistlerin fidye yazılımı vakalarını kapatma yetkisi yoktur. Bu olay veri kaybına yol açabileceği için L2/Senior ekipler tarafından derinlemesine incelenmeli ve host izole edilmelidir.</p><p>[EN] <strong>Why you can't resolve?</strong> This is a Critical Ransomware event. L1 analysts do not have the authority to close such high-impact cases. It must be ESCALATED for full forensic recovery.</p>",
        verdictError: "<h3>Verdict Contradiction</h3><p>[TR] <strong>Hata:</strong> Açık bir saldırı örüntüsü (Ransomware/Bruteforce) 'False Positive' olarak işaretlenemez. Bu, gerçek bir saldırıyı görmezden gelmek (Missing a Threat) anlamına gelir ve şirket için büyük risk oluşturur.</p><p>[EN] <strong>Error:</strong> You cannot flag a confirmed attack pattern as 'False Positive'. This represents a major security risk (False Negative) for the organization.</p>",
        logicError: "<h3>Conflicting Decision Logic</h3><p>[TR] Eğer bir vakaya 'Zararsız (False Positive)' diyorsanız, onu üst birime (Escalate) gönderemezsiniz. Bu çelişkili bir karardır.</p><p>[EN] You cannot escalate a case that you have already deemed benign (False Positive). This is a conflict in investigative logic.</p>"
    };

    let lastErrorType = "";
    let activeTab = 'open';

    function showToast(msg, errorType = "") {
        const t = document.getElementById('toast');
        const trigger = document.getElementById('error-guide-trigger');
        t.innerText = msg;
        t.style.display = 'block';
        if(errorType) {
            lastErrorType = errorType;
            trigger.style.display = 'block';
        }
        setTimeout(() => { t.style.display = 'none'; }, 5000);
    }

    function openMentorGuide() {
        const content = document.getElementById('kbContent');
        content.innerHTML = mentorGuide[lastErrorType] || "General Analysis Error.";
        document.getElementById('kbModal').style.display = 'flex';
        document.getElementById('error-guide-trigger').style.display = 'none';
    }

    function renderTable() {
        const body = document.getElementById('queueBody');
        body.innerHTML = "";
        Object.entries(cases).forEach(([key, c]) => {
            if(c.status !== activeTab) return;
            let vClass = c.verdict === 'True Positive' ? 'badge-tp' : (c.verdict === 'False Positive' ? 'badge-fp' : (c.verdict === 'Escalated' ? 'badge-esc' : ''));
            body.innerHTML += `<tr class="alert-row" onclick="openIncident(${key})"><td class="mono">${c.id}</td><td><strong>${c.title}</strong></td><td><span style="color:${c.sev==='CRITICAL'?'#f85149':'#d29922'}">${c.sev}</span></td><td><span class="badge ${vClass}">${c.verdict}</span></td><td><button class="btn-filter" style="width:100%">${c.status==='open'?'ANALYZE':'REPORT'}</button></td></tr>`;
        });
        updateCounts();
    }

    function openIncident(key) {
        const c = cases[key];
        const body = document.getElementById('panelBody');
        const footer = document.getElementById('panelFooter');
        document.getElementById('panelId').innerText = c.id;

        if(c.status === 'open') {
            body.innerHTML = `<div class="card"><h4>Root Cause</h4><div class="threat-info">${c.root_cause}</div></div><div class="card"><h4>Artifacts</h4>${c.q.map((q, i) => `<label style="font-size:0.7rem; color:#8b949e; display:block; margin-bottom:5px;">${q.l}</label><input type="text" class="artifact-input" id="ans-${i}" autocomplete="off">`).join('')}</div><div class="card"><h4>Verdict</h4><select class="artifact-input" id="verdictSelect"><option value="True Positive">True Positive</option><option value="False Positive">False Positive</option></select><textarea class="artifact-input" id="analystNotes" style="height:100px; resize:none;" placeholder="Summary..."></textarea></div>`;
            footer.innerHTML = `<button class="btn-footer btn-esc" style="background:#d29922;" onclick="handleEscalate(${key})">ESCALATE L2</button><button class="btn-footer btn-res" style="background:#238636; color:white;" onclick="handleResolve(${key})">RESOLVE CASE</button>`;
        } else {
            body.innerHTML = `<div class="card"><h3>Closed Report</h3><p>Verdict: ${c.verdict}</p><hr style="border:0; border-top:1px solid #30363d; margin:15px 0;"><p>${c.report}</p></div>`;
            footer.innerHTML = `<button class="btn-footer btn-res" style="grid-column: span 2; background:#30363d; color:white;" onclick="closePanel()">CLOSE</button>`;
        }
        document.getElementById('sidePanel').classList.add('panel-active');
    }

    function handleResolve(key) {
        const c = cases[key];
        const verdict = document.getElementById('verdictSelect').value;
        let correct = true;
        c.q.forEach((q, i) => { if(document.getElementById(`ans-${i}`).value.trim().toLowerCase() !== q.a.toLowerCase()) correct = false; });

        if(!correct) { showToast("ARTIFACT VERIFICATION FAILED", "artifactError"); return; }
        if(key === 1) { showToast("INSUFFICIENT AUTHORITY", "authorityError"); return; }
        if(verdict === "False Positive") { showToast("VERDICT ERROR", "verdictError"); return; }

        cases[key].status = 'closed';
        cases[key].verdict = verdict;
        cases[key].report = document.getElementById('analystNotes').value || "Resolved.";
        showToast("Case Resolved Successfully", "");
        renderTable(); closePanel();
    }

    function handleEscalate(key) {
        const verdict = document.getElementById('verdictSelect').value;
        if(verdict === "False Positive") { showToast("LOGIC ERROR", "logicError"); return; }
        
        showToast("Case Escalated to Tier-2", "");
        cases[key].status = 'closed';
        cases[key].verdict = "Escalated";
        cases[key].report = "Escalated for deep forensic analysis.";
        renderTable(); closePanel();
    }

    function switchTab(tab) { activeTab = tab; renderTable(); }
    function updateCounts() { document.getElementById('count-open').innerText = Object.values(cases).filter(c => c.status === 'open').length; }
    function closePanel() { document.getElementById('sidePanel').classList.remove('panel-active'); }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }
    window.onload = () => { document.getElementById('welcomeModal').style.display = 'flex'; renderTable(); };
</script>
