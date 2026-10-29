# AssignFlow — Academic Assignment Management Platform

> "A simpler way to organize, submit, and manage academic work."

AssignFlow is a modern, light academic SaaS platform designed for higher education coursework. It provides intentionally separate, dedicated command centers for **Students** and **Faculty**, backed by real-time Supabase PostgreSQL, Authentication, Row Level Security (RLS), and Storage.

---

## 🌟 Key Features

### For Students
- **Personal Academic Command Center (`/dashboard`)**: Dynamic greeting, real database summary metrics (Due Today, Due This Week, In Progress, Completed), immediate attention alerts for overdue/urgent tasks, and dynamic academic completion progress.
- **Assignment Tracker (`/assignments`)**: Multi-criteria search, filtering (All, In Progress, Submitted, Graded, Overdue), course filtering, priority sorting, and deadline badges.
- **Submission Desk (`/assignments/[id]`)**: Written answers and file attachments with automatic late detection based on official assignment deadlines.
- **Course Enrollment (`/courses`)**: Join courses via 6-character class join codes, browse course syllabi, and view instructor info.
- **Coursework Calendar (`/calendar`)**: Month-view academic calendar displaying deadlines and assignment agendas.
- **Grade & Feedback Tracking (`/submissions`)**: View instructor scores, max points, and qualitative grading feedback.
- **Class Announcements (`/announcements`)**: Real-time broadcast notices from instructors.

### For Teachers & Faculty
- **Faculty Dashboard (`/teacher/dashboard`)**: KPI metrics (Active Curricula, Published Assignments, Pending Reviews, Total Students), review queue, and course rosters.
- **Course Management (`/teacher/courses`)**: Create classes with unique 6-character student join codes, edit curricula, and manage enrollments.
- **Assignment Publishing Desk (`/teacher/assignments`)**: Create, schedule, edit, and publish assignments with points, instructions, and late-submission flags.
- **Interactive Grading Desk (`/teacher/submissions`)**: Rapid review workflow to evaluate student answers, review attachments, assign scores, and provide qualitative feedback.
- **Student Directory (`/teacher/students`)**: Roster of enrolled students across all active courses.
- **Announcements Manager (`/teacher/announcements`)**: Post, update, and manage course announcements.
- **Academic Analytics (`/teacher/analytics`)**: Authentic course performance metrics, grading progress, and submission rates computed directly from database records.

---

## 🛠 Tech Stack

- **Framework**: [Next.js 15 (App Router)](https://nextjs.org/)
- **UI Library**: [React 19](https://react.dev/) & [Tailwind CSS](https://tailwindcss.com/)
- **Language**: [TypeScript](https://www.typescriptlang.org/)
- **Backend & Database**: [Supabase](https://supabase.com/) (PostgreSQL, Auth, RLS, Storage)
- **SSR & Authentication**: `@supabase/ssr` with cookie-based session management and session refresh middleware
- **Icons**: [Lucide React](https://lucide.dev/)
- **Dates**: `date-fns`

---

## 📁 Project Structure

```text
├── src/
│   ├── app/
│   │   ├── page.tsx                     # Public landing page with visual preview
│   │   ├── login/page.tsx               # Authentication sign-in
│   │   ├── signup/page.tsx              # Account creation with role selection
│   │   ├── auth/callback/route.ts       # Supabase auth code exchange callback
│   │   ├── dashboard/page.tsx           # Student dashboard command center
│   │   ├── assignments/                 # Student assignments and detail submission desk
│   │   ├── courses/                     # Student courses and join modal
│   │   ├── calendar/page.tsx            # Academic deadline calendar
│   │   ├── submissions/page.tsx         # Student submissions & grades
│   │   ├── announcements/page.tsx       # Student announcements view
│   │   ├── settings/page.tsx            # Account profile & session management
│   │   └── teacher/
│   │       ├── dashboard/page.tsx       # Faculty dashboard
│   │       ├── courses/page.tsx         # Faculty course management & join codes
│   │       ├── assignments/page.tsx     # Faculty assignment publisher
│   │       ├── submissions/page.tsx     # Faculty grading desk
│   │       ├── students/page.tsx        # Enrolled student directory
│   │       ├── announcements/page.tsx   # Announcements manager
│   │       └── analytics/page.tsx       # Gradebook performance metrics
│   ├── components/
│   │   ├── ui/                          # Reusable buttons, inputs, modals, badges, cards
│   │   └── navigation/                  # Desktop sidebar, topbar, mobile nav drawer
│   ├── lib/
│   │   ├── supabase/                    # Browser & server SSR clients
│   │   ├── auth/                        # Auth context & session hooks
│   │   └── utils.ts                     # Formatting, styling & deadline helpers
│   ├── middleware.ts                    # Next.js session refresh & route protection
│   └── types/                           # TypeScript database and domain models
├── supabase/
│   └── migrations/                      # Complete reproducible PostgreSQL schema & RLS
├── .env.example                         # Safe template for environment variables
└── README.md
```

---

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/your-username/assignflow.git
cd assignflow
```

### 2. Install dependencies
```bash
npm install
```

### 3. Environment Configuration
Create a `.env.local` file in the project root (never committed to version control):

```env
NEXT_PUBLIC_SUPABASE_URL=https://yghkiymytoraydxkasov.supabase.co
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=sb_publishable_k9T5ZBofmoneoj9r7tbqxw_GfOLEnM1
```

A template is provided in `.env.example`:
```env
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=your_supabase_publishable_key
```

### 4. Database Setup & Supabase Migration
Open your Supabase project's **SQL Editor**:
[https://supabase.com/dashboard/project/yghkiymytoraydxkasov/sql/new](https://supabase.com/dashboard/project/yghkiymytoraydxkasov/sql/new)

Copy and execute the complete migration script located at:
```text
supabase/migrations/20250101000000_initial_schema.sql
```

This single migration handles:
- `profiles` table linked to `auth.users` with automated profile trigger.
- `courses` table with 6-character unique `join_code`.
- `course_members` enrollment table with duplicate enrollment prevention.
- `assignments` table with publication status (`draft`, `published`, `closed`).
- `submissions` table with storage paths and grading columns.
- `announcements` and `notifications` tables.
- Storage bucket `submissions` initialization and object access rules.
- Complete Row Level Security (RLS) policies enforcing role isolation.

### 5. Run Development Server
```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 🔒 Security & Row Level Security (RLS)

- **Role Isolation**: Students cannot access teacher-only routes or modify assignments, courses, or grades. The database enforces this at the PostgreSQL RLS level via `auth.uid()`.
- **Privilege Escalation Prevention**: Role assignment is established securely on account creation and protected from arbitrary frontend mutations.
- **Private Submissions**: Students can only access their own submissions and grades; teachers can only view submissions for assignments in their owned courses.
- **Credentials Protection**: `.env.local` is strictly excluded in `.gitignore`. No secret keys or service role keys are exposed to the client.

---

## 📦 Deployment to Vercel

1. Push your repository to GitHub.
2. Import the project in the [Vercel Dashboard](https://vercel.com/new).
3. Set the Environment Variables:
   - `NEXT_PUBLIC_SUPABASE_URL`
   - `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`
4. Deploy!
5. In your Supabase Dashboard under **Authentication > URL Configuration**, add your production Vercel URL to the **Redirect URLs** list:
   - `https://your-domain.vercel.app/auth/callback`

---

## 🧪 Testing Commands

```bash
# Type check and lint
npm run lint

# Production build test
npm run build
```

