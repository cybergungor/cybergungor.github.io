---
layout: default
title: SentinelGuard - Log Analytics
permalink: /logs/
---

<style>
    /* Full Screen Overrides */
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    header, .navbar { max-width: 100% !important; border-bottom: 1px solid #30363d; }

    .siem-shell {
        font-family: 'Inter', -apple-system, sans-serif;
        color: #e6edf3;
        display: flex;
        flex-direction: column;
        height: calc(100vh - 60px);
    }

    /* --- BILGILENDIRME MODALI (DISCLAIMER) --- */
    .disclaimer-modal {
        position: fixed; top:0; left:0; width:100%; height:100%;
        background: rgba(1, 4, 9, 0.98); z-index: 10000;
        display: flex; align-items: center; justify-content: center;
        backdrop-filter: blur(12px);
    }
    .disclaimer-box {
        background: #0d1117; border: 1px solid #30363d; width: 750px;
        border-radius: 8px; overflow: hidden; box-shadow: 0 0 50px rgba(88, 166, 255, 0.1);
    }
    .disclaimer-header {
        background: rgba(248, 81, 73, 0.1); color: #f85149;
        padding: 15px; font-family: 'JetBrains Mono', monospace;
        font-weight: bold; text-align: center; border-bottom: 1px solid rgba(248, 81, 73, 0.3);
    }
    .disclaimer-body { padding: 30px; display: flex; gap: 30px; }
    .lang-section { flex: 1; }
    .lang-section h3 { font-size: 0.85rem; color: #58a6ff; margin-bottom: 15px; font-family: 'JetBrains Mono'; border-bottom: 1px solid #30363d; padding-bottom: 8px; }
    .lang-section p { font-size: 0.85rem; color: #8b949e; line-height: 1.6; }
    .v-divider { width: 1px; background: #30363d; }
    .btn-ack {
        width: 100%; padding: 15px; background: #238636; color: #fff;
        border: none; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono';
    }

    /* --- SEARCH & HEADER --- */
    .siem-header { background: #0d1117; padding: 15px 25px; border-bottom: 1px solid #30363d; display: flex; align-items: center; }
    .search-container { flex: 1; display: flex; background: #161b22; border: 1px solid #30363d; border-radius: 4px; align-items: center; }
    .terminal-prompt { background: #21262d; color: #58a6ff; padding: 10px 15px; font-family: 'JetBrains Mono'; font-size: 0.8rem; border-right: 1px solid #30363d; white-space: nowrap; }
    #searchInput { background: transparent; border: none; color: #e6edf3; padding: 10px 15px; width: 100%; font-family: 'JetBrains Mono'; font-size: 0.85rem; }
    .btn-run { background: #238636; border: none; color: #fff; padding: 0 20px; height: 40px; cursor: pointer; font-weight: bold; }

    /* --- LAYOUT --- */
    .timeline-strip { background: #0d1117; height: 40px; border-bottom: 1px solid #30363d; display: flex; align-items: flex-end; padding: 0 25px; gap: 2px; }
    .siem-main { display: flex; flex: 1; overflow: hidden; }
    .siem-aside { width: 260px; background: #0d1117; border-right: 1px solid #30363d; padding: 20px; }
    .aside-section h3 { font-size: 0.7rem; color: #8b949e; text-transform: uppercase; margin-bottom: 15px; border-bottom: 1px solid #21262d; }
    .field-item { display: flex; justify-content: space-between; font-size: 0.75rem; padding: 6px 0; color: #58a6ff; cursor: pointer; }
    
    .siem-content { flex: 1; background: #010409; display: flex; flex-direction: column; overflow: hidden; }
    .results-toolbar { background: #0d1117; padding: 10px 25px; border-bottom: 1px solid #30363d; font-size: 0.75rem; color: #8b949e; display: flex; justify-content: space-between; }
    .table-wrapper { overflow-y: auto; flex: 1; }
    .log-table { width: 100%; border-collapse: collapse; font-size: 0.8rem; }
    .log-table th { background: #161b22; color: #8b949e; padding: 10px 15px; text-align: left; position: sticky; top: 0; border-bottom: 1px solid #30363d; }
    .log-table td { padding: 10px 15px; border-bottom: 1px solid #21262d; font-family: 'JetBrains Mono'; color: #c9d1d9; }
    .log-table tr:hover { background: rgba(88, 166, 255, 0.05); cursor: pointer; }

    .lvl { font-weight: bold; font-size: 0.65rem; border-radius: 2px; padding: 2px 5px; }
    .lvl-critical { color: #f85149; border: 1px solid rgba(248, 81, 73, 0.4); }
    .lvl-warn { color: #d29922; border: 1px solid rgba(210, 153, 34, 0.4); }
    .lvl-info { color: #58a6ff; border: 1px solid rgba(88, 166, 255, 0.4); }
    .highlight-red { color: #f85149; font-weight: bold; }

    /* Inspector Modal */
    .inspector-modal { position: fixed; top:0; left:0; width:100%; height:100%; background: rgba(1, 4, 9, 0.85); z-index: 9000; display: none; align-items: center; justify-content: center; }
    .inspector-content { background: #0d1117; border: 1px solid #58a6ff; width: 800px; border-radius: 8px; padding: 30px; box-shadow: 0 0 40px rgba(88, 166, 255, 0.2); }
</style>

<div id="welcomeModal" class="disclaimer-modal">
    <div class="disclaimer-box">
        <div class="disclaimer-header">
            SYSTEM ACCESS MESSAGE // SİSTEM ERİŞİM MESAJI
        </div>
        <div class="disclaimer-body">
            <div class="lang-section">
                <h3>[TR] SİMÜLASYON ORTAMI</h3>
                <p>Bu arayüz gerçek dünya SIEM ürünlerinin (Splunk, Sentinel vb.) birebir replikasıdır.</p>
                <p style="margin-top:10px;"><strong>Analiz:</strong> Alerts kısmındaki vakaları çözmek için buradaki log telemetrisini kullanın.</p>
            </div>
            <div class="v-divider"></div>
            <div class="lang-section">
                <h3>[EN] SIMULATION MODE</h3>
                <p>This is a high-fidelity replica of real-world SIEM platforms used in modern SOCs.</p>
                <p style="margin-top:10px;"><strong>Task:</strong> Use the log telemetry here to investigate and solve the playbooks in the Alerts dashboard.</p>
            </div>
        </div>
        <button class="btn-ack" onclick="closeModal('welcomeModal')">ACKNOWLEDGE & INITIALIZE SYSTEM</button>
    </div>
</div>

<div class="siem-shell">
    <div class="siem-header">
        <div class="search-container">
            <div class="terminal-prompt">index=main | search</div>
            <input type="text" id="searchInput" placeholder="Search by Case ID (#SOC-5092), IP, or process..." onkeyup="filterLogs()">
            <button class="btn-run" onclick="filterLogs()">RUN QUERY</button>
        </div>
    </div>

    <div class="timeline-strip" id="timeline"></div>

    <div class="siem-main">
        <div class="siem-aside">
            <div class="aside-section">
                <h3>Selected Fields</h3>
                <div class="field-item">host <span class="field-count">10</span></div>
                <div class="field-item">source_ip <span class="field-count">142</span></div>
            </div>
            <div class="aside-section" style="margin-top: 30px;">
                <h3>Interesting Fields</h3>
                <div class="field-item">event_code <span class="field-count">4688</span></div>
                <div class="field-item">user_account <span class="field-count">8</span></div>
                <div class="field-item">mitre_id <span class="field-count">3</span></div>
            </div>
        </div>

        <div class="siem-content">
            <div class="results-toolbar">
                <span id="resultCount">Querying...</span>
                <span>Real-time Forensic Stream</span>
            </div>
            <div class="table-wrapper">
                <table class="log-table">
                    <thead>
                        <tr>
                            <th width="180">Timestamp</th>
                            <th width="80">Level</th>
                            <th width="140">Host</th>
                            <th>Raw Telemetry Stream</th>
                        </tr>
                    </thead>
                    <tbody id="logsBody"></tbody>
                </table>
            </div>
        </div>
    </div>
</div>

<div id="inspectorModal" class="inspector-modal">
    <div class="inspector-content">
        <h2 style="color:#58a6ff; font-family:'JetBrains Mono'; font-size:1rem; margin:0;">EVENT INSPECTOR</h2>
        <hr style="border:0; border-top:1px solid #30363d; margin:20px 0;">
        <div id="insBody" style="font-family:'JetBrains Mono'; font-size:0.85rem; color:#c9d1d9; line-height:1.6;"></div>
        <button class="btn-ack" style="margin-top:30px;" onclick="closeModal('inspectorModal')">CLOSE INSPECTOR</button>
    </div>
</div>

<script>
    const hosts = ["FINANCE-SRV-01", "DC-01", "WEB-SRV-PROD", "FILE-SRV-02", "FW-PERIMETER"];
    const levels = ["INFO", "INFO", "WARN", "ERROR"];
    
    let logs = [
        { time: "2025-12-20 10:15:22", level: "CRITICAL", host: "FINANCE-SRV-01", msg: "#SOC-5092 | SysmonID=11 | Image=tasksche.exe | Target=D:\\Data\\Budget.xlsx.locky | Entropy=7.92 | TargetIP=10.20.5.100" },
        { time: "2025-12-20 11:20:45", level: "WARN", host: "DC-01", msg: "#SOC-1022 | WinEventID=4625 | Account=adm_emir | Reason=0xc000006d | SourceIP=192.168.1.150 | Type=3" },
        { time: "2025-12-20 09:05:12", level: "ERROR", host: "WEB-SRV-PROD", msg: "#SOC-883 | ApacheLog | src=172.16.45.10 | payload='products.php?id=1 UNION SELECT' | agent=sqlmap/1.4.7" }
    ];

    function generate() {
        for(let i=0; i<250; i++) {
            let d = new Date(); d.setSeconds(d.getSeconds() - (i * 20));
            logs.push({
                time: d.toISOString().replace('T', ' ').substring(0, 19),
                level: levels[Math.floor(Math.random()*levels.length)],
                host: hosts[Math.floor(Math.random()*hosts.length)],
                msg: `EventID=${Math.floor(Math.random()*9000)} SourceIP=192.168.1.${Math.floor(Math.random()*255)} Action=Allow User=svc_system`
            });
        }
        logs.sort((a,b) => new Date(b.time) - new Date(a.time));
    }

    function render(data) {
        document.getElementById('logsBody').innerHTML = data.map((l, i) => `
            <tr onclick="inspect(${i})">
                <td style="color:#8b949e">${l.time}</td>
                <td><span class="lvl lvl-${l.level.toLowerCase()}">${l.level}</span></td>
                <td style="color:#d29922">${l.host}</td>
                <td style="color:#8b949e">${l.msg.replace(/(#SOC-\d+|sqlmap|tasksche|locky)/gi, '<span class="highlight-red">$1</span>')}</td>
            </tr>
        `).join('');
        document.getElementById('resultCount').innerText = `${data.length} events indexed`;
    }

    function filterLogs() {
        const v = document.getElementById('searchInput').value.toLowerCase();
        render(logs.filter(l => l.msg.toLowerCase().includes(v) || l.host.toLowerCase().includes(v)));
    }

    function inspect(i) {
        const l = logs[i];
        document.getElementById('insBody').innerHTML = `<strong>TIMESTAMP:</strong> ${l.time}<br><strong>HOST:</strong> ${l.host}<br><br><strong>PAYLOAD:</strong><br><div style="background:#010409; padding:15px; border:1px solid #30363d; color:#3fb950;">${l.msg}</div>`;
        document.getElementById('inspectorModal').style.display = 'flex';
    }

    function closeModal(id) { document.getElementById(id).style.display = 'none'; }

    window.onload = () => {
        generate(); render(logs);
        const timeline = document.getElementById('timeline');
        for(let i=0; i<100; i++) timeline.innerHTML += `<div style="height:${Math.random()*30+5}px; width:4px; background:#238636; opacity:0.6;"></div>`;
    };
</script>
