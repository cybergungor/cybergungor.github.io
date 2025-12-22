---
layout: null
permalink: /admin/
---
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Portal Login | Secure Gateway</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #050608; /* Daha derin siyah */
            --surface: #0d1117; /* Panel rengi */
            --border: #30363d;
            --accent: #58a6ff;
            --text: #c9d1d9;
            --error: #f85149;
            --alert-glow: rgba(248, 81, 73, 0.3);
        }

        body, html { margin: 0; padding: 0; height: 100%; background: var(--bg); font-family: 'Inter', sans-serif; color: var(--text); display: flex; align-items: center; justify-content: center; overflow: hidden; }

        /* --- ORTAK PANEL STİLLERİ --- */
        .panel-container {
            width: 100%; max-width: 450px; background: var(--surface);
            border: 1px solid var(--border); padding: 40px; border-radius: 8px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.6);
            position: relative; overflow: hidden;
        }

        /* --- 1. GİRİŞ PANELİ --- */
        #login-panel .brand { font-family: 'JetBrains Mono'; color: var(--accent); font-size: 0.8rem; margin-bottom: 20px; display: block; letter-spacing: 1px; }
        #login-panel h1 { font-size: 1.4rem; color: #ffffff; margin-bottom: 10px; font-weight: 600; }
        #login-panel p.sub { font-size: 0.85rem; color: #8b949e; margin-bottom: 30px; }

        .input-group { margin-bottom: 20px; }
        label { display: block; font-size: 0.75rem; margin-bottom: 8px; color: #8b949e; font-weight: 600; text-transform: uppercase; font-family: 'JetBrains Mono'; }
        input {
            width: 100%; padding: 12px; background: #010409; border: 1px solid var(--border);
            color: #fff; border-radius: 4px; outline: none; box-sizing: border-box; font-family: 'JetBrains Mono'; transition: 0.3s;
        }
        input:focus { border-color: var(--accent); box-shadow: 0 0 10px rgba(88, 166, 255, 0.2); }

        .btn-login {
            width: 100%; padding: 12px; background: var(--accent); border: none;
            color: #050608; font-weight: 700; border-radius: 4px; cursor: pointer;
            font-size: 0.9rem; transition: 0.2s; font-family: 'JetBrains Mono';
        }
        .btn-login:hover { filter: brightness(1.1); box-shadow: 0 0 15px rgba(88, 166, 255, 0.4); }

        /* --- 2. PROFESYONEL HONEYPOT RAPORU --- */
        #analysis-panel { 
            display: none; max-width: 800px; 
            /* "Dandik" kenarlık yerine profesyonel glow efekti */
            border: 1px solid var(--error);
            box-shadow: 0 0 30px var(--alert-glow), inset 0 0 20px var(--alert-glow);
            background-image: radial-gradient(circle at top right, rgba(248, 81, 73, 0.1) 0%, transparent 60%),
                              linear-gradient(to bottom, #0d1117, #050608);
        }
        
        /* Arka plan ızgarası (Grid) */
        #analysis-panel::before {
            content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background-image: linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
                              linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);
            background-size: 20px 20px; pointer-events: none; opacity: 0.5;
        }

        .report-header { border-bottom: 1px solid rgba(248, 81, 73, 0.3); padding-bottom: 20px; margin-bottom: 30px; position: relative; z-index: 2; }
        .report-header h2 { color: var(--error); font-family: 'JetBrains Mono'; font-size: 1.1rem; margin: 0; letter-spacing: 1px; text-shadow: 0 0 10px var(--alert-glow); }
        
        .log-area { background: #010409; padding: 20px; border-radius: 4px; font-family: 'JetBrains Mono'; font-size: 12px; line-height: 1.6; margin-bottom: 30px; border: 1px solid var(--border); position: relative; z-index: 2; }
        .log-line { color: #8b949e; margin-bottom: 5px; }
        .log-line span.time { color: #58a6ff; margin-right: 10px; }
        .log-line span.alert { color: var(--error); font-weight: bold; }

        .deception-info { position: relative; z-index: 2; }
        .deception-info h3 { font-family: 'JetBrains Mono'; font-size: 1rem; color: var(--accent); margin-bottom: 15px; }
        .deception-info p { font-size: 0.9rem; color: #c9d1d9; line-height: 1.7; }

        .back-btn {
            display: inline-block; margin-top: 30px; padding: 10px 25px;
            border: 1px solid var(--accent); color: var(--accent);
            text-decoration: none; font-size: 0.8rem; font-family: 'JetBrains Mono';
            transition: 0.3s; background: rgba(88, 166, 255, 0.05);
        }
        .back-btn:hover { background: var(--accent); color: #050608; box-shadow: 0 0 20px rgba(88, 166, 255, 0.4); }
    </style>
</head>
<body>

    <div id="login-panel" class="panel-container">
        <span class="brand">SECURE GATEWAY // v3.1.2</span>
        <h1>Portal Login</h1>
        <p class="sub">Enter your credentials to access the management console.</p>
        
        <div class="input-group">
            <label>Username / ID</label>
            <input type="text" id="user-input" autocomplete="off">
        </div>
        <div class="input-group">
            <label>Password</label>
            <input type="password" id="pass-input" autocomplete="off">
        </div>
        
        <button class="btn-login" onclick="startAnalysis()">AUTHENTICATE</button>
    </div>

    <div id="analysis-panel" class="panel-container">
        <div class="report-header">
            <h2>[INCIDENT_REPORT] SECURITY_PROTOCOL_VIOLATION</h2>
        </div>

        <div class="log-area" id="log-content">
            </div>

        <div class="deception-info">
            <h3>[ANALYSIS] ALDATMA TEKNOLOJİSİ (HONEYPOT)</h3>
            <p>
                Bu portal, kurumsal siber güvenlik altyapılarında uygulanan <strong>"Active Defense"</strong> (Aktif Savunma) stratejilerinin bir parçasıdır. 
                Yapılan giriş denemesi, sistem tarafından yetkisiz erişim/keşif faaliyeti olarak sınıflandırılmış ve bu güvenli <strong>Honeypot</strong> segmentine yönlendirilmiştir.
                <br><br>
                Bu çalışma, siber saldırganların davranışlarını analiz etmek amacıyla tasarlanmış profesyonel bir siber güvenlik laboratuvarı deneyidir.
            </p>
            <a href="/" class="back-btn">RETURN TO ROOT_SERVER</a>
        </div>
    </div>

    <script>
        function startAnalysis() {
            const login = document.getElementById('login-panel');
            const analysis = document.getElementById('analysis-panel');
            const logContent = document.getElementById('log-content');
            const user = document.getElementById('user-input').value || "ANONYMOUS_USER";

            login.style.display = 'none';
            analysis.style.display = 'block';

            const logs = [
                `> [INFO] Initiating secure handshake for user: ${user}`,
                `> [WARN] Credential validation failed. Identity unverified.`,
                `> [SYSTEM] Invoking surveillance protocols (Layer 7 Analysis).`,
                `> [SOC] <span class="alert">THREAT DETECTED.</span> Rerouting session to isolated Honeypot segment...`,
                `> [STATUS] Session quarantined. Incident logged with ID #9942.`
            ];

            let i = 0;
            function typeLog() {
                if (i < logs.length) {
                    const line = document.createElement('div');
                    line.className = 'log-line';
                    line.innerHTML = `<span class="time">[${new Date().toLocaleTimeString()}]</span> ${logs[i]}`;
                    logContent.appendChild(line);
                    i++;
                    setTimeout(typeLog, 800);
                }
            }
            typeLog();
        }
    </script>

</body>
</html>
