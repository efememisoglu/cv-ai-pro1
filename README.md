# CV AI PRO

Yapay zeka destekli profesyonel CV oluşturucu — Next.js, Node.js, MongoDB ve OpenAI GPT-4 Turbo.

![CV AI PRO](assets/cv2-0be5c672-4d41-40b4-a752-6c04462958db.png)

## Özellikler

### Temel
- **Landing page** — Tasarıma uygun koyu hero, özellikler, şablonlar, mobil mockup
- **JWT kimlik doğrulama** — Kayıt, giriş, bcrypt şifreleme
- **CV oluşturucu** — 5 adımlı form + canlı önizleme
- **8 şablon** — 4 ücretsiz + 4 premium
- **TR/EN arayüz** — Dil seçici
- **CV paylaşım linki** — Herkese açık `/cv/[id]`
- **OpenAI** — Metin iyileştirme, ön yazı
- **PDF indirme** — Tarayıcı yazdırma

### Premium / Orta seviye
- **Word (.docx) indirme** — Premium
- **Profil fotoğrafı** — Yükleme (max 2MB)
- **ATS analizi** — Premium
- **AI beceri önerisi** — Premium
- **Stripe ödeme** — Tek seferlik Premium
- **SendGrid e-posta** — Hoş geldin + şifre sıfırlama
- **Şifremi unuttum** — `/forgot-password` → `/reset-password`

### Güvenlik
- Rate limiting, CORS, input validation

## Kurulum

### Gereksinimler

- Node.js 18+
- MongoDB (yerel veya Atlas)
- OpenAI API anahtarı

### 1. Ortam değişkenleri

```bash
# Backend
cp backend/.env.example backend/.env

# Frontend
cp frontend/.env.example frontend/.env.local
```

`backend/.env` dosyasını düzenleyin:

```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/cv-ai-pro
JWT_SECRET=guclu-bir-gizli-anahtar
OPENAI_API_KEY=sk-...
FRONTEND_URL=http://localhost:3000
BACKEND_URL=http://localhost:5000

# Opsiyonel
SENDGRID_API_KEY=
SENDGRID_FROM=noreply@yourdomain.com
STRIPE_SECRET_KEY=sk_test_...
STRIPE_PRICE_ID=price_...
STRIPE_WEBHOOK_SECRET=whsec_...
```

`frontend/.env.local`:
```env
NEXT_PUBLIC_API_URL=http://localhost:5000/api
NEXT_PUBLIC_BACKEND_URL=http://localhost:5000
```

### Stripe webhook (yerel test)
```bash
stripe listen --forward-to localhost:5000/api/stripe/webhook
```
Çıkan `whsec_...` değerini `STRIPE_WEBHOOK_SECRET` olarak kaydedin.

### Premium test (Stripe olmadan)
MongoDB Atlas → `users` koleksiyonunda `premium: true` yapın.
```

### 2. Bağımlılıklar

```bash
cd backend && npm install
cd ../frontend && npm install
```

### 3. Çalıştırma

İki terminalde:

```bash
# Terminal 1 — API
cd backend && npm run dev

# Terminal 2 — Web
cd frontend && npm run dev
```

- Frontend: http://localhost:3000
- API: http://localhost:5000/api/health

## Proje yapısı

```
cv-ai-pro/
├── backend/          # Express + MongoDB + OpenAI
│   └── src/
│       ├── models/   # User, CV
│       ├── routes/   # auth, cv, ai
│       └── services/ # openai.js
└── frontend/         # Next.js 14 + Tailwind
    └── src/
        ├── app/      # Sayfalar
        ├── components/
        └── store/    # Zustand CV state
```

## API uçları

| Method | Endpoint | Açıklama |
|--------|----------|----------|
| POST | `/api/auth/register` | Kayıt |
| POST | `/api/auth/login` | Giriş |
| GET | `/api/cv` | CV listesi |
| POST | `/api/cv` | CV oluştur |
| POST | `/api/ai/improve` | Metin iyileştir |
| POST | `/api/ai/suggest-skills` | Beceri öner |
| POST | `/api/ai/ats-analyze` | ATS analizi |

## Teknolojiler

Next.js · React · Tailwind CSS · Node.js · Express · MongoDB · OpenAI API · JWT · Zustand
