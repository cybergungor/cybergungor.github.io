---
layout: default
title: Home
permalink: /home/
---

<div class="infinite-log-bg">
    <div class="log-col"><div class="loop-stream s1">
        <span>[INFO] Connection established from 192.168.1.45</span>
        <span>[WARN] Unauthorized access attempt on PORT 22</span>
        <span>[DEBUG] SIEM Correlation Engine: Rule_08 fired</span>
        <span>[INFO] User Emirhan_G session initialized</span>
        <span>[ALERT] Potential Brute Force detected on Endpoint_01</span>
        <span>[INFO] Firewall: Outbound traffic permitted to 8.8.8.8</span>
        <span>SRC: 10.0.0.12 -> DST: 10.0.0.254 [TCP/443]</span>
        <span>MITRE: T1059.001 (PowerShell Execution)</span>
        <span>[INFO] Connection established from 192.168.1.45</span>
        <span>[WARN] Unauthorized access attempt on PORT 22</span>
        <span>[DEBUG] SIEM Correlation Engine: Rule_08 fired</span>
        <span>[INFO] User Emirhan_G session initialized</span>
        <span>[ALERT] Potential Brute Force detected on Endpoint_01</span>
        <span>[INFO] Firewall: Outbound traffic permitted to 8.8.8.8</span>
        <span>SRC: 10.0.0.12 -> DST: 10.0.0.254 [TCP/443]</span>
        <span>MITRE: T1059.001 (PowerShell Execution)</span>
    </div></div>

    <div class="log-col"><div class="loop-stream s2">
        <span>LOC: 41.0082° N, 28.9784° E (Istanbul)</span>
        <span>ISP: TurkNet Communication Services</span>
        <span>ASN: AS12735 | TZ: Europe/Istanbul</span>
        <span>[SUCCESS] Auth: User Emirhan_G logged in</span>
        <span>INDICATOR: Malicious HASH detected (MD5)</span>
        <span>RULE_09: Brute Force Attempt [PORT_22]</span>
        <span>STATUS: ACTIVE_INVESTIGATION</span>
        <span>[INFO] System Kernel updated (v6.1.0)</span>
        <span>LOC: 41.0082° N, 28.9784° E (Istanbul)</span>
        <span>ISP: TurkNet Communication Services</span>
        <span>ASN: AS12735 | TZ: Europe/Istanbul</span>
        <span>[SUCCESS] Auth: User Emirhan_G logged in</span>
        <span>INDICATOR: Malicious HASH detected (MD5)</span>
        <span>RULE_09: Brute Force Attempt [PORT_22]</span>
        <span>STATUS: ACTIVE_INVESTIGATION</span>
        <span>[INFO] System Kernel updated (v6.1.0)</span>
    </div></div>

    <div class="log-col hide-mobile"><div class="loop-stream s3">
        <span>THREAT_LEVEL: LOW | STATUS: SECURE</span>
        <span>CORRELATION_ID: 0x99F2B81A</span>
        <span>LOG_SOURCE: WinEventLog:Security</span>
        <span>ACTION: ALLOW | POLICY: Default_Rule</span>
        <span>[DEBUG] CPU_TEMP: 42°C | FAN: 2100rpm</span>
        <span>0x45 0x6E 0x6D 0x69 0x72 0x68 0x61 0x6E</span>
        <span>ENCRYPTION: AES-256-GCM | MODE: CBC</span>
        <span>[INFO] Heartbeat signal from SOC_Sensor_A</span>
        <span>THREAT_LEVEL: LOW | STATUS: SECURE</span>
        <span>CORRELATION_ID: 0x99F2B81A</span>
        <span>LOG_SOURCE: WinEventLog:Security</span>
        <span>ACTION: ALLOW | POLICY: Default_Rule</span>
        <span>[DEBUG] CPU_TEMP: 42°C | FAN: 2100rpm</span>
        <span>0x45 0x6E 0x6D 0x69 0x72 0x68 0x61 0x6E</span>
        <span>ENCRYPTION: AES-256-GCM | MODE: CBC</span>
        <span>[INFO] Heartbeat signal from SOC_Sensor_A</span>
    </div></div>
    
    <div class="log-col hide-mobile"><div class="loop-stream s1" style="animation-duration: 50s;">
        <span>SRC: 192.168.1.105 -> DST: 8.8.8.8</span>
        <span>[ALERT] Unusual outbound traffic</span>
        <span>STATUS: MONITORING_NODE_04</span>
        <span>[INFO] Update service checking...</span>
        <span>SRC: 192.168.1.105 -> DST: 8.8.8.8</span>
        <span>[ALERT] Unusual outbound traffic</span>
        <span>STATUS: MONITORING_NODE_04</span>
        <span>[INFO] Update service checking...</span>
    </div></div>
