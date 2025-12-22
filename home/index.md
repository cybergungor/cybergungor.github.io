---
layout: default
title: Home
permalink: /home/
---

<div class="infinite-log-bg">
    <div class="log-col"><div class="loop-stream s1">
        <span>[INFO] Connection established: 192.168.1.45</span>
        <span>[WARN] Unauthorized access PORT 22</span>
        <span>[DEBUG] SIEM Correlation: Rule_08 fired</span>
        <span>[ALERT] Brute Force detected: Endpoint_01</span>
        <span>SRC: 10.0.0.12 -> DST: 10.0.0.254 [TCP/443]</span>
        <span>MITRE: T1059.001 (PowerShell Execution)</span>
        <span>[INFO] Connection established: 192.168.1.45</span>
        <span>[WARN] Unauthorized access PORT 22</span>
    </div></div>
    <div class="log-col hide-mobile"><div class="loop-stream s2">
        <span>INDICATOR: Malicious HASH detected (MD5)</span>
        <span>RULE_09: Brute Force Attempt [PORT_22]</span>
        <span>[INFO] System Kernel updated (v6.1.0)</span>
        <span>ISP: TurkNet Communication Services</span>
        <span>[SUCCESS] Auth: User Emirhan_G logged in</span>
        <span>ENCRYPTION: AES-256-GCM | MODE: CBC</span>
        <span>INDICATOR: Malicious HASH detected (MD5)</span>
        <span>RULE_09: Brute Force Attempt [PORT_22]</span>
    </div></div>
    <div class="log-col"><div class="loop-stream s3">
        <span>LOC: 41.0082° N, 28.9784° E (Istanbul)</span>
        <span>NETWORK_STATUS: SECURE | THREAT: LOW</span>
        <span>ASN: AS12735 | TZ: Europe/Istanbul</span>
        <span>[INFO] Firewall: Outbound traffic permitted</span>
        <span>LOC: 41.0082° N, 28.9784° E (Istanbul)</span>
        <span>NETWORK_STATUS: SECURE | THREAT: LOW</span>
    </div></div>
</div>

<div class="dashboard-wrapper">
    <div class="dashboard-header">
        <div class="lang-en">
            <span class="status-badge"><span class="pulse-dot"></span> Available for Projects</span>
            <h1 class="main-title">Emirhan Gungoroglu</h1>
            <div class="command-line">
                <span class="prompt">root@cyberlab:~$</span>
                <span class="typewriter-text">./detect_threats --active</span>
            </div>
        </div>
        <div class="lang-tr" style="display: none;">
            <span class="status-badge"><span class="pulse-dot"></span> Projelere Açık</span>
            <h1 class="main-title">Emirhan Güngöroğlu</h1>
            <div class="command-line">
                <span class="prompt">root@siberlab:~$</span>
                <span class="typewriter-text">./tehdit_algila --aktif</span>
            </div>
        </div>
    </div>

    <div class="metrics-bar">
        <div class="metric-item">
            <label>THREAT LEVEL</label>
            <span class="val success">LOW_DETECTION</span>
        </div>
        <div class="metric-item">
            <label>SYSTEM STATUS</label>
            <span class="val">OPERATIONAL</span>
        </div>
        <div class="metric-item">
            <span class="val" id="session-id">LOADING...</span>
        </div>
    </div>

    <div class="bento-container">
        <a href="/blog/" class="bento-card blog-module">
            <div class="card-inner">
                <div class="card-head"><span class="tag">01 / DATABASE</span> 📝</div>
                <h2>Blog / Write-ups</h2>
                <p>Technical write-ups and learning notes about Blue Team operations and SOC workflows.</p>
                <div class="footer-link">EXPLORE_DATA &rarr;</div>
            </div>
        </a>

        <a href="/labs/" class="bento-card">
            <div class="card-inner">
                <div class="card-head"><span class="tag">02 / RANGE</span> 🛡️</div>
                <h2>Labs / Simulation</h2>
                <p>Interactive security scenarios and hands-on incident response labs.</p>
                <div class="footer-link">INIT_LAB &rarr;</div>
            </div>
        </a>

        <a href="/about/" class="bento-card">
            <div class="card-inner">
                <div class="card-head"><span class="tag">03 / BIO</span> 👤</div>
                <h2>About / Operator</h2>
                <p>The mission and background of Emirhan Gungoroglu.</p>
                <div class="footer-link">READ_BIO &rarr;</div>
            </div>
        </a>
    </div>
