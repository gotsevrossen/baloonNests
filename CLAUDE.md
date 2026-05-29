# Claude Design Intelligence — CLAUDE.md

This file encodes your personal web design and UI/UX skills so Claude in VS Code (Claude Code, API, etc.) behaves the same as in claude.ai. It consolidates four skills: `web-design`, `web-taste`, `frontend-design`, and `web-animation` + `ui-ux-pro-max`.

---

## When These Rules Apply

Apply everything in this file whenever the task involves:
- Designing, building, auditing, or refactoring any frontend interface
- Choosing colors, typography, layout, spacing, or motion
- Creating or improving components, pages, dashboards, landing pages, or apps
- Any task that changes how something **looks, feels, moves, or is interacted with**

Skip for: pure backend logic, API design, database work, infrastructure, non-visual scripts.

---

## 0. READ THE BRIEF FIRST (Anti-Default Discipline)

Before writing any code, output one line:

> **"Reading this as: `<page kind>` for `<audience>`, with a `<vibe>` language, leaning toward `<design system or aesthetic family>`."**

Examples:
- *"Reading this as: B2B SaaS landing for technical buyers, with a Linear-style minimalist language, leaning toward Tailwind v4 + Geist + restrained motion."*
- *"Reading this as: solo designer portfolio for hiring managers, with an editorial / kinetic-type language, leaning toward native CSS + scroll-driven animation."*

If the brief is ambiguous, ask **one** clarifying question, not a dump of questions. If context is enough to infer, declare the read and proceed.

**NEVER default to:** AI-purple gradients, centered hero over dark mesh, three equal feature cards, glassmorphism everywhere, infinite micro-animations, Inter + slate-900. These are LLM defaults. Reach past them deliberately.

---

## 1. THREE DIALS

After the design read, set three dials. Every layout, motion, and density decision flows from these.

| Dial | 1 | 10 | Default |
|---|---|---|---|
| `DESIGN_VARIANCE` | Perfect Symmetry | Artsy Chaos | **8** |
| `MOTION_INTENSITY` | Static | Cinematic / Physics | **6** |
| `VISUAL_DENSITY` | Art Gallery / Airy | Cockpit / Packed | **4** |

### Dial inference by brief signal

| Signal | VARIANCE | MOTION | DENSITY |
|---|---|---|---|
| "minimalist / clean / calm / editorial / Linear-style" | 5–6 | 3–4 | 2–3 |
| "premium consumer / Apple-y / luxury / brand" | 7–8 | 5–7 | 3–4 |
| "playful / wild / Dribbble / Awwwards / agency" | 9–10 | 8–10 | 3–4 |
| "trust-first / public-sector / accessibility-critical" | 3–4 | 2–3 | 4–5 |
| Landing page (SaaS) | 7 | 6 | 4 |
| Landing page (Agency / creative) | 9 | 8 | 3 |
| Portfolio (Designer) | 8 | 7 | 3 |
| Portfolio (Developer) | 6 | 5 | 4 |

---

## 2. DEFAULT STACK & ARCHITECTURE

Unless the brief demands a specific design system:

- **Framework:** React or Next.js (default to Server Components). Wrap anything with `useState`, Motion, or scroll listeners in `'use client'` isolated leaf components.
- **Styling:** Tailwind v4. For v4, use `@tailwindcss/postcss` or the Vite plugin — NOT `tailwindcss` in `postcss.config.js`.
- **Animation:** Motion (formerly Framer Motion). Import from `motion/react`. Never use `useState` for continuous values (mouse position, scroll) — use `useMotionValue` / `useTransform` / `useScroll`.
- **Icons:** Priority order: `@phosphor-icons/react` → `hugeicons-react` → `@radix-ui/react-icons` → `@tabler/icons-react`. One family per project. Never hand-roll SVG icon paths. Never use emoji as structural icons.
- **Fonts:** Always `next/font` or self-hosted `@font-face` + `font-display: swap`. Never `<link>` to Google Fonts in production.
- **State:** Local `useState` / `useReducer` for isolated UI. Zustand or Jotai for global. Never `useState` for pointer/scroll physics.

