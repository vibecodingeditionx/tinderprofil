# 📋 PRD — TinderSnap: AI Tinder Profil Fotoğrafı Üreticisi
## Telegram WebApp | Uçtan Uca Geliştirme Dökümanı

> **Versiyon:** 1.0.0  
> **Stack:** React + TypeScript + Vite (Frontend) | FastAPI + Python (Backend) | Supabase (DB) | Google Gemini (AI)  
> **Deployment:** Vercel (Frontend) + Railway (Backend) + Supabase (DB/Storage)

---

## 1. 🎯 PROJE GENEL BAKIŞI

### 1.1 Proje Özeti
**TinderSnap**, Telegram WebApp üzerinde çalışan, yapay zeka destekli bir Tinder profil fotoğrafı üreticisidir. Kullanıcı selfie çeker veya fotoğraf yükler; sistem **Google Gemini** kullanarak analiz eder ve Tinder profilinde kullanılabilecek 6 adet profesyonel, çekici fotoğraf üretir.

### 1.2 Temel Değer Önerisi
- 📸 1 selfie → 6 Tinder-ready fotoğraf
- 🤖 Yapay zeka ile yüz koruma + stil optimizasyonu
- ⚡ Telegram içinden anında erişim, uygulama indirme yok
- 💎 Freemium model: 3 ücretsiz deneme, sonrası Telegram Stars ile ödeme

### 1.3 Başarı Kriterleri
| Metrik | Hedef |
|--------|-------|
| Fotoğraf üretim süresi | < 30 saniye |
| Kullanıcı dönüşüm oranı (free → paid) | ≥ %15 |
| Kullanıcı memnuniyeti (CSAT) | ≥ 4.2/5 |
| Onboarding tamamlama oranı | ≥ %85 |

---

## 2. 👥 HEDEF KİTLE VE KULLANICI HİKAYELERİ

### 2.1 Hedef Kullanıcılar
- **Birincil:** 18-35 yaş, aktif dating app kullanıcıları
- **İkincil:** Sosyal medya için fotoğraf kalitesini artırmak isteyenler
- **Coğrafya:** Türkiye, Rusya, Orta Doğu, Avrupa (Telegram kullanım yoğunluğuna göre)

### 2.2 Kullanıcı Hikayeleri

**Temel Kullanıcı Hikayeleri:**
1. Kullanıcı olarak Telegram'dan direkt uygulamaya girebilmek istiyorum
2. Selfie çekip 6 farklı Tinder fotoğrafı üretebilmek istiyorum
3. Üretilen fotoğrafları telefona indirebilmek istiyorum
4. Üretim geçmişimi görebilmek istiyorum
5. Beğendiğim stili tekrar kullanabilmek istiyorum

**Premium Kullanıcı Hikayeleri:**
6. Daha fazla fotoğraf üretmek için token satın almak istiyorum
7. Farklı stil şablonları seçebilmek istiyorum
8. HD kalitede fotoğraf indirebilmek istiyorum

---

## 3. 💰 MONETİZASYON MODELİ

### 3.1 Freemium Yapısı
| Plan | Özellik | Ücret |
|------|---------|-------|
| **Ücretsiz** | 3 fotoğraf üretim hakkı (hesap başına) | Ücretsiz |
| **Starter Pack** | +10 üretim hakkı | 50 Telegram Stars |
| **Pro Pack** | +30 üretim hakkı + HD kalite | 120 Telegram Stars |
| **Unlimited** | Sınırsız üretim (30 gün) | 300 Telegram Stars |

### 3.2 Token/Kredi Sistemi
- Her fotoğraf seti üretimi = 1 token tüketimi
- Başlangıç tokeni: 3 (ücretsiz)
- Token bakiyesi Supabase `token_balances` tablosunda tutulur
- Token işlemleri RPC ile atomik yapılır

### 3.3 Ödeme Akışı (Telegram Stars)
- Currency: `XTR`
- Provider token: `""` (boş string)
- Payload: Sadece `transaction_id` (UUID, max 128 byte)
- Pre-checkout: 10 saniye içinde yanıtlanır

---

## 4. 🏗️ TEKNOLOJİ STACK

### 4.1 Frontend
```json
{
  "react": "^18.3.1",
  "react-dom": "^18.3.1",
  "react-router-dom": "^6.22.3",
  "@twa-dev/sdk": "^6.9.1",
  "@supabase/supabase-js": "^2.58.0",
  "@tanstack/react-query": "^5.90.2",
  "framer-motion": "^11.1.6",
  "i18next": "^25.5.2",
  "react-i18next": "^16.0.0",
  "i18next-browser-languagedetector": "^8.2.0",
  "i18next-http-backend": "^3.0.2",
  "react-hook-form": "^7.63.0",
  "@hookform/resolvers": "^5.2.2",
  "zod": "^4.1.11",
  "lucide-react": "^0.344.0",
  "react-hot-toast": "^2.4.1",
  "clsx": "^2.1.1",
  "tailwind-merge": "^3.3.1"
}
```

### 4.2 Backend
```txt
fastapi==0.104.1
uvicorn[standard]==0.23.2
python-multipart==0.0.6
pydantic>=2.5.0
pydantic-settings==2.1.0
python-dotenv==1.0.0
aiohttp>=3.9.0
httpx~=0.25.2
python-telegram-bot[job-queue]==20.7
supabase==2.4.4
postgrest>=0.14.0,<0.17.0
gotrue==1.3.0
google-generativeai==0.3.2
Pillow==10.2.0
python-jose==3.3.0
```

### 4.3 AI Entegrasyonu
- **Model:** `gemini-2.0-flash-lite` (metin analizi + prompt üretimi)
- **Görüntü Üretimi:** Google Gemini Imagen API (`imagegeneration@006` veya Imagen 3)
- **Fallback:** Eğer Imagen API kullanılamıyorsa Stable Diffusion API (Replicate)

### 4.4 Altyapı
- **Frontend Deploy:** Vercel
- **Backend Deploy:** Railway
- **Veritabanı:** Supabase (PostgreSQL 15+)
- **Dosya Depolama:** Supabase Storage (bucket: `profile-photos`)
- **CDN:** Supabase Storage CDN (otomatik)

---

## 5. 📁 PROJE YAPISI

