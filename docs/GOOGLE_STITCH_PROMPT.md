# Google Stitch / AI Tasarım Promptu — TinderSnap

**Apple Dark Mode felsefesiyle, minimal Tinder tarzı Telegram WebApp tasarım brief'i.**

Bu metni Google Stitch veya benzeri AI tasarım araçlarına vererek **TinderSnap** uygulamasının tüm sayfalarını sıfırdan, tutarlı ve yayın kalitesinde tasarlayabilirsin.

---

## Tasarım Felsefesi

**"Apple dark mode'da Tinder nasıl tasarlardı?"**

- **Minimal:** Her ekranda tek bir odak noktası. Gereksiz öğe yok.
- **Derin:** Katmanlı siyah tonları, derinlik hissi.
- **Yumuşak:** Keskin kenarlar yerine organik, yuvarlak formlar.
- **İnsan odaklı:** Her piksel bir amaca hizmet eder.
- **Sessiz lüks:** Gösterişsiz, rafine detaylar.
- **Tinder DNA:** Flört, eğlence, oyunculuk — ama Apple'ın disipliniyle.

---

## Proje Özeti

**TinderSnap**, Telegram içinde açılan bir WebApp. Kullanıcı bir selfie yükler; yapay zeka (Gemini) bu fotoğraftan 6 adet Tinder tarzı profil fotoğrafı üretir. Freemium: 3 ücretsiz deneme, sonrası Telegram Stars ile token satın alınır.

- **Platform:** Telegram Mini App (mobil öncelikli, dar viewport)
- **Hedef kitle:** 18–35 yaş, dating app kullanıcıları
- **Diller:** EN, TR, RU (placeholder metinler İngilizce)

---

## Tasarım Sistemi

### Renk Paleti — Apple Dark

| Rol | Değer | Kullanım |
|-----|-------|----------|
| **Background (en derin)** | `#000000` | Ana sayfa arka planı |
| **Background (secondary)** | `#0C0C0E` | Kartlar ve içerik blokları |
| **Surface** | `#161618` | Kartlar, input, modal |
| **Elevated** | `#1C1C1E` | Hover, seçili durum |
| **Border** | `#2C2C2E` | İnce ayırıcılar |
| **Primary** | `#FF375F` | CTA, ana aksiyon (Tinder kırmızı) |
| **Primary muted** | `rgba(255,55,95,0.12)` | Seçili arka plan, vurgu |
| **Text primary** | `#FFFFFF` | Başlıklar, kritik metin |
| **Text secondary** | `#98989A` | Açıklama, ikincil metin |
| **Text tertiary** | `#636366` | Placeholder, disabled |
| **Success** | `#34C759` | Başarı, onay |
| **Destructive** | `#FF3B30` | Çıkış, silme |

**Kural:** Tinder kırmızısı (`#FF375F`) sadece CTA ve kritik aksiyonlarda. Ekranda en fazla 1–2 primary buton.

---

### Tipografi — SF Pro Ruhu

**Font:** SF Pro Display / SF Pro Text (veya Inter, system-ui fallback)

| Seviye | Boyut | Ağırlık | Line-height | Kullanım |
|--------|-------|---------|-------------|----------|
| **Large Title** | 34px | Bold (700) | 1.2 | Sayfa başlığı |
| **Title 1** | 28px | Semibold (600) | 1.25 | Bölüm başlığı |
| **Title 2** | 22px | Semibold (600) | 1.3 | Kart başlığı |
| **Title 3** | 20px | Semibold (600) | 1.3 | Alt başlık |
| **Body** | 17px | Regular (400) | 1.47 | Gövde metni |
| **Callout** | 16px | Regular (400) | 1.37 | Açıklama |
| **Footnote** | 13px | Regular (400) | 1.35 | Küçük, etiket |
| **Caption** | 12px | Regular (400) | 1.33 | Tarih, badge |

**Kurallar:**
- Başlık hiyerarşisi net: her sayfada tek Large Title.
- Gövde metinlerde 17px minimum (okunabilirlik).
- Metin kontrastı: primary en az #FFFFFF, secondary #98989A.
- Letter-spacing: başlıklarda -0.5px.

---

### Spacing & Layout

- **Base unit:** 8px (grid: 8, 16, 24, 32, 40, 48)
- **Sayfa padding:** 20px yatay, 16px üst
- **Kart padding:** 20px iç
- **Bileşen arası:** 12–16px
- **Safe area:** `padding-bottom: env(safe-area-inset-bottom)` tüm sabit alt elementlerde

---

### Bileşen Kuralları

**Butonlar:**
- Min-height: 50px (Apple dokunma alanı)
- Border-radius: 14px (yumuşak)
- Primary: dolu arka plan, beyaz metin
- Secondary: outline veya surface arka plan
- Danger: outline, kırmızı metin

**Kartlar:**
- Border-radius: 16px
- Arka plan: #161618
- Border: 1px #2C2C2E (opsiyonel)
- Gölge: minimal, sadece elevated durumda

