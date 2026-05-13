# Exam Prep App — Implementation Plan

> Use this plan to build an interactive study app for **any** Microsoft certification exam.
> Start a new project for each exam. Replace `[EXAM-ID]` and `[EXAM-NAME]` throughout.
> Each phase builds on the previous one — don't skip ahead.
>
> **Target:** 100% exam objective coverage with at least one flashcard AND one quiz question per bullet in the official "Skills Measured" document.

---

## Phase 0: Get the Official Skills Measured Document

### Goal
Download the exact list of testable objectives before writing any content.

### Prompt
```
I need to pass [EXAM-ID]: [EXAM-NAME]. My workspace is empty at c:\source\[exam-id].

1. Go to the official exam page: https://learn.microsoft.com/en-us/credentials/certifications/[exam-slug]/
2. Find the "Skills Measured" section (or download the study guide PDF)
3. Save the COMPLETE skills measured document as content_covered_in_the_exam.md with proper markdown formatting:
   - H2 per domain with weight percentages
   - H3 per sub-section
   - Bullet per testable objective
4. Count and list every bullet — this is the coverage target

This document is the single source of truth. Every bullet MUST have content by the end.
```

### Expected Output
```
[exam-id]/
└── content_covered_in_the_exam.md    ← The coverage checklist
```

### Quality Check
- [ ] Every domain has its weight percentage
- [ ] Every sub-section has numbered bullets
- [ ] Total bullet count is clear (aim: 80-120 objectives for most exams)

---

## Phase 1: Research & Scaffold

### Goal
Build the knowledge base from official Microsoft sources.

### Prompt
```
Using the content_covered_in_the_exam.md as your checklist:

1. Create a README.md with exam overview, domains, weights, and key links
2. Create a detailed study-guide.md with every exam objective and MS Learn module links
3. Create a study-tracker.md with a day-by-day cram plan using READ → DRILL → RECALL methodology (generic — "Day 1, Day 2" etc., no hardcoded dates)
4. Create a cheat-sheets/ folder with one detailed cheat sheet per exam domain, including:
   - Exact service names, model names, API parameters, SDK classes
   - REST endpoint patterns and URL formats
   - "Numbers to remember" section (minimums, limits, defaults, timeouts)
   - Decision tables: "when to use A vs B" for every choice the exam tests
5. Create a practice-questions.md with 25+ scenario-based multiple choice questions with answers and explanations

Focus on EXAM-LEVEL detail — exact parameter names, model versions, config values, CLI commands. Not conceptual overviews. The exam tests specifics.

CRITICAL: Cross-reference every bullet in content_covered_in_the_exam.md. If a bullet has no cheat sheet entry, add one.
```

### Expected Output
```
[exam-id]/
├── content_covered_in_the_exam.md  ← From Phase 0
├── README.md
├── study-guide.md
├── study-tracker.md
├── practice-questions.md
└── cheat-sheets/
    ├── 1-[domain-name].md
    ├── 2-[domain-name].md
    └── ... (one per domain)
```

### Quality Check
- [ ] Every exam objective from the skills measured doc has at least one cheat sheet entry
- [ ] Every cheat sheet has a "Numbers to Remember" section
- [ ] Every cheat sheet has at least one "When to Use A vs B" decision table
- [ ] MS Learn links are real and working
- [ ] Practice questions are scenario-based ("You need to..."), not definitional ("What is...")

---

## Phase 2: Expert Review & Traps

### Goal
Verify accuracy and build the "tricky stuff" content that separates pass from fail.

