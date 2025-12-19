---
layout: default
title: SOC Dashboard
permalink: /alerts/
---

<div class="soar-layout">
    
    <div class="main-panel">
        
        <div class="stats-grid">
            <div class="stat-card danger-glow">
                <h3>CRITICAL</h3>
                <div class="number">2</div>
            </div>
            <div class="stat-card warning-glow">
                <h3>HIGH</h3>
                <div class="number">5</div>
            </div>
            <div class="stat-card info-glow">
                <h3>OPEN</h3>
                <div class="number">7</div>
            </div>
        </div>

        <div class="alerts-table-wrapper">
            <div class="table-header">
                <span class="blink">●</span> LIVE THREAT FEED // SPLUNK_ES_DEMO
            </div>
            <table class="soar-table">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>RULE NAME</th>
                        <th>SEVERITY</th>
                        <th>STATUS</th>
                        <th>ACTION</th>
                    </tr>
                </thead>
                <tbody>
                    <tr id="alert-101">
                        <td class="mono">#101</td>
                        <td>Mimikatz.exe Exec</td>
                        <td><span class="badge critical">CRITICAL</span></td>
                        <td><span class="status new">NEW</span></td>
                        <td><button class="btn-investigate" onclick="investigate('alert-101')">ANALYZE</button></td>
                    </tr>
                    <tr id="alert-102">
                        <td class="mono">#102</td>
                        <td>Brute Force (RDP)</td>
                        <td><span class="badge high">HIGH</span></td>
                        <td><span class="status new">NEW</span></td>
                        <td><button class="btn-investigate" onclick="investigate('alert-102')">ANALYZE</button></td>
                    </tr>
                    <tr id="alert-103">
                        <td class="mono">#103</td>
                        <td>Suspicious PowerShell</td>
                        <td><span class="badge medium">MEDIUM</span></td>
                        <td><span class="status progress">IN PROG</span></td>
                        <td><button class="btn-investigate" disabled>ASSIGNED</button></td>
                    </tr>
                     <tr id="alert-104">
                        <td class="mono">#104</td>
                        <td>User Added to Admin</td>
                        <td><span class="badge critical">CRITICAL</span></td>
                        <td><span class="status closed">CLOSED</span></td>
                        <td><button class="btn-investigate" disabled>RESOLVED</button></td>
                    </tr>
                     <tr id="alert-105">
                        <td class="mono">#105</td>
                        <td>Internal Port Scan</td>
                        <td><span class="badge low">LOW</span></td>
                        <td><span class="status closed">CLOSED</span></td>
                        <td><button class="btn-investigate" disabled>FALSE POS</button></td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>

    <div class="side-panel">
        <div class="info-box">
            <div class="info-header">
                <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="16" x2="12" y2="12"></line><line x1="12" y1="8" x2="12.01" y2="8"></line></svg>
                SİMÜLASYON MODU
            </div>
            <p>
                Bu ekran, gerçek bir <strong>SOAR (Güvenlik Orkestrasyonu ve Müdahalesi)</strong> ürününün arayüzünü simüle etmek amacıyla, tamamen <strong>eğitim ve bilgilendirme</strong> amaçlı tasarlanmıştır.
            </p>
            <hr>
            <p>
                Burada gördüğünüz alarmlar ve veriler örnektir. Amacım, ziyaretçilere bir SOC analistinin günlük hayatta kullandığı operasyonel panellerin (Splunk, QRadar, Sentinel vb.) nasıl göründüğünü ve işlediğini göstermektir.
            </p>
            <div class="system-meta">
                <span>SYSTEM:</span> DEMO_ENV<br>
                <span>VERSION:</span> v2.4.0<br>
                <span>ACCESS:</span> READ_ONLY
            </div>
        </div>
    </div>

</div>

<script>
    function investigate(rowId) {
        let row = document.getElementById(rowId);
        let statusSpan = row.querySelector('.status');
        let btn = row.querySelector('.btn-investigate');
        
        statusSpan.className = 'status progress';
        statusSpan.innerText = 'IN PROGRESS';
        
        btn.innerText = 'LOADING...';
        btn.disabled = true;
        
        row.style.backgroundColor = "rgba(88, 166, 255, 0.1)";
        
        setTimeout(() => {
            alert("Vaka #" + rowId + " incelemeye alındı.\nOperatör: Emirhan G.");
            btn.innerText = 'ASSIGNED';
        }, 500);
    }
</script>

