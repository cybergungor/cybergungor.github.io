---
layout: default
title: Log Analysis
permalink: /logs/
---

<style>
    /* Full Screen Modu */
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
                <h3>[TR] SİMÜLASYON ORTAMI</h3>
                <p>Görüntülemekte olduğunuz arayüz, gerçek dünya <strong>SIEM</strong> ürünlerinin (Splunk, QRadar vb.) eğitim amaçlı replikasıdır. Buradaki veriler simülasyondur.</p>
                <p><strong>Nasıl Kullanılır:</strong> Tablodaki herhangi bir log satırına tıklayarak detaylı <strong>Log İnceleme (Drill-Down)</strong> penceresini açabilirsiniz.</p>
            </div>
            <div class="divider"></div>
            <div class="lang-section">
                <h3>[EN] SIMULATION ENVIRONMENT</h3>
                <p>This interface is an educational replica of real-world <strong>SIEM</strong> dashboards. All data displayed here is simulated.</p>
                <p><strong>How to use:</strong> Click on any log row in the table to open the detailed <strong>Log Drill-Down</strong> inspector view.</p>
            </div>
        </div>
        <div class="disclaimer-footer">
            <button class="btn-ack" onclick="closeWelcomeModal()">[ ACKNOWLEDGE & ENTER ]</button>
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

            <div class="network-grid">
                <div class="net-box">
                    <h4>SOURCE (Attacker/Client)</h4>
                    <div class="net-row"><span>IP Address:</span> <strong id="dSrcIP" class="mono text-red"></strong></div>
                    <div class="net-row"><span>Port:</span> <strong id="dSrcPort" class="mono"></strong></div>
                    <div class="net-row"><span>Geo Location:</span> <strong id="dGeo"></strong></div>
                </div>
                
                <div class="net-arrow">
                    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#58a6ff" stroke-width="2"><line x1="5" y1="12" x2="19" y2="12"></line><polyline points="12 5 19 12 12 19"></polyline></svg>
                </div>

                <div class="net-box">
                    <h4>DESTINATION (Target)</h4>
                    <div class="net-row"><span>IP Address:</span> <strong id="dDstIP" class="mono text-blue"></strong></div>
                    <div class="net-row"><span>Port:</span> <strong id="dDstPort" class="mono"></strong></div>
                    <div class="net-row"><span>Service:</span> <strong id="dService"></strong></div>
                </div>
            </div>

            <div class="context-grid">
                <div class="ctx-box">
                    <label>USER ACCOUNT</label>
                    <strong id="dUser"></strong>
                </div>
                <div class="ctx-box">
                    <label>PROCESS NAME</label>
                    <strong id="dProcess" class="mono"></strong>
                </div>
                <div class="ctx-box">
                    <label>ACTION</label>
                    <strong id="dAction"></strong>
                </div>
                <div class="ctx-box">
                    <label>PROTOCOL</label>
                    <strong id="dProtocol"></strong>
                </div>
            </div>

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
            <input type="text" id="searchInput" placeholder="Search logs (e.g. 192.168, Fail, Mimikatz)..." onkeyup="filterLogs()">
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
                <div class="field-name">source_ip <span class="count">8</span></div>
                <div class="field-name">dest_port <span class="count">4</span></div>
                <div class="field-name">event_code <span class="count">12</span></div>
                <div class="field-name">user_account <span class="count">5</span></div>
                <div class="field-name">host <span class="count">6</span></div>
            </div>
            <div class="ad-box">
                <strong>SYSTEM STATUS</strong><br>
                Indexing: 4500 EPS<br>Storage: 82% Full
            </div>
        </div>

        <div class="siem-results">
            <div class="results-header">
                <span id="resultCount">0 events matched</span>
                <span style="font-size:0.75rem; color:#58a6ff; margin-left:auto; margin-right:10px;">*Click rows for details</span>
                <div class="pagination">
                    <button disabled>&lt;</button>
                    <button class="active">1</button>
                    <button>&gt;</button>
                </div>
            </div>

            <table class="logs-table">
                <thead>
                    <tr>
                        <th width="140">Time</th>
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