### Prompt
```
Review all the content we just created with an expert panel approach:

1. Verify all technical claims against the official Microsoft docs. Fix any errors.
2. Create a traps-and-distinctions.md covering:
   - "Sounds the same" service confusions (things easily mixed up)
   - Parameter traps (common wrong answers that sound right)
   - API endpoint traps (different URL patterns per service)
   - Lifecycle traps (what's immutable after creation, what requires rebuild)
   - "When to use A vs B" decision scenarios the exam loves to test
   - Numbers that trip people up (minimums, limits, defaults)
   - The exact format: scenario | correct answer | why NOT the other answer
3. Create a flashcards.md with 130+ Q&A cards across all domains:
   - Cover every "when to choose" decision
   - Include exact model names, parameter ranges, SDK classes, CLI commands
   - Include cards for every trap in the traps file
   - Instructions at top: cover answer column, say answer out loud, star wrong ones
```

### Expected Output
```
[exam-id]/
├── ... (existing files)
├── traps-and-distinctions.md    ← NEW
└── flashcards.md                ← NEW
```

### Quality Check
- [ ] Every "trap" has a scenario + correct answer + "why NOT the other"
- [ ] Flashcards cover all 6 (or however many) domains proportionally to exam weight
- [ ] No factual errors (verified against docs)
- [ ] At least 10 "when to choose A vs B" flashcards

---

## Phase 3: Build the Interactive App

### Goal
Create a single-file study app that works in any browser, saves progress locally, deploys to GitHub Pages.

### Prompt
```
Build a single-file HTML/JS interactive study app (index.html) using the design specs from the Architecture Notes section below. Include:

1. EXAM_CONFIG block at the top of the file:
   - id, code, name, url, examDate (null or 'YYYY-MM-DD'), passingScore, ghPages
   - Everything else derives from this config (title, localStorage prefix, header, countdown)

2. Dashboard (landing page) with:
   - Study flow guidance: Explore (Mind Maps) → Review (Cheat Sheets) → Practice (Flashcards) → Validate (Quiz)
   - Mastery ring SVG (% of cards mastered)
   - 30-day activity heat map (color-coded by XP earned)
   - Quick stats: mastered count, starred count, day streak, last quiz score

3. Flashcards tab — all cards from flashcards.md with:
   - Domain filter buttons
   - Click-to-flip card animation (CSS 3D transform)
   - "Got it" ✓ / "Again" ✗ buttons (+5 XP each)
   - Auto-star wrong cards, track mastered cards in localStorage
   - "Review Starred" mode for weak cards
   - Progress bar showing position in deck

4. Quiz tab — all questions from practice-questions.md with:
   - BOTH single-answer AND multi-select questions (ans can be number OR array)
   - "Select N" indicator for multi-select, Submit button, checkbox inputs
   - Domain filter buttons + "X questions available" counter
   - Choose 10 or 25 question quiz length
   - Immediate feedback with correct answer highlighted + explanation (+10 XP each)
   - Score shown as pass/fail (≥70%)
   - Wrong-answer review panel after quiz completion (handles both single and multi-select)
   - Quiz history saved in localStorage
   - CRITICAL: render options with document.createTextNode() not innerHTML (prevents HTML tags in options like <prosody> from being parsed)

5. Mind Maps tab — one interactive mind map per domain using markmap programmatic API:
   - DO NOT use markmap-autoloader (it doesn't work with dynamic content)
   - Lazy-load d3 → markmap-view → markmap-lib via sequential promise chain
   - Render only when user expands a domain (container must be visible first)
   - SVG must have explicit height (600px) not min-height
   - Fullscreen toggle button per map
   - Override SVG text fill with var(--text) for theme compatibility
   - See Architecture Notes for exact implementation pattern

6. Progress tab — localStorage-based tracking:
   - Cards mastered vs need review per domain (color-coded progress bars)
   - Total reviews count
   - Last quiz score
   - Study streak counter (consecutive days studied)
   - Reset all progress button (only clears THIS exam's keys, not all localStorage)

7. Cheat Sheets tab — collapsible reference sections:
   - All content from the cheat-sheets/ folder converted to HTML tables
   - Domain filter buttons
   - Click to expand/collapse each section

8. Navigation:
   - Desktop: Fixed left sidebar (220px) with icon + label buttons
   - Mobile (≤768px): Bottom tab bar with icons, sidebar hidden, hamburger toggle
   - Theme toggle (🌙/☀️) with dark/light CSS custom properties
   - Zen Mode (🧘) hides sidebar + top bar for focused study
   - XP badge in top bar

Requirements:
- Mobile-friendly (responsive at 768px and 375px breakpoints)
- Works offline (no server needed, just open the file)
- No dependencies except markmap CDN (lazy-loaded on first use)
- All data embedded in the JS (FLASHCARDS array, QUIZ_QUESTIONS array, etc.)
- Inter font from Google Fonts CDN
- Dark + light theme via CSS custom properties + data-theme attribute
- Persistent state via localStorage (prefix keys with EXAM_CONFIG.id)
- Flashcard IDs auto-normalized to sequential 1,2,3... at runtime with localStorage migration
```

