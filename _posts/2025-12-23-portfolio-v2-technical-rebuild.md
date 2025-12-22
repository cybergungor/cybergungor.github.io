---
layout: post
title: "Technical Log: The Great Portfolio Rebuild v2.0"
date: 2025-12-23 00:00:00 +0300
categories: [blog]
tags: [website, blue-team, updates, rebuild]
---

<div class="language-switcher" style="margin-bottom: 40px; text-align: right;">
    <button onclick="switchLang('tr')" id="btn-tr" class="tactical-btn" style="padding: 6px 20px; cursor: pointer; border: 1px solid var(--cyber-cyan);">TÜRKÇE</button>
    <button onclick="switchLang('en')" id="btn-en" class="tactical-btn" style="padding: 6px 20px; cursor: pointer; border: 1px solid var(--cyber-cyan); opacity: 0.5;">ENGLISH</button>
</div>

<div id="content-tr" class="lang-content">

# [LOG] Portfolyo Altyapı ve Arayüz Modernizasyonu

Son bir haftadır, kişisel siber güvenlik portfolyom üzerinde kapsamlı bir "Redesign" süreci yürüttüm. Bir siber güvenlik öğrencisi olarak hedefim, sıradan bir web sitesi yerine bir **Mavi Takım (Blue Team)** operatörünün terminalini andıran, yüksek performanslı bir platform oluşturmaktı.

## 🛠 Neler Değişti? (Haftalık Değişim Günlüğü)

### 1. Giriş Portalı ve Mekansal Derinlik
Ana sayfa (Landing Page), ziyaretçinin ilk karşılaştığı "güvenlik kapısı" olarak yeniden tasarlandı:
* **3D Grid Teknolojisi:** Perspektif açısı ayarlanmış, sonsuza uzanan hareketli bir ızgara sistemiyle siber uzay atmosferi sağlandı.
* **Merkezi Odak (Forced Centering):** İçeriği ekranın tam merkezine kilitleyen "Nuclear Option" CSS entegrasyonuyla tüm cihazlarda mükemmel hizalama sağlandı.
* **Lazer Tarayıcı:** Logonun üzerinden geçen dinamik lazer çizgisiyle "Kimlik Tarama" görsel teması tamamlandı.

### 2. Tactical HUD (Navigasyon Paneli)
Üst bar, sadece linklerden oluşan bir yapıdan çıkıp bir "HUD" (Heads-Up Display) paneline dönüştürüldü:
* **Glassmorphism:** Arka planı bulanıklaştıran şeffaf cam efektiyle modern bir görünüm sağlandı.
* **Sistem Monitörü:** Navbar'ın ortasına `VPN: ENCRYPTED`, `THREAT_LEVEL: LOW` gibi HUD verileri eklenerek siber güvenlik teması güçlendirildi.
* **Durum LED'leri:** `LIVE_ALERTS` ve `LOG_SEARCH` gibi butonlara, sistemin aktif olduğunu simgeleyen yanıp sönen interaktif LED ışıkları eklendi.

### 3. Hakkımda Sayfası ve Kimlik Tasarımı
About sayfası artık bir biyografiden çok bir "Operatör Personel Dosyası" niteliğinde:
* **Neon Cyber İmza:** İsim kısmına özel neon gölgeleme ve alt çizgi efekti eklenerek görsel hiyerarşi artırıldı.
* **Gelişmiş Tipografi:** Tüm site boyunca profesyonel siber güvenlik raporlarında kullanılan monospaced fontlar optimize edildi.

### 4. Varlık Yönetimi ve SEO
* **Asset Restoration:** Favicon (tarayıcı ikonu) ve meta verileri (OG:Image) güncellenerek sitenin sosyal medyadaki profesyonel görünümü düzeltildi.
* **Sosyal Entegrasyon:** GitHub ve LinkedIn bağlantıları, neon hover efektli orijinal ikonlarıyla footer alanına (Operational Status barının yanına) sabitlendi.

---
</div>

<div id="content-en" class="lang-content" style="display: none;">

# [LOG] Portfolio Infrastructure and UI Modernization

For the past week, I have carried out a comprehensive "Redesign" process on my personal cybersecurity portfolio. As a cybersecurity student, my goal was to create a high-performance platform that resembles a **Blue Team** operator's terminal rather than an ordinary website.

## 🛠 What's Changed? (Weekly Change Log)

### 1. Entry Portal and Spatial Depth
The Landing Page was redesigned as the "security gateway" the visitor first encounters:
* **3D Grid Technology:** A moving grid system with an adjusted perspective angle extending to infinity was provided to create a cyberspace atmosphere.
* **Central Focus (Forced Centering):** Perfect alignment was achieved on all devices with the "Nuclear Option" CSS integration that locks the content exactly in the center of the screen.
* **Laser Scanner:** The visual theme of "Identity Scanning" was completed with a dynamic laser line passing over the logo.

### 2. Tactical HUD (Navigation Panel)
The top bar was transformed from a structure consisting only of links into a "HUD" (Heads-Up Display) panel:
* **Glassmorphism:** A modern look was achieved with a transparent glass effect that blurs the background.
* **System Monitor:** HUD data such as `VPN: ENCRYPTED` and `THREAT_LEVEL: LOW` were added to the center of the navbar to strengthen the cybersecurity theme.
* **Status LEDs:** Interactive pulsing LED lights symbolizing the system's activity were added to buttons like `LIVE_ALERTS` and `LOG_SEARCH`.

### 3. About Page and Identity Design
The About page is now more of an "Operator Personnel File" than a biography:
* **Neon Cyber Signature:** Visual hierarchy was increased by adding special neon shading and underline effects to the name section.
* **Advanced Typography:** Monospaced fonts used in professional cybersecurity reports were optimized throughout the site.

### 4. Asset Management and SEO
* **Asset Restoration:** Favicon (browser icon) and metadata (OG:Image) were updated to fix the site's professional appearance on social media.
* **Social Integration:** GitHub and LinkedIn links were pinned to the footer (next to the Operational Status bar) with original icons featuring neon hover effects.

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
