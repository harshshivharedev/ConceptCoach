# Clean Frontend Roadmap

Goal:

```txt
Simple
Clean
Workable
Responsive
API-connected
Easy to understand
```

Fancy animations, complex dashboard, and unnecessary UI abhi nahi banayenge.

---

# Phase 1: Frontend Foundation

## 1. Folder structure

```txt
frontend/src/
├── assets/
├── components/
│   ├── Navbar.jsx
│   ├── Button.jsx
│   ├── InputField.jsx
│   ├── Loading.jsx
│   ├── ErrorMessage.jsx
│   ├── ChatMessage.jsx
│   ├── ChatInput.jsx
│   ├── RoundProgress.jsx
│   └── ProtectedRoute.jsx
│
├── pages/
│   ├── Home.jsx
│   ├── Login.jsx
│   ├── Signup.jsx
│   ├── StartSession.jsx
│   ├── ChatSession.jsx
│   └── SessionComplete.jsx
│
├── services/
│   └── api.js
│
├── context/
│   └── AuthContext.jsx
│
├── App.jsx
├── main.jsx
├── App.css
└── index.css
```

## 2. Required routes

```txt
/
/login
/signup
/start-session
/session/:sessionId
/session/:sessionId/complete
```

Later:

```txt
/dashboard
/history
/report/:sessionId
```

---

# Phase 2: Visual Design Direction

## Design style

- White or soft gray background
- One primary accent color
- Dark readable text
- Simple cards
- Medium spacing
- Clear form labels
- Minimal borders
- No excessive gradients
- No unnecessary animations
- Mobile responsive layout

## Basic visual system

```txt
Primary: blue or green
Background: #f8fafc
Text: #172033
Muted text: #64748b
Border: #e2e8f0
Success: green
Error: red
```

Use consistent:

- button height
- input height
- border radius
- page max width
- spacing
- typography

## Main layout

```txt
Navbar
Main content with max-width
Page-specific content
```

---

# Phase 3: Home Page

Route:

```txt
/
```

## Purpose

User ko product samjhana aur auth page par bhejna.

## Content

### Header

```txt
ConceptCheck
Login
Get Started
```

### Main section

```txt
Test your real understanding.
Explain a concept. Answer deeper questions. Improve your reasoning.
```

Buttons:

```txt
Get Started
Login
```

### How it works

Three simple steps:

```txt
1. Explain
2. Discuss
3. Improve
```

### Small feature section

```txt
AI-guided concept discussion
Reasoning-based questions
Progress tracking
```

Home page marketing-heavy nahi honi chahiye. User ko quickly app start karna chahiye.

---

# Phase 4: Signup Page

Route:

```txt
/signup
```

## Fields

```txt
Full name
Username
Email
Password
```

## Flow

```txt
User form fill karega
→ validation
→ POST /api/users/register
→ success
→ /login redirect
```

## Frontend validation

- Full name required
- Username required
- Valid email
- Password required
- Password minimum length
- Duplicate user error show karo

## UI states

```txt
idle
loading
success
error
```

Signup ke waqt button:

```txt
Create account
```

Loading ke waqt:

```txt
Creating account...
```

---

# Phase 5: Login Page

Route:

```txt
/login
```

## Fields

```txt
Email or username
Password
```

## Flow

```txt
User login form submit
→ POST /api/users/login
→ httpOnly cookies set
→ /start-session redirect
```

## Important API setting

Axios use karoge to:

```js
withCredentials: true
```

required hoga.

## UI

```txt
Welcome back
Continue learning
Email or username
Password
Login
Create account
```

Login errors:

```txt
Invalid credentials
User not found
Server error
```

---

# Phase 6: API Service Layer

File:

```txt
src/services/api.js
```

Yahan saari backend requests rahengi.

## Functions

```txt
registerUser()
loginUser()
logoutUser()
createExplanation()
startSession()
sendSessionMessage()
```

## API mapping

```txt
registerUser
→ POST /api/users/register

loginUser
→ POST /api/users/login

logoutUser
→ POST /api/users/logout

createExplanation
→ POST /api/explanations

startSession
→ POST /api/sessions

sendSessionMessage
→ POST /api/sessions/:sessionId/messages
```

Frontend components ke andar direct fetch/axios code nahi bharna. Components service functions call karenge.

---

# Phase 7: Auth State

File:

```txt
src/context/AuthContext.jsx
```

## Purpose

App ko pata rahega user logged in hai ya nahi.

State:

```txt
user
isAuthenticated
loading
```

Functions:

```txt
login()
logout()
```

Important:

- token frontend localStorage mein save nahi karna
- backend httpOnly cookies handle karega
- frontend `withCredentials: true` ke saath request karega

## Protected routes

`ProtectedRoute.jsx` check karega:

```txt
User authenticated?
→ page show

User not authenticated?
→ /login redirect
```

Protected pages:

```txt
/start-session
/session/:sessionId
```

---

# Phase 8: Start Session Page

Route:

```txt
/start-session
```

## Purpose

User ka topic aur initial explanation lena.

## UI fields

```txt
Topic
Explanation textarea
Maximum rounds
Start discussion button
```

### Topic example

```txt
Binary Search
```

### Explanation placeholder

```txt
Explain the concept in your own words...
```

### Max rounds

Abhi simple select:

```txt
3 rounds
4 rounds
5 rounds
```

Default:

```txt
3 rounds
```

## Flow

First request:

```txt
POST /api/explanations
```

Response:

```txt
explanationId
```

Second request:

```txt
POST /api/sessions
```

Body:

```json
{
  "explanationId": "...",
  "maxRounds": 3
}
```

Response:

```txt
sessionId
firstMessage
```

Then:

```txt
/session/:sessionId
```

par redirect.

## States

```txt
empty
typing
submitting explanation
starting session
error
```

---

# Phase 9: Chat Session Page

Route:

```txt
/session/:sessionId
```

Ye frontend ka main page hoga.

## Layout

```txt
Topic header
Round progress
Chat messages
Answer input
Send button
```

## Header

```txt
Binary Search
Round 1 of 3
Status: Active
```

## Chat messages

AI message:

```txt
Why is binary search dependent on sorted data?
```

User message:

```txt
Because sorted order helps us discard one side.
```

Different styles:

```txt
AI message: left aligned
User message: right aligned
```

## Input area

```txt
textarea
Send answer button
```

Send karne ke baad:

```txt
button disabled
loading indicator
```

## API flow

```txt
User answer
→ POST /api/sessions/:sessionId/messages
→ response
→ userMessage add to chat
→ nextMessage add to chat
→ currentRound update
```

## Frontend state

```js
messages
content
currentRound
maxRounds
status
loading
error
```

## Important behavior

AI message response se:

```txt
nextMessage
```

show hoga.

Session active:

```txt
input enabled
```

Session completed:

```txt
input disabled
```

---

# Phase 10: Session Complete Screen

Route:

```txt
/session/:sessionId/complete
```

Abhi report nahi banani, isliye simple screen:

```txt
Discussion completed
You completed all rounds successfully.
Detailed evaluation will be available soon.
```

Buttons:

```txt
Start another session
Go to home
```

Later isi screen par:

- final report
- score
- gaps
- practice questions

add honge.

---

# Phase 11: Loading and Error Handling

Har API request ke liye states handle karna:

## Loading

```txt
Loading...
Starting session...
Sending answer...
```

## Error

```txt
Unable to start session
Session not found
Your session has expired
Something went wrong
```

## Empty states

```txt
No messages yet
No previous sessions
```

User ko blank screen kabhi nahi dikhani.

---

# Phase 12: Logout

Navbar mein:

```txt
Logout
```

Flow:

```txt
POST /api/users/logout
→ auth state clear
→ /login redirect
```

Backend cookie clear karega.

---

# Phase 13: Responsive Design

## Desktop

```txt
Centered content
Max width around 900px
Chat comfortable width
```

## Mobile

```txt
Full-width form
Stacked layout
Large textarea
Bottom-friendly send button
No horizontal overflow
```

Important:

- inputs screen se bahar nahi jaane chahiye
- chat messages wrap hone chahiye
- buttons readable hone chahiye
- navbar mobile par simple ho

---

# Phase 14: Frontend API Testing

Testing sequence:

```txt
1. Signup
2. Login
3. Start session page open
4. Topic submit
5. Explanation save
6. Session start
7. AI message show
8. User answer send
9. Next AI question show
10. Max rounds complete
11. Complete screen show
12. Logout
```

Test errors:

```txt
Without login
Empty topic
Short explanation
Empty answer
Wrong session ID
Completed session answer
Backend unavailable
```

---

# Phase 15: Later Features

Frontend ka next expansion:

## Final report

```txt
Initial score
Final score
Strong points
Missing points
Resolved gaps
Remaining gaps
Hints used
Ideal explanation
```

## Practice questions

```txt
Question 1 of 5
Answer input
AI feedback
Score
Correct explanation
```

## Dashboard

```txt
Total sessions
Completed sessions
Average score
Weak concepts
Progress trend
```

## History

```txt
Topic
Date
Rounds
Status
Open session
```

## Demo mode

Baad mein:

```txt
Home
→ Try Demo
→ Limited chat
→ Signup prompt
```

---

# Exact Development Order

## Phase 1: Routing

- Check React Router
- Create routes
- Create page files
- Render placeholder pages

## Phase 2: Auth UI

- Login form
- Signup form
- Validation
- API service
- Backend connection
- Redirects

## Phase 3: Auth protection

- Auth context
- ProtectedRoute
- Logout
- Cookie handling

## Phase 4: Start Session

- Explanation form
- Max rounds select
- Save explanation
- Start session
- Redirect to chat

## Phase 5: Chat

- Chat messages
- User input
- Send answer
- AI response
- Round counter
- Loading/error handling

## Phase 6: Completion

- Detect completed status
- Disable input
- Completion screen
- Start new session button

## Phase 7: Polish

- Responsive layout
- Consistent styles
- Empty/loading/error states
- Remove Vite demo code
- Final frontend testing

## Phase 8: Later Product Features

- Final report
- Practice questions
- Dashboard
- History
- Demo mode
- Voice input

# Frontend MVP Success Criteria

Frontend MVP complete tab hoga jab:

```txt
User signup kar sake
→ login kar sake
→ topic explain kar sake
→ session start kar sake
→ AI questions dekh sake
→ answers bhej sake
→ next questions receive kar sake
→ rounds complete kar sake
→ logout kar sake
```

Abhi sabse pehla frontend task **routing aur placeholder pages** hai. Uske baad login/signup ko backend se connect karenge.