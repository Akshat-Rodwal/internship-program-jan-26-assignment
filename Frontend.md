# **Frontend Engineer Assignment (No Code, High-Quality UI Focus)**

## **Evaluation Criteria**

* Frontend tech selection and reasoning (framework, state, data fetching)
* UI architecture (routing, component design, reusable patterns)
* API calling strategy (error handling, retries, abort, pagination)
* Browser-level caching + offline-friendly patterns
* Debugging + observability (logging, tracing, error boundaries)
* Security basics on client (token handling, safe downloads, XSS considerations)
* UX quality for async jobs (progress, partial results, resilience)

---

## **Problem 1: Video-to-Notes Platform (Frontend System Design)**

**Goal:** Upload video → job runs async → user sees status + outputs: Summary.md, highlights (timestamps), assets. [READ MORE ABOUT THE PROJECT](./Video-summary-platform.md)

**Your solution must include**

* **Screens:** Upload, Jobs list, Job detail (status/logs), Results (markdown + highlights)
* **UI states:** loading, queued, processing, success, failed, retry, partial output
* **API calling plan:** how you poll/stream job progress (polling vs SSE), abort on navigation
* **Caching:** what to cache in browser (job list, job detail, results), TTL strategy, invalidation
* **Debugging plan:** how you would debug “stuck processing” from frontend side (network logs, correlation id display)

**Your Solution for problem 1:**

1️⃣ Screens

1. Upload Screen

Drag & drop video upload

File validation (type, size limit)

Upload progress bar

Start processing CTA

Upload error handling

2. Jobs List

All jobs with status badge (Queued / Processing / Success / Failed)

Filter + search

Retry button for failed jobs

Last updated timestamp

3. Job Detail Screen

Job metadata (created at, file name, size)

Live status indicator

Progress bar (percentage or step-based)

Logs panel (collapsible)

Cancel job button

4. Results Screen

Markdown preview (rendered Summary.md)

Highlights with clickable timestamps

Asset gallery with download buttons

2️⃣ UI States

For every job:

Loading (skeleton UI)

Queued

Processing (with progress %)

Partial output available

Success

Failed (error message + retry)

Cancelled

Each state has:

Status badge color

Toast notifications

Clear CTA (Retry / View Result)

3️⃣ API Calling Plan

Job Creation
POST /jobs

Progress Tracking
Hybrid approach:

Default polling every 5 seconds

If backend supports → Server Sent Events (SSE)

Polling stops when:

Job success / failed

User navigates away (AbortController)

Tab inactive (Page Visibility API)

Retries:

Exponential backoff (max 3 retries)

Abort:

AbortController on component unmount

4️⃣ Caching Strategy

Using React Query + IndexedDB

Data	TTL
Job list	2 min
Job detail	1 min
Results	10 min

Invalidation triggers:

New job created

Retry action

Manual refresh

Status = processing → force refetch

5️⃣ Debugging “Stuck Processing”

Frontend debugging plan:

Check network polling requests

Display correlation ID in UI

Show last updated timestamp

Add collapsible raw API response panel

Compare job ID with backend logs

## **Problem 2: LinkedIn Automation Platform (Frontend System Design)**

**Goal:** Connect LinkedIn → persona setup → draft preview → approve → schedule → posting history. [READ MORE ABOUT THE PROJECT](./linkedin-automation.md)

**Your solution must include**

* **Screens:** Connect, Persona editor, Drafts (3 variants), Approval, Scheduler, Post history
* **Form UX:** persona inputs validation, topic input rules, guardrails for scheduling
* **API calling:** draft generation request lifecycle, optimistic UI vs strict confirmation
* **Caching:** drafts caching, schedule list caching, refetch triggers after approval/post
* **Debugging:** how you surface posting failures to user and capture details for support

**Your Solution for problem 2:**

1️⃣ Screens

LinkedIn Connect screen

Persona Editor

Draft Variants (3 drafts view)

Approval screen

Scheduler

Post History

2️⃣ Form UX

Persona:

Required fields (tone, industry, audience)

Character counter

Inline validation errors

Topic:

Minimum 10 characters

Spam keyword validation

No scheduling in past

Max 3 posts per day

Confirmation modal before scheduling.

3️⃣ API Lifecycle

Draft generation:
POST /drafts

Lifecycle:

Loading spinner

Skeleton placeholders

Success → render 3 drafts

Error → retry CTA

Approval:

Optimistic UI update

Posting:

Strict confirmation from backend

Abort draft generation on navigation.

4️⃣ Caching

Drafts cached (5 min TTL)

Post history (2 min TTL)

Invalidate after approval or posting

5️⃣ Debugging Failures

Show error reason

Timestamp

Correlation ID

Retry button

“Report Issue” button sends:

Draft ID

API error

User environment info

## **Problem 3: DOCX Template → Bulk Generator (Frontend System Design)**