### Data Structures
```js
const EXAM_CONFIG = { id, code, name, url, examDate, passingScore, ghPages };
const DOMAINS = [{id, name, weight, color}];
const FLASHCARDS = [{id, d, q, a}];
const QUIZ_QUESTIONS = [{d, q, opts, ans, exp}];  // ans: number (single) or array (multi-select)
const CHEATSHEETS = [{d, title, content}];          // content is HTML string
const MINDMAPS = [{id, title, color, md}];          // md is markdown string for markmap
```

### Expected Output
```
[exam-id]/
├── index.html    ← The app (single file, ~2000-2500 lines)
└── ... (existing files)
```

### Quality Check
- [ ] All flashcards from flashcards.md are in the FLASHCARDS array
- [ ] All practice questions are in the QUIZ_QUESTIONS array
- [ ] All cheat sheet content is in the CHEATSHEETS array
- [ ] Mind maps cover every cheat sheet topic
- [ ] Domain filters work on all tabs
- [ ] Progress persists across page refreshes
- [ ] Looks good on mobile (test at 375px width)
- [ ] Mind maps render when expanded (not all on page load)
- [ ] Quiz options with HTML-like content (e.g. `<prosody>`) render as text, not parsed as HTML
- [ ] Multi-select quiz questions show checkboxes and a Submit button
- [ ] Theme toggle works and persists across refreshes
- [ ] Zen mode hides all chrome

---

## Phase 4: Coverage Verification (100% Target)

### Goal
Ensure EVERY bullet in `content_covered_in_the_exam.md` has at least one flashcard AND one quiz question.

### Prompt
```
Do a systematic 100% coverage audit:

1. Read content_covered_in_the_exam.md and number every bullet point (the testable objectives)
2. For EACH numbered bullet, check:
   - Is there at least one flashcard covering it? (cite the FC ID)
   - Is there at least one quiz question covering it? (cite the quiz Q)
3. Produce a coverage table:
   | # | Objective | FC? | Quiz? | Status (✅/🟡/🔴) |
4. For every 🟡 (partial) or 🔴 (missing) item:
   - Add a flashcard to the FLASHCARDS array
   - Add a quiz question to the QUIZ_QUESTIONS array
   - For "minimize effort" scenarios, add a quiz Q where the constraint changes the answer
   - For "select two" scenarios, add a multi-select quiz Q (ans as array)
5. Count flashcards and quiz questions per domain
6. Check proportionality vs exam weight (±20% tolerance)
7. If any domain is significantly over/under-represented, rebalance
8. Ensure flashcard IDs are sequential
9. Update README with current content counts
```

### Coverage Targets
| Metric | Minimum |
|--------|---------|
| Exam objectives with ✅ (FC + Quiz) | **100%** |
| Flashcards per domain | Proportional to exam weight ±20% |
| Quiz questions total | **80+** (at least 1 per objective) |
| Multi-select quiz questions | **At least 5** |
| "Minimize effort" constraint questions | **At least 5** |
| Scenario-based (not definitional) quiz questions | **≥75%** |

