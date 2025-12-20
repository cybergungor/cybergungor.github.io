---
layout: default
title: SOAR - Interactive Incident Response
permalink: /alerts/
---

<style>
    main { max-width: 98% !important; margin: 0 auto !important; }
    .soar-container { padding: 20px; font-family: 'Inter', sans-serif; }
    
    /* Dashboard Header */
    .dashboard-header { 
        display: flex; 
        justify-content: space-between; 
        align-items: center; 
        margin-bottom: 30px; 
        border-bottom: 1px solid var(--border-color); 
        padding-bottom: 20px; 
    }
    .score-card { background: var(--card-bg); border: 1px solid var(--border-color); padding: 15px 25px; border-radius: 8px; text-align: center; }
    .score-number { font-size: 2rem; font-weight: bold; color: var(--success); display: block; font-family: 'JetBrains Mono', monospace; }
    
    /* Table Styling */
    .soar-table { width: 100%; border-collapse: collapse; background: var(--card-bg); border-radius: 8px; overflow: hidden; box-shadow: 0 4px 15px rgba(0,0,0,0.3); }
    .soar-table th { background: #1c2128; padding: 15px; text-align: left; color: var(--text-muted); font-size: 0.75rem; text-transform: uppercase; letter-spacing: 1px; }
    .soar-table td { padding: 15px; border-bottom: 1px solid var(--border-color); font-size: 0.9rem; }
    
    /* Badges */
    .status-badge { padding: 4px 10px; border-radius: 20px; font-size: 0.7rem; font-weight: bold; text-transform: uppercase; }
    .status-open { background: rgba(255, 95, 86, 0.1); color: #ff5f56; border: 1px solid rgba(255, 95, 86, 0.3); }
    .status-solved { background: rgba(63, 185, 80, 0.1); color: #3fb950; border: 1px solid #3fb950; }
    .mitre-tag { font-family: 'JetBrains Mono', monospace; font-size: 0.7rem; color: var(--accent); background: rgba(88, 166, 255, 0.1); padding: 2px 6px; border-radius: 4px; }

    /* Modal Styling */
    .modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); display: none; justify-content: center; align-items: center; z-index: 9999; backdrop-filter: blur(8px); }
    .modal-content { background: #0d1117; border: 1px solid var(--accent); width: 95%; max-width: 650px; border-radius: 12px; padding: 35px; position: relative; box-shadow: 0 0 40px rgba(88, 166, 255, 0.2); }
    .modal-header h2 { margin-top: 0; color: var(--accent); font-family: 'JetBrains Mono', monospace; font-size: 1.2rem; }
    .close-btn { position: absolute; top: 20px; right: 20px; color: var(--text-muted); cursor: pointer; font-size: 1.8rem; transition: 0.3s; }
    .close-btn:hover { color: #fff; }
    
    .playbook-instruction { background: rgba(88, 166, 255, 0.05); border-left: 4px solid var(--accent); padding: 15px; margin: 20px 0; font-size: 0.85rem; color: var(--text-main); line-height: 1.6; }
    .question-box { margin-bottom: 25px; }
    .question-box label { display: block; margin-bottom: 10px; font-weight: 600; font-size: 0.8rem; color: var(--text-muted); text-transform: uppercase; }
    .ans-input { width: 100%; background: #161b22; border: 1px solid #30363d; color: white; padding: 12px; border-radius: 6px; font-family: 'JetBrains Mono', monospace; transition: 0.3s; }
    .ans-input:focus { border-color: var(--accent); outline: none; box-shadow: 0 0 10px rgba(88, 166, 255, 0.1); }
    
    .btn-submit { width: 100%; padding: 14px; background: var(--accent); color: white; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; font-family: 'JetBrains Mono', monospace; letter-spacing: 1px; }
    .btn-submit:hover { filter: brightness(1.2); transform: translateY(-2px); }
    
    .msg-feedback { text-align: center; margin-top: 20px; font-weight: bold; display: none; font-size: 0.9rem; padding: 10px; border-radius: 4px; }
    .success { color: var(--success); background: rgba(63, 185, 80, 0.1); border: 1px solid var(--success); }
    .error { color: var(--danger); background: rgba(255, 95, 86, 0.1); border: 1px solid var(--danger); }

    .case-resolved-row { opacity: 0.5; filter: grayscale(0.5); pointer-events: none; }
</style>

<div class="soar-container">
    <div class="dashboard-header">
        <div>
            <h1>Security Operations Center <span class="status-badge status-open" style="margin-left: 10px; animation: pulse 2s infinite;">Operational</span></h1>
            <p style="color: var(--text-muted); font-size: 0.9rem; margin-top: 5px;">Investigate active security threats and execute incident playbooks.</p>
        </div>
        <div class="score-card">
            <span class="score-number" id="globalScore">0 / 3</span>
            <span style="font-size: 0.65rem; color: var(--text-muted); font-weight: bold; letter-spacing: 1px;">CASES RESOLVED</span>
        </div>
    </div>

    <table class="soar-table">
        <thead>
            <tr>
                <th>Case ID</th>
                <th>Incident Taxonomy</th>
                <th>Severity</th>
                <th>Status</th>
                <th>Action</th>
            </tr>
        </thead>
        <tbody id="caseTableBody">
            <tr id="row-1">
                <td class="mono">#SOC-5092</td>
                <td><strong>Host-Based Ransomware Activity</strong> <span class="mitre-tag">T1486</span></td>
                <td><span class="badge critical">CRITICAL</span></td>
                <td><span id="stat-1" class="status-badge status-open">Open</span></td>
                <td><button class="btn-view" onclick="openPlaybook(1)">EXECUTE PLAYBOOK</button></td>
            </tr>
            <tr id="row-2">
                <td class="mono">#SOC-1022</td>
                <td><strong>Domain Admin Brute Force</strong> <span class="mitre-tag">T1110</span></td>
                <td><span class="badge high">HIGH</span></td>
                <td><span id="stat-2" class="status-badge status-open">Open</span></td>
                <td><button class="btn-view" onclick="openPlaybook(2)">EXECUTE PLAYBOOK</button></td>
            </tr>
            <tr id="row-3">
                <td class="mono">#SOC-883</td>
                <td><strong>Blind SQL Injection Attempt</strong> <span class="mitre-tag">T1190</span></td>
                <td><span class="badge high">HIGH</span></td>
                <td><span id="stat-3" class="status-badge status-open">Open</span></td>
                <td><button class="btn-view" onclick="openPlaybook(3)">EXECUTE PLAYBOOK</button></td>
            </tr>
        </tbody>
    </table>
</div>

<div id="playbookModal" class="modal">
    <div class="modal-content">
        <span class="close-btn" onclick="closeModal()">&times;</span>
        <div class="modal-header">
            <h2 id="modalTitle">INCIDENT INVESTIGATION</h2>
        </div>
        
        <div class="playbook-instruction">
            <strong>Investigation Steps:</strong><br>
            Navigate to the <a href="/logs/" target="_blank" style="color: var(--accent); text-decoration: underline;">Log Search</a> dashboard. Filter logs using the Case ID or relevant Hostnames. Identify forensic artifacts to validate the findings.
        </div>

        <div id="questionsArea"></div>
        
        <div id="feedbackMsg" class="msg-feedback"></div>
        
        <div style="margin-top: 25px;">
            <button class="btn-submit" id="submitBtn" onclick="validateAnswers()">COMPLETE ANALYSIS</button>
        </div>
    </div>
</div>

<script>
    // --- INCIDENT SCENARIOS (PROFESSIONAL TELEMETRY) ---
    const cases = {
        1: {
            title: "Playbook: Ransomware Analysis (T1486)",
            questions: [
                { label: "Identify the file extension appended by the encryption process:", ans: ".locky" },
                { label: "Detected File Entropy value (e.g., 7.92):", ans: "7.92" },
                { label: "Target IP Address found in SMB telemetry:", ans: "10.20.5.100" }
            ]
        },
        2: {
            title: "Playbook: Brute Force Investigation (T1110)",
            questions: [
                { label: "NTLM FailureReason code (e.g., 0xc000...):", ans: "0xc000006d" },
                { label: "Identify the targeted Domain Admin account:", ans: "adm_emir" },
                { label: "Windows LogonType detected in Event ID 4625:", ans: "3" }
            ]
        },
        3: {
            title: "Playbook: SQL Injection Drill-Down (T1190)",
            questions: [
                { label: "Extract the User-Agent string used by the attacker:", ans: "sqlmap/1.4.7" },
                { label: "Database schema targeted in the UNION SELECT payload:", ans: "information_schema" },
                { label: "HTTP Response Code returned by the server:", ans: "200" }
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
                <input type="text" class="ans-input" id="ans-${i}" placeholder="Enter artifact value from logs...">
            </div>
        `).join('');

        document.getElementById('feedbackMsg').style.display = 'none';
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

        const feedback = document.getElementById('feedbackMsg');
        if (allCorrect) {
            feedback.innerText = "VERIFIED: Threat mitigated and evidence validated. Closing case...";
            feedback.className = "msg-feedback success";
            feedback.style.display = 'block';
            document.getElementById('submitBtn').style.display = 'none';
            
            setTimeout(() => {
                document.getElementById(`row-${activeCaseId}`).classList.add('case-resolved-row');
                document.getElementById(`stat-${activeCaseId}`).innerText = "Resolved";
                document.getElementById(`stat-${activeCaseId}`).className = "status-badge status-solved";
                if(!cases[activeCaseId].alreadySolved) {
                    solvedCount++;
                    cases[activeCaseId].alreadySolved = true;
                }
                document.getElementById('globalScore').innerText = `${solvedCount} / 3`;
                closeModal();
            }, 2000);
        } else {
            feedback.innerText = "VALIDATION FAILED: Inconsistent forensic data. Re-examine logs.";
            feedback.className = "msg-feedback error";
            feedback.style.display = 'block';
        }
    }

    function closeModal() {
        document.getElementById('playbookModal').style.display = 'none';
    }
</script>
