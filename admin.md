---
layout: null
permalink: /admin/
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>403 - Forbidden: Access is Denied</title>
    <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #0b0e14;
            --surface: #161b22;
            --danger: #f85149;
            --text-muted: #8b949e;
            --text-main: #d1d5db;
        }

        body, html { margin: 0; padding: 0; height: 100%; background: var(--bg); font-family: 'JetBrains Mono', monospace; color: var(--text-main); display: flex; align-items: center; justify-content: center; }

        .incident-report {
            width: 90%; max-width: 750px; border: 1px solid var(--danger);
            background: rgba(248, 81, 73, 0.02); padding: 40px; position: relative;
        }

        .header { border-bottom: 1px solid var(--danger); padding-bottom: 20px; margin-bottom: 30px; }
        .header h1 { color: var(--danger); margin: 0; font-size: 1.2rem; letter-spacing: 2px; }
        .header p { font-size: 0.8rem; margin: 10px 0 0; color: var(--text-muted); }

        .terminal-log { font-size: 0.85rem; line-height: 1.8; margin-bottom: 40px; }
        .log-entry { margin-bottom: 10px; display: none; }
        .label { color: var(--danger); font-weight: bold; margin-right: 10px; }

        /* Şaka Kısmı (Başta gizli) */
        #joke-reveal {
            display: none; border-top: 1px solid #30363d; padding-top: 30px;
            animation: fadeIn 1s ease-in;
        }
        .reveal-title { color: #58a6ff; font-weight: bold; margin-bottom: 10px; }
        .reveal-text { color: var(--text-muted); font-size: 0.9rem; }

        .back-btn {
            display: inline-block; margin-top: 25px; padding: 10px 25px;
            border: 1px solid #58a6ff; color: #58a6ff; text-decoration: none;
            font-size: 0.8rem; transition: 0.2s;
        }
        .back-btn:hover { background: #58a6ff; color: #0b0e14; }

        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    </style>
</head>
<body>

<div class="incident-report">
    <div class="header">
        <h1>[INCIDENT_REPORT_#4902] SECURITY VIOLATION</h1>
        <p>Unauthorized Directory Traversal Attempt Detected on /admin/</p>
    </div>

    <div class="terminal-log" id="log-container">
        </div>

    <div id="joke-reveal">
        <div class="reveal-title">GOTTEN! // HONEYPOT_SUCCESSFUL</div>
        <p class="reveal-text">
            Sakin ol, bu sadece bir şaka ve siber güvenlik çalışmalarımdaki <strong>Honeypot</strong> konseptinin bir gösterimidir. <br><br>
            Bir SOC analisti adayı olarak, bu sayfayı meraklı ziyaretçilerin ve botların keşif (reconnaissance) alışkanlıklarını analiz etmek için kurdum. IP adresin gerçekten izlenmedi, ancak bu deneyim siber güvenlikteki "aldatma tekniklerinin" (deception technology) ne kadar etkili olduğunu gösteriyor.
        </p>
        <a href="/" class="back-btn">RETURN TO ROOT_DIRECTORY</a>
    </div>
</div>

<script>
    const logContainer = document.getElementById('log-container');
    const jokeSection = document.getElementById('joke-reveal');

    // Ciddi durması için tasarlanmış teknik loglar
    const forensicLogs = [
        { label: "EVENT", msg: "UNAUTHORIZED_ACCESS_ATTEMPT_AT_ADMIN_PANEL" },
        { label: "IP_STATE", msg: "ORIGIN_SEGMENT_IDENTIFIED" },
        { label: "ACTION", msg: "INITIATING_FORENSIC_SNAPSHOT..." },
        { label: "TRACE", msg: "UPSTREAM_GATEWAY_NOTIFIED" },
        { label: "STATUS", msg: "METADATA_CORRELATION_COMPLETE" },
        { label: "ALERT", msg: "NOTIFYING_SYSTEM_ADMINISTRATOR: EMIRHAN_G" }
    ];

    let i = 0;
    function printLog() {
        if (i < forensicLogs.length) {
            const div = document.createElement('div');
            div.className = 'log-entry';
            div.style.display = 'block';
            div.innerHTML = `<span class="label">> [${forensicLogs[i].label}]</span> ${forensicLogs[i].msg}`;
            logContainer.appendChild(div);
            i++;
            setTimeout(printLog, 800); // Gerçekçi bir hızda akar
        } else {
            // Loglar bitince şakayı açıkla
            setTimeout(() => {
                jokeSection.style.display = 'block';
            }, 1500);
        }
    }

    window.onload = printLog;
</script>

</body>
</html>
