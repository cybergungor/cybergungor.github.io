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
                  <span class="stat-value" style="color: var(--accent)">Cyber Security Analyst</span>
              </div>
              <div class="stat-row">
                  <span class="stat-label">STATUS:</span>
                  <span class="stat-value" style="color: var(--success)">ACTIVE</span>
              </div>
          </div>
      </div>
  </div>

  <div class="bio-section">
      
      <div class="lang-en">
          <h1>whoami</h1>
          <p class="intro-text">
              Hello, I'm <strong>Emirhan Gungoroglu</strong>. I am a cybersecurity student passionate about 
              <strong>Blue Team</strong> operations. My journey is fueled by a curiosity to understand 
              how systems work, how they are attacked, and most importantly, <span style="color: var(--accent);">how to defend them.</span>
          </p>

          <p>
              I focus on bridging the gap between theoretical knowledge and real-world application. 
              Currently, I am sharpening my skills through <strong>LetsDefend</strong>, investigating 
              real-world simulated cyber attacks and practicing <strong>Incident Response</strong> 
              in a realistic SOC environment.
          </p>

          <hr style="border: 0; border-top: 1px solid var(--border-color); margin: 2rem 0;">

          <h3>// Technical Proficiency</h3>
          <p>Core technologies and frameworks integrated into my workflow:</p>
          <div class="badge-container">
              <span class="tech-badge">SIEM (Splunk / ELK)</span>
              <span class="tech-badge">Network Traffic Analysis</span>
              <span class="tech-badge">Log Correlation</span>
              <span class="tech-badge">MITRE ATT&CK Framework</span>
              <span class="tech-badge">Endpoint Detection (EDR)</span>
              <span class="tech-badge">Vulnerability Assessment</span>
          </div>

          <br>

          <h3>// Professional Focus Areas</h3>
          <div class="focus-grid">
              <div class="focus-item">
                  <h4>🛡️ SOC Operations</h4>
                  <p>Monitoring and triaging security alerts using enterprise-grade SIEM solutions.</p>
              </div>
              <div class="focus-item">
                  <h4>🔍 Threat Analysis</h4>
                  <p>Identifying IoCs and mapping attacker behaviors to the MITRE framework.</p>
              </div>
              <div class="focus-item">
                  <h4>🚨 Incident Response</h4>
                  <p>Investigating true positives and executing containment steps via SOC playbooks.</p>
              </div>
          </div>
      </div>

      <div class="lang-tr" style="display: none;">
          <h1>whoami</h1>
          <p class="intro-text">
              Merhaba, ben <strong>Emirhan Güngöroğlu</strong>. <strong>Blue Team</strong> operasyonlarına odaklanmış 
              bir siber güvenlik öğrencisiyim. Yolculuğum; sistemlerin nasıl çalıştığını, nasıl saldırıya 
              uğradığını ve en önemlisi <span style="color: var(--accent);">onları nasıl savunacağımı</span> anlama merakıyla şekilleniyor.
          </p>

          <p>
              Teorik bilgi ile gerçek dünya uygulamaları arasındaki boşluğu doldurmaya odaklanıyorum. 
              Şu anda <strong>LetsDefend</strong> üzerinden gerçekçi bir SOC ortamında siber saldırıları 
              inceliyor ve <strong>Olay Müdahale (IR)</strong> pratikleri yapıyorum.
          </p>

          <hr style="border: 0; border-top: 1px solid var(--border-color); margin: 2rem 0;">

          <h3>// Teknik Yetkinlikler</h3>
          <p>Çalışma süreçlerime entegre ettiğim temel teknolojiler:</p>
          <div class="badge-container">
              <span class="tech-badge">SIEM (Splunk / ELK)</span>
              <span class="tech-badge">Ağ Trafiği Analizi</span>
              <span class="tech-badge">Log Korelasyonu</span>
              <span class="tech-badge">MITRE ATT&CK Framework</span>
              <span class="tech-badge">Uç Birim Tespiti (EDR)</span>
              <span class="tech-badge">Zafiyet Değerlendirmesi</span>
          </div>

          <br>

          <h3>// Profesyonel Odak Noktaları</h3>
          <div class="focus-grid">
              <div class="focus-item">
                  <h4>🛡️ SOC Operasyonları</h4>
                  <p>Kurumsal SIEM çözümleri ile güvenlik alarmlarını izleme ve önceliklendirme.</p>
              </div>
              <div class="focus-item">
                  <h4>🔍 Tehdit Analizi</h4>
                  <p>Saldırı izlerini (IoC) tanımlama ve saldırgan yöntemlerini MITRE ile eşleştirme.</p>
              </div>
              <div class="focus-item">
                  <h4>🚨 Olay Müdahale</h4>
                  <p>Gerçek saldırıları inceleme ve SOC playbookları üzerinden sınırlama adımlarını uygulama.</p>
              </div>
          </div>
      </div>

      <br>
      <h3>// Connect</h3>
      <p><a href="https://linkedin.com/in/emirhangungoroglu" style="color: var(--accent); font-weight: bold;">Connect on LinkedIn &rarr;</a></p>
  </div>