### Quality Check
- [ ] Coverage table shows ✅ for every bullet in the skills measured doc
- [ ] Zero 🔴 (missing) items remain
- [ ] Zero 🟡 (partial) items remain
- [ ] Flashcard count per domain is proportional to exam weight
- [ ] README content counts match actual counts
- [ ] Quiz includes both single-answer and multi-select question types

---

## Phase 5: Expert Panel Review & Practice Assessment Comparison

### Goal
Catch remaining errors, validate against real exam question patterns, confirm exam readiness.

### Prompt
```
Final expert panel review across all content:

1. Certification exam coach:
   - Are quiz questions scenario-based like the real exam? Count definitional vs scenario-based (target: ≥75% scenario).
   - Are distractors realistic (wrong answers that SOUND right)?
   - Do we have "minimize effort/cost" constraint questions where the constraint changes the answer?
   - Do we have multi-select "Select TWO" questions?
   - Do we have API workflow/step-ordering questions?
   - Do we have "Portal vs SDK vs REST API" decision questions?
   - Any missing question types?
   - Check for duplicate/near-duplicate quiz questions — remove them.

2. Cognitive scientist: Does the study method support active recall and spaced repetition? Is the READ → DRILL → RECALL cycle sound?

3. Technical expert:
   - Verify all facts against current Microsoft docs. Flag anything outdated or incorrect.
   - Check exact parameter names, model versions, API paths.
   - Check for deprecated services/features — mark them clearly.
   - Check SSML namespace (https vs http), SDK class names, endpoint URL patterns.

4. UX designer: Is the app mobile-friendly? Any friction in the flashcard flow? Are domain filters intuitive?

5. Content deduplication:
   - Find and remove duplicate flashcards (same question, different IDs)
   - Find and remove near-duplicate quiz questions
   - Verify all URLs (GitHub Pages, MS Learn links, exam page links) are consistent

Fix everything found. Then give me:
- Total flashcards, quiz questions, cheat sheet sections, mind maps
- Content coverage per domain vs exam weight
- Any remaining gaps and whether they matter for passing
- Final coverage table: all objectives ✅/🟡/🔴
```

### Compare Against Official Practice Assessment
```
Take the official Microsoft practice assessment for [EXAM-ID]:
https://learn.microsoft.com/en-us/credentials/certifications/[exam-slug]/practice/assessment?assessment-type=practice&assessmentId=[id]

For each question encountered, note:
- Question pattern (scenario, multi-select, ordering, code completion, etc.)
- Topic tested
- Whether our app covers this topic

Add quiz questions and flashcards for any patterns or topics we're missing.
```

### Quality Check
- [ ] No factual errors remaining
- [ ] No duplicate flashcards or quiz questions
- [ ] Quiz distractors are realistic (not obviously wrong)
- [ ] ≥75% of quiz questions are scenario-based
- [ ] At least 5 multi-select questions
- [ ] At least 5 "minimize effort" constraint questions
- [ ] Study tracker references the app with correct URL
- [ ] Study tracker uses generic dates (Day 1, Day 2... not specific calendar dates)
- [ ] All content counts in README are accurate
- [ ] All URLs consistent (GitHub Pages, repo, exam page)

---

## Phase 6: Ship It

### Goal
Deploy to GitHub Pages and share with team.

### Prompt
```
1. Initialize git repo if needed
2. Push to https://github.com/[org]/pass_[exam-id].git
3. Update README.md with:
   - Link to the live GitHub Pages app at the top
   - Feature table (flashcards, quiz, mind maps, progress, cheat sheets) with accurate counts
   - Full repository file structure
   - "How to Study" section with step-by-step instructions
   - Contributing note
4. Verify all URLs match (EXAM_CONFIG.ghPages, README links, study-tracker links)
5. Give me a short Teams message I can share with colleagues about the app
```

### Post-Deploy
- [ ] Enable GitHub Pages: repo Settings → Pages → Source: main, folder: / (root) → Save
- [ ] Test the live URL: `https://[org].github.io/pass_[exam-id]/`
- [ ] Test on mobile
- [ ] Verify quiz multi-select works
- [ ] Verify mind maps render (expand one, check text is visible)
- [ ] Verify theme toggle works
- [ ] Share with team

