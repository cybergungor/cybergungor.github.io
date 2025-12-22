---
layout: post
title: "Technical Log: The Great Portfolio Rebuild v2.0"
date: 2025-12-23 00:00:00 +0300
categories: [blog]
tags: [website, blue-team, updates, rebuild]
---

<div class="lang-switch-box" style="margin-bottom: 50px; text-align: center;">
    <button onclick="switchLang('tr')" id="btn-tr" style="background: none; border: 1px solid #00f2ff; color: #00f2ff; padding: 10px 20px; cursor: pointer; font-family: 'JetBrains Mono';">Turkish</button>
    <button onclick="switchLang('en')" id="btn-en" style="background: none; border: 1px solid #00f2ff; color: #00f2ff; padding: 10px 20px; cursor: pointer; font-family: 'JetBrains Mono'; opacity: 0.5;">English</button>
</div>

<div id="content-tr">

# [LOG] Portfolyo v2.0 Modernizasyon Raporu

Sitemi sıradan bir blogdan, profesyonel bir <code>Blue Team</code> operasyon merkezine dönüştürdüğüm bir haftalık redesign sürecini tamamladım. 

Yapılan tüm güncellemeler aşağıda adım adım listelenmiştir:

<br>

### 1- Giriş Ekranı (The Gateway)

Ana sayfa, bir siber güvenlik portalına yakışacak şekilde tamamen derinlik kazandı.

Arka plana siber uzayı simgeleyen hareketli bir <code>3D Izgara (Grid)</code> sistemi entegre edildi.

Tüm içerik, ekranın dikey ve yatay ekseninde tam merkezde duracak şekilde <code>Absolute Centering</code> ile sabitlendi.

Logonun üzerine kimlik doğrulama hissi veren dinamik bir <code>Lazer Tarama</code> çizgisi eklendi.

<br>
<hr style="opacity: 0.1">
<br>

### 2- Tactical HUD (Navigasyon Paneli)

Üst bar (Navbar), sadece bir menü değil, aktif bir kontrol paneli haline getirildi.

Arka plana <code>Glassmorphism</code> uygulanarak şeffaf ve bulanık bir cam efekti verildi.

Navbar'ın merkezine <code>VPN: ENCRYPTED</code> ve <code>THREAT: LOW</code> gibi canlı sistem verileri eklendi.

Kritik butonlara, sistemin aktif olduğunu simgeleyen yanıp sönen <code>Pulse LED</code> ışıkları yerleştirildi.

<br>
<hr style="opacity: 0.1">
<br>

### 3- Kimlik ve Tipografi (About Page)

Hakkımda sayfası, siber güvenlik operatörü kimliğini ön plana çıkaracak şekilde güncellendi.

İsim başlığı, sönük bir prefix ve parlayan neon bir imza olan <code>Cyber Signature</code> tasarımıyla değiştirildi.

Sitenin tamamında, teknik raporlarda standart olan <code>JetBrains Mono</code> font ailesi optimize edildi.

<br>
<hr style="opacity: 0.1">
<br>

### 4- Teknik Altyapı ve Varlıklar (Assets)

Görünmeyen ama profesyonellik için kritik olan tüm teknik detaylar onarıldı.

Hatalı olan tarayıcı sekme ikonu <code>Favicon</code> ve sosyal medya paylaşım görselleri düzeltildi.

<code>GitHub</code> ve <code>LinkedIn</code> bağlantıları, neon parlamalı ikonlarıyla beraber sayfanın en altına sabitlendi.

Sitenin çalışma durumunu gösteren <code>SYSTEM: OPERATIONAL</code> barı tekrar aktif hale getirildi.

<br>

---

</div>

<div id="content-en" style="display: none;">

# [LOG] Portfolio v2.0 Modernization Report

I have completed the one-week redesign process where I transformed my site from an ordinary blog into a professional <code>Blue Team</code> operations center.

All updates are listed step by step below:

<br>

### 1- The Gateway (Landing Page)

The home page has gained full depth, fitting for a cybersecurity portal.

A moving <code>3D Grid</code> system representing cyberspace was integrated into the background.

All content was fixed with <code>Absolute Centering</code> to stand exactly in the center of the screen on both axes.

A dynamic <code>Laser Scanning</code> line was added over the logo to give a sense of identity verification.

<br>
<hr style="opacity: 0.1">
<br>

### 2- Tactical HUD (Navigation Panel)

The top bar (Navbar) has been transformed from just a menu into an active control panel.

A <code>Glassmorphism</code> effect was applied to the background for a transparent and blurred glass look.

Live system data such as <code>VPN: ENCRYPTED</code> and <code>THREAT: LOW</code> were added to the center of the navbar.

Blinking <code>Pulse LED</code> lights were placed on critical buttons to symbolize that the system is active.

<br>
<hr style="opacity: 0.1">
<br>

### 3- Identity and Typography (About Page)

The About page has been updated to highlight the cybersecurity operator identity.

The name header was replaced with the <code>Cyber Signature</code> design, featuring a faint prefix and a glowing neon signature.

The <code>JetBrains Mono</code> font family, standard in technical reports, was optimized across the entire site.

<br>
<hr style="opacity: 0.1">
<br>

### 4- Technical Infrastructure and Assets

All technical details, invisible but critical for professionalism, were repaired.

The faulty browser tab icon <code>Favicon</code> and social media sharing images were fixed.

<code>GitHub</code> and <code>LinkedIn</code> links were pinned to the bottom of the page with their neon-glowing icons.

The <code>SYSTEM: OPERATIONAL</code> bar, indicating the site's operating status, was reactivated.

<br>

---

</div>

<script>
function switchLang(lang) {
    const tr = document.getElementById('content-tr');
    const en = document.getElementById('content-en');
    const btnTr = document.getElementById('btn-tr');
    const btnEn = document.getElementById('btn-en');
    if (lang === 'tr') {
        tr.style.display = 'block'; en.style.display = 'none';
        btnTr.style.opacity = '1'; btnEn.style.opacity = '0.5';
    } else {
        tr.style.display = 'none'; en.style.display = 'block';
        btnTr.style.opacity = '0.5'; btnEn.style.opacity = '1';
    }
}
</script>