```
tindersnap/
├── frontend/
│   ├── public/
│   │   ├── locales/
│   │   │   ├── en/
│   │   │   │   ├── common.json
│   │   │   │   ├── auth.json
│   │   │   │   ├── navigation.json
│   │   │   │   ├── errors.json
│   │   │   │   ├── onboarding.json
│   │   │   │   ├── generate.json
│   │   │   │   ├── settings.json
│   │   │   │   └── tokens.json
│   │   │   ├── tr/
│   │   │   │   └── [same structure]
│   │   │   └── ru/
│   │   │       └── [same structure]
│   │   ├── manifest.json
│   │   └── logo.svg
│   ├── src/
│   │   ├── components/
│   │   │   ├── common/
│   │   │   │   ├── ErrorBoundary.tsx
│   │   │   │   ├── LoadingSpinner.tsx
│   │   │   │   ├── FullScreenLoader.tsx
│   │   │   │   ├── PageTransition.tsx
│   │   │   │   ├── EmptyState.tsx
│   │   │   │   └── ErrorState.tsx
│   │   │   ├── layout/
│   │   │   │   ├── BottomNavigation.tsx
│   │   │   │   └── Header.tsx
│   │   │   ├── ui/
│   │   │   │   ├── Button.tsx
│   │   │   │   ├── Input.tsx
│   │   │   │   ├── Modal.tsx
│   │   │   │   ├── Card.tsx
│   │   │   │   ├── Badge.tsx
│   │   │   │   ├── SkeletonLoader.tsx
│   │   │   │   ├── LanguageModal.tsx
│   │   │   │   └── TelegramProfileCard.tsx
│   │   │   ├── photo/
│   │   │   │   ├── PhotoUploader.tsx
│   │   │   │   ├── CameraCapture.tsx
│   │   │   │   ├── PhotoPreview.tsx
│   │   │   │   ├── GeneratedPhotoGrid.tsx
│   │   │   │   ├── GeneratedPhotoCard.tsx
│   │   │   │   ├── StyleSelector.tsx
│   │   │   │   └── GenerationProgress.tsx
│   │   │   └── tokens/
│   │   │       ├── TokenBalance.tsx
│   │   │       ├── PackageCard.tsx
│   │   │       └── PurchaseModal.tsx
│   │   ├── contexts/
│   │   │   ├── TelegramContext.tsx
│   │   │   ├── AuthContext.tsx
│   │   │   └── TokenContext.tsx
│   │   ├── hooks/
│   │   │   ├── use-i18n.ts
│   │   │   ├── use-local-storage.ts
│   │   │   ├── use-photo-upload.ts
│   │   │   ├── use-generation.ts
│   │   │   └── use-token-balance.ts
│   │   ├── lib/
│   │   │   ├── i18n.ts
│   │   │   ├── supabase.ts
│   │   │   ├── api.ts
│   │   │   └── utils.ts
│   │   ├── pages/
│   │   │   ├── HomePage.tsx
│   │   │   ├── OnboardingPage.tsx
│   │   │   ├── GeneratePage.tsx
│   │   │   ├── HistoryPage.tsx
│   │   │   ├── SettingsPage.tsx
│   │   │   ├── TokensPage.tsx
│   │   │   └── ResultPage.tsx
│   │   ├── services/
│   │   │   ├── telegramPaymentService.ts
│   │   │   └── photoService.ts
│   │   ├── types/
│   │   │   ├── index.ts
│   │   │   ├── supabase.ts
│   │   │   └── photo.ts
│   │   ├── utils/
│   │   │   └── helpers.ts
│   │   ├── App.tsx
│   │   ├── main.tsx
│   │   └── index.css
│   ├── supabase/
│   │   ├── migrations/
│   │   │   ├── 001_create_users_and_profiles.sql
│   │   │   ├── 002_create_token_system.sql
│   │   │   ├── 003_create_photo_tables.sql
│   │   │   ├── 004_create_rpc_functions.sql
│   │   │   └── 005_create_rls_policies.sql
│   │   └── seed/
│   │       └── initial_packages.sql
│   ├── index.html
│   ├── vite.config.ts
│   ├── tailwind.config.js
│   ├── tsconfig.json
│   ├── .env.example
│   └── package.json
│
├── backend/
│   ├── api/
│   │   ├── endpoints/
│   │   │   ├── __init__.py
│   │   │   ├── auth.py
│   │   │   ├── payment.py
│   │   │   ├── generate.py
│   │   │   └── history.py
│   │   ├── __init__.py
│   │   └── router.py
│   ├── core/
│   │   ├── __init__.py
│   │   ├── config.py
│   │   └── schemas.py
│   ├── services/
│   │   ├── __init__.py
│   │   ├── telegram_service.py
│   │   ├── payment_service.py
│   │   ├── supabase_service.py
│   │   ├── gemini_service.py
│   │   └── image_service.py
│   ├── main.py
│   ├── requirements.txt
│   ├── Procfile
│   ├── runtime.txt
│   ├── railway.json
│   ├── nixpacks.toml
│   └── .env.example
│
├── .gitignore
└── README.md
```

---

## 6. 🗄️ VERİTABANI ŞEMASI

### 6.1 Migration 001: users_and_profiles.sql
```sql
create extension if not exists "uuid-ossp";

-- Users
create table public.users (
  id uuid primary key default uuid_generate_v4(),
  telegram_id bigint unique not null,
  telegram_username text,
  telegram_first_name text,
  telegram_last_name text,
  telegram_language_code text default 'en',
  is_premium boolean default false,
  created_at timestamptz default now() not null,
  updated_at timestamptz default now() not null,
  last_seen_at timestamptz default now() not null
);

-- User Profiles
create table public.user_profiles (
  id uuid primary key default uuid_generate_v4(),
  user_id uuid references public.users(id) on delete cascade not null unique,
  display_name text,
  preferred_language text default 'en',
  notification_enabled boolean default true,
  theme_preference text default 'dark',
  onboarding_completed boolean default false,
  onboarding_data jsonb default '{}'::jsonb,
  preferred_style text default 'natural',
  created_at timestamptz default now() not null,
  updated_at timestamptz default now() not null
);

alter table public.users enable row level security;
alter table public.user_profiles enable row level security;

create index idx_users_telegram_id on public.users(telegram_id);

-- Auto-create profile trigger
create or replace function public.create_user_profile_for_new_user()
returns trigger language plpgsql security invoker set search_path = '' as $$
begin
  insert into public.user_profiles (user_id, preferred_language)
  values (new.id, coalesce(new.telegram_language_code, 'en'));
  return new;
end;
$$;

create trigger create_profile_on_user_insert
  after insert on public.users
  for each row execute function public.create_user_profile_for_new_user();
```

### 6.2 Migration 002: token_system.sql
```sql
-- Token Balances
create table public.token_balances (
  id uuid primary key default uuid_generate_v4(),
  user_id uuid references public.users(id) on delete cascade not null unique,
  balance integer default 3 not null check (balance >= 0),
  total_earned integer default 3 not null,
  total_spent integer default 0 not null,
  is_unlimited boolean default false not null,
  unlimited_expires_at timestamptz,
  created_at timestamptz default now() not null,
  updated_at timestamptz default now() not null
);

-- Token Packages
create table public.token_packages (
  id text primary key,
  name text not null,
  name_tr text not null,
  name_ru text not null,
  description text not null,
  description_tr text not null,
  tokens integer,
  is_unlimited boolean default false,
  duration_days integer,
  stars integer not null,
  is_popular boolean default false,
  sort_order integer default 0,
  is_active boolean default true,
  created_at timestamptz default now()
);

-- Token Transactions
create table public.token_transactions (
  id uuid primary key default uuid_generate_v4(),
  user_id uuid references public.users(id) on delete cascade not null,
  type text not null check (type in ('earn', 'spend', 'purchase', 'refund', 'bonus')),
  amount integer not null,
  balance_after integer not null,
  description text,
  reference_id text,
  telegram_charge_id text,
  created_at timestamptz default now() not null
);

-- Pending Transactions (in-memory alternatifi olarak DB'de)
create table public.pending_transactions (
  id uuid primary key default uuid_generate_v4(),
  user_id uuid references public.users(id) on delete cascade not null,
  package_id text references public.token_packages(id) not null,
  stars integer not null,
  status text default 'pending' check (status in ('pending', 'completed', 'failed', 'cancelled')),
  telegram_charge_id text,
  created_at timestamptz default now() not null,
  completed_at timestamptz
);

alter table public.token_balances enable row level security;
alter table public.token_packages enable row level security;
alter table public.token_transactions enable row level security;
alter table public.pending_transactions enable row level security;

-- Auto-create token balance trigger
create or replace function public.create_token_balance_for_new_user()
returns trigger language plpgsql security invoker set search_path = '' as $$
begin
  insert into public.token_balances (user_id, balance, total_earned)
  values (new.id, 3, 3);
  return new;
end;
$$;

create trigger create_token_on_user_insert
  after insert on public.users
  for each row execute function public.create_token_balance_for_new_user();
```

