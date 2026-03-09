# TinderSnap — AI Tinder Profil Fotoğrafı Üreticisi

Telegram WebApp ile çalışan, yapay zeka destekli Tinder profil fotoğrafı üreticisi.

## Stack

- **Frontend:** React 18, TypeScript, Vite, Tailwind CSS, Telegram WebApp SDK
- **Backend:** FastAPI, Python 3.10+
- **DB & Storage:** Supabase (PostgreSQL, Storage)
- **AI:** Google Gemini (analiz + Imagen)

## Hızlı Başlangıç

### 1. Supabase Proje Oluşturma (Todo 9)

1. [Supabase](https://supabase.com) hesabı açın.
2. **New Project** ile yeni proje oluşturun (region seçin, şifre belirleyin).
3. **Settings > API** bölümünden şunları alın:
   - `Project URL` → `SUPABASE_URL` ve `VITE_SUPABASE_URL`
   - `anon public` key → `SUPABASE_ANON_KEY` ve `VITE_SUPABASE_ANON_KEY`
   - `service_role` key → `SUPABASE_SERVICE_ROLE_KEY` (sadece backend, gizli tutun)

### 2. Supabase Storage Bucket'ları (Todo 10)

1. Dashboard’da **Storage** > **New bucket**.
2. **source-photos**: Adı `source-photos`, **Private** seçin (kullanıcı selfie’leri).
3. **generated-photos**: Adı `generated-photos`, **Public** seçin (üretilen fotoğraflar).
4. İsteğe bağlı: **Policies** ile `source-photos` için kullanıcı bazlı upload/read kısıtları ekleyin (backend service role ile de yönetilebilir).

### 3. Veritabanı Migration’ları

Supabase **SQL Editor**’da sırayla çalıştırın:

- `frontend/supabase/migrations/001_create_users_and_profiles.sql`
- `frontend/supabase/migrations/002_create_token_system.sql`
- `frontend/supabase/migrations/003_create_photo_tables.sql`
- `frontend/supabase/migrations/004_create_rpc_functions.sql`
- `frontend/supabase/migrations/005_create_rls_policies.sql`
- `frontend/supabase/seed/initial_packages.sql` (token paketleri + foto stilleri)

### 4. Ortam Değişkenleri

- Kök `.env.example` dosyasını referans alın.
- **Frontend:** `frontend/.env` içine `VITE_*` değişkenlerini koyun.
- **Backend:** `backend/.env` içine `BOT_TOKEN`, `SUPABASE_*`, `GEMINI_API_KEY` vb. koyun.

### 5. Çalıştırma

```bash
# Frontend
cd frontend && npm install && npm run dev

# Backend
cd backend && python -m venv venv && source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

## Proje Yapısı

- `frontend/` — React + Vite + Telegram WebApp
- `backend/` — FastAPI, auth, payment, generate, webhook
- `frontend/supabase/` — migrations + seed

## Deployment

### Backend (Railway)

1. [Railway](https://railway.app) projesi oluşturun, **Deploy from GitHub** veya **Nixpacks** ile `backend/` klasörünü bağlayın.
2. **Variables** ekleyin: `BOT_TOKEN`, `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`, `GEMINI_API_KEY`, `WEBHOOK_URL` (backend’in public URL’i, örn. `https://xxx.railway.app`), `FRONTEND_URL`, `ENVIRONMENT=production`.
3. Deploy sonrası **Settings** > **Networking** > **Generate domain** ile public URL alın; bu URL’i `WEBHOOK_URL` ve BotFather’da kullanın.
4. `Procfile`, `runtime.txt`, `railway.json` ve `nixpacks.toml` backend klasöründe; frontend için `railway.json` ve `nixpacks.toml` frontend klasöründe hazırdır. Uygulama başlarken production’da webhook otomatik ayarlanır.

### Frontend (Railway)

1. Railway’da **ayrı bir servis** oluşturun (aynı projede veya yeni proje); **Deploy from GitHub** ile `frontend/` klasörünü **root** olarak bağlayın (veya monorepo ise **Root Directory** → `frontend`).
2. **Variables** ekleyin: `VITE_BACKEND_URL` (backend’in Railway URL’i), `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`; isteğe `VITE_PRIVACY_URL`, `VITE_TERMS_URL`, `VITE_DEBUG_MODE=false`.
3. Build: `npm ci && npm run build`; Start: `npx serve dist -s -l $PORT` (SPA fallback). `railway.json` ve `nixpacks.toml` frontend klasöründe hazırdır.
4. **Settings** > **Networking** > **Generate domain** ile frontend URL’ini alın; BotFather’da Web App URL olarak bu adresi verin.

### BotFather ve WebApp

1. [@BotFather](https://t.me/BotFather) → bot oluşturun veya mevcut botu seçin.
2. **Bot Settings** → **Menu Button** veya **Configure Web App** → Web App URL: Railway frontend servisinizin URL’i (örn. `https://xxx.railway.app`).
3. **Payments** → Telegram Stars (XTR) ödemeyi aktifleştirin; gerekirse **Provider** ekleyin.
4. Webhook, backend production’da açıldığında `WEBHOOK_URL` ile otomatik set edilir (`POST /webhook`). Gerekirse **setWebhook** manuel çağrılabilir.

### Webhook ve Health Check

- **GET /webhook** — Health check; `{"status": "ok", "message": "Webhook endpoint is active"}` döner. Production’da erişilebilir olmalı.
- **GET /health** — Uygulama durumu (timestamp, environment).
- **POST /webhook** — Telegram update’leri; her durumda **200** dönülmeli.

### Backend API Test (curl)

```bash
# Health
curl -s https://YOUR_BACKEND_URL/health

# Webhook health
curl -s https://YOUR_BACKEND_URL/webhook

# Paketler (auth yok)
curl -s https://YOUR_BACKEND_URL/api/payment/packages

# Stiller (auth yok)
curl -s https://YOUR_BACKEND_URL/api/generate/styles
```

Korumalı endpoint’ler (`/api/auth/telegram`, `/api/auth/preferences`, `/api/generate/upload`, `/api/generate/status/:id`, `/api/generate/history`, `/api/payment/create-invoice`) için geçerli Telegram WebApp `X-Init-Data` header’ı gerekir; bunlar WebApp veya Postman ile test edilir.

## PRD ve Kurallar

Detaylı gereksinimler ve 110 todo listesi için `prd.md` ve `.cursor/rules/project-rules.mdc` dosyalarına bakın.
