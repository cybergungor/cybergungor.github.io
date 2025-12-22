---
layout: default
title: About Me
permalink: /about/
---

<div class="post-static-bg">
    <div class="log-column"><div class="flow-stream s1">
        <span>[IDENTITY] SCANNING_OPERATOR_PROFILE...</span>
        <span>[INFO] ROLE: CYBER_SECURITY_STUDENT</span>
        <span>[STATUS] VERIFYING_CERTIFICATIONS...</span>
        <span>[INFO] PICUS_CERT: THREAT_HUNTING_FOUNDATIONS</span>
    </div></div>
    <div class="log-col hide-mobile"><div class="moving-stream s2">
        <span>0x45 0x6D 0x69 0x72 0x68 0x61 0x6E 0x5F 0x47</span>
        <span>SYSTEM_ACCESS: GRANTED // CLEARANCE: LEVEL_4</span>
    </div></div>
</div>

<div class="about-main-wrapper">
    <header class="about-static-header-clean">
        <div class="about-tags-simple">
            <span class="cyan-accent">OPERATOR_IDENTITY</span> // STATUS: ACTIVE
        </div>
        <h1 class="about-pure-white-title">About / Emirhan Gungoroglu</h1>
        <div class="about-path-simple">
            <span class="green-prompt">guest@cyberlab:~$</span> whoami --roles --status
        </div>
    </header>

    <div class="about-flex-container">
        <div class="profile-section">
            <div class="profile-card-tactical">
                <div class="image-wrapper">
                    <img src="/assets/images/profile.jpeg" alt="Emirhan Gungoroglu" class="profile-img">
                    <div class="scan-line"></div>
                </div>
                <div class="profile-stats">
                    <div class="stat-row">
                        <span class="stat-label">OPERATOR:</span>
                        <span class="stat-value">Emirhan G.</span>
                    </div>
                    <div class="stat-row">
                        <span class="stat-label">ROLE:</span>
                        <span class="stat-value" style="color: var(--cyber-cyan)">SOC Analyst</span>
                    </div>
                    <div class="stat-row">
                        <span class="stat-label">STATUS:</span>
                        <span class="stat-value" style="color: #3fb950">ACTIVE</span>
                    </div>
                </div>
            </div>
        </div>

        <div class="bio-section">
            <div class="lang-en">
                <h2 class="section-heading">whoami</h2>
                <p class="intro-text">
                    Hello, I'm <strong>Emirhan Gungoroglu</strong>. I am a cybersecurity student passionate about 
                    <strong>Blue Team</strong> operations. My journey is fueled by a curiosity to understand 
                    how systems work, how they are attacked, and most importantly, <span style="color: var(--cyber-cyan);">how to defend them.</span>
                </p>
                <p>
                    I focus on bridging the gap between theoretical knowledge and real-world application. 
                    Currently, I am sharpening my skills through <strong>LetsDefend</strong>, investigating 
                    real-world simulated cyber attacks and practicing <strong>Incident Response</strong> 
                    in a realistic SOC environment.
                </p>

                <div class="tactical-hr"></div>

                <h3>// Technical Proficiency</h3>
                <div class="badge-container">
                    <span class="tech-badge">SIEM (Splunk / ELK)</span>
                    <span class="tech-badge">Network Traffic Analysis</span>
                    <span class="tech-badge">Log Correlation</span>
                    <span class="tech-badge">MITRE ATT&CK Framework</span>
                    <span class="tech-badge">Endpoint Detection (EDR)</span>
                    <span class="tech-badge">Vulnerability Assessment</span>
                </div>

                <br>
                <h3>// Professional Focus Areas</h3>
                <div class="focus-grid">
                    <div class="focus-item">
                        <h4>🛡️ SOC Operations</h4>
                        <p>Monitoring and triaging security alerts using enterprise-grade SIEM solutions.</p>
                    </div>
                    <div class="focus-item">
                        <h4>🔍 Threat Analysis</h4>
                        <p>Identifying IoCs and mapping attacker behaviors to the MITRE framework.</p>
                    </div>
                    <div class="focus-item">
                        <h4>🚨 Incident Response</h4>
                        <p>Investigating true positives and executing containment steps via SOC playbooks.</p>
                    </div>
                </div>
            </div>

            <br>
            <h3>// Connect</h3>
            <p><a href="https://linkedin.com/in/emirhangungoroglu" class="linkedin-btn">Connect on LinkedIn &rarr;</a></p>
        </div>
    </div>
</div>

<style>
/* --- CORE INTEGRATION --- */
:root {
    --cyber-bg: #08090a;
    --cyber-cyan: #00f2ff;
    --border-soft: rgba(255, 255, 255, 0.08);
}

