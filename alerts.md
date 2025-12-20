---
layout: default
title: SentinelGuard - Hardened SOC Platform
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

[Image of a professional cybersecurity incident management console with alert filtering, investigative side panel, and toast notification popups]

<div id="toast-container">
    <div id="toast"></div>
    <button id="mentor-btn" onclick="openMentor()">NEREDE HATA YAPTIM? (REASONING)</button>
</div>

<div id="mentorModal" class="modal-overlay">
    <div class="modal-box">
        <div class="modal-header">ANALYST MENTORING SYSTEM</div>
        <div class="modal-body" id="mentorBody"></div>
        <div style="padding:15px; text-align:right;"><button class="btn-filter" style="background:#58a6ff; color:#000" onclick="closeModal('mentorModal')">ANLADIM</button></div>
    </div>
</div>

<div class="soc-wrapper">
    <div class="soc-top-bar">
        <div style="display:flex; gap:10px;">
            <button class="btn-filter active" id="tab-open" onclick="switchTab('open')">ACTIVE QUEUE (<span id="count-open">0</span>)</button>
            <button class="btn-filter" id="tab-closed" onclick="switchTab('closed')">CLOSED ARCHIVE (<span id="count-closed">0</span>)</button>
        </div>
        <div style="font-family:'JetBrains Mono'; font-size:0.7rem; color:#8b949e;">SENTINEL GUARD // SYSTEM STATUS: <span style="color:#3fb950">NOMINAL</span></div>
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
        <span onclick="closePanel()" style="cursor:pointer; font-size:20px;">×</span>
    </div>
    <div style="padding:20px; flex:1; overflow-y:auto;" id="pBody"></div>
    <div style="padding:20px; border-top:1px solid #30363d; display:grid; grid-template-columns:1fr 1fr; gap:10px;" id="pFooter"></div>
</div>

