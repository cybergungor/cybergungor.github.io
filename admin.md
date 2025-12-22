---
layout: null
permalink: /admin/
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Login | Management Portal</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #0b1016;
            --surface: #161b22;
            --border: #30363d;
            --accent: #58a6ff;
            --text: #d1d5db;
            --error: #f85149;
        }

        body, html { margin: 0; padding: 0; height: 100%; background: var(--bg); font-family: 'Inter', sans-serif; color: var(--text); display: flex; align-items: center; justify-content: center; }

        /* --- 1. GİRİŞ PANELİ (INITIAL STATE) --- */
        #login-panel {
            width: 100%; max-width: 400px; background: var(--surface);
            border: 1px solid var(--border); padding: 40px; border-radius: 6px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.4);
        }

        .brand { font-family: 'JetBrains Mono'; color: var(--accent); font-size: 0.9rem; margin-bottom: 25px; display: block; }
        h1 { font-size: 1.5rem; color: #fff; margin-bottom: 8px; font-weight: 600; }
        p.sub { font-size: 0.85rem; color: #8b949e; margin-bottom: 30px; }

        .input-group { margin-bottom: 20px; }
        label { display: block; font-size: 0.8rem; margin-bottom: 8px; color: #8b949e; font-weight: 500; }
        input {
            width: 100%; padding: 10px; background: #0d1117; border: 1px solid var(--border);
            color: #fff; border-radius: 4px; outline: none; box-sizing: border-box;
        }
        input:focus { border-color: var(--accent); }

        .btn-login {
            width: 100%; padding: 12px; background: var(--accent); border: none;
            color: #0b1016; font-weight: 600; border-radius: 4px; cursor: pointer;
            font-size: 0.9rem; transition: 0.2s;
        }
        .btn-login:hover { filter: brightness(1.1); }

        /* --- 2. ANALİZ EKRANI (REVEAL STATE) --- */
        #analysis-panel { display: none; width: 90%; max-width: 800px; animation: fadeIn 0.6s ease-out; }
        
        .report-box { background: var(--surface); border: 1px solid var(--error); padding: 40px; }
        .report-header { border-bottom: 1px solid var(--border); padding-bottom: 20px; margin-bottom: 30px; }
        .report-header h2 { color: var(--error); font-family: 'JetBrains Mono'; font-size: 1.1rem; margin: 0; }
        
        .log-area { background: #050608; padding: 20px; border-radius: 4px; font-family: 'JetBrains Mono'; font-size: 13px; line-height: 1.6; margin-bottom: 30px; }
        .log-line { color: #8b949e; margin-bottom: 5px; }
        .log-line span { color: var(--accent); }

        .deception-info h3 { font-family: 'JetBrains Mono'; font-size: 1rem; color: var(--accent); margin-bottom: 15px; }
        .deception-info p { font-size: 0.95rem; color: #8b949e; line-height: 1.7; }

        .back-btn {
            display: inline-block; margin-top: 30px; padding: 10px 25px;
            border: 1px solid var(--accent); color: var(--accent);
            text-decoration: none; font-size: 0.85rem; font-family: 'JetBrains Mono';
        }
        .back-btn:hover { background: var(--accent); color: var(--bg); }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <div id="login-panel">
        <span class="brand">SOC_MANAGEMENT // v2.04</span>
        <h1>Management Portal</h1>
        <p class="sub">Please verify your identity to continue.</p>
        
        <div class="input-group">
            <label>Operator ID</label>
            <input type="text" placeholder="e.g. admin_emirhan" id="user-input">
        </div>
        <div class="input-group">
            <label>Security Key</label>
            <input type="password" placeholder="••••••••" id="pass-input">
        </div>
        
        <button class="btn-login" onclick="startAnalysis()">AUTHENTICATE</button>
    </div>

    <div id="analysis-panel">
        <div class="report-box">
            <div class="report-header">
                <h2>[INCIDENT_REPORT] UNAUTHORIZED_ACCESS_INTERCEPTED</h2>
            </div>

            <div class="log-area" id="log-content">
                </div>

            <div class="deception-info">
                <h3>[ANALYSIS] ALDATMA TEKNOLOJİSİ (HONEYPOT)</h3>
                <p>
                    Bu portal, kurumsal siber güvenlik altyapılarında uygulanan <strong>"Active Defense"</strong> (Aktif Savunma) stratejilerinin bir parçasıdır. 
                    Yapılan giriş denemesi, sistem tarafından yetkisiz erişim/keşif faaliyeti olarak sınıflandırılmış ve bu güvenli <strong>Honeypot</strong> segmentine yönlendirilmiştir.
                    <br><br>
                    Bu çalışma, siber saldırganların davranışlarını analiz etmek ve kritik sistemleri korumak amacıyla tasarlanmış profesyonel bir siber güvenlik laboratuvarı deneyidir.
                </p>
                <a href="/" class="back-btn">RETURN TO ROOT_SERVER</a>
            </div>
        </div>
    </div>

    <script>
        function startAnalysis() {
            const login = document.getElementById('login-panel');
            const analysis = document.getElementById('analysis-panel');
            const logContent = document.getElementById('log-content');

            // Kullanıcı verilerini al (Aslında kontrol etmiyoruz, her türlü honeypot'a gider)
            const user = document.getElementById('user-input').value || "anonymous";

            login.style.display = 'none';
            analysis.style.display = 'block';

            const logs = [
                `> [INFO] Initializing authentication for: ${user}`,
                "> [WARN] Identity signature mismatch. Analyzing session...",
                "> [INFO] Metadata correlation complete (Heuristic Engine)",
                "> [SOC] Routing suspected threat to isolated Honeypot segment...",
                "> [STATUS] Connection intercepted and logged. Access denied."
            ];

            let i = 0;
            function typeLog() {
                if (i < logs.length) {
                    const line = document.createElement('div');
                    line.className = 'log-line';
                    line.innerHTML = `<span>[${new Date().toLocaleTimeString()}]</span> ${logs[i]}`;
                    logContent.appendChild(line);
                    i++;
                    setTimeout(typeLog, 700);
                }
            }
            typeLog();
        }
    </script>

</body>
</html>