### 6.3 Migration 003: photo_tables.sql
```sql
-- Photo Styles (seed data)
create table public.photo_styles (
  id text primary key,
  name text not null,
  name_tr text not null,
  name_ru text not null,
  description text not null,
  description_tr text not null,
  prompt_template text not null,
  thumbnail_url text,
  is_premium boolean default false,
  sort_order integer default 0,
  is_active boolean default true
);

-- Generation Jobs
create table public.generation_jobs (
  id uuid primary key default uuid_generate_v4(),
  user_id uuid references public.users(id) on delete cascade not null,
  status text default 'pending' check (status in ('pending', 'analyzing', 'generating', 'completed', 'failed')),
  style_id text references public.photo_styles(id),
  source_photo_url text not null,
  source_photo_path text not null,
  analysis_result jsonb,
  error_message text,
  token_cost integer default 1,
  created_at timestamptz default now() not null,
  started_at timestamptz,
  completed_at timestamptz
);

-- Generated Photos
create table public.generated_photos (
  id uuid primary key default uuid_generate_v4(),
  job_id uuid references public.generation_jobs(id) on delete cascade not null,
  user_id uuid references public.users(id) on delete cascade not null,
  photo_url text not null,
  photo_path text not null,
  photo_index integer not null check (photo_index between 1 and 6),
  is_hd boolean default false,
  is_favorite boolean default false,
  download_count integer default 0,
  created_at timestamptz default now() not null
);

-- Indexes
create index idx_generation_jobs_user on public.generation_jobs(user_id);
create index idx_generation_jobs_status on public.generation_jobs(status);
create index idx_generated_photos_job on public.generated_photos(job_id);
create index idx_generated_photos_user on public.generated_photos(user_id);

alter table public.generation_jobs enable row level security;
alter table public.generated_photos enable row level security;
alter table public.photo_styles enable row level security;
```

### 6.4 Migration 004: rpc_functions.sql
```sql
-- RPC: get_user_data
create or replace function public.get_user_data(p_telegram_id bigint)
returns json language plpgsql security definer set search_path = '' as $$
declare
  v_user public.users;
  v_profile public.user_profiles;
  v_tokens public.token_balances;
begin
  select * into v_user from public.users where telegram_id = p_telegram_id;
  if not found then return null; end if;
  select * into v_profile from public.user_profiles where user_id = v_user.id;
  select * into v_tokens from public.token_balances where user_id = v_user.id;
  return json_build_object(
    'user', row_to_json(v_user),
    'profile', row_to_json(v_profile),
    'tokens', row_to_json(v_tokens)
  );
end;
$$;

-- RPC: upsert_user
create or replace function public.upsert_user(
  p_telegram_id bigint,
  p_username text,
  p_first_name text,
  p_last_name text,
  p_language_code text
) returns json language plpgsql security definer set search_path = '' as $$
declare
  v_user_id uuid;
begin
  insert into public.users (telegram_id, telegram_username, telegram_first_name, telegram_last_name, telegram_language_code, last_seen_at)
  values (p_telegram_id, p_username, p_first_name, p_last_name, p_language_code, now())
  on conflict (telegram_id) do update set
    telegram_username = excluded.telegram_username,
    telegram_first_name = excluded.telegram_first_name,
    telegram_last_name = excluded.telegram_last_name,
    last_seen_at = now(),
    updated_at = now()
  returning id into v_user_id;
  return public.get_user_data(p_telegram_id);
end;
$$;

-- RPC: complete_onboarding
create or replace function public.complete_onboarding(
  p_telegram_id bigint,
  p_onboarding_data jsonb
) returns boolean language plpgsql security definer set search_path = '' as $$
declare
  v_user_id uuid;
begin
  select id into v_user_id from public.users where telegram_id = p_telegram_id;
  if not found then return false; end if;
  update public.user_profiles set
    onboarding_completed = true,
    onboarding_data = p_onboarding_data,
    updated_at = now()
  where user_id = v_user_id;
  return true;
end;
$$;

-- RPC: use_token
create or replace function public.use_token(
  p_telegram_id bigint,
  p_description text default 'Photo generation'
) returns json language plpgsql security definer set search_path = '' as $$
declare
  v_user_id uuid;
  v_balance public.token_balances;
begin
  select id into v_user_id from public.users where telegram_id = p_telegram_id;
  select * into v_balance from public.token_balances where user_id = v_user_id;
  
  if v_balance.is_unlimited and (v_balance.unlimited_expires_at is null or v_balance.unlimited_expires_at > now()) then
    return json_build_object('success', true, 'balance', v_balance.balance, 'unlimited', true);
  end if;
  
  if v_balance.balance <= 0 then
    return json_build_object('success', false, 'error', 'Insufficient tokens');
  end if;
  
  update public.token_balances set
    balance = balance - 1,
    total_spent = total_spent + 1,
    updated_at = now()
  where user_id = v_user_id;
  
  insert into public.token_transactions (user_id, type, amount, balance_after, description)
  values (v_user_id, 'spend', -1, v_balance.balance - 1, p_description);
  
  return json_build_object('success', true, 'balance', v_balance.balance - 1, 'unlimited', false);
end;
$$;

-- RPC: add_tokens (ödeme sonrası)
create or replace function public.add_tokens(
  p_telegram_id bigint,
  p_package_id text,
  p_telegram_charge_id text
) returns json language plpgsql security definer set search_path = '' as $$
declare
  v_user_id uuid;
  v_package public.token_packages;
  v_new_balance integer;
begin
  select id into v_user_id from public.users where telegram_id = p_telegram_id;
  select * into v_package from public.token_packages where id = p_package_id;
  
  if v_package.is_unlimited then
    update public.token_balances set
      is_unlimited = true,
      unlimited_expires_at = now() + (v_package.duration_days || ' days')::interval,
      updated_at = now()
    where user_id = v_user_id;
  else
    update public.token_balances set
      balance = balance + v_package.tokens,
      total_earned = total_earned + v_package.tokens,
      updated_at = now()
    where user_id = v_user_id
    returning balance into v_new_balance;
    
    insert into public.token_transactions (user_id, type, amount, balance_after, description, reference_id, telegram_charge_id)
    values (v_user_id, 'purchase', v_package.tokens, v_new_balance, 'Package: ' || v_package.name, p_package_id, p_telegram_charge_id);
  end if;
  
  return json_build_object('success', true);
end;
$$;
```

### 6.5 Migration 005: rls_policies.sql
```sql
-- Users RLS
create policy "Users can read own data" on public.users
  for select using (telegram_id = (current_setting('app.telegram_id', true))::bigint);

-- User Profiles RLS  
create policy "Users can read own profile" on public.user_profiles
  for select using (user_id in (select id from public.users where telegram_id = (current_setting('app.telegram_id', true))::bigint));

-- Token Balances RLS
create policy "Users can read own balance" on public.token_balances
  for select using (user_id in (select id from public.users where telegram_id = (current_setting('app.telegram_id', true))::bigint));

-- Token Packages RLS (public read)
create policy "Anyone can read active packages" on public.token_packages
  for select using (is_active = true);

-- Photo Styles RLS (public read)
create policy "Anyone can read active styles" on public.photo_styles
  for select using (is_active = true);

-- Generation Jobs RLS
create policy "Users can read own jobs" on public.generation_jobs
  for select using (user_id in (select id from public.users where telegram_id = (current_setting('app.telegram_id', true))::bigint));

-- Generated Photos RLS
create policy "Users can read own photos" on public.generated_photos
  for select using (user_id in (select id from public.users where telegram_id = (current_setting('app.telegram_id', true))::bigint));
```

