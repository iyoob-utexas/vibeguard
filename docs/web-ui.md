# Web UI Standards

This document expands on the rules in [`AGENTS.md §5.18`](../AGENTS.md) with rationale and failure patterns. The normative source is `AGENTS.md`; this document is the explanation behind it.

---

## 1. Stack choices

### Use modern React

**Rule:** Functional components with hooks only. No class-based components, no Redux, no Angular, no jQuery.

**Why:** Class components require lifecycle boilerplate that hooks eliminate. Redux adds indirection and boilerplate for problems that React Query, SWR, and Context already solve more narrowly. jQuery duplicates browser APIs that have been native since 2015. These patterns generate large, hard-to-refactor files and introduce AI-generated bugs that are difficult to trace.

**What to use instead:**

| Need | Preferred |
|---|---|
| Local state | `useState`, `useReducer` |
| Shared state | React Context, Zustand (if scope justifies) |
| Server data | React Query (TanStack Query) or SWR |
| Side effects | `useEffect` with exhaustive deps |
| DOM manipulation | Native browser APIs |

### Use Tailwind CSS

**Rule:** Utility-first CSS only. No hand-written vanilla CSS files or classes for layout and component styling.

**Why:** Vanilla CSS in AI-generated projects grows unmanageably — duplicate rules, specificity conflicts, dead styles, and no design system enforcing consistency. Tailwind keeps styles co-located with markup, makes components portable, and tree-shakes unused styles at build time.

**Allowed:** CSS Modules or `styled-components` for the rare case where a Tailwind class composition becomes unreadable. Not for general use.

---

## 2. Code structure and modularity

**Rule:** No component file exceeds 300 lines. Logic used in more than one place gets extracted.

**Why:** AI coding tools, when not constrained, will dump entire features — data fetching, business logic, rendering, and local state — into one component file. These files become impossible to review, test, or modify without breaking something else.

**Patterns to enforce:**

- One component per file, one responsibility per component.
- Extract data-fetching logic into a custom hook (`useFoo`).
- Extract business logic into a plain function in `utils/` or `lib/`.
- Extract shared UI patterns (modals, tables, forms) into a `components/` library.
- Page components compose; they do not contain inline logic.

**File structure example:**

```
features/
  invoices/
    InvoiceList.tsx          # renders the list, <300 lines
    InvoiceRow.tsx           # single row, <150 lines
    useInvoices.ts           # data fetching + caching hook
    invoiceUtils.ts          # formatting, filtering, sorting
    invoices.test.tsx        # component tests
```

---

## 3. No hardcoded dynamic content

**Rule:** Labels, messages, error strings, API base URLs, thresholds, and environment-specific values live in constants or config files — never inline in JSX or TypeScript.

**Why:** Hardcoded strings make copy changes require code deployments. Hardcoded URLs mean you can't switch environments without a build. Hardcoded thresholds mean you can't tune behavior without touching rendering logic.

**What this looks like:**

```ts
// Wrong
<Button>Submit Order</Button>
fetch("https://api.myapp.com/orders")

// Right
import { LABELS, API_BASE } from "@/config"
<Button>{LABELS.submitOrder}</Button>
fetch(`${API_BASE}/orders`)
```

Environment URLs belong in `.env` files and are accessed only through a validated config module — never imported directly from `process.env` scattered across components.

---

## 4. Edge states

**Rule:** Every component that loads async data MUST handle: loading, empty, error, partial/stale, and success states. Lists that may grow large MUST paginate or use virtual scroll.

**Why:** In demos and tests, data loads instantly and always succeeds. In production, APIs fail, networks are slow, and datasets grow. A UI that handles only the happy path ships with a hidden set of broken states that users discover after launch.

### Required states for async components

| State | What to show |
|---|---|
| Loading | Skeleton or spinner; disable interactive elements that depend on data |
| Empty | Explanatory message or CTA; not a blank screen |
| Error | Human-readable message; retry action where appropriate |
| Partial/stale | Indicate data may be outdated; show last-updated time if relevant |
| Success | The content |

### Pagination and large data

- Lists that may exceed 50 items MUST paginate or use infinite scroll with an intersection observer.
- MUST NOT `fetch all records` and filter client-side when the dataset is unbounded.
- Virtual lists (TanStack Virtual, react-window) for rows exceeding 500 items where DOM performance matters.
- Always show a loading indicator when fetching the next page.
- Always detect and communicate end-of-list.

---

## 5. Render safety and infinite loops

**Rule:** `useEffect` dependency arrays must be exhaustive. Derived values must not be stored in state. Lint with `eslint-plugin-react-hooks`.

**Why:** A missing dependency in `useEffect` produces stale closures — the effect runs with outdated values. An unnecessary dependency produces infinite loops — state updates trigger the effect, which updates state, which triggers the effect. Both cause subtle bugs that are expensive to diagnose in production.

**Common failure patterns:**

