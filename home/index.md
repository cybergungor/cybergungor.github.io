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
            <span class="metric-label">STATUS:</span>
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
/* --- DASHBOARD WRAPPER (CENTERED & CONSTRAINED) --- */
.dashboard-wrapper { 
    max-width: 1000px; /* Çok genişlemeyi önler */
    margin: 0 auto; /* Sayfayı ortalar */
    padding: 0 20px;
    animation: fadeIn 0.8s ease-out;
}

/* --- ADVANCED BACKGROUND --- */
body {
    background-color: #0d1117;
    background-image: 
        linear-gradient(rgba(88, 166, 255, 0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(88, 166, 255, 0.03) 1px, transparent 1px),
        radial-gradient(circle at 0% 0%, rgba(88, 166, 255, 0.06) 0%, transparent 40%),
        radial-gradient(circle at 100% 100%, rgba(88, 166, 255, 0.06) 0%, transparent 40%);
    background-size: 50px 50px, 50px 50px, 100% 100%, 100% 100%;
    animation: backgroundScroll 100s linear infinite;
}

@keyframes backgroundScroll { from { background-position: 0 0; } to { background-position: 0 1000px; } }

/* --- HERO SECTION --- */
.dashboard-header { padding: 3rem 0 2rem 0; }
.main-title { font-size: clamp(2.5rem, 5vw, 3.8rem); color: #f0f6fc; font-weight: 800; letter-spacing: -2px; line-height: 1; margin: 1rem 0; }

.typewriter-text {
    font-family: 'JetBrains Mono', monospace; color: var(--accent); font-size: 1.1rem;
    border-right: 2px solid var(--accent); width: fit-content; white-space: nowrap; overflow: hidden;
    animation: typing 4s steps(30, end) infinite, blink 0.8s step-end infinite;
}
@keyframes typing { 0%, 10% { width: 0 } 50%, 90% { width: 100% } 100% { width: 0 } }
@keyframes blink { 50% { border-color: transparent } }

/* --- METRICS BAR --- */
.metrics-bar {
    display: flex; justify-content: space-between; gap: 1rem; margin-bottom: 2.5rem;
    padding: 12px 20px; background: rgba(22, 27, 34, 0.7);
    border: 1px solid var(--border-color); border-radius: 12px;
    font-family: 'JetBrains Mono', monospace; font-size: 11px;
}
.metric-value { color: var(--text-bright); font-weight: bold; }
.status-low { color: #3fb950; text-shadow: 0 0 5px rgba(63, 185, 80, 0.5); }

/* --- BENTO CONTAINER (SYMMETRIC) --- */
.bento-container { 
    display: grid; 
    grid-template-columns: repeat(6, 1fr); /* 6 sütunlu hassas yapı */
    gap: 24px; /* Kartlar arası boşluk artırıldı */
}

.bento-card {
    background: #161b22; border: 1px solid var(--border-color);
    border-radius: 20px; padding: 28px; text-decoration: none !important;
    display: flex; flex-direction: column; justify-content: space-between;
    transition: all 0.4s ease; min-height: 240px;
}

/* Kart Genişlik Ayarları */
.blog-module { grid-column: span 3; grid-row: span 2; } /* Sol büyük kart */
.labs-module { grid-column: span 3; } /* Sağ üst */
.about-module { grid-column: span 3; } /* Sağ alt */

.bento-card:hover {
    transform: translateY(-6px); border-color: var(--accent);
    box-shadow: 0 12px 30px rgba(0,0,0,0.4), 0 0 15px rgba(88,166,255,0.1);
}

.card-icon { font-size: 1.8rem; margin-bottom: 0.8rem; }
.tag { font-family: 'JetBrains Mono', monospace; color: var(--accent); font-size: 10px; text-transform: uppercase; letter-spacing: 1.5px; font-weight: bold; }
.bento-card h2 { color: #fff; font-size: 1.6rem; margin: 10px 0; border: none; padding: 0; }
.bento-card p { color: var(--text-muted); font-size: 0.9rem; line-height: 1.5; }
.card-footer { font-size: 12px; font-weight: bold; color: var(--text-bright); opacity: 0.4; margin-top: 15px; }
.bento-card:hover .card-footer { opacity: 1; }

/* Responsive */
@media (max-width: 850px) {
    .bento-container { grid-template-columns: 1fr; }
    .blog-module, .labs-module, .about-module { grid-column: span 1; }
    .metrics-bar { flex-direction: column; }
}
</style>

<script>
    document.getElementById('session-id').innerText = Math.random().toString(36).substring(2, 10).toUpperCase();
</script>