### 6.6 Seed: initial_packages.sql
```sql
-- Token Packages
insert into public.token_packages (id, name, name_tr, name_ru, description, description_tr, tokens, stars, is_popular, sort_order) values
  ('starter', 'Starter Pack', 'Başlangıç Paketi', 'Стартовый пакет', '+10 photo generations', '+10 fotoğraf üretimi', 10, 50, false, 1),
  ('pro', 'Pro Pack', 'Pro Paket', 'Про пакет', '+30 photo generations + HD quality', '+30 fotoğraf üretimi + HD kalite', 30, 120, true, 2),
  ('unlimited', 'Unlimited 30 Days', '30 Gün Sınırsız', '30 Дней безлимит', 'Unlimited generations for 30 days', '30 gün sınırsız üretim', null, 300, false, 3);

update public.token_packages set is_unlimited = true, duration_days = 30 where id = 'unlimited';

-- Photo Styles
insert into public.photo_styles (id, name, name_tr, name_ru, description, description_tr, prompt_template, sort_order) values
  ('natural', 'Natural', 'Doğal', 'Натуральный', 'Clean, natural look', 'Temiz, doğal görünüm',
   'professional dating profile photo, natural lighting, warm smile, casual style, photorealistic, high quality, {face_description}', 1),
  ('outdoor', 'Outdoor Adventure', 'Doğa Gezisi', 'Приключение', 'Active outdoor vibe', 'Aktif dış mekan havası',
   'professional dating profile photo, outdoor setting, adventure style, natural background, photorealistic, high quality, {face_description}', 2),
  ('social', 'Social Night Out', 'Sosyal Ortam', 'Светская жизнь', 'Social and confident look', 'Sosyal ve özgüvenli görünüm',
   'professional dating profile photo, evening social setting, confident smile, stylish outfit, bokeh background, photorealistic, {face_description}', 3),
  ('professional', 'Professional', 'Profesyonel', 'Профессиональный', 'Smart professional look', 'Akıllı profesyonel görünüm',
   'professional headshot photo, business casual outfit, neutral background, confident expression, studio lighting, photorealistic, {face_description}', 4),
  ('cozy', 'Cozy Home', 'Rahat Ev', 'Уютный дом', 'Warm and approachable', 'Sıcak ve ulaşılabilir',
   'casual home photo for dating profile, cozy indoor setting, warm lighting, relaxed smile, photorealistic, high quality, {face_description}', 5),
  ('travel', 'Travel Vibes', 'Seyahat Havası', 'Путешественник', 'Worldly and adventurous', 'Dünyagez ve maceracı',
   'travel lifestyle photo for dating profile, iconic location background, happy expression, casual travel outfit, photorealistic, {face_description}', 6);
```

---

## 7. 🤖 AI ENTEGRASYON MİMARİSİ

### 7.1 İşlem Akışı

```
Kullanıcı Fotoğrafı
        ↓
[1] Supabase Storage'a yükle (source_photos bucket)
        ↓
[2] Gemini Vision ile yüz analizi
    - Yüz özellikleri çıkarma (yaş tahmini, saç rengi, göz rengi, ten rengi, yüz şekli)
    - JSON yapılandırılmış çıktı
        ↓
[3] Seçilen stile göre 6 adet prompt üret
    - face_description placeholder'ını doldur
    - Her prompt benzersiz kompozisyon/sahne içermeli
        ↓
[4] Gemini Imagen API ile 6 fotoğraf üret (paralel)
    - Model: imagen-3.0-generate-001
    - Her fotoğraf: 1024x1024
        ↓
[5] Üretilen fotoğrafları Supabase Storage'a kaydet (generated_photos bucket)
        ↓
[6] generated_photos tablosuna kayıt
        ↓
[7] Frontend'e WebSocket veya polling ile bildir
        ↓
[8] Kullanıcıya fotoğrafları göster + indirme imkânı sun
```

### 7.2 gemini_service.py

```python
import google.generativeai as genai
from PIL import Image
import base64
import json
import asyncio
from typing import Optional

class GeminiService:
    def __init__(self, api_key: str):
        genai.configure(api_key=api_key)
        self.vision_model = genai.GenerativeModel("gemini-2.0-flash-lite")
        
    async def analyze_face(self, image_bytes: bytes) -> dict:
        """Yüklenen selfie'den yüz özelliklerini çıkar"""
        prompt = """Analyze this person's photo and extract the following in JSON format:
        {
          "age_estimate": "20s/30s/40s etc",
          "hair_color": "color description",
          "hair_style": "style description",
          "eye_color": "color",
          "skin_tone": "fair/medium/olive/dark",
          "face_shape": "oval/round/square/heart/oblong",
          "distinctive_features": ["list", "of", "features"],
          "gender_presentation": "masculine/feminine/androgynous",
          "overall_style": "casual/professional/alternative etc"
        }
        Return ONLY the JSON, no other text."""
        
        image_part = {"mime_type": "image/jpeg", "data": base64.b64encode(image_bytes).decode()}
        response = self.vision_model.generate_content([prompt, image_part])
        
        text = response.text.strip()
        text = text.replace("```json", "").replace("```", "").strip()
        return json.loads(text)
    
    async def generate_photo_prompts(self, analysis: dict, style: dict, count: int = 6) -> list[str]:
        """Yüz analizi ve stile göre prompt listesi üret"""
        face_desc = (
            f"{analysis.get('age_estimate', '')} person, "
            f"{analysis.get('hair_color', '')} {analysis.get('hair_style', '')} hair, "
            f"{analysis.get('eye_color', '')} eyes, "
            f"{analysis.get('skin_tone', '')} skin tone, "
            f"{analysis.get('face_shape', '')} face shape"
        )
        
        prompts = []
        base_template = style["prompt_template"].replace("{face_description}", face_desc)
        
        scene_variations = [
            "front facing portrait",
            "three-quarter angle, slight smile",
            "candid laugh, side lighting",
            "looking away confidently",
            "close-up portrait with depth of field",
            "full smile, warm golden hour light"
        ]
        
        for i in range(count):
            variation = scene_variations[i % len(scene_variations)]
            prompts.append(f"{base_template}, {variation}, DSLR quality, 85mm lens")
        
        return prompts
    
    async def generate_image(self, prompt: str) -> Optional[bytes]:
        """Tek bir fotoğraf üret"""
        try:
            imagen = genai.ImageGenerationModel("imagen-3.0-generate-001")
            result = imagen.generate_images(
                prompt=prompt,
                number_of_images=1,
                aspect_ratio="1:1",
                safety_filter_level="block_some"
            )
            if result.images:
                return result.images[0]._image_bytes
        except Exception as e:
            logger.error(f"Image generation failed: {e}")
            return None
    
    async def generate_all_photos(self, prompts: list[str]) -> list[Optional[bytes]]:
        """6 fotoğrafı paralel üret"""
        tasks = [self.generate_image(prompt) for prompt in prompts]
        return await asyncio.gather(*tasks)
```

### 7.3 image_service.py (Supabase Storage)

