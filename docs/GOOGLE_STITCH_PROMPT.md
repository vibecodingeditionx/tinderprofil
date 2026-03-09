# Google Stitch / AI Tasarım Promptu — TinderSnap Tüm Sayfalar

Bu metni Google Stitch veya benzeri bir AI tasarım aracına vererek **TinderSnap** Telegram WebApp’inin tüm sayfalarının tutarlı, mobil öncelikli tasarımlarını üretmesini sağlayabilirsin.

---

## Proje Özeti

**TinderSnap**, Telegram içinde açılan bir WebApp’tir. Kullanıcı bir selfie yükler; yapay zeka (Google Gemini) bu fotoğraftan 6 adet Tinder tarzı profil fotoğrafı üretir. Freemium: 3 ücretsiz deneme, sonrası Telegram Stars ile token satın alınır.

- **Platform:** Telegram Mini App (mobil öncelikli, dar viewport)
- **Hedef kitle:** 18–35 yaş, dating app kullanıcıları
- **Diller:** EN, TR, RU (metinler placeholder olarak İngilizce yazılabilir)

---

## Tasarım Sistemi (Zorunlu)

Tüm ekranlar bu sisteme uymalı:

### Renkler
- **Arka plan:** `#0A0A0A` (app-black), sayfa içi bloklar `#141414` (app-dark)
- **Kartlar:** `#1C1C1E` (app-card), border `#3A3A3C` (app-border)
- **Primary (CTA, vurgu):** `#FF4D6D` (Tinder kırmızı-pembe), hover/light `#FF6B84`
- **Secondary (ikincil vurgu):** `#FF9F43` (turuncu)
- **Metin:** Ana `#FFFFFF`, ikincil `#8E8E93`, soluk `#636366`
- **Primary muted (arka plan vurgu):** `rgba(255,77,109,0.15)`

### Tipografi
- Font: Inter veya SF Pro Display, sistem sans-serif fallback
- Başlıklar: bold, 22–24px (sayfa başlığı), 16–18px (bölüm)
- Gövde: 14–16px, line-height 1.4–1.5
- Küçük metin: 12px, muted renk

### Bileşen Kuralları
- Butonlar: min-height 44px, border-radius 12px, primary butonlar #FF4D6D
- Kartlar: border-radius 12–16px, padding 16–24px, ince border veya gölge
- Input/alan: min-height 48px, font-size 16px (zoom önleme)
- Alt navigasyon ve sabit alanlar: `padding-bottom: env(safe-area-inset-bottom)` kullan
- Mobil: tek sütun, yeterli dokunma alanı (min 44px)

### Genel UX
- Koyu tema (Telegram ile uyumlu)
- Sayfa geçişlerinde hafif slide/fade düşün
- Loading: skeleton veya spinner, primary renkte
- Boş durumlar: kısa metin + CTA butonu
- Hata durumları: net mesaj + “Tekrar dene” butonu

---

## Sayfa Listesi ve Detaylı Brief

Her sayfa için: ekran adı, amaç, üst alan (header), içerik blokları, alt CTA/navigasyon ve özel notlar.

---

### 1. HomePage (Ana Sayfa)

**Amaç:** Kullanıcıyı karşılamak, token bakiyesini göstermek, “Fotoğraf oluştur” aksiyonuna yönlendirmek.

**Layout:**
- Üst: Sol tarafta “Merhaba, [İsim] 👋” (veya “Welcome, [Name]”), sağ üstte **token bakiye widget’ı** (örn. yıldız ikonu + “3” veya “12” gibi sayı, küçük badge).
- Hemen altında kısa tanım: “1 selfie → 6 Tinder fotoğrafı” gibi bir cümle.
- **Son üretimler** bölümü: “Son Üretimler” başlığı, en fazla 3 satırda liste (tarih + “Tamamlandı”/“Bekliyor” chip’i). Liste boşsa “Henüz üretim yok” + “Şimdi oluştur” linki.
- “Tümünü gör” linki → Geçmiş sayfasına gidecek şekilde göster.
- **Stil galerisi**: 6 fotoğraf stilinin küçük kartları (Natural, Outdoor, Social, Professional, Cozy, Travel). Yatay kaydırılabilir veya 2x3 grid. Her kartta stil adı + küçük örnek görsel/ikon.
- Alt kısım: Telegram MainButton ile aynı işlevi gören büyük **“Fotoğraf Oluştur”** CTA butonu (primary renk, tam genişlik).

**Not:** Alt navigasyon bu sayfada görünür (Ana Sayfa seçili).

---

### 2. OnboardingPage (Hoş Geldin / Kurulum)

**Amaç:** İlk kullanıcıyı 4 adımda uygulamaya alıştırmak; tercih ve izin toplamak.