</div>

<div class="dashboard-wrapper">
    <div class="dashboard-header">
        <div class="lang-en">
            <span class="status-badge"><span class="pulse-dot"></span> Available for Projects</span>
            <h1 class="main-title">Emirhan Gungoroglu</h1>
            <p class="typewriter-text">./detecting_threats --mode=active</p>
        </div>
        <div class="lang-tr" style="display: none;">
            <span class="status-badge"><span class="pulse-dot"></span> Projelere Açık</span>
            <h1 class="main-title">Emirhan Güngöroğlu</h1>
            <p class="typewriter-text">./tehdit_tespiti --mod=aktif</p>
        </div>
    </div>

    <div class="metrics-bar">
        <div class="metric-item"><span class="metric-label">THREAT_LEVEL:</span> <span class="metric-value status-low">LOW</span></div>
        <div class="metric-item"><span class="metric-label">NETWORK:</span> <span class="metric-value">SECURE</span></div>
        <div class="metric-item"><span class="metric-label">SESSION:</span> <span class="metric-value" id="session-id">LOADING...</span></div>
    </div>

    <div class="bento-container">
        <a href="/blog/" class="bento-card blog-module">
            <div class="card-content">
                <div class="card-icon">📝</div>
                <span class="tag">Knowledge Base</span>
                <h2>Blog</h2>
                <div class="lang-en"><p>In-depth technical write-ups on SOC operations and SIEM rules.</p></div>
                <div class="lang-tr" style="display: none;"><p>SOC operasyonları ve SIEM kuralları üzerine derinlemesine yazılar.</p></div>
            </div>
            <div class="card-footer">Explore Articles &rarr;</div>
        </a>
        <a href="/labs/" class="bento-card">
            <div class="card-content">
                <div class="card-icon">🛡️</div>
                <h2>Labs</h2>
                <p>Security simulations.</p>
            </div>
            <div class="card-footer">Enter &rarr;</div>
        </a>
        <a href="/about/" class="bento-card">
            <div class="card-content">
                <div class="card-icon">👤</div>
                <h2>About</h2>
                <p>The Analyst.</p>
            </div>
            <div class="card-footer">Meet &rarr;</div>
        </a>
    </div>
</div>

<style>
/* --- INFINITE LOG LOOP BACKGROUND --- */
.infinite-log-bg {
    position: fixed;
    top: 0; left: 0; width: 100vw; height: 100vh;
    display: flex; justify-content: space-around;
    opacity: 0.12; pointer-events: none; z-index: -1;
    overflow: hidden; background: #0d1117;
}

.log-col {
    flex: 1; display: flex; flex-direction: column;
    font-family: 'JetBrains Mono', monospace; font-size: 10px;
    color: #58a6ff; padding: 0 10px;
}

.loop-stream {
    display: flex; flex-direction: column;
    animation: scrollDown infinite linear;
}

.loop-stream span {
    padding: 10px 0; white-space: nowrap;
}

/* Animasyon Hızları */
.s1 { animation-duration: 30s; }
.s2 { animation-duration: 45s; }
.s3 { animation-duration: 35s; }

@keyframes scrollDown {
    0% { transform: translateY(-50%); } /* Üstten başla */
    100% { transform: translateY(0); } /* Aşağı ak */
}

/* --- DASHBOARD STYLES --- */
.dashboard-wrapper { max-width: 1100px; margin: 0 auto; padding: 4rem 2rem; position: relative; z-index: 2; }
.main-title { font-size: 3.5rem; color: #fff; font-weight: 800; letter-spacing: -2px; }
.metrics-bar { display: flex; gap: 2rem; margin-bottom: 3rem; background: rgba(22, 27, 34, 0.8); padding: 15px; border-radius: 12px; border: 1px solid #30363d; backdrop-filter: blur(8px); }
.bento-container { display: grid; grid-template-columns: 1.8fr 1fr; gap: 20px; }
.bento-card { background: rgba(22, 27, 34, 0.9); border: 1px solid #30363d; border-radius: 20px; padding: 30px; text-decoration: none !important; transition: 0.3s; }
.bento-card:hover { border-color: #58a6ff; transform: translateY(-5px); }
.blog-module { grid-row: span 2; }

@media (max-width: 850px) {
    .bento-container { grid-template-columns: 1fr; }
    .hide-mobile { display: none; }
    .main-title { font-size: 2.5rem; }
}
</style>

<script>
    document.getElementById('session-id').innerText = Math.random().toString(36).substring(2, 10).toUpperCase();
</script>
