---
layout: default
title: Labs
permalink: /labs/
categories: [labs]
---

<div class="infinite-log-bg">
    <div class="log-col"><div class="moving-stream s1">
        <span>[SCAN] Nmap: 10.0.0.5 -p 80,443,22,3389 [UP]</span>
        <span>[VULN] CVE-2021-3177 (Python RCE) - CRITICAL</span>
        <span>[LAB] Simulation Station: NODE_04_ACTIVE</span>
        <span>[EXPL] MS17-010: EternalBlue payload delivered</span>
        <span>[SCAN] Nmap: 10.0.0.5 -p 80,443,22,3389 [UP]</span>
    </div></div>
    <div class="log-col hide-mobile"><div class="moving-stream s2">
        <span>0x53 0x49 0x4D 0x55 0x4C 0x41 0x54 0x49 0x4F 0x4E</span>
        <span>EXPLOITATION_SUCCESS: root access obtained</span>
        <span>BRUTE_FORCE: Attempting SSH (User: admin)</span>
        <span>0x53 0x49 0x4D 0x55 0x4C 0x41 0x54 0x49 0x4F 0x4E</span>
    </div></div>
</div>

<div class="feed-container">
    <div class="feed-header">
        <span class="status-badge">Cyber Range</span>
        <h1 class="page-title">./start.sh --labs</h1>
        
        <div class="terminal-entry">
            <a href="/labs/station/" class="terminal-btn">
                <span class="btn-icon">🚀</span> ENTER_CYBER_GUNGOR_TERMINAL
            </a>
        </div>

        <p class="terminal-text">>> Initializing Blue Team scenarios and investigation reports...</p>
        
        <div class="search-wrapper">
            <span class="prompt">root@cyberlab:~/labs$ grep -i</span>
            <input type="text" id="labSearch" placeholder='"investigation_id"...' autocomplete="off">
        </div>
    </div>

    <div class="post-list" id="labContainer">
        {% for post in site.posts %}
            {% if post.categories contains 'labs' or post.tags contains 'labs' %}
            <a href="{{ post.url }}" class="post-item lab-item" data-search="{{ post.title | downcase }}">
                <div class="post-meta">
                    <span class="post-tag status-done">COMPLETED</span>
                    <span class="post-date">{{ post.date | date: "%d %b %Y" }}</span>
                </div>
                <div class="post-content">
                    <h3 class="post-title">{{ post.title }}</h3>
                </div>
                <div class="post-arrow">
                    <span>VIEW_REPORT</span>
                    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                        <circle cx="11" cy="11" r="8"></circle>
                        <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
                    </svg>
                </div>
            </a>
            {% endif %}
        {% endfor %}
    </div>
    
    <div id="noResults" class="error-msg" style="display: none;">
        <p>>> ERROR: Simulation ID not found in range.</p>
    </div>
</div>

<script>
document.getElementById('labSearch').addEventListener('keyup', function() {
    let filter = this.value.toLowerCase();
    let posts = document.getElementsByClassName('lab-item');
    let hasResults = false;

    for (let i = 0; i < posts.length; i++) {
        let text = posts[i].getAttribute('data-search');
        if (text.includes(filter)) {
            posts[i].style.display = "flex"; 
            hasResults = true;
        } else {
            posts[i].style.display = "none";
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

/* --- LOG BACKGROUND --- */
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

.terminal-entry { margin: 1.5rem 0 2rem 0; }
.terminal-btn {
    display: inline-flex; align-items: center; gap: 12px;
    background: rgba(0, 242, 255, 0.05); color: var(--accent);
    padding: 12px 24px; border: 1px solid var(--accent); border-radius: 8px;
    text-decoration: none; font-family: 'JetBrains Mono'; font-size: 0.9rem; font-weight: bold;
    transition: 0.3s;
}
.terminal-btn:hover { background: var(--accent); color: #000; box-shadow: 0 0 20px rgba(0, 242, 255, 0.3); }

.terminal-text { color: var(--accent); font-family: 'JetBrains Mono'; font-size: 0.85rem; opacity: 0.8; }

/* --- SEARCH WRAPPER --- */
.search-wrapper {
    display: flex; align-items: center; background: rgba(0,0,0,0.3);
    border: 1px solid var(--border); padding: 12px 20px; border-radius: 12px;
    font-family: 'JetBrains Mono', monospace; margin-top: 2rem; margin-bottom: 3rem; backdrop-filter: blur(5px);
}
.search-wrapper:focus-within { border-color: var(--accent); box-shadow: 0 0 15px rgba(0, 242, 255, 0.1); }
.prompt { color: #3fb950; margin-right: 12px; font-size: 0.9rem; }
#labSearch { background: transparent; border: none; color: #fff; width: 100%; outline: none; font-size: 0.95rem; }

/* --- LAB ITEMS --- */
.post-list { display: flex; flex-direction: column; gap: 12px; }
.post-item {
    display: flex; align-items: center; background: var(--card-bg);
    border: 1px solid var(--border); padding: 20px 30px; border-radius: 16px;
    text-decoration: none; transition: 0.3s cubic-bezier(0.2, 0, 0, 1);
    backdrop-filter: blur(10px);
}
.post-item:hover { transform: translateX(10px); border-color: var(--accent); }

.post-meta {
    display: flex; flex-direction: column; gap: 6px; min-width: 140px;
    border-right: 1px solid var(--border); margin-right: 25px; padding-right: 20px;
}
.post-date { font-family: 'JetBrains Mono'; font-size: 11px; color: #8b949e; }
.status-done { font-size: 9px; color: #3fb950; font-weight: 800; border: 1px solid rgba(63, 185, 80, 0.3); background: rgba(63, 185, 80, 0.05); padding: 2px 6px; border-radius: 4px; text-align: center; }

.post-content { flex-grow: 1; }
.post-title { font-size: 1.25rem; color: #fff; margin: 0; font-weight: 700; }
.post-arrow { display: flex; align-items: center; gap: 10px; color: #8b949e; font-family: 'JetBrains Mono'; font-size: 11px; }
.post-item:hover .post-arrow { color: var(--accent); }

.error-msg { text-align: center; color: #8b949e; margin-top: 3rem; font-family: 'JetBrains Mono'; }
.status-badge { display: inline-block; background: rgba(0, 242, 255, 0.1); color: var(--accent); padding: 4px 12px; border-radius: 20px; font-size: 10px; font-weight: bold; text-transform: uppercase; letter-spacing: 1px; }

@media (max-width: 768px) {
    .post-item { flex-direction: column; align-items: flex-start; }
    .post-meta { border-right: none; border-bottom: 1px solid var(--border); width: 100%; margin: 0 0 15px 0; padding: 0 0 10px 0; }
    .post-arrow { align-self: flex-end; }
}
</style>
