---
layout: default
title: Security Operations Center - Incident Response
permalink: /alerts/
---

<style>
    /* Global Page Overrides */
    main { max-width: 100% !important; margin: 0 !important; padding: 20px !important; background: #080a0c; }
    body { background: #080a0c; }

    .soc-dashboard {
        font-family: 'Inter', -apple-system, sans-serif;
        color: #e6edf3;
    }

    /* Header & Stats */
    .soc-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 25px;
        padding: 0 10px;
    }

    .soc-title-group h1 {
        margin: 0;
        font-size: 1.1rem;
        letter-spacing: 1.5px;
        color: #58a6ff;
        font-family: 'JetBrains Mono', monospace;
    }

    .soc-status-indicator {
        font-size: 0.7rem;
        color: #8b949e;
        margin-top: 5px;
        display: flex;
        align-items: center;
        gap: 8px;
    }

    .live-dot {
        width: 8px;
        height: 8px;
        background: #ff5f56;
        border-radius: 50%;
        box-shadow: 0 0 8px #ff5f56;
        animation: pulse-red 2s infinite;
    }

    @keyframes pulse-red {
        0% { opacity: 1; } 50% { opacity: 0.3; } 100% { opacity: 1; }
    }

    .resolved-counter {
        background: #161b22;
        border: 1px solid #30363d;
        padding: 8px 16px;
        border-radius: 4px;
        font-family: 'JetBrains Mono', monospace;
        font-size: 0.8rem;
    }

    .resolved-counter span { color: #3fb950; font-weight: bold; }

    /* Professional Table Design */
    .incident-container {
        background: #0d1117;
        border: 1px solid #30363d;
        border-radius: 6px;
        overflow: hidden;
    }

    .incident-table {
        width: 100%;
        border-collapse: collapse;
        font-size: 0.85rem;
    }

    .incident-table th {
        background: #161b22;
        color: #8b949e;
        text-align: left;
        padding: 12px 15px;
        font-weight: 500;
        border-bottom: 1px solid #30363d;
        text-transform: uppercase;
        font-size: 0.7rem;
        letter-spacing: 1px;
    }

    .incident-table td {
        padding: 14px 15px;
        border-bottom: 1px solid #21262d;
    }

    .incident-table tr:hover {
        background: #121d2f; /* Çok hafif mavi vurgu */
    }

    /* Column Styles */
    .case-id { color: #58a6ff; font-family: 'JetBrains Mono', monospace; font-weight: bold; }
    .taxonomy { font-weight: 500; }
    .mitre-tag {
        color: #8b949e;
        background: rgba(110, 118, 129, 0.1);
        border: 1px solid #30363d;
        padding: 2px 6px;
        border-radius: 3px;
        font-size: 0.7rem;
        margin-left: 8px;
        font-family: 'JetBrains Mono', monospace;
    }

    /* Severity Badges (Flat Design) */
    .sev { padding: 3px 8px; border-radius: 3px; font-size: 0.65rem; font-weight: 700; letter-spacing: 0.5px; }
    .sev-critical { background: rgba(248, 81, 73, 0.15); color: #f85149; border: 1px solid rgba(248, 81, 73, 0.3); }
    .sev-high { background: rgba(210, 153, 34, 0.15); color: #d29922; border: 1px solid rgba(210, 153, 34, 0.3); }

    .status-text { font-family: 'JetBrains Mono', monospace; font-size: 0.75rem; text-transform: uppercase; }
    .status-open { color: #f85149; }
    .status-closed { color: #3fb950; }

    /* Modern Action Button (Subtle) */
    .btn-action {
        background: transparent;
        border: 1px solid #30363d;
        color: #c9d1d9;
        padding: 5px 12px;
        font-size: 0.75rem;
        border-radius: 4px;
        cursor: pointer;
        font-family: 'JetBrains Mono', monospace;
        transition: all 0.2s;
    }

    .btn-action:hover {
        border-color: #58a6ff;
        color: #58a6ff;
        background: rgba(88, 166, 255, 0.05);
    }

    /* Modal (Playbook View) */
    .modal {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background: rgba(1, 4, 9, 0.85); display: none;
        align-items: center; justify-content: center; z-index: 1000;
        backdrop-filter: blur(4px);
    }

    .modal-content {
        background: #0d1117;
        border: 1px solid #30363d;
        width: 100%; max-width: 600px;
        border-radius: 8px;
        padding: 30px;
        box-shadow: 0 20px 50px rgba(0,0,0,0.5);
    }

    .modal-header {
        border-bottom: 1px solid #30363d;
        padding-bottom: 15px;
        margin-bottom: 20px;
    }

    .modal-header h2 { margin: 0; font-size: 1rem; color: #58a6ff; font-family: 'JetBrains Mono', monospace; }

    .playbook-step { margin-bottom: 20px; }
    .playbook-step label { display: block; font-size: 0.75rem; color: #8b949e; margin-bottom: 8px; text-transform: uppercase; }
    .playbook-input {
        width: 100%; background: #010409; border: 1px solid #30363d;
        color: #e6edf3; padding: 10px; font-family: 'JetBrains Mono', monospace;
        font-size: 0.85rem; border-radius: 4px;
    }

    .playbook-input:focus { outline: none; border-color: #58a6ff; }

    .btn-submit {
        width: 100%; background: #238636; color: #fff;
        border: 1px solid rgba(240, 246, 252, 0.1);
        padding: 12px; border-radius: 4px; font-weight: bold;
        cursor: pointer; margin-top: 10px; font-size: 0.85rem;
    }

    .btn-submit:hover { background: #2ea043; }

    .feedback { text-align: center; margin-top: 15px; font-size: 0.8rem; font-weight: bold; }
    .resolved-row { opacity: 0.4; }
</style>

<div class="soc-dashboard">
    <div class="soc-header">
        <div class="soc-title-group">
            <h1>SECURITY OPERATIONS CENTER</h1>
            <div class="soc-status-indicator">
                <div class="live-dot"></div> MONITORING ACTIVE // THREAT FEED: SENTINEL_v4
            </div>
        </div>
        <div class="resolved-counter">CASES RESOLVED: <span id="solvedCount">0 / 3</span></div>
    </div>

    <div class="incident-container">
        <table class="incident-table">
            <thead>
                <tr>
                    <th width="120">Case ID</th>
                    <th>Incident Taxonomy</th>
                    <th width="120">Severity</th>
                    <th width="100">Status</th>
                    <th width="120">Action</th>
                </tr>
            </thead>
            <tbody id="caseTable">
                <tr id="row-1">
                    <td class="case-id">#SOC-5092</td>
                    <td class="taxonomy">Host-Based Ransomware Activity <span class="mitre-tag">T1486</span></td>
                    <td><span class="sev sev-critical">CRITICAL</span></td>
                    <td><span class="status-text status-open" id="stat-1">Open</span></td>
                    <td><button class="btn-action" onclick="openPlaybook(1)">ANALYZE</button></td>
                </tr>
                <tr id="row-2">
                    <td class="case-id">#SOC-1022</td>
                    <td class="taxonomy">Domain Admin Brute Force <span class="mitre-tag">T1110</span></td>
                    <td><span class="sev sev-high">HIGH</span></td>
                    <td><span class="status-text status-open" id="stat-2">Open</span></td>
                    <td><button class="btn-action" onclick="openPlaybook(2)">ANALYZE</button></td>
                </tr>
                <tr id="row-3">
                    <td class="case-id">#SOC-883</td>
                    <td class="taxonomy">Blind SQL Injection Attempt <span class="mitre-tag">T1190</span></td>
                    <td><span class="sev sev-high">HIGH</span></td>
                    <td><span class="status-text status-open" id="stat-3">Open</span></td>
                    <td><button class="btn-action" onclick="openPlaybook(3)">ANALYZE</button></td>
                </tr>
            </tbody>
        </table>
    </div>
</div>

<div id="playbookModal" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h2 id="modalTitle">INCIDENT PLAYBOOK</h2>
        </div>
        
        <div style="font-size: 0.8rem; color: #8b949e; margin-bottom: 20px; line-height: 1.5;">
            Cross-reference forensic artifacts in the <a href="/logs/" target="_blank" style="color: #58a6ff;">Log Search</a> dashboard. Enter the correct data to mitigate the threat.
        </div>

        <div id="questionsArea"></div>
        <div id="feedback" class="feedback"></div>

        <button class="btn-submit" id="submitBtn" onclick="validate()">COMPLETE ANALYSIS</button>
        <button class="btn-action" style="width:100%; margin-top:10px; border:none;" onclick="closeModal()">CANCEL</button>
    </div>
</div>

<script>
    const playbookData = {
        1: {
            title: "Playbook: Ransomware Analysis",
            questions: [
                { label: "Detected File Extension:", ans: ".locky" },
                { label: "Target Host IP:", ans: "10.20.5.100" }
            ]
        },
        2: {
            title: "Playbook: Brute Force Investigation",
            questions: [
                { label: "Target Account Name:", ans: "adm_emir" },
                { label: "Attacker Source IP:", ans: "192.168.1.150" }
            ]
        },
        3: {
            title: "Playbook: Web Attack Analysis",
            questions: [
                { label: "Attacker User-Agent:", ans: "sqlmap/1.4.7" },
                { label: "Targeted Database Schema:", ans: "information_schema" }
            ]
        }
    };

    let activeId = null;
    let solved = 0;

    function openPlaybook(id) {
        activeId = id;
        const data = playbookData[id];
        document.getElementById('modalTitle').innerText = data.title;
        const qArea = document.getElementById('questionsArea');
        qArea.innerHTML = data.questions.map((q, i) => `
            <div class="playbook-step">
                <label>${q.label}</label>
                <input type="text" class="playbook-input" id="ans-${i}" autocomplete="off">
            </div>
        `).join('');
        document.getElementById('feedback').innerText = "";
        document.getElementById('playbookModal').style.display = 'flex';
    }

    function validate() {
        const data = playbookData[activeId];
        let correct = true;
        data.questions.forEach((q, i) => {
            if (document.getElementById(`ans-${i}`).value.trim().toLowerCase() !== q.ans.toLowerCase()) {
                correct = false;
                document.getElementById(`ans-${i}`).style.borderColor = "#f85149";
            } else {
                document.getElementById(`ans-${i}`).style.borderColor = "#238636";
            }
        });

        const fb = document.getElementById('feedback');
        if (correct) {
            fb.style.color = "#3fb950";
            fb.innerText = "ARTIFACTS VERIFIED. CLOSING INCIDENT...";
            setTimeout(() => {
                document.getElementById(`row-${activeId}`).classList.add('resolved-row');
                document.getElementById(`stat-${activeId}`).innerText = "Closed";
                document.getElementById(`stat-${activeId}`).className = "status-text status-closed";
                if(!playbookData[activeId].done) {
                    solved++;
                    playbookData[activeId].done = true;
                    document.getElementById('solvedCount').innerText = `${solved} / 3`;
                }
                closeModal();
            }, 1500);
        } else {
            fb.style.color = "#f85149";
            fb.innerText = "VALIDATION FAILED. INCONSISTENT DATA.";
        }
    }

    function closeModal() { document.getElementById('playbookModal').style.display = 'none'; }
</script>
