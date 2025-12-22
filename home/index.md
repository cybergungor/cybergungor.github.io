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
        <span>[ALERT] Potential Brute Force detected on Endpoint_01</span>
        <span>SRC: 10.0.0.12 -> DST: 10.0.0.254 [TCP/443]</span>
        <span>MITRE: T1059.001 (PowerShell Execution)</span>
        <span>LOG_SOURCE: WinEventLog:Security</span>
        <span>LOC: 41.0082° N, 28.9784° E (Istanbul)</span>
        <span>[INFO] Connection established from 192.168.1.45</span>
        <span>[WARN] Unauthorized access attempt on PORT 22</span>
        <span>[DEBUG] SIEM Correlation Engine: Rule_08 fired</span>
        <span>[ALERT] Potential Brute Force detected on Endpoint_01</span>
        <span>SRC: 10.0.0.12 -> DST: 10.0.0.254 [TCP/443]</span>
        <span>MITRE: T1059.001 (PowerShell Execution)</span>
        <span>LOG_SOURCE: WinEventLog:Security</span>
        <span>LOC: 41.0082° N, 28.9784° E (Istanbul)</span>
    </div></div>
    <div class="log-col"><div class="stream s2">
        <span>INDICATOR: Malicious HASH detected (MD5)</span>
        <span>RULE_09: Brute Force Attempt [PORT_22]</span>
        <span>[INFO] System Kernel updated (v6.1.0)</span>
        <span>ISP: TurkNet Communication Services</span>
        <span>[SUCCESS] Auth: User Emirhan_G logged in</span>
        <span>0x45 0x6E 0x6D 0x69 0x72 0x68 0x61 0x6E</span>
        <span>ENCRYPTION: AES-256-GCM | MODE: CBC</span>
        <span>STATUS: ACTIVE_INVESTIGATION</span>
        <span>INDICATOR: Malicious HASH detected (MD5)</span>
        <span>RULE_09: Brute Force Attempt [PORT_22]</span>
        <span>[INFO] System Kernel updated (v6.1.0)</span>
        <span>ISP: TurkNet Communication Services</span>
        <span>[SUCCESS] Auth: User Emirhan_G logged in</span>
        <span>0x45 0x6E 0x6D 0x69 0x72 0x68 0x61 0x6E</span>
        <span>ENCRYPTION: AES-256-GCM | MODE: CBC</span>
        <span>STATUS: ACTIVE_INVESTIGATION</span>
    </div></div>
    <div class="log-col hide-mobile"><div class="stream s3">
        <span>[DEBUG] CPU_TEMP: 42°C | FAN: 2100rpm</span>
        <span>[INFO] Heartbeat signal from SOC_Sensor_A</span>
        <span>NETWORK_STATUS: SECURE | THREAT_LEVEL: LOW</span>
        <span>ASN: AS12735 | TZ: Europe/Istanbul</span>
        <span>[INFO] Firewall: Outbound traffic permitted</span>
        <span>INDICATOR: Suspicious LDAP query detected</span>
        <span>0x53 0x4F 0x43 0x5F 0x41 0x6E 0x61 0x6C</span>
        <span>MITRE: T1562.001 (Impair Defenses)</span>
        <span>[DEBUG] CPU_TEMP: 42°C | FAN: 2100rpm</span>
        <span>[INFO] Heartbeat signal from SOC_Sensor_A</span>
        <span>NETWORK_STATUS: SECURE | THREAT_LEVEL: LOW</span>
        <span>ASN: AS12735 | TZ: Europe/Istanbul</span>
        <span>[INFO] Firewall: Outbound traffic permitted</span>
        <span>INDICATOR: Suspicious LDAP query detected</span>
        <span>0x53 0x4F 0x43 0x5F 0x41 0x6E 0x61 0x6C</span>
        <span>MITRE: T1562.001 (Impair Defenses)</span>
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
                <div class="lang-en"><p>In-depth write-ups on SOC operations, SIEM rules, and detection engineering.</p></div>
                <div class="lang-tr" style="display: none;"><p>SOC operasyonları, SIEM kuralları ve tespit mühendisliği üzerine yazılar.</p></div>
            </div>
            <div class="card-footer">Explore Articles &rarr;</div>
        </a>

        <a href="/labs/" class="bento-card">
            <div class="card-content">
                <div class="card-icon">🛡️</div>
                <span class="tag">Practical</span>
                <h2>Labs</h2>
                <div class="lang-en"><p>Interactive security scenarios and simulation walkthroughs.</p></div>
                <div class="lang-tr" style="display: none;"><p>Etkileşimli güvenlik senaryoları ve simülasyon çözümleri.</p></div>
            </div>
            <div class="card-footer">Launch Station &rarr;</div>
        </a>

        <a href="/about/" class="bento-card">
            <div class="card-content">
                <div class="card-icon">👤</div>
                <span class="tag">Identity</span>
                <h2>About</h2>
                <div class="lang-en"><p>Blue Team enthusiast documenting a cybersecurity journey.</p></div>
                <div class="lang-tr" style="display: none;"><p>Siber güvenlik yolculuğunu belgeleyen Blue Team meraklısı.</p></div>
            </div>
            <div class="card-footer">Meet Operator &rarr;</div>
        </a>
    </div>
