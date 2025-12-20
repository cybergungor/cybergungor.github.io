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

    /* --- INITIAL SIMULATION DISCLAIMER --- */
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
    .v-divider { width: 1px; background: #30363d; }
    .lang-section h3 { font-size: 0.8rem; color: #58a6ff; margin-bottom: 10px; font-family: 'JetBrains Mono'; }
    .lang-section p { font-size: 0.85rem; color: #8b949e; }
    
    .btn-ack { 
        width: 100%; padding: 18px; background: #238636; color: #fff; border: none; 
        font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; transition: 0.2s;
    }
    .btn-ack:hover { background: #2ea043; }

    /* --- SEARCH BAR (DARK SIEM STYLE) --- */
    .siem-header { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; align-items: center; }
    .search-input-group { 
        flex: 1; display: flex; background: #010409; border: 1px solid #30363d; 
        border-radius: 6px; overflow: hidden; transition: border-color 0.3s;
    }
    .search-input-group:focus-within { border-color: #58a6ff; box-shadow: 0 0 0 3px rgba(88, 166, 255, 0.1); }
    .prompt-label { background: #161b22; color: #58a6ff; padding: 10px 18px; font-family: 'JetBrains Mono'; font-size: 0.8rem; border-right: 1px solid #30363d; }
    #searchInput { background: transparent; border: none; color: #e6edf3; padding: 10px 15px; width: 100%; font-family: 'JetBrains Mono'; font-size: 0.85rem; outline: none; }
    .btn-search { background: #238636; border: none; color: #fff; padding: 0 25px; cursor: pointer; transition: 0.2s; font-weight: bold; }

    /* --- LAYOUT --- */
    .siem-layout { display: flex; flex: 1; overflow: hidden; }
    .siem-sidebar { width: 260px; background: #0d1117; border-right: 1px solid #30363d; padding: 20px; overflow-y: auto; }
    .sidebar-title { font-size: 0.7rem; color: #8b949e; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 15px; border-bottom: 1px solid #21262d; padding-bottom: 5px; }
    .field-link { display: flex; justify-content: space-between; font-size: 0.75rem; padding: 8px 10px; color: #58a6ff; cursor: pointer; border-radius: 4px; transition: 0.2s; }
    .field-link:hover { background: rgba(88, 166, 255, 0.1); }
    .field-values { display: none; background: #161b22; margin-top: 5px; border-radius: 4px; padding: 5px; border: 1px solid #30363d; }
    .value-item { display: flex; justify-content: space-between; font-size: 0.7rem; padding: 4px 8px; color: #8b949e; cursor: pointer; }

    .siem-results { flex: 1; background: #010409; display: flex; flex-direction: column; overflow: hidden; }
    .results-info { background: #0d1117; padding: 10px 25px; border-bottom: 1px solid #30363d; font-size: 0.75rem; color: #8b949e; display: flex; justify-content: space-between; }
    
    .table-container { overflow-y: auto; flex: 1; }
    .log-table { width: 100%; border-collapse: collapse; font-size: 0.75rem; table-layout: fixed; }
    .log-table th { background: #161b22; color: #8b949e; text-align: left; padding: 12px; position: sticky; top: 0; border-bottom: 1px solid #30363d; z-index: 10; }
    .log-table td { padding: 10px 12px; border-bottom: 1px solid #21262d; font-family: 'JetBrains Mono', monospace; color: #c9d1d9; vertical-align: top; }
    .log-table tr:hover { background: rgba(88, 166, 255, 0.04); cursor: pointer; }

    .lvl { font-weight: bold; font-size: 0.65rem; border: 1px solid; border-radius: 3px; padding: 2px 6px; margin-right: 8px; }
    .lvl-critical { color: #f85149; border-color: rgba(248,81,73,0.4); }
    .lvl-warn { color: #d29922; border-color: rgba(210,153,34,0.4); }
    .lvl-info { color: #58a6ff; border-color: rgba(88,166,255,0.4); }

    /* --- SURROUNDING MODAL --- */
    .modal-surround { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.85); z-index: 9999; display: none; align-items: center; justify-content: center; backdrop-filter: blur(8px); }
    .modal-content { background: #0d1117; border: 1px solid #30363d; width: 90%; height: 85%; border-radius: 12px; display: flex; flex-direction: column; overflow: hidden; }
    .modal-header { padding: 15px 25px; background: #161b22; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .active-log { background: rgba(88, 166, 255, 0.08); color: #fff; border-left: 4px solid #58a6ff; padding-left: 15px; }
    .hl-id { color: #f85149; font-weight: bold; }

    @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
</style>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">LOG ANALYSIS TERMINAL // V5.2 SENTINEL</div>
        <div class="disclaimer-body">
            <div class="lang-section">
                <h3>[TR] SİMÜLASYON MODU</h3>
                <p>Bu panel profesyonel SIEM ürünlerinin (Splunk vb.) replikasıdır. Vakaları çözmek için arama çubuğunu ve dinamik filtreleri kullanın.</p>
            </div>
            <div class="v-divider"></div>
            <div class="lang-section">
                <h3>[EN] SIMULATION MODE</h3>
                <p>High-fidelity SIEM replication. Use the search bar and dynamic sidebar filters to isolate forensic evidence for your cases.</p>
            </div>
        </div>
        <button class="btn-ack" onclick="closeModal('welcomeModal')">START LOG ANALYSIS</button>
    </div>
</div>

<div class="siem-wrapper">
    <div class="siem-header">
        <div class="search-input-group">
            <div class="prompt-label">index=security | search</div>
            <input type="text" id="searchInput" placeholder="Search by Case ID (#SOC-5092), IP, or Keyword..." onkeyup="if(event.key==='Enter')filterLogs()">
            <button class="btn-search" onclick="filterLogs()">SEARCH</button>
        </div>
    </div>

    <div class="siem-layout">
        <div class="siem-sidebar">
            <div class="sidebar-title">Dimensions</div>
            <div class="field-group">
                <div class="field-link" onclick="toggleField('host')">host</div>
                <div class="field-values" id="vals-host"></div>
            </div>
            <div class="field-group">
                <div class="field-link" onclick="toggleField('User')">User</div>
                <div class="field-values" id="vals-User"></div>
            </div>
            <div class="field-group">
                <div class="field-link" onclick="toggleField('Action')">Action</div>
                <div class="field-values" id="vals-Action"></div>
            </div>
        </div>

        <div class="siem-results">
            <div class="results-info">
                <span id="resultCount">Querying forensic vault...</span>
                <span>Sorted by: _time descending</span>
            </div>
            <div class="table-container">
                <table class="log-table">
                    <thead>
                        <tr>
                            <th width="40"></th>
                            <th width="170">_time</th>
                            <th width="140">host</th>
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
            <span style="font-family:'JetBrains Mono'; color:#58a6ff; font-weight:bold;">SOURCE VIEW: Surrounding Events</span>
            <span style="cursor:pointer; color:#8b949e;" onclick="closeSurround()">✕</span>
        </div>
        <div id="surroundBody" style="flex:1; overflow-y:auto; padding:25px; font-family:'JetBrains Mono'; font-size:0.75rem; line-height:1.8; color:#8b949e;"></div>
        <div style="padding:15px; border-top:1px solid #30363d; text-align:right;">
            <button class="btn-search" style="background:#30363d" onclick="closeSurround()">CLOSE SOURCE</button>
        </div>
    </div>
</div>

<script>
    const hosts = ["DC-01", "SQL-SRV-PROD", "FINANCE-PC", "WEB-CLUSTER-01", "MAIL-GATEWAY", "FIREWALL-HQ"];
    const users = ["adm_emir", "svc_sql", "system", "jdoe", "msmith", "root", "apache"];
    
    let allLogs = [];

    function rand(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }

    function generateEngine() {
        // 500+ random background noise logs
        for(let i=0; i<500; i++) {
            let d = new Date(); d.setSeconds(d.getSeconds() - (i * rand(2, 60)));
            const ip = `${rand(10,192)}.${rand(0,255)}.${rand(0,255)}.${rand(2,254)}`;
            const host = hosts[rand(0,5)];
            const user = users[rand(0,6)];
            const action = rand(0,1) ? 'Allow' : 'Deny';
            
            const raw = `EventID=${rand(1000,9999)} Source_IP=${ip} user=${user} action=${action} proto=TCP port=${rand(22,65535)} msg="Standard telemetry stream"`;
            allLogs.push({ time: d.toISOString().replace('T', ' ').substring(0, 19), lvl: rand(0,10) > 8 ? "WARN" : "INFO", host, raw, user, action });
        }

        // Evidence Injection (Matching Alerts)
        const evidence = [
            { lvl: "CRITICAL", host: "FINANCE-SRV-01", raw: "#SOC-5092 | Malware: Ransomware | Process: tasksche.exe | Encrypting: Budget.xlsx.locky | Target: 10.20.5.100", user: "system", action: "Deny" },
            { lvl: "WARN", host: "DC-01", raw: "#SOC-1022 | Bruteforce Detection | Account: adm_emir | FailureCode: 0xc000006d | SourceIP: 192.168.1.150", user: "adm_emir", action: "Block" },
            { lvl: "ERROR", host: "WEB-SRV-PROD", raw: "#SOC-883 | SQLi Injection | src: 172.16.45.10 | payload: 'UNION SELECT table_name' | Agent: sqlmap/1.4.7", user: "apache", action: "Block" }
        ];

        evidence.forEach(e => {
            let d = new Date(); d.setMinutes(d.getMinutes() - rand(5, 60));
            allLogs.push({ time: d.toISOString().replace('T', ' ').substring(0, 19), lvl: e.lvl, host: e.host, raw: e.raw, user: e.user, action: e.action });
        });

        allLogs.sort((a,b) => new Date(b.time) - new Date(a.time));
    }

    function render(data) {
        document.getElementById('logsBody').innerHTML = data.map((l, i) => `
            <tr>
                <td style="color:#8b949e; cursor:pointer;" onclick="showSurround(${i})">🔍</td>
                <td style="color:#8b949e">${l.time}</td>
                <td style="color:#d29922">${l.host}</td>
                <td><span class="lvl lvl-${l.lvl.toLowerCase()}">${l.lvl}</span> ${l.raw.replace(/(#SOC-\d+|tasksche|locky|sqlmap|0xc000006d)/gi, '<span class="hl-id">$1</span>')}</td>
            </tr>
        `).join('');
        document.getElementById('resultCount').innerText = `${data.length} events indexed`;
    }

    function toggleField(fieldName) {
        const panel = document.getElementById(`vals-${fieldName}`);
        const isVisible = panel.style.display === 'block';
        document.querySelectorAll('.field-values').forEach(p => p.style.display = 'none');
        if (!isVisible) {
            panel.style.display = 'block';
            const stats = {};
            allLogs.forEach(l => {
                let val = (fieldName === 'host') ? l.host : (fieldName === 'User') ? l.user : l.action;
                stats[val] = (stats[val] || 0) + 1;
            });
            panel.innerHTML = Object.entries(stats).sort((a,b)=>b[1]-a[1]).slice(0,5).map(([v,c])=>`
                <div class="value-item" onclick="applyFilter('${v}')"><span>${v}</span><span style="color:#3fb950">${c}</span></div>
            `).join('');
        }
    }

    function applyFilter(v) { document.getElementById('searchInput').value = v; filterLogs(); }

    function filterLogs() {
        const v = document.getElementById('searchInput').value.toLowerCase();
        render(allLogs.filter(l => l.raw.toLowerCase().includes(v) || l.host.toLowerCase().includes(v)));
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

    window.onload = () => {
        document.getElementById('welcomeModal').style.display = 'flex';
        generateEngine();
        render(allLogs);
    };
</script>
