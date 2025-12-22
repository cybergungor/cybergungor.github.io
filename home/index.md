---
layout: default
title: Home
permalink: /home/
---

<div class="fresh-dashboard">
    <div class="dashboard-header">
        <div class="lang-en">
            <span class="status-badge">Available for Projects</span>
            <h1 class="main-title">Emirhan Gungoroglu</h1>
            <p class="main-subtitle">Cybersecurity Student & Blue Team Enthusiast. <br>Welcome to my professional knowledge base.</p>
        </div>
        
        <div class="lang-tr" style="display: none;">
            <span class="status-badge">Projelere Açık</span>
            <h1 class="main-title">Emirhan Güngöroğlu</h1>
            <p class="main-subtitle">Siber Güvenlik Öğrencisi ve Blue Team Meraklısı. <br>Profesyonel bilgi bankama hoş geldiniz.</p>
        </div>
    </div>

    <div class="bento-container">
        <a href="/blog/" class="bento-card blog-module">
            <div class="card-body">
                <span class="tag">Knowledge Base</span>
                <h2>Blog</h2>
                <div class="lang-en"><p>In-depth write-ups on SOC operations, SIEM rules, and detection logic.</p></div>
                <div class="lang-tr" style="display: none;"><p>SOC operasyonları, SIEM kuralları ve tespit mantığı üzerine derinlemesine yazılar.</p></div>
            </div>
            <div class="card-footer">Explore Articles &rarr;</div>
        </a>

        <a href="/labs/" class="bento-card labs-module">
            <div class="card-body">
                <span class="tag">Practical</span>
                <h2>Labs</h2>
                <div class="lang-en"><p>Interactive security simulations and walkthroughs.</p></div>
                <div class="lang-tr" style="display: none;"><p>Etkileşimli güvenlik simülasyonları ve lab çözümleri.</p></div>
            </div>
            <div class="card-footer">Launch Station &rarr;</div>
        </a>

        <a href="/about/" class="bento-card about-module">
            <div class="card-body">
                <span class="tag">Identity</span>
                <h2>About</h2>
                <div class="lang-en"><p>Background, focus areas, and personal mission.</p></div>
                <div class="lang-tr" style="display: none;"><p>Deneyimler, odak noktaları ve kişisel vizyon.</p></div>
            </div>
            <div class="card-footer">Meet Operator &rarr;</div>
        </a>
    </div>
</div>

<style>
/* CSS Değişkenleri (Landing Page ile Uyumlu) */
:root {
    --accent-blue: #58a6ff;
    --card-bg: #161b22;
    --text-dim: #8b949e;
}

/* Header Tasarımı */
.dashboard-header { padding: 2rem 0; text-align: left; }
.status-badge { 
    background: rgba(63, 185, 80, 0.1); color: #3fb950; 
    border: 1px solid rgba(63, 185, 80, 0.3); 
    padding: 6px 14px; border-radius: 20px; font-size: 11px; font-weight: bold; 
}
.main-title { font-size: 3.5rem; color: #f0f6fc; margin: 1.5rem 0 0.5rem 0; letter-spacing: -2px; line-height: 1.1; }
.main-subtitle { font-size: 1.15rem; color: var(--text-dim); line-height: 1.6; }

/* Bento Grid Yerleşimi */
.bento-container {
    display: grid;
    grid-template-columns: 1.6fr 1fr;
    gap: 20px;
    margin-top: 2rem;
}

/* Kart Tasarımı (Landing Page Glow Efektiyle Uyumlu) */
.bento-card {
    background: var(--card-bg);
    border: 1px solid #30363d;
    border-radius: 24px;
    padding: 30px;
    text-decoration: none !important;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    min-height: 220px;
}

.blog-module { grid-row: span 2; }

.bento-card:hover {
    transform: translateY(-8px) scale(1.01);
    border-color: var(--accent-blue);
    box-shadow: 0 0 25px rgba(88, 166, 255, 0.15);
    background: #1c2128;
}

.tag { color: var(--accent-blue); font-size: 10px; text-transform: uppercase; letter-spacing: 2px; font-weight: 800; }
.bento-card h2 { color: #f0f6fc; font-size: 1.8rem; margin: 15px 0 10px 0; border: none; padding: 0; }
.bento-card p { color: var(--text-dim); font-size: 0.95rem; line-height: 1.5; margin: 0; }
.card-footer { margin-top: 20px; color: #f0f6fc; font-size: 13px; font-weight: bold; opacity: 0.7; }

/* Mobil Uyumluluk */
@media (max-width: 850px) {
    .bento-container { grid-template-columns: 1fr; }
    .blog-module { grid-row: span 1; }
    .main-title { font-size: 2.8rem; }
}
</style>