**Genel:** Adım göstergesi (1/4, 2/4, 3/4, 4/4) üstte veya üst-orta. “İleri” / “Başla” butonları sağ altta veya ortada. Geri ok sol üstte (2–4. adımlarda).

- **Adım 1 – Karşılama:** Büyük logo veya illüstrasyon, “TinderSnap’e Hoş Geldin” başlığı, “1 selfie ile 6 Tinder fotoğrafı oluştur” alt metni. Tek CTA: “Başla”.
- **Adım 2 – Tercih:** “Hangi tarz sana daha yakın?” Başlık altında 3–4 seçenek kartı (örn. Doğal, Sosyal, Profesyonel, Macera). Tek seçim, seçilen kart vurgulu (border veya arka plan primary muted).
- **Adım 3 – Bildirim:** “Hazır olduğunda haber verelim mi?” metni, bildirim aç/kapa toggle. “Devam et” butonu.
- **Adım 4 – Tamamlandı:** Onay/başarı ikonu (örn. tik), “Hazırsın!” başlığı, “Şimdi ilk fotoğraflarını oluştur” alt metni. CTA: “Fotoğraf Oluştur” → Generate sayfasına yönlendirme.

**Not:** Bu sayfada alt navigasyon **gösterilmez**. Safe area (özellikle alt) korunmalı.

---

### 3. GeneratePage (Fotoğraf Oluştur)

**Amaç:** Selfie yükleme → stil seçimi → üretim başlatma ve ilerleme gösterme. 3 aşamalı tek sayfa (adımlar sırayla gösterilir).

**Aşama 1 – Fotoğraf yükleme:**
- Başlık: “Fotoğrafını ekle”.
- Büyük yükleme alanı: Kesikli çerçeve, kamera ikonu, “Çek veya galeriden seç” metni. Drag-over durumunda border primary renkte.
- Küçük uyarı: “JPG/PNG, max 10MB. Yüz net görünmeli.”
- Yüklenen fotoğraf önizlemesi (küçük crop) + “Değiştir” butonu.
- Token kontrolü: Bakiye 0 ise uyarı banner’ı + “Token al” butonu.
- Alt: “Devam” butonu (fotoğraf seçildikten sonra aktif).

**Aşama 2 – Stil seçimi:**
- Başlık: “Stilini seç”.
- 6 stil kartı: Yatay scroll veya 2 sütun grid. Her kart: stil adı (Natural, Outdoor, Social, Professional, Cozy, Travel), kısa açıklama (1 satır), örnek görsel veya ikon. Seçilen kart primary border veya glow.
- Alt: “Üretimi başlat” butonu.

**Aşama 3 – Üretim ilerlemesi:**
- Başlık: “Fotoğrafların hazırlanıyor”.
- Dikey progress: “Analiz ediliyor…” → “Fotoğraflar üretiliyor…” → “Tamamlandı”. Progress bar (primary renk) veya adım göstergesi.
- İsteğe bağlı: Basit animasyon (pulse/glow).
- Tamamlanınca kısa metin: “Hazır! Sonuçlara git” ve buton → Result sayfasına yönlendirme.

**Not:** Bu sayfada alt navigasyon **gösterilmez**. Header’da geri butonu olabilir.

---

### 4. ResultPage (Üretim Sonuçları)

**Amaç:** 6 üretilen fotoğrafı göstermek; indirme, favorileme ve paylaşma sunmak.

**Layout:**
- Üst: “Fotoğrafların hazır” benzeri başlık, isteğe bağlı stil adı veya tarih.
- **Grid:** 6 fotoğraf, 2 sütun x 3 satır veya 3 sütun x 2 satır. Her hücre: kare oran, yuvarlatılmış köşe, tıklanınca tam ekran.
- Her fotoğraf kartında: küçük **İndir** ikonu (köşede) ve isteğe **Favori** (kalp) ikonu.
- Alt bölüm: “Yeni üretim yap” (secondary veya outline buton), “Paylaş” (Telegram’a gönder) butonu.
- Tam ekran görünüm: Fotoğraf tam ekran, kapat (X), İndir ve Favori butonları.

**Not:** Alt navigasyon bu sayfada **gösterilmez** veya sadece “Ana sayfa” / “Yeni üretim” kısayolları verilebilir.

---

### 5. HistoryPage (Geçmiş)

**Amaç:** Kullanıcının geçmiş üretimlerini listelemek; her birine tıklanınca Result sayfasına gitmek.

