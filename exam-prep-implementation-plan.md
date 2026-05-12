# Exam Prep App — Implementation Plan

> Use this plan to build an interactive study app for **any** Microsoft certification exam.
> Start a new project for each exam. Replace `[EXAM-ID]` and `[EXAM-NAME]` throughout.
> Each phase builds on the previous one — don't skip ahead.

---

## Phase 1: Research & Scaffold

### Goal
Build the knowledge base from official Microsoft sources.

### Prompt
```
I need to pass [EXAM-ID]: [EXAM-NAME]. My workspace is empty at c:\source\[exam-id].

1. Research the official exam page and study guide on learn.microsoft.com
2. Create a README.md with exam overview, domains, weights, and key links
3. Create a detailed study-guide.md with every exam objective and MS Learn module links
4. Create a study-tracker.md with a day-by-day cram plan using READ → DRILL → RECALL methodology (make it generic — no hardcoded dates, use "Day 1, Day 2" etc. relative to exam day)
5. Create a cheat-sheets/ folder with one detailed cheat sheet per exam domain, including:
   - Exact service names, model names, API parameters, SDK classes
   - REST endpoint patterns and URL formats
   - "Numbers to remember" section (minimums, limits, defaults, timeouts)
   - Decision tables: "when to use A vs B" for every choice the exam tests
6. Create a practice-questions.md with 25+ scenario-based multiple choice questions with answers and explanations

Focus on EXAM-LEVEL detail — exact parameter names, model versions, config values, CLI commands. Not conceptual overviews. The exam tests specifics.
```

### Expected Output
```
[exam-id]/
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
- [ ] Every exam objective from the study guide has at least one cheat sheet entry
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
Build a single-file HTML/JS interactive study app (index.html) with a modern dark theme that includes:

1. Flashcards tab — all cards from flashcards.md with:
   - Domain filter buttons
   - Click-to-flip card animation (CSS 3D transform)
   - "Got it" ✓ / "Again" ✗ buttons
   - Auto-star wrong cards, track mastered cards in localStorage
   - "Review Starred" mode for weak cards
   - Progress bar showing position in deck

2. Quiz tab — all questions from practice-questions.md with:
   - Domain filter buttons
   - Choose 10 or 25 question quiz length
   - Immediate feedback with correct answer highlighted + explanation
   - Score shown as pass/fail (≥70%)
   - Wrong-answer review panel after quiz completion
   - Quiz history saved in localStorage

3. Mind Maps tab — one interactive mind map per domain using markmap:
   - Use markmap-autoloader from CDN (https://cdn.jsdelivr.net/npm/markmap-autoloader@latest)
   - Set manual rendering mode — render only when user expands a domain
   - Full topic hierarchy matching the cheat sheets content
   - Include all "when to use" decisions as branches in the tree

4. Progress tab — localStorage-based tracking:
   - Cards mastered vs need review per domain (color-coded progress bars)
   - Total reviews count
   - Last quiz score
   - Study streak counter (consecutive days studied)
   - Reset all progress button

5. Cheat Sheets tab — collapsible reference sections:
   - All content from the cheat-sheets/ folder converted to HTML tables
   - Domain filter buttons
   - Click to expand/collapse each section

Requirements:
- Mobile-friendly (responsive, touch-friendly buttons)
- Works offline (no server needed, just open the file)
- No dependencies except markmap CDN
- All data embedded in the JS (FLASHCARDS array, QUIZ_QUESTIONS array, etc.)
- Dark theme, clean UI, smooth animations
- Persistent state via localStorage (prefix keys with exam ID to avoid conflicts)
```

### Expected Output
```
[exam-id]/
├── index.html    ← The app (single file, ~1500 lines)
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

---

## Phase 4: Content Sync & Gap Analysis

### Goal
Ensure 100% content synchronicity between markdown files, app data, and mind maps.

### Prompt
```
Do a systematic content audit:

1. Count flashcards, quiz questions, and cheat sheet sections per domain
2. Identify any domain underrepresented relative to its exam weight
3. Check that every "when to choose A vs B" decision in the cheat sheets has a matching flashcard AND quiz question
4. Check every topic in the mind maps matches the cheat sheets (full content synchronicity)
5. Look for topics where we have the "what" but not the "when to choose" — add decision context
6. Look for topics where definitions are too thin for exam-level questions (e.g., "facetable" should explain WHEN to use it, not just what it means)
7. Add missing content to fix ALL gaps found
8. Ensure flashcard IDs are sequential and don't skip
9. Update README with current content counts
```

### Quality Check
- [ ] Flashcard count per domain is proportional to exam weight (±20%)
- [ ] Every decision table in cheat sheets has matching flashcard(s) + quiz Q(s)
- [ ] Mind map nodes match cheat sheet headings 1:1
- [ ] No "thin" definitions remaining — all have "when to use" context
- [ ] README content counts match actual counts

---

## Phase 5: Final Expert Panel Review

### Goal
Catch remaining errors, validate methodology, confirm exam readiness.

### Prompt
```
Final expert panel review across all content:

1. Certification exam coach: Are quiz questions scenario-based like the real exam? Are distractors realistic (wrong answers that SOUND right)? Any missing question types?
2. Cognitive scientist: Does the study method support active recall and spaced repetition? Is the READ → DRILL → RECALL cycle sound? Any methodology improvements?
3. Technical expert: Verify all facts against current Microsoft docs. Flag anything outdated or incorrect. Check exact parameter names, model versions, API paths.
4. UX designer: Is the app mobile-friendly? Any friction in the flashcard flow? Are domain filters intuitive?

Fix everything found. Then give me a summary of:
- Total flashcards, quiz questions, cheat sheet sections, mind maps
- Content coverage per domain vs exam weight
- Any remaining gaps and whether they matter for passing
```

### Quality Check
- [ ] No factual errors remaining
- [ ] Quiz distractors are realistic (not obviously wrong)
- [ ] Study tracker references the app (not stale file references)
- [ ] Study tracker uses generic dates (Day 1, Day 2... not specific calendar dates)
- [ ] All content counts in README are accurate

---

## Phase 6: Ship It

### Goal
Deploy to GitHub Pages and share with team.

### Prompt
```
1. Initialize git repo if needed
2. Push to https://github.com/demo-ninjas/pass_ai_102.git
3. Update README.md with:
   - Link to the live GitHub Pages app at the top
   - Feature table (flashcards, quiz, mind maps, progress, cheat sheets) with counts
   - Full repository file structure
   - "How to Study" section with step-by-step instructions
   - Contributing note
4. Give me a short Teams message I can share with colleagues about the app
5. Give me a prompt I can use to evaluate whether this app + Microsoft Learn combined is superior to Microsoft Learn alone for passing this exam
```

### Post-Deploy
- [ ] Enable GitHub Pages: repo Settings → Pages → Source: main, folder: / (root) → Save
- [ ] Test the live URL: `https://[org].github.io/pass_[exam-id]/`
- [ ] Test on mobile
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
| Phase 1: Research & Scaffold | 15-20 min |
| Phase 2: Expert Review & Traps | 10-15 min |
| Phase 3: Build App | 15-20 min |
| Phase 4: Content Sync | 10-15 min |
| Phase 5: Final Review | 5-10 min |
| Phase 6: Ship | 5 min |
| **Total** | **~60-90 min per exam** |
