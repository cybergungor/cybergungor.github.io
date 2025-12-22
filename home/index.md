---
layout: default
title: Home
permalink: /home/
---

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
            <span class="metric-label">SYSTEM_STATUS:</span>
            <span class="metric-value">OPERATIONAL</span>
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
                <div class="lang-en"><p>Deep-dive analysis on SOC operations, SIEM correlation rules, and emerging threats.</p></div>
                <div class="lang-tr" style="display: none;"><p>SOC operasyonları, SIEM kuralları ve yeni nesil tehditler üzerine derinlemesine analizler.</p></div>
            </div>
            <div class="card-footer">Explore Repository &rarr;</div>
        </a>

        <a href="/labs/" class="bento-card labs-module">
            <div class="card-content">
                <div class="card-icon">🛡️</div>
                <span class="tag">Simulations</span>
                <h2>Labs</h2>
                <div class="lang-en"><p>Hands-on incident response scenarios and malware analysis labs.</p></div>
                <div class="lang-tr" style="display: none;"><p>Uygulamalı olay müdahale senaryoları ve zararlı yazılım analiz labları.</p></div>
            </div>
            <div class="card-footer">Enter Station &rarr;</div>
        </a>

        <a href="/about/" class="bento-card about-module">
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
/* --- DASHBOARD WRAPPER & ANIMATIONS --- */
.dashboard-wrapper { 
    animation: fadeIn 0.8s ease-out; 
    position: relative;
    z-index: 2;
}

@keyframes fadeIn { 
    from { opacity: 0; transform: translateY(15px); } 
    to { opacity: 1; transform: translateY(0); } 
}

/* --- ADVANCED BACKGROUND EFFECTS --- */
body {
    background-color: #0d1117;
    background-image: 
        linear-gradient(rgba(88, 166, 255, 0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(88, 166, 255, 0.03) 1px, transparent 1px),
        radial-gradient(circle at 0% 0%, rgba(88, 166, 255, 0.07) 0%, transparent 45%),
        radial-gradient(circle at 100% 100%, rgba(88, 166, 255, 0.07) 0%, transparent 45%);
    background-size: 60px 60px, 60px 60px, 100% 100%, 100% 100%;
    background-attachment: fixed;
    animation: backgroundScroll 80s linear infinite;
}

@keyframes backgroundScroll {
    from { background-position: 0 0, 0 0, 0 0, 0 0; }
    to { background-position: 0 1000px, 0 0, 0 0, 0 0; }
}

/* Scanning Radar Line */
body::before {
    content: ""; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
    background: linear-gradient(to bottom, transparent, rgba(88, 166, 255, 0.02) 50%, transparent);
    background-size: 100% 200%; animation: scanLine 10s linear infinite;
    pointer-events: none; z-index: 1;
}

@keyframes scanLine { 
    0% { background-position: 0 -100%; } 
    100% { background-position: 0 100%; } 
}

/* --- HERO & TYPEWRITER --- */
.dashboard-header { padding: 2rem 0; }
.main-title { 
    font-size: clamp(2.5rem, 8vw, 4.5rem); 
    color: #f0f6fc; 
    margin: 0.5rem 0; 
    letter-spacing: -3px; 
    font-weight: 800; 
    line-height: 1;
}

.typewriter-text {
    font-family: 'JetBrains Mono', monospace;
    color: var(--accent);
    font-size: 1.2rem;
    border-right: 3px solid var(--accent);
    width: fit-content;
    white-space: nowrap;
    overflow: hidden;
    animation: typing 4s steps(30, end) infinite, blink 0.8s step-end infinite;
}

@keyframes typing { 0% { width: 0 } 50% { width: 100% } 90% { width: 100% } 100% { width: 0 } }
@keyframes blink { from, to { border-color: transparent } 50% { border-color: var(--accent) } }

/* --- STATUS & METRICS --- */
.status-badge { 
    display: inline-flex; align-items: center; gap: 8px;
    background: rgba(63, 185, 80, 0.1); color: #3fb950; 
    padding: 6px 14px; border-radius: 20px; font-size: 11px; font-weight: 800; text-transform: uppercase;
}
.pulse-dot { width: 8px; height: 8px; background: #3fb950; border-radius: 50%; box-shadow: 0 0 8px #3fb950; animation: pulse 2s infinite; }
@keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.4; } 100% { opacity: 1; } }

.metrics-bar {
    display: flex; gap: 2rem; margin-bottom: 3rem;
    padding: 15px 25px; background: rgba(22, 27, 34, 0.6);
    border: 1px solid var(--border-color); border-radius: 16px;
    font-family: 'JetBrains Mono', monospace; font-size: 12px;
    backdrop-filter: blur(10px);
}
.metric-label { color: var(--text-muted); }
.metric-value { color: var(--text-bright); font-weight: bold; }
.status-low { color: #3fb950; text-shadow: 0 0 8px rgba(63, 185, 80, 0.4); }

/* --- BENTO CONTAINER --- */
.bento-container { display: grid; grid-template-columns: 1.8fr 1fr; gap: 20px; }
.bento-card {
    background: rgba(22, 27, 34, 0.7); border: 1px solid var(--border-color);
    border-radius: 28px; padding: 35px; text-decoration: none !important;
    display: flex; flex-direction: column; justify-content: space-between;
    transition: all 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    min-height: 220px; position: relative; overflow: hidden;
    backdrop-filter: blur(5px);
}
.blog-module { grid-row: span 2; }

.bento-card:hover {
    transform: translateY(-10px) scale(1.01);
    border-color: var(--accent);
    box-shadow: 0 15px 40px rgba(88, 166, 255, 0.2);
    background: rgba(28, 33, 40, 0.9);
}

.card-icon { font-size: 2.2rem; margin-bottom: 1.2rem; filter: drop-shadow(0 0 10px rgba(255,255,255,0.1)); }
.tag { font-family: 'JetBrains Mono', monospace; color: var(--accent); font-size: 10px; text-transform: uppercase; letter-spacing: 2px; font-weight: 800; }
.bento-card h2 { color: var(--text-bright); font-size: 2rem; margin: 12px 0; border: none; padding: 0; font-weight: 700; }
.bento-card p { color: var(--text-muted); font-size: 1rem; line-height: 1.6; margin: 0; }
.card-footer { margin-top: 25px; color: var(--text-bright); font-size: 13px; font-weight: bold; opacity: 0.5; transition: 0.3s; }
.bento-card:hover .card-footer { opacity: 1; transform: translateX(5px); }

/* --- RESPONSIVE --- */
@media (max-width: 900px) {
    .bento-container { grid-template-columns: 1fr; }
    .blog-module { grid-row: span 1; }
    .metrics-bar { flex-direction: column; gap: 0.8rem; }
    .main-title { font-size: 3rem; }
}
</style>

<script>
    // Küçük bir sürpriz: Rastgele Session ID üretici
    document.getElementById('session-id').innerText = Math.random().toString(36).substring(2, 10).toUpperCase();
</script>
