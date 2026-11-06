# ConceptCoach

### AI-powered Socratic learning for real conceptual understanding

ConceptCoach helps learners discover whether they truly understand a topic—not just whether they can remember its definition.

Instead of giving an instant answer, the platform asks the learner to explain a concept in their own words. An AI coach then asks focused, reasoning-based follow-up questions, tracks the discussion, and produces a personalised evaluation report.

> **Explain → Question → Think → Answer → Improve → Evaluate**

[Live application](https://conceptcoachai.vercel.app) · [Backend repository](https://github.com/shivhareharshdev/Explained.AI) · [GitHub profile](https://github.com/shivhareharshdev)

## At a glance

| Question | Answer |
| --- | --- |
| What is it? | An AI coach that tests and improves conceptual understanding. |
| Who is it for? | Students, self-learners and developers preparing for technical interviews. |
| What makes it different? | The learner explains first; the AI asks questions instead of immediately giving answers. |
| What does the learner receive? | A guided discussion, scores, strengths, gaps and an ideal explanation. |
| Is there a live demo? | Yes — [open ConceptCoach](https://conceptcoachai.vercel.app). |

## Why ConceptCoach?

Most learning tools optimise for recall. ConceptCoach is designed around retrieval, reasoning and explanation—the skills that reveal genuine understanding.

It helps learners identify:

- What they already understand correctly
- Missing assumptions and reasoning gaps
- Incorrect statements and misconceptions
- Whether their explanation is clear and complete
- What they should practise next

The result is a structured learning conversation rather than a conventional chatbot response or static quiz.

## Complete learner journey

1. **Sign up or log in** — Authentication keeps every session private and connected to the learner.
2. **Choose a concept** — For example, Binary Search, React State, JWT or Database Indexing.
3. **Explain it independently** — The learner writes what they understand in their own words.
4. **Start the coaching session** — ConceptCoach analyses the explanation and asks one reasoning-based question.
5. **Answer and think deeper** — The learner responds while the AI follows up on gaps, assumptions and misconceptions.
6. **Complete 3–5 rounds** — The session tracks progress and ends automatically at the selected round limit.
7. **Read the evaluation report** — Scores and qualitative feedback show what was understood before and after the discussion.
8. **Review history** — Completed sessions can be replayed later as read-only learning records.

```text
Sign up → Choose topic → Explain → AI probes → Learner reasons
        → Guided rounds → Evaluation report → Review progress
```

### Example

If a learner chooses **Binary Search**, ConceptCoach does not simply explain binary search. It may ask:

> “Why can the left half be discarded when the target is greater than the middle element?”

The learner’s answer helps the system evaluate reasoning, not just memorised definitions.

## Features

### Learning experience

- Topic-based concept explanation sessions
- AI-generated, reasoning-focused follow-up questions
- Configurable 3, 4 or 5 round discussions
- Live round progress and AI thinking state
- Personalised report with overall, clarity, correctness, reasoning and communication scores
- Strengths, reasoning gaps, resolved gaps, remaining gaps and next steps
- History page to replay previous discussions
- Read-only completed sessions to protect learning records

### Engineering highlights

- JWT authentication with access and refresh tokens in `httpOnly` cookies
- Protected React routes and protected API routes
- Ownership checks for sessions, messages and reports
- Centralised API client with credentials and normalised errors
- Same-origin `/api` routing in development and production
- Responsive interface built with reusable UI patterns and shared Tailwind class tokens

## Tech stack

| Area | Technology |
| --- | --- |
| Frontend | React 19, React Router 7, Tailwind CSS 4 |
| Build tooling | Vite 8, Oxlint |
| Backend | Node.js, Express 5 |
| Database | MongoDB Atlas with Mongoose 9 |
| Authentication | JWT, bcrypt, `httpOnly` cookies |
| AI | Google Gemini via `@google/generative-ai` |
| Deployment | Vercel (frontend), Render (API) |

## How the system works

```text
Browser (React)
        │  authenticated /api requests
        ▼
Express + Node.js API
        ├── Authenticates the learner with JWT cookies
        ├── Validates session ownership
        ├── Stores explanations and conversation messages
        └── Sends context-aware prompts to Gemini
                │
                └── MongoDB Atlas stores users, sessions, messages and reports
```

The frontend keeps API calls in one service layer (`frontend/src/services/api.js`). The backend owns authentication, session state, conversation history and AI orchestration. This separation keeps UI components focused on presentation and user interaction.

### Request lifecycle

```text
User submits an explanation
        ↓
Frontend sends a credentialed API request
        ↓
API authenticates the user and creates a learning session
        ↓
AI receives the topic, explanation and conversation context
        ↓
AI returns the next probing question
        ↓
Question, answer and round state are saved in MongoDB
        ↓
After the final round, the report is generated and displayed
```

## Repository structure

```text
.
├── frontend/
│   ├── src/
│   │   ├── components/ui/   # Navbar, hero, protected routes and UI building blocks
│   │   ├── context/         # Authentication state and session bootstrap
│   │   ├── pages/           # Home, auth, sessions, reports and history
│   │   ├── services/        # Centralised API client
│   │   └── styles/          # Shared styles and class tokens
│   ├── vite.config.js
│   └── package.json
├── backend/                 # Backend workspace / local checkout
└── project/                 # Product and implementation notes
```

## Run the frontend locally

```bash
cd frontend
npm install
npm run dev
```

The frontend runs at `http://localhost:5173`.

The local Vite server proxies `/api/*` to the backend at `http://localhost:8000`. Start the API separately using the instructions in the [backend repository](https://github.com/shivhareharsh-005/Explained.AI).

### Environment variables

Copy the example file when needed:

```bash
cp .env.example .env
```

| Variable | Required | Description |
| --- | --- | --- |
| `VITE_API_URL` | No | API base URL. Defaults to `/api`. |

For local development, the default `/api` value is recommended because Vite handles the proxy. Production also uses `/api`, with Vercel rewriting API requests to the Render service.

## Useful scripts

Run these from `frontend/`:

```bash
npm run dev       # Start the Vite development server
npm run build     # Create a production build
npm run preview   # Preview the production build locally
npm run lint      # Run Oxlint
```

## Deployment

### Frontend — Vercel

- Framework preset: `Vite`
- Build command: `npm run build`
- Output directory: `dist`
- API routing: configured through `frontend/vercel.json`

### Backend — Render

- Install command: `npm install`
- Start command: `node src/index.js`
- Required services: MongoDB Atlas and Gemini API credentials

The Render free tier may sleep after inactivity, so the first API request after a pause can take longer than usual.

## Roadmap

- Automated API integration and component tests
- Refresh-token rotation and silent session refresh
- Streaming AI responses with SSE
- Markdown rendering for AI feedback
- Targeted practice questions after each report
- Per-topic analytics and learning trends
- Rate limiting and additional production hardening

## Project status

The core MVP learning loop is implemented: authentication, explanation submission, AI-guided sessions, round tracking, report viewing and history are available. The roadmap focuses on deeper practice, analytics, testing and production hardening.

## Author

Built by [Harsh Shivhare](https://github.com/shivhareharshdev).