</div>

<style>
/* --- 1. INFINITE LOG BACKGROUND (FIXED & FULL) --- */
.infinite-log-bg {
    position: fixed;
    top: 0; left: 0; width: 100vw; height: 100vh;
    display: flex; justify-content: space-around;
    opacity: 0.12; pointer-events: none; z-index: -1;
    overflow: hidden; background: #0d1117;
}
.log-col { flex: 1; display: flex; flex-direction: column; font-family: 'JetBrains Mono', monospace; font-size: 10px; color: #58a6ff; }
.loop-stream { display: flex; flex-direction: column; animation: scrollDown infinite linear; }
.loop-stream span { padding: 12px 0; white-space: nowrap; }
.s1 { animation-duration: 35s; } .s2 { animation-duration: 50s; } .s3 { animation-duration: 40s; }
@keyframes scrollDown { 0% { transform: translateY(-50%); } 100% { transform: translateY(0); } }

/* --- 2. DASHBOARD LAYOUT & WRAPPER --- */
.dashboard-wrapper { max-width: 1100px; margin: 0 auto; padding: 4rem 2rem; position: relative; z-index: 2; }

/* Typewriter Effect */
.main-title { font-size: clamp(2.5rem, 7vw, 4rem); color: #fff; font-weight: 800; letter-spacing: -2.5px; margin: 0.5rem 0; }
.typewriter-text {
    font-family: 'JetBrains Mono', monospace; color: #58a6ff; font-size: 1.2rem;
    border-right: 3px solid #58a6ff; width: fit-content; white-space: nowrap; overflow: hidden;
    animation: typing 4s steps(30, end) infinite, blink 0.8s step-end infinite;
}
@keyframes typing { 0%, 10% { width: 0 } 50%, 90% { width: 100% } 100% { width: 0 } }
@keyframes blink { 50% { border-color: transparent } }

/* Status Badge */
.status-badge { display: inline-flex; align-items: center; gap: 8px; background: rgba(63, 185, 80, 0.1); color: #3fb950; padding: 6px 14px; border-radius: 20px; font-size: 11px; font-weight: bold; }
.pulse-dot { width: 8px; height: 8px; background: #3fb950; border-radius: 50%; animation: pulse 2s infinite; }
@keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.4; } 100% { opacity: 1; } }

/* --- 3. METRICS BAR --- */
.metrics-bar { display: flex; justify-content: space-between; gap: 1rem; margin-bottom: 3rem; padding: 15px 25px; background: rgba(22, 27, 34, 0.8); border: 1px solid #30363d; border-radius: 12px; font-family: 'JetBrains Mono', monospace; font-size: 11px; backdrop-filter: blur(8px); }
.metric-value { color: #f0f6fc; font-weight: bold; }
.status-low { color: #3fb950; text-shadow: 0 0 5px rgba(63, 185, 80, 0.5); }

/* --- 4. BENTO CONTAINER (TABLE STYLE) --- */
.bento-container { display: grid; grid-template-columns: 1.8fr 1fr; gap: 24px; }
.bento-card { background: rgba(22, 27, 34, 0.9); border: 1px solid #30363d; border-radius: 24px; padding: 32px; text-decoration: none !important; display: flex; flex-direction: column; justify-content: space-between; min-height: 220px; transition: 0.4s; }
.bento-card:hover { border-color: #58a6ff; transform: translateY(-8px); box-shadow: 0 10px 40px rgba(0,0,0,0.5); }
.blog-module { grid-row: span 2; }
.card-icon { font-size: 2rem; margin-bottom: 1rem; }
.tag { font-family: 'JetBrains Mono', monospace; color: #58a6ff; font-size: 10px; text-transform: uppercase; letter-spacing: 2px; font-weight: 800; }
.bento-card h2 { color: #fff; font-size: 1.8rem; margin: 10px 0; border: none; padding: 0; }
.bento-card p { color: #8b949e; font-size: 0.95rem; line-height: 1.5; }
.card-footer { margin-top: 20px; color: #fff; font-size: 13px; opacity: 0.4; font-weight: bold; }
.bento-card:hover .card-footer { opacity: 1; }

@media (max-width: 850px) {
    .bento-container { grid-template-columns: 1fr; }
    .blog-module { grid-row: span 1; }
    .hide-mobile { display: none; }
}
</style>

<script>
    document.getElementById('session-id').innerText = Math.random().toString(36).substring(2, 10).toUpperCase();
</script>
