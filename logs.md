---
layout: default
title: Log Analysis & Threat Hunting
permalink: /logs/
---

<style>
    /* Full Screen Overrides for SIEM Experience */
    main { max-width: 98% !important; margin: 0 auto !important; padding: 1rem !important; }
    header, .navbar { max-width: 100% !important; }
</style>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-content">
        <div class="disclaimer-header">
            <span class="blink">⚠️</span> SYSTEM ACCESS MESSAGE // SİSTEM ERİŞİM MESAJI
        </div>
        <div class="disclaimer-body">
            <div class="lang-section">
                <h3>[TR] LOG ANALİZ PANELİ</h3>
                <p>Bu panel, aktif güvenlik vakalarını (Alerts) analiz etmeniz için gereken ham telemetri verilerini içerir.</p>
                <p><strong>İpucu:</strong> Arama çubuğuna Vaka ID'lerini (Örn: #SOC-5092) yazarak adli bilişim kanıtlarını bulabilirsiniz.</p>
            </div>
            <div class="divider"></div>
            <div class="lang-section">
                <h3>[EN] LOG SEARCH INTERFACE</h3>
                <p>This interface contains raw telemetry required to investigate active security cases from the Alerts dashboard.</p>
                <p><strong>Pro-Tip:</strong> Search for Case IDs (e.g., #SOC-5092) to filter forensic artifacts and complete your playbooks.</p>
            </div>
        </div>
        <div class="disclaimer-footer">
            <button class="btn-ack" onclick="closeWelcomeModal()">[ INITIALIZE DATA STREAM ]</button>
        </div>
    </div>
</div>

<div id="logDetailModal" class="detail-modal">
    <div class="detail-content">
        <div class="detail-header">
            <div class="dh-left">
                <h2>EVENT INSPECTOR</h2>
                <span class="dh-id" id="detailId">ID: 0000</span>
            </div>
            <span class="close-detail" onclick="closeDetailModal()">&times;</span>
        </div>
        
        <div class="detail-body">
            <div class="meta-grid">
                <div class="meta-item">
                    <label>TIMESTAMP</label>
                    <span id="dTime" class="mono">...</span>
                </div>
                <div class="meta-item">
                    <label>HOST / ASSET</label>
                    <span id="dHost" class="mono text-accent">...</span>
                </div>
                <div class="meta-item">
                    <label>DATA SOURCE</label>
                    <span id="dSource" class="mono">WinEventLog:Security</span>
                </div>
                <div class="meta-item">
                    <label>SEVERITY</label>
                    <span id="dLevel" class="badge">...</span>
                </div>
            </div>

            <hr class="detail-divider">

            <div class="raw-section">
                <div class="raw-header">
                    <span>RAW LOG PAYLOAD</span>
                    <button class="btn-copy">Copy Log</button>
                </div>
                <pre id="dRaw" class="raw-view"></pre>
            </div>
        </div>
    </div>
</div>

<div class="siem-container">

    <div class="search-section">
        <div class="search-wrapper">
            <div class="terminal-label">root@siem:~$ index=main | search</div>
            <input type="text" id="searchInput" placeholder="Search by ID (#SOC-5092), IP, or keywords..." onkeyup="filterLogs()">
            <button class="btn-search" onclick="filterLogs()">
                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3"><circle cx="11" cy="11" r="8"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>
            </button>
        </div>
        <div class="time-picker">
            <span>Last 24 Hours</span>
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"></polyline></svg>
        </div>
    </div>

    <div class="timeline-wrapper">
        <div class="timeline-bars" id="timeline"></div>
    </div>

    <div class="siem-body">
        <div class="siem-sidebar">
            <h3>INTERESTING FIELDS</h3>
            <div class="field-group">
                <div class="field-name">source_ip <span class="count">42</span></div>
                <div class="field-name">dest_port <span class="count">12</span></div>
                <div class="field-name">event_code <span class="count">4688</span></div>
                <div class="field-name">mitre_technique <span class="count">3</span></div>
                <div class="field-name">host <span class="count">10</span></div>
            </div>
            <div class="ad-box">
                <strong>ENGINE STATUS</strong><br>
                Uptime: 99.9%<br>Indexing: Active
            </div>
        </div>

        <div class="siem-results">
            <div class="results-header">
                <span id="resultCount">Generating logs...</span>
                <span style="font-size:0.75rem; color:#58a6ff; margin-left:auto; margin-right:10px;">*Real-time Telemetry Stream</span>
                <div class="pagination">
                    <button disabled>&lt;</button>
                    <button class="active">1</button>
                    <button>&gt;</button>
                </div>
            </div>

            <div class="table-scroll-wrapper">
                <table class="logs-table">
                    <thead>
                        <tr>
                            <th width="160">Time</th>
                            <th width="90">Level</th>
                            <th width="140">Host</th>
                            <th>Raw Event Preview</th>
                        </tr>
                    </thead>
                    <tbody id="logsTableBody"></tbody>
                </table>
            </div>
        </div>
    </div>
</div>

<script>
    // --- 1. LOG GENERATOR & FORENSIC ARTIFACTS ---
    
    const hosts = ["FINANCE-SRV-01", "HR-PC-04", "CEO-LAPTOP", "DC-01", "WEB-SRV-PROD", "FILE-SRV-02", "IDS-SENSOR-01", "FW-PERIMETER", "DEV-WORKSTATION", "GUEST-WIFI"];
    const users = ["admin", "system", "jdoe", "msmith", "svc_backup", "guest", "root", "apache"];
    const levels = ["INFO", "INFO", "WARN", "ERROR"]; 
    
    let generatedLogs = [];

    function rand(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }
    function pick(arr) { return arr[Math.floor(Math.random() * arr.length)]; }

    // --- PLAYBOOK EVIDENCE (CRITICAL LOGS) ---
    const forensicEvidence = [
        // Case #SOC-5092 (Ransomware)
        { 
            time: "2025-12-20 10:15:22", level: "CRITICAL", host: "FINANCE-SRV-01", 
            msg: "#SOC-5092 | Sysmon EventID=11 (FileCreate) | Image: C:\\Windows\\Temp\\tasksche.exe | Target: D:\\Data\\Budget.xlsx.locky | Entropy: 7.92 | Dest_IP: 10.20.5.100" 
        },
        // Case #SOC-1022 (Brute Force)
        { 
            time: "2025-12-20 11:20:45", level: "WARN", host: "DC-01", 
            msg: "#SOC-1022 | WinEventLog:Security EventID=4625 (Logon Failure) | Account: adm_emir | FailureReason: 0xc000006d | SourceIP: 192.168.1.150 | LogonType: 3" 
        },
        // Case #SOC-883 (SQLi)
        { 
            time: "2025-12-20 09:05:12", level: "ERROR", host: "WEB-SRV-PROD", 
            msg: "#SOC-883 | Apache_Log | 172.16.45.10 - 'GET /products.php?id=1' UNION SELECT table_name FROM information_schema.columns-- | Agent: sqlmap/1.4.7 | Code: 200" 
        }
    ];

    function generateRandomLog(id) {
        let date = new Date();
        date.setSeconds(date.getSeconds() - (id * rand(5, 30)));
        const host = pick(hosts);
        const user = pick(users);
        const ip = `192.168.1.${rand(10, 200)}`;
        
        const templates = [
            `EventCode=4624 LogonType=3 AccountName=${user} Source_IP=${ip} Status=Success`,
            `Action=ALLOW Interface=Internal Source=${ip} Destination=8.8.8.8 Protocol=UDP Port=53`,
            `EventCode=4688 NewProcessName=C:\\Windows\\System32\\svchost.exe CreatorProcess=services.exe User=SYSTEM`,
            `sshd[${rand(1000, 9999)}]: Accepted password for ${user} from ${ip} port ${rand(40000, 60000)} ssh2`,
            `Web_Traffic: ${ip} - GET /assets/css/main.css HTTP/1.1 200 4521`
        ];

        return {
            time: date.toISOString().replace('T', ' ').substring(0, 19),
            level: pick(levels),
            host: host,
            msg: pick(templates)
        };
    }

    // --- 2. INITIALIZATION ---

    document.addEventListener("DOMContentLoaded", function() {
        document.getElementById('welcomeModal').style.display = 'flex';
        
        // Populate logs: Evidence + 250 randoms
        generatedLogs = [...forensicEvidence];
        for(let i=0; i<250; i++) {
            generatedLogs.push(generateRandomLog(i));
        }
        
        // Sort by time (Newest first)
        generatedLogs.sort((a, b) => new Date(b.time) - new Date(a.time));
        
        renderLogs(generatedLogs);
        initTimeline();
    });

    function closeWelcomeModal() { document.getElementById('welcomeModal').style.display = 'none'; }

    function showDetails(index) {
        const log = generatedLogs[index];
        document.getElementById('detailId').innerText = "EVENT_ID: " + (20000 + index);
        document.getElementById('dTime').innerText = log.time;
        document.getElementById('dHost').innerText = log.host;
        const badge = document.getElementById('dLevel');
        badge.innerText = log.level;
        badge.className = "badge " + log.level.toLowerCase();
        document.getElementById('dRaw').innerText = log.msg;
        document.getElementById('logDetailModal').style.display = 'flex';
    }

    function closeDetailModal() { document.getElementById('logDetailModal').style.display = 'none'; }

    function highlightKeywords(text) {
        text = text.replace(/(CRITICAL|ERROR|Fail|Block|Deny|Ransomware|mimikatz|locky|sqlmap)/gi, '<span class="hl-red">$1</span>');
        text = text.replace(/(WARN|High|Suspicious|UNION SELECT)/gi, '<span class="hl-orange">$1</span>');
        return text;
    }

    function renderLogs(logs) {
        const tbody = document.getElementById("logsTableBody");
        tbody.innerHTML = "";
        const limit = Math.min(logs.length, 150);
        
        for(let i=0; i<limit; i++) {
            let log = logs[i];
            tbody.innerHTML += `<tr onclick="showDetails(${i})">
                <td class="mono time">${log.time}</td>
                <td class="mono"><span class="badge ${log.level.toLowerCase()}">${log.level}</span></td>
                <td class="mono host">${log.host}</td>
                <td class="mono raw">${highlightKeywords(log.msg)}</td>
            </tr>`;
        }
        document.getElementById("resultCount").innerText = logs.length + " events indexed";
    }

    function filterLogs() {
        let input = document.getElementById("searchInput").value.toLowerCase();
        let filtered = generatedLogs.filter(log => log.msg.toLowerCase().includes(input) || log.host.toLowerCase().includes(input));
        renderLogs(filtered);
    }

    function initTimeline() {
        const timeline = document.getElementById("timeline");
        for(let i=0; i<80; i++) {
            let h = rand(5, 45);
            let color = Math.random() > 0.95 ? '#ff5f56' : '#3fb950';
            timeline.innerHTML += `<div style="height:${h}px; background:${color}; width: 100%; opacity:0.6;"></div>`;
        }
    }
</script>

<style>
    /* SIEM REPLICA STYLING */
    .disclaimer-modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.95); z-index: 10000; display: none; align-items: center; justify-content: center; backdrop-filter: blur(8px); }
    .disclaimer-content { background: #0d1117; border: 2px solid var(--accent); width: 90%; max-width: 750px; border-radius: 8px; box-shadow: 0 0 40px rgba(88, 166, 255, 0.2); }
    .disclaimer-header { padding: 15px; color: var(--accent); font-family: 'JetBrains Mono', monospace; font-weight: bold; text-align: center; border-bottom: 1px solid #30363d; }
    .disclaimer-body { padding: 25px; color: #c9d1d9; display: flex; gap: 20px; font-size: 0.9rem; }
    .divider { width: 1px; background: #30363d; }
    .btn-ack { width: 100%; padding: 15px; background: #238636; color: white; border: none; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono', monospace; }
    
    .detail-modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 9000; display: none; align-items: center; justify-content: center; }
    .detail-content { background: #0d1117; border: 1px solid var(--accent); width: 95%; max-width: 900px; border-radius: 6px; display: flex; flex-direction: column; max-height: 90vh; }
    .detail-header { background: #161b22; padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #30363d; }
    .close-detail { color: #fff; font-size: 28px; cursor: pointer; }
    .detail-body { padding: 25px; overflow-y: auto; color: #c9d1d9; }
    .meta-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 15px; }
    .meta-item label { display: block; font-size: 0.65rem; color: #8b949e; margin-bottom: 4px; font-weight: bold; }
    .raw-view { background: #000; padding: 15px; border: 1px solid #30363d; color: #3fb950; font-size: 0.85rem; white-space: pre-wrap; word-break: break-all; }

    .siem-container { display: flex; flex-direction: column; gap: 10px; height: 85vh; }
    .terminal-label { background: #21262d; color: var(--accent); padding: 12px 15px; font-family: 'JetBrains Mono', monospace; font-size: 0.8rem; border-right: 1px solid #30363d; }
    #searchInput { width: 100%; border: none; padding: 12px; font-family: 'JetBrains Mono', monospace; background: #fff; color: #000; outline: none; }
    .timeline-wrapper { background: #0d1117; height: 50px; border: 1px solid #30363d; display: flex; align-items: flex-end; padding: 0 10px; gap: 2px; }
    .siem-body { display: flex; gap: 15px; flex: 1; overflow: hidden; }
    .siem-sidebar { width: 220px; background: #161b22; padding: 15px; border: 1px solid #30363d; }
    .siem-results { flex: 1; background: #0d1117; border: 1px solid #30363d; display: flex; flex-direction: column; overflow: hidden; }
    .table-scroll-wrapper { overflow-y: auto; flex: 1; }
    .logs-table { width: 100%; border-collapse: collapse; font-size: 0.8rem; }
    .logs-table th { background: #161b22; color: #8b949e; padding: 10px; text-align: left; position: sticky; top: 0; }
    .logs-table td { padding: 8px 10px; border-bottom: 1px solid #21262d; cursor: pointer; }
    .logs-table tr:hover { background: rgba(88, 166, 255, 0.1); }
    
    .hl-red { color: #ff5f56; font-weight: bold; }
    .hl-orange { color: #d29922; font-weight: bold; }
    .badge { padding: 2px 6px; border-radius: 3px; font-size: 0.65rem; }
    .critical, .error { background: #ff5f56; color: #fff; }
    .high, .warn { background: #d29922; color: #fff; }
    .info { background: #58a6ff; color: #fff; }
    .blink { animation: blinker 1s infinite; }
    @keyframes blinker { 50% { opacity: 0; } }
</style>
