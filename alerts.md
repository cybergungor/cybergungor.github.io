---
layout: default
title: SentinelGuard - SOC Analyst Station
permalink: /alerts/
---

<style>
    /* MONITOR WORKSTATION SETUP */
    main { background: #010409; padding-bottom: 100px !important; }
    
    .workstation-container {
        max-width: 1400px; /* Genişliği artırdık */
        margin: 50px auto;
        padding: 0 40px; /* Yanlardan daha fazla pay bıraktık */
    }

    /* Monitör Kasası - Yatayda Genişleme */
    .monitor-frame {
        background: #1a1a1a;
        padding: 12px;
        border-radius: 12px;
        border: 4px solid #333;
        box-shadow: 0 40px 100px rgba(0,0,0,0.9);
        position: relative;
        z-index: 1;
        width: 100%; /* Konteynırı tam kaplasın */
    }

    /* Monitör Ayağı */
    .monitor-frame::after {
        content: "";
        position: absolute;
        bottom: -50px;
        left: 50%;
        transform: translateX(-50%);
        width: 300px; /* Ayak genişliğini de orantılı artırdık */
        height: 50px;
        background: linear-gradient(to bottom, #222, #111);
        clip-path: polygon(15% 0%, 85% 0%, 100% 100%, 0% 100%);
        z-index: -1;
    }

    /* Ekran Paneli - Geniş Dikdörtgen Formu */
    .screen-container {
        border-radius: 4px;
        overflow: hidden;
        border: 10px solid #080808;
        position: relative;
        background: #0d1117;
        width: 100%;
        /* Yüksekliği sabit tutarak yanlara açılmayı sağlıyoruz */
        height: 650px; 
        display: flex;
        flex-direction: column;
    }

    .screen-glare {
        position: absolute;
        top: 0; left: 0; width: 100%; height: 100%;
        background: linear-gradient(110deg, rgba(255,255,255,0.02) 0%, rgba(255,255,255,0) 15%);
        pointer-events: none;
        z-index: 10;
    }

    /* SOC UI STYLES */
    .soc-wrapper { 
        display: flex; 
        flex-direction: column; 
        height: 100%; 
        width: 100%;
        color: #e6edf3; 
        font-family: 'Inter', sans-serif; 
        position: relative;
    }
    
    .soc-top-bar { 
        background: #0d1117; 
        padding: 15px 25px; 
        border-bottom: 1px solid #30363d; 
        display: flex; 
        justify-content: space-between; 
        align-items: center; 
    }

    /* Yan Panel - Geniş Ekranda Daha Şık Durması İçin */
    .side-panel { 
        position: absolute; 
        top: 0; 
        right: -100%; 
        width: 450px; 
        height: 100%; 
        background: #0d1117; 
        border-left: 1px solid #30363d; 
        transition: right 0.4s cubic-bezier(0.05, 0.7, 0.1, 1); 
        z-index: 9000; 
        display: flex; 
        flex-direction: column; 
    }
    .panel-active { right: 0; box-shadow: -15px 0 40px rgba(0,0,0,0.6); }
    
    .table-holder { flex: 1; overflow-y: auto; overflow-x: hidden; }
    .incident-table { width: 100%; border-collapse: collapse; table-layout: fixed; }
    .incident-table th { background: #161b22; text-align: left; padding: 15px; font-size: 0.7rem; color: #8b949e; position: sticky; top: 0; z-index: 5; }
    .incident-table td { padding: 15px; border-bottom: 1px solid #21262d; font-size: 0.85rem; word-wrap: break-word; }

    /* Diğer Buton ve Badge Stilleri (Aynı Kaldı) */
    .btn-filter { background: #161b22; border: 1px solid #30363d; color: #8b949e; padding: 8px 18px; border-radius: 6px; font-size: 0.75rem; cursor: pointer; }
    .btn-filter.active { background: #58a6ff; color: #0d1117; font-weight: bold; }
    .card { background: #161b22; border: 1px solid #30363d; border-radius: 8px; padding: 15px; margin-bottom: 15px; }
    .artifact-input { width: 100%; background: #010409; border: 1px solid #30363d; color: #e6edf3; padding: 10px; font-size: 0.85rem; border-radius: 4px; margin-top: 5px; }
    .badge { padding: 4px 8px; border-radius: 4px; font-size: 0.7rem; font-weight: bold; }
    .badge-tp { background: rgba(248,81,73,0.1); color: #f85149; border: 1px solid #f85149; }
    .badge-fp { background: rgba(88,166,255,0.1); color: #58a6ff; border: 1px solid #58a6ff; }
    .badge-esc { background: rgba(210,153,34,0.1); color: #d29922; border: 1px solid #d29922; }

    #toast-container { position: absolute; top: 20px; left: 50%; transform: translateX(-50%); z-index: 11000; display: flex; flex-direction: column; align-items: center; width: 70%; }
    #toast { background: #0d1117; border: 1px solid #58a6ff; color: #fff; padding: 12px 25px; border-radius: 8px; font-family: 'JetBrains Mono'; font-size: 0.8rem; box-shadow: 0 10px 30px rgba(0,0,0,0.5); display: none; width: 100%; text-align: center; }
    #mentor-btn { background: #f85149; color: #fff; font-size: 0.65rem; padding: 6px 15px; border-radius: 20px; cursor: pointer; display: none; font-weight: bold; border: none; margin-top: 5px; }

    .labs-intro { padding: 30px; background: #0d1117; border: 1px solid #30363d; border-radius: 12px; margin-bottom: 40px; max-width: 1400px; margin-left: auto; margin-right: auto; }
</style>


<div class="labs-intro">
    <div style="display: flex; align-items: center; gap: 15px; margin-bottom: 15px;">
        <div style="width: 12px; height: 12px; background: #3fb950; border-radius: 50%; box-shadow: 0 0 10px #3fb950;"></div>
        <h2 style="margin: 0; color: #58a6ff; font-family: 'JetBrains Mono'; font-size: 1.4rem;">LAB-01: Incident Response Terminal</h2>
    </div>
    <p style="color: #8b949e; line-height: 1.6; font-size: 0.95rem;">
        Analiz yeteneklerinizi gerçek dünya senaryoları üzerinde deneyimleyin.
    </p>
</div>

<div class="workstation-container">
    <div class="monitor-frame">
        <div class="screen-container">
            <div class="screen-glare"></div>
            
            <div class="soc-wrapper">
                <div id="toast-container">
                    <div id="toast"></div>
                    <button id="mentor-btn" onclick="openMentor()">ANALİZ REHBERİNİ AÇ</button>
                </div>

                <div class="soc-top-bar">
                    <div style="display:flex; gap:10px;">
                        <button class="btn-filter active" id="tab-open-btn" onclick="switchTab('open')">ACTIVE QUEUE (<span id="count-open">0</span>)</button>
                        <button class="btn-filter" id="tab-closed-btn" onclick="switchTab('closed')">CLOSED ARCHIVE (<span id="count-closed">0</span>)</button>
                    </div>
                    <div style="font-family:'JetBrains Mono'; font-size:0.7rem; color:#8b949e;">SG-TERM-01 // STATUS: <span style="color:#3fb950">AUTHORIZED</span></div>
                </div>

                <div class="table-holder">
                    <table class="incident-table">
                        <thead>
                            <tr><th width="110">Case ID</th><th>Description</th><th width="100">Severity</th><th width="120">Verdict</th><th width="100">Action</th></tr>
                        </thead>
                        <tbody id="queueBody"></tbody>
                    </table>
                </div>

                <div id="sidePanel" class="side-panel">
                    <div style="background:#161b22; padding:20px; border-bottom:1px solid #30363d; display:flex; justify-content:space-between; align-items:center;">
                        <span id="pID" style="color:#58a6ff; font-weight:bold;">#SOC-0000</span>
                        <span onclick="closePanel()" style="cursor:pointer; font-size:24px;">×</span>
                    </div>
                    <div style="padding:25px; flex:1; overflow-y:auto;" id="pBody"></div>
                    <div style="padding:20px; border-top:1px solid #30363d; display:grid; grid-template-columns:1fr 1fr; gap:10px;" id="pFooter"></div>
                </div>
            </div>
        </div>
    </div>
</div>

<div id="mentorModal" class="modal-overlay" style="position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(1, 4, 9, 0.98); z-index: 12000; display: none; align-items: center; justify-content: center;">
    <div class="modal-box">
        <div class="modal-header" style="background: #161b22; padding: 20px; color: #58a6ff; text-align: center; border-bottom: 1px solid #30363d;">ANALYST MENTOR // ROOT CAUSE</div>
        <div class="modal-body" id="mentorBody" style="padding: 30px; color: #8b949e;"></div>
        <div style="padding:15px; text-align:right;"><button class="btn-filter" style="background:#58a6ff; color:#000" onclick="closeModal('mentorModal')">ANLADIM</button></div>
    </div>
</div>

<script>
    // Önceki script içeriğinin aynısı (değişiklik yok)
    const cases = {
        1: { id: "#SOC-5092", title: "Ransomware Pattern Detection", sev: "CRITICAL", status: "open", verdict: "Pending", q: [{l: "Process Name:", a: "tasksche.exe"}, {l: "Target Host IP:", a: "10.20.5.100"}] },
        2: { id: "#SOC-1022", title: "Privileged Account Bruteforce", sev: "HIGH", status: "open", verdict: "Pending", q: [{l: "Target Account:", a: "adm_emir"}, {l: "Source IP:", a: "192.168.1.150"}] },
        3: { id: "#SOC-883", title: "SQLi Attempt: Web-Prod", sev: "HIGH", status: "open", verdict: "Pending", q: [{l: "Tool Agent:", a: "sqlmap/1.4.7"}, {l: "Target Schema:", a: "information_schema"}] },
        101: { id: "#SOC-2201", title: "PS Remote Exfiltration", sev: "HIGH", status: "closed", verdict: "True Positive", report: "Confirmed data theft attempt." },
        102: { id: "#SOC-2188", title: "MFA Push Fatigue", sev: "MEDIUM", status: "closed", verdict: "True Positive", report: "User spammed with MFA requests." },
        103: { id: "#SOC-2015", title: "Internal Port Scan", sev: "LOW", status: "closed", verdict: "False Positive", report: "Authorized Nessus scan confirmed." }
    };

    const mentorDocs = {
        art: "<h3>Artifact Mismatch</h3><p>Kanıtlar uyuşmuyor. Logs sayfasındaki değerleri tam kopyalayın.</p>",
        auth: "<h3>Yetki Hatası</h3><p>Kritik vakalar L1 tarafından çözülemez. Escalate edin.</p>",
        logic: "<h3>Mantık Hatası</h3><p>Saldırı olan vakaya FP diyemezsiniz.</p>"
    };

    let activeTab = 'open';
    let errorKey = "";

    function showToast(msg, err) {
        const t = document.getElementById('toast');
        const m = document.getElementById('mentor-btn');
        t.innerText = msg; t.style.display = 'block';
        if(err) { errorKey = err; m.style.display = 'block'; } else { m.style.display = 'none'; }
        setTimeout(() => { t.style.display = 'none'; }, 4000);
    }

    function openMentor() {
        document.getElementById('mentorBody').innerHTML = mentorDocs[errorKey];
        document.getElementById('mentorModal').style.display = 'flex';
    }

    function renderTable() {
        const b = document.getElementById('queueBody'); b.innerHTML = "";
        Object.keys(cases).forEach(key => {
            const c = cases[key];
            if(c.status === activeTab) {
                const vCls = c.verdict === 'True Positive' ? 'badge-tp' : (c.verdict === 'False Positive' ? 'badge-fp' : (c.verdict === 'Escalated' ? 'badge-esc' : ''));
                b.innerHTML += `<tr class="alert-row" onclick="openCase(${key})"><td>${c.id}</td><td><strong>${c.title}</strong></td><td><span style="color:${c.sev==='CRITICAL'?'#f85149':c.sev==='HIGH'?'#d29922':'#8b949e'}">${c.sev}</span></td><td><span class="badge ${vCls}">${c.verdict}</span></td><td><button class="btn-filter" style="width:100%">${c.status==='open'?'ANALYZE':'VIEW'}</button></td></tr>`;
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
            b.innerHTML = `<div class="card"><h4>Verification</h4>${c.q.map((q,i)=>`<label style="font-size:0.7rem; color:#8b949e;">${q.l}</label><input class="artifact-input" id="ans-${i}">`).join('')}</div><div class="card"><h4>Verdict</h4><select class="artifact-input" id="vSel"><option value="True Positive">True Positive</option><option value="False Positive">False Positive</option></select><textarea class="artifact-input" id="aNotes" style="height:80px;"></textarea></div>`;
            f.innerHTML = `<button class="btn-footer" style="background:#d29922" onclick="doEsc(${k})">ESCALATE</button><button class="btn-footer" style="background:#238636; color:white;" onclick="doRes(${k})">RESOLVE</button>`;
        } else {
            b.innerHTML = `<div class="card"><h4>Archive Report</h4><p>Verdict: <span class="badge ${c.verdict==='True Positive'?'badge-tp':(c.verdict==='False Positive'?'badge-fp':'badge-esc')}">${c.verdict}</span></p><hr style="border:0; border-top:1px solid #30363d; margin:15px 0;"><p style="font-size:0.85rem;">${c.report}</p></div>`;
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
        if(!checkValid(k)) { showToast("ARTIFACT MISMATCH!", "art"); return; }
        if(k == 1) { showToast("AUTHORITY ERROR!", "auth"); return; }
        const v = document.getElementById('vSel').value;
        if(v === "False Positive") { showToast("LOGIC ERROR!", "logic"); return; }
        cases[k].status = 'closed'; cases[k].verdict = v;
        cases[k].report = document.getElementById('aNotes').value || "Resolved.";
        showToast("Case Resolved."); renderTable(); closePanel();
    }

    function doEsc(k) {
        if(!checkValid(k)) { showToast("ESC FAILED!", "art"); return; }
        const v = document.getElementById('vSel').value;
        if(v === "False Positive") { showToast("LOGIC ERROR!", "logic"); return; }
        cases[k].status = 'closed'; cases[k].verdict = "Escalated";
        cases[k].report = "Escalated to Tier-2.";
        showToast("Escalated to L2."); renderTable(); closePanel();
    }

    function switchTab(t) { activeTab = t; document.getElementById('tab-open-btn').classList.toggle('active', t === 'open'); document.getElementById('tab-closed-btn').classList.toggle('active', t === 'closed'); renderTable(); }
    function updateStats() { document.getElementById('count-open').innerText = Object.values(cases).filter(c => c.status === 'open').length; document.getElementById('count-closed').innerText = Object.values(cases).filter(c => c.status === 'closed').length; }
    function closePanel() { document.getElementById('sidePanel').classList.remove('panel-active'); }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }
    window.onload = () => { renderTable(); };
</script>
