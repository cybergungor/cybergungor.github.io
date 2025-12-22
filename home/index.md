---
layout: default
title: Home
permalink: /home/
---

<div class="infinite-log-bg">
    <div class="log-col"><div class="moving-stream s1">
        <span>[INFO] Connection established: 192.168.1.45</span>
        <span>[WARN] Unauthorized access PORT 22</span>
        <span>[DEBUG] SIEM Correlation: Rule_08 fired</span>
        <span>[ALERT] Brute Force detected: Endpoint_01</span>
        <span>SRC: 10.0.0.12 -> DST: 10.0.0.254 [TCP/443]</span>
        <span>[INFO] Connection established: 192.168.1.45</span>
    </div></div>
    <div class="log-col hide-mobile"><div class="moving-stream s2">
        <span>INDICATOR: Malicious HASH detected (MD5)</span>
        <span>RULE_09: Brute Force Attempt [PORT_22]</span>
        <span>[INFO] System Kernel updated (v6.1.0)</span>
        <span>ISP: TurkNet Communication Services</span>
        <span>INDICATOR: Malicious HASH detected (MD5)</span>
        <span>RULE_09: Brute Force Attempt [PORT_22]</span>
    </div></div>
    <div class="log-col"><div class="moving-stream s3">
        <span>LOC: 41.0082° N, 28.9784° E (Istanbul)</span>
        <span>NETWORK_STATUS: SECURE | THREAT: LOW</span>
        <span>ASN: AS12735 | TZ: Europe/Istanbul</span>
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
            <label>SESSION_ID</label>
            <span class="val" id="session-id">LOADING...</span>
        </div>
    </div>

    <div class="bento-container">
        <a href="/blog/" class="bento-card">
            <div class="card-inner">
                <div class="card-head"><span class="tag">01 / DATABASE</span> 📝</div>
                <h2>Blog / Write-ups</h2>
                <p>Technical investigations on Blue Team operations and SOC workflows.</p>
                <div class="footer-link">EXPLORE_DATA &rarr;</div>
            </div>
        </a>

        <a href="/labs/" class="bento-card">
            <div class="card-inner">
                <div class="card-head"><span class="tag">02 / RANGE</span> 🛡️</div>
                <h2>Labs / Simulations</h2>
                <p>Interactive security scenarios and hands-on IR labs.</p>
                <div class="footer-link">INIT_LAB &rarr;</div>
            </div>
        </a>

        <a href="/about/" class="bento-card about-wide">
            <div class="card-inner">
                <div class="card-head"><span class="tag">03 / BIO</span> 👤</div>
                <div class="about-flex">
                    <div class="about-text">
                        <h2>About / Operator</h2>
                        <p>Documenting a cybersecurity journey focused on defense and threat hunting.</p>
                    </div>
                    <div class="footer-link">READ_FULL_BIO &rarr;</div>
                </div>
            </div>
        </a>
    </div>
</div>

<style>
/* --- CORE STYLES --- */
:root {
    --bg: #08090a;
    --card-bg: rgba(17, 18, 20, 0.9);
    --accent: #00f2ff;
    --border: rgba(255, 255, 255, 0.06);
}

body { background-color: var(--bg); color: #c9d1d9; font-family: 'Inter', sans-serif; margin: 0; }

/* --- LOG BACKGROUND --- */
.infinite-log-bg {
    position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
    display: flex; justify-content: space-around; opacity: 0.07;
    z-index: -1; pointer-events: none; font-family: 'JetBrains Mono', monospace; font-size: 10px;
}
.moving-stream { display: flex; flex-direction: column; animation: scrollDown infinite linear; }
.moving-stream span { padding: 12px 0; color: var(--accent); }
.s1 { animation-duration: 35s; } .s2 { animation-duration: 50s; } .s3 { animation-duration: 40s; }
@keyframes scrollDown { 0% { transform: translateY(-50%); } 100% { transform: translateY(0); } }

/* --- DASHBOARD WRAPPER --- */
.dashboard-wrapper { max-width: 950px; margin: 0 auto; padding: 2.5rem 1.5rem; position: relative; }

.main-title { 
    font-size: 2.8rem; font-weight: 800; letter-spacing: -1.5px; color: #fff; margin: 5px 0; 
    text-shadow: 0 0 15px rgba(0, 242, 255, 0.1);
}

.command-line { display: flex; gap: 8px; font-family: 'JetBrains Mono'; color: var(--accent); font-size: 0.95rem; margin-bottom: 1.5rem; }
.typewriter-text { border-right: 2px solid var(--accent); animation: blink 0.8s infinite; }

/* --- METRICS BAR (COMPACT) --- */
.metrics-bar { 
    display: flex; justify-content: space-between; align-items: center;
    background: var(--card-bg); border: 1px solid var(--border);
    padding: 12px 25px; border-radius: 10px; margin-bottom: 2rem;
    backdrop-filter: blur(8px);
}
.metric-item label { display: block; font-size: 8px; color: #8b949e; letter-spacing: 1px; text-transform: uppercase; }
.metric-item .val { font-family: 'JetBrains Mono'; font-weight: bold; font-size: 12px; }
.success { color: #3fb950; }

/* --- BENTO GRID (2x1 Layout) --- */
.bento-container { 
    display: grid; 
    grid-template-columns: repeat(2, 1fr); 
    gap: 16px; 
}

.bento-card { 
    background: var(--card-bg); border: 1px solid var(--border);
    border-radius: 16px; padding: 25px; text-decoration: none !important;
    transition: 0.3s ease; backdrop-filter: blur(10px);
}

/* About kartını tam genişlik yapar */
.about-wide { 
    grid-column: span 2; 
    min-height: auto; 
}

.bento-card:hover {
    border-color: var(--accent);
    transform: translateY(-4px);
    background: rgba(22, 24, 28, 0.95);
    box-shadow: 0 0 20px rgba(0, 242, 255, 0.05);
}

.card-head { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; }
.tag { font-family: 'JetBrains Mono'; font-size: 9px; color: var(--accent); letter-spacing: 1px; }
.bento-card h2 { color: #fff; font-size: 1.4rem; margin: 0 0 10px 0; }
.bento-card p { color: #8b949e; line-height: 1.5; font-size: 0.85rem; margin: 0; }

.about-flex { display: flex; justify-content: space-between; align-items: flex-end; }
.footer-link { font-size: 11px; font-weight: bold; color: var(--accent); margin-top: 15px; }

@keyframes blink { 50% { border-color: transparent; } }
@media (max-width: 768px) { 
    .bento-container { grid-template-columns: 1fr; } 
    .about-wide { grid-column: span 1; }
    .main-title { font-size: 2.2rem; } 
}
</style>

<script>
    document.getElementById('session-id').innerText = "SID_" + Math.random().toString(36).substring(2, 8).toUpperCase();
</script>
