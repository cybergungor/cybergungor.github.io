---
layout: post
title: "Technical Log: The Great Portfolio Rebuild v2.0"
date: 2025-12-23 00:00:00 +0300
categories: [blog]
tags: [website, blue-team, updates, rebuild]
---

<div class="terminal-switcher" style="margin-bottom: 40px; border: 1px dashed var(--cyber-border); padding: 15px; display: flex; justify-content: center; gap: 20px;">
    <button onclick="switchLang('tr')" id="btn-tr" class="tactical-btn" style="color: var(--success-green);">[ SELECT_LANG: TR ]</button>
    <button onclick="switchLang('en')" id="btn-en" class="tactical-btn" style="color: var(--cyber-cyan); opacity: 0.5;">[ SELECT_LANG: EN ]</button>
</div>

<div id="content-tr" class="lang-content">

## 🟢 [LOG_ENTRY] Sistem Modernizasyonu v2.0

Bu rapor, **CyberGungor** portfolyosunun son bir hafta içerisindeki teknolojik evrimini belgelemektedir. Statik bir blog yapısından, yaşayan bir **Mavi Takım Operasyon Merkezi** arayüzüne geçiş süreci aşağıda detaylandırılmıştır.

---

### 📡 FAZ 1: Giriş ve Derinlik (The Gateway)
Giriş ekranı, sistemin "ilk temas" noktasıdır ve artık çok daha dinamik.

* **3D Izgara Sistemi:** Arka plana, perspektifi ayarlanmış ve sürekli hareket eden bir <span style="color: var(--cyber-cyan); font-weight: bold;">Siber Izgara (Grid)</span> eklendi.
* **Merkezi Hizalama:** İçerik, tüm cihazlarda ekranın tam merkezine <span style="color: #fff; font-weight: bold;">(Absolute Centering)</span> kilitlendi.
* **Lazer Tarayıcı:** Logo üzerine, kimlik doğrulamasını temsil eden <span style="color: var(--cyber-cyan);">dinamik lazer animasyonu</span> entegre edildi.

---

### 🛡️ FAZ 2: Tactical HUD (Navigasyon)
Menü yapısı, bir operatörün ihtiyaç duyacağı **Heads-Up Display (HUD)** konseptine dönüştürüldü.

* **Buzlu Cam (Glassmorphism):** Navbar, arkasındaki akışı gösteren <span style="color: var(--cyber-cyan);">Blur</span> efektiyle modernize edildi.
* **Sistem Monitörleri:** Navigasyonun merkezine gerçek zamanlı veri hissi veren göstergeler eklendi:
    > `STATUS: SECURE // THREAT_LEVEL: LOW // VPN: ACTIVE`
* **LED Durum Işıkları:** `LIVE_ALERTS` ve `LOG_SEARCH` butonlarına, sistemin aktif olduğunu simgeleyen <span style="color: var(--alert-red); font-weight: bold;">yanıp sönen (pulse) LED'ler</span> eklendi.

---

### 👤 FAZ 3: Operatör Kimliği (About Page)
Kişisel sayfa, bir biyografiden çok bir **"Personel Dosyası"** hiyerarşisine kavuşturuldu.

* **Cyber Signature:** İsim başlığı, sönük bir "About /" ön eki ve <span style="color: #fff; text-shadow: 0 0 15px var(--cyber-cyan); font-weight: 900;">Parlayan Neon Bir İmza</span> ile değiştirildi.
* **Karakteristik Tipografi:** Raporların okunabilirliğini artırmak için <span style="color: var(--success-green);">JetBrains Mono</span> font ailesi tüm teknik alanlara yayıldı.

---

### ⚙️ FAZ 4: Varlık Yönetimi (Back-end & SEO)
Görünmeyen kısımlardaki hatalar giderilerek sistem stabilitesi sağlandı.

* **Asset Restoration:** Tarayıcı sekmesindeki <span style="color: var(--cyber-cyan);">Favicon</span> ve sosyal medya paylaşımlarında çıkan kapak görselleri geri yüklendi.
* **Sosyal Entegrasyon:** GitHub ve LinkedIn bağlantıları, neon hover efektli orijinal ikonlarıyla **Footer** alanına (Sistem Durum Çubuğu yanına) sabitlendi.

---
</div>

<div id="content-en" class="lang-content" style="display: none;">

## 🔵 [LOG_ENTRY] System Modernization v2.0

This report documents the technological evolution of the **CyberGungor** portfolio over the last week. The transition process from a static blog to a living **Blue Team Operations Center** interface is detailed below.

---

### 📡 PHASE 1: The Gateway
The landing page is the "initial contact" point and is now much more dynamic.

* **3D Grid System:** A perspective-adjusted, constantly moving <span style="color: var(--cyber-cyan); font-weight: bold;">Cyber Grid</span> was added to the background.
* **Centralized Focus:** Content is locked to the exact center <span style="color: #fff; font-weight: bold;">(Absolute Centering)</span> of the screen on all devices.
* **Laser Scanner:** A <span style="color: var(--cyber-cyan);">dynamic laser animation</span> representing identity verification was integrated over the logo.

---

### 🛡️ PHASE 2: Tactical HUD (Navigation)
The menu structure was transformed into a **Heads-Up Display (HUD)** concept an operator would need.

* **Glassmorphism:** The navbar was modernized with a <span style="color: var(--cyber-cyan);">Blur</span> effect that shows the flow behind it.
* **System Monitors:** Indicators providing a real-time data feel were added to the center of the navigation:
    > `STATUS: SECURE // THREAT_LEVEL: LOW // VPN: ACTIVE`
* **LED Status Lights:** <span style="color: var(--alert-red); font-weight: bold;">Pulsing LEDs</span> symbolizing the system's activity were added to the `LIVE_ALERTS` and `LOG_SEARCH` buttons.

---

### 👤 PHASE 3: Operator Identity (About Page)
The personal page has been given a **"Personnel File"** hierarchy rather than a biography.

* **Cyber Signature:** The name header was replaced with a faint "About /" prefix and a <span style="color: #fff; text-shadow: 0 0 15px var(--cyber-cyan); font-weight: 900;">Glowing Neon Signature</span>.
* **Characteristic Typography:** The <span style="color: var(--success-green);">JetBrains Mono</span> font family was spread across all technical areas to increase the readability of reports.

---

### ⚙️ PHASE 4: Asset Management (Back-end & SEO)
Errors in invisible parts were resolved, ensuring system stability.

* **Asset Restoration:** The <span style="color: var(--cyber-cyan);">Favicon</span> in the browser tab and cover images appearing on social media shares were restored.
* **Social Integration:** GitHub and LinkedIn links were pinned to the **Footer** (next to the System Status Bar) with original icons featuring neon hover effects.

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
.lang-content { animation: fadeIn 0.8s ease-in-out; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
.tactical-btn { background: none; border: none; font-family: 'JetBrains Mono'; font-weight: bold; cursor: pointer; transition: 0.3s; }
.tactical-btn:hover { text-shadow: 0 0 10px currentColor; }
</style>
