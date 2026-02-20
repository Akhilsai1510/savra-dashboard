# Savra Teacher Insights Dashboard

A production-ready Teacher Insights Dashboard built for school administrators to monitor teacher activity, content creation trends, and performance analytics.

🔗 **Live Demo**: [your-netlify-link-here]

---

## Features

- **Overview Stats** — Total Lesson Plans, Quizzes, Assessments, and Question Papers
- **Weekly Activity Chart** — Canvas-based line chart showing daily content creation trends
- **Time Filters** — This Week / This Month / All Time toggle
- **Per Teacher Analysis** — Dropdown + clickable list to drill into individual teacher data
- **Recent Activity Log** — Sorted by latest, showing type, subject, grade, and date
- **AI Pulse Summary** — Dynamic insights generated from live data (most active teacher, low engagement alerts)
- **Subject Breakdown** — Animated bar chart showing activity distribution by subject
- **Duplicate Handling** — Deduplication via composite key `teacher_id|activity_type|created_at`

---

## Architecture Decisions

### Single-File Architecture
The entire dashboard is built as a single `index.html` file with embedded CSS and JavaScript. This was a deliberate choice for:
- **Zero dependencies** — no npm, no build step, no bundler needed
- **Instant deployment** — drag and drop to any static host
- **Portability** — works offline, in any browser, without a server

### Data Layer
- Data is embedded as a JavaScript array parsed from the provided Excel dataset
- A deduplication function runs on load using a composite key (`teacher_id|activity_type|created_at`) to silently remove duplicate entries before any rendering
- All filtering (by time period and by teacher) is done in-memory using pure JavaScript array methods

### Rendering
- **Stats cards** — computed dynamically from filtered data on every state change
- **Chart** — hand-drawn using the HTML5 Canvas API with bezier curve smoothing (no Chart.js dependency)
- **Teacher list & selector** — stay in sync; clicking either updates all panels simultaneously

### State Management
Two global variables drive the entire UI:
- `currentFilter` — `'week'` | `'month'` | `'all'`
- `currentTeacher` — teacher ID or `'all'`

Every UI interaction updates one of these and calls `refresh()`, which re-renders all components.

---

## Tech Stack

| Layer | Choice |
|-------|--------|
| Frontend | Vanilla HTML, CSS, JavaScript |
| Charts | HTML5 Canvas API (custom) |
| Fonts | Google Fonts (DM Serif Display + DM Sans) |
| Hosting | Netlify / GitHub Pages |
| Build | None required |

---

## How to Run Locally

```bash
# Just open the file in your browser
open index.html
```

No server, no npm install, no setup needed.

---

## Future Scalability Improvements

### 1. Backend API
Replace the embedded data array with a REST API:
```
GET /api/activities?teacher_id=T001&from=2026-02-11&to=2026-02-18
```
Built with Node.js + Express or Python + FastAPI, backed by PostgreSQL.

### 2. Database & ORM
Move to a proper relational schema:
- `teachers` table (id, name, subject, grade)
- `activities` table (id, teacher_id, type, subject, grade, created_at)
- Unique constraint on `(teacher_id, activity_type, created_at)` to enforce deduplication at DB level

### 3. Authentication
Add a role-based login system:
- **Principal** — sees all teachers and school-wide analytics
- **Teacher** — sees only their own data
- JWT tokens + bcrypt password hashing

### 4. Real-time Updates
Use WebSockets or Server-Sent Events to push live activity updates to the dashboard without page refresh.

### 5. Export & Reports
- CSV export per teacher
- PDF report generation for parent/board meetings

### 6. React Migration
As the feature set grows, migrate to React + Recharts for better component reusability and state management with Redux or Zustand.

---

## Dataset

Source: `Savra_Teacher_Data_Set.xlsx`

Fields used:
- `teacher_id`
- `teacher_name`
- `grade`
- `subject`
- `activity_type` (Lesson Plan / Quiz / Assessment / Question Paper)
- `created_at`

---

## Submission

Built for Savra's Founding Full-Stack Engineer Intern — Round 2 Technical Assignment.