<script>
    const cases = {
        1: { id: "#SOC-5092", title: "Ransomware Pattern Detection", sev: "CRITICAL", status: "open", verdict: "Pending", q: [{l: "Process Name:", a: "tasksche.exe"}, {l: "Target Host IP:", a: "10.20.5.100"}] },
        2: { id: "#SOC-1022", title: "Privileged Account Bruteforce", sev: "HIGH", status: "open", verdict: "Pending", q: [{l: "User Account:", a: "adm_emir"}, {l: "Attacker Source IP:", a: "192.168.1.150"}] },
        3: { id: "#SOC-883", title: "SQLi Exploit Attempt", sev: "HIGH", status: "open", verdict: "Pending", q: [{l: "Tool Agent String:", a: "sqlmap/1.4.7"}, {l: "Target DB Schema:", a: "information_schema"}] },
        
        // ÖNCEKİ KAPALI VAKALAR
        101: { id: "#SOC-2201", title: "Unauthorized PS Exfiltration", sev: "HIGH", status: "closed", verdict: "True Positive", report: "Confirmed Red Team exercise from External IP." },
        102: { id: "#SOC-2188", title: "MFA Push Fatigue", sev: "MEDIUM", status: "closed", verdict: "True Positive", report: "User reported account hijack via push spamming." },
        103: { id: "#SOC-2015", title: "Scheduled Vulnerability Scan", sev: "LOW", status: "closed", verdict: "False Positive", report: "Verified Nessus scan on Dev subnet." }
    };

    const mentorData = {
        art: "<h3>Kanıt Doğrulama Hatası (Artifact Error)</h3><p>Girdiğiniz veriler (asdasd vb.) SIEM loglarındaki teknik verilerle uyuşmuyor. Bir analist her zaman <strong>Logs</strong> sayfasındaki IP, Hash veya Process isimlerini birebir kullanmalıdır.</p>",
        auth: "<h3>Yetki Hatası (Authority Error)</h3><p><strong>Neden Kapatamazsınız?</strong> Ransomware (SOC-5092) vakaları kritik seviyededir. L1 analistlerin fidye yazılımı vakalarını kapatma yetkisi yoktur. Bunu mutlaka <strong>ESCALATE</strong> etmelisiniz.</p>",
        logic: "<h3>Mantık Hatası (Verdict Logic)</h3><p>Saldırı örüntüsü olduğu kesinleşmiş bir vakayı 'False Positive' olarak işaretleyemezsiniz veya zararsız dediğiniz bir vakayı üst birime (Escalate) gönderemezsiniz.</p>"
    };

    let activeTab = 'open';
    let lastErr = "";

    function showToast(m, err) {
        const t = document.getElementById('toast');
        const mBtn = document.getElementById('mentor-btn');
        t.innerText = m; t.style.display = 'block';
        if(err) { lastErr = err; mBtn.style.display = 'block'; } else { mBtn.style.display = 'none'; }
        setTimeout(() => { t.style.display = 'none'; }, 4000);
    }

    function openMentor() {
        document.getElementById('mentorBody').innerHTML = mentorData[lastErr];
        document.getElementById('mentorModal').style.display = 'flex';
    }

    function renderTable() {
        const b = document.getElementById('queueBody'); b.innerHTML = "";
        let found = false;
        Object.entries(cases).forEach(([k, c]) => {
            if(c.status === activeTab) {
                found = true;
                let vCls = c.verdict === 'True Positive' ? 'badge-tp' : (c.verdict === 'False Positive' ? 'badge-fp' : (c.verdict === 'Escalated' ? 'badge-esc' : ''));
                b.innerHTML += `<tr class="alert-row" onclick="openCase(${k})"><td>${c.id}</td><td><strong>${c.title}</strong></td><td>${c.sev}</td><td><span class="badge ${vCls}">${c.verdict}</span></td><td><button class="btn-filter" style="width:100%">${c.status==='open'?'ANALYZE':'VIEW'}</button></td></tr>`;
            }
        });
        updateCounts();
    }

    function openCase(k) {
        const c = cases[k]; document.getElementById('pID').innerText = c.id;
        const b = document.getElementById('pBody'); const f = document.getElementById('pFooter');
        
        if(c.status === 'open') {
            b.innerHTML = `<div class="card"><h4>Technical Verification</h4>${c.q.map((q,i)=>`<label style="font-size:0.7rem; color:#8b949e">${q.l}</label><input class="artifact-input" id="ans-${i}" placeholder="Enter artifact from logs...">`).join('')}</div><div class="card"><h4>Verdict Decision</h4><select class="artifact-input" id="vSel"><option value="True Positive">True Positive</option><option value="False Positive">False Positive</option></select><textarea class="artifact-input" id="aNotes" placeholder="Investigation Summary..." style="height:80px;"></textarea></div>`;
            f.innerHTML = `<button class="btn-footer" style="background:#d29922; color:#000" onclick="doEsc(${k})">ESCALATE L2</button><button class="btn-footer" style="background:#238636; color:white;" onclick="doRes(${k})">RESOLVE</button>`;
        } else {
            b.innerHTML = `<div class="card"><h4>Incident Closing Report</h4><p>Verdict: <span class="badge ${c.verdict === 'True Positive' ? 'badge-tp' : (c.verdict === 'False Positive' ? 'badge-fp' : 'badge-esc')}">${c.verdict}</span></p><hr style="border:0; border-top:1px solid #30363d; margin:15px 0;"><p style="font-size:0.85rem; line-height:1.6">${c.report || 'Archived record summary.'}</p></div>`;
            f.innerHTML = `<button class="btn-footer" style="background:#30363d; color:white; grid-column:span 2;" onclick="closePanel()">CLOSE VIEW</button>`;
        }
        document.getElementById('sidePanel').classList.add('panel-active');
    }

    function validateArtifacts(k) {
        let ok = true;
        cases[k].q.forEach((q,i) => { 
            const inp = document.getElementById(`ans-${i}`);
            if(inp.value.trim().toLowerCase() !== q.a.toLowerCase()) {
                ok = false;
                inp.style.borderColor = "#f85149";
            } else {
                inp.style.borderColor = "#238636";
            }
        });
        return ok;
    }

    function doRes(k) {
        if(!validateArtifacts(k)) { showToast("ARTIFACT VERIFICATION FAILED!", "art"); return; }
        if(k == 1) { showToast("SECURITY PROTOCOL ERROR: MUST ESCALATE!", "auth"); return; }
        const selV = document.getElementById('vSel').value;
        if(selV === "False Positive") { showToast("DECISION ERROR: Pattern is Malicious.", "logic"); return; }

        cases[k].status = 'closed'; cases[k].verdict = selV;
        cases[k].report = document.getElementById('aNotes').value || "Incident resolved via standard IR playbook.";
        showToast("Case Successfully Resolved.", ""); renderTable(); closePanel();
    }

    function doEsc(k) {
        // EN KRİTİK DÜZELTME: validateArtifacts FALSE ise RETURN yapıyoruz.
        if(!validateArtifacts(k)) { 
            showToast("ESC FAILED: Invalid Artifacts!", "art"); 
            return; // SÜRECİ BURADA DURDURUYORUZ
        }
        
        const selV = document.getElementById('vSel').value;
        if(selV === "False Positive") { 
            showToast("LOGIC ERROR: Benign cases cannot be escalated.", "logic"); 
            return; 
        }

        cases[k].status = 'closed'; cases[k].verdict = "Escalated";
        cases[k].report = "Escalated to Tier-2 SOC Team. Reason: High severity / L1 Authority limit. Analyst Verdict: " + selV;
        showToast("Escalated to L2 SOC Analyst.", ""); renderTable(); closePanel();
    }

    function switchTab(t) { 
        activeTab = t; 
        document.getElementById('tab-open').classList.toggle('active', t==='open'); 
        document.getElementById('tab-closed').classList.toggle('active', t==='closed'); 
        renderTable(); 
    }

    function updateCounts() { 
        document.getElementById('count-open').innerText = Object.values(cases).filter(c=>c.status==='open').length; 
        document.getElementById('count-closed').innerText = Object.values(cases).filter(c=>c.status==='closed').length; 
    }

    function closePanel() { document.getElementById('sidePanel').classList.remove('panel-active'); }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }
    window.onload = () => { renderTable(); };
</script>