</div>

<style>
  /* SENİN ORİJİNAL TASARIMIN VE STİLLERİN (DOKUNULMADI) */
  .about-container { display: flex; gap: 3rem; align-items: flex-start; padding-top: 1rem; }
  .bio-section { flex: 1; }
  .profile-section { flex: 0 0 280px; }
  .profile-card { background: #161b22; border: 1px solid #30363d; padding: 10px; border-radius: 8px; position: sticky; top: 100px; }
  .image-wrapper { position: relative; overflow: hidden; border-radius: 4px; border: 1px solid var(--accent); }
  .profile-img { width: 100%; height: auto; display: block; filter: grayscale(100%) contrast(1.2); transition: all 0.5s ease; }
  .profile-card:hover .profile-img { filter: grayscale(0%) contrast(1); transform: scale(1.05); }
  .scan-line { position: absolute; top: 0; left: 0; width: 100%; height: 2px; background: rgba(88, 166, 255, 0.5); box-shadow: 0 0 10px var(--accent); animation: scan 3s linear infinite; opacity: 0.5; pointer-events: none; }
  @keyframes scan { 0% { top: 0%; } 100% { top: 100%; } }
  .profile-stats { margin-top: 1rem; font-family: 'JetBrains Mono', monospace; font-size: 0.85rem; padding: 0 5px; }
  .stat-row { display: flex; justify-content: space-between; border-bottom: 1px dashed #30363d; padding: 6px 0; }
  .stat-row:last-child { border-bottom: none; }
  .stat-label { color: var(--text-muted); }
  .stat-value { font-weight: bold; color: #e6edf3; }
  .intro-text { font-size: 1.1rem; line-height: 1.8; color: #e6edf3; }
  .focus-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 1.5rem; margin-top: 1rem; }
  .focus-item { background: rgba(22, 27, 34, 0.5); border: 1px solid var(--border-color); padding: 1.5rem; border-radius: 8px; }
  .focus-item h4 { margin-top: 0; font-size: 1rem; color: var(--accent); }
  .focus-item p { font-size: 0.9rem; color: var(--text-muted); margin-bottom: 0; }
  .badge-container { display: flex; flex-wrap: wrap; gap: 10px; margin-top: 1rem; }
  .tech-badge { background: rgba(88, 166, 255, 0.1); color: var(--accent); border: 1px solid rgba(88, 166, 255, 0.3); padding: 5px 12px; border-radius: 4px; font-size: 0.85rem; font-family: 'JetBrains Mono', monospace; }

  @media (max-width: 768px) {
      .about-container { flex-direction: column; }
      .profile-section { flex: 0 0 auto; width: 100%; max-width: 300px; margin: 0 auto; }
      .profile-card { position: static; }
  }
</style>
