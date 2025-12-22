---
layout: default
title: Home
permalink: /home/
---

<div class="log-bg left-logs">
    <div class="log-content">
        12.22.2025 23:01:45 [INFO] Connection established from 192.168.1.45 <br>
        12.22.2025 23:01:46 [WARN] Unauthorized access attempt on PORT 22 <br>
        12.22.2025 23:01:48 [DEBUG] SIEM Correlation Engine: Rule_08 fired <br>
        12.22.2025 23:01:50 [INFO] User Emirhan_G session initialized <br>
        12.22.2025 23:01:52 [INFO] Firewall: Outbound traffic permitted to 8.8.8.8 <br>
        12.22.2025 23:01:55 [ALERT] Potential Brute Force detected on Endpoint_01 <br>
        12.22.2025 23:01:58 [INFO] Heartbeat signal received from SOC_Sensor_A <br>
        12.22.2025 23:01:45 [INFO] Connection established from 192.168.1.45 <br>
        12.22.2025 23:01:46 [WARN] Unauthorized access attempt on PORT 22 <br>
        12.22.2025 23:01:50 [INFO] User Emirhan_G session initialized <br>
    </div>
</div>

<div class="log-bg right-logs">
    <div class="log-content delay">
        SRC_IP: 10.0.0.12 -> DST_IP: 10.0.0.254 [TCP/443] <br>
        STATUS: 200 OK | PAYLOAD_SIZE: 1245 bytes <br>
        THREAD_ID: 0x8F2A | PRIORITY: HIGH <br>
        MITRE_ATTACK: T1059.001 (PowerShell Execution) <br>
        LOG_SOURCE: WinEventLog:Security <br>
        ACTION: ALLOW | POLICY: Default_Drop_Rule <br>
        HASH: 5e884898da28047151d0e56f8dc6292773603d <br>
        SRC_IP: 10.0.0.12 -> DST_IP: 10.0.0.254 [TCP/443] <br>
        STATUS: 200 OK | PAYLOAD_SIZE: 1245 bytes <br>
        LOG_SOURCE: WinEventLog:Security <br>
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
        <div class="metric-item">
            <span class="metric-label">THREAT_LEVEL:</span>
            <span class="metric-value status-low">LOW</span>
        </div>
        <div class="metric-item">
            <span class="metric-label">NETWORK:</span>
            <span class="metric-value">SECURE</span>
        </div>
        <div class="metric-item">
            <span class="metric-label">SESSION:</span>
            <span class="metric-value" id="session-id">LOADING...</span>
        </div>
    </div>

    <div class="bento-container">
        <a href="/blog/" class="bento-card blog-module">
            <div class="card-content">
                <div class="card-icon">📝</div>
                <span class="tag">Knowledge Base</span>
                <h2>Blog</h2>
                <div class="lang-en"><p>In-depth write-ups on SOC operations, SIEM rules, and detection engineering.</p></div>
                <div class="lang-tr" style="display: none;"><p>SOC operasyonları, SIEM kuralları ve tespit mühendisliği üzerine derinlemesine yazılar.</p></div>
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
/* --- LOG STREAM BACKGROUND (NEW) --- */
.log-bg {
    position: fixed;
    top: 0;
    width: 250px;
    height: 100vh;
    font-family: 'JetBrains Mono', monospace;
    font-size: 9px;
    line-height: 2;
    color: rgba(88, 166, 255, 0.08); /* Çok silik mavi - göz yormaz */
    overflow: hidden;
    pointer-events: none;
    z-index: -1; /* En arka plana atar */
}

.left-logs { left: 20px; }
.right-logs { right: 20px; text-align: right; }

.log-content {
    display: flex;
    flex-direction: column;
    animation: scrollLogs 45s linear infinite;
}

.log-content.delay {
    animation-duration: 60s; /* Sağ taraf farklı hızda aksın ki yapay durmasın */
}

@keyframes scrollLogs {
    0% { transform: translateY(0); }
    100% { transform: translateY(-50%); }
}

/* --- DASHBOARD WRAPPER --- */
.dashboard-wrapper { 
    max-width: 1100px;
    margin: 0 auto;
    padding: 0 40px;
    position: relative;
    z-index: 2; /* Logların üzerinde durmasını sağlar */
}

/* --- HERO & TYPEWRITER --- */
.dashboard-header { padding: 3rem 0 2rem 0; }
.main-title { font-size: clamp(2.5rem, 6vw, 4rem); color: #f0f6fc; font-weight: 800; letter-spacing: -2.5px; margin: 0.5rem 0; }

.typewriter-text {
    font-family: 'JetBrains Mono', monospace; color: var(--accent); font-size: 1.1rem;
    border-right: 2px solid var(--accent); width: fit-content; white-space: nowrap; overflow: hidden;
    animation: typing 4s steps(30, end) infinite, blink 0.8s step-end infinite;
}
@keyframes typing { 0%, 10% { width: 0 } 50%, 90% { width: 100% } 100% { width: 0 } }
@keyframes blink { 50% { border-color: transparent } }

/* --- METRICS BAR --- */
.metrics-bar {
    display: flex; justify-content: space-between; gap: 1rem; margin-bottom: 3rem;
    padding: 12px 25px; background: rgba(22, 27, 34, 0.6);
    border: 1px solid var(--border-color); border-radius: 16px;
    font-family: 'JetBrains Mono', monospace; font-size: 11px;
    backdrop-filter: blur(10px);
}
.metric-value { color: var(--text-bright); font-weight: bold; }
.status-low { color: #3fb950; text-shadow: 0 0 5px rgba(63, 185, 80, 0.5); }

/* --- BENTO GRID --- */
.bento-container { display: grid; grid-template-columns: 1.8fr 1fr; gap: 24px; }
.bento-card {
    background: rgba(22, 27, 34, 0.8);
    backdrop-filter: blur(8px);
    border: 1px solid #30363d;
    border-radius: 28px;
    padding: 32px;
    transition: 0.4s ease;
    text-decoration: none !important;
    display: flex; flex-direction: column; justify-content: space-between;
    min-height: 220px;
}
.blog-module { grid-row: span 2; }

.bento-card:hover {
    border-color: var(--accent);
    box-shadow: 0 10px 40px rgba(0,0,0,0.4), 0 0 20px rgba(88, 166, 255, 0.1);
    transform: translateY(-5px);
}

.card-icon { font-size: 2rem; margin-bottom: 1rem; }
.tag { font-family: 'JetBrains Mono', monospace; color: var(--accent); font-size: 10px; text-transform: uppercase; letter-spacing: 2px; font-weight: 800; }
.bento-card h2 { color: #f0f6fc; font-size: 1.8rem; margin: 10px 0; border: none; padding: 0; }
.bento-card p { color: var(--text-muted); font-size: 0.95rem; line-height: 1.5; margin: 0; }
.card-footer { font-size: 12px; font-weight: bold; color: var(--text-bright); opacity: 0.4; margin-top: 20px; }
.bento-card:hover .card-footer { opacity: 1; }

@media (max-width: 850px) {
    .bento-container { grid-template-columns: 1fr; }
    .blog-module { grid-row: span 1; }
    .log-bg { display: none; } /* Mobilde karmaşayı önlemek için logları gizliyoruz */
}
</style>

<script>
    document.getElementById('session-id').innerText = Math.random().toString(36).substring(2, 10).toUpperCase();
</script>