---

## Exam-Specific Tips

When using these prompts, emphasize the key "decision areas" for each exam:

| Exam | High-Value "A vs B" Decisions to Focus On |
|------|-------------------------------------------|
| **AZ-104** (Azure Admin) | NSG vs ASG vs Firewall vs WAF, Storage tiers (hot/cool/archive) vs access tiers, VM availability sets vs zones vs scale sets, VPN Gateway vs ExpressRoute vs peering, Azure Policy vs RBAC vs Blueprints, Load Balancer vs App Gateway vs Front Door vs Traffic Manager |
| **AZ-204** (Azure Developer) | App Service vs Functions vs Container Apps vs AKS, Cosmos DB consistency levels (strong → eventual), Blob lease vs snapshot vs versioning, Azure Cache vs CDN, Service Bus vs Event Grid vs Event Hub vs Queue Storage, Managed Identity vs SAS vs connection string |
| **AZ-305** (Azure Architect) | Availability Zones vs Sets vs Regions, RTO/RPO strategies, Azure AD B2B vs B2C, Hub-spoke vs mesh networking, SQL Database vs SQL MI vs SQL on VM vs Cosmos DB, Synapse vs Databricks vs HDInsight |
| **AZ-400** (DevOps) | Deployment slots vs blue-green vs canary vs rolling, YAML vs Classic pipelines, Artifacts vs Packages, Branch policies vs pull request triggers, Azure Boards vs GitHub Issues, SonarQube vs WhiteSource vs OWASP ZAP |
| **AZ-500** (Azure Security) | Key Vault vs managed identity vs service principal, Defender for Cloud plans, Sentinel workbooks vs analytics rules vs playbooks, Conditional Access vs MFA vs PIM, Private Link vs Service Endpoint vs Firewall rules, CMK vs SSE vs TDE |
| **DP-203** (Data Engineer) | Dedicated SQL vs Serverless SQL vs Spark pool, Star schema vs snowflake, Partition strategies (hash vs round-robin vs range), Data Factory vs Synapse pipelines, Delta Lake vs Parquet vs CSV, Slowly changing dimensions (Type 1 vs 2 vs 3) |
| **DP-300** (Database Admin) | Azure SQL DB vs Managed Instance vs SQL on VM, DTU vs vCore purchasing, Active geo-replication vs failover groups, TDE vs Always Encrypted vs Dynamic Data Masking, Elastic pools vs single databases, Automated vs long-term backup retention |
| **AI-900** (AI Fundamentals) | Lighter version of AI-102 — same services at conceptual level. Focus on "which service for which task" and Responsible AI principles |
| **SC-900** (Security Fundamentals) | Authentication vs authorization, Azure AD vs on-premises AD, Conditional Access vs MFA, Defender vs Sentinel vs Monitor, Zero Trust principles, Compliance Manager vs Purview |

---

## Architecture Notes


## Architecture & Design Notes

### Why Single HTML File?
- Zero install friction (just open in browser)
- Works offline (no server needed)
- GitHub Pages deploys instantly
- Works on phone for study-on-the-go
- All state in localStorage (survives page refresh)
- Easy to fork and customize

### localStorage Key Strategy
Prefix all keys with exam ID to avoid conflicts if studying for multiple exams:
```
[exam-id]_starred, [exam-id]_mastered, [exam-id]_reviews
[exam-id]_quizHistory, [exam-id]_streak, [exam-id]_theme
[exam-id]_xp, [exam-id]_activity
```

### Visual Design Specifications

Based on cognitive science research for learning tools:

