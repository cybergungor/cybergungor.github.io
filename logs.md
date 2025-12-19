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
                <p>Görüntülemekte olduğunuz arayüz, gerçek dünya <strong>SIEM</strong> ürünlerinin (Splunk, QRadar vb.) eğitim amaçlı replikasıdır.</p>
                <p><strong>Log Motoru:</strong> Sayfa yüklendiğinde "Sentetik Log Üreticisi" devreye girer ve yüzlerce rastgele güvenlik olayı oluşturur.</p>
            </div>
            <div class="divider"></div>
            <div class="lang-section">
                <h3>[EN] SIMULATION ENVIRONMENT</h3>
                <p>This interface is an educational replica of real-world <strong>SIEM</strong> dashboards. Data is synthetically generated.</p>
                <p><strong>Log Engine:</strong> Upon loading, the "Synthetic Log Generator" creates hundreds of random security events for analysis.</p>
            </div>
        </div>
        <div class="disclaimer-footer">
            <button class="btn-ack" onclick="closeWelcomeModal()">[ INITIALIZE LOG STREAM ]</button>
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
                <div class="field-name">source_ip <span class="count">241</span></div>
                <div class="field-name">dest_port <span class="count">80</span></div>
                <div class="field-name">event_code <span class="count">4625</span></div>
                <div class="field-name">user_account <span class="count">12</span></div>
                <div class="field-name">host <span class="count">18</span></div>
            </div>
            <div class="ad-box">
                <strong>SYSTEM STATUS</strong><br>
                Indexing: 12,500 EPS<br>Storage: 82% Full
            </div>
        </div>

        <div class="siem-results">
            <div class="results-header">
                <span id="resultCount">Generating logs...</span>
                <span style="font-size:0.75rem; color:#58a6ff; margin-left:auto; margin-right:10px;">*Live Data Stream</span>
                <div class="pagination">
                    <button disabled>&lt;</button>
                    <button class="active">1</button>
                    <button>2</button>
                    <button>3</button>
                    <button>...</button>
                    <button>99</button>
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
    // --- 1. LOG GENERATOR ENGINE (Sihirli Kısım Burası) ---
    
    // Yardımcı Veriler
    const hosts = ["FINANCE-SRV-01", "HR-PC-04", "CEO-LAPTOP", "DC-01", "WEB-SRV-PROD", "FILE-SRV-02", "IDS-SENSOR-01", "FW-PERIMETER", "DEV-WORKSTATION", "GUEST-WIFI"];
    const users = ["admin", "system", "jdoe", "msmith", "svc_backup", "guest", "root", "apache"];
    const actions = ["Accepted", "Failed", "Blocked", "Deleted", "Created", "Modified"];
    const levels = ["INFO", "INFO", "INFO", "WARN", "WARN", "ERROR", "CRITICAL"]; // INFO daha sık çıksın diye çok yazdım
    
    let generatedLogs = [];

    // Rastgele Sayı Üretici
    function rand(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }
    // Rastgele IP Üretici
    function randIP() { return `192.168.${rand(1, 50)}.${rand(1, 255)}`; }
    // Rastgele Eleman Seçici
    function pick(arr) { return arr[Math.floor(Math.random() * arr.length)]; }

    // Log Şablonları (Templates)
    function generateLogEntry(id) {
        const type = rand(1, 6);
        let msg = "";
        let level = pick(levels);
        let host = pick(hosts);
        let user = pick(users);
        let src = randIP();
        
        // Şablon 1: Windows Login Failure
        if (type === 1) {
            msg = `EventCode=4625 LogonType=3 AccountName=${user} Status=0xC000006D SubStatus=0xC0000064 Message='An account failed to log on.' Source_Network_Address=${src}`;
            level = "WARN";
        } 
        // Şablon 2: Firewall Block
        else if (type === 2) {
            msg = `Action=DENY Interface=WAN Source=${src} Destination=10.0.0.5 Protocol=TCP Port=${rand(1000, 65000)} Message='Access Control List (ACL) Block'`;
            if(Math.random() > 0.8) level = "ERROR"; else level = "INFO";
            host = "FW-PERIMETER";
        }
        // Şablon 3: Process Creation (Normal)
        else if (type === 3) {
            msg = `EventCode=4688 NewProcessName=C:\\Windows\\System32\\svchost.exe CreatorProcess=services.exe User=${user} CommandLine='-k netsvcs -p'`;
            level = "INFO";
        }
        // Şablon 4: Web Server Log
        else if (type === 4) {
            const methods = ["GET", "POST"];
            const paths = ["/index.html", "/login.php", "/api/v1/data", "/admin", "/static/css/style.css"];
            const codes = [200, 200, 200, 404, 500, 302];
            msg = `${src} - - [${new Date().toISOString()}] "${pick(methods)} ${pick(paths)} HTTP/1.1" ${pick(codes)} ${rand(100, 5000)} "-" "Mozilla/5.0"`;
            host = "WEB-SRV-PROD";
        }
        // Şablon 5: SSH Activity
        else if (type === 5) {
            msg = `sshd[${rand(1000,9999)}]: ${pick(actions)} password for ${user} from ${src} port ${rand(30000,60000)} ssh2`;
            host = "LINUX-SRV-01";
        }
        // Şablon 6: Nadir Kritik Olaylar (Kırmızı yapacaklarımız)
        else {
            if(Math.random() > 0.7) {
                msg = `EventCode=4688 NewProcessName=mimikatz.exe User=${user} Message='Potentially malicious tool detected'`;
                level = "CRITICAL";
            } else {
                msg = `Suricata_Alert SID=${rand(200000,300000)} Msg='ET SCAN Potential Nmap Scan' Source=${src}`;
                level = "HIGH";
            }
        }

        // Zamanı biraz geriye doğru dağıt
        let date = new Date();
        date.setSeconds(date.getSeconds() - (id * rand(2, 60))); // Her log birkaç saniye/dakika arayla
        
        return {
            time: date.toISOString().replace('T', ' ').substring(0, 19),
            level: level,
            host: host,
            msg: msg
        };
    }

    // --- 2. BAŞLATMA VE RENDER ---
    
    // Welcome Modal Logic
    document.addEventListener("DOMContentLoaded", function() {
         document.getElementById('welcomeModal').style.display = 'flex';
         
         // 250 Tane Log Üretelim!
         for(let i=0; i<250; i++) {
             generatedLogs.push(generateLogEntry(i));
         }
         
         // Kritik Logları (Manuel yazdıklarımız) en başa ekleyelim ki gözüksün
         const manualLogs = [
            { time: new Date().toISOString().replace('T', ' ').substring(0, 19), level: "CRITICAL", host: "FINANCE-SRV-01", msg: "EventCode=4663 Action=WriteData ProcessName=tasksche.exe FilePath=D:\\Finance\\Budget.xlsx.wnry Message='Ransomware activity detected.'" },
            { time: new Date().toISOString().replace('T', ' ').substring(0, 19), level: "CRITICAL", host: "HR-MANAGER-PC", msg: "Sysmon EventID=3 Image=rundll32.exe DestIP=45.133.1.22 Msg='Cobalt Strike Beacon activity'" }
         ];
         generatedLogs = manualLogs.concat(generatedLogs);
         
         renderLogs(generatedLogs);
         initTimeline();
    });

    function closeWelcomeModal() {
        document.getElementById('welcomeModal').style.display = 'none';
    }

    // Detail Modal Logic
    function showDetails(index) {
        const log = generatedLogs[index];
        document.getElementById('detailId').innerText = "ID: " + (10000 + index);
        document.getElementById('dTime').innerText = log.time;
        document.getElementById('dHost').innerText = log.host;
        
        const badge = document.getElementById('dLevel');
        badge.innerText = log.level;
        badge.className = "badge " + log.level.toLowerCase();

        document.getElementById('dRaw').innerText = log.msg; // Basit simülasyon için raw mesajı gösteriyoruz
        document.getElementById('logDetailModal').style.display = 'flex';
    }

    function closeDetailModal() {
        document.getElementById('logDetailModal').style.display = 'none';
    }

    // Görüntüleme Helperları
    function highlightKeywords(text) {
        text = text.replace(/(CRITICAL|ERROR|Fail|Block|Deny|Malicious|Ransomware|Mimikatz)/gi, '<span class="hl-red">$1</span>');
        text = text.replace(/(WARN|High|Suspicious)/gi, '<span class="hl-orange">$1</span>');
        return text; // IP maskelemeyi kaldırdım ki daha gerçekçi dursun
    }

    function renderLogs(logs) {
        const tbody = document.getElementById("logsTableBody");
        const countSpan = document.getElementById("resultCount");
        tbody.innerHTML = "";
        
        // Performans için sadece ilk 100 tanesini renderlayalım (Infinite scroll havası)
        const limit = logs.length > 100 ? 100 : logs.length;
        
        for(let i=0; i<limit; i++) {
            let log = logs[i];
            let row = `<tr onclick="showDetails(${i})">
                <td class="mono time">${log.time}</td>
                <td class="mono"><span class="badge ${log.level.toLowerCase()}">${log.level}</span></td>
                <td class="mono host">${log.host}</td>
                <td class="mono raw">${highlightKeywords(log.msg)}</td>
            </tr>`;
            tbody.innerHTML += row;
        }
        
        countSpan.innerText = logs.length + " events generated (Live)";
    }

    function filterLogs() {
        let input = document.getElementById("searchInput").value.toLowerCase();
        let filtered = generatedLogs.filter(log => {
            return log.msg.toLowerCase().includes(input) || log.host.toLowerCase().includes(input) || log.level.toLowerCase().includes(input);
        });
        renderLogs(filtered);
    }

    function initTimeline() {
        const timeline = document.getElementById("timeline");
        timeline.innerHTML = ""; // Temizle
        for(let i=0; i<80; i++) { // Daha fazla çubuk
            let h = Math.floor(Math.random() * 40) + 5;
            let color = Math.random() > 0.9 ? '#ff5f56' : '#3fb950';
            let bar = `<div style="height:${h}px; background:${color}; width: 100%; opacity:0.7;"></div>`;
            timeline.innerHTML += bar;
        }
    }