</div>

<style>
/* --- TACTICAL CORE STYLES --- */
:root {
    --bg: #08090a;
    --card-bg: rgba(17, 18, 20, 0.85);
    --accent: #00f2ff; /* Daha keskin ve profesyonel bir turkuaz/mavi */
    --border: rgba(255, 255, 255, 0.08);
}

body {
    background-color: var(--bg);
    color: #c9d1d9;
    font-family: 'Inter', -apple-system, sans-serif;
    margin: 0;
}

/* --- LOG BACKGROUND (Okunabilir ve Tüm Ekranda) --- */
.infinite-log-bg {
    position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
    display: flex; justify-content: space-around; opacity: 0.08;
    z-index: -1; pointer-events: none; font-family: 'JetBrains Mono', monospace; font-size: 11px;
}
.loop-stream { display: flex; flex-direction: column; animation: scrollDown infinite linear; }
.loop-stream span { padding: 15px 0; white-space: nowrap; color: var(--accent); }
.s1 { animation-duration: 35s; } .s2 { animation-duration: 50s; } .s3 { animation-duration: 40s; }
@keyframes scrollDown { 0% { transform: translateY(-50%); } 100% { transform: translateY(0); } }

/* --- DASHBOARD WRAPPER --- */
.dashboard-wrapper { max-width: 1100px; margin: 0 auto; padding: 4rem 2rem; position: relative; }

.main-title { 
    font-size: 3.5rem; font-weight: 800; letter-spacing: -2px; color: #fff; margin: 10px 0; 
    text-shadow: 0 0 20px rgba(0, 242, 255, 0.1);
}

.command-line { display: flex; gap: 10px; font-family: 'JetBrains Mono'; color: var(--accent); margin-bottom: 2rem; }
.typewriter-text { border-right: 2px solid var(--accent); animation: blink 0.8s infinite; }

/* --- METRICS BAR --- */
.metrics-bar { 
    display: flex; justify-content: space-between; align-items: center;
    background: var(--card-bg); border: 1px solid var(--border);
    padding: 15px 30px; border-radius: 12px; margin-bottom: 3rem;
    backdrop-filter: blur(10px);
}
.metric-item label { display: block; font-size: 9px; color: #8b949e; letter-spacing: 1px; }
.metric-item .val { font-family: 'JetBrains Mono'; font-weight: bold; font-size: 13px; }
.success { color: #3fb950; }

/* --- BENTO CARDS --- */
.bento-container { display: grid; grid-template-columns: 1.8fr 1fr; gap: 20px; }
.bento-card { 
    background: var(--card-bg); border: 1px solid var(--border);
    border-radius: 24px; padding: 35px; text-decoration: none !important;
    transition: 0.4s cubic-bezier(0.2, 0, 0, 1); backdrop-filter: blur(12px);
}
.blog-module { grid-row: span 2; }

.bento-card:hover {
    border-color: var(--accent);
    transform: translateY(-5px);
    background: rgba(22, 24, 28, 0.95);
    box-shadow: 0 10px 40px rgba(0,0,0,0.4), 0 0 20px rgba(0, 242, 255, 0.05);
}

.card-head { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
.tag { font-family: 'JetBrains Mono'; font-size: 10px; color: var(--accent); letter-spacing: 1px; }
.bento-card h2 { color: #fff; font-size: 1.8rem; margin: 0 0 15px 0; }
.bento-card p { color: #8b949e; line-height: 1.6; font-size: 0.95rem; }
.footer-link { margin-top: 25px; font-size: 12px; font-weight: bold; color: var(--accent); }

@keyframes blink { 50% { border-color: transparent; } }
@media (max-width: 850px) { .bento-container { grid-template-columns: 1fr; } .main-title { font-size: 2.5rem; } }
</style>

<script>
    document.getElementById('session-id').innerText = "SID_" + Math.random().toString(36).substring(2, 8).toUpperCase();
</script>