**Layout:**
- Başlık: “Üretim geçmişi”.
- Liste: Her satır bir “iş” kartı. Kartta: solda 6 küçük thumbnail (grid veya yatay strip), sağda veya altta tarih + stil adı. “Tamamlandı” / “Başarısız” chip’i.
- Sayfalama: 10’ar öğe, altta “Daha fazla” veya sayfa numaraları.
- Boş durum: “Henüz üretim yok” metni + “İlk fotoğraflarını oluştur” CTA.
- Loading: Kartlar için skeleton (6 kare + 2 satır metin).

**Not:** Alt navigasyon görünür (Geçmiş seçili).

---

### 6. TokensPage (Token / Satın Al)

**Amaç:** Mevcut bakiye göstermek, paket seçtirip Telegram Stars ile satın alma akışına götürmek.

**Layout:**
- Üst: “Token’ların” veya “Bakiye” başlığı. Hemen altında büyük bakiye alanı: yıldız/coin ikonu + sayı (örn. “3”) + “ücretsiz deneme” veya “token” etiketi.
- Açıklama: “Her üretim 1 token kullanır. Paket seç, Stars ile öde.”
- **3 paket kartı** (dikey liste):
  - **Starter:** “10 üretim”, “50 Stars”, “Başlangıç paketi” alt metni.
  - **Pro:** “30 üretim + HD”, “120 Stars”, “En popüler” rozeti (primary veya secondary).
  - **Unlimited:** “30 gün sınırsız”, “300 Stars”.
- Her kartta “Satın al” veya “Seç” butonu (primary/secondary).
- Ödeme sonrası: “Ödeme başarılı” toast/mesajı; bakiye alanı güncellenmiş gösterilebilir (ekranda metin olarak “Bakiye: 13” gibi).

**Not:** Alt navigasyon görünür (Token seçili). Bu sayfada alt navigasyon gizlenebilir de (PRD’de tokens hide listesinde).

---

### 7. SettingsPage (Ayarlar)

**Amaç:** Profil, dil, bildirim, yasal linkler ve çıkış tek yerden yönetilsin.

**Layout (sırayla):**
- **Telegram profili:** Avatar (yuvarlak), ad soyad, @username. Kart veya üst blok.
- **Bakiye:** “X token kaldı” + “Token satın al” linki/butonu (Tokens sayfasına).
- **Dil:** “Dil” satırı, sağda mevcut dil (EN/TR/RU). Tıklanınca dil seçim modal’ı (3 seçenek).
- **Tercih edilen stil:** Varsayılan stil (Natural vb.) gösterilip değiştirilebilir (liste veya dropdown).
- **Bildirimler:** Aç/Kapa toggle.
- **Gizlilik politikası:** Satır, tıklanınca dış link veya in-app sayfa.
- **Kullanım koşulları:** Aynı şekilde.
- **Çıkış:** “Çıkış yap” butonu (danger/outline veya metin).
- Footer: “TinderSnap v1.0” veya “Versiyon 1.0.0”.

**Not:** Alt navigasyon görünür (Ayarlar seçili).

---

## Alt Navigasyon (Bottom Navigation)

Tüm sayfalarda (Onboarding, Generate, Result, Tokens hariç – proje kurallarına göre) sabit alt navigasyon:

- **Ana Sayfa** (ev ikonu)
- **Oluştur** (kamera/fotoğraf ikonu)
- **Geçmiş** (saat/liste ikonu)
- **Token** (yıldız/coin ikonu)
- **Ayarlar** (dişli ikonu)

Arka plan: app-card, üst border ince. Seçili öğe primary renkte, diğerleri muted. Alt padding: `env(safe-area-inset-bottom)`.

---

## Ek Talimatlar

1. **Tutarlılık:** Tüm sayfalarda aynı renk paleti, aynı border-radius ve buton stilleri kullan.
2. **Mobil öncelik:** 375x812 veya 390x844 gibi bir viewport için tasarla; büyük parmak alanları.
3. **Placeholder metin:** İngilizce yazılabilir; “Welcome”, “Create photos”, “Token balance”, “Settings” vb.
4. **İkonlar:** Lucide veya benzeri minimal çizgi ikonlar; doluluk oranı düşük.
5. **Telegram:** Üst/alt safe area’yı mutlaka bırak; Telegram UI ile çakışma olmasın.
6. **Erişilebilirlik:** Buton ve linklerde yeterli kontrast; focus durumları düşünülebilir.

---

## Çıktı Beklentisi

- Her sayfa için ayrı ekran tasarımı (wireframe veya high-fidelity).
- İsteğe bağlı: bileşen seti (buton, kart, input, list row) tek bir “Design system” sayfasında toplanabilir.
- Responsive: Öncelik mobil; tablet için sadece max-width ile genişleyen layout yeterli.

Bu brief’i Google Stitch’e vererek “TinderSnap Telegram WebApp – tüm sayfalar” için tutarlı ekran tasarımları üretebilirsin.
