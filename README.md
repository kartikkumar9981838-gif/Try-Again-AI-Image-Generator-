# VisionMint AI 🎨

A production-ready SaaS AI Image Generator built with **Next.js 14**, **Supabase**, and **Hugging Face** — 100% free to deploy.

![VisionMint AI](https://img.shields.io/badge/Next.js-14-black?style=for-the-badge&logo=next.js)
![Supabase](https://img.shields.io/badge/Supabase-Free-green?style=for-the-badge&logo=supabase)
![Hugging Face](https://img.shields.io/badge/HuggingFace-Free_API-yellow?style=for-the-badge)
![Vercel](https://img.shields.io/badge/Deploy-Vercel-black?style=for-the-badge&logo=vercel)

## ✨ Features

- 🎨 **AI Image Generation** — Stable Diffusion XL via Hugging Face
- 🎭 **8 Art Style Presets** — Photorealistic, Anime, Cyberpunk, Oil Painting, and more
- 📐 **4 Aspect Ratios** — Square, Landscape, Portrait, Standard
- 💾 **Image History** — All generations saved to your gallery
- ⬇️ **Free Downloads** — Full resolution PNG downloads
- 🔐 **Auth System** — Email + Google login via Supabase
- ⚡ **Credit System** — 10 free credits on signup
- 🚀 **Rate Limiting** — 5 generations per minute per IP
- 📱 **Fully Responsive** — Mobile, tablet, desktop
- 🔍 **SEO Optimized** — Meta tags, OG, Twitter cards

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Next.js 14 (App Router) |
| Styling | Tailwind CSS |
| Database + Auth | Supabase (free tier) |
| AI API | Hugging Face Inference API |
| Deployment | Vercel |
| Fonts | Playfair Display + DM Sans |

---

## 🚀 Quick Setup (5 Steps)

### Step 1: Clone & Install

```bash
git clone https://github.com/yourname/visionmint-ai.git
cd visionmint-ai
npm install
```

### Step 2: Set Up Supabase

1. Go to [supabase.com](https://supabase.com) → Create new project (free)
2. Go to **SQL Editor** → Run the contents of `lib/schema.sql`
3. Go to **Settings → API** → Copy your:
   - Project URL
   - `anon` public key
   - `service_role` secret key
4. Go to **Authentication → Providers** → Enable Google OAuth
   - Add your Google Client ID and Secret

### Step 3: Get Hugging Face API Key

1. Go to [huggingface.co](https://huggingface.co) → Sign up (free)
2. Go to **Settings → Access Tokens** → New token (read permission)
3. Copy your token

### Step 4: Configure Environment Variables

```bash
cp .env.local.example .env.local
```

Fill in `.env.local`:

```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key

HUGGINGFACE_API_KEY=hf_your_token_here

NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=generate_with_openssl_rand_base64_32

NEXT_PUBLIC_APP_URL=http://localhost:3000
```

Generate `NEXTAUTH_SECRET`:
```bash
openssl rand -base64 32
```

### Step 5: Run

```bash
npm run dev
# Open http://localhost:3000
```

---

## 📁 Project Structure

```
visionmint-ai/
├── app/
│   ├── page.tsx              # Homepage (Hero + Features + Gallery + Pricing)
│   ├── layout.tsx            # Root layout (fonts, metadata, SEO)
│   ├── globals.css           # Design system (Tailwind + custom CSS)
│   ├── dashboard/
│   │   └── page.tsx          # Main generator dashboard
│   ├── history/
│   │   └── page.tsx          # User image history gallery
│   ├── auth/
│   │   ├── login/page.tsx    # Login page
│   │   ├── signup/page.tsx   # Signup page
│   │   └── callback/route.ts # OAuth callback
│   └── api/
│       └── generate-image/
│           └── route.ts      # Main AI generation API route
├── components/
│   ├── layout/
│   │   ├── Navbar.tsx        # Sticky navbar with mobile menu
│   │   └── Footer.tsx        # Footer with links
│   └── ui/
│       ├── HeroSection.tsx   # Landing hero with prompt input
│       ├── FeaturesSection.tsx
│       ├── GallerySection.tsx
│       ├── PricingSection.tsx
│       └── ImageGenerator.tsx # Main tool UI component
├── lib/
│   ├── supabase/
│   │   ├── client.ts         # Browser Supabase client
│   │   └── server.ts         # Server Supabase client
│   ├── database.types.ts     # TypeScript DB types
│   └── schema.sql            # Run this in Supabase SQL editor
├── utils/
│   ├── prompt-enhancer.ts    # Style presets + prompt enhancement
│   ├── rate-limit.ts         # In-memory rate limiter
│   └── cn.ts                 # Tailwind class merger
├── middleware.ts              # Auth session refresh
└── vercel.json               # Vercel deployment config
```

---

## 🌐 Deploy to Vercel

1. Push your code to GitHub
2. Go to [vercel.com](https://vercel.com) → Import project
3. Add all environment variables from `.env.local`
4. Deploy!

Update `NEXT_PUBLIC_APP_URL` and `NEXTAUTH_URL` to your Vercel URL after deployment.

---

## 🔧 Available AI Models

| Model | Quality | Speed |
|-------|---------|-------|
| `stabilityai/stable-diffusion-xl-base-1.0` | ⭐⭐⭐⭐⭐ | Slower |
| `stabilityai/stable-diffusion-2-1` | ⭐⭐⭐⭐ | Medium |
| `runwayml/stable-diffusion-v1-5` | ⭐⭐⭐ | Fast |

---

## 🎨 Art Style Presets

| Style | Best For |
|-------|---------|
| Photorealistic | Product shots, landscapes, portraits |
| Digital Art | Concept art, illustrations |
| Anime | Character art, manga |
| Oil Painting | Classical portraits, scenic |
| Watercolor | Soft artistic scenes |
| Fantasy Art | Creatures, magical worlds |
| Cyberpunk | Sci-fi, neon cityscapes |
| Minimalist | Clean logos, icons |

---

## 📊 API Reference

### POST `/api/generate-image`

**Request Body:**
```json
{
  "prompt": "A magical forest at twilight",
  "style": "photorealistic",
  "aspectRatio": "1:1",
  "model": "stabilityai/stable-diffusion-xl-base-1.0",
  "userId": "optional-user-id"
}
```

**Response:**
```json
{
  "success": true,
  "imageUrl": "data:image/png;base64,...",
  "id": "uuid",
  "creditsRemaining": 9,
  "enhancedPrompt": "photorealistic, ultra-detailed, ..."
}
```

**Rate Limit:** 5 requests/minute per IP

---

## 🔒 Security Features

- ✅ API key kept server-side only (never exposed to client)
- ✅ Row Level Security (RLS) on all Supabase tables
- ✅ Input validation and sanitization
- ✅ Rate limiting per IP address
- ✅ Model allowlist (prevents unauthorized model usage)
- ✅ Credit system prevents abuse

---

## 📈 Production Improvements (Optional)

For production at scale, consider:

1. **Image Storage**: Upload generated images to Supabase Storage instead of returning base64
2. **Redis Rate Limiting**: Replace in-memory rate limiter with Upstash Redis
3. **CDN**: Serve images via Cloudflare CDN
4. **Queue System**: Add BullMQ for managing generation jobs
5. **Stripe Payments**: Integrate for paid credit packages
6. **Analytics**: Add Vercel Analytics or Plausible

---

## 📝 License

MIT License — free for personal and commercial use.

---

Made with ❤️ for creators everywhere.
