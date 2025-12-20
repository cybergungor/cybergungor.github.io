---
layout: default
title: SOAR - Interactive Investigation
permalink: /alerts/
---

<style>
    main { max-width: 98% !important; margin: 0 auto !important; }
    .soar-container { padding: 20px; font-family: 'Inter', sans-serif; }
    .dashboard-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 30px; border-bottom: 1px solid var(--border-color); padding-bottom: 20px; }
    .score-card { background: var(--card-bg); border: 1px solid var(--border-color); padding: 15px 25px; border-radius: 8px; text-align: center; }
    .score-number { font-size: 2rem; font-weight: bold; color: var(--success); display: block; }
    
    .soar-table { width: 100%; border-collapse: collapse; background: var(--card-bg); border-radius: 8px; overflow: hidden; }
    .soar-table th { background: #1c2128; padding: 15px; text-align: left; color: var(--text-muted); font-size: 0.8rem; text-transform: uppercase; }
    .soar-table td { padding: 15px; border-bottom: 1px solid var(--border-color); }
    .status-badge { padding: 4px 10px; border-radius: 20px; font-size: 0.75rem; font-weight: bold; }
    .status-new { background: rgba(255, 95, 86, 0.2); color: #ff5f56; }
    .status-solved { background: rgba(63, 185, 80, 0.2); color: #3fb950; border: 1px solid #3fb950; }

    /* Modal Stilleri */
    .modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.85); display: none; justify-content: center; align-items: center; z-index: 9999; backdrop-filter: blur(5px); }
    .modal-content { background: #0d1117; border: 1px solid var(--accent); width: 90%; max-width: 600px; border-radius: 12px; padding: 30px; position: relative; }
    .modal-header h2 { margin-top: 0; color: var(--accent); font-family: 'JetBrains Mono', monospace; }
    .close-btn { position: absolute; top: 20px; right: 20px; color: var(--text-muted); cursor: pointer; font-size: 1.5rem; }
    
    .instruction { background: rgba(88, 166, 255, 0.1); border-left: 4px solid var(--accent); padding: 15px; margin: 20px 0; font-size: 0.9rem; }
    .question-box { margin-bottom: 20px; }
    .question-box label { display: block; margin-bottom: 8px; font-weight: bold; font-size: 0.85rem; color: var(--text-main); }
    .ans-input { width: 100%; background: #161b22; border: 1px solid #30363d; color: white; padding: 12px; border-radius: 6px; font-family: 'JetBrains Mono', monospace; }
    .ans-input:focus { border-color: var(--accent); outline: none; }
    
    .btn-submit { width: 100%; padding: 12px; background: var(--accent); color: white; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; transition: 0.3s; }
    .btn-submit:hover { opacity: 0.8; transform: translateY(-2px); }
    
    .success-msg { color: var(--success); text-align: center; margin-top: 15px; display: none; font-weight: bold; }
    .error-msg { color: #ff5f56; text-align: center; margin-top: 15px; display: none; font-size: 0.85rem; }

    .case-resolved-row { opacity: 0.6; background: rgba(63, 185, 80, 0.05) !important; }
</style>

<div class="soar-container">
    <div class="dashboard-header">
        <div>
            <h1>Security Operations Center <span class="status-badge status-new" style="margin-left: 10px;">LIVE RESPONSE</span></h1>
            <p style="color: var(--text-muted);">Mevcut alarmları analiz edin ve Playbook adımlarını tamamlayın.</p>
        </div>
        <div class="score-card">
            <span class="score-number" id="globalScore">0 / 3</span>
            <span style="font-size: 0.7rem; color: var(--text-muted);">VAKA ÇÖZÜLDÜ</span>
        </div>
    </div>

    <table class="soar-table">
        <thead>
            <tr>
                <th>Vaka ID</th>
                <th>Olay Adı</th>
                <th>Kritiklik</th>
                <th>Durum</th>
                <th>İnceleme</th>
            </tr>
        </thead>
        <tbody id="caseTableBody">
            <tr id="row-1">
                <td class="mono">#SOC-5092</td>
                <td><strong>Ransomware Execution Attempt</strong></td>
                <td><span class="badge critical">CRITICAL</span></td>
                <td><span id="stat-1" class="status-badge status-new">AÇIK</span></td>
                <td><button class="btn-view" onclick="openPlaybook(1)">PLAYBOOK'U AÇ</button></td>
            </tr>
            <tr id="row-2">
                <td class="mono">#SOC-1022</td>
                <td><strong>Brute Force Attack - Domain Admin</strong></td>
                <td><span class="badge high">HIGH</span></td>
                <td><span id="stat-2" class="status-badge status-new">AÇIK</span></td>
                <td><button class="btn-view" onclick="openPlaybook(2)">PLAYBOOK'U AÇ</button></td>
            </tr>
            <tr id="row-3">
                <td class="mono">#SOC-883</td>
                <td><strong>SQL Injection Detected (Web)</strong></td>
                <td><span class="badge high">HIGH</span></td>
                <td><span id="stat-3" class="status-badge status-new">AÇIK</span></td>
                <td><button class="btn-view" onclick="openPlaybook(3)">PLAYBOOK'U AÇ</button></td>
            </tr>
        </tbody>
    </table>
</div>

<div id="playbookModal" class="modal">
    <div class="modal-content">
        <span class="close-btn" onclick="closeModal()">&times;</span>
        <div class="modal-header">
            <h2 id="modalTitle">VAKA ANALİZİ</h2>
        </div>
        
        <div class="instruction">
            <strong>Nasıl Çözülür?</strong><br>
            <a href="/logs/" target="_blank" style="color: var(--accent); font-weight: bold;">Log Search</a> sayfasını açın. Bu vakayla ilgili (ID, Host veya Olay Adı) araması yapın. Logların içindeki gizli bilgileri bulup buraya yazın.
        </div>

        <div id="questionsArea"></div>
        
        <div id="errorMsg" class="error-msg">❌ Yanıtlar hatalı! Lütfen logları daha dikkatli inceleyin.</div>
        <div id="successMsg" class="success-msg">✔️ Başarılı! Tehdit bertaraf edildi ve vaka kapatıldı.</div>
        
        <div style="margin-top: 20px;">
            <button class="btn-submit" id="submitBtn" onclick="validateAnswers()">ANALİZİ TAMAMLA</button>
        </div>
    </div>
</div>

<script>
    const cases = {
        1: {
            title: "Ransomware Analizi (#5092)",
            questions: [
                { label: "Saldırıda kullanılan zararlı Process (İşlem) adı nedir?", ans: "tasksche.exe" },
                { label: "Saldırganın şifrelemeye çalıştığı hedef IP adresi nedir?", ans: "10.20.5.100" }
            ]
        },
        2: {
            title: "Brute Force Analizi (#1022)",
            questions: [
                { label: "Saldırıya uğrayan kullanıcı adı (AccountName) nedir?", ans: "adm_emir" },
                { label: "Saldırganın kaynak (Source) IP adresi nedir?", ans: "192.168.1.150" }
            ]
        },
        3: {
            title: "SQL Injection Analizi (#883)",
            questions: [
                { label: "Saldırganın kullandığı aracın (User-Agent) adı nedir?", ans: "sqlmap/1.4.7" },
                { label: "Saldırıya maruz kalan PHP dosyasının adı nedir?", ans: "products.php" }
            ]
        }
    };

    let activeCaseId = null;
    let solvedCount = 0;

    function openPlaybook(id) {
        activeCaseId = id;
        const c = cases[id];
        document.getElementById('modalTitle').innerText = c.title;
        const qArea = document.getElementById('questionsArea');
        
        qArea.innerHTML = c.questions.map((q, i) => `
            <div class="question-box">
                <label>${q.label}</label>
                <input type="text" class="ans-input" id="ans-${i}" placeholder="Logdan bulduğunuz cevabı yazın...">
            </div>
        `).join('');

        document.getElementById('errorMsg').style.display = 'none';
        document.getElementById('successMsg').style.display = 'none';
        document.getElementById('submitBtn').style.display = 'block';
        document.getElementById('playbookModal').style.display = 'flex';
    }

    function validateAnswers() {
        const c = cases[activeCaseId];
        let allCorrect = true;

        c.questions.forEach((q, i) => {
            const userVal = document.getElementById(`ans-${i}`).value.trim().toLowerCase();
            if (userVal !== q.ans.toLowerCase()) {
                allCorrect = false;
                document.getElementById(`ans-${i}`).style.borderColor = "#ff5f56";
            } else {
                document.getElementById(`ans-${i}`).style.borderColor = "#3fb950";
            }
        });

        if (allCorrect) {
            document.getElementById('successMsg').style.display = 'block';
            document.getElementById('errorMsg').style.display = 'none';
            document.getElementById('submitBtn').style.display = 'none';
            
            // Satırı Güncelle
            setTimeout(() => {
                document.getElementById(`row-${activeCaseId}`).classList.add('case-resolved-row');
                document.getElementById(`stat-${activeCaseId}`).innerText = "ÇÖZÜLDÜ";
                document.getElementById(`stat-${activeCaseId}`).className = "status-badge status-solved";
                if(!cases[activeCaseId].alreadySolved) {
                    solvedCount++;
                    cases[activeCaseId].alreadySolved = true;
                }
                document.getElementById('globalScore').innerText = `${solvedCount} / 3`;
                closeModal();
            }, 1500);
        } else {
            document.getElementById('errorMsg').style.display = 'block';
        }
    }

    function closeModal() {
        document.getElementById('playbookModal').style.display = 'none';
    }
</script>