<style>
    /* ===== SAYFA DÜZENİ (GRID) ===== */
    .soar-layout {
        display: flex;
        flex-wrap: wrap;
        gap: 2rem;
        align-items: flex-start;
    }

    .main-panel {
        flex: 3; /* Sol taraf 3 birim genişlikte */
        min-width: 300px;
    }

    .side-panel {
        flex: 1.2; /* Sağ taraf 1.2 birim genişlikte */
        min-width: 250px;
    }

    /* ===== INFO BOX (SAĞ PANEL) TASARIMI ===== */
    .info-box {
        background: rgba(22, 27, 34, 0.8);
        border: 1px solid #30363d;
        border-top: 3px solid var(--accent);
        padding: 1.5rem;
        border-radius: 6px;
        font-size: 0.9rem;
        color: var(--text-muted);
        position: sticky; /* Aşağı kaydırınca sabit kalsın */
        top: 100px; 
    }

    .info-header {
        display: flex;
        align-items: center;
        gap: 10px;
        color: #e6edf3;
        font-weight: bold;
        font-family: 'JetBrains Mono', monospace;
        margin-bottom: 1rem;
        padding-bottom: 0.5rem;
        border-bottom: 1px dashed #30363d;
    }

    .info-box p { margin-bottom: 1rem; line-height: 1.6; }
    .info-box hr { border: 0; border-top: 1px solid #30363d; margin: 1rem 0; }
    
    .system-meta {
        font-family: 'JetBrains Mono', monospace;
        font-size: 0.75rem;
        color: #58a6ff;
        background: rgba(88, 166, 255, 0.1);
        padding: 10px;
        border-radius: 4px;
    }

    /* ===== DASHBOARD TASARIMI ===== */
    .stats-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
        gap: 1rem;
        margin-bottom: 2rem;
    }

    .stat-card {
        background: #161b22;
        border: 1px solid #30363d;
        padding: 1rem;
        border-radius: 6px;
        text-align: center;
    }

    .stat-card h3 { margin: 0; font-size: 0.8rem; color: #8b949e; letter-spacing: 1px; }
    .stat-card .number { font-size: 2rem; font-weight: bold; color: #e6edf3; font-family: 'JetBrains Mono', monospace; }

    .danger-glow { border-bottom: 3px solid #ff5f56; }
    .warning-glow { border-bottom: 3px solid #d29922; }
    .info-glow { border-bottom: 3px solid #58a6ff; }

    /* Tablo Alanı */
    .alerts-table-wrapper {
        background: #161b22;
        border: 1px solid #30363d;
        border-radius: 8px;
        overflow: hidden;
    }
    
    .table-header {
        background: #0d1117;
        padding: 10px 15px;
        font-family: 'JetBrains Mono', monospace;
        font-size: 0.75rem;
        color: #3fb950;
        border-bottom: 1px solid #30363d;
    }
    .blink { animation: blinker 1.5s linear infinite; color: #ff5f56; margin-right: 5px; }
    @keyframes blinker { 50% { opacity: 0; } }

    .soar-table {
        width: 100%;
        border-collapse: collapse;
        font-size: 0.85rem;
    }

    .soar-table th {
        background: #161b22;
        color: #8b949e;
        text-align: left;
        padding: 12px;
        border-bottom: 1px solid #30363d;
        font-size: 0.75rem;
    }

    .soar-table td {
        padding: 12px;
        border-bottom: 1px solid #30363d;
        color: #c9d1d9;
    }
    
    .soar-table tr:hover { background-color: rgba(255,255,255,0.02); }
    .mono { font-family: 'JetBrains Mono', monospace; }

    /* Rozetler ve Butonlar */
    .badge { padding: 3px 6px; border-radius: 4px; font-weight: bold; font-size: 0.65rem; }
    .critical { background: rgba(255, 95, 86, 0.15); color: #ff5f56; border: 1px solid #ff5f56; }
    .high { background: rgba(210, 153, 34, 0.15); color: #d29922; border: 1px solid #d29922; }
    .medium { background: rgba(88, 166, 255, 0.15); color: #58a6ff; border: 1px solid #58a6ff; }
    .low { background: rgba(139, 148, 158, 0.15); color: #8b949e; border: 1px solid #8b949e; }

    .status { font-weight: bold; font-size: 0.7rem; }
    .new { color: #58a6ff; }
    .progress { color: #d29922; }
    .closed { color: #3fb950; }

    .btn-investigate {
        background: transparent;
        border: 1px solid #30363d;
        color: var(--accent);
        padding: 5px 10px;
        border-radius: 4px;
        cursor: pointer;
        font-family: 'JetBrains Mono', monospace;
        font-size: 0.7rem;
        transition: all 0.2s;
    }
    .btn-investigate:hover:not(:disabled) {
        background: var(--accent);
        color: #0d1117;
    }
    .btn-investigate:disabled { color: #484f58; border-color: transparent; }

    /* Mobil Uyumluluk */
    @media (max-width: 768px) {
        .soar-layout { flex-direction: column-reverse; } /* Telefondaysa Bilgi en altta dursun veya en üstte (normal column) */
        .side-panel { width: 100%; }
    }
</style>
