---
layout: default
title: SentinelGuard - Advanced Log Analytics
permalink: /logs/
---

<style>
    /* SIEM Deep Dark Theme */
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    header, .navbar { max-width: 100% !important; border-bottom: 1px solid #30363d; }

    .siem-wrapper { font-family: 'Inter', sans-serif; color: #e6edf3; display: flex; flex-direction: column; height: calc(100vh - 60px); }

    /* --- INITIAL DISCLAIMER MODAL (BİLGİLENDİRME EKRANI) --- */
    .disclaimer-modal {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background: rgba(1, 4, 9, 0.96); z-index: 10000;
        display: none; /* JS ile açılacak */
        align-items: center; justify-content: center; backdrop-filter: blur(15px);
    }
    .disclaimer-box { 
        background: #0d1117; border: 1px solid #30363d; width: 700px; 
        border-radius: 12px; overflow: hidden; box-shadow: 0 25px 50px rgba(0,0,0,0.5);
        animation: slideUp 0.4s ease-out;
    }
    .disclaimer-header { background: #161b22; color: #58a6ff; padding: 20px; font-family: 'JetBrains Mono'; font-weight: bold; text-align: center; border-bottom: 1px solid #30363d; }
    .disclaimer-body { padding: 30px; display: flex; gap: 30px; line-height: 1.6; }
    .v-divider { width: 1px; background: #30363d; }
    .lang-section h3 { font-size: 0.8rem; color: #58a6ff; margin-bottom: 10px; font-family: 'JetBrains Mono'; }
    .lang-section p { font-size: 0.85rem; color: #8b949e; }
    
    .btn-ack { 
        width: 100%; padding: 18px; background: #238636; color: #fff; border: none; 
        font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; transition: 0.2s;
    }
    .btn-ack:hover { background: #2ea043; }
    .btn-ack:active { transform: scale(0.98); }

    /* --- PROFESSIONAL SEARCH BAR (DARK) --- */
    .siem-header { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; align-items: center; }
    .search-input-group { 
        flex: 1; display: flex; background: #010409; border: 1px solid #30363d; 
        border-radius: 6px; overflow: hidden; transition: border-color 0.3s;
    }
    .search-input-group:focus-within { border-color: #58a6ff; box-shadow: 0 0 0 3px rgba(88, 166, 255, 0.1); }
    
    .prompt-label { background: #161b22; color: #58a6ff; padding: 10px 18px; font-family: 'JetBrains Mono'; font-size: 0.8rem; border-right: 1px solid #30363d; }
    #searchInput { background: transparent; border: none; color: #e6edf3; padding: 10px 15px; width: 100%; font-family: 'JetBrains Mono'; font-size: 0.85rem; outline: none; }
    
    .btn-search { background: #238636; border: none; color: #fff; padding: 0 25px; cursor: pointer; transition: 0.2s; }
    .btn-search:hover { background: #2ea043; }
    .btn-search:active { opacity: 0.7; }

    /* --- LAYOUT --- */
    .siem-layout { display: flex; flex: 1; overflow: hidden; }
    .siem-sidebar { width: 260px; background: #0d1117; border-right: 1px solid #30363d; padding: 20px; overflow-y: auto; }
    .sidebar-title { font-size: 0.7rem; color: #8b949e; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 15px; border-bottom: 1px solid #21262d; padding-bottom: 5px; }
    .field-link { display: flex; justify-content: space-between; font-size: 0.75rem; padding: 6px 0; color: #58a6ff; cursor: pointer; }

    .siem-results { flex: 1; background: #010409; display: flex; flex-direction: column; overflow: hidden; }
    .results-info { background: #0d1117; padding: 10px 25px; border-bottom: 1px solid #30363d; font-size: 0.75rem; color: #8b949e; display: flex; justify-content: space-between; }
    
    .table-container { overflow-y: auto; flex: 1; }
    .log-table { width: 100%; border-collapse: collapse; font-size: 0.75rem; table-layout: fixed; }
    .log-table th { background: #161b22; color: #8b949e; text-align: left; padding: 12px; position: sticky; top: 0; border-bottom: 1px solid #30363d; z-index: 10; font-weight: 600; }
    .log-table td { padding: 10px 12px; border-bottom: 1px solid #21262d; font-family: 'JetBrains Mono', monospace; color: #c9d1d9; vertical-align: top; }
    .log-table tr:hover { background: rgba(88, 166, 255, 0.04); cursor: pointer; }

    /* Badges */
    .lvl { font-weight: bold; font-size: 0.65rem; border: 1px solid; border-radius: 3px; padding: 2px 6px; margin-right: 8px; }
    .lvl-critical { color: #f85149; border-color: rgba(248,81,73,0.4); }
    .lvl-warn { color: #d29922; border-color: rgba(210, 153, 34, 0.4); }
    .lvl-info { color: #58a6ff; border-color: rgba(88, 166, 255, 0.4); }

    /* --- SURROUNDING MODAL --- */
    .modal-surround { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.85); z-index: 9999; display: none; align-items: center; justify-content: center; backdrop-filter: blur(8px); }
    .modal-content { background: #0d1117; border: 1px solid #30363d; width: 90%; height: 85%; border-radius: 12px; display: flex; flex-direction: column; overflow: hidden; box-shadow: 0 30px 60px rgba(0,0,0,0.8); }
    .modal-header { padding: 15px 25px; background: #161b22; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .modal-header span { font-size: 0.8rem; font-family: 'JetBrains Mono'; color: #58a6ff; font-weight: bold; }
    .close-icon { color: #8b949e; font-size: 1.5rem; cursor: pointer; transition: 0.2s; width: 32px; height: 32px; display: flex; align-items: center; justify-content: center; border-radius: 50%; }
    .close-icon:hover { color: #fff; background: rgba(255,255,255,0.1); }
    .modal-body { flex: 1; overflow-y: auto; padding: 25px; font-family: 'JetBrains Mono'; font-size: 0.75rem; line-height: 1.8; color: #8b949e; }
    .active-log { background: rgba(88, 166, 255, 0.08); color: #fff; border-left: 4px solid #58a6ff; padding-left: 15px; border-radius: 0 4px 4px 0; }

    @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
    .hl-id { color: #f85149; font-weight: bold; }
</style>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">LOG ANALYSIS TERMINAL // V4.2_SENTINEL</div>
        <div class="disclaimer-body">
            <div class="lang-section">
                <h3>[TR] SİMÜLASYON MODU</h3>
                <p>Bu panel profesyonel SIEM ürünlerinin birebir replikasıdır. Vakaları çözmek için arama çubuğunu kullanarak delillere ulaşın.</p>
            </div>
            <div class="v-divider"></div>
            <div class="lang-section">
                <h3>[EN] SIMULATION MODE</h3>
                <p>High-fidelity SIEM replication. Unfiltered forensic stream. Use the search bar to isolate indicators of compromise (IoC).</p>
            </div>
        </div>
        <button class="btn-ack" onclick="closeModal('welcomeModal')">INITIALIZE DATA STREAM</button>
    </div>
</div>

<div class="siem-wrapper">
    <div class="siem-header">
        <div class="search-input-group">
            <div class="prompt-label">index=security | search</div>
            <input type="text" id="searchInput" placeholder="Search by CaseID, Host, IP, or Artifact..." onkeyup="filterLogs()">
            <button class="btn-search" onclick="filterLogs()">
                <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3"><circle cx="11" cy="11" r="8"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>
            </button>
        </div>
    </div>

    <div class="siem-layout">
        <div class="siem-sidebar">
            <h4 class="sidebar-title">Interesting Fields</h4>
            <div class="field-link">EventID <span class="field-count">42</span></div>
            <div class="field-link">Source_IP <span class="field-count">115</span></div>
            <div class="field-link">User <span class="field-count">14</span></div>
            <div class="field-link">Process <span class="field-count">28</span></div>
        </div>

        <div class="siem-results">
            <div class="results-info">
                <span id="resultCount">Indexing forensic data...</span>
                <span>Real-time Stream: Active</span>
            </div>
            <div class="table-container">
                <table class="log-table">
                    <thead>
                        <tr>
                            <th width="50">#</th>
                            <th width="170">_time</th>
                            <th width="150">host</th>
                            <th>_raw (Security Events)</th>
                        </tr>
                    </thead>
                    <tbody id="logsBody"></tbody>
                </table>
            </div>
        </div>
    </div>
</div>

<div id="surroundModal" class="modal-surround">
    <div class="modal-content">
        <div class="modal-header">
            <span>FORENSIC CONTEXT: Surrounding Events</span>
            <div class="close-icon" onclick="closeSurround()">✕</div>
        </div>
        <div class="modal-body" id="surroundBody"></div>
    </div>
</div>

<script>
    const hosts = ["DC-01", "SQL-SRV-PROD", "FINANCE-PC", "WEB-CLUSTER-01", "MAIL-GATEWAY", "FIREWALL-HQ"];
    const users = ["adm_emir", "svc_sql", "system", "jdoe", "msmith", "root", "apache"];
    
    let allLogs = [];

    function rand(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }

    function generateEngine() {
        for(let i=0; i<400; i++) {
            let d = new Date(); d.setSeconds(d.getSeconds() - (i * rand(2, 60)));
            const ip = `${rand(10,192)}.${rand(0,255)}.${rand(0,255)}.${rand(2,254)}`;
            
            const rawTemplates = [
                `EventID=${rand(1000,9999)} SourceIP=${ip} User=${users[rand(0,6)]} Action=${rand(0,1)?'Allow':'Deny'} Port=${rand(22,65535)}`,
                `SecurityLog: Logon Failure AccountName=${users[rand(0,6)]} Source_IP=${ip} Reason=BadPassword Status=0xc000006d`,
                `SystemLog: Service "${['Defender', 'SQLServer', 'Cron'][rand(0,2)]}" status=RUNNING`,
                `WebServer: ${ip} - GET /api/v1/meta HTTP/1.1 200 ${rand(500,2000)}`
            ];

            allLogs.push({
                time: d.toISOString().replace('T', ' ').substring(0, 19),
                lvl: i % 12 === 0 ? "WARN" : "INFO",
                host: hosts[rand(0,5)],
                raw: rawTemplates[rand(0,3)]
            });
        }

        // --- Vaka Delilleri (Evidence) ---
        const evidence = [
            { lvl: "CRITICAL", host: "FINANCE-SRV-01", raw: "#SOC-5092 | Malware: Ransomware | Process: tasksche.exe | Encrypting: Budget.xlsx.locky | Target: 10.20.5.100 | Entropy: 7.92" },
            { lvl: "WARN", host: "DC-01", raw: "#SOC-1022 | Bruteforce Detection | Account: adm_emir | FailureCode: 0xc000006d | SourceIP: 192.168.1.150 | Type: 3" },
            { lvl: "ERROR", host: "WEB-SRV-PROD", raw: "#SOC-883 | SQLi Injection | src: 172.16.45.10 | payload: 'UNION SELECT table_name' | Agent: sqlmap/1.4.7" }
        ];

        evidence.forEach(e => {
            let d = new Date(); d.setMinutes(d.getMinutes() - rand(5, 60));
            allLogs.push({ time: d.toISOString().replace('T', ' ').substring(0, 19), lvl: e.lvl, host: e.host, raw: e.raw });
        });

        allLogs.sort((a,b) => new Date(b.time) - new Date(a.time));
    }

    function render(data) {
        document.getElementById('logsBody').innerHTML = data.map((l, i) => `
            <tr>
                <td style="color:#8b949e; cursor:pointer; text-align:center;" onclick="showSurround(${i})">🔍</td>
                <td style="color:#8b949e">${l.time}</td>
                <td style="color:#d29922">${l.host}</td>
                <td><span class="lvl lvl-${l.lvl.toLowerCase()}">${l.lvl}</span> ${l.raw.replace(/(#SOC-\d+|tasksche|locky|sqlmap|0xc000006d)/gi, '<span class="hl-id">$1</span>')}</td>
            </tr>
        `).join('');
        document.getElementById('resultCount').innerText = `${data.length} events indexed`;
    }

    function showSurround(index) {
        const body = document.getElementById('surroundBody');
        body.innerHTML = "";
        const start = Math.max(0, index - 5);
        const end = Math.min(allLogs.length - 1, index + 5);
        for(let i=end; i>=start; i--) {
            const l = allLogs[i];
            body.innerHTML += `<div class="${i === index ? 'active-log' : ''}">[${l.time}] host=${l.host} lvl=${l.lvl} msg="${l.raw}"</div>`;
        }
        document.getElementById('surroundModal').style.display = 'flex';
    }

    function closeSurround() { document.getElementById('surroundModal').style.display = 'none'; }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }
    
    function filterLogs() {
        const v = document.getElementById('searchInput').value.toLowerCase();
        render(allLogs.filter(l => l.raw.toLowerCase().includes(v) || l.host.toLowerCase().includes(v)));
    }

    // SAYFA YÜKLENDİĞİNDE ÇALIŞACAK KISIM (ÖNEMLİ!)
    window.onload = () => {
        // Uyarı Ekranını Göster
        document.getElementById('welcomeModal').style.display = 'flex';
        
        // Logları Üret ve Çiz
        generateEngine();
        render(allLogs);
    };
</script>
