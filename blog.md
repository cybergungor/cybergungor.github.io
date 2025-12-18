---
layout: default
title: Blog
permalink: /blog/
---

<div class="feed-container">
    <div class="feed-header">
        <h1>/var/log/blog_posts</h1>
        <p class="terminal-text">>> Listing all write-ups and security notes...</p>
        
        <div class="search-wrapper">
            <span class="prompt">root@blog:~$ grep -i</span>
            <input type="text" id="searchInput" placeholder='"search_term"' autocomplete="off">
        </div>
    </div>

    <div class="post-list" id="postList">
        {% for post in site.posts %}
            {% if post.categories contains 'blog' or post.tags contains 'blog' %}
            <a href="{{ post.url }}" class="post-item" data-title="{{ post.title | downcase }}">
                <div class="post-meta">
                    <span class="post-date">{{ post.date | date: "%d %b %Y" }}</span>
                    <span class="post-tag">Write-up</span>
                </div>
                <h3 class="post-title">{{ post.title }}</h3>
                <div class="post-arrow">
                    <span>READ_LOG</span>
                    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <path d="M5 12h14M12 5l7 7-7 7"/>
                    </svg>
                </div>
            </a>
            {% endif %}
        {% endfor %}
    </div>
    
    <div id="noResults" style="display: none; text-align: center; color: var(--text-muted); margin-top: 2rem;">
        <p>>> Error: Pattern not found in logs.</p>
    </div>
</div>

<script>
    document.getElementById('searchInput').addEventListener('keyup', function() {
        let filter = this.value.toLowerCase();
        let list = document.getElementById('postList');
        let items = list.getElementsByClassName('post-item');
        let hasResults = false;

        for (let i = 0; i < items.length; i++) {
            let title = items[i].getAttribute('data-title');
            
            // Eğer başlık aranan kelimeyi içeriyorsa göster, yoksa gizle
            if (title.indexOf(filter) > -1) {
                items[i].style.display = "flex";
                hasResults = true;
            } else {
                items[i].style.display = "none";
            }
        }
        
        // Hiç sonuç yoksa uyarı mesajını göster
        document.getElementById('noResults').style.display = hasResults ? "none" : "block";
    });
</script>

<style>
/* ===== BLOG AKIŞ TASARIMI & ARAMA ===== */
.feed-container {
    max-width: 800px;
    margin: 0 auto;
}

.feed-header {
    margin-bottom: 2rem;
    border-bottom: 1px solid var(--border-color);
    padding-bottom: 1rem;
}

.terminal-text {
    color: var(--accent);
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.9rem;
    margin-top: 0.5rem;
    margin-bottom: 1.5rem;
}

/* Arama Kutusu Stili */
.search-wrapper {
    display: flex;
    align-items: center;
    background: #0d1117; /* Arka planla aynı */
    border: 1px solid var(--border-color);
    padding: 10px 15px;
    border-radius: 6px;
    font-family: 'JetBrains Mono', monospace;
    margin-top: 1rem;
    transition: border-color 0.3s;
}

.search-wrapper:focus-within {
    border-color: var(--accent);
    box-shadow: 0 0 10px rgba(88, 166, 255, 0.1);
}

.prompt {
    color: #3fb950; /* Yeşil terminal promptu */
    margin-right: 10px;
    font-weight: bold;
    font-size: 0.9rem;
    white-space: nowrap;
}

#searchInput {
    background: transparent;
    border: none;
    color: #e6edf3;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.95rem;
    width: 100%;
    outline: none;
}

#searchInput::placeholder {
    color: var(--text-muted);
    opacity: 0.5;
}

/* Liste Elemanları (Mevcut Tasarımın Aynısı) */
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
    text-decoration: none;
}

.post-item:hover {
    transform: translateX(10px);
    border-color: var(--accent);
    box-shadow: -5px 0 15px -5px rgba(88, 166, 255, 0.2);
}

.post-item::before {
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    width: 3px;
    background: var(--accent);
    opacity: 0;
    transition: opacity 0.3s;
}

.post-item:hover::before { opacity: 1; }

.post-meta {
    display: flex;
    flex-direction: column;
    gap: 0.3rem;
    min-width: 120px;
    border-right: 1px solid var(--border-color);
    margin-right: 1.5rem;
    padding-right: 1.5rem;
}

.post-date { font-family: 'JetBrains Mono', monospace; font-size: 0.85rem; color: var(--text-muted); }
.post-tag { font-size: 0.75rem; color: var(--accent); text-transform: uppercase; letter-spacing: 1px; }

.post-title { flex-grow: 1; margin: 0; font-size: 1.1rem; color: #e6edf3; font-weight: 500; }

.post-arrow {
    display: flex;
    align-items: center;
    gap: 10px;
    color: var(--text-muted);
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.8rem;
    transition: color 0.3s;
}

.post-item:hover .post-arrow { color: var(--accent); }

@media (max-width: 600px) {
    .post-item { flex-direction: column; align-items: flex-start; gap: 1rem; }
    .post-meta { border-right: none; margin-bottom: 0.5rem; border-bottom: 1px solid var(--border-color); width: 100%; padding-bottom: 0.5rem; }
    .post-arrow { align-self: flex-end; }
    .prompt { font-size: 0.8rem; }
}
</style>
