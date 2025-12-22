---
layout: null
permalink: /admin/
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>SECURE_INTERCEPT // EMIRHAN_GUNGOROGLU</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #0b0e14; /* Kurumsal Siyah */
            --surface: #161b22; /* Panel Rengi */
            --border: #30363d; /* İnce Kenarlık */
            --accent: #58a6ff; /* Splunk/Sentinel Mavisi */
            --alert: #f85149; /* Kritik Hata Kırmızısı */
            --text-main: #d1d5db;
        }

        body, html { margin: 0; padding: 0; height: 100%; background: var(--bg); color: var(--text-main); font-family: 'Inter', sans-serif; display: flex; align-items: center; justify-content: center; }

        /* --- ANA RAPOR PANELİ --- */
        .incident-dashboard {
            width: 90%; max-width: 850px;
            background: var(--surface);
            border: 1px solid var(--border);
            padding: 40px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.5);
        }

        /* Üst Bilgi */
        .header { display: flex; justify-content: space-between; align-items: flex-start; border-bottom: 1px solid var(--border); padding-bottom: 25px; margin-bottom: 30px; }
        .header-title h1 { margin: 0; font-family: 'JetBrains Mono'; font-size: 1.1rem; color: var(--text-main); letter-spacing: 1px; }
        .header-title p { margin: 5px 0 0; font-size: 0.75rem; color: #8b949e; text-transform: uppercase; }
        .status-badge { background: rgba(248, 81, 73, 0.1); border: 1px solid var(--alert); color: var(--alert); padding: 4px 12px; font-size: 10px; font-family: 'JetBrains Mono'; font-weight: bold; border-radius: 2px; }

        /* Veri Tablosu */
        .metadata-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 20px; margin-bottom: 35px; }
        .data-point { background: rgba(255,255,255,0.02); border: 1px solid var(--border); padding: 15px; }
        .data-label { font-size: 10px; color: #8b949e; font-family: 'JetBrains Mono'; margin-bottom: 5px; text-transform: uppercase; }
        .data-value { font-family: 'JetBrains Mono'; font-size: 0.9rem; color: var(--accent); }

        /* Sistem Logları */
        .console-logs { background: #050608; border-radius: 4px; padding: 20px; font-family: 'JetBrains Mono'; font-size: 12px; line-height: 1.6; height: 150px; overflow: hidden; margin-bottom: 30px; }
        .log-line { color: #484f58; margin-bottom: 4px; }
        .log-line.active { color: var(--accent); }

        /* Açıklama Alanı (Başta gizli) */
        #reveal-zone { display: none; animation: fadeIn 0.8s ease-out; }
        .deception-note { border-top: 1px solid var(--border); padding-top: 30px; }
        .deception-note h2 { font-size: 1rem; color: var(--accent); margin-bottom: 15px; font-family: 'JetBrains Mono'; }
        .deception-note p { font-size: 0.9rem; color: #8b949e; line-height: 1.7; margin-bottom: 20px; }

        .action-btn {
            display: inline-block; padding: 10px 25px;
            border: 1px solid var(--accent); color: var(--accent);
            text-decoration: none; font-size: 0.8rem; font-family: 'JetBrains Mono';
            transition: 0.3s;
        }
        .action-btn:hover { background: var(--accent); color: var(--bg); }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

<div class="incident-dashboard">
    <div class="header">
        <div class="header-title">
            <h1>INTELLIGENCE_REPORT // CASE_ID: 8392-ALPHA</h1>
            <p>Unauthorized Directory Traversal Attempt Detected</p>
        </div>
        <div class="status-badge">BREACH_ATTEMPT</div>
    </div>

    <div class="metadata-grid">
        <div class="data-point">
            <div class="data-label">Origin Network</div>
            <div id="origin-ip" class="data-value">SPOOFED_IP_MASKED</div>
        </div>
        <div class="data-point">
            <div class="data-label">Timestamp</div>
            <div id="timestamp" class="data-value">-- : -- : --</div>
        </div>
    </div>

    <div class="console-logs" id="log-box">
        </div>

    <div id="reveal-zone">
        <div class="deception-note">
            <h2>[ANALYSIS] ALDATMA TEKNOLOJİSİ (HONEYPOT)</h2>
            <p>
                Bu sayfa, kurumsal siber güvenlik stratejilerinde kullanılan <strong>"Active Defense"</strong> mekanizmalarının bir parçası olarak tasarlanmıştır. 
                Giriş denemeniz, sistem tarafından bir tehdit aktörü veya keşif (recon) botu olarak sınıflandırılmış ve bu özel "Bal Küpü" (Honeypot) segmentine yönlendirilmiştir.
                <br><br>
                Bu çalışma, siber saldırganların davranış modellerini analiz etmek ve gerçek sistem varlıklarını korumak amacıyla kurduğum bir siber güvenlik laboratuvarı deneyidir.
            </p>
            <a href="/" class="action-btn">RETURN TO ROOT_SERVER</a>
        </div>
    </div>
</div>

<script>
    const logBox = document.getElementById('log-box');
    const revealZone = document.getElementById('reveal-zone');
    document.getElementById('timestamp').innerText = new Date().toISOString().replace('T', ' ').split('.')[0];
    document.getElementById('origin-ip').innerText = Array(4).fill(0).map(()=>Math.floor(Math.random()*255)).join('.');

    const sequence = [
        "> [INFO] Intercepting request at /admin layer...",
        "> [WARN] User identity unverified. Initiating fingerprinting...",
        "> [INFO] Metadata correlation in progress (Heuristic Analysis)...",
        "> [INFO] Logging connection string and HTTP headers...",
        "> [SOC] Notifying Primary Operator: EMIRHAN_G...",
        "> [ALERT] SESSION_LOCKED // REDIRECTING_TO_HONEYPOT_SEGMENT"
    ];

    let i = 0;
    function runSequence() {
        if (i < sequence.length) {
            const p = document.createElement('div');
            p.className = 'log-line active';
            p.innerText = sequence[i];
            logBox.appendChild(p);
            i++;
            setTimeout(runSequence, 900);
        } else {
            setTimeout(() => {
                revealZone.style.display = 'block';
            }, 1000);
        }
    }

    window.onload = runSequence;
</script>

</body>
</html>