**Goal:** Upload template → review fields → single generate → bulk via CSV → ZIP download + per-row report. [READ MORE ABOUT THE PROJECT](./docs-template-output-generation.md)

**Your solution must include**

* **Screens:** Template upload, Field review/editor, Single fill form, Bulk upload, Bulk run status, Report table, Downloads
* **Field UI:** field types (text/number/date), required/default, inline validation
* **Bulk UX:** CSV upload constraints, mapping UI (optional), progress + partial success
* **Browser caching:** template metadata caching, field schema caching, bulk report pagination caching
* **Downloads:** safe download UX (signed URL flow assumed), progress indicator

**Your Solution for problem 3:**

1️⃣ Screens

Template Upload

Field Review

Single Fill Form

Bulk CSV Upload

Bulk Run Status

Report Table

Downloads

2️⃣ Field UI

Field types:

Text

Number

Date

Each field supports:

Required toggle

Default value

Inline validation

Real-time error display

3️⃣ Bulk UX

CSV rules:

Max 5MB

Header validation

Preview first 5 rows

Optional mapping UI:

Map CSV column → template field

Progress UI:

Percentage

Success / failed counter

Partial success:

Download per-row error report

4️⃣ Browser Caching

Template metadata (10 min)

Field schema (15 min)

Bulk reports paginated cache

5️⃣ Safe Downloads

Backend provides signed URL

Show download progress

Disable duplicate clicks

Handle expired URL gracefully

## **Problem 4: Character-Based Video Series Generator (Frontend System Design)**

**Goal:** Define characters once → create episode from story → view episode package (script/scenes/assets/render plan). [READ MORE ABOUT THE PROJECT](./char-based-video-generation.md)

**Your solution must include**

* **Screens:** Character library, Relationship editor, Episode creator, Episode detail (scenes), Asset gallery
* **Consistency UX:** show “locked character profile” per episode, version badges
* **API calling:** long-running generation job UI (progress, resume)
* **Caching:** character library caching, episode package caching, asset thumbnails caching

**Your Solution for problem 4:**

1️⃣ Screens

Character Library

Relationship Editor

Episode Creator

Episode Detail

Asset Gallery

2️⃣ Consistency UX

Locked character badge per episode

Version tags (v1, v2)

Warning if character updated after episode creation

3️⃣ Long Running Job UI

Step-based progress (script → scenes → render)

Percentage indicator

Resume option

Partial preview support

Abort generation on navigation.

4️⃣ Caching

Character library (5 min TTL)

Episode package (10 min TTL)

Asset thumbnails cached by browser

## **Cross-Cutting** 

Answer these in **bullet points** (max 1 page total):

1. **Frontend stack choice**

* EDIT YOUR ANSWER HERE: Framework (Next.js/Vue/etc), state management, router, UI kit, why.

Framework: Next.js (App Router)

State Management: React Query (server state) + Zustand (local state)

UI Kit: Tailwind CSS + ShadCN

Router: Next built-in routing

Why:

SSR + performance

Strong ecosystem

Better data fetching control

Production scalability

2. **API layer design**

* Fetch/Axios choice, typed client generation (OpenAPI), error normalization, retries, request dedupe, abort controllers.
  
Axios with interceptors

OpenAPI typed client generation

Centralized error normalization

Retry with exponential backoff

Request deduplication via React Query

AbortController for cancellation

3. **Browser caching plan**

* What you cache (GET responses, derived state), where (memory, IndexedDB, localStorage), TTL/invalidation rules.
* How you handle “job status updates” without stale UI.
  
Cache:

GET responses

Job lists

Draft previews

Template metadata

Storage:

Memory (React Query)

IndexedDB (persistent)

localStorage (non-sensitive flags only)

TTL rules:

Jobs: 2 min

Drafts: 5 min

Results: 10 min

For job updates:

Force refetch if status = processing

No stale status allowed

4. **Debugging & observability**

* Error boundaries, client-side logging approach, correlation id propagation, “report a problem” payload.
* How you would debug: slow uploads, failed downloads, intermittent 500s.
  
React Error Boundaries

Client logging via Sentry

Correlation ID in headers

“Report Problem” payload includes:

user id

job id

API response

browser info

Debug scenarios:

Slow uploads:

Check network throttling

Check file size

Failed downloads:

Check signed URL expiry

Intermittent 500:

Inspect retry logs

Use correlation ID tracing

5. **Security basics**

* Token storage approach, CSRF considerations (if cookies), XSS avoidance for markdown rendering, safe file download patterns.

Token Handling:

Prefer HttpOnly secure cookies

If JWT → store in memory (not localStorage)

CSRF:

SameSite cookies

CSRF token header validation

XSS:

Sanitize markdown using DOMPurify

Avoid dangerouslySetInnerHTML

Safe downloads:

Signed URLs

No direct file path exposure