| Element | Spec | Rationale |
|---------|------|-----------|
| **Font** | Inter (Google Fonts CDN) | Highly legible at all sizes, modern/clean |
| **Base dark** | #0F172A (Deep Navy) | Blue linked to focus/productivity |
| **Base light** | #FDFDFD (Off-white) | Less eye strain than pure white |
| **Success** | #76BA99 (Sage Green) | Calm progress feeling |
| **Attention** | #FF8C42 (Muted Amber) | Grabs attention without panic |
| **Primary accent** | #818CF8 / #6366F1 (Indigo) | Primary actions |
| **Error/wrong** | #FB7185 (Soft Red) | Not harsh, but clear |
| **Border radius** | 12px | Modern, friendly feel |
| **Shadows** | `0 4px 24px rgba(0,0,0,.3)` dark / `.06` light | Cards "float" to signal focus |
| **Line height** | 1.55–1.6 | Prevents text crowding |
| **Spacing** | 24–28px content padding, 12px card gaps | Generous whitespace reduces cognitive load |

### Layout Pattern: "Multimodal Knowledge Hub"

The app follows the **Source-to-View** pattern — one dataset rendered in multiple modes:

| Mode | Pattern | UX Purpose |
|------|---------|------------|
| Dashboard | Learning Hub | Status overview + study flow guidance |
| Mind Maps | Infinite Canvas | Big-picture connections |
| Cheat Sheets | High-Density Grid | Quick reference review |
| Flashcards | Minimalist Overlay | Active recall focus |
| Quiz | Progressive Disclosure | Validation testing |

**Study Flow** (shown on dashboard):
1. **Explore** → Mind Maps (see connections)
2. **Review** → Cheat Sheets (familiarize)
3. **Practice** → Flashcards (active recall)
4. **Validate** → Quiz (test mastery)

### Navigation Pattern

**Desktop**: Fixed left sidebar (220px) with icon + label buttons
**Mobile (≤768px)**: Bottom tab bar with icons only, sidebar hidden

Features:
- Dashboard as landing page (not flashcards)
- Zen Mode toggle (🧘): hides sidebar + top bar chrome for deep focus
- XP badge in top bar for quiet gamification
- Theme toggle (🌙/☀️) in top bar

### Markmap Integration — Pitfalls & Solutions

> These lessons were learned the hard way during the AI-900 build. Follow this guide exactly to avoid blank mind maps.

**Pitfall 1: markmap-autoloader doesn't work with dynamic content**
- `markmap-autoloader` scans for `<script type="text/template">` in the DOM
- `<script>` tags inserted via `innerHTML` are **inert** — the browser ignores them entirely
- Even `document.createElement('script')` works for the DOM node, but the autoloader still fails because it renders at 0×0 inside `display:none` containers
- **Solution**: Don't use `markmap-autoloader`. Use the programmatic API instead.

**Pitfall 2: Correct CDN URLs**
```html
<!-- Load in this exact order via sequential promise chain -->
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdn.jsdelivr.net/npm/markmap-view"></script>
<script src="https://cdn.jsdelivr.net/npm/markmap-lib@0.18.12/dist/browser/index.iife.js"></script>
```
- `markmap-view` needs `d3` as a global — load d3 first
- `markmap-lib` browser build is at `dist/browser/index.iife.js` (NOT `dist/browser/index.js`)
- Both libraries expose their API on `window.markmap`
- **Do NOT guess CDN paths** — verify with `https://data.jsdelivr.com/v1/packages/npm/PACKAGE@VERSION`

**Pitfall 3: SVG needs explicit dimensions BEFORE render**
- markmap measures the SVG to calculate layout
- If the SVG is inside a hidden (`display:none`) container, it measures as 0×0
- **Solution**: Only call `Markmap.create()` AFTER the container is `display:block` and SVG has `height:600px` (not `min-height`, not `auto`)

**Pitfall 4: Dark theme text is invisible**
- markmap renders text in dark colors by default (designed for white backgrounds)
- On a dark theme, the text is invisible
- **Solution**: Override with CSS `fill` on `svg text` elements. Also update inline fills when theme toggles.