<script>
    // --- 1. DATA (Zenginleştirilmiş Veri) ---
    const logsData = [
        { 
            time: "2025-12-19 10:42:15", level: "CRITICAL", host: "FINANCE-SRV-01", 
            msg: "EventCode=4663 Action=WriteData ProcessName=tasksche.exe FilePath=D:\\Finance\\Budget.xlsx.wnry",
            details: { src: "192.168.1.105", srcPort: "49211", geo: "Internal (LAN)", dst: "192.168.1.10", dstPort: "445 (SMB)", user: "SYSTEM", process: "tasksche.exe", action: "File Write (Encrypted)", proto: "TCP", service: "SMB" }
        },
        { 
            time: "2025-12-19 10:41:05", level: "CRITICAL", host: "HR-MANAGER-PC", 
            msg: "Sysmon EventID=3 Image=rundll32.exe DestIP=45.133.1.22 Msg='Cobalt Strike Beacon activity'",
            details: { src: "192.168.1.20", srcPort: "51022", geo: "Internal", dst: "45.133.1.22", dstPort: "443 (HTTPS)", user: "hr_manager", process: "rundll32.exe", action: "C2 Connection", proto: "HTTPS", service: "Web" }
        },
        { 
            time: "2025-12-19 10:38:22", level: "HIGH", host: "CEO-LAPTOP", 
            msg: "EventCode=4688 NewProcessName=mimikatz.exe CreatorProcess=powershell.exe Alert='Credential Dumping'",
            details: { src: "Localhost", srcPort: "N/A", geo: "Local", dst: "Localhost", dstPort: "N/A", user: "Administrator", process: "mimikatz.exe", action: "Process Create", proto: "N/A", service: "OS" }
        },
        { 
            time: "2025-12-19 10:30:00", level: "WARN", host: "VPN-GATEWAY", 
            msg: "GeoLocation_Alert User=jdoe Source_IP=203.0.113.5 (China) Message='Impossible Travel Detected'",
            details: { src: "203.0.113.5", srcPort: "34991", geo: "CN (China)", dst: "10.0.0.1", dstPort: "1194 (VPN)", user: "jdoe", process: "openvpn-daemon", action: "Login Attempt", proto: "UDP", service: "VPN" }
        },
        { 
            time: "2025-12-19 10:28:45", level: "ERROR", host: "WEB-SRV-PROD", 
            msg: "WAF_Log Action=BLOCK Rule='SQL Injection' Payload='UNION SELECT 1,2,3--' Source_IP=185.200.1.1",
            details: { src: "185.200.1.1", srcPort: "44211", geo: "RU (Russia)", dst: "192.168.1.80", dstPort: "80 (HTTP)", user: "Anonymous", process: "apache2", action: "WAF Block", proto: "HTTP", service: "Web App" }
        },
        { 
            time: "2025-12-19 10:15:33", level: "INFO", host: "IDS-SENSOR-01", 
            msg: "Suricata_Alert SID=2001219 Msg='ET SCAN Potential Internal Network Scan' Source=192.168.1.50",
            details: { src: "192.168.1.50", srcPort: "Multiple", geo: "Internal", dst: "192.168.1.*", dstPort: "Multiple", user: "N/A", process: "nmap", action: "Port Scan", proto: "TCP", service: "N/A" }
        },
        { 
            time: "2025-12-19 09:55:10", level: "ERROR", host: "LINUX-JUMP-HOST", 
            msg: "sshd[1299]: Failed password for invalid user admin from 192.168.1.5 port 4422 ssh2",
            details: { src: "192.168.1.5", srcPort: "4422", geo: "Internal", dst: "192.168.1.99", dstPort: "22 (SSH)", user: "admin", process: "sshd", action: "Auth Failed", proto: "TCP", service: "SSH" }
        }
    ];

    // --- 2. FONKSİYONLAR ---
    
    // Welcome Modal
    document.addEventListener("DOMContentLoaded", function() {
         document.getElementById('welcomeModal').style.display = 'flex';
    });
    function closeWelcomeModal() {
        document.getElementById('welcomeModal').style.display = 'none';
    }

    // Detail Modal
    function showDetails(index) {
        const log = logsData[index];
        const det = log.details;

        // Header
        document.getElementById('detailId').innerText = "ID: " + (5000 + index);
        document.getElementById('dTime').innerText = log.time;
        document.getElementById('dHost').innerText = log.host;
        
        // Level Renklendirme
        const badge = document.getElementById('dLevel');
        badge.innerText = log.level;
        badge.className = "badge " + log.level.toLowerCase();

        // Network
        document.getElementById('dSrcIP').innerText = det.src;
        document.getElementById('dSrcPort').innerText = det.srcPort;
        document.getElementById('dGeo').innerText = det.geo;
        
        document.getElementById('dDstIP').innerText = det.dst;
        document.getElementById('dDstPort').innerText = det.dstPort;
        document.getElementById('dService').innerText = det.service;

        // Context
        document.getElementById('dUser').innerText = det.user;
        document.getElementById('dProcess').innerText = det.process;
        document.getElementById('dAction').innerText = det.action;
        document.getElementById('dProtocol').innerText = det.proto;

        // Raw
        document.getElementById('dRaw').innerText = log.msg;

        document.getElementById('logDetailModal').style.display = 'flex';
    }

    function closeDetailModal() {
        document.getElementById('logDetailModal').style.display = 'none';
    }

    // IP Maskeleme & Highlight
    function maskIPs(text) { return text.replace(/(\d{1,3}\.\d{1,3}\.\d{1,3})\.\d{1,3}/g, "$1.***"); }
    function highlightKeywords(text) {
        text = text.replace(/(CRITICAL|ERROR|Fail|Block|Deny|Malicious|Ransomware|Mimikatz)/gi, '<span class="hl-red">$1</span>');
        text = text.replace(/(WARN|High|Suspicious)/gi, '<span class="hl-orange">$1</span>');
        return maskIPs(text);
    }

    // Tabloyu Çizme
    function renderLogs(logs) {
        const tbody = document.getElementById("logsTableBody");
        const countSpan = document.getElementById("resultCount");
        tbody.innerHTML = "";
        
        logs.forEach((log, index) => {
            let row = `<tr onclick="showDetails(${index})">
                <td class="mono time">${log.time}</td>
                <td class="mono"><span class="badge ${log.level.toLowerCase()}">${log.level}</span></td>
                <td class="mono host">${log.host}</td>
                <td class="mono raw">${highlightKeywords(log.msg)}</td>
            </tr>`;
            tbody.innerHTML += row;
        });
        
        countSpan.innerText = logs.length + " events matched";
    }

    function filterLogs() {
        let input = document.getElementById("searchInput").value.toLowerCase();
        let filtered = logsData.filter(log => {
            return log.msg.toLowerCase().includes(input) || log.host.toLowerCase().includes(input);
        });
        renderLogs(filtered);
    }

    function initTimeline() {
        const timeline = document.getElementById("timeline");
        for(let i=0; i<60; i++) {
            let h = Math.floor(Math.random() * 40) + 5;
            let color = Math.random() > 0.9 ? '#ff5f56' : '#3fb950';
            let bar = `<div style="height:${h}px; background:${color}; width: 100%; opacity:0.7;"></div>`;
            timeline.innerHTML += bar;
        }
    }

    window.onload = function() {
        renderLogs(logsData);
        initTimeline();
    };
