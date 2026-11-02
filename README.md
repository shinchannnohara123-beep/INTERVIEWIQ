# InterviewIQ — AI-Based Interview Practice Platform

**InterviewIQ** is a full-stack, production-level web application designed to help students and candidates practice technical interviews with real-time AI evaluation, actionable feedback, and performance analytics.

Built with a clean SaaS blue-and-white theme, soft shadows, rounded cards, and a dual database engine supporting both **Live Supabase PostgreSQL** and an **Instant Offline PostgreSQL Mirror** for zero-friction evaluation.

---

## 🚀 Key Features

1. **Authentication & Protected Routes**
   - Secure registration, email/password login, and session persistence.
   - 1-Click **"Continue as Demo Candidate"** for instant testing and academic grading.
   - Protected route guards across all candidate dashboards and active interviews.

2. **Practice Dashboard**
   - **Metrics Cards**: Total Interviews, Average Score, Best Score, and Readiness Tier.
   - **Circular Progress Ring**: Animated SVG score indicator for skill mastery index.
   - **Score Progression Chart**: Recharts area chart showing score trends over time.
   - **Recent Interview Cards**: Session status, score badges, timestamps, and quick review.

3. **Job Role Tracks & Smart Filtering**
   - Multi-role tracks: Frontend Developer, Backend Engineer, Full Stack, Data Science / ML, DevOps, Product Manager, and Junior Software Engineer.
   - Search by role title or tech stack keywords.
   - Filter by domain categories (Engineering, Data & AI, DevOps & Infra, Product) and seniority (Junior, Mid-Level, Senior).
   - Configurable session setup (select difficulty and question count: 3, 5, or Full Mock).

4. **Live AI Interview Flow**
   - Sequential question view with topic pills and difficulty tags.
   - Animated progress bar indicating question completion status.
   - Large answer textarea with real-time word counter and character counter.
   - Speech-to-Text dictation toggle using Web Speech API.
   - Form validation: Prevents empty answers and disables submission during evaluation.
   - Smooth transitions with multi-step **"Analyzing your answer..."** AI radar loader.

5. **Multi-Dimensional AI Evaluation Engine**
   - Adheres strictly to requirements: no generic feedback, evaluated on candidate's actual answer, and explains **WHY** scores are given.
   - Returns structured JSON:
     - `relevance_score` (0-100)
     - `clarity_score` (0-100)
     - `content_score` (0-100)
     - `technical_score` (0-100)
     - `communication_score` (0-100)
     - `overall_score` (0-100)
     - `strengths` (string[])
     - `weaknesses` (string[])
     - `suggestions` (string[])
     - `feedback` (detailed rationale)
   - Dual-mode backend: Uses Google Gemini 3.8 Flash (`@google/genai`) when an API key is provided, and seamless intelligent semantic evaluation fallback when running offline.

6. **Comprehensive Result Page**
   - Big visual score ring with readiness verdict ("Interview Ready", "Competitive", "Developing").
   - Score breakdown bars across all 5 competencies.
   - Consolidated Strengths & Weaknesses cards.
   - Tailored actionable recommendations.
   - Question-by-question accordion deep dive showing prompt, candidate answer, sub-scores, and AI justification.
   - Celebratory confetti on high scores and PDF export support.

7. **Performance History & Analytics**
   - Chronological session log with status and score badges.
   - Recharts score progression charts and radar competency distribution.

8. **Supabase Schema Explorer & Live Connection Manager**
   - Dedicated in-app Supabase page with the exact SQL schema definition and one-click copy.
   - Live credential switcher to test real cloud Supabase or instant local database.

---

## 🛠️ Tech Stack

- **Frontend**: React 19 + TypeScript + Vite 8
- **Styling**: Tailwind CSS v4 + Lucide React icons
- **Data Visualization**: Recharts (AreaChart, BarChart) + Animated SVG
- **Database**: Supabase (PostgreSQL with RLS & Triggers) + Persistent Local Mirror
- **Backend AI Service**: Node.js + Express + `@google/genai` (port 5000)

---

## 📂 Project Architecture

```
interview-iq/
├── index.html                   # HTML entry with Plus Jakarta Sans & branding
├── vite.config.ts               # Vite configuration with Tailwind v4 & API proxy
├── server/
│   ├── index.ts                 # Express backend server (POST /api/evaluate, GET /api/health)
│   └── ai-service.ts            # Gemini 3.8 Flash integration & semantic heuristic fallback
├── src/
│   ├── main.tsx                 # React DOM root
│   ├── App.tsx                  # Application routing & layout
│   ├── index.css                # Tailwind CSS v4 styles & custom utilities
│   ├── types/
│   │   └── database.types.ts    # Complete TypeScript types matching Supabase schema
│   ├── lib/
│   │   ├── supabase.ts          # Unified Supabase & database abstraction
│   │   ├── mock-db.ts           # Persistent local database store & seeds
│   │   └── ai-client.ts         # Frontend AI client with resilient fallback
│   ├── context/
│   │   └── AuthContext.tsx      # Authentication state, login, register, demo mode
│   ├── components/
│   │   ├── layout/              # Sidebar, Header, Layout, ProtectedRoute
│   │   ├── ui/                  # Button, Card, Badge, Progress
│   │   ├── charts/              # CircularProgress, ScoreTrendChart, RadarSkillsChart
│   │   └── interview/           # QuestionView, AnswerInput, AnalyzingLoader, SetupInterviewModal
│   └── pages/
│       ├── Dashboard.tsx        # Overview stats, charts, recent sessions
│       ├── JobRoles.tsx         # Role tracks, search, category & difficulty filters
│       ├── InterviewSession.tsx # Live mock interview flow
│       ├── InterviewResult.tsx  # Result analysis, score breakdown, feedback
│       ├── InterviewHistory.tsx # Past session history
│       ├── Analytics.tsx        # Long-term performance intelligence
│       ├── SupabaseSetup.tsx    # Supabase schema viewer & live connection form
│       ├── Login.tsx            # Candidate login
│       └── Register.tsx         # Account registration
└── supabase/
    └── schema.sql               # Full PostgreSQL schema with tables, RLS & seed data
```

---

## ⚡ Quick Start

### 1. Prerequisites
- Node.js (v18+)
- npm or pnpm

### 2. Run the Application
From the `interview-iq` directory:
```bash
npm run dev
```

This launches both:
- **Vite Frontend**: `http://localhost:5173`
- **AI Backend Service**: `http://localhost:5000`

### 3. Optional Environment Configuration (`.env`)
To enable live Google Gemini evaluations or connect to a remote Supabase project:
```env
PORT=5000
GEMINI_API_KEY=your_gemini_api_key_here
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
```
*(If left blank, the app runs automatically with the local persistent database and intelligent evaluation engine.)*

---

## 🗄️ Supabase Database Schema

The database schema matches the required specification and is saved in `supabase/schema.sql`. You can copy it directly from `supabase/schema.sql` or from the in-app **Supabase DB Schema** page (`/supabase-schema`) and execute it in your Supabase SQL Editor.

Included tables:
- `profiles`
- `job_roles`
- `questions`
- `interviews`
- `interview_questions`
- `answers`
- `evaluations`
- `performance_history`
- RLS Policies & Trigger for `auth.users` profile synchronization.
