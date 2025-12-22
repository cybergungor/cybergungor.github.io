---
layout: default
title: Home
permalink: /home/
---

<div class="log-background-grid">
    <div class="log-column">
        <div class="log-stream">
            SRC: 192.168.1.105 -> DST: 8.8.8.8 [UDP/53] <br>
            PACKET_SIZE: 1500 bytes | TTL: 64 <br>
            PROTO: ICMP | TYPE: ECHO_REQUEST <br>
            SRC: 172.16.0.5 -> DST: 172.16.0.1 [TCP/22] <br>
            SRC: 192.168.1.105 -> DST: 8.8.8.8 [UDP/53] <br>
            PROTO: ICMP | TYPE: ECHO_REQUEST <br>
            SRC: 172.16.0.5 -> DST: 172.16.0.1 [TCP/22] <br>
            SRC: 192.168.1.105 -> DST: 8.8.8.8 [UDP/53] <br>
            SRC: 192.168.1.105 -> DST: 8.8.8.8 [UDP/53] <br>
            PACKET_SIZE: 1500 bytes | TTL: 64 <br>
            PROTO: ICMP | TYPE: ECHO_REQUEST <br>
            SRC: 172.16.0.5 -> DST: 172.16.0.1 [TCP/22] <br>
        </div>
    </div>
    <div class="log-column">
        <div class="log-stream delay-1">
            [INFO] System Kernel updated (v6.1.0) <br>
            [SUCCESS] Auth: User Emirhan_G logged in <br>
            [DEBUG] CPU_TEMP: 42°C | FAN_SPEED: 2100rpm <br>
            [INFO] Service: Nginx restarted <br>
            [SUCCESS] Auth: User Emirhan_G logged in <br>
            [DEBUG] CPU_TEMP: 42°C | FAN_SPEED: 2100rpm <br>
            [INFO] System Kernel updated (v6.1.0) <br>
            [SUCCESS] Auth: User Emirhan_G logged in <br>
            [DEBUG] CPU_TEMP: 42°C | FAN_SPEED: 2100rpm <br>
            [INFO] System Kernel updated (v6.1.0) <br>
            [SUCCESS] Auth: User Emirhan_G logged in <br>
            [DEBUG] CPU_TEMP: 42°C | FAN_SPEED: 2100rpm <br>
        </div>
    </div>
    <div class="log-column">
        <div class="log-stream delay-2">
            MITRE: T1059.001 (PowerShell Execution) <br>
            THREAT_INTEL: IP 45.33.22.11 blacklisted <br>
            INDICATOR: Malicious HASH detected <br>
            POLICY: Blocked traffic RU/CN <br>
            ALERT: Suspicious LDAP query <br>
            MITRE: T1059.001 (PowerShell Execution) <br>
            INDICATOR: Malicious HASH detected <br>
            ALERT: Suspicious LDAP query <br>
            MITRE: T1059.001 (PowerShell Execution) <br>
            THREAT_INTEL: IP 45.33.22.11 blacklisted <br>
            INDICATOR: Malicious HASH detected <br>
            POLICY: Blocked traffic RU/CN <br>
        </div>
    </div>
    <div class="log-column">
        <div class="log-stream delay-3">
            LOC: 41.0082 N, 28.9784 E (Istanbul) <br>
            ISP: TurkNet Services <br>
            ASN: AS12735 | TZ: Europe/Istanbul <br>
            OS: Linux_x86_64 <br>
            BROWSER: BlueTeam_Analyst <br>
            LOC: 41.0082 N, 28.9784 E <br>
            ASN: AS12735 | TZ: Europe/Istanbul <br>
            OS: Linux_x86_64 <br>
            LOC: 41.0082 N, 28.9784 E (Istanbul) <br>
            ISP: TurkNet Services <br>
            ASN: AS12735 | TZ: Europe/Istanbul <br>
            OS: Linux_x86_64 <br>
        </div>
    </div>
    <div class="log-column">
        <div class="log-stream delay-4">
            RULE_09: Brute Force Attempt [22] <br>
            LOG_SOURCE: WinEventLog:Security <br>
            ACTION: DROPPED | BY: Cisco_FW <br>
            CORRELATION_ID: 0x99F2B81A <br>
            STATUS: ACTIVE_INVESTIGATION <br>
            ACTION: DROPPED | BY: Cisco_FW <br>
            RULE_09: Brute Force Attempt [22] <br>
            LOG_SOURCE: WinEventLog:Security <br>
            RULE_09: Brute Force Attempt [22] <br>
            LOG_SOURCE: WinEventLog:Security <br>
            ACTION: DROPPED | BY: Cisco_FW <br>
            CORRELATION_ID: 0x99F2B81A <br>
        </div>
    </div>
     <div class="log-column">
        <div class="log-stream delay-1">
            0x45 0x6E 0x6D 0x69 0x72 0x68 0x61 0x6E <br>
            TOKEN: {{ site.time | date: "%H%M%S" }} <br>
            STATUS: MONITORING_NODE_01 <br>
            UPLINK: 1Gbps | DOWMLINK: 1Gbps <br>
            ENCRYPTION: AES-256-GCM <br>
            0x45 0x6E 0x6D 0x69 0x72 0x68 0x61 0x6E <br>
            TOKEN: {{ site.time | date: "%H%M%S" }} <br>
            UPLINK: 1Gbps | DOWMLINK: 1Gbps <br>
            0x45 0x6E 0x6D 0x69 0x72 0x68 0x61 0x6E <br>
            TOKEN: {{ site.time | date: "%H%M%S" }} <br>
            STATUS: MONITORING_NODE_01 <br>
            UPLINK: 1Gbps | DOWMLINK: 1Gbps <br>
        </div>
    </div>
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

        <a href="/labs/" class="bento-card labs-module">
            <div class="card-content">
                <div class="card-icon">🛡️</div>
                <span class="tag">Simulations</span>
                <h2>Labs</h2>
                <div class="lang-en"><p>Interactive security scenarios and walkthroughs.</p></div>
                <div class="lang-tr" style="display: none;"><p>Etkileşimli güvenlik senaryoları ve çözüm rehberleri.</p></div>
            </div>
            <div class="card-footer">Launch Station &rarr;</div>
        </a>

        <a href="/about/" class="bento-card about-module">
            <div class="card-content">
                <div class="card-icon">👤</div>
                <span class="tag">Identity</span>
                <h2>About</h2>
                <div class="lang-en"><p>Blue Team enthusiast and security student.</p></div>
                <div class="lang-tr" style="display: none;"><p>Blue Team meraklısı ve siber güvenlik öğrencisi.</p></div>
            </div>
            <div class="card-footer">Meet Operator &rarr;</div>
        </a>
    </div>
