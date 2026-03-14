# ARCHITECTURE.md — Dubai Property 3D World Platform

## System Overview

The platform is a three-tier web application with a 3D processing pipeline.

```
┌─────────────────────────────────────────────────────────────────┐
│                        CLIENT (Browser)                         │
│  Next.js 16 App Router · React 19 · TailwindCSS 4 · Three.js   │
│                                                                 │
│  ┌──────────┐ ┌──────────────┐ ┌────────────┐ ┌──────────────┐ │
│  │ Landing  │ │  ROI Wizard  │ │ Prop Pulse │ │  3D Viewer   │ │
│  │   App    │ │   (Tool 1)   │ │  (Tool 2)  │ │ (Gaussian    │ │
│  │          │ │              │ │            │ │  Splat)      │ │
│  └──────────┘ └──────────────┘ └────────────┘ └──────────────┘ │
└─────────────────────────┬───────────────────────────────────────┘
                          │ HTTPS / WebSocket
┌─────────────────────────▼───────────────────────────────────────┐
│                     BACKEND (Node.js + Express)                  │
│                                                                  │
│  ┌──────────┐ ┌──────────────┐ ┌────────────┐ ┌──────────────┐  │
│  │  Auth    │ │  Property    │ │  Market /  │ │  3D Jobs /   │  │
│  │Middleware│ │  / ROI / AI  │ │  Developer │ │  Upload      │  │
│  └──────────┘ └──────────────┘ └────────────┘ └──────────────┘  │
└──────┬──────────────┬──────────────┬──────────────┬─────────────┘
       │              │              │              │
┌──────▼──────┐ ┌─────▼─────┐ ┌─────▼─────┐ ┌─────▼──────┐ ┌────────────┐ ┌────────────┐
│  Supabase   │ │UploadThing│ │Uploadcare  │ │ World Labs │ │ Google     │ │ Crustdata  │
│  Auth + DB  │ │  (Files)  │ │  (CDN)     │ │ Marble API │ │ Gemini AI  │ │ (B2B Data) │
│  + Storage  │ │           │ │            │ │  (3D Gen)  │ │ (Content)  │ │            │
└─────────────┘ └───────────┘ └────────────┘ └────────────┘ └────────────┘ └────────────┘
```

---

## Directory Structure

```
hackethon/
├── AGENTS.md                    # AI agent instructions
├── ARCHITECTURE.md              # This file
├── frontend/                    # Next.js 16 application
│   ├── app/                     # App Router pages & layouts
│   │   ├── (marketing)/         # Landing pages (grouped route)
│   │   ├── (tools)/             # ROI Wizard + Prop Pulse
│   │   ├── (viewer)/            # 3D World Viewer
│   │   ├── (auth)/              # Login, signup, forgot password
│   │   ├── (dashboard)/         # User/broker dashboard
│   │   ├── api/                 # Route handlers (uploadthing, webhooks)
│   │   ├── layout.tsx           # Root layout
│   │   └── page.tsx             # Home page
│   ├── components/              # Shared React components
│   │   ├── ui/                  # Design system primitives
│   │   ├── forms/               # Form components
│   │   ├── charts/              # Recharts wrappers
│   │   ├── maps/                # Mapbox components
│   │   └── viewer/              # 3D Viewer components
│   ├── lib/                     # Utilities, clients, helpers
│   │   ├── supabase/            # Supabase client setup
│   │   ├── uploadthing/         # UploadThing config
│   │   └── utils/               # General utilities
│   ├── hooks/                   # Custom React hooks
│   ├── types/                   # TypeScript type definitions
│   ├── public/                  # Static assets
│   └── package.json
├── backend/                     # Node.js + Express API
│   ├── src/
│   │   ├── routes/              # Express route handlers
│   │   ├── middleware/          # Auth, validation, error handling
│   │   ├── services/            # Business logic layer
│   │   ├── models/              # Data models / Supabase queries
│   │   ├── jobs/                # Background job processors
│   │   ├── config/              # Environment config
│   │   ├── utils/               # Utility functions
│   │   └── index.ts             # App entry point
│   └── package.json
├── docs/                        # Project documentation
│   ├── prd.md                   # Product requirements
│   ├── SPEC.md                  # Technical specification
│   ├── FRONTEND.md              # Frontend rules
│   ├── BACKEND.md               # Backend rules
│   ├── DATABASE.md              # Database rules
│   ├── DESIGN.md                # Design system
│   ├── SECURITY.md              # Security guidelines
│   ├── design-docs/             # Architecture decision records
│   ├── exec-plans/              # Execution plans
│   ├── generated/               # Auto-generated docs
│   ├── product-specs/           # Feature specifications
│   └── references/              # Third-party API references
└── .env.example                 # Environment variable template
```

---

## Data Flow

1. **User Auth** → Supabase Auth handles login/signup on the frontend via `@supabase/ssr` → JWT stored in HTTP-only cookies → Backend validates JWT via `authMiddleware` (no `/api/auth` routes needed)
2. **Property Data** → Backend fetches from Supabase DB → Served via REST API → Consumed by frontend Server Components
3. **File Uploads** → Frontend uses UploadThing components → Files stored via UploadThing → URLs saved to Supabase DB
4. **Image CDN** → Property images served via Uploadcare CDN with on-the-fly transformations
5. **3D Generation** → Photos uploaded → Backend triggers World Labs Marble API → Async job polls for completion → 3D scene URL stored in DB → Frontend loads scene in Three.js viewer
6. **Video Export** → Server-side headless rendering of 3D scene → FFmpeg encoding → MP4 delivered to user
7. **Developer Intelligence** → User enters developer name → Backend queries Crustdata API for B2B data (headcount, funding, hiring) → Data fed to Gemini AI → AI generates institutional-grade due diligence report → Report rendered as shareable page with PDF export

---

## Key Integration Points

| Integration | Purpose | Auth Method |
|---|---|---|
| Supabase | Auth, DB, RLS, Storage | Service Role Key (backend), Anon Key + JWT (frontend) |
| UploadThing | File upload handling | API key + upload token |
| Uploadcare | CDN, image transformations | Public key + signed URLs |
| World Labs Marble API | 3D world generation from photos | API key |
| Google Gemini AI | AI content generation (descriptions, ROI narratives, developer reports) | API key |
| Crustdata | B2B data enrichment for developer intelligence | API key |
| Mapbox | Map rendering, geocoding | Access token |
| DLD API (future) | Dubai Land Department transaction data | API key |

