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

    /* --- INITIAL DISCLAIMER MODAL --- */
    .disclaimer-modal {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background: rgba(1, 4, 9, 0.96); z-index: 10000;
        display: none; align-items: center; justify-content: center; backdrop-filter: blur(15px);
    }
    .disclaimer-box { 
        background: #0d1117; border: 1px solid #30363d; width: 700px; 
        border-radius: 12px; overflow: hidden; box-shadow: 0 25px 50px rgba(0,0,0,0.5);
        animation: slideUp 0.4s ease-out;
    }
    .disclaimer-header { background: #161b22; color: #58a6ff; padding: 20px; font-family: 'JetBrains Mono'; font-weight: bold; text-align: center; border-bottom: 1px solid #30363d; }
    .disclaimer-body { padding: 30px; display: flex; gap: 30px; line-height: 1.6; }
    .btn-ack { 
        width: 100%; padding: 18px; background: #238636; color: #fff; border: none; 
        font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; transition: 0.2s;
    }
    .btn-ack:hover { background: #2ea043; }
    .btn-ack:active { transform: scale(0.98); }

    /* --- SEARCH BAR --- */
    .siem-header { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; align-items: center; }
    .search-input-group { flex: 1; display: flex; background: #010409; border: 1px solid #30363d; border-radius: 6px; overflow: hidden; }
    .prompt-label { background: #161b22; color: #58a6ff; padding: 10px 18px; font-family: 'JetBrains Mono'; font-size: 0.8rem; border-right: 1px solid #30363d; }
    #searchInput { background: transparent; border: none; color: #e6edf3; padding: 10px 15px; width: 100%; font-family: 'JetBrains Mono'; font-size: 0.85rem; outline: none; }
    .btn-search { background: #238636; border: none; color: #fff; padding: 0 25px; cursor: pointer; transition: 0.2s; }
    .btn-search:hover { background: #2ea043; }

    /* --- LAYOUT --- */
    .siem-layout { display: flex; flex: 1; overflow: hidden; }
    .siem-sidebar { width: 260px; background: #0d1117; border-right: 1px solid #30363d; padding: 20px; overflow-y: auto; }
    .sidebar-title { font-size: 0.7rem; color: #8b949e; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 15px; border-bottom: 1px solid #21262d; padding-bottom: 5px; }
    
    .field-link { display: flex; justify-content: space-between; font-size: 0.75rem; padding: 8px 10px; color: #58a6ff; cursor: pointer; border-radius: 4px; transition: 0.2s; }
    .field-link:hover { background: rgba(88, 166, 255, 0.1); color: #fff; }

    .siem-results { flex: 1; background: #010409; display: flex; flex-direction: column; overflow: hidden; }
    .results-info { background: #0d1117; padding: 10px 25px; border-bottom: 1px solid #30363d; font-size: 0.75rem; color: #8b949e; display: flex; justify-content: space-between; }
    
    .table-container { overflow-y: auto; flex: 1; }
    .log-table { width: 100%; border-collapse: collapse; font-size: 0.75rem; table-layout: fixed; }
    .log-table th { background: #161b22; color: #8b949e; text-align: left; padding: 12px; position: sticky; top: 0; border-bottom: 1px solid #30363d; z-index: 10; }
    .log-table td { padding: 10px 12px; border-bottom: 1px solid #21262d; font-family: 'JetBrains Mono', monospace; color: #c9d1d9; vertical-align: top; }
    .log-table tr:hover { background: rgba(88, 166, 255, 0.04); cursor: pointer; }

    /* Popover Info Row */
    .info-row { background: #0d1117 !important; color: #8b949e; font-size: 0.7rem; display: none; }
    .info-content { padding: 15px; border-left: 3px solid #58a6ff; }

    /* Badges */
    .lvl { font-weight: bold; font-size: 0.65rem; border: 1px solid; border-radius: 3px; padding: 2px 6px; margin-right: 8px; }
    .lvl-critical { color: #f85149; border-color: rgba(248,81,73,0.4); }
    .lvl-warn { color: #d29922; border-color: rgba(210,153,34,0.4); }
    .lvl-info { color: #58a6ff; border-color: rgba(88,166,255,0.4); }

    /* --- SURROUNDING MODAL --- */
    .modal-surround { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.85); z-index: 9999; display: none; align-items: center; justify-content: center; backdrop-filter: blur(8px); }
    .modal-content { background: #0d1117; border: 1px solid #30363d; width: 90%; height: 85%; border-radius: 12px; display: flex; flex-direction: column; overflow: hidden; box-shadow: 0 30px 60px rgba(0,0,0,0.8); }
    .modal-header { padding: 15px 25px; background: #161b22; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .modal-body { flex: 1; overflow-y: auto; padding: 25px; font-family: 'JetBrains Mono'; font-size: 0.75rem; line-height: 1.8; color: #8b949e; }
    .active-log { background: rgba(88, 166, 255, 0.08); color: #fff; border-left: 4px solid #58a6ff; padding-left: 15px; }

    /* Modern Close Button (Bottom Right) */
    .surround-footer { padding: 15px 25px; background: #0d1117; border-top: 1px solid #30363d; display: flex; justify-content: flex-end; }
    .btn-close-modal { 
        background: #30363d; color: #fff; border: none; padding: 10px 25px; 
        border-radius: 4px; font-family: 'JetBrains Mono'; font-weight: bold; cursor: pointer; transition: 0.2s;
    }
    .btn-close-modal:hover { background: #f85149; }
    .btn-close-modal:active { transform: scale(0.95); }

    @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
    .hl-id { color: #f85149; font-weight: bold; }
</style>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">SENTINELGUARD LOG ANALYTICS v4.2</div>
        <div class="disclaimer-body">
            <div class="lang-section">
                <h3>[TR] ANALİZ REHBERİ</h3>
                <p>Loglara tıklayarak teknik detayları görebilir, yan paneldeki alanlara basarak hızlı filtreleme yapabilirsiniz.</p>
            </div>
            <div class="v-divider"></div>
            <div class="lang-section">
                <h3>[EN] ANALYSIS GUIDE</h3>
                <p>Click on logs for insights, use sidebar fields for filtering, and use the 🔍 icon for correlation analysis.</p>
            </div>
        </div>
        <button class="btn-ack" onclick="closeModal('welcomeModal')">INITIALIZE TERMINAL</button>
    </div>
</div>

<div class="siem-wrapper">
    <div class="siem-header">
        <div class="search-input-group">
            <div class="prompt-label">index=security | search</div>
            <input type="text" id="searchInput" placeholder="Enter keywords or filter criteria..." onkeyup="filterLogs()">
            <button class="btn-search" onclick="filterLogs()">SEARCH</button>
        </div>
    </div>

    <div class="siem-layout">
        <div class="siem-sidebar">
            <h4 class="sidebar-title">Quick Filters</h4>
            <div class="field-link" onclick="applyFieldFilter('EventID=4625')">EventID=4625 <span class="field-count">Failed</span></div>
            <div class="field-link" onclick="applyFieldFilter('EventID=4624')">EventID=4624 <span class="field-count">Success</span></div>
            <div class="field-link" onclick="applyFieldFilter('adm_emir')">User=adm_emir <span class="field-count">Admin</span></div>
            <div class="field-link" onclick="applyFieldFilter('DENY')">Action=DENY <span class="field-count">Block</span></div>
            <div class="field-link" onclick="applyFieldFilter('sqlmap')">Agent=sqlmap <span class="field-count">Attack</span></div>
            
            <h4 class="sidebar-title" style="margin-top:25px;">Systems</h4>
            <div class="field-link" onclick="applyFieldFilter('DC-01')">host=DC-01</div>
            <div class="field-link" onclick="applyFieldFilter('FW-HQ')">host=FW-HQ</div>
        </div>

        <div class="siem-results">
            <div class="results-info">
                <span id="resultCount">Syncing telemetry...</span>
                <span>Forensic View: Active</span>
            </div>
            <div class="table-container">
                <table class="log-table">
                    <thead>
                        <tr>
                            <th width="40">#</th>
                            <th width="170">_time</th>
                            <th width="140">host</th>
                            <th>_raw (Click for Insight)</th>
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
            <span>FORENSIC CONTEXT: Surrounding Events (-5s / +5s)</span>
        </div>
        <div class="modal-body" id="surroundBody"></div>
        <div class="surround-footer">
            <button class="btn-close-modal" onclick="closeSurround()">CLOSE SOURCE VIEW</button>
        </div>
    </div>
</div>

<script>
    const users = ["adm_emir", "svc_sql", "system", "jdoe", "msmith", "root"];
    let allLogs = [];

    function rand(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }

    function generateEngine() {
        for(let i=0; i<400; i++) {
            let d = new Date(); d.setSeconds(d.getSeconds() - (i * 20));
            const ip = `${rand(10,192)}.${rand(0,255)}.${rand(0,255)}.${rand(2,254)}`;
            const rawTemplates = [
                `EventID=${rand(1000,9000)} SourceIP=${ip} User=${users[rand(0,5)]} Action=Allow`,
                `SecurityLog: Logon Failure Account=${users[rand(0,5)]} IP=${ip} Reason=BadPassword`,
                `Firewall: DENY TCP ${ip} -> 10.0.0.5 Port=${rand(80,443)}`,
                `WebServer: ${ip} - GET /index.html 200`
            ];
            allLogs.push({ time: d.toISOString().replace('T', ' ').substring(0, 19), lvl: i % 15 === 0 ? "WARN" : "INFO", host: "HOST-"+rand(1,5), raw: rawTemplates[rand(0,3)], info: "Baseline activity detected. No immediate action required." });
        }

        // Evidence Logs
        const evidence = [
            { lvl: "CRITICAL", host: "FINANCE-SRV-01", raw: "#SOC-5092 | Malware: Ransomware | Process: tasksche.exe | Encrypting: Budget.xlsx.locky | Target: 10.20.5.100", info: "ALERT: Host-based ransomware pattern detected. Unusually high entropy detected in file write operations." },
            { lvl: "WARN", host: "DC-01", raw: "#SOC-1022 | Bruteforce Detection | Account: adm_emir | FailureCode: 0xc000006d | SourceIP: 192.168.1.150", info: "SUSPICIOUS: Repeated failed login attempts against a Domain Admin account. Originating from internal workstation." },
            { lvl: "ERROR", host: "WEB-SRV-PROD", raw: "#SOC-883 | SQLi Injection | src: 172.16.45.10 | payload: 'UNION SELECT' | Agent: sqlmap/1.4.7", info: "EXPLOIT: External IP attempting SQL injection via web application parameters. WAF rules triggered." }
        ];
        evidence.forEach(e => {
            let d = new Date(); d.setMinutes(d.getMinutes() - rand(10, 50));
            allLogs.push({ time: d.toISOString().replace('T', ' ').substring(0, 19), lvl: e.lvl, host: e.host, raw: e.raw, info: e.info });
        });
        allLogs.sort((a,b) => new Date(b.time) - new Date(a.time));
    }

    function render(data) {
        const body = document.getElementById('logsBody');
        body.innerHTML = data.map((l, i) => `
            <tr onclick="toggleInfo(${i})">
                <td style="color:#8b949e; cursor:pointer;" onclick="event.stopPropagation(); showSurround(${i})">🔍</td>
                <td style="color:#8b949e">${l.time}</td>
                <td style="color:#d29922">${l.host}</td>
                <td><span class="lvl lvl-${l.lvl.toLowerCase()}">${l.lvl}</span> ${l.raw.replace(/(#SOC-\d+|tasksche|locky|sqlmap)/gi, '<span class="hl-id">$1</span>')}</td>
            </tr>
            <tr id="info-${i}" class="info-row"><td colspan="4"><div class="info-content"><strong>Analyst Insight:</strong> ${l.info}</div></td></tr>
        `).join('');
        document.getElementById('resultCount').innerText = `${data.length} events indexed`;
    }

    function toggleInfo(i) {
        const row = document.getElementById(`info-${i}`);
        row.style.display = (row.style.display === 'table-row') ? 'none' : 'table-row';
    }

    function applyFieldFilter(val) {
        document.getElementById('searchInput').value = val;
        filterLogs();
    }

    function showSurround(index) {
        const body = document.getElementById('surroundBody');
        body.innerHTML = "";
        const start = Math.max(0, index - 5);
        const end = Math.min(allLogs.length - 1, index + 5);
        for(let i=end; i>=start; i--) {
            const l = allLogs[i];
            body.innerHTML += `<div class="${i === index ? 'active-log' : ''}">[${l.time}] host=${l.host} msg="${l.raw}"</div>`;
        }
        document.getElementById('surroundModal').style.display = 'flex';
    }

    function filterLogs() {
        const v = document.getElementById('searchInput').value.toLowerCase();
        render(allLogs.filter(l => l.raw.toLowerCase().includes(v) || l.host.toLowerCase().includes(v)));
    }

    function closeSurround() { document.getElementById('surroundModal').style.display = 'none'; }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }

    window.onload = () => {
        document.getElementById('welcomeModal').style.display = 'flex';
        generateEngine();
        render(allLogs);
    };
</script>
