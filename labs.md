---
layout: default
title: Labs
permalink: /labs/
categories: [labs]
---
<div style="text-align: center; margin-top: 50px;">
    <a href="/labs/station/" style="background: #238636; color: white; padding: 15px 30px; border-radius: 8px; text-decoration: none; font-weight: bold;">
        🚀 ENTER CYBER RANGE TERMINAL
    </a>
</div>
<div class="feed-container">
    <div class="feed-header">
        <h1>./start.sh</h1>
        <p class="terminal-text">>> Initializing Blue Team scenarios and investigation reports...</p>
        
        <div class="search-wrapper">
            <span class="prompt">root@labs:~$ grep -i</span>
            <input type="text" id="labSearch" placeholder='"investigation_id"...' autocomplete="off">
        </div>
    </div>

    <div class="post-list" id="labContainer">
        {% for post in site.posts %}
            {% if post.categories contains 'labs' or post.tags contains 'labs' %}
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
        
        <div id="noResults" style="display: none; text-align: center; color: var(--text-muted); padding: 2rem; border: 1px dashed #30363d; margin-top: 1rem; border-radius: 8px;">
            >> ERROR: Simulation ID not found.
        </div>
    </div>
</div>

<script>
document.getElementById('labSearch').addEventListener('keyup', function() {
    let filter = this.value.toLowerCase();
    let posts = document.getElementsByClassName('lab-item'); // Sadece lab itemlarını seçer
    let hasResults = false;

    for (let i = 0; i < posts.length; i++) {
        let text = posts[i].innerText.toLowerCase();
        
        if (text.includes(filter)) {
            posts[i].style.display = ""; 
            hasResults = true;
        } else {
            posts[i].style.display = "none";
        }
    }

    document.getElementById('noResults').style.display = hasResults ? "none" : "block";
});
</script>

<style>
/* ===== LABS VE ARAMA TASARIMI ===== */
.feed-container { max-width: 800px; margin: 0 auto; }
.feed-header { margin-bottom: 2rem; border-bottom: 1px solid var(--border-color); padding-bottom: 1rem; }
.terminal-text { color: var(--accent); font-family: 'JetBrains Mono', monospace; font-size: 0.9rem; margin-top: 0.5rem; }

/* Arama Kutusu (Labs Özel Rengi: Yeşil) */
.search-wrapper {
    display: flex;
    align-items: center;
    background: #0d1117;
    border: 1px solid var(--border-color);
    padding: 0.5rem 1rem;
    border-radius: 6px;
    margin-top: 1.5rem;
    font-family: 'JetBrains Mono', monospace;
}
.search-wrapper:focus-within {
    border-color: #3fb950; /* Yeşil Vurgu */
    box-shadow: 0 0 10px rgba(63, 185, 80, 0.2);
}
.prompt {
    color: #3fb950; /* Prompt Rengi */
    margin-right: 10px;
    white-space: nowrap;
    font-size: 0.9rem;
}
#labSearch {
    background: transparent;
    border: none;
    color: #e6edf3;
    width: 100%;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.9rem;
    outline: none;
}

/* Liste Elemanları */
.post-list { display: flex; flex-direction: column; gap: 1rem; }
.post-item {
    display: flex; align-items: center; justify-content: space-between;
    background: var(--card-bg); border: 1px solid var(--border-color);
    padding: 1.5rem; border-radius: 8px;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    text-decoration: none; position: relative; overflow: hidden;
}

/* Labs Yeşil Hover Efekti */
.lab-item:hover {
    transform: translateX(10px);
    border-color: #3fb950;
    box-shadow: -5px 0 15px -5px rgba(63, 185, 80, 0.2);
}
.lab-item::before {
    content: ''; position: absolute; left: 0; top: 0; bottom: 0; width: 3px;
    background: #3fb950; opacity: 0; transition: opacity 0.3s;
}
.lab-item:hover::before { opacity: 1; }

.post-meta {
    display: flex; flex-direction: column; gap: 0.5rem; min-width: 120px;
    border-right: 1px solid var(--border-color); margin-right: 1.5rem; padding-right: 1.5rem;
}
.post-date { font-family: 'JetBrains Mono', monospace; font-size: 0.85rem; color: var(--text-muted); }
.status-done {
    font-size: 0.7rem; color: #3fb950; background: rgba(63, 185, 80, 0.1);
    padding: 2px 6px; border-radius: 4px; text-align: center;
    border: 1px solid rgba(63, 185, 80, 0.3); font-weight: bold;
}

.post-title { flex-grow: 1; margin: 0; font-size: 1.1rem; color: #e6edf3; font-weight: 500; }
.post-arrow {
    display: flex; align-items: center; gap: 10px; color: var(--text-muted);
    font-family: 'JetBrains Mono', monospace; font-size: 0.8rem; transition: color 0.3s;
}
.lab-item:hover .post-arrow { color: #3fb950; }

@media (max-width: 600px) {
    .post-item { flex-direction: column; align-items: flex-start; gap: 1rem; }
    .post-meta { border-right: none; margin-bottom: 0.5rem; border-bottom: 1px solid var(--border-color); width: 100%; padding-bottom: 0.5rem; }
    .post-arrow { align-self: flex-end; }
}
</style>