```ts
// WRONG: object literal in deps causes infinite loop
useEffect(() => {
  fetchData(options)
}, [{ id: userId }]) // new object every render

// WRONG: derived value stored in state
const [fullName, setFullName] = useState(`${first} ${last}`)

// WRONG: missing dep causes stale closure
useEffect(() => {
  doSomething(value) // value changes but effect doesn't re-run
}, []) // eslint-plugin-react-hooks would flag this
```

**Correct patterns:**

```ts
// Derived values belong outside state
const fullName = `${first} ${last}`

// Stable refs for callbacks
const onSubmit = useCallback(() => {
  saveData(formValues)
}, [formValues])
```

---

## 6. API call efficiency

**Rule:** Data fetching goes through a caching layer. No fetch calls inside a render path without memoization. No duplicate in-flight requests for the same resource.

**Why:** Without a caching layer, components re-fetch on every mount. Navigating back and forth between pages issues redundant API calls. In applications with concurrent components, the same endpoint may be called dozens of times per second. This runs up API costs, exhausts rate limits, and degrades performance.

**Preferred tools:**

- **React Query (TanStack Query):** Full-featured — caching, deduplication, background refresh, pagination, mutation, optimistic updates.
- **SWR:** Lightweight — good for read-heavy, straightforward caching patterns.

**What these tools provide automatically:**

- Request deduplication: 10 components calling `useQuery("user")` issue one network request.
- Background revalidation: stale data shows immediately; fresh data replaces it silently.
- Cache invalidation after mutations.
- Error and retry handling.

**What to avoid:**

```ts
// WRONG: fetch inside useEffect with no caching
useEffect(() => {
  fetch("/api/user").then(r => r.json()).then(setUser)
}, [userId]) // re-fetches every time userId changes; no dedup

// WRONG: fetch at component top level
const data = await fetch("/api/data") // fetch inside render
```

---

## 7. Server-side vs. client-side data

**Rule:** Authorization, business rules, and data validation are server-side responsibilities. PII, tokens, and secrets must not be over-fetched to the client or embedded in bundles.

**Why:** Anything that reaches the browser is visible. Client-side authorization checks are trivially bypassed by editing JavaScript or intercepting requests. PII in client bundles or local storage is a breach waiting for a misconfiguration.

**Decision table:**

| Data type | Where it lives | Why |
|---|---|---|
| Auth session / JWT | HTTP-only cookie or server session | Cannot be read by client JS |
| User permissions | Fetched from server per request | Client cannot self-authorize |
| PII (email, DOB, SSN) | Server only; send minimum needed | Data minimization |
| API keys / secrets | Server environment variables | Never in client bundles |
| Display preferences | Client state or localStorage | No security consequence |
| Cached public data | Client cache (React Query) | Performance; not sensitive |

**What to audit:**

- No `NEXT_PUBLIC_` or Vite `VITE_` prefixed variable should contain a secret.
- No authorization decision should happen exclusively in a React component.
- No full user record or list should be fetched when only a subset is displayed.

---

## 8. Responsiveness

**Rule:** Layouts must be correct at 320px (small mobile), 768px (tablet), 1024px (desktop), and 1440px (wide). Test at 200% browser zoom.

**Why:** A layout that looks good on a 1440px monitor can be unusable on a phone or overflow horizontally at 768px. AI-generated layouts default to desktop widths because that is what most training examples assume.

**Tailwind breakpoint defaults:**

```
sm:  640px+
md:  768px+
lg:  1024px+
xl:  1280px+
2xl: 1536px+
```

Start with the mobile layout; layer breakpoints upward with `md:`, `lg:`, etc.

**Common failures to prevent:**

- Fixed pixel widths on containers (`w-[800px]`): use `max-w-` with `w-full`.
- Horizontal overflow on mobile: always test with `overflow: auto` on small viewports.
- Text that overflows its container: use `truncate`, `break-words`, or `line-clamp`.
- Tables that don't scroll on mobile: wrap in `overflow-x-auto`.
- Touch targets under 44×44px: use `min-h-11 min-w-11` on interactive elements.

---

## 9. Testing

See [`AGENTS.md §6`](../AGENTS.md) for the complete testing requirements. For web UI specifically:

| Test type | Tool | What it catches |
|---|---|---|
| Visual regression | Chromatic, Percy, or Playwright screenshots | Unintended style changes across deploys |
| Responsiveness | Playwright multi-viewport | Layout breakage at each breakpoint |
| Component unit | Vitest + React Testing Library | Each UI state (loading, empty, error, success) |
| End-to-end | Playwright or Cypress | Critical user paths; destructive actions |
| API integration | MSW (Mock Service Worker) | Frontend-to-backend contract |
| Accessibility | axe-core via jest-axe or Playwright | WCAG violations; keyboard, focus, ARIA |
| Performance | Lighthouse CI, bundlewatch | Core Web Vitals, bundle regression |

**Testing each UI state is not optional.** A component test that only covers the success case leaves loading, empty, and error states unverified — these are the states users see most during incidents.
