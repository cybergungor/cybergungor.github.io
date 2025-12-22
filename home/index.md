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
            <span class="metric-label">ACTIVE_LABS:</span>
            <span class="metric-value">12+</span>
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
/* Dashboard General Settings */
.dashboard-wrapper { animation: fadeIn 0.8s ease-out; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

/* Hero & Typewriter */
.dashboard-header { padding: 1.5rem 0; }
.main-title { font-size: clamp(2.5rem, 7vw, 4.5rem); color: #f0f6fc; margin: 0.5rem 0; letter-spacing: -3px; font-weight: 800; }

.typewriter-text {
    font-family: 'JetBrains Mono', monospace;
    color: var(--accent);
    font-size: 1.2rem;
    border-right: 3px solid var(--accent);
    width: fit-content;
    white-space: nowrap;
    overflow: hidden;
    animation: typing 3.5s steps(30, end) infinite, blink 0.8s step-end infinite;
}

@keyframes typing { 0% { width: 0 } 50% { width: 100% } 90% { width: 100% } 100% { width: 0 } }
@keyframes blink { from, to { border-color: transparent } 50% { border-color: var(--accent) } }

/* Status Badge */
.status-badge { 
    display: inline-flex; align-items: center; gap: 8px;
    background: rgba(63, 185, 80, 0.1); color: #3fb950; 
    padding: 6px 14px; border-radius: 20px; font-size: 11px; font-weight: 800; text-transform: uppercase;
}
.pulse-dot { width: 8px; height: 8px; background: #3fb950; border-radius: 50%; box-shadow: 0 0 8px #3fb950; animation: pulse 2s infinite; }
@keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.4; } 100% { opacity: 1; } }

/* Metrics Bar */
.metrics-bar {
    display: flex; gap: 2rem; margin: 1.5rem 0 2.5rem 0;
    padding: 15px 25px; background: rgba(22, 27, 34, 0.5);
    border: 1px solid var(--border-color); border-radius: 12px;
    font-family: 'JetBrains Mono', monospace; font-size: 12px;
}
.metric-label { color: var(--text-muted); }
.metric-value { color: var(--text-bright); font-weight: bold; }
.status-low { color: #3fb950; text-shadow: 0 0 5px rgba(63, 185, 80, 0.5); }

/* Bento Container & Cards */
.bento-container { display: grid; grid-template-columns: 1.7fr 1fr; gap: 15px; }
.bento-card {
    background: #161b22; border: 1px solid var(--border-color);
    border-radius: 24px; padding: 30px; text-decoration: none !important;
    display: flex; flex-direction: column; justify-content: space-between;
    transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    min-height: 200px; position: relative; overflow: hidden;
}
.blog-module { grid-row: span 2; }

/* Card Hover Effects */
.bento-card:hover {
    transform: translateY(-8px);
    border-color: var(--accent);
    box-shadow: 0 10px 30px rgba(88, 166, 255, 0.15);
}
.bento-card::after {
    content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
    background: radial-gradient(circle at top right, rgba(88, 166, 255, 0.05), transparent);
    pointer-events: none;
}

/* Card Content Styles */
.card-icon { font-size: 2rem; margin-bottom: 1rem; }
.tag { font-family: 'JetBrains Mono', monospace; color: var(--accent); font-size: 10px; text-transform: uppercase; letter-spacing: 2px; font-weight: 800; }
.bento-card h2 { color: var(--text-bright); font-size: 1.8rem; margin: 10px 0; border: none; padding: 0; }
.bento-card p { color: var(--text-muted); font-size: 0.95rem; line-height: 1.5; margin: 0; }
.card-footer { margin-top: 20px; color: var(--text-bright); font-size: 13px; font-weight: bold; opacity: 0.6; }

/* Responsive Adjustments */
@media (max-width: 850px) {
    .bento-container { grid-template-columns: 1fr; }
    .blog-module { grid-row: span 1; }
    .metrics-bar { flex-direction: column; gap: 0.5rem; }
}
</style>
