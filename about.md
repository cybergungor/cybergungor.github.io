---
layout: default
title: About Me
permalink: /about/
---

<div class="about-container">

  <div class="profile-section">
      <div class="profile-card">
          <div class="image-wrapper">
              <img src="/assets/images/profile.jpeg" alt="Emirhan Gungoroglu" class="profile-img">
              <div class="scan-line"></div>
          </div>
          <div class="profile-stats">
              <div class="stat-row">
                  <span class="stat-label">OPERATOR:</span>
                  <span class="stat-value">Emirhan G.</span>
              </div>
              <div class="stat-row">
                  <span class="stat-label">ROLE:</span>
                  <span class="stat-value" style="color: var(--accent)">Blue Team Analyst</span>
              </div>
              <div class="stat-row">
                  <span class="stat-label">STATUS:</span>
                  <span class="stat-value" style="color: var(--success)">ACTIVE</span>
              </div>
          </div>
      </div>
  </div>

  <div class="bio-section">
      <h1>whoami</h1>
      <p class="intro-text">
        Hello, I'm <strong>Emirhan Gungoroglu</strong>. A cybersecurity student passionate about the 
        <strong>Blue Team</strong> side of operations. My journey is fueled by a curiosity to understand 
        how systems work, how they are attacked, and most importantly, <span style="color: var(--accent);">how to defend them.</span>
      </p>

      <p>
        I focus on bridging the gap between theoretical knowledge and real-world application. 
        Currently, I am actively sharpening my skills through <strong>LetsDefend</strong>, 
        where I investigate real-world simulated cyber attacks, analyze logs, and practice 
        <strong>Incident Response</strong> in a realistic SOC environment.
      </p>

      <hr style="border: 0; border-top: 1px solid var(--border-color); margin: 2rem 0;">

      <h3>// Technical Arsenal</h3>
      <p>Tools and technologies I work with everyday:</p>
      
      <div class="badge-container">
        <span class="tech-badge">LetsDefend (SOC)</span>
        <span class="tech-badge">Splunk Enterprise</span>
        <span class="tech-badge">Threat Hunting</span>
        <span class="tech-badge">Linux / Bash</span>
        <span class="tech-badge">Wireshark</span>
        <span class="tech-badge">MITRE ATT&CK</span>
        <span class="tech-badge">EDR Concepts</span>
      </div>

      <br>

      <h3>// Core Focus Areas</h3>
      <div class="focus-grid">
        <div class="focus-item">
          <h4>🛡️ Blue Team Operations</h4>
          <p>Understanding the defensive lifecycle, from monitoring to incident response.</p>
        </div>
        <div class="focus-item">
          <h4>🔍 Detection Engineering</h4>
          <p>Writing queries and rules to detect anomalies in sea of logs.</p>
        </div>
        <div class="focus-item">
          <h4>🚨 Incident Response</h4>
          <p>Handling alerts and investigating true positives using SOC playbooks.</p>
        </div>
      </div>

      <br>

      <h3>// Purpose of This Knowledge Base</h3>
      <div class="alert alert-info">
        <strong>Note:</strong> This website serves as a technical learning journal. 
        I document my hands-on labs, error troubleshooting processes, and security concepts 
        to solidify my understanding and help others in the community.
      </div>

      <h3>// Connect</h3>
      <p>
        I am always open to discussing cybersecurity topics, potential collaborations, or Blue Team roles.
      </p>
      <p>
        <a href="https://linkedin.com/in/emirhangungoroglu" style="color: var(--accent); font-weight: bold;">Connect on LinkedIn &rarr;</a>
      </p>
  </div>

</div>

<style>
  /* ===== GENEL DÜZEN (FLEXBOX) ===== */
  .about-container {
      display: flex;
      gap: 3rem; /* Sol ve sağ arası boşluk */
      align-items: flex-start;
      padding-top: 1rem;
  }

  .bio-section {
      flex: 1; /* Sağ taraf kalan alanı kaplasın */
  }

  /* ===== SOL PROFİL KARTI TASARIMI ===== */
  .profile-section {
      flex: 0 0 280px; /* Sabit genişlik */
  }

  .profile-card {
      background: #161b22;
      border: 1px solid #30363d;
      padding: 10px;
      border-radius: 8px;
      position: sticky; /* Sayfa kayarken sabit kalsın */
      top: 100px;
  }

  .image-wrapper {
      position: relative;
      overflow: hidden;
      border-radius: 4px;
      border: 1px solid var(--accent);
  }

  .profile-img {
      width: 100%;
      height: auto;
      display: block;
      filter: grayscale(100%) contrast(1.2);
      transition: all 0.5s ease;
  }

  .profile-card:hover .profile-img {
      filter: grayscale(0%) contrast(1);
      transform: scale(1.05);
  }

  /* Tarama Çizgisi */
  .scan-line {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 2px;
      background: rgba(88, 166, 255, 0.5);
      box-shadow: 0 0 10px var(--accent);
      animation: scan 3s linear infinite;
      opacity: 0.5;
      pointer-events: none;
  }

  @keyframes scan {
      0% { top: 0%; }
      100% { top: 100%; }
  }

  /* İstatistik Yazıları */
  .profile-stats {
      margin-top: 1rem;
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.85rem;
      padding: 0 5px;
  }

  .stat-row {
      display: flex;
      justify-content: space-between;
      border-bottom: 1px dashed #30363d;
      padding: 6px 0;
  }
  .stat-row:last-child { border-bottom: none; }
  .stat-label { color: var(--text-muted); }
  .stat-value { font-weight: bold; color: #e6edf3; }

  /* ===== MEVCUT STİLLER ===== */
  .intro-text {
    font-size: 1.1rem;
    line-height: 1.8;
    color: #e6edf3;
  }
  
  .focus-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 1.5rem;
    margin-top: 1rem;
  }

  .focus-item {
    background: rgba(22, 27, 34, 0.5);
    border: 1px solid var(--border-color);
    padding: 1.5rem;
    border-radius: 8px;
  }

  .focus-item h4 {
    margin-top: 0;
    font-size: 1rem;
    color: var(--accent);
  }

  .focus-item p {
    font-size: 0.9rem;
    color: var(--text-muted);
    margin-bottom: 0;
  }
  
  .badge-container {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 1rem;
  }
  
  .tech-badge {
      background: rgba(88, 166, 255, 0.1);
      color: var(--accent);
      border: 1px solid rgba(88, 166, 255, 0.3);
      padding: 5px 12px;
      border-radius: 4px;
      font-size: 0.85rem;
      font-family: 'JetBrains Mono', monospace;
  }
  
  .alert-info {
      background: rgba(56, 139, 253, 0.15);
      border-left: 4px solid #388bfd;
      padding: 1rem;
      border-radius: 4px;
      color: #c9d1d9;
  }

  /* Mobil Uyumluluk */
  @media (max-width: 768px) {
      .about-container {
          flex-direction: column;
      }
      .profile-section {
          flex: 0 0 auto;
          width: 100%;
          max-width: 300px;
          margin: 0 auto;
      }
      .profile-card { position: static; }
  }
</style>