</div>

<style>
/* --- FULL SCREEN LOG BACKGROUND (FIXED) --- */
.log-background-grid {
    position: fixed;
    top: 0; left: 0; width: 100%; height: 100%;
    display: flex;
    justify-content: space-around; /* Sütunları tüm yataya yayar */
    opacity: 0.1; /* Göz yormaması için düşük opaklık */
    pointer-events: none;
    z-index: -1;
    overflow: hidden;
    background-color: #0d1117; /* Siyah zemin */
}

.log-column {
    flex: 1;
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px;
    line-height: 2;
    color: #58a6ff; /* Mavi log rengi */
    text-align: left;
    white-space: nowrap;
}

.log-stream {
    display: flex;
    flex-direction: column;
    animation: scrollLogs 40s linear infinite;
}

/* Farklı hızlar ve gecikmelerle tüm alanı dolduruyoruz */
.delay-1 { animation-duration: 55s; animation-delay: -5s; }
.delay-2 { animation-duration: 45s; animation-delay: -15s; }
.delay-3 { animation-duration: 65s; animation-delay: -2s; }
.delay-4 { animation-duration: 50s; animation-delay: -10s; }

@keyframes scrollLogs {
    0% { transform: translateY(0); }
    100% { transform: translateY(-50%); } /* Döngünün kesilmemesi için içerik içerde 2-3 kez tekrarlanmalı */
}

/* --- DASHBOARD WRAPPER --- */
.dashboard-wrapper { 
    max-width: 1100px; margin: 0 auto; padding: 3rem 1.5rem;
    position: relative; z-index: 2;
}

.dashboard-header { margin-bottom: 2rem; }
.main-title { font-size: 3.5rem; color: #fff; font-weight: 800; letter-spacing: -2px; }
.typewriter-text { font-family: 'JetBrains Mono', monospace; color: #58a6ff; font-size: 1.2rem; }

/* --- METRICS & BENTO --- */
.metrics-bar { display: flex; gap: 2rem; margin-bottom: 3rem; background: rgba(22, 27, 34, 0.8); padding: 15px; border-radius: 12px; border: 1px solid #30363d; backdrop-filter: blur(5px); }
.bento-container { display: grid; grid-template-columns: 1.8fr 1fr; gap: 20px; }
.bento-card { background: rgba(22, 27, 34, 0.9); border: 1px solid #30363d; border-radius: 20px; padding: 30px; text-decoration: none !important; transition: 0.3s; }
.bento-card:hover { border-color: #58a6ff; transform: translateY(-5px); box-shadow: 0 0 20px rgba(88,166,255,0.1); }
.blog-module { grid-row: span 2; }
.tag { font-size: 10px; color: #58a6ff; text-transform: uppercase; letter-spacing: 2px; }
.bento-card h2 { color: #fff; margin: 10px 0; font-size: 1.8rem; }
.bento-card p { color: #8b949e; font-size: 0.95rem; }
.card-footer { margin-top: 20px; color: #fff; font-size: 13px; opacity: 0.5; }

@media (max-width: 850px) {
    .bento-container { grid-template-columns: 1fr; }
    .log-background-grid { opacity: 0.05; } /* Mobilde daha silik */
}
</style>

<script>
    document.getElementById('session-id').innerText = Math.random().toString(36).substring(2, 10).toUpperCase();
</script>