</script>

<style>
    /* ===== MODALS ===== */
    /* Disclaimer (Welcome) */
    .disclaimer-modal {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background-color: rgba(0, 0, 0, 0.95); backdrop-filter: blur(8px);
        z-index: 10000; display: none; align-items: center; justify-content: center;
    }
    .disclaimer-content {
        background: #0d1117; border: 2px solid #ff5f56; width: 90%; max-width: 700px;
        border-radius: 8px; box-shadow: 0 0 40px rgba(255, 95, 86, 0.2);
    }
    .disclaimer-header { padding: 15px; color: #ff5f56; font-family: 'JetBrains Mono', monospace; font-weight: bold; text-align: center; border-bottom: 1px solid #ff5f56; }
    .disclaimer-body { padding: 20px; color: #c9d1d9; font-size: 0.9rem; display: flex; gap: 20px; }
    .divider { width: 1px; background: #30363d; }
    .btn-ack { width: 100%; padding: 15px; background: #238636; color: white; border: none; font-weight: bold; cursor: pointer; }
    .btn-ack:hover { background: #2ea043; }
    .blink { animation: blinker 1s infinite; }

    /* Log Detail Modal (NEW) */
    .detail-modal {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background-color: rgba(0, 0, 0, 0.8); backdrop-filter: blur(4px);
        z-index: 9000; display: none; align-items: center; justify-content: center;
    }
    .detail-content {
        background: #0d1117; border: 1px solid #58a6ff; width: 95%; max-width: 900px;
        border-radius: 6px; box-shadow: 0 0 50px rgba(88, 166, 255, 0.2);
        display: flex; flex-direction: column; max-height: 90vh;
    }
    .detail-header {
        background: #161b22; padding: 15px 20px; border-bottom: 1px solid #30363d;
        display: flex; justify-content: space-between; align-items: center;
    }
    .dh-left h2 { margin: 0; font-size: 1rem; color: #58a6ff; font-family: 'JetBrains Mono', monospace; display: inline-block; margin-right: 15px; }
    .dh-id { font-family: 'JetBrains Mono', monospace; color: #8b949e; font-size: 0.9rem; }
    .close-detail { color: #fff; font-size: 28px; cursor: pointer; }
    
    .detail-body { padding: 25px; overflow-y: auto; font-family: 'Inter', sans-serif; color: #c9d1d9; }
    
    /* Meta Grid */
    .meta-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; margin-bottom: 20px; }
    .meta-item label { display: block; font-size: 0.7rem; color: #8b949e; margin-bottom: 4px; font-weight: bold; }
    .meta-item span { font-size: 0.9rem; color: #e6edf3; }
    .detail-divider { border: 0; border-top: 1px solid #30363d; margin: 20px 0; }

    /* Network Grid */
    .network-grid { display: flex; align-items: center; gap: 20px; margin-bottom: 20px; }
    .net-box { flex: 1; background: #161b22; border: 1px solid #30363d; padding: 15px; border-radius: 4px; }
    .net-box h4 { margin: 0 0 10px 0; font-size: 0.8rem; color: #58a6ff; font-family: 'JetBrains Mono', monospace; }
    .net-row { display: flex; justify-content: space-between; font-size: 0.85rem; margin-bottom: 5px; border-bottom: 1px dashed #21262d; padding-bottom: 2px; }
    .net-arrow { color: #58a6ff; }

    /* Context Grid */
    .context-grid { display: grid; grid-template-columns: 1fr 1fr 1fr 1fr; gap: 15px; margin-bottom: 20px; }
    .ctx-box { background: rgba(56, 139, 253, 0.05); border: 1px solid rgba(56, 139, 253, 0.2); padding: 10px; border-radius: 4px; }
    .ctx-box label { display: block; font-size: 0.65rem; color: #8b949e; margin-bottom: 3px; }
    .ctx-box strong { font-size: 0.9rem; color: #c9d1d9; }

    /* Raw Log */
    .raw-section { margin-top: 10px; }
    .raw-header { display: flex; justify-content: space-between; margin-bottom: 5px; font-size: 0.75rem; color: #8b949e; font-weight: bold; }
    .btn-copy { background: transparent; border: 1px solid #30363d; color: #58a6ff; cursor: pointer; padding: 2px 8px; border-radius: 3px; font-size: 0.7rem; }
    .raw-view { background: #000; padding: 15px; border-radius: 4px; border: 1px solid #30363d; color: #3fb950; font-size: 0.85rem; white-space: pre-wrap; word-break: break-all; }

    .text-red { color: #ff5f56; }
    .text-blue { color: #58a6ff; }
    .text-accent { color: #d29922; }

    /* Table Cursor */
    .logs-table tr:hover { cursor: pointer; background-color: rgba(88, 166, 255, 0.08) !important; }

    /* ===== SIEM LAYOUT ===== */
    .siem-container { display: flex; flex-direction: column; gap: 10px; font-family: 'Inter', sans-serif; height: 85vh; }
    .search-section { display: flex; gap: 10px; align-items: center; margin-bottom: 5px; }
    .search-wrapper { flex: 1; display: flex; align-items: center; background: #fff; border-radius: 4px; overflow: hidden; }
    .terminal-label { background: #21262d; color: #58a6ff; padding: 12px 15px; font-family: 'JetBrains Mono', monospace; font-size: 0.85rem; font-weight: bold; border-right: 1px solid #30363d; white-space: nowrap; }
    #searchInput { width: 100%; border: none; padding: 12px 15px; font-family: 'JetBrains Mono', monospace; font-size: 0.9rem; color: #24292f; background: #fff; outline: none; flex: 1; }
    .btn-search { background: #3fb950; border: none; padding: 0 15px; height: 100%; cursor: pointer; color: #fff; display: flex; align-items: center; }
    .time-picker { background: #161b22; border: 1px solid #30363d; color: #c9d1d9; padding: 10px 15px; border-radius: 4px; font-size: 0.85rem; display: flex; align-items: center; gap: 8px; cursor: pointer; }
    .timeline-wrapper { background: #0d1117; height: 50px; border: 1px solid #30363d; margin-bottom: 10px; border-radius: 4px; padding: 0 10px; display: flex; align-items: flex-end; }
    .timeline-bars { display: flex; gap: 2px; width: 100%; align-items: flex-end; padding-bottom: 2px; }
    .siem-body { display: flex; gap: 15px; flex: 1; overflow: hidden; }
    .siem-sidebar { width: 220px; background: #161b22; border: 1px solid #30363d; border-radius: 4px; padding: 15px; display: flex; flex-direction: column; gap: 20px; overflow-y: auto; }
    .siem-sidebar h3 { margin: 0 0 10px 0; font-size: 0.75rem; color: #8b949e; border-bottom: 1px solid #30363d; padding-bottom: 5px; }
    .field-name { font-size: 0.8rem; color: #58a6ff; cursor: pointer; display: flex; justify-content: space-between; margin-bottom: 6px; }
    .count { color: #8b949e; font-size: 0.75rem; }
    .ad-box { background: rgba(63, 185, 80, 0.1); border: 1px solid #3fb950; padding: 10px; font-size: 0.75rem; color: #3fb950; border-radius: 4px; font-family: 'JetBrains Mono', monospace; }
    .siem-results { flex: 1; display: flex; flex-direction: column; background: #0d1117; border: 1px solid #30363d; border-radius: 4px; overflow: hidden; }
    .results-header { background: #161b22; padding: 8px 15px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    #resultCount { font-size: 0.8rem; color: #8b949e; font-weight: bold; }
    .pagination button { background: transparent; border: 1px solid #30363d; color: #c9d1d9; padding: 2px 8px; border-radius: 3px; cursor: pointer; font-size: 0.75rem; }
    .pagination button.active { background: #58a6ff; color: #0d1117; border-color: #58a6ff; }
    .logs-table { width: 100%; border-collapse: collapse; font-size: 0.8rem; }
    .logs-table th { background: #161b22; color: #8b949e; text-align: left; padding: 8px 10px; font-weight: normal; border-bottom: 1px solid #30363d; }
    .logs-table td { padding: 6px 10px; border-bottom: 1px solid #21262d; vertical-align: top; color: #c9d1d9; }
    .mono { font-family: 'JetBrains Mono', monospace; }
    .time { color: #8b949e; white-space: nowrap; }
    .host { color: #d29922; }
    .raw { word-break: break-all; line-height: 1.5; }
    .hl-red { color: #ff5f56; font-weight: bold; }
    .hl-orange { color: #d29922; font-weight: bold; }
    .badge { padding: 2px 6px; border-radius: 3px; font-size: 0.65rem; font-weight: bold; }
    .critical, .error { background: #ff5f56; color: #0d1117; }
    .high, .warn { background: #d29922; color: #0d1117; }
    .info { background: #58a6ff; color: #0d1117; }
    @keyframes blinker { 50% { opacity: 0; } }
    @media(max-width: 768px) {
        .siem-body { flex-direction: column; }
        .siem-sidebar, .timeline-wrapper { display: none; }
        .search-section { flex-direction: column; }
        .terminal-label { display: none; }
        .network-grid { flex-direction: column; }
        .context-grid { grid-template-columns: 1fr 1fr; }
    }
</style>