### When to use a real design system

| Brief reads as… | Use |
|---|---|
| Microsoft / enterprise SaaS / dashboards | `@fluentui/react-components` |
| Google-ish UI / Material-flavored | `@material/web` + Material 3 tokens |
| IBM B2B / enterprise analytics | `@carbon/react` |
| Shopify app surfaces | Polaris React |
| Atlassian / Jira-style | `@atlaskit/*` |
| GitHub-style devtool | `@primer/css` or `@primer/react-brand` |
| UK public-sector | `govuk-frontend` |
| US public-sector | `uswds` |
| Modern SaaS, own the components | shadcn/ui (never ship defaults) |
| Indie / small team / AI marketing | Tailwind v4 + `dark:` variant |

**One system per project. Do not mix.**

---

## 3. DESIGN LAWS (Apply to Everything)

### Color
- Use OKLCH. Reduce chroma as lightness approaches 0 or 100.
- Never use `#000` or `#fff` — tint every neutral toward the brand hue (chroma 0.005–0.01 minimum).
- Pick a **color strategy** before picking colors:
  - **Restrained:** tinted neutrals + one accent ≤10%. Default for product UI.
  - **Committed:** one saturated color carries 30–60% of the surface. Default for brand pages.
  - **Full palette:** 3–4 named roles, each deliberate. Campaigns, data viz.
  - **Drenched:** the surface IS the color. Brand heroes.
- The "one accent ≤10%" rule applies to Restrained only. Don't collapse everything to Restrained.

### Theme (Dark vs. Light)
Not a default. Write one sentence of physical scene first: who uses this, where, under what ambient light, in what mood. If the sentence doesn't force the answer, it's not concrete enough.

### Typography
- Cap body line length at 65–75ch.
- Hierarchy through scale + weight contrast (≥1.25 ratio between steps).
- Choose fonts that are beautiful and unexpected. Avoid Inter, Roboto, Arial, Space Grotesk as defaults. Pair a distinctive display font with a refined body font.

### Layout
- Vary spacing for rhythm. Same padding everywhere is monotony.
- Use CSS Grid for layout. Vary cell sizes for hierarchy.
- Cards are the lazy answer — use them only when they're genuinely the best affordance. Nested cards are always wrong.
- Don't wrap everything in a container.
- Use asymmetry, overlap, diagonal flow, and grid-breaking elements where appropriate.

### Motion
- Don't animate CSS layout properties (`width`, `height`, `padding`, `margin`).
- Ease out with exponential curves. **Never `ease-in` for UI animations** — it starts slow and feels sluggish.
- Custom curves (stronger than built-in CSS):
  ```css
  --ease-out: cubic-bezier(0.23, 1, 0.32, 1);
  --ease-in-out: cubic-bezier(0.77, 0, 0.175, 1);
  --ease-drawer: cubic-bezier(0.32, 0.72, 0, 1);
  ```

### Copy
- Every word earns its place. No restated headings, no intros that repeat the title.
- No em dashes. Use commas, colons, semicolons, periods, or parentheses.

---

## 4. ABSOLUTE BANS

Match-and-refuse. If you're about to write any of these, rewrite with different structure.

| Ban | Instead |
|---|---|
| Side-stripe borders (`border-left` / `border-right` > 1px as accent) | Full borders, background tints, leading icons/numbers |
| Gradient text (`background-clip: text` + gradient) | Single solid color; emphasis via weight or size |
| Glassmorphism as default | Rare and purposeful, or nothing |
| The hero-metric template (big number, small label, gradient accent) | Find a more specific data presentation |
| Identical card grids (same-size cards, icon + heading + text repeated) | Varied grid cells, different affordances |
| Modal as first thought | Exhaust inline / progressive disclosure alternatives first |
| Emoji as icons | Vector icon libraries |
| `transition: all` | Specify exact properties: `transition: transform 200ms ease-out` |
| `scale(0)` entry animation | Start from `scale(0.95)` with `opacity: 0` |
| Animation on keyboard-triggered actions | Remove animation entirely |
| AI-purple gradients + centered hero over dark mesh | Read the brief and choose a real direction |
| Inter / Roboto / Arial as the "safe" font choice | Pick something with character |

