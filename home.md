---
layout: default
title: Home
permalink: /home/
---

<div class="fresh-hero">
    <div class="lang-en">
        <span class="status-badge">Available for Projects</span>
        <h1 class="main-title">Emirhan Gungoroglu</h1>
        <p class="main-subtitle">Cybersecurity Student & Blue Team Enthusiast. <br>Documenting the journey of defending digital infrastructures.</p>
    </div>
    
    <div class="lang-tr" style="display: none;">
        <span class="status-badge">Projelere Açık</span>
        <h1 class="main-title">Emirhan Güngöroğlu</h1>
        <p class="main-subtitle">Siber Güvenlik Öğrencisi ve Blue Team Meraklısı. <br>Dijital altyapıları savunma yolculuğumu belgeliyorum.</p>
    </div>
</div>

<div class="bento-grid">
    <a href="/blog/" class="bento-item blog-card">
        <div class="bento-content">
            <div class="lang-en">
                <span class="category">Articles</span>
                <h2>Blog</h2>
                <p>Technical write-ups and security research.</p>
            </div>
            <div class="lang-tr" style="display: none;">
                <span class="category">Makaleler</span>
                <h2>Blog</h2>
                <p>Teknik yazılar ve güvenlik araştırmaları.</p>
            </div>
        </div>
        <div class="bento-footer">Explore &rarr;</div>
    </a>

    <a href="/labs/" class="bento-item">
        <div class="lang-en"><h2>Labs</h2><p>Simulation walkthroughs.</p></div>
        <div class="lang-tr" style="display: none;"><h2>Lablar</h2><p>Simülasyon çözümleri.</p></div>
        <div class="bento-footer">Enter &rarr;</div>
    </a>

    <a href="/about/" class="bento-item">
        <div class="lang-en"><h2>About</h2><p>The person behind the screen.</p></div>
        <div class="lang-tr" style="display: none;"><h2>Hakkımda</h2><p>Ekranın arkasındaki kişi.</p></div>
        <div class="bento-footer">Meet &rarr;</div>
    </a>
</div>

<style>
    .fresh-hero { padding: 2rem 0; }
    .status-badge { background: rgba(63, 185, 80, 0.1); color: var(--success); border: 1px solid rgba(63, 185, 80, 0.3); padding: 5px 12px; border-radius: 20px; font-size: 11px; font-weight: bold; }
    .main-title { font-size: clamp(2.5rem, 6vw, 4rem); font-weight: 800; color: var(--text-bright); margin: 1rem 0; letter-spacing: -2px; line-height: 1.1; }
    .main-subtitle { font-size: 1.2rem; color: var(--text-muted); max-width: 600px; }

    .bento-grid { display: grid; grid-template-columns: 1.5fr 1fr; gap: 20px; margin-top: 3rem; }
    .bento-item { background: var(--card-bg); border: 1px solid var(--border-color); border-radius: 24px; padding: 30px; display: flex; flex-direction: column; justify-content: space-between; transition: 0.4s; min-height: 180px; }
    .bento-item:hover { border-color: var(--accent); transform: translateY(-5px); background: #1c2128; }
    .blog-card { grid-row: span 2; }
    .category { font-size: 10px; text-transform: uppercase; color: var(--accent); letter-spacing: 2px; font-weight: bold; }
    .bento-item h2 { margin: 10px 0; font-size: 1.8rem; border: none; padding: 0; }
    .bento-item p { font-size: 0.95rem; color: var(--text-muted); margin: 0; }
    .bento-footer { font-size: 13px; font-weight: bold; margin-top: 20px; color: var(--accent); }

    @media (max-width: 768px) { .bento-grid { grid-template-columns: 1fr; } .blog-card { grid-row: span 1; } }
</style>