```python
from supabase import create_client
import uuid
from datetime import datetime

class ImageService:
    def __init__(self, supabase_url: str, service_role_key: str):
        self.supabase = create_client(supabase_url, service_role_key)
    
    async def upload_source_photo(self, user_id: str, image_bytes: bytes, mime_type: str = "image/jpeg") -> dict:
        """Kaynak selfie'yi Supabase Storage'a yükle"""
        path = f"{user_id}/{uuid.uuid4()}.jpg"
        result = self.supabase.storage.from_("source-photos").upload(
            path, image_bytes, {"content-type": mime_type, "upsert": "true"}
        )
        public_url = self.supabase.storage.from_("source-photos").get_public_url(path)
        return {"path": path, "url": public_url}
    
    async def upload_generated_photo(self, user_id: str, job_id: str, photo_bytes: bytes, index: int) -> dict:
        """Üretilen fotoğrafı kaydet"""
        path = f"{user_id}/{job_id}/photo_{index}.jpg"
        result = self.supabase.storage.from_("generated-photos").upload(
            path, photo_bytes, {"content-type": "image/jpeg", "upsert": "true"}
        )
        public_url = self.supabase.storage.from_("generated-photos").get_public_url(path)
        return {"path": path, "url": public_url}
```

---

## 8. 🔌 BACKEND API ENDPOINT'LERİ

### 8.1 core/config.py
```python
from pydantic_settings import BaseSettings
import sys

class Settings(BaseSettings):
    ENVIRONMENT: str = "development"
    DEBUG: bool = False
    
    # Zorunlu - başlangıçta validate edilir
    BOT_TOKEN: str
    SUPABASE_URL: str
    SUPABASE_ANON_KEY: str
    SUPABASE_SERVICE_ROLE_KEY: str
    GEMINI_API_KEY: str
    
    FRONTEND_URL: str = "http://localhost:5173"
    WEBHOOK_URL: str = ""
    BOT_USERNAME: str = ""
    
    class Config:
        case_sensitive = True
        env_file = ".env"
        extra = "ignore"

settings = Settings()

# Startup validation
required = ["BOT_TOKEN", "SUPABASE_URL", "SUPABASE_ANON_KEY", "SUPABASE_SERVICE_ROLE_KEY", "GEMINI_API_KEY"]
for field in required:
    if not getattr(settings, field):
        raise ValueError(f"Missing required env var: {field}")
```

### 8.2 main.py
```python
import logging
import sys
from contextlib import asynccontextmanager
from fastapi import FastAPI, Request, Response
from fastapi.middleware.cors import CORSMiddleware
from fastapi.responses import JSONResponse
from datetime import datetime

logging.basicConfig(level=logging.INFO, stream=sys.stdout, force=True,
    format="%(asctime)s %(levelname)s %(name)s: %(message)s")
logger = logging.getLogger(__name__)

@asynccontextmanager
async def lifespan(app: FastAPI):
    if settings.ENVIRONMENT == "production" and settings.WEBHOOK_URL:
        await telegram_service.set_webhook(
            f"{settings.WEBHOOK_URL}/webhook",
            allowed_updates=["message", "callback_query", "pre_checkout_query"]
        )
        logger.info("Webhook set")
    yield

app = FastAPI(title="TinderSnap API", lifespan=lifespan)

# Request body cache middleware (webhook için zorunlu)
class CacheRequestBodyMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        body = await request.body()
        async def cached_body(): return body
        request._body = body
        return await call_next(request)

app.add_middleware(CacheRequestBodyMiddleware)

app.add_middleware(CORSMiddleware,
    allow_origins=[settings.FRONTEND_URL] if settings.ENVIRONMENT == "production" else ["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"]
)

@app.exception_handler(Exception)
async def global_exception_handler(request, exc):
    logger.error(f"Unhandled exception: {exc}")
    return JSONResponse(status_code=500, content={"detail": "Sunucu hatası oluştu"})

@app.get("/")
async def root(): return {"app": "TinderSnap API", "version": "1.0.0"}

@app.get("/health")
async def health(): return {"status": "ok", "timestamp": datetime.utcnow().isoformat(), "environment": settings.ENVIRONMENT}

@app.get("/webhook")
async def webhook_health(): return {"status": "ok", "message": "Webhook endpoint is active"}

@app.post("/webhook")
async def telegram_webhook(request: Request):
    data = await request.json()
    # Sadece keys log'la, içerik değil
    logger.info(f"Webhook received keys: {list(data.keys())}")
    
    if "pre_checkout_query" in data:
        query = data["pre_checkout_query"]
        await telegram_service.answer_pre_checkout_query(query["id"], ok=True)
        return Response(status_code=200)
    
    if "message" in data and "successful_payment" in data["message"]:
        payment = data["message"]["successful_payment"]
        if payment.get("currency") == "XTR":
            await payment_service.process_successful_payment(
                invoice_payload=payment.get("invoice_payload"),
                charge_id=payment.get("telegram_payment_charge_id"),
                from_user=data["message"].get("from", {})
            )
        return Response(status_code=200)
    
    return Response(status_code=200)

app.include_router(api_router, prefix="/api")
```

### 8.3 api/endpoints/auth.py
```python
POST /api/auth/telegram
  Body: { init_data: str }
  Response: { success, user_id, telegram_id, session, message }
  - initData doğrulama (hash + auth_date 24 saat)
  - Supabase RPC upsert_user
  - Profil ve token bilgisi döndür

POST /api/auth/complete-onboarding
  Body: { telegram_id: int, onboarding_data: dict }
  Headers: X-Init-Data (doğrulama için)
  Response: { success, message }
  - initData doğrulama zorunlu
  - RPC complete_onboarding çağır
```

### 8.4 api/endpoints/generate.py
```python
POST /api/generate/upload
  Body: multipart/form-data { photo: File, style_id: str }
  Headers: X-Init-Data
  Response: { success, job_id, message }
  - initData doğrulama
  - Token bakiyesi kontrol et (use_token RPC)
  - Fotoğrafı validate et (format, boyut max 10MB, yüz var mı?)
  - Supabase Storage'a yükle
  - generation_jobs tablosuna 'pending' kayıt oluştur
  - Background task olarak generate_photos başlat
  - job_id döndür

GET /api/generate/status/{job_id}
  Headers: X-Init-Data
  Response: { job_id, status, progress, photos: [], error }
  - İşin durumunu döndür (polling için)
  - Tamamlandıysa generated_photos listesi

GET /api/generate/styles
  Response: { styles: [StyleObject] }
  - Supabase photo_styles tablosundan aktif stiller
  - Kimlik doğrulama gerekmez

GET /api/generate/history
  Headers: X-Init-Data
  Query: ?page=1&limit=10
  Response: { jobs: [...], total, page, has_more }
  - Kullanıcının önceki üretimlerini döndür
```

### 8.5 api/endpoints/payment.py
```python
GET /api/payment/packages
  Response: { packages: [...] }
  - Aktif token paketlerini döndür

POST /api/payment/create-invoice
  Body: { package_id: str, user_id: int }
  Headers: X-Init-Data
  Response: { success, invoice_url, transaction_id }
  - initData doğrulama
  - pending_transactions'a kayıt oluştur
  - Telegram Stars invoice linki oluştur
  - invoice_url döndür
```

