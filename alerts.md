---
layout: default
title: SentinelGuard - SOC Analyst Station
permalink: /alerts/
---

<style>
    /* MONITOR WORKSTATION SETUP */
    main { background: #010409; padding-bottom: 80px !important; }
    
    .workstation-container {
        max-width: 1400px; /* Daha geniş, gerçek monitör ölçeği */
        margin: 50px auto;
        padding: 0 20px;
    }

    /* Monitörün Dış Kasası */
    .monitor-frame {
        background: #1a1a1a;
        padding: 15px; /* İnce modern çerçeve */
        border-radius: 12px;
        border: 4px solid #333;
        box-shadow: 
            0 30px 60px rgba(0,0,0,0.9), 
            0 0 20px rgba(88, 166, 255, 0.05); /* Hafif led parlaması */
        position: relative;
    }

    /* Monitör Ayağı (Daha Gerçekçi) */
    .monitor-frame::after {
        content: "";
        position: absolute;
        bottom: -50px;
        left: 50%;
        transform: translateX(-50%);
        width: 250px;
        height: 50px;
        background: linear-gradient(to bottom, #222 0%, #111 100%);
        clip-path: polygon(20% 0%, 80% 0%, 100% 100%, 0% 100%); /* Trapez ayak tasarımı */
    }

    /* Ekranın İç Siyah Paneli (Bezel) */
    .screen-container {
        border-radius: 4px;
        overflow: hidden;
        border: 10px solid #080808;
        position: relative;
        background: #0d1117;
        aspect-ratio: 21 / 9; /* Ultra geniş monitör oranı */
        min-height: 600px;
    }

    /* Ekran Üzerindeki Işık Yansıması */
    .screen-glare {
        position: absolute;
        top: 0; left: 0; width: 100%; height: 100%;
        background: linear-gradient(110deg, rgba(255,255,255,0.03) 0%, rgba(255,255,255,0) 25%);
        pointer-events: none;
        z-index: 10;
    }

    /* SOC UI STYLES */
    .soc-wrapper { 
        display: flex; 
        flex-direction: column; 
        height: 100%; 
        color: #e6edf3; 
        font-family: 'Inter', sans-serif; 
    }
    
    #toast-container { position: absolute; top: 20px; left: 50%; transform: translateX(-50%); z-index: 11000; display: flex; flex-direction: column; align-items: center; width: 60%; }
    #toast { background: #0d1117; border: 1px solid #58a6ff; color: #fff; padding: 12px 25px; border-radius: 8px; font-family: 'JetBrains Mono'; font-size: 0.8rem; box-shadow: 0 10px 30px rgba(0,0,0,0.5); display: none; width: 100%; text-align: center; }
    #mentor-btn { background: #f85149; color: #fff; font-size: 0.65rem; padding: 6px 15px; border-radius: 20px; cursor: pointer; display: none; font-weight: bold; border: none; margin-top: 5px; }

    .modal-overlay { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(1, 4, 9, 0.98); z-index: 12000; display: none; align-items: center; justify-content: center; backdrop-filter: blur(10px); }
    .modal-box { background: #0d1117; border: 1px solid #30363d; width: 80%; max-width: 600px; border-radius: 12px; overflow: hidden; }
    
    .soc-top-bar { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .btn-filter { background: #161b22; border: 1px solid #30363d; color: #8b949e; padding: 8px 18px; border-radius: 6px; font-size: 0.75rem; cursor: pointer; }
    .btn-filter.active { background: #58a6ff; color: #0d1117; font-weight: bold; border-color: #58a6ff; }
    
    .table-holder { flex: 1; overflow-y: auto; background: #0d1117; }
    .incident-table { width: 100%; border-collapse: collapse; }
    .incident-table th { background: #161b22; text-align: left; padding: 15px; font-size: 0.7rem; color: #8b949e; position: sticky; top: 0; z-index: 5; }
    .incident-table td { padding: 15px; border-bottom: 1px solid #21262d; font-size: 0.85rem; }

    /* Yan Panel (Geniş Ekranda Sağdan Kayar) */
    .side-panel { 
        position: absolute; top: 0; right: -500px; width: 500px; height: 100%; 
        background: #0d1117; border-left: 1px solid #30363d; 
        transition: 0.4s cubic-bezier(0.05, 0.7, 0.1, 1); z-index: 9000; 
        display: flex; flex-direction: column; 
    }
    .panel-active { right: 0; box-shadow: -10px 0 50px rgba(0,0,0,0.5); }
    
    .card { background: #161b22; border: 1px solid #30363d; border-radius: 8px; padding: 15px; margin-bottom: 15px; }
    .artifact-input { width: 100%; background: #010409; border: 1px solid #30363d; color: #e6edf3; padding: 10px; font-size: 0.85rem; border-radius: 4px; margin-top: 5px; }
    .badge { padding: 4px 8px; border-radius: 4px; font-size: 0.7rem; font-weight: bold; }
    .badge-tp { background: rgba(248,81,73,0.1); color: #f85149; border: 1px solid #f85149; }
    .badge-fp { background: rgba(88,166,255,0.1); color: #58a6ff; border: 1px solid #58a6ff; }
    .badge-esc { background: rgba(210,153,34,0.1); color: #d29922; border: 1px solid #d29922; }

    /* LABS INTRO SECTION */
    .labs-intro { padding: 30px; background: #0d1117; border: 1px solid #30363d; border-radius: 12px; margin-bottom: 40px; max-width: 1400px; margin-left: auto; margin-right: auto; }
</style>

<div class="labs-intro">
    <div style="display: flex; align-items: center; gap: 15px; margin-bottom: 15px;">
        <div style="width: 12px; height: 12px; background: #3fb950; border-radius: 50%; box-shadow: 0 0 10px #3fb950;"></div>
        <h2 style="margin: 0; color: #58a6ff; font-family: 'JetBrains Mono'; font-size: 1.4rem;">LAB-01: Incident Response Terminal</h2>
    </div>
    <p style="color: #8b949e; line-height: 1.6; font-size: 0.95rem;">
        SentinelGuard terminaline hoş geldiniz. Bu istasyon, kritik altyapı alarmlarını analiz etmek, 
        delilleri doğrulamak ve vaka müdahale süreçlerini yönetmek için kullanılan profesyonel bir <strong>Analist İş İstasyonudur.</strong>
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

                <div id="mentorModal" class="modal-overlay">
                    <div class="modal-box">
                        <div class="modal-header">ANALYST MENTOR // ROOT CAUSE</div>
                        <div class="modal-body" id="mentorBody"></div>
                        <div style="padding:15px; text-align:right;"><button class="btn-filter" style="background:#58a6ff; color:#000" onclick="closeModal('mentorModal')">ANLADIM</button></div>
                    </div>
                </div>

                <div class="soc-top-bar">
                    <div style="display:flex; gap:10px;">
                        <button class="btn-filter active" id="tab-open-btn" onclick="switchTab('open')">ACTIVE QUEUE (<span id="count-open">0</span>)</button>
                        <button class="btn-filter" id="tab-closed-btn" onclick="switchTab('closed')">CLOSED ARCHIVE (<span id="count-closed">0</span>)</button>
                    </div>
                    <div style="font-family:'JetBrains Mono'; font-size:0.7rem; color:#8b949e;">WORKSTATION_ID: SG-TERM-01 // STATUS: <span style="color:#3fb950">ONLINE</span></div>
                </div>

                <div class="table-holder">
                    <table class="incident-table">
                        <thead>
                            <tr><th width="110">Case ID</th><th>Technical Description</th><th width="120">Severity</th><th width="130">Verdict</th><th width="110">Action</th></tr>
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

<script>
    const cases = {
        1: { id: "#SOC-5092", title: "Ransomware Pattern Detection", sev: "CRITICAL", status: "open", verdict: "Pending", q: [{l: "Process Name:", a: "tasksche.exe"}, {l: "Target Host IP:", a: "10.20.5.100"}] },
        2: { id: "#SOC-1022", title: "Privileged Account Bruteforce", sev: "HIGH", status: "open", verdict: "Pending", q: [{l: "Target User Account:", a: "adm_emir"}, {l: "Attacker Source IP:", a: "192.168.1.150"}] },
        3: { id: "#SOC-883", title: "SQLi Attempt: Web-Prod", sev: "HIGH", status: "open", verdict: "Pending", q: [{l: "Tool Agent String:", a: "sqlmap/1.4.7"}, {l: "Target DB Schema:", a: "information_schema"}] },
        
        101: { id: "#SOC-2201", title: "PS Remote Exfiltration", sev: "HIGH", status: "closed", verdict: "True Positive", report: "Confirmed data theft attempt via PowerShell Invoke-WebRequest." },
        102: { id: "#SOC-2188", title: "MFA Push Fatigue", sev: "MEDIUM", status: "closed", verdict: "True Positive", report: "User spammed with 50+ MFA requests. Password reset initiated." },
        103: { id: "#SOC-2015", title: "Internal Port Scan", sev: "LOW", status: "closed", verdict: "False Positive", report: "Authorized Nessus vulnerability scan confirmed by IT." }
    };

    const mentorDocs = {
        art: "<h3>Artifact Mismatch</h3><p>Girdiğiniz kanıtlar loglardaki verilerle eşleşmiyor. Lütfen <strong>Logs</strong> sekmesine gidip doğru değeri (Örn: tasksche.exe) birebir kopyalayın.</p>",
        auth: "<h3>Yetki Hatası</h3><p>Ransomware (SOC-5092) gibi kritik vakaları L1 analistler kapatamaz. Bu vakayı mutlaka <strong>Escalate</strong> ederek L2 ekibine aktarmalısınız.</p>",
        logic: "<h3>Mantık Hatası</h3><p>Zararlı olduğu kanıtlanmış bir vakayı 'False Positive' yapamazsınız. Ayrıca zararsız bir vakayı üst birime (Escalate) devredemezsiniz.</p>"
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
            b.innerHTML = `<div class="card"><h4>Technical Verification</h4>${c.q.map((q,i)=>`<label style="font-size:0.7rem; color:#8b949e;">${q.l}</label><input class="artifact-input" id="ans-${i}" placeholder="Value from logs...">`).join('')}</div><div class="card"><h4>Verdict Decision</h4><select class="artifact-input" id="vSel"><option value="True Positive">True Positive</option><option value="False Positive">False Positive</option></select><textarea class="artifact-input" id="aNotes" placeholder="Summary..." style="height:80px;"></textarea></div>`;
            f.innerHTML = `<button class="btn-footer" style="background:#d29922" onclick="doEsc(${k})">ESCALATE L2</button><button class="btn-footer" style="background:#238636; color:white;" onclick="doRes(${k})">RESOLVE</button>`;
        } else {
            b.innerHTML = `<div class="card"><h4>Closure Report</h4><p>Verdict: <span class="badge ${c.verdict==='True Positive'?'badge-tp':(c.verdict==='False Positive'?'badge-fp':'badge-esc')}">${c.verdict}</span></p><hr style="border:0; border-top:1px solid #30363d; margin:15px 0;"><p style="font-size:0.85rem;">${c.report}</p></div>`;
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
        if(k == 1) { showToast("AUTHORITY ERROR: MUST ESCALATE!", "auth"); return; }
        const v = document.getElementById('vSel').value;
        if(v === "False Positive") { showToast("LOGIC ERROR: Attack confirmed.", "logic"); return; }
        cases[k].status = 'closed'; cases[k].verdict = v;
        cases[k].report = document.getElementById('aNotes').value || "Resolved via IR playbook.";
        showToast("Case Resolved."); renderTable(); closePanel();
    }

    function doEsc(k) {
        if(!checkValid(k)) { showToast("ESC FAILED: Invalid artifacts.", "art"); return; }
        const v = document.getElementById('vSel').value;
        if(v === "False Positive") { showToast("LOGIC ERROR: Benign cases cannot be escalated.", "logic"); return; }
        cases[k].status = 'closed'; cases[k].verdict = "Escalated";
        cases[k].report = "Escalated to Tier-2. Verdict: " + v;
        showToast("Escalated to L2."); renderTable(); closePanel();
    }

    function switchTab(t) { activeTab = t; document.getElementById('tab-open-btn').classList.toggle('active', t==='open'); document.getElementById('tab-closed-btn').classList.toggle('active', t==='closed'); renderTable(); }
    function updateStats() { document.getElementById('count-open').innerText = Object.values(cases).filter(c => c.status === 'open').length; document.getElementById('count-closed').innerText = Object.values(cases).filter(c => c.status === 'closed').length; }
    function closePanel() { document.getElementById('sidePanel').classList.remove('panel-active'); }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }
    window.onload = () => { renderTable(); };
</script>
