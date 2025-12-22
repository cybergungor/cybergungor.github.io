---
layout: default
title: Home
permalink: /home/
---

<div class="infinite-log-bg">
    <div class="log-col"><div class="loop-stream s1">
        <span>0xDE 0xAD 0xBE 0xEF // SYSTEM_INIT_SUCCESS</span>
        <span>TRACERT -> 192.168.100.22 [OK]</span>
        <span>MITRE_T1059: Powershell_Detected</span>
        <span>SIEM_ALARM: High_Entropy_detected</span>
        <span>0xDE 0xAD 0xBE 0xEF // SYSTEM_INIT_SUCCESS</span>
        <span>TRACERT -> 192.168.100.22 [OK]</span>
    </div></div>
    <div class="log-col hide-mobile"><div class="loop-stream s2">
        <span>LAT: 41.008 // LON: 28.978 // ISTANBUL_NODE</span>
        <span>EDR_VERSION: 2.5.4-STABLE</span>
        <span>MEM_DUMP: Process_08F2 complete</span>
        <span>LAT: 41.008 // LON: 28.978 // ISTANBUL_NODE</span>
    </div></div>
</div>

<div class="dashboard-wrapper">
    <div class="dashboard-header">
        <div class="lang-en">
            <span class="status-badge">Available for Projects</span>
            <h1 class="main-title">EMIRHAN GUNGOROGLU</h1>
            <div class="command-line">
                <span class="prompt">guest@cyberlab:~$</span>
                <span class="typewriter-text">whoami --roles --status</span>
            </div>
        </div>
        <div class="lang-tr" style="display: none;">
            <span class="status-badge">Projelere Açık</span>
            <h1 class="main-title">EMİRHAN GÜNGÖROĞLU</h1>
            <div class="command-line">
                <span class="prompt">misafir@siberlab:~$</span>
                <span class="typewriter-text">kimim_ben --roller --durum</span>
            </div>
        </div>
    </div>

    <div class="metrics-row">
        <div class="m-card">
            <label>THREAT LEVEL</label>
            <div class="val low-glow">LOW_DETECTION</div>
        </div>
        <div class="m-card">
            <label>DEFENSE_STATUS</label>
            <div class="val">ACTIVE_SHIELD</div>
        </div>
        <div class="m-card">
            <label>SESSION_ID</label>
            <div class="val" id="session-id">LOADING...</div>
        </div>
    </div>

    <div class="bento-grid">
        <a href="/blog/" class="bento-card blog-mod">
            <div class="card-inner">
                <span class="index">01</span>
                <div class="icon">📁</div>
                <h2>Blog / Write-ups</h2>
                <p>Technical investigations and SOC analysis reports.</p>
                <div class="btn-text">ACCESS_DATA &rarr;</div>
            </div>
        </a>

        <a href="/labs/" class="bento-card lab-mod">
            <div class="card-inner">
                <span class="index">02</span>
                <div class="icon">🔬</div>
                <h2>Labs / Range</h2>
                <p>Live simulations and malware analysis results.</p>
                <div class="btn-text">INIT_SIM &rarr;</div>
            </div>
        </a>

        <a href="/about/" class="bento-card about-mod">
            <div class="card-inner">
                <span class="index">03</span>
                <div class="icon">👤</div>
                <h2>About / Operator</h2>
                <p>Background and mission of Gungoroglu.</p>
                <div class="btn-text">READ_BIO &rarr;</div>
            </div>
        </a>
    </div>
</div>

<style>
/* --- PREMIUM THEME COLORS --- */
:root {
    --bg-main: #0a0b10; /* Derin siyah-lacivert */
    --accent: #ccff00; /* Cyber Lime (Fark yaratan renk) */
    --accent-dim: rgba(204, 255, 0, 0.2);
    --text-primary: #ffffff;
    --text-dim: #8b949e;
}

body {
    background-color: var(--bg-main);
    /* Noise texture ekliyoruz */
    background-image: url('https://grainy-gradients.vercel.app/noise.svg');
    background-attachment: fixed;
    color: var(--text-primary);
}

/* --- LOGS (Çok daha silik ve estetik) --- */
.infinite-log-bg {
    position: fixed; top: 0; left: 0; width: 100%; height: 100%;
    display: flex; justify-content: space-between; opacity: 0.04;
    z-index: -1; font-family: 'JetBrains Mono', monospace; font-size: 11px;
}
.loop-stream { display: flex; flex-direction: column; animation: scrollDown 40s linear infinite; }
@keyframes scrollDown { 0% { transform: translateY(-50%); } 100% { transform: translateY(0); } }

/* --- HEADER --- */
.dashboard-wrapper { max-width: 1100px; margin: 0 auto; padding: 4rem 2rem; }
.main-title { 
    font-family: 'Space Grotesk', sans-serif; /* Fütüristik font */
    font-size: 4.5rem; font-weight: 800; letter-spacing: -4px; 
    text-transform: uppercase; margin: 10px 0;
}
.command-line { display: flex; gap: 10px; font-family: 'JetBrains Mono'; font-size: 1.1rem; color: var(--accent); }

/* --- METRICS --- */
.metrics-row { display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin: 3rem 0; }
.m-card { 
    background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.05); 
    padding: 15px; border-radius: 8px; text-align: left;
}
.m-card label { font-size: 9px; color: var(--text-dim); letter-spacing: 2px; }
.m-card .val { font-family: 'JetBrains Mono'; font-size: 13px; color: var(--text-primary); font-weight: bold; }
.low-glow { color: var(--accent) !important; text-shadow: 0 0 10px var(--accent-dim); }

/* --- BENTO CARDS --- */
.bento-grid { display: grid; grid-template-columns: 1.4fr 1fr; gap: 20px; }
.bento-card { 
    background: rgba(255,255,255,0.02); 
    border: 1px solid rgba(255,255,255,0.05);
    border-radius: 20px; padding: 40px; text-decoration: none !important;
    position: relative; overflow: hidden; transition: 0.4s cubic-bezier(0.23, 1, 0.32, 1);
    backdrop-filter: blur(10px);
}
.blog-mod { grid-row: span 2; }

.bento-card:hover {
    background: rgba(255,255,255,0.05);
    border-color: var(--accent);
    transform: scale(1.02);
}

.index { position: absolute; top: 30px; right: 30px; font-size: 4rem; opacity: 0.03; font-weight: 900; }
.icon { font-size: 2rem; margin-bottom: 20px; }
.bento-card h2 { font-family: 'Space Grotesk'; font-size: 2rem; margin-bottom: 10px; color: #fff; }
.btn-text { margin-top: 30px; font-size: 11px; font-weight: bold; color: var(--accent); letter-spacing: 2px; }

@media (max-width: 850px) { .bento-grid { grid-template-columns: 1fr; } .main-title { font-size: 2.5rem; } }
</style>

<script>
    document.getElementById('session-id').innerText = Math.random().toString(36).substring(2, 12).toUpperCase();
</script>
