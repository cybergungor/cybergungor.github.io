---
layout: default
title: Labs
permalink: /labs/
---

<div class="feed-container">
    <div class="feed-header">
        <h1>./start_simulation.sh</h1>
        <p class="terminal-text">>> Initializing Blue Team scenarios and investigation reports...</p>
    </div>

    <div class="post-list">
        {% for post in site.posts %}
            {% if post.categories contains 'labs' %}
            <a href="{{ post.url }}" class="post-item lab-item">
                <div class="post-meta">
                    <span class="post-date">{{ post.date | date: "%d %b %Y" }}</span>
                    <span class="post-tag status-done">COMPLETED</span>
                </div>
                
                <h3 class="post-title">{{ post.title }}</h3>
                
                <div class="post-arrow">
                    <span>VIEW_REPORT</span>
                    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <circle cx="11" cy="11" r="8"></circle>
                        <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
                        <line x1="11" y1="8" x2="11" y2="14"></line>
                        <line x1="8" y1="11" x2="14" y2="11"></line>
                    </svg>
                </div>
            </a>
            {% endif %}
        {% endfor %}
    </div>
</div>

<style>
/* ===== LABS ÖZEL TASARIM ===== */
.feed-container {
    max-width: 800px;
    margin: 0 auto;
}

.feed-header {
    margin-bottom: 3rem;
    border-bottom: 1px solid var(--border-color);
    padding-bottom: 1rem;
}

.terminal-text {
    color: var(--accent);
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.9rem;
    margin-top: 0.5rem;
}

/* Liste Elemanları */
.post-list {
    display: flex;
    flex-direction: column;
    gap: 1rem;
}

.post-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: var(--card-bg);
    border: 1px solid var(--border-color);
    padding: 1.5rem;
    border-radius: 8px;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    position: relative;
    overflow: hidden;
    text-decoration: none; /* Link alt çizgisini kaldır */
}

/* Labs için Yeşil/Turkuaz vurgu (Blue Team hissi) */
.lab-item:hover {
    transform: translateX(10px);
    border-color: #3fb950; /* Lablarda Yeşil Vurgu */
    box-shadow: -5px 0 15px -5px rgba(63, 185, 80, 0.2);
}

.lab-item::before {
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    width: 3px;
    background: #3fb950; /* Yeşil çizgi */
    opacity: 0;
    transition: opacity 0.3s;
}

.lab-item:hover::before {
    opacity: 1;
}

/* İçerik Düzeni */
.post-meta {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    min-width: 120px;
    border-right: 1px solid var(--border-color);
    margin-right: 1.5rem;
    padding-right: 1.5rem;
}

.post-date {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.85rem;
    color: var(--text-muted);
}

/* Durum Etiketi */
.status-done {
    font-size: 0.7rem;
    color: #3fb950; /* Yeşil yazı */
    background: rgba(63, 185, 80, 0.1);
    padding: 2px 6px;
    border-radius: 4px;
    text-align: center;
    border: 1px solid rgba(63, 185, 80, 0.3);
    font-weight: bold;
}

.post-title {
    flex-grow: 1;
    margin: 0;
    font-size: 1.1rem;
    color: #e6edf3;
    font-weight: 500;
}

/* Ok İkonu ve Yazısı */
.post-arrow {
    display: flex;
    align-items: center;
    gap: 10px;
    color: var(--text-muted);
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.8rem;
    transition: color 0.3s;
}

.lab-item:hover .post-arrow {
    color: #3fb950; /* Hover olunca yeşil olsun */
}

/* MOBİL UYUM */
@media (max-width: 600px) {
    .post-item { flex-direction: column; align-items: flex-start; gap: 1rem; }
    .post-meta { border-right: none; margin-bottom: 0.5rem; border-bottom: 1px solid var(--border-color); width: 100%; padding-bottom: 0.5rem; align-items: flex-start;}
    .post-arrow { align-self: flex-end; }
}
</style>
