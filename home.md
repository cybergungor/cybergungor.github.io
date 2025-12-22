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
                <p>In-depth technical write-ups and security concepts.</p>
            </div>
            <div class="lang-tr" style="display: none;">
                <span class="category">Makaleler</span>
                <h2>Blog</h2>
                <p>Derinlemesine teknik yazılar ve güvenlik konseptleri.</p>
            </div>
        </div>
        <div class="bento-footer">
            <span class="lang-en">Explore Posts &rarr;</span>
            <span class="lang-tr" style="display: none;">Yazıları İncele &rarr;</span>
        </div>
    </a>

    <a href="/labs/" class="bento-item labs-card">
        <div class="bento-content">
            <div class="lang-en">
                <span class="category">Practical</span>
                <h2>Labs</h2>
                <p>Hands-on simulation walkthroughs.</p>
            </div>
            <div class="lang-tr" style="display: none;">
                <span class="category">Pratik</span>
                <h2>Lablar</h2>
                <p>Uygulamalı simülasyon çözümleri.</p>
            </div>
        </div>
        <div class="bento-footer">
            <span class="lang-en">Enter Range &rarr;</span>
            <span class="lang-tr" style="display: none;">İstasyonu Başlat &rarr;</span>
        </div>
    </a>

    <a href="/about/" class="bento-item about-card">
        <div class="bento-content">
            <div class="lang-en">
                <span class="category">Identity</span>
                <h2>About</h2>
                <p>The mission and the person behind the screen.</p>
            </div>
            <div class="lang-tr" style="display: none;">
                <span class="category">Kimlik</span>
                <h2>Hakkımda</h2>
                <p>Ekranın arkasındaki kişi ve vizyonu.</p>
            </div>
        </div>
        <div class="bento-footer">
            <span class="lang-en">Meet Operator &rarr;</span>
            <span class="lang-tr" style="display: none;">Operatörle Tanış &rarr;</span>
        </div>
    </a>

</div>

<style>
    /* TYPOGRAPHY & COLORS */
    :root {
        --bg-fresh: #0d1117;
        --card-bg: #161b22;
        --text-main: #f0f6fc;
        --text-dim: #8b949e;
        --accent-blue: #58a6ff;
    }

    .fresh-hero {
        padding: 3rem 0 2rem 0;
        text-align: left;
    }

    .status-badge {
        background: rgba(35, 134, 54, 0.1);
        color: #3fb950;
        border: 1px solid rgba(35, 134, 54, 0.3);
        padding: 5px 12px;
        border-radius: 20px;
        font-size: 11px;
        font-weight: 600;
        text-transform: uppercase;
        letter-spacing: 0.5px;
    }

    .main-title {
        font-size: clamp(2.5rem, 8vw, 4rem);
        font-weight: 800;
        margin: 1rem 0;
        letter-spacing: -1.5px;
        color: var(--text-main);
        line-height: 1;
    }

    .main-subtitle {
        font-size: 1.2rem;
        color: var(--text-dim);
        max-width: 600px;
        line-height: 1.5;
    }

    /* BENTO GRID LAYOUT */
    .bento-grid {
        display: grid;
        grid-template-columns: 1.8fr 1fr;
        gap: 15px;
        margin-top: 2rem;
    }

    .bento-item {
        background: var(--card-bg);
        border: 1px solid #30363d;
        border-radius: 20px;
        padding: 30px;
        text-decoration: none !important;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        min-height: 200px;
    }

    .blog-card { grid-row: span 2; }

    .bento-item:hover {
        border-color: var(--accent-blue);
        transform: scale(1.02);
        box-shadow: 0 20px 40px rgba(0,0,0,0.4);
        background: #1c2128;
    }

    .category {
        font-size: 10px;
        text-transform: uppercase;
        color: var(--accent-blue);
        letter-spacing: 2px;
        font-weight: 800;
    }

    .bento-content h2 {
        font-size: 1.8rem;
        margin: 10px 0;
        color: var(--text-main);
    }

    .bento-content p {
        color: var(--text-dim);
        font-size: 0.95rem;
        line-height: 1.5;
    }

    .bento-footer {
        font-size: 13px;
        font-weight: 600;
        color: var(--text-main);
        opacity: 0.8;
    }

    /* RESPONSIVE DESIGN */
    @media (max-width: 850px) {
        .bento-grid { grid-template-columns: 1fr; }
        .blog-card { grid-row: span 1; }
        .main-title { font-size: 2.8rem; }
    }
</style>