---

## 5. AI SLOP TEST

Before delivering, ask: *"Could someone look at this and say 'AI made that' without doubt?"* If yes, it's failed.

Run two checks:
1. **First-order:** Can someone guess the palette from the domain alone? ("observability → dark blue", "healthcare → white + teal"). If yes, rework until the answer isn't obvious from the domain.
2. **Second-order:** Can someone guess the aesthetic family from domain + anti-references? If yes, rework again.

---

## 6. ANIMATION DECISION FRAMEWORK

Before writing any animation code, answer in order:

### 6.1 Should this animate at all?

| Frequency | Decision |
|---|---|
| 100+ times/day (keyboard shortcuts, command palette) | **No animation. Ever.** |
| Tens of times/day (hover effects, list navigation) | Remove or drastically reduce |
| Occasional (modals, drawers, toasts) | Standard animation |
| Rare / first-time (onboarding, celebrations) | Can add delight |

### 6.2 What is the purpose?
Every animation must answer "why does this animate?" Valid: spatial consistency, state indication, explanation, press feedback, preventing jarring changes. "It looks cool" is not valid if the user sees it often.

### 6.3 Easing

- Entering/exiting → `ease-out` (starts fast, feels responsive)
- Moving/morphing on screen → `ease-in-out`
- Hover / color change → `ease`
- Constant motion → `linear`
- Default → `ease-out`

### 6.4 Duration

| Element | Duration |
|---|---|
| Button press feedback | 100–160ms |
| Tooltips, small popovers | 125–200ms |
| Dropdowns, selects | 150–250ms |
| Modals, drawers | 200–500ms |
| Marketing / explanatory | Longer OK |

UI animations should stay under 300ms. A faster-spinning spinner makes the app feel faster even if load time is identical.

### 6.5 Other animation rules
- **Stagger:** when multiple elements enter together, stagger 30–80ms between items. Never block interaction during stagger.
- **Asymmetric timing:** slow where user is deciding (e.g., hold-to-delete: 2s linear), fast where system responds (release: 200ms ease-out).
- **Springs:** use for drag interactions, elements that should feel alive, interruptible gestures. Use `useSpring` from Motion.
- **Hardware acceleration:** use `transform: "translateX()"` not `x` prop in Motion when main thread is under load.
- **CSS over JS under load:** CSS animations run off the main thread. Use CSS for predetermined animations; JS for dynamic/interruptible ones.
- **`prefers-reduced-motion`:** Keep opacity/color transitions that aid comprehension. Remove movement and position animations.
  ```css
  @media (prefers-reduced-motion: reduce) {
    .element { animation: fade 0.2s ease; }
  }
  ```
- **Touch hover:** gate hover animations:
  ```css
  @media (hover: hover) and (pointer: fine) {
    .element:hover { transform: scale(1.05); }
  }
  ```

---

## 7. ACCESSIBILITY (CRITICAL — Priority 1)

- Color contrast ≥ 4.5:1 for normal text; ≥ 3:1 for large text
- Visible focus rings on all interactive elements (2–4px)
- Descriptive alt text for meaningful images
- `aria-label` for icon-only buttons
- Tab order matches visual order
- `<label>` with `for` attribute on all form fields
- Skip-to-main-content link for keyboard users
- Sequential `h1` → `h6`, no skipped levels
- Never convey info by color alone — add icon or text
- Support system text scaling; avoid truncation as text grows
- `voiceover` / screen reader: meaningful labels, logical reading order

---

## 8. TOUCH & INTERACTION (CRITICAL — Priority 2)

- Min touch target: 44×44pt (iOS) / 48×48dp (Android)
- Min 8px gap between touch targets
- Use `click`/`tap` for primary interactions; never rely on hover alone
- Disable button during async ops; show spinner or progress
- Clear error messages near the problem field
- Add `cursor: pointer` to clickable elements
- Use `touch-action: manipulation` to reduce 300ms tap delay
- Visual feedback on press (ripple/highlight) within 80–150ms