### 8.6 Background Task: generate_photos
```python
async def generate_photos_task(job_id: str, user_id: str, source_photo_url: str, style_id: str):
    try:
        # 1. Job'u 'analyzing' olarak güncelle
        await supabase_service.update_job_status(job_id, 'analyzing')
        
        # 2. Kaynak fotoğrafı indir
        image_bytes = await download_image(source_photo_url)
        
        # 3. Gemini ile yüz analizi
        analysis = await gemini_service.analyze_face(image_bytes)
        await supabase_service.update_job_analysis(job_id, analysis)
        
        # 4. Job'u 'generating' olarak güncelle
        await supabase_service.update_job_status(job_id, 'generating')
        
        # 5. Style'ı al ve promptları üret
        style = await supabase_service.get_style(style_id)
        prompts = await gemini_service.generate_photo_prompts(analysis, style, count=6)
        
        # 6. 6 fotoğrafı paralel üret
        generated_bytes_list = await gemini_service.generate_all_photos(prompts)
        
        # 7. Üretilen fotoğrafları Storage'a yükle ve DB'ye kaydet
        for i, photo_bytes in enumerate(generated_bytes_list):
            if photo_bytes:
                upload_result = await image_service.upload_generated_photo(user_id, job_id, photo_bytes, i+1)
                await supabase_service.create_generated_photo(job_id, user_id, upload_result, i+1)
        
        # 8. Job'u 'completed' olarak güncelle
        await supabase_service.update_job_status(job_id, 'completed')
        
        # 9. Kullanıcıya Telegram bildirimi gönder
        user = await supabase_service.get_user_by_id(user_id)
        await telegram_service.send_message(
            chat_id=user['telegram_id'],
            text="✅ Tinder fotoğrafların hazır! Uygulamaya geri dön."
        )
    except Exception as e:
        logger.error(f"Generation failed for job {job_id}: {e}")
        await supabase_service.update_job_status(job_id, 'failed', str(e))
```

---

## 9. 🖥️ FRONTEND SAYFALARI VE AKIŞLAR

### 9.1 Provider Hiyerarşisi (main.tsx)
```tsx
import './lib/i18n'; // İlk import

<TelegramProvider>
  <AuthProvider>
    <TokenProvider>
      <Router>
        <App />
      </Router>
    </TokenProvider>
  </AuthProvider>
</TelegramProvider>
```

### 9.2 App.tsx Routing
```tsx
const HomePage = lazy(() => import('./pages/HomePage'));
const OnboardingPage = lazy(() => import('./pages/OnboardingPage'));
const GeneratePage = lazy(() => import('./pages/GeneratePage'));
const ResultPage = lazy(() => import('./pages/ResultPage'));
const HistoryPage = lazy(() => import('./pages/HistoryPage'));
const TokensPage = lazy(() => import('./pages/TokensPage'));
const SettingsPage = lazy(() => import('./pages/SettingsPage'));

const hideNavigation = ['/onboarding', '/generate', '/result'];

<Routes>
  <Route path="/" element={<HomePage />} />
  <Route path="/onboarding" element={<OnboardingPage />} />
  <Route path="/generate" element={<ProtectedRoute><GeneratePage /></ProtectedRoute>} />
  <Route path="/result/:jobId" element={<ProtectedRoute><ResultPage /></ProtectedRoute>} />
  <Route path="/history" element={<ProtectedRoute><HistoryPage /></ProtectedRoute>} />
  <Route path="/tokens" element={<ProtectedRoute><TokensPage /></ProtectedRoute>} />
  <Route path="/settings" element={<ProtectedRoute><SettingsPage /></ProtectedRoute>} />
</Routes>
```

### 9.3 Sayfa Detayları

#### HomePage.tsx
- Telegram profil karşılama ("Merhaba, {first_name}! 👋")
- Token bakiyesi widget (sağ üst)
- "Fotoğraf Oluştur" ana CTA butonu (Telegram MainButton)
- Son 3 üretim önizlemesi (History kısayolu)
- Style galeri kartları

#### OnboardingPage.tsx (Multi-step)
- **Adım 1:** Karşılama + uygulama tanıtımı (animasyonlu)
- **Adım 2:** Kişilik tercihi seçimi (hangi stil en çok hangisine uyuyor)
- **Adım 3:** Bildirim izni (opsiyonel)
- **Adım 4:** Tamamlandı animasyonu
- `POST /api/auth/complete-onboarding` ile kayıt
- i18n zorunlu tüm metinlerde

#### GeneratePage.tsx (Ana Özellik)
1. **Fotoğraf Yükleme Aşaması:**
   - Camera Capture (mobil kamera) veya Galeri Yükleme
   - Drag & Drop destekli (desktop)
   - Dosya format validasyonu (jpg, png, webp, max 10MB)
   - Yüz algılama önizlemesi
   - Token bakiyesi kontrolü (yetersizse token sayfasına yönlendir)

2. **Stil Seçimi Aşaması:**
   - Yatay scroll'lu stil kartları (6 stil)
   - Her kartta önizleme görseli + isim + açıklama
   - Seçilen stil highlight

3. **Üretim İlerleme Aşaması:**
   - Animasyonlu progress bar
   - Aşama göstergesi: "Analiz ediliyor... → Üretiliyor... → Tamamlandı"
   - Polling: `GET /api/generate/status/{jobId}` her 3 saniyede
   - Tamamlanınca `/result/:jobId` yönlendir

