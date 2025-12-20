---
layout: default
title: SentinelGuard - Log Analytics & Forensics
permalink: /logs/
---

<style>
    /* Full Screen Overrides */
    main { max-width: 100% !important; margin: 0 !important; padding: 0 !important; background: #010409; }
    header, .navbar { max-width: 100% !important; border-bottom: 1px solid #30363d; }

    .siem-shell { font-family: 'Inter', sans-serif; color: #e6edf3; display: flex; flex-direction: column; height: calc(100vh - 60px); }

    /* --- INITIAL DISCLAIMER MODAL --- */
    .disclaimer-modal {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
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

    /* --- SEARCH AREA --- */
    .siem-header { background: #0d1117; padding: 12px 20px; border-bottom: 1px solid #30363d; display: flex; align-items: center; gap: 10px; }
    .search-box { flex: 1; display: flex; background: #161b22; border: 1px solid #30363d; border-radius: 4px; overflow: hidden; }
    .term-prompt { background: #21262d; color: #58a6ff; padding: 8px 12px; font-family: 'JetBrains Mono'; font-size: 0.75rem; border-right: 1px solid #30363d; }
    #searchInput { background: transparent; border: none; color: #e6edf3; padding: 8px 12px; width: 100%; font-family: 'JetBrains Mono'; font-size: 0.8rem; outline: none; }
    .btn-run { background: #238636; border: none; color: #fff; padding: 0 15px; cursor: pointer; font-weight: bold; font-size: 0.75rem; }

    /* --- TABLE DESIGN --- */
    .siem-main { display: flex; flex: 1; overflow: hidden; }
    .siem-content { flex: 1; background: #010409; display: flex; flex-direction: column; }
    .table-scroll { overflow-y: auto; flex: 1; }
    
    .log-table { width: 100%; border-collapse: collapse; font-size: 0.75rem; table-layout: fixed; }
    .log-table th { background: #161b22; color: #8b949e; text-align: left; padding: 10px; position: sticky; top: 0; border-bottom: 1px solid #30363d; z-index: 10; }
    .log-table td { padding: 8px 10px; border-bottom: 1px solid #21262d; vertical-align: top; font-family: 'JetBrains Mono', monospace; word-wrap: break-word; }
    .log-table tr:hover { background: rgba(88, 166, 255, 0.03); }

    /* Action Icons */
    .btn-drill { color: #8b949e; cursor: pointer; margin-right: 10px; transition: 0.2s; }
    .btn-drill:hover { color: #58a6ff; }

    /* Status Badges */
    .lvl { font-weight: bold; padding: 1px 4px; border-radius: 2px; font-size: 0.6rem; border: 1px solid transparent; }
    .lvl-critical { color: #f85149; border-color: rgba(248,81,73,0.3); background: rgba(248,81,73,0.05); }
    .lvl-warn { color: #d29922; border-color: rgba(210,153,34,0.3); background: rgba(210,153,34,0.05); }
    .lvl-info { color: #58a6ff; border-color: rgba(88,166,255,0.3); background: rgba(88,166,255,0.05); }

    /* --- SPLUNK-STYLE MODAL (SURROUNDING EVENTS) --- */
    .surround-modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(1,4,9,0.9); z-index: 9999; display: none; align-items: center; justify-content: center; backdrop-filter: blur(5px); }
    .surround-content { background: #0d1117; border: 1px solid #30363d; width: 90%; height: 80%; border-radius: 8px; display: flex; flex-direction: column; box-shadow: 0 20px 60px rgba(0,0,0,0.7); }
    .surround-header { padding: 15px 20px; background: #161b22; border-bottom: 1px solid #30363d; display: flex; justify-content: space-between; }
    .surround-body { flex: 1; overflow-y: auto; padding: 20px; font-family: 'JetBrains Mono', monospace; font-size: 0.75rem; color: #8b949e; line-height: 1.6; }
    .event-focus { background: rgba(88, 166, 255, 0.1); color: #e6edf3; border-left: 3px solid #58a6ff; padding-left: 10px; margin: 10px 0; }
    
    .hl-red { color: #f85149; font-weight: bold; }
    .hl-blue { color: #58a6ff; }
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
        <div class="search-box">
            <div class="term-prompt">index=security | search</div>
            <input type="text" id="searchInput" placeholder="Search by CaseID (#SOC-5092), IP, or Hash..." onkeyup="filterLogs()">
            <button class="btn-run" onclick="filterLogs()">SEARCH</button>
        </div>
    </div>

    <div class="siem-main">
        <div class="siem-content">
            <div class="table-scroll">
                <table class="log-table">
                    <thead>
                        <tr>
                            <th width="40"></th>
                            <th width="160">_time</th>
                            <th width="80">severity</th>
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

<div id="surroundModal" class="surround-modal">
    <div class="surround-content">
        <div class="surround-header">
            <div style="font-size: 0.8rem; font-weight: bold; color: #58a6ff;">SOURCE VIEW: Surrounding Events (-5s / +5s)</div>
            <span style="cursor:pointer" onclick="closeSurround()">✕</span>
        </div>
        <div class="surround-body" id="surroundBody"></div>
        <div style="padding: 15px; border-top: 1px solid #30363d; text-align: right;">
            <button class="btn-run" style="background:#30363d" onclick="closeSurround()">CLOSE</button>
        </div>
    </div>
</div>

<script>
    const hosts = ["DC-01", "FINANCE-SRV-01", "WEB-SRV-PROD", "FW-PERIMETER", "MAIL-GATEWAY"];
    const users = ["adm_emir", "svc_sql", "system", "jdoe", "anonymous"];
    
    let mainLogs = [
        { time: "2025-12-20 10:15:22", lvl: "CRITICAL", host: "FINANCE-SRV-01", raw: "#SOC-5092 | EventID=11 | Image=tasksche.exe | Target=D:\\Data\\Budget.xlsx.locky | Entropy=7.92 | DestIP=10.20.5.100 | User=svc_backup" },
        { time: "2025-12-20 11:20:45", lvl: "WARN", host: "DC-01", raw: "#SOC-1022 | EventID=4625 | Status=0xc000006d | SubStatus=0xc0000064 | Account=adm_emir | SourceIP=192.168.1.150 | Type=3" },
        { time: "2025-12-20 09:05:12", lvl: "ERROR", host: "WEB-SRV-PROD", raw: "#SOC-883 | src=172.16.45.10 | uri='/products.php?id=1' | payload='UNION SELECT table_name' | agent='sqlmap/1.4.7' | code=200" }
    ];

    function generate() {
        for(let i=0; i<300; i++) {
            let d = new Date(); d.setSeconds(d.getSeconds() - (i * 15));
            mainLogs.push({
                time: d.toISOString().replace('T', ' ').substring(0, 19),
                lvl: i % 15 === 0 ? "WARN" : "INFO",
                host: hosts[Math.floor(Math.random()*hosts.length)],
                raw: `EventID=${Math.floor(Math.random()*8999)+1000} SourceIP=192.168.1.${Math.floor(Math.random()*253)+2} User=${users[Math.floor(Math.random()*users.length)]} Action=${Math.random()>0.5?'Allowed':'Blocked'} proto=TCP port=${Math.floor(Math.random()*65000)+1024}`
            });
        }
        mainLogs.sort((a,b) => new Date(b.time) - new Date(a.time));
    }

    function render(data) {
        document.getElementById('logsBody').innerHTML = data.map((l, i) => `
            <tr>
                <td><span class="btn-drill" onclick="showSurround(${i})">🔍</span></td>
                <td style="color:#8b949e">${l.time}</td>
                <td><span class="lvl lvl-${l.lvl.toLowerCase()}">${l.lvl}</span></td>
                <td style="color:#d29922">${l.host}</td>
                <td style="color:#c9d1d9">${l.raw.replace(/(#SOC-\d+|tasksche|locky|sqlmap|0xc000006d)/gi, '<span class="hl-red">$1</span>')}</td>
            </tr>
        `).join('');
    }

    function showSurround(index) {
        const body = document.getElementById('surroundBody');
        body.innerHTML = "";
        const start = Math.max(0, index - 5);
        const end = Math.min(mainLogs.length - 1, index + 5);
        for(let i=end; i>=start; i--) {
            const l = mainLogs[i];
            body.innerHTML += `<div class="timeline-item ${i === index ? 'event-focus' : ''}">[${l.time}] host=${l.host} lvl=${l.lvl} msg="${l.raw}"</div>`;
        }
        document.getElementById('surroundModal').style.display = 'flex';
    }

    function closeSurround() { document.getElementById('surroundModal').style.display = 'none'; }
    function closeModal(id) { document.getElementById(id).style.display = 'none'; }
    
    function filterLogs() {
        const v = document.getElementById('searchInput').value.toLowerCase();
        render(mainLogs.filter(l => l.raw.toLowerCase().includes(v) || l.host.toLowerCase().includes(v)));
    }

    window.onload = () => {
        document.getElementById('welcomeModal').style.display = 'flex';
        generate();
        render(mainLogs);
    };
</script>
