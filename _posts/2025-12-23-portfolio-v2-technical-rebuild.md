---
layout: post
title: "Technical Log: The Great Portfolio Rebuild v2.0"
date: 2025-12-23 00:00:00 +0300
categories: [blog]
tags: [website, blue-team, updates, rebuild]
---
Haklısın Emirhan, önceki taslak görsel olarak çok yoğun ve karışık duruyordu. Bir siber güvenlik portfolyosunda "Log" veya "Changelog" (Değişim Günlüğü) mantığı, bilginin temiz, okunabilir ve profesyonel bir hiyerarşide sunulmasını gerektirir.

Yazıyı tamamen baştan, bir "Sistem Güncelleme Raporu" ciddiyetinde, teknik detayları ezmeden ama profesyonelliği ön plana çıkaracak şekilde düzenledim. Bu sürümde her şey daha geniş, ferah ve "premium" duracak.

📄 2025-12-23-cyberlab-v2-update.md
Markdown

---
layout: post
title: "System Update: CyberLab v2.0 - Tactical HUD & Identity Reconstruction"
date: 2025-12-23 01:00:00 +0300
categories: [Updates, SOC-Analyst]
---

<div class="language-switcher" style="margin-bottom: 50px; text-align: center; border-bottom: 1px solid var(--cyber-border); padding-bottom: 20px;">
    <button onclick="switchLang('tr')" id="btn-tr" class="tactical-btn" style="padding: 8px 25px; cursor: pointer; border: 1px solid var(--cyber-cyan); font-family: 'JetBrains Mono';">TR</button>
    <button onclick="switchLang('en')" id="btn-en" class="tactical-btn" style="padding: 8px 25px; cursor: pointer; border: 1px solid var(--cyber-cyan); opacity: 0.5; font-family: 'JetBrains Mono';">EN</button>
</div>

<div id="content-tr" class="lang-content">

## [REPORT] Operasyonel Revizyon: Versiyon 2.0 Yayında

Bu hafta, dijital kimliğimi temsil eden **CyberLab** projesinde köklü bir altyapı ve arayüz değişimine gittim. Bir siber güvenlik öğrencisi olarak sadece bir web sitesi değil, bir **SOC (Security Operations Center)** operatörünün ihtiyaç duyacağı görsel disiplini yansıtan bir terminal inşa etmeyi hedefledim.

---

### 🛡️ Faz 1: Giriş Katmanı (The Gateway)
Ziyaretçinin ilk karşılaştığı alan, sistemin "güven seviyesini" belirleyen en kritik noktadır.
* **3D Izgara (Grid) Entegrasyonu:** Perspektif derinliğine sahip hareketli zemin ızgarası ile statik tasarımlardan uzaklaşıp dinamik bir siber uzay atmosferi oluşturuldu.
* **Merkezi Odak Teknolojisi:** İçeriği ekranın tam merkezine (X ve Y ekseni) sabitleyen gelişmiş CSS mimarisiyle, cihaz bağımsız mükemmel simetri sağlandı.
* **Lazer Kimlik Taraması:** Logo üzerine eklenen neon lazer çizgisi, sistemin sürekli aktif ve "tarama modunda" olduğunu simgeliyor.

### 📡 Faz 2: Tactical HUD & Navigasyon
Geleneksel menü yapıları terk edilerek, modern bir kontrol paneli (Heads-Up Display) kurgulandı:
* **Glassmorphism:** Navbar arkasındaki bulanıklık (blur) efektiyle arayüze derinlik ve teknolojik bir şıklık katıldı.
* **HUD Monitor Detayları:** Navbar üzerine `VPN`, `THREAT_LEVEL` ve `LOC` gibi canlı veri alanları eklenerek "Operasyonel Durum" farkındalığı artırıldı.
* **Interaktif LED'ler:** `LIVE_ALERTS` ve `LOG_SEARCH` butonları, yanıp sönen aktif LED ışıklarıyla "tıklanabilir" birer sistem modülüne dönüştürüldü.

### 👤 Faz 3: Kimlik (About) & Görsel İmzalar
Kişisel marka değerini artırmak adına "About" sayfası bir CV'den çok bir **"Operatör Dosyası"** haline getirildi:
* **Neon Signature:** İsim kısmına eklenen neon parlamalı büyük imza tasarımı, siber güvenlik dünyasındaki benzersiz kimliği temsil ediyor.
* **Varlık Restorasyonu:** Kaybolan Favicon ve meta veriler (SEO) geri yüklenerek sitemin sosyal medya ve tarayıcı üzerindeki profesyonel görünümü %100 oranında kurtarıldı.

---
</div>

<div id="content-en" class="lang-content" style="display: none;">

## [REPORT] Operational Revision: Version 2.0 is Live

This week, I underwent a fundamental infrastructure and interface change in the **CyberLab** project, which represents my digital identity. As a cybersecurity student, I aimed to build not just a website, but a terminal that reflects the visual discipline a **SOC (Security Operations Center)** operator would need.

---

### 🛡️ Phase 1: The Gateway
The first area a visitor encounters is the most critical point that determines the "confidence level" of the system.
* **3D Grid Integration:** Moving away from static designs, a dynamic cyberspace atmosphere was created with a moving ground grid with perspective depth.
* **Central Focus Technology:** Perfect symmetry independent of the device was achieved with an advanced CSS architecture that fixes the content exactly in the center of the screen (X and Y axes).
* **Laser Identity Scanning:** The neon laser line added over the logo symbolizes that the system is constantly active and in "scanning mode."

### 📡 Phase 2: Tactical HUD & Navigation
Traditional menu structures were abandoned and a modern control panel (Heads-Up Display) was designed:
* **Glassmorphism:** Depth and technological elegance were added to the interface with a blur effect behind the navbar.
* **HUD Monitor Details:** "Operational Status" awareness was increased by adding live data fields such as `VPN`, `THREAT_LEVEL`, and `LOC` to the navbar.
* **Interactive LEDs:** `LIVE_ALERTS` and `LOG_SEARCH` buttons were transformed into clickable system modules with blinking active LED lights.

### 👤 Phase 3: Identity (About) & Visual Signatures
In order to increase personal brand value, the "About" page has been turned into an **"Operator Personnel File"** rather than a biography:
* **Neon Signature:** The large neon-glowing signature design added to the name section represents a unique identity in the cybersecurity world.
* **Asset Restoration:** The missing Favicon and metadata (SEO) were restored, and the professional appearance of my site on social media and browsers was 100% recovered.

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

<style>
.lang-content { animation: fadeIn 1s ease-in-out; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
.tactical-btn:hover { background: var(--cyber-cyan); color: #000; box-shadow: 0 0 20px var(--cyber-cyan); }
</style>