body { background-color: var(--cyber-bg) !important; color: #c9d1d9 !important; margin: 0; }

/* NAVBAR ÇAKIŞMASINI ÖNLEYEN ANA KONTEYNER */
.about-main-wrapper {
    max-width: 1000px;
    margin: 0 auto !important;
    padding: 150px 20px 60px 20px !important; /* Devasa üst boşluk ile çakışma biter */
    position: relative;
    z-index: 5;
}

/* BAŞLIK: KUTUSUZ, ŞEFFAF VE STATİK (HATA BURADAYDI) */
.about-static-header-clean {
    margin-bottom: 3rem;
    border-bottom: 1px solid var(--border-soft);
    padding-bottom: 1.5rem;
    position: static !important; /* Kaymayı durdurur */
    background: transparent !important; /* Kutuyu yok eder */
    border-top: none !important;
    border-left: none !important;
    border-right: none !important;
}

.about-pure-white-title {
    color: #ffffff !important; /* Saf Beyaz */
    font-size: clamp(2rem, 5vw, 3rem) !important;
    font-weight: 800 !important;
    margin: 10px 0 !important;
    letter-spacing: -1.5px;
}

.about-tags-simple { font-family: 'JetBrains Mono'; font-size: 11px; color: #8b949e; }
.cyan-accent { color: var(--cyber-cyan); font-weight: bold; }
.about-path-simple { font-family: 'JetBrains Mono'; font-size: 0.9rem; color: #8b949e; opacity: 0.8; }
.green-prompt { color: #3fb950; }

/* DİĞER STİLLER (DOKUNULMADI) */
.about-flex-container { display: flex; gap: 3rem; align-items: flex-start; }
.profile-section { flex: 0 0 280px; }
.bio-section { flex: 1; }
.profile-card-tactical { background: rgba(17, 18, 20, 0.9); border: 1px solid var(--border-soft); padding: 10px; border-radius: 12px; backdrop-filter: blur(10px); }
.image-wrapper { position: relative; overflow: hidden; border-radius: 8px; border: 1px solid var(--cyber-cyan); }
.profile-img { width: 100%; height: auto; display: block; filter: grayscale(100%); transition: 0.5s; }
.profile-card-tactical:hover .profile-img { filter: grayscale(0%); transform: scale(1.05); }
.scan-line { position: absolute; top: 0; left: 0; width: 100%; height: 2px; background: var(--cyber-cyan); box-shadow: 0 0 10px var(--cyber-cyan); animation: scan 3s linear infinite; opacity: 0.5; }
@keyframes scan { 0% { top: 0%; } 100% { top: 100%; } }
.profile-stats { margin-top: 1rem; font-family: 'JetBrains Mono'; font-size: 0.85rem; }
.stat-row { display: flex; justify-content: space-between; border-bottom: 1px dashed var(--border-soft); padding: 8px 0; }
.intro-text { font-size: 1.1rem; line-height: 1.8; color: #fff; }
.tactical-hr { border-top: 1px solid var(--border-soft); margin: 2rem 0; }
.badge-container { display: flex; flex-wrap: wrap; gap: 8px; }
.tech-badge { background: rgba(0, 242, 255, 0.05); color: var(--cyber-cyan); border: 1px solid rgba(0, 242, 255, 0.2); padding: 5px 12px; border-radius: 4px; font-size: 0.8rem; font-family: 'JetBrains Mono'; }
.focus-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 1rem; margin-top: 1rem; }
.focus-item { background: rgba(255, 255, 255, 0.02); border: 1px solid var(--border-soft); padding: 1.5rem; border-radius: 8px; }
.focus-item h4 { color: var(--cyber-cyan); margin: 0 0 10px 0; }
.linkedin-btn { color: var(--cyber-cyan); text-decoration: none; font-weight: bold; }

/* BACKGROUND LOGS */
.post-static-bg { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; display: flex; justify-content: space-around; opacity: 0.04; z-index: -1; pointer-events: none; }
.flow-stream { display: flex; flex-direction: column; animation: scrollLog 50s linear infinite; }
.flow-stream span { padding: 15px 0; font-family: 'JetBrains Mono'; font-size: 10px; color: var(--cyber-cyan); }
@keyframes scrollLog { 0% { transform: translateY(-50%); } 100% { transform: translateY(0); } }

@media (max-width: 850px) {
    .about-flex-container { flex-direction: column; }
    .profile-section { width: 100%; max-width: 320px; margin: 0 auto; }
    .about-main-wrapper { padding-top: 110px !important; }
}
</style>