#### ResultPage.tsx
- 6 fotoğraf grid (3x2)
- Her fotoğraf tıklanınca tam ekran görünüm
- İndirme butonu (her fotoğraf için ayrı)
- Favori işaretleme
- "Yeni Üretim Yap" CTA
- Paylaşma butonu (Telegram'a gönder)

#### HistoryPage.tsx
- Sayfalanmış üretim geçmişi (10'ar)
- Her üretim kartında: tarih, stil, 6 küçük thumbnail
- Skeleton loading
- Empty state (ilk kullanım için CTA)

#### TokensPage.tsx (Ödeme Sayfası)
- Mevcut bakiye gösterimi
- 3 paket kartı (Starter, Pro, Unlimited)
- "En Popüler" rozeti (Pro)
- Satın al → `POST /api/payment/create-invoice` → `WebApp.openInvoice()`
- Ödeme callback: paid/cancelled/failed toast
- paid: Token bakiyesini yenile

#### SettingsPage.tsx (Zorunlu Bölümler)
- Telegram profil kartı (avatar, isim, @username)
- Mevcut token bakiyesi + "Token Satın Al" linki
- Dil değiştirme modal (EN / TR / RU)
- Tercih edilen stil değiştirme
- Bildirim ayarları toggle
- Gizlilik Politikası linki
- Kullanım Koşulları linki
- Çıkış butonu (auth state temizle)
- Uygulama versiyonu (footer)

---

## 10. 🎨 UI/UX TASARIM SİSTEMİ

### 10.1 Renk Paleti
```javascript
// tailwind.config.js
colors: {
  'app-black': '#0A0A0A',
  'app-dark': '#141414',
  'app-card': '#1C1C1E',
  'app-gray': '#2C2C2E',
  'app-border': '#3A3A3C',
  'app-primary': {
    DEFAULT: '#FF4D6D',       // Tinder kırmızı-pembe
    light: '#FF6B84',
    dark: '#E63957',
    muted: 'rgba(255,77,109,0.15)',
    glow: 'rgba(255,77,109,0.35)'
  },
  'app-secondary': {
    DEFAULT: '#FF9F43',        // Turuncu vurgu
    muted: 'rgba(255,159,67,0.15)'
  },
  'app-white': '#FFFFFF',
  'app-text': {
    primary: '#FFFFFF',
    secondary: '#8E8E93',
    muted: '#636366'
  }
}
```

### 10.2 Tipografi
```javascript
fontFamily: {
  sans: ['Inter var', 'SF Pro Display', '-apple-system', 'sans-serif'],
}
```

### 10.3 Animasyonlar
- Sayfa geçişleri: Framer Motion `AnimatePresence`
- Fotoğraf üretim progress: Pulse animasyonu + glow efekti
- Stil seçimi: Spring animasyonu ile scale
- Result grid: Staggered fade-in (her kart 100ms gecikmeli)
- Upload alanı: Drag-over border animasyonu

### 10.4 Dil Desteği
- English (zorunlu, fallback)
- Turkish
- Russian

---

## 11. 🔒 GÜVENLİK GEREKSİNİMLERİ

### 11.1 Backend Güvenlik
- Her korumalı endpoint'te `initData` hash doğrulaması (HMAC-SHA256)
- `auth_date` 24 saat sınırı
- DEBUG modda doğrulama atlanabilir (sadece development)
- initData ham string ASLA loglanmaz
- CORS production'da sadece `FRONTEND_URL`

### 11.2 Dosya Güvenliği
- Yüklenen dosya MIME type kontrolü (image/jpeg, image/png, image/webp)
- Max boyut: 10MB
- Dosya adı sanitizasyonu (UUID kullan)
- Supabase Storage bucket policy: private (sadece authenticated)

### 11.3 Token Güvenliği
- Token harcama işlemleri atomik RPC ile
- Negatif bakiye check constraint
- Her üretim job'u kullanıcıyla eşleştirilir (user_id FK)

---

## 12. 📦 ENVIRONMENT VARIABLES

### 12.1 Frontend (.env.example)
```env
VITE_SUPABASE_URL=https://xxx.supabase.co
VITE_SUPABASE_ANON_KEY=xxx
VITE_BACKEND_URL=https://your-backend.railway.app
VITE_DEBUG_MODE=false
```

### 12.2 Backend (.env.example)
```env
ENVIRONMENT=production
DEBUG=false
BOT_TOKEN=xxx
TELEGRAM_BOT_TOKEN=xxx
WEBHOOK_URL=https://your-backend.railway.app
SUPABASE_URL=https://xxx.supabase.co
SUPABASE_ANON_KEY=xxx
SUPABASE_SERVICE_ROLE_KEY=xxx
FRONTEND_URL=https://your-frontend.vercel.app
BOT_USERNAME=your_bot_username
GEMINI_API_KEY=xxx
```

---

## 13. 🚀 DEPLOYMENT

### 13.1 Backend Dosyaları
```
# runtime.txt
python-3.10.15

# Procfile
web: uvicorn main:app --host 0.0.0.0 --port $PORT

# railway.json
{
  "build": { "builder": "NIXPACKS" },
  "deploy": {
    "startCommand": "uvicorn main:app --host 0.0.0.0 --port $PORT",
    "restartPolicyType": "ON_FAILURE",
    "healthcheckPath": "/health"
  }
}

# nixpacks.toml
[phases.setup]
nixPkgs = ["python310", "pip"]
[phases.install]
cmds = ["pip install -r requirements.txt"]
[start]
cmd = "uvicorn main:app --host 0.0.0.0 --port $PORT"
```

### 13.2 Supabase Storage Buckets
1. **source-photos** (private) — kullanıcı selfie'leri
2. **generated-photos** (public) — üretilen fotoğraflar

### 13.3 Telegram Bot Setup
1. BotFather'dan bot oluştur
2. WebApp URL'i ekle (Menu Button)
3. Payments aktifle (Stars için)
4. Webhook otomatik ayarlanır (backend lifespan'de)

---

## 14. ✅ 100+ TODO LİSTESİ (FAZLAR HALINDE)

### FAZA 1: Proje Kurulumu (1–10)
```
1.  [ ] Git repo oluştur; frontend/ ve backend/ klasörleri kur
2.  [ ] Frontend: Vite + React + TypeScript scaffolding
3.  [ ] Frontend: Tüm npm bağımlılıklarını yükle (package.json)
4.  [ ] Frontend: Tailwind CSS konfigürasyonu (renk paleti, fontlar, animasyonlar)
5.  [ ] Frontend: index.html (telegram-web-app.js, viewport meta, safe-area CSS)
6.  [ ] Backend: Python virtual env + requirements.txt
7.  [ ] Backend: Klasör yapısı (api/, core/, services/)
8.  [ ] Backend: .env.example (frontend + backend)
9.  [ ] Supabase proje oluştur; URL ve key'leri al
10. [ ] Supabase Storage bucket'ları oluştur (source-photos: private, generated-photos: public)
```

### FAZA 2: Veritabanı (11–20)
```
11. [ ] Migration 001: users + user_profiles tabloları
12. [ ] Migration 002: token_balances + token_packages + token_transactions + pending_transactions
13. [ ] Migration 003: photo_styles + generation_jobs + generated_photos
14. [ ] Migration 004: Tüm RPC fonksiyonları (get_user_data, upsert_user, complete_onboarding, use_token, add_tokens)
15. [ ] Migration 005: RLS politikaları (tüm tablolar için)
16. [ ] Auto-trigger: Yeni kullanıcı → user_profile + token_balance otomatik oluştur
17. [ ] Seed: token_packages (3 paket)
18. [ ] Seed: photo_styles (6 stil + prompt şablonları)
19. [ ] Tüm index'leri oluştur (telegram_id, user_id, job status vb.)
20. [ ] Supabase Storage policy'lerini ayarla
```

### FAZA 3: Backend Core (21–30)
```
21. [ ] core/config.py – pydantic-settings, zorunlu env validasyonu
22. [ ] core/schemas.py – Tüm Pydantic modelleri (Request/Response)
23. [ ] main.py – logging, CORS, lifespan, exception handler
24. [ ] main.py – Request body cache middleware (webhook için zorunlu)
25. [ ] main.py – GET /, GET /health, GET /webhook, POST /webhook
26. [ ] api/router.py – Alt router'ları prefix/tags ile birleştir
27. [ ] services/supabase_service.py – Admin client, tüm DB operasyonları
28. [ ] services/telegram_service.py – Bot API çağrıları (send_message, answer_pre_checkout, set_webhook)
29. [ ] services/payment_service.py – Invoice oluşturma, ödeme işleme
30. [ ] POST /webhook – pre_checkout_query + successful_payment işleme
```

### FAZA 4: Auth Endpoint'leri (31–38)
```
31. [ ] validate_telegram_webapp_data() – HMAC-SHA256, auth_date 24 saat
32. [ ] DEBUG modda doğrulama bypass
33. [ ] api/endpoints/auth.py – POST /api/auth/telegram
34. [ ] Upsert user (RPC upsert_user çağır)
35. [ ] Profil + token bilgisi döndür
36. [ ] api/endpoints/auth.py – POST /api/auth/complete-onboarding
37. [ ] complete_onboarding RPC çağır
38. [ ] initData doğrulama dependency (tüm korumalı endpoint'ler için tekrar kullanılabilir)
```

### FAZA 5: AI ve Fotoğraf Üretim (39–52)
```
39. [ ] services/gemini_service.py – genai yapılandırma, model init
40. [ ] analyze_face() – Vision model ile yüz analizi, JSON çıktı
41. [ ] generate_photo_prompts() – 6 benzersiz prompt üretme
42. [ ] generate_image() – Imagen API ile tek fotoğraf
43. [ ] generate_all_photos() – 6 paralel üretim (asyncio.gather)
44. [ ] services/image_service.py – Supabase Storage upload/download
45. [ ] upload_source_photo() + upload_generated_photo()
46. [ ] api/endpoints/generate.py – GET /api/generate/styles
47. [ ] api/endpoints/generate.py – POST /api/generate/upload
48. [ ] Dosya validasyonu (MIME, boyut, yüz var mı?)
49. [ ] Token kullanımı (use_token RPC)
50. [ ] Background task: generate_photos_task() – tam akış
51. [ ] api/endpoints/generate.py – GET /api/generate/status/{job_id}
52. [ ] api/endpoints/generate.py – GET /api/generate/history (sayfalanmış)
```

### FAZA 6: Ödeme Sistemi (53–60)
```
53. [ ] api/endpoints/payment.py – GET /api/payment/packages
54. [ ] api/endpoints/payment.py – POST /api/payment/create-invoice
55. [ ] pending_transactions tablosuna kayıt
56. [ ] Telegram Stars invoice oluşturma (currency: XTR, provider_token: "")
57. [ ] Invoice payload: sadece transaction_id (UUID, max 128 byte)
58. [ ] POST /webhook – successful_payment işleme
59. [ ] add_tokens RPC çağır (ödeme başarılı)
60. [ ] Kullanıcıya Telegram bildirimi (ödeme başarılı mesajı)
```

### FAZA 7: Frontend Core (61–70)
```
61. [ ] lib/i18n.ts – Backend, LanguageDetector, Telegram dil detector, fallback: 'en'
62. [ ] public/locales/en/ + tr/ + ru/ – common, auth, navigation, errors, onboarding, generate, settings, tokens
63. [ ] lib/supabase.ts – Supabase client (anon key)
64. [ ] lib/api.ts – apiFetch helper (base URL, auth headers, error handling)
65. [ ] contexts/TelegramContext.tsx – WebApp.ready(), expand(), tüm metodlar
66. [ ] contexts/AuthContext.tsx – initData ile /api/auth/telegram, user state
67. [ ] contexts/TokenContext.tsx – token bakiyesi, paketler, satın alma
68. [ ] main.tsx – Provider hiyerarşisi (Telegram → Auth → Token → Router → App)
69. [ ] App.tsx – Routes, lazy loading, Suspense, ErrorBoundary, BottomNavigation
70. [ ] ProtectedRoute wrapper – onboarding_completed kontrolü + yönlendirme
```

### FAZA 8: UI Komponentleri (71–82)
```
71. [ ] ErrorBoundary.tsx
72. [ ] LoadingSpinner.tsx + FullScreenLoader.tsx
73. [ ] PageTransition.tsx (Framer Motion AnimatePresence)
74. [ ] BottomNavigation.tsx – safe-area-inset-bottom, hideNavigation listesi
75. [ ] Header.tsx – title, BackButton, rightAction
76. [ ] Button.tsx – primary, secondary, danger, loading state varyantları
77. [ ] Input.tsx – text, file, error state
78. [ ] Modal.tsx + LanguageModal.tsx
79. [ ] Card.tsx + SkeletonLoader.tsx
80. [ ] EmptyState.tsx + ErrorState.tsx
81. [ ] TelegramProfileCard.tsx – avatar, ad, @username
82. [ ] TokenBalance.tsx – bakiye widget (üst bar veya ayarlar)
```

### FAZA 9: Sayfalar (83–95)
```
83. [ ] OnboardingPage.tsx – 4 adım, framer motion, i18n, complete-onboarding API
84. [ ] HomePage.tsx – karşılama, son üretimler, CTA, Telegram MainButton
85. [ ] GeneratePage.tsx – Aşama 1: Fotoğraf yükleme (camera + galeri)
86. [ ] GeneratePage.tsx – Aşama 2: Stil seçimi (6 stil kartı)
87. [ ] GeneratePage.tsx – Aşama 3: Üretim progress (polling + animasyon)
88. [ ] PhotoUploader.tsx + CameraCapture.tsx komponentleri
89. [ ] StyleSelector.tsx + GenerationProgress.tsx komponentleri
90. [ ] ResultPage.tsx – 6 fotoğraf grid, indirme, favori, paylaş
91. [ ] GeneratedPhotoGrid.tsx + GeneratedPhotoCard.tsx + tam ekran modal
92. [ ] HistoryPage.tsx – sayfalanmış geçmiş, skeleton, empty state
93. [ ] TokensPage.tsx – 3 paket kartı, satın al, openInvoice, callback
94. [ ] PackageCard.tsx + PurchaseModal.tsx
95. [ ] SettingsPage.tsx – Zorunlu tüm bölümler (profil, bakiye, dil, gizlilik, çıkış, versiyon)
```

### FAZA 10: i18n (96–100)
```
96.  [ ] common.json (en/tr/ru) – genel metinler
97.  [ ] navigation.json – tab isimleri, header başlıkları
98.  [ ] errors.json – hata mesajları
99.  [ ] generate.json + onboarding.json + settings.json + tokens.json
100. [ ] Dil değiştirme: i18n.changeLanguage() + backend/Supabase tercih güncelleme
```

### FAZA 11: Test ve Deployment (101–110)
```
101. [ ] Backend: Tüm endpoint'leri Postman/curl ile test et
102. [ ] AI pipeline: Gerçek fotoğraf ile uçtan uca test
103. [ ] Ödeme: Telegram Stars test ödemesi yap
104. [ ] Token sistemi: use_token ve add_tokens RPC atomik test
105. [ ] Frontend: Telegram WebApp'te mobil test
106. [ ] Frontend: i18n dil değiştirme testi (EN/TR/RU)
107. [ ] Backend deploy: Railway (env vars, webhook URL)
108. [ ] Frontend deploy: Vercel (env vars, build)
109. [ ] BotFather: WebApp URL güncelle, Stars ödeme aktifle
110. [ ] Production webhook doğrula (GET /webhook health check)
```

---

## 15. 🚫 KRİTİK KURALLAR

| Konu | Kural |
|------|-------|
| **Mock Data** | ASLA kullanma; sadece Supabase ve backend API |
| **Gemini Model** | `gemini-2.0-flash-lite` (analiz), `imagen-3.0-generate-001` (üretim) |
| **Webhook** | Her zaman 200 dön; body loglanmaz; sadece production'da kur |
| **Stars Ödeme** | Currency: `XTR`; provider_token: `""`; payload max 128 byte |
| **InitData** | Hash + auth_date 24 saat; her korumalı endpoint'te zorunlu |
| **CORS** | Production'da `["*"]` yok; sadece FRONTEND_URL |
| **Deep Link** | Max 64 karakter |
| **Token İşlem** | Atomik RPC ile; negatif bakiye yok |
| **Dosya Upload** | UUID ile path; MIME kontrolü; max 10MB |
| **Loglama** | initData, token, payment payload içeriği loglanmaz |
| **Provider Sırası** | Telegram → Auth → Token → Router → App |
| **Locales Path** | Sadece `public/locales/{{lng}}/{{ns}}.json` |
| **Onboarding Guard** | `onboarding_completed === false` → `/onboarding` yönlendir |
| **API Hata** | Her çağrıda try/catch; kullanıcıya toast; sessiz fail yok |
| **Request Body** | POST /webhook için cache middleware zorunlu |

---

*Bu PRD, proje kuralları dosyasındaki tüm zorunlu standartlar ve kısıtlamalar gözetilerek hazırlanmıştır. Geliştirme sürecinde bu döküman referans alınarak 110 todo adım adım tamamlanmalıdır.*