---

## 9. UI/UX RULE PRIORITIES

| Priority | Category | Key Checks |
|---|---|---|
| 1 | Accessibility | Contrast 4.5:1, Alt text, Keyboard nav, Aria-labels |
| 2 | Touch & Interaction | Min 44×44px, 8px+ spacing, Loading feedback |
| 3 | Performance | WebP/AVIF, Lazy loading, CLS < 0.1 |
| 4 | Style | Match product type, SVG icons, Consistency |
| 5 | Layout & Responsive | Mobile-first, Viewport meta, No horizontal scroll |
| 6 | Typography & Color | Base 16px, Line-height 1.5, Semantic color tokens |
| 7 | Animation | 150–300ms, Motion conveys meaning, `prefers-reduced-motion` |
| 8 | Forms & Feedback | Visible labels, Error near field, Progressive disclosure |
| 9 | Navigation | Predictable back, Bottom nav ≤5 items, Deep linking |
| 10 | Charts & Data | Legends, Tooltips, Accessible colors |

---

## 10. PRE-DELIVERY CHECKLIST

### Visual Quality
- [ ] No emoji used as icons
- [ ] Icons from a single consistent family
- [ ] Pressed states don't shift layout bounds
- [ ] Semantic color tokens used consistently (no ad-hoc hex values)
- [ ] AI slop test passed (both orders)
- [ ] Font choice is distinctive and intentional

### Interaction
- [ ] All tappable elements provide pressed feedback
- [ ] Touch targets ≥ 44×44pt (iOS) / 48×48dp (Android)
- [ ] Micro-interaction timing 150–300ms with appropriate easing
- [ ] Disabled states are visually clear and non-interactive
- [ ] `transition: all` replaced with specific properties
- [ ] No animation on keyboard-triggered actions

### Light/Dark Mode
- [ ] Primary text contrast ≥ 4.5:1 in both modes
- [ ] Secondary text contrast ≥ 3:1 in both modes
- [ ] Borders/dividers visible in both modes
- [ ] Both themes tested before delivery

### Layout
- [ ] Body text capped at 65–75ch
- [ ] Spacing varies for rhythm (not uniform everywhere)
- [ ] Mobile-first responsive behavior verified
- [ ] No horizontal scroll
- [ ] Safe areas respected (notch, gesture bar, status bar)

### Accessibility
- [ ] All meaningful images/icons have labels
- [ ] Form fields have labels, hints, and error messages
- [ ] Color is not the only indicator of meaning
- [ ] `prefers-reduced-motion` respected
- [ ] Focus order matches visual order

---

## 11. COMMANDS (when using the `impeccable` design system)

If the project has `.pi/skills/impeccable/` set up, these slash commands are available:

| Command | Description |
|---|---|
| `/impeccable craft [feature]` | Shape then build a feature end-to-end |
| `/impeccable shape [feature]` | Plan UX/UI before writing code |
| `/impeccable teach` | Set up PRODUCT.md and DESIGN.md context |
| `/impeccable document` | Generate DESIGN.md from existing code |
| `/impeccable critique [target]` | UX design review with heuristic scoring |
| `/impeccable audit [target]` | Technical checks (a11y, perf, responsive) |
| `/impeccable polish [target]` | Final quality pass before shipping |
| `/impeccable bolder [target]` | Amplify safe or bland designs |
| `/impeccable quieter [target]` | Tone down aggressive designs |
| `/impeccable animate [target]` | Add purposeful motion |
| `/impeccable colorize [target]` | Add strategic color to monochromatic UIs |
| `/impeccable live` | Visual variant mode: pick elements, generate alternatives |

Load context before any design work: `node .pi/skills/impeccable/scripts/load-context.mjs`

---

*Consolidated from: `web-design` v3.1.1, `web-taste`, `frontend-design`, `web-animation` (Emil Kowalski), `ui-ux-pro-max`*
