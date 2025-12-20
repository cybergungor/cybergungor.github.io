---
layout: default
title: SentinelGuard - Advanced Log Analytics (SIEM)
permalink: /logs/
---

<style>
    /* Full Screen & Theme Overrides */
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    header, .navbar { max-width: 100% !important; border-bottom: 1px solid #30363d; }

    .siem-wrapper { font-family: 'Inter', -apple-system, sans-serif; color: #e6edf3; display: flex; flex-direction: column; height: calc(100vh - 60px); }

    /* --- INITIAL SIMULATION DISCLAIMER (BİLGİLENDİRME EKRANI) --- */
    .disclaimer-modal {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background: rgba(1, 4, 9, 0.96); z-index: 10000;
        display: none; align-items: center; justify-content: center; backdrop-filter: blur(15px);
    }
    .disclaimer-box { 
        background: #0d1117; border: 1px solid #30363d; width: 750px; 
        border-radius: 12px; overflow: hidden; box-shadow: 0 25px 50px rgba(0,0,0,0.5);
        animation: slideUp 0.4s ease-out;
    }
    .disclaimer-header { background: #161b22; color: #58a6ff; padding: 20px; font-family: 'JetBrains Mono'; font-weight: bold; text-align: center; border-bottom: 1px solid #30363d; }
    .disclaimer-body { padding: 30px; display: flex; gap: 30px; line-height: 1.6; }
    .v-divider { width: 1px; background: #30363d; }
    .lang-section h3 { font-size: 0.8rem; color: #58a6ff; margin-bottom: 12px; font-family: 'JetBrains Mono'; border-bottom: 1px solid #30363d; padding-bottom: 5px; }
    .lang-section p { font-size: 0.85rem; color: #8b949e; margin-bottom: 10px; }
    .lang-section ul { padding-left: 15px; color: #8b949e; font-size: 0.8rem; }
    
    .btn-ack { 
        width: 100%; padding: 18px; background: #238636; color: #fff; border: none; 
        font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; transition: 0.2s;
    }
    .btn-ack:hover { background: #2ea043; }
    .btn-ack:active { transform: scale(0.98); }

    /* --- SEARCH BAR (MODERN DARK SIEM) --- */
    .siem-header { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; align-items: center; }
    .search-input-group { flex: 1; display: flex; background: #010409; border: 1px solid #30363d; border-radius: 6px; overflow: hidden; transition: 0.3s; }
    .search-input-group:focus-within { border-color: #58a6ff; box-shadow: 0 0 0 3px rgba(88, 166, 255, 0.1); }
    .prompt-label { background: #161b22; color: #58a6ff; padding: 10px 18px; font-family: 'JetBrains Mono'; font-size: 0.8rem; border-right: 1px solid #30363d; }
    #searchInput { background: transparent; border: none; color: #e6edf3; padding: 10px 15px; width: 100%; font-family: 'JetBrains Mono'; font-size: 0.85rem; outline: none; }
    .btn-search { background: #238636; border: none; color: #fff; padding: 0 25px; cursor: pointer; font-weight: bold; transition: 0.2s; }

    /* --- LAYOUT & SIDEBAR --- */
    .siem-layout { display: flex; flex: 1; overflow: hidden; }
    .siem-sidebar { width: 280px; background: #0d1117; border-right: 1px solid #30363d; padding: 15px; overflow-y: auto; }
    .sidebar-title { font-size: 0.7rem; color: #8b949e; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 10px; font-weight: bold; }
    
    .field-group { margin-bottom: 8px; }
    .field-link { display: flex; justify-content: space-between; font-size: 0.75rem; padding: 8px 10px; color: #e6edf3; cursor: pointer; border-radius: 4px; transition: 0.2s; }
    .field-link:hover { background: rgba(255, 255, 255, 0.05); color: #58a6ff; }
    .field-values { display: none; background: #161b22; margin-top: 5px; border-radius: 4px; padding: 5px; border: 1px solid #30363d; }
    .value-item { display: flex; justify-content: space-between; font-size: 0.7rem; padding: 4px 8px; color: #8b949e; cursor: pointer; border-radius: 3px; }
    .value-item:hover { color: #58a6ff; background: #21262d; }

    /* --- RESULTS TABLE --- */
    .siem-results { flex: 1; background: #010409; display: flex; flex-direction: column; overflow: hidden; }
    .results-info { background: #0d1117; padding: 10px 25px; border-bottom: 1px solid #30363d; font-size: 0.75rem; color: #8b949e; display: flex; justify-content: space-between; }
    .table-container { overflow-y: auto; flex: 1; }
    .log-table { width: 100%; border-collapse: collapse; font-size: 0.75rem; table-layout: fixed; }
    .log-table th { background: #161b22; color: #8b949e; text-align: left; padding: 12px; position: sticky; top: 0; border-bottom: 1px solid #30363d; z-index: 10; font-weight: 600; }
    .log-table td { padding: 10px 12px; border-bottom: 1px solid #21262d; font-family: 'JetBrains Mono', monospace; color: #c9d1d9; vertical-align: top; }
    .log-table tr:hover { background: rgba(88, 166, 255, 0.04); cursor: pointer; }

    /* Info Popover */
    .info-row { background: #0d1117 !important; display: none; }
    .info-content { padding: 15px; border-left: 3px solid #58a6ff; color: #8b949e; font-size: 0.7rem; line-height: 1.6; }

    /* Badges & Highlights */
    .lvl { font-weight: bold; font-size: 0.65rem; border: 1px solid; border-radius: 3px; padding: 2px 6px; margin-right: 8px; }
    .lvl-critical { color: #f85149; border-color: rgba(248, 81, 73, 0.4); }
    .lvl-warn { color: #d29922; border-color: rgba(210, 153, 34, 0.4); }
    .lvl-info { color: #58a6ff; border-color: rgba(88, 166, 255, 0.4); }
    .hl-id { color: #f85149; font-weight: bold; }

    /* --- SURROUNDING MODAL --- */
    .modal-surround { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.85); z-index: 9999; display: none; align-items: center; justify-content: center; backdrop-filter: blur(8px); }
    .modal-content { background: #0d1117; border: 1px solid #30363d; width: 90%; height: 85%; border-radius: 12px; display: flex; flex-direction: column; overflow: hidden; box-shadow: 0 30px 60px rgba(0,0,0,0.8); }
    .modal-header { padding: 15px 25px; background: #161b22; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .modal-footer { padding: 15px 25px; background: #0d1117; border-top: 1px solid #30363d; display: flex; justify-content: flex-end; }
    .btn-close-modal { background: #30363d; color: #fff; border: none; padding: 10px 25px; border-radius: 4px; font-weight: bold; cursor: pointer; transition: 0.2s; }
    .btn-close-modal:hover { background: #f85149; }
    
    .active-log { background: rgba(88, 166, 255, 0.08); color: #fff; border-left: 4px solid #58a6ff; padding-left: 15px; border-radius: 0 4px 4px 0; }
    @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
</style>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">SENTINEL GUARD // SYSTEM ACCESS v5.0.2</div>
        <div class="disclaimer-body">
            <div class="lang-section">
                <h3>[TR] SİMÜLASYON REHBERİ</h3>
                <p>Bu panel, gerçek dünya <strong>SIEM</strong> (Splunk, QRadar vb.) arayüzlerinin profesyonel bir simülasyonudur.</p>
                <ul>
                    <li><strong>Arama:</strong> Case ID (#SOC-5092) veya IP kullanarak filtreleme yapın.</li>
                    <li><strong>Detay:</strong> Loglara tıklayarak teknik analiz notlarını okuyun.</li>
                    <li><strong>Bağlam:</strong> 🔍 ikonuna basarak olay anının 10sn öncesini/sonrasını görün.</li>
                </ul>
            </div>
            <div class="v-divider"></div>
            <div class="lang-section">
                <h3>[EN] SIMULATION GUIDE</h3>
                <p>This dashboard is a high-fidelity replica of real-world <strong>SIEM</strong> platforms.</p>
                <ul>
                    <li><strong>Search:</strong> Use Case IDs or technical artifacts to hunt for threats.</li>
                    <li><strong>Insight:</strong> Click on any row to expand Analyst Insight notes.</li>
                    <li><strong>Correlation:</strong> Use 🔍 for Surrounding Events (Correlation Analysis).</li>
                </ul>
            </div>
        </div>
        <button class="btn-ack" onclick="closeModal('welcomeModal')">INITIALIZE THREAT HUNTING SESSION</button>
    </div>
</div>

<div class="siem-wrapper">
    <div class="siem-header">
        <div class="search-input-group">
            <div class="prompt-label">index=security | search</div>
            <input type="text" id="searchInput" placeholder="Search Case ID, IP, Host or Process name..." onkeyup="if(event.key==='Enter')filterLogs()">
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
                <span id="resultCount">Querying telemetry vault...</span>
                <span>Sorted by: Time Descending</span>
            </div>
            <div class="table-container">
                <table class="log-table">
                    <thead>
                        <tr>
                            <th width="40"></th>
                            <th width="170">_time</th>
                            <th width="140">host</th>
                            <th>_raw (Event Data)</th>
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
            <span style="font-family:'JetBrains Mono'; color:#58a6ff; font-weight:bold;">SOURCE VIEW: Surrounding Events (-5s / +5s)</span>
        </div>
        <div id="surroundBody" class="modal-body"></div>
        <div class="modal-footer">
            <button class="btn-close-modal" onclick="closeSurround()">CLOSE ANALYZER</button>
        </div>
    </div>
</div>

<script>
    const config = {
        hosts: ["DC-01", "SQL-SRV-PROD", "FINANCE-PC", "WEB-CLUSTER-01", "MAIL-GATEWAY", "FW-HQ", "CEO-LAPTOP", "JUMP-HOST"],
        users: ["adm_emir", "svc_sql", "system", "jdoe", "msmith", "root", "apache", "backup_svc"],
        actions: ["Allow", "Deny", "Block", "Accept"]
    };
    
    let allLogs = [];

    function rand(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }

    function generateEngine() {
        // 1. Generate 500+ random background noise logs
        for(let i=0; i<500; i++) {
            let d = new Date(); d.setSeconds(d.getSeconds() - (i * rand(5, 50)));
            const ip = `${rand(10,192)}.${rand(0,255)}.${rand(0,255)}.${rand(2,254)}`;
            const host = config.hosts[rand(0, config.hosts.length - 1)];
            const user = config.users[rand(0, config.users.length - 1)];
            const action = config.actions[rand(0, 3)];

            const raw = `EventID=${rand(1000,9999)} src_ip=${ip} user=${user} action=${action} proto=TCP port=${rand(22,65535)} msg="Standard telemetry heartbeat"`;
            allLogs.push({ time: d.toISOString().replace('T', ' ').substring(0, 19), lvl: rand(0,10) > 8 ? "WARN" : "INFO", host, raw, user, action, info: "Routine event logged by local agent. No suspicious patterns identified." });
        }

        // 2. Inject Playbook Evidence (Hidden Artifacts)
        const evidence = [
            { lvl: "CRITICAL", host: "FINANCE-SRV-01", raw: "#SOC-5092 | Malware: Ransomware | Process: tasksche.exe | Encrypting: Budget.xlsx.locky | Target: 10.20.5.100 | Entropy: 7.92", user: "system", action: "Deny", info: "ALERT: Host-based ransomware pattern. Known ransomware process 'tasksche.exe' modifying high-value assets." },
            { lvl: "WARN", host: "DC-01", raw: "#SOC-1022 | NTLM Bruteforce | Target: adm_emir | FailureCode: 0xc000006d | SourceIP: 192.168.1.150 | Type: 3", user: "adm_emir", action: "Block", info: "SUSPICIOUS: 50+ failed logons detected against a privileged account. Source IP matches internal attacker station." },
            { lvl: "ERROR", host: "WEB-SRV-PROD", raw: "#SOC-883 | SQLi Exploit Attempt | src: 172.16.45.10 | payload: 'UNION SELECT table_name' | Agent: sqlmap/1.4.7", user: "apache", action: "Block", info: "EXPLOIT: SQL Injection signature detected. Database schema 'information_schema' was targeted via products.php." }
        ];

        evidence.forEach(e => {
            let d = new Date(); d.setMinutes(d.getMinutes() - rand(5, 90));
            allLogs.push({ time: d.toISOString().replace('T', ' ').substring(0, 19), lvl: e.lvl, host: e.host, raw: e.raw, user: e.user, action: e.action, info: e.info });
        });

        // Mix everything
        allLogs.sort((a,b) => new Date(b.time) - new Date(a.time));
    }

    function render(data) {
        const body = document.getElementById('logsBody');
        body.innerHTML = data.map((l, i) => `
            <tr onclick="toggleInfo(${i})">
                <td style="color:#8b949e; text-align:center;" onclick="event.stopPropagation(); showSurround(${i})">🔍</td>
                <td style="color:#8b949e">${l.time}</td>
                <td style="color:#d29922">${l.host}</td>
                <td><span class="lvl lvl-${l.lvl.toLowerCase()}">${l.lvl}</span> ${l.raw.replace(/(#SOC-\d+|tasksche|locky|sqlmap|0xc000006d)/gi, '<span class="hl-id">$1</span>')}</td>
            </tr>
            <tr id="info-${i}" class="info-row"><td colspan="4"><div class="info-content"><strong>ANALYST INSIGHT:</strong> ${l.info}</div></td></tr>
        `).join('');
        document.getElementById('resultCount').innerText = `${data.length} events indexed`;
    }

    function toggleInfo(i) {
        const row = document.getElementById(`info-${i}`);
        row.style.display = (row.style.display === 'table-row') ? 'none' : 'table-row';
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
