---
layout: default
title: SentinelGuard - Advanced Log Analytics
permalink: /logs/
---

<style>
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

    /* --- SEARCH BAR (SPLUNK DARK) --- */
    .siem-header { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; align-items: center; }
    .search-input-group { flex: 1; display: flex; background: #010409; border: 1px solid #30363d; border-radius: 6px; overflow: hidden; }
    .prompt-label { background: #161b22; color: #58a6ff; padding: 10px 18px; font-family: 'JetBrains Mono'; font-size: 0.8rem; border-right: 1px solid #30363d; }
    #searchInput { background: transparent; border: none; color: #e6edf3; padding: 10px 15px; width: 100%; font-family: 'JetBrains Mono'; font-size: 0.85rem; outline: none; }
    .btn-search { background: #238636; border: none; color: #fff; padding: 0 25px; cursor: pointer; transition: 0.2s; font-weight: bold; }

    /* --- LAYOUT --- */
    .siem-layout { display: flex; flex: 1; overflow: hidden; }
    
    /* --- REALISTIC SIDEBAR (FIELDS) --- */
    .siem-sidebar { width: 280px; background: #0d1117; border-right: 1px solid #30363d; padding: 15px; overflow-y: auto; }
    .sidebar-section { margin-bottom: 25px; }
    .sidebar-title { font-size: 0.7rem; color: #8b949e; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 10px; font-weight: bold; }
    
    .field-group { margin-bottom: 8px; }
    .field-name { 
        display: flex; justify-content: space-between; font-size: 0.75rem; 
        padding: 6px 8px; color: #e6edf3; cursor: pointer; border-radius: 4px; 
    }
    .field-name:hover { background: rgba(255, 255, 255, 0.05); }
    .field-name.active { color: #58a6ff; font-weight: bold; }

    /* Field Value List (Popup style within sidebar) */
    .field-values { 
        display: none; background: #161b22; margin: 4px 0 10px 10px; 
        border-radius: 4px; padding: 5px; border: 1px solid #30363d;
    }
    .value-item { 
        display: flex; justify-content: space-between; font-size: 0.7rem; 
        padding: 4px 8px; color: #8b949e; cursor: pointer; border-radius: 3px;
    }
    .value-item:hover { background: #21262d; color: #58a6ff; }
    .val-count { font-family: 'JetBrains Mono'; color: #3fb950; font-size: 0.65rem; }

    /* --- LOG RESULTS --- */
    .siem-results { flex: 1; background: #010409; display: flex; flex-direction: column; overflow: hidden; }
    .results-info { background: #0d1117; padding: 10px 25px; border-bottom: 1px solid #30363d; font-size: 0.75rem; color: #8b949e; display: flex; justify-content: space-between; }
    .table-container { overflow-y: auto; flex: 1; }
    .log-table { width: 100%; border-collapse: collapse; font-size: 0.75rem; table-layout: fixed; }
    .log-table th { background: #161b22; color: #8b949e; text-align: left; padding: 12px; position: sticky; top: 0; border-bottom: 1px solid #30363d; z-index: 10; }
    .log-table td { padding: 10px 12px; border-bottom: 1px solid #21262d; font-family: 'JetBrains Mono', monospace; color: #c9d1d9; vertical-align: top; }
    .log-table tr:hover { background: rgba(88, 166, 255, 0.04); cursor: pointer; }

    /* Badges */
    .lvl { font-weight: bold; font-size: 0.65rem; border: 1px solid; border-radius: 3px; padding: 2px 6px; margin-right: 8px; }
    .lvl-critical { color: #f85149; border-color: rgba(248, 81, 73, 0.4); }
    .lvl-warn { color: #d29922; border-color: rgba(210, 153, 34, 0.4); }
    .lvl-info { color: #58a6ff; border-color: rgba(88, 166, 255, 0.4); }

    /* Surrounding Modal */
    .modal-surround { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.85); z-index: 9999; display: none; align-items: center; justify-content: center; backdrop-filter: blur(8px); }
    .modal-content { background: #0d1117; border: 1px solid #30363d; width: 90%; height: 85%; border-radius: 12px; display: flex; flex-direction: column; overflow: hidden; }
    .modal-header { padding: 15px 25px; background: #161b22; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    .btn-close-modal { background: #30363d; color: #fff; border: none; padding: 8px 20px; border-radius: 4px; cursor: pointer; font-weight: bold; }
    .btn-close-modal:hover { background: #f85149; }

    .active-log { background: rgba(88, 166, 255, 0.08); color: #fff; border-left: 4px solid #58a6ff; padding-left: 15px; }
    .hl-id { color: #f85149; font-weight: bold; }

    @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
</style>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">SENTINELGUARD LOG ANALYTICS v4.2</div>
        <div class="disclaimer-body">
            <div class="lang-section">
                <h3>[TR] DİNAMİK ANALİZ</h3>
                <p>Sol taraftaki alanlara (User, Host vb.) tıklayarak veri özetlerini görebilir ve aramanızı otomatik olarak daraltabilirsiniz.</p>
            </div>
            <div class="v-divider"></div>
            <div class="lang-section">
                <h3>[EN] DYNAMIC ANALYSIS</h3>
                <p>Click on sidebar fields to reveal value summaries and apply filters to your current search query instantly.</p>
            </div>
        </div>
        <button class="btn-ack" onclick="closeModal('welcomeModal')">START INVESTIGATION</button>
    </div>
</div>

<div class="siem-wrapper">
    <div class="siem-header">
        <div class="search-input-group">
            <div class="prompt-label">index=security | search</div>
            <input type="text" id="searchInput" placeholder="Search by artifacts or use sidebar filters..." onkeyup="handleKeyUp(event)">
            <button class="btn-search" onclick="filterLogs()">SEARCH</button>
        </div>
    </div>

    <div class="siem-layout">
        <div class="siem-sidebar" id="sidebarFields">
            <div class="sidebar-section">
                <div class="sidebar-title">Selected Fields</div>
                <div class="field-group">
                    <div class="field-name" onclick="toggleField('host')"><span>> host</span><span class="field-count">6</span></div>
                    <div class="field-values" id="vals-host"></div>
                </div>
            </div>

            <div class="sidebar-section">
                <div class="sidebar-title">Interesting Fields</div>
                <div class="field-group">
                    <div class="field-name" onclick="toggleField('User')"><span>> User</span><span class="field-count">8</span></div>
                    <div class="field-values" id="vals-User"></div>
                </div>
                <div class="field-group">
                    <div class="field-name" onclick="toggleField('EventID')"><span>> EventID</span><span class="field-count">12</span></div>
                    <div class="field-values" id="vals-EventID"></div>
                </div>
                <div class="field-group">
                    <div class="field-name" onclick="toggleField('Action')"><span>> Action</span><span class="field-count">2</span></div>
                    <div class="field-values" id="vals-Action"></div>
                </div>
            </div>
        </div>

        <div class="siem-results">
            <div class="results-info">
                <span id="resultCount">Indexing...</span>
                <span>Real-time Stream: Active</span>
            </div>
            <div class="table-container">
                <table class="log-table">
                    <thead>
                        <tr>
                            <th width="40">#</th>
                            <th width="170">_time</th>
                            <th width="120">host</th>
                            <th>_raw</th>
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
            <span style="font-family:'JetBrains Mono'; color:#58a6ff; font-weight:bold;">SURROUNDING EVENTS (-5s / +5s)</span>
            <button class="btn-close-modal" onclick="closeSurround()">✕</button>
        </div>
        <div id="surroundBody" style="flex:1; overflow-y:auto; padding:25px; font-family:'JetBrains Mono'; font-size:0.75rem; line-height:1.8; color:#8b949e;"></div>
    </div>
</div>

<script>
    const config = {
        hosts: ["DC-01", "SQL-SRV-PROD", "FINANCE-PC", "WEB-CLUSTER-01", "MAIL-GATEWAY", "FIREWALL-HQ"],
        users: ["adm_emir", "svc_sql", "system", "jdoe", "msmith", "root", "apache"],
        eventIds: ["4624", "4625", "4688", "11", "3", "4720"]
    };
    
    let allLogs = [];

    function rand(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }

    function generateEngine() {
        for(let i=0; i<400; i++) {
            let d = new Date(); d.setSeconds(d.getSeconds() - (i * 20));
            const ip = `${rand(10,192)}.${rand(0,255)}.${rand(0,255)}.${rand(2,254)}`;
            const host = config.hosts[rand(0,5)];
            const user = config.users[rand(0,6)];
            const eid = config.eventIds[rand(0,5)];
            const action = rand(0,1) ? 'Allow' : 'Deny';

            const raw = `EventID=${eid} SourceIP=${ip} User=${user} Action=${action} host=${host} Port=${rand(22,65535)}`;
            allLogs.push({ time: d.toISOString().replace('T', ' ').substring(0, 19), lvl: rand(0,10) > 8 ? "WARN" : "INFO", host: host, raw: raw, user: user, eid: eid, action: action });
        }
        
        // Inject Cases
        allLogs.push({ time: "2025-12-20 10:15:22", lvl: "CRITICAL", host: "FINANCE-SRV-01", raw: "#SOC-5092 | Malware: Ransomware | Process: tasksche.exe | Encrypting: Budget.xlsx.locky | Target: 10.20.5.100", user: "system", eid: "11", action: "Deny" });
        allLogs.push({ time: "2025-12-20 11:20:45", lvl: "WARN", host: "DC-01", raw: "#SOC-1022 | Bruteforce Account: adm_emir | FailureCode: 0xc000006d | SourceIP: 192.168.1.150", user: "adm_emir", eid: "4625", action: "Deny" });
        allLogs.sort((a,b) => new Date(b.time) - new Date(a.time));
    }

    function render(data) {
        document.getElementById('logsBody').innerHTML = data.map((l, i) => `
            <tr>
                <td style="color:#8b949e; cursor:pointer; text-align:center;" onclick="showSurround(${i})">🔍</td>
                <td style="color:#8b949e">${l.time}</td>
                <td style="color:#d29922">${l.host}</td>
                <td><span class="lvl lvl-${l.lvl.toLowerCase()}">${l.lvl}</span> ${l.raw.replace(/(#SOC-\d+|tasksche|locky|sqlmap)/gi, '<span class="hl-id">$1</span>')}</td>
            </tr>
        `).join('');
        document.getElementById('resultCount').innerText = `${data.length} events matching query`;
    }

    // --- DYNAMIC SIDEBAR LOGIC ---
    function toggleField(fieldName) {
        const panel = document.getElementById(`vals-${fieldName}`);
        const isVisible = panel.style.display === 'block';
        
        // Close all first
        document.querySelectorAll('.field-values').forEach(p => p.style.display = 'none');
        
        if (!isVisible) {
            panel.style.display = 'block';
            calculateFieldStats(fieldName);
        }
    }

    function calculateFieldStats(fieldName) {
        const stats = {};
        const logs = document.getElementById('searchInput').value ? allLogs.filter(l => l.raw.includes(document.getElementById('searchInput').value)) : allLogs;
        
        logs.forEach(l => {
            let val;
            if(fieldName === 'host') val = l.host;
            else if(fieldName === 'User') val = l.user;
            else if(fieldName === 'EventID') val = l.eid;
            else if(fieldName === 'Action') val = l.action;
            
            stats[val] = (stats[val] || 0) + 1;
        });

        const panel = document.getElementById(`vals-${fieldName}`);
        panel.innerHTML = Object.entries(stats)
            .sort((a,b) => b[1] - a[1])
            .slice(0, 5)
            .map(([val, count]) => `
                <div class="value-item" onclick="applyFilter('${fieldName}', '${val}')">
                    <span>${val}</span>
                    <span class="val-count">${count}</span>
                </div>
            `).join('');
    }

    function applyFilter(key, val) {
        const searchInput = document.getElementById('searchInput');
        searchInput.value = `${key}="${val}"`;
        filterLogs();
    }

    function filterLogs() {
        const v = document.getElementById('searchInput').value.toLowerCase().replace(/"/g, '').split('=');
        let filtered;
        if(v.length === 2) {
            const key = v[0].trim();
            const val = v[1].trim();
            filtered = allLogs.filter(l => (l[key] && l[key].toLowerCase() === val) || l.raw.toLowerCase().includes(val));
        } else {
            filtered = allLogs.filter(l => l.raw.toLowerCase().includes(v[0]));
        }
        render(filtered);
    }

    function handleKeyUp(e) { if(e.key === 'Enter') filterLogs(); }
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
    function closeSurround() { document.getElementById('surroundModal').style.display = 'none'; }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }

    window.onload = () => {
        document.getElementById('welcomeModal').style.display = 'flex';
        generateEngine();
        render(allLogs);
    };
</script>