**Input:**
- Min-height: 50px
- Font-size: 17px (zoom önleme)
- Border-radius: 12px

**İkonlar:**
- Boyut: 24px (standart), 20px (kompakt)
- Stroke: 1.5–2px
- Renk: text-secondary veya primary

---

**Genel UX:**
- Sayfa geçişleri: hafif fade (200–300ms)
- Loading: sistem spinner veya skeleton
- Boş durum: minimal metin + tek CTA
- Hata: net mesaj + "Tekrar dene" butonu

---

## Sayfa Brief'leri

Her sayfa sıfırdan tasarlanmalı. Minimal, odaklı, Apple dark estetiği.

---

### 1. HomePage (Ana Sayfa)

**Odak:** Kullanıcıyı karşılamak, token göstermek, "Fotoğraf oluştur" aksiyonuna yönlendirmek.

**Layout:**
- **Header:** Sol: "Merhaba, [İsim]" (Large Title). Sağ: token badge — yuvarlak, minimal, sayı + küçük ikon.
- **Alt:** "1 selfie → 6 Tinder fotoğrafı" (Callout, text-secondary).
- **Son üretimler:** Başlık "Son Üretimler" (Title 3). Yatay scroll veya 2 sütun grid. Her kart: 6 thumbnail grid + tarih + chip (Tamamlandı/Bekliyor). Boşsa: "Henüz üretim yok" + "Şimdi oluştur" linki.
- **Stil galerisi:** 6 stil kartı: Natural, Outdoor, Social, Professional, Cozy, Travel. Yatay scroll. Her kart: minimal ikon, stil adı, tek satır. Seçim yok — sadece önizleme.
- **CTA:** Tam genişlik, 50px yükseklik, primary renk. "Fotoğraf Oluştur". Alt navigasyonun hemen üstünde.

**Not:** Alt navigasyon görünür (Ana Sayfa seçili).

---

### 2. OnboardingPage (Hoş Geldin)

**Odak:** 4 adımda ilk kullanıcıyı alıştırmak. Minimal, akıcı.

**Genel:** Adım göstergesi (1/4, 2/4, 3/4, 4/4) üstte veya üst-orta. Geri ok sol üstte (2–4. adımlarda). Alt navigasyon **gösterilmez**.

- **Adım 1:** Logo veya minimal illüstrasyon. "TinderSnap'e Hoş Geldin" (Large Title). "1 selfie ile 6 Tinder fotoğrafı oluştur" (Callout). Tek CTA: "Başla".
- **Adım 2:** "Hangi tarz sana daha yakın?" (Title 1). 3–4 seçenek kartı: Doğal, Sosyal, Profesyonel, Macera. Tek seçim. Seçilen kart: primary muted arka plan veya ince primary border.
- **Adım 3:** "Hazır olduğunda haber verelim mi?" (Title 3). Toggle (Apple stili). "Devam et" butonu.
- **Adım 4:** Onay ikonu (✓). "Hazırsın!" (Large Title). "Şimdi ilk fotoğraflarını oluştur" (Callout). CTA: "Fotoğraf Oluştur".

**Not:** Safe area her adımda korunmalı.

---

### 3. GeneratePage (Fotoğraf Oluştur)

**Odak:** Selfie yükleme → stil seçimi → üretim. 3 aşamalı tek sayfa (adımlar sırayla).

**Aşama 1 — Fotoğraf:**
- Başlık: "Fotoğrafını ekle" (Title 1).
- Yükleme alanı: Kesikli çerçeve, 16px radius. Ortada kamera ikonu + "Çek veya galeriden seç" (Callout). Drag-over: primary border.
- Uyarı: "JPG/PNG, max 10MB. Yüz net görünmeli." (Footnote, text-tertiary).
- Önizleme: Yüklenen fotoğraf + "Değiştir" linki.
- Bakiye 0: Uyarı banner + "Token al" butonu.
- Alt: "Devam" (fotoğraf seçildikten sonra aktif).

**Aşama 2 — Stil:**
- Başlık: "Stilini seç" (Title 1).
- 6 stil kartı: Yatay scroll veya 2 sütun. Her kart: stil adı, 1 satır açıklama, örnek görsel/ikon. Seçilen: primary border veya glow.
- Alt: "Üretimi başlat".

**Aşama 3 — İlerleme:**
- Başlık: "Fotoğrafların hazırlanıyor" (Title 1).
- Progress: "Analiz ediliyor…" → "Fotoğraflar üretiliyor…" → "Tamamlandı". Progress bar (primary renk).
- Tamamlanınca: "Hazır! Sonuçlara git" + buton.

**Not:** Alt navigasyon **gösterilmez**. Header'da geri butonu.

---

### 4. ResultPage (Üretim Sonuçları)

**Odak:** 6 fotoğrafı göstermek; indirme, favori, paylaşma.

