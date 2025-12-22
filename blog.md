---
layout: default
title: Blog
permalink: /blog/
---

<div class="infinite-log-bg">
    <div class="log-col"><div class="moving-stream s1">
        <span>[DB_QUERY] SELECT * FROM security_logs</span>
        <span>[ANALYSIS] Pattern matching: T1059.001</span>
        <span>[STATUS] Indexing new write-ups...</span>
        <span>[INFO] Database connection: STABLE</span>
        <span>[DB_QUERY] SELECT * FROM security_logs</span>
        <span>[ANALYSIS] Pattern matching: T1059.001</span>
    </div></div>
    <div class="log-col hide-mobile"><div class="moving-stream s2">
        <span>0x42 0x4C 0x4F 0x47 0x5F 0x44 0x41 0x54 0x41</span>
        <span>FETCHING_METADATA: SUCCESS</span>
        <span>PARSING_MARKDOWN_FILES...</span>
        <span>0x42 0x4C 0x4F 0x47 0x5F 0x44 0x41 0x54 0x41</span>
    </div></div>
</div>

<div class="feed-container">
    <div class="feed-header">
        <span class="status-badge">Knowledge Base</span>
        <h1 class="page-title">Blog / Investigations</h1>
        <p class="terminal-text">>> Listing all write-ups and security notes...</p>
        
        <div class="search-wrapper">
            <span class="prompt">root@cyberlab:~/blog$ grep -i</span>
            <input type="text" id="searchInput" placeholder='"search_term"' autocomplete="off">
        </div>
    </div>

    <div class="post-list" id="postList">
        {% for post in site.posts %}
            {% if post.categories contains 'blog' or post.tags contains 'blog' %}
            <a href="{{ post.url }}" class="post-item" data-title="{{ post.title | downcase }}">
                <div class="post-meta">
                    <span class="post-tag">#{{ post.categories | first | upcase }}</span>
                    <span class="post-date">{{ post.date | date: "%d %b %Y" }}</span>
                </div>
                <div class="post-content">
                    <h3 class="post-title">{{ post.title }}</h3>
                    <p class="post-excerpt">{{ post.excerpt | strip_html | truncatewords: 15 }}</p>
                </div>
                <div class="post-arrow">
                    <span>ANALYZE</span>
                    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <path d="M5 12h14M12 5l7 7-7 7"/>
                    </svg>
                </div>
            </a>
            {% endif %}
        {% endfor %}
    </div>
    
    <div id="noResults" class="error-msg" style="display: none;">
        <p>>> Error: Pattern not found in local database.</p>
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
            if (title.indexOf(filter) > -1) {
                items[i].style.display = "flex";
                hasResults = true;
            } else {
                items[i].style.display = "none";
            }
        }
        document.getElementById('noResults').style.display = hasResults ? "none" : "block";
    });
</script>

<style>
/* --- CORE INTEGRATION --- */
:root {
    --bg: #08090a;
    --card-bg: rgba(17, 18, 20, 0.9);
    --accent: #00f2ff;
    --border: rgba(255, 255, 255, 0.06);
}

body { background-color: var(--bg); color: #c9d1d9; font-family: 'Inter', sans-serif; }

/* --- LOG BACKGROUND (Birebir Home ile aynı) --- */
.infinite-log-bg {
    position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
    display: flex; justify-content: space-around; opacity: 0.07;
    z-index: -1; pointer-events: none; font-family: 'JetBrains Mono', monospace; font-size: 10px;
}
.moving-stream { display: flex; flex-direction: column; animation: scrollDown infinite linear; }
.moving-stream span { padding: 12px 0; color: var(--accent); }
@keyframes scrollDown { 0% { transform: translateY(-50%); } 100% { transform: translateY(0); } }
.s1 { animation-duration: 35s; } .s2 { animation-duration: 50s; }

/* --- FEED CONTAINER --- */
.feed-container { max-width: 950px; margin: 0 auto; padding: 3rem 1.5rem; }

.page-title { 
    font-size: 2.8rem; font-weight: 800; letter-spacing: -1.5px; color: #fff; margin: 10px 0; 
    text-shadow: 0 0 15px rgba(0, 242, 255, 0.1);
}

.terminal-text { color: var(--accent); font-family: 'JetBrains Mono'; font-size: 0.85rem; opacity: 0.8; margin-bottom: 2rem; }

/* --- SEARCH WRAPPER --- */
.search-wrapper {
    display: flex; align-items: center; background: rgba(0,0,0,0.3);
    border: 1px solid var(--border); padding: 12px 20px; border-radius: 12px;
    font-family: 'JetBrains Mono', monospace; margin-bottom: 3rem; backdrop-filter: blur(5px);
}
.search-wrapper:focus-within { border-color: var(--accent); box-shadow: 0 0 15px rgba(0, 242, 255, 0.1); }
.prompt { color: #3fb950; margin-right: 12px; font-size: 0.9rem; }
#searchInput { background: transparent; border: none; color: #fff; width: 100%; outline: none; font-size: 0.95rem; }

/* --- POST ITEMS (Tactical Design) --- */
.post-list { display: flex; flex-direction: column; gap: 12px; }

.post-item {
    display: flex; align-items: center; background: var(--card-bg);
    border: 1px solid var(--border); padding: 20px 30px; border-radius: 16px;
    text-decoration: none; transition: 0.3s cubic-bezier(0.2, 0, 0, 1);
    backdrop-filter: blur(10px);
}

.post-item:hover {
    transform: translateX(10px); border-color: var(--accent);
    background: rgba(22, 24, 28, 0.95); box-shadow: 0 10px 30px rgba(0,0,0,0.5);
}

.post-meta {
    display: flex; flex-direction: column; gap: 4px; min-width: 140px;
    border-right: 1px solid var(--border); margin-right: 25px; padding-right: 20px;
}
.post-date { font-family: 'JetBrains Mono'; font-size: 11px; color: #8b949e; }
.post-tag { font-family: 'JetBrains Mono'; font-size: 10px; color: var(--accent); font-weight: bold; }

.post-content { flex-grow: 1; }
.post-title { font-size: 1.25rem; color: #fff; margin: 0 0 5px 0; font-weight: 700; }
.post-excerpt { color: #8b949e; font-size: 0.9rem; margin: 0; line-height: 1.4; }

.post-arrow { display: flex; align-items: center; gap: 10px; color: #8b949e; font-family: 'JetBrains Mono'; font-size: 11px; }
.post-item:hover .post-arrow { color: var(--accent); }

.error-msg { text-align: center; color: #8b949e; margin-top: 3rem; font-family: 'JetBrains Mono'; }

.status-badge { display: inline-block; background: rgba(0, 242, 255, 0.1); color: var(--accent); padding: 4px 12px; border-radius: 20px; font-size: 10px; font-weight: bold; text-transform: uppercase; letter-spacing: 1px; }

@media (max-width: 768px) {
    .post-item { flex-direction: column; align-items: flex-start; }
    .post-meta { border-right: none; border-bottom: 1px solid var(--border); width: 100%; margin: 0 0 15px 0; padding: 0 0 10px 0; }
    .post-arrow { align-self: flex-end; margin-top: 10px; }
}
</style>
