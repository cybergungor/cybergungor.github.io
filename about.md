---
layout: default
title: About Me
permalink: /about/
---

<div class="about-container">
  
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

<style>
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
</style>
