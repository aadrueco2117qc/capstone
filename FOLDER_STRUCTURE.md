# EyeGuard — Folder Structure

```
v0-eyestrain-main/
│
├── app/                          FRONTEND — Next.js App Router pages
│   ├── layout.tsx                Root layout (AuthProvider wrapper)
│   ├── page.tsx                  Landing page
│   ├── globals.css               Global styles and Tailwind theme
│   │
│   ├── dashboard/                Main user dashboard
│   │   ├── layout.tsx
│   │   └── page.tsx              Displays predictions, analytics, recommendations
│   │
│   ├── daily-log/
│   │   └── page.tsx              Form to log daily screen time and symptoms
│   │
│   ├── analytics/
│   │   ├── layout.tsx
│   │   └── page.tsx              Historical analytics and trend charts
│   │
│   ├── risk-prediction/
│   │   └── page.tsx              Detailed risk breakdown and 7-day forecast
│   │
│   ├── trends/
│   │   └── page.tsx              Long-term trend visualization
│   │
│   ├── settings/
│   │   └── page.tsx              User profile and preferences
│   │
│   ├── login/
│   │   ├── layout.tsx
│   │   └── page.tsx              Login page
│   │
│   ├── signup/
│   │   ├── layout.tsx
│   │   └── page.tsx              Registration page
│   │
│   ├── auth/
│   │   ├── layout.tsx
│   │   └── callback/
│   │       └── route.ts          Supabase OAuth callback handler
│   │
│   └── api/                      Next.js API routes (server-side)
│       └── predict-supabase/
│           └── route.ts          ML prediction endpoint (Supabase-connected)
│
├── backend/                      BACKEND — Flask Python API
│   ├── app.py                    Flask app factory, blueprint registration
│   ├── config.py                 Environment-based configuration
│   ├── models.py                 SQLAlchemy database models
│   ├── requirements.txt          Python dependencies
│   ├── README.md                 Backend setup guide
│   │
│   ├── routes/                   API route handlers (Flask blueprints)
│   │   ├── __init__.py
│   │   ├── auth.py               Register, login, logout, token refresh
│   │   ├── users.py              Profile, settings, password management
│   │   ├── logs.py               Daily log CRUD operations
│   │   ├── predictions.py        ML prediction endpoints
│   │   └── analytics.py          Summary, trends, insights
│   │
│   ├── ml/                       MACHINE LEARNING pipeline
│   │   ├── __init__.py
│   │   ├── features.py           Feature engineering (12 features)
│   │   ├── models.py             ML model class definitions
│   │   ├── training.py           Training pipeline + synthetic data generation
│   │   ├── predictor.py          Prediction service (today, tomorrow, week)
│   │   ├── storage.py            Model serialization and loading (pickle)
│   │   └── README.md             ML documentation
│   │
│   └── utils/                    Shared backend utilities
│       ├── __init__.py
│       ├── validation.py         Input validation functions
│       └── response.py           Standardized API response formatting
│
├── components/                   FRONTEND — Reusable React components
│   ├── auth-guard.tsx            Protects routes from unauthenticated access
│   ├── main-layout.tsx           Page layout wrapper with sidebar
│   ├── sidebar.tsx               Navigation sidebar (mobile-responsive)
│   ├── dashboard-card.tsx        MetricCard, ChartCard, StatCard components
│   ├── dashboard-empty-state.tsx Empty state UI for new users
│   ├── screen-time-form.tsx      Daily log submission form
│   ├── form-components.tsx       InputField, Button, Select, Slider, Toggle
│   ├── theme-provider.tsx        Dark/light mode provider
│   ├── admin-layout.tsx          Admin panel layout wrapper
│   ├── admin-sidebar.tsx         Admin navigation sidebar
│   ├── admin-header.tsx          Admin top header bar
│   │
│   └── ui/                       shadcn/ui component library (57 files)
│       ├── button.tsx
│       ├── card.tsx
│       ├── input.tsx
│       ├── select.tsx
│       ├── table.tsx
│       ├── chart.tsx
│       ├── dialog.tsx
│       ├── toast.tsx
│       └── ...                   (accordion, badge, calendar, etc.)
│
├── lib/                          FRONTEND — Utilities and services
│   ├── api.ts                    Type-safe API client (all endpoint calls)
│   ├── auth-context.tsx          Global auth state (useAuth hook)
│   ├── hooks.ts                  SWR data-fetching hooks
│   ├── csv-import.ts             CSV parsing for admin data import
│   ├── utils.ts                  General utility functions
│   │
│   └── supabase/                 Supabase client configuration
│       ├── client.ts             Browser-side Supabase client
│       ├── server.ts             Server-side Supabase client
│       └── proxy.ts              Supabase proxy helper
│
├── hooks/                        FRONTEND — Custom React hooks
│   ├── use-mobile.ts             Detects mobile viewport
│   └── use-toast.ts              Toast notification hook
│
├── database/                     DATABASE — Schema and migrations
│   └── schema.sql                Supabase table definitions + RLS policies
│
├── docs/                         DOCUMENTATION — Developer guides
│   └── AUTHENTICATION.md         Authentication implementation guide
│
├── public/                       STATIC ASSETS
│   ├── icon.svg
│   ├── apple-icon.png
│   ├── icon-dark-32x32.png
│   ├── icon-light-32x32.png
│   └── placeholder images
│
├── styles/
│   └── globals.css               Additional global styles
│
├── PROJECT_OVERVIEW.md           Full project documentation (start here)
├── FOLDER_STRUCTURE.md           This file
│
├── next.config.mjs               Next.js configuration
├── tsconfig.json                 TypeScript configuration
├── postcss.config.mjs            PostCSS / Tailwind configuration
├── components.json               shadcn/ui configuration
├── middleware.ts                 Next.js middleware (auth session handling)
├── package.json                  Frontend dependencies
├── pnpm-lock.yaml                Lockfile
└── .gitignore
```

---

## What Lives Where

| You want to... | Go to... |
|---|---|
| Add a new page | `app/your-page/page.tsx` |
| Add a reusable UI component | `components/` |
| Call a backend API from frontend | `lib/api.ts` |
| Manage auth state | `lib/auth-context.tsx` |
| Add a new Flask API endpoint | `backend/routes/` |
| Change database models | `backend/models.py` |
| Modify ML features or models | `backend/ml/` |
| Update database schema | `database/schema.sql` |
| Add a shadcn/ui component | `components/ui/` |
| Configure Supabase | `lib/supabase/` |
