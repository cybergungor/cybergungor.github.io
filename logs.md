---
layout: default
title: SentinelGuard - Advanced Log Analytics
permalink: /logs/
---

<style>
    /* Splunk Professional UI Overrides */
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    header, .navbar { max-width: 100% !important; border-bottom: 1px solid #30363d; }

    .siem-wrapper { font-family: 'Inter', sans-serif; color: #e6edf3; display: flex; flex-direction: column; height: calc(100vh - 60px); }

    /* --- INITIAL DISCLAIMER MODAL (BILGILENDIRME EKRANI) --- */
    .disclaimer-modal {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background: rgba(1, 4, 9, 0.98); z-index: 10000;
        display: flex; align-items: center; justify-content: center; backdrop-filter: blur(12px);
    }
    .disclaimer-box { background: #0d1117; border: 1px solid #30363d; width: 750px; border-radius: 8px; overflow: hidden; box-shadow: 0 0 50px rgba(88, 166, 255, 0.1); }
    .disclaimer-header { background: rgba(248, 81, 73, 0.1); color: #f85149; padding: 15px; font-family: 'JetBrains Mono'; font-weight: bold; text-align: center; border-bottom: 1px solid rgba(248, 81, 73, 0.3); }
    .disclaimer-body { padding: 30px; display: flex; gap: 30px; }
    .lang-section { flex: 1; }
    .lang-section h3 { font-size: 0.85rem; color: #58a6ff; margin-bottom: 15px; font-family: 'JetBrains Mono'; border-bottom: 1px solid #30363d; padding-bottom: 8px; }
    .lang-section p { font-size: 0.85rem; color: #8b949e; line-height: 1.6; }
    .v-divider { width: 1px; background: #30363d; }
    .btn-ack { width: 100%; padding: 15px; background: #238636; color: #fff; border: none; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono'; }

    /* --- SEARCH BAR (SPLUNK STYLE) --- */
    .siem-header { background: #0d1117; padding: 12px 20px; border-bottom: 1px solid #30363d; display: flex; align-items: center; gap: 10px; }
    .search-input-group { flex: 1; display: flex; background: #fff; border-radius: 4px; overflow: hidden; box-shadow: 0 0 10px rgba(0,0,0,0.5); }
    .prompt-label { background: #f0f0f0; color: #555; padding: 8px 15px; font-family: 'JetBrains Mono'; font-size: 0.75rem; border-right: 1px solid #ccc; font-weight: bold; }
    #searchInput { background: #fff; border: none; color: #000; padding: 8px 12px; width: 100%; font-family: 'JetBrains Mono'; font-size: 0.85rem; outline: none; }
    .btn-search { background: #5d9b2d; border: none; color: #fff; padding: 0 25px; cursor: pointer; }

    /* --- MAIN CONTENT --- */
    .siem-layout { display: flex; flex: 1; overflow: hidden; }
    
    /* Sidebar */
    .siem-sidebar { width: 280px; background: #0d1117; border-right: 1px solid #30363d; padding: 15px; overflow-y: auto; }
    .sidebar-title { font-size: 0.7rem; color: #8b949e; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 15px; border-bottom: 1px solid #21262d; padding-bottom: 5px; }
    .field-link { display: flex; justify-content: space-between; font-size: 0.75rem; padding: 5px 0; color: #58a6ff; cursor: pointer; }
    .field-link:hover { text-decoration: underline; }
    .field-count { color: #8b949e; font-family: 'JetBrains Mono'; }

    /* Log Area */
    .siem-results { flex: 1; background: #010409; display: flex; flex-direction: column; overflow: hidden; }
    .results-info { background: #161b22; padding: 8px 20px; border-bottom: 1px solid #30363d; font-size: 0.7rem; color: #8b949e; display: flex; justify-content: space-between; }
    .table-container { overflow-y: auto; flex: 1; }
    
    .log-table { width: 100%; border-collapse: collapse; font-size: 0.75rem; table-layout: fixed; }
    .log-table th { background: #161b22; color: #8b949e; text-align: left; padding: 10px; position: sticky; top: 0; border-bottom: 1px solid #30363d; }
    .log-table td { padding: 8px 10px; border-bottom: 1px solid #161b22; vertical-align: top; font-family: 'JetBrains Mono', monospace; line-height: 1.4; color: #c9d1d9; }
    .log-table tr:hover { background: rgba(255, 255, 255, 0.02); }

    /* Status Badges */
    .lvl { font-weight: bold; font-size: 0.65rem; border: 1px solid; border-radius: 2px; padding: 1px 4px; margin-right: 5px; }
    .lvl-critical { color: #f85149; border-color: rgba(248,81,73,0.4); }
    .lvl-warn { color: #d29922; border-color: rgba(210,153,34,0.4); }
    .lvl-info { color: #58a6ff; border-color: rgba(88,166,255,0.4); }

    /* Modal Surrounding (Büyüteç Ekranı) */
    .modal-surround { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); z-index: 9999; display: none; align-items: center; justify-content: center; }
    .modal-content { background: #0d1117; border: 1px solid #30363d; width: 90%; height: 80%; border-radius: 6px; display: flex; flex-direction: column; }
    .modal-header { padding: 15px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; background: #161b22; font-family: 'JetBrains Mono'; font-size: 0.8rem; }
    .modal-body { flex: 1; overflow-y: auto; padding: 20px; font-family: 'JetBrains Mono'; font-size: 0.75rem; line-height: 1.6; color: #8b949e; }
    .active-log { background: rgba(88, 166, 255, 0.1); color: #fff; border-left: 3px solid #58a6ff; padding-left: 10px; }

    .hl-id { color: #f85149; font-weight: bold; }
</style>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">LOG ANALYSIS TERMINAL // SİSTEM ERİŞİM MESAJI</div>
        <div class="disclaimer-body">
            <div class="lang-section">
                <h3>[TR] SİMÜLASYON ORTAMI</h3>
                <p>Bu panel gerçek bir SIEM arayüzüdür. Loglar dağınık üretilmiştir; vaka kanıtlarını bulmak için Case ID veya IP adreslerini arama çubuğunda filtreleyin.</p>
            </div>
            <div class="v-divider"></div>
            <div class="lang-section">
                <h3>[EN] SIMULATION MODE</h3>
                <p>High-fidelity SIEM interface. Logs are unsorted; use Case IDs or technical artifacts in the search bar to isolate evidence for your playbooks.</p>
            </div>
        </div>
        <button class="btn-ack" onclick="closeModal('welcomeModal')">INITIALIZE SESSION</button>
    </div>
</div>

<div class="siem-wrapper">
    <div class="siem-header">
        <div class="search-input-group">
            <div class="prompt-label">index=security | search</div>
            <input type="text" id="searchInput" placeholder="Enter keywords (e.g. #SOC-5092, sqlmap, 192.168.1.150)..." onkeyup="filterLogs()">
            <button class="btn-search" onclick="filterLogs()">
                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3"><circle cx="11" cy="11" r="8"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>
            </button>
        </div>
    </div>

    <div class="siem-layout">
        <div class="siem-sidebar">
            <h4 class="sidebar-title">Selected Fields</h4>
            <div class="field-link">host <span class="field-count">12</span></div>
            <div class="field-link">source <span class="field-count">6</span></div>
            
            <h4 class="sidebar-title" style="margin-top:25px;">Interesting Fields</h4>
            <div class="field-link">EventCode <span class="field-count">58</span></div>
            <div class="field-link">User <span class="field-count">142</span></div>
            <div class="field-link">process_name <span class="field-count">24</span></div>
        </div>

        <div class="siem-results">
            <div class="results-info">
                <span id="resultCount">Querying database...</span>
                <span>Real-time Forensic Stream</span>
            </div>
            <div class="table-container">
                <table class="log-table">
                    <thead>
                        <tr>
                            <th width="40"></th>
                            <th width="160">_time</th>
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
            <span>SOURCE VIEW: Surrounding Events (-5s / +5s)</span>
            <span style="cursor:pointer" onclick="closeSurround()">✕</span>
        </div>
        <div class="modal-body" id="surroundBody"></div>
        <div style="padding:15px; border-top:1px solid #30363d; text-align:right;">
            <button class="btn-search" style="background:#21262d" onclick="closeSurround()">CLOSE</button>
        </div>
    </div>
</div>

<script>
    const hosts = ["DC-01", "SQL-SRV", "FINANCE-PC", "WEB-PROD-01", "MAIL-GW", "VPN-GATE"];
    const users = ["admin", "system", "svc_sql", "jdoe", "msmith", "root", "apache", "adm_emir"];
    
    let allLogs = [];

    function rand(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }

    function generateEngine() {
        // 1. Rastgele 400 log üret
        for(let i=0; i<400; i++) {
            let d = new Date(); d.setSeconds(d.getSeconds() - (i * rand(2, 60)));
            const ip = `${rand(10,192)}.${rand(0,255)}.${rand(0,255)}.${rand(2,254)}`;
            
            const rawTemplates = [
                `EventCode=${rand(1000,9999)} SourceIP=${ip} User=${users[rand(0,7)]} Action=${rand(0,1)?'Allowed':'Blocked'} proto=TCP dest_port=${rand(22,65535)}`,
                `WinEventLog:Security:4624 AccountName=${users[rand(0,7)]} LogonType=3 SourceNetworkAddress=${ip} Status=Success`,
                `Service Control Manager: Service "${['WinDefend', 'Spooler', 'BITS'][rand(0,2)]}" entered the ${['running', 'stopped'][rand(0,1)]} state.`,
                `Apache_Access: ${ip} - - [${d.toISOString()}] "GET /api/v1/health HTTP/1.1" 200 ${rand(500,2000)}`
            ];

            allLogs.push({
                time: d.toISOString().replace('T', ' ').substring(0, 19),
                lvl: i % 10 === 0 ? "WARN" : "INFO",
                host: hosts[rand(0,5)],
                raw: rawTemplates[rand(0,3)]
            });
        }

        // 2. Vaka Delillerini (Evidence) Enjekte Et
        const evidence = [
            { lvl: "CRITICAL", host: "FINANCE-SRV-01", raw: "#SOC-5092 | SysmonID=11 | Image=tasksche.exe | Target=Budget.xlsx.locky | Entropy=7.92 | TargetIP=10.20.5.100" },
            { lvl: "WARN", host: "DC-01", raw: "#SOC-1022 | WinEventID=4625 | Account=adm_emir | Reason=0xc000006d | SourceIP=192.168.1.150 | Type=3" },
            { lvl: "ERROR", host: "WEB-SRV-PROD", raw: "#SOC-883 | ApacheLog | src=172.16.45.10 | payload='products.php?id=1 UNION SELECT' | agent=sqlmap/1.4.7 | code=200" }
        ];

        evidence.forEach(e => {
            let d = new Date(); d.setMinutes(d.getMinutes() - rand(5, 60));
            allLogs.push({
                time: d.toISOString().replace('T', ' ').substring(0, 19),
                lvl: e.lvl,
                host: e.host,
                raw: e.raw
            });
        });

        allLogs.sort((a,b) => new Date(b.time) - new Date(a.time));
    }

    function render(data) {
        const body = document.getElementById('logsBody');
        body.innerHTML = data.map((l, i) => `
            <tr>
                <td><span style="cursor:pointer; color:#8b949e" onclick="showSurround(${i})">🔍</span></td>
                <td style="color:#8b949e">${l.time}</td>
                <td style="color:#d29922">${l.host}</td>
                <td style="word-break:break-all">
                    <span class="lvl lvl-${l.lvl.toLowerCase()}">${l.lvl}</span> 
                    ${l.raw.replace(/(#SOC-\d+|tasksche|locky|sqlmap|0xc000006d)/gi, '<span class="hl-id">$1</span>')}
                </td>
            </tr>
        `).join('');
        document.getElementById('resultCount').innerText = `${data.length} events found`;
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

    window.onload = () => {
        document.getElementById('welcomeModal').style.display = 'flex';
        generateEngine();
        render(allLogs);
    };
</script>