</script>

<style>
    /* ===== MODALS ===== */
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

    /* Log Detail Modal */
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
    
    .meta-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; margin-bottom: 20px; }
    .meta-item label { display: block; font-size: 0.7rem; color: #8b949e; margin-bottom: 4px; font-weight: bold; }
    .meta-item span { font-size: 0.9rem; color: #e6edf3; }
    .detail-divider { border: 0; border-top: 1px solid #30363d; margin: 20px 0; }
    
    .raw-section { margin-top: 10px; }
    .raw-header { display: flex; justify-content: space-between; margin-bottom: 5px; font-size: 0.75rem; color: #8b949e; font-weight: bold; }
    .btn-copy { background: transparent; border: 1px solid #30363d; color: #58a6ff; cursor: pointer; padding: 2px 8px; border-radius: 3px; font-size: 0.7rem; }
    .raw-view { background: #000; padding: 15px; border-radius: 4px; border: 1px solid #30363d; color: #3fb950; font-size: 0.85rem; white-space: pre-wrap; word-break: break-all; }

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
    
    /* Results Table Scrollable Area */
    .siem-results { flex: 1; display: flex; flex-direction: column; background: #0d1117; border: 1px solid #30363d; border-radius: 4px; overflow: hidden; }
    .results-header { background: #161b22; padding: 8px 15px; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; align-items: center; }
    #resultCount { font-size: 0.8rem; color: #8b949e; font-weight: bold; }
    .table-scroll-wrapper { overflow-y: auto; flex: 1; } /* Tabloyu kaydırılabilir yapar */

    .pagination button { background: transparent; border: 1px solid #30363d; color: #c9d1d9; padding: 2px 8px; border-radius: 3px; cursor: pointer; font-size: 0.75rem; }
    .pagination button.active { background: #58a6ff; color: #0d1117; border-color: #58a6ff; }
    .logs-table { width: 100%; border-collapse: collapse; font-size: 0.8rem; }
    .logs-table th { background: #161b22; color: #8b949e; text-align: left; padding: 8px 10px; font-weight: normal; border-bottom: 1px solid #30363d; position: sticky; top: 0; z-index: 10; }
    .logs-table td { padding: 6px 10px; border-bottom: 1px solid #21262d; vertical-align: top; color: #c9d1d9; }
    .logs-table tr:hover { cursor: pointer; background-color: rgba(88, 166, 255, 0.08) !important; }
    
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
    
    @media(max-width: 768px) {
        .siem-body { flex-direction: column; }
        .siem-sidebar, .timeline-wrapper { display: none; }
        .search-section { flex-direction: column; }
        .terminal-label { display: none; }
        .meta-grid { grid-template-columns: 1fr 1fr; }
        .disclaimer-body { flex-direction: column; gap: 1rem; }
        .divider { width: 100%; height: 1px; }
    }
</style>