**Working implementation pattern**:
```js
// Lazy-load scripts on first use
function loadScript(url) {
  return new Promise((res, rej) => {
    const s = document.createElement('script');
    s.src = url; s.onload = res; s.onerror = rej;
    document.head.appendChild(s);
  });
}

// Load deps once, cache the promise
let depsPromise = null;
function loadDeps() {
  if (!depsPromise) {
    depsPromise = loadScript('https://cdn.jsdelivr.net/npm/d3@7')
      .then(() => loadScript('https://cdn.jsdelivr.net/npm/markmap-view'))
      .then(() => loadScript('https://cdn.jsdelivr.net/npm/markmap-lib@0.18.12/dist/browser/index.iife.js'));
  }
  return depsPromise;
}

// Track rendered maps to avoid re-rendering
const mmRendered = {};

// Render one mind map (call ONLY when container is visible)
function renderMindmap(id, containerId, markdown) {
  if (mmRendered[id]) return;
  const body = document.getElementById(containerId);
  body.innerHTML = '<p>Loading mind map…</p>';
  loadDeps().then(() => {
    const { Transformer, Markmap } = window.markmap;
    const { root } = new Transformer().transform(markdown);
    body.innerHTML = '';
    const svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
    svg.style.width = '100%';
    svg.style.height = '600px';  // MUST be explicit, not min-height
    body.appendChild(svg);
    const mm = Markmap.create(svg, { initialExpandLevel: -1 }, root);
    setTimeout(() => mm.fit(), 100);  // auto-zoom after layout
    mmRendered[id] = true;
  }).catch(e => {
    body.innerHTML = '<p>Failed to load. Check internet connection.<br>' + e + '</p>';
  });
}
```

**Required CSS for theme compatibility**:
```css
.mm-body svg text { fill: var(--text) !important; font-size: 14px !important; }
.mm-body svg .markmap-link { stroke-opacity: .6 !important; }
```

**Theme toggle must also update mind map text**:
```js
// In the theme toggle click handler:
const textColor = getComputedStyle(document.documentElement).getPropertyValue('--text').trim();
document.querySelectorAll('.mm-body svg text').forEach(t => { t.style.fill = textColor });
```

### Gamification Without Distraction

| Feature | Implementation | Purpose |
|---------|---------------|---------|
| XP Badge | Top bar, `+5` per card, `+10` per quiz answer | Sense of progress |
| Day Streak | Consecutive days with activity | Consistency motivation |
| Heat Map | 30-day grid on dashboard, color-coded by activity | Visual consistency tracking |
| Mastery Ring | SVG circular progress on dashboard | "How close am I?" at a glance |

All gamification is **non-blocking** — no pop-ups, no level-up screens. Just quiet counters that accumulate.

---

### Content Data Structure
All content lives in JS arrays at the top of the script:
```js
const DOMAINS = [{id, name, weight, color}];
const FLASHCARDS = [{id, d, q, a}];
const QUIZ_QUESTIONS = [{d, q, opts, ans, exp}];
const CHEATSHEETS = [{d, title, content}];  // content is HTML string
const MINDMAPS = [{id, title, color, md}];  // md is markdown string for markmap
```

### Estimated Build Time
| Phase | Time |
|-------|------|
| Phase 0: Get Skills Measured Doc | 5 min |
| Phase 1: Research & Scaffold | 15-20 min |
| Phase 2: Expert Review & Traps | 10-15 min |
| Phase 3: Build App | 15-20 min |
| Phase 4: 100% Coverage Verification | 15-20 min |
| Phase 5: Expert Panel + Practice Assessment | 10-15 min |
| Phase 6: Ship | 5 min |
| **Total** | **~75-100 min per exam** |

### AI-102 Reference Numbers (Achieved)
| Metric | Count |
|--------|-------|
| Exam objectives (bullets in skills doc) | 106 |
| Flashcards | 204 |
| Quiz questions (single + multi-select) | 103 |
| Cheat sheet sections | 33 |
| Mind maps | 6 |
| Coverage: objectives with FC + Quiz | 106/106 (100%) |