**Layout:**
- Başlık: "Fotoğrafların hazır" (Large Title). İsteğe tarih/stil.
- **Grid:** 6 fotoğraf, 2x3 veya 3x2. Kare oran, 12px radius. Tıklanınca tam ekran.
- Her kartta: köşede İndir ikonu, Favori (kalp) ikonu.
- Alt: "Yeni üretim yap" (secondary), "Paylaş" (primary).
- Tam ekran: Fotoğraf tam ekran, kapat (X), İndir, Favori.

**Not:** Alt navigasyon **gösterilmez**. Sadece ana sayfa / yeni üretim kısayolları.

---

### 5. HistoryPage (Geçmiş)

**Odak:** Geçmiş üretimleri listelemek.

**Layout:**
- Başlık: "Üretim geçmişi" (Large Title).
- Liste: Her satır bir iş kartı. Solda 6 thumbnail (grid veya strip), sağda veya altta tarih + stil adı. Chip: Tamamlandı/Başarısız.
- Sayfalama: 10'ar öğe, "Daha fazla" veya sayfa numaraları.
- Boş: "Henüz üretim yok" + "İlk fotoğraflarını oluştur" CTA.
- Loading: Skeleton (6 kare + 2 satır metin).

**Not:** Alt navigasyon görünür (Geçmiş seçili).

---

### 6. TokensPage (Token / Satın Al)

**Odak:** Bakiye göstermek, paket seçtirip satın alma akışına götürmek.

**Layout:**
- Başlık: "Token'ların" (Large Title).
- Bakiye alanı: Büyük, merkezi. İkon + sayı + "ücretsiz deneme" veya "token" etiketi.
- Açıklama: "Her üretim 1 token kullanır. Paket seç, Stars ile öde." (Callout).
- **3 paket kartı:**
  - Starter: 10 üretim, 50 Stars, "Başlangıç paketi"
  - Pro: 30 üretim + HD, 120 Stars, "En popüler" rozeti
  - Unlimited: 30 gün sınırsız, 300 Stars
- Her kartta "Satın al" butonu (primary).
- Ödeme sonrası: Toast veya inline mesaj; bakiye güncellenir.

**Not:** Alt navigasyon görünür veya gizlenebilir (proje kurallarına göre).

---

### 7. SettingsPage (Ayarlar)

**Odak:** Profil, dil, bildirim, yasal, çıkış. Tek yerden yönetim.

**Layout (sırayla):**
- **Telegram profili:** Avatar (yuvarlak), ad soyad, @username. Üst kart.
- **Bakiye:** "X token kaldı" + "Token satın al" linki.
- **Dil:** Satır, sağda mevcut dil. Tıklanınca modal (EN/TR/RU).
- **Tercih edilen stil:** Varsayılan stil, değiştirilebilir.
- **Bildirimler:** Toggle.
- **Gizlilik politikası:** Satır, tıklanınca link.
- **Kullanım koşulları:** Aynı.
- **Çıkış:** "Çıkış yap" (danger, metin veya outline).
- Footer: "TinderSnap v1.0" (Caption, text-tertiary).

**Not:** Alt navigasyon görünür (Ayarlar seçili).

---

## Alt Navigasyon (Bottom Navigation)

Tüm sayfalarda (Onboarding, Generate, Result, Tokens hariç – proje kurallarına göre):

- **Ana Sayfa** (ev)
- **Oluştur** (kamera)
- **Geçmiş** (saat/liste)
- **Token** (yıldız/coin)
- **Ayarlar** (dişli)

Arka plan: #161618. Üst border: 1px #2C2C2E. Seçili: primary renk. Diğerleri: text-tertiary. Alt padding: `env(safe-area-inset-bottom)`.

---

## Ek Talimatlar

1. **Tutarlılık:** Tüm sayfalarda aynı renk paleti, aynı border-radius, aynı tipografi.
2. **Mobil öncelik:** 375x812 veya 390x844 viewport. Min 44px dokunma alanı.
3. **Placeholder metin:** İngilizce: "Welcome", "Create photos", "Token balance", "Settings".
4. **İkonlar:** Lucide veya SF Symbols. Minimal, ince stroke.
5. **Telegram:** Üst/alt safe area mutlaka korunmalı.
6. **Erişilebilirlik:** Buton ve linklerde yeterli kontrast; focus durumları.
7. **Apple ruhu:** Her ekranda boşluk, nefes alan. Gereksiz süsleme yok.

---

## Çıktı Beklentisi

- Her sayfa için ayrı ekran tasarımı (high-fidelity).
- İsteğe bağlı: bileşen seti (buton, kart, input, list row) tek "Design system" sayfasında.
- Responsive: Öncelik mobil; tablet için max-width ile genişleyen layout.

Bu brief ile TinderSnap'in tüm sayfalarını Apple dark konseptinde, minimal ve Tinder tarzında tasarlayabilirsin.
