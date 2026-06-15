---
name: saas-redesign
description: This skill should be used when the user asks to "redesign the UI", "add a sidebar", "convert top nav to sidebar", "modernize the interface", "SaaS layout", or wants to transform the navigation from a top bar to a vertical left sidebar with a polished professional look.
---

# SaaS Redesign

Transform this Vue 3 app's horizontal top navigation bar into a modern SaaS-style interface with a fixed vertical left sidebar, consistent spacing, and professional polish.

## Phase 0 — Path Confirmation

Verify the project root is `/Users/jln/inventory-management/` by confirming `client/src/App.vue` exists at that path. If the file is missing, locate the correct root before proceeding.

## Phase 1 — Discovery

Read the following files in full before writing any code:

- `client/src/App.vue` — current nav markup, CSS class names, `.app-header` / `.filter-bar` sticky structure, global CSS variables and design tokens
- `client/src/main.js` — router configuration; extract all 6 route paths and their human-readable labels
- `client/src/components/ProfileMenu.vue` — understand the profile menu structure (it moves to sidebar footer)
- `client/src/components/LanguageSwitcher.vue` — understand language toggle structure (it also moves to sidebar footer)

Note from discovery: the current `.app-header` is `position: sticky; top: 0; height: 70px`. The `.filter-bar` is `position: sticky; top: 70px`. Both values need to change in the new layout.

## Phase 2 — Target Layout

The new layout follows this structure:

```
┌────────────────┬──────────────────────────────────────┐
│  SIDEBAR 240px │  MAIN CONTENT                        │
│  pos: fixed    │  margin-left: var(--sidebar-width)   │
│                │                                      │
│  [Logo/Brand]  │  [Top bar 56px — sticky, top: 0]    │
│                │  [FilterBar — sticky, top: 56px]     │
│  Dashboard     │                                      │
│  Inventory     │  <router-view />                     │
│  Orders        │                                      │
│  Finance       │                                      │
│  Demand        │                                      │
│  Reports       │                                      │
│                │                                      │
│  [Profile]     │                                      │
│  [Language]    │                                      │
└────────────────┴──────────────────────────────────────┘
```

### Design Tokens

Apply these values as CSS custom properties on `:root` in App.vue:

```css
--sidebar-width: 240px;
--topbar-height: 56px;
--sidebar-bg: #0f172a;
--sidebar-text: #94a3b8;
--sidebar-text-active: #ffffff;
--sidebar-active-bg: #1e293b;
--sidebar-active-border: #2563eb;
--sidebar-hover-bg: #1e293b;
```

### Sidebar Styles

```css
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  width: var(--sidebar-width);
  height: 100vh;
  background: var(--sidebar-bg);
  display: flex;
  flex-direction: column;
  z-index: 100;
  overflow-y: auto;
}

.sidebar-logo {
  padding: 1.25rem 1rem;
  border-bottom: 1px solid #1e293b;
  flex-shrink: 0;
}

.sidebar-nav {
  flex: 1;
  padding: 0.75rem 0;
}

.sidebar-link {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.625rem 1rem;
  color: var(--sidebar-text);
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  border-left: 3px solid transparent;
  transition: background 0.15s, color 0.15s;
}

.sidebar-link:hover {
  background: var(--sidebar-hover-bg);
  color: var(--sidebar-text-active);
}

.sidebar-link.router-link-active {
  background: var(--sidebar-active-bg);
  color: var(--sidebar-text-active);
  border-left-color: var(--sidebar-active-border);
}

.sidebar-footer {
  padding: 0.75rem 1rem;
  border-top: 1px solid #1e293b;
  flex-shrink: 0;
}
```

### Top Bar Styles

The top bar lives inside `.main-content` and is `position: sticky; top: 0`:

```css
.topbar {
  position: sticky;
  top: 0;
  height: var(--topbar-height);
  background: #ffffff;
  border-bottom: 1px solid #e2e8f0;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding: 0 1.5rem;
  gap: 1rem;
  z-index: 90;
  flex-shrink: 0;
}
```

### Main Content Styles

```css
.main-content {
  margin-left: var(--sidebar-width);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: #f8fafc;
}

.page-content {
  flex: 1;
  padding: 1.5rem;
}
```

### FilterBar Update

In `client/src/components/FilterBar.vue`, change the sticky offset:
- Find `top: 70px` (the old header height offset)
- Replace with `top: var(--topbar-height)` — the FilterBar now sticks below the new top bar

### Responsive Behavior

Three breakpoints, implemented via `@media` queries in App.vue:

**Desktop (≥ 1024px):** Full sidebar at 240px, labels visible. Default state above.

**Tablet (768px – 1023px):** Icon-only rail. Override `--sidebar-width: 64px`. Hide nav label text with `v-show` (or `.sidebar-label` set to `display: none` via media query). Keep icons. Add `title` attribute to each `.sidebar-link` for tooltip on hover.

**Mobile (< 768px):** Sidebar hidden by default; hamburger button in top bar opens it as an overlay.

```css
/* Tablet */
@media (max-width: 1023px) {
  :root { --sidebar-width: 64px; }
  .sidebar-label { display: none; }
  .sidebar-link { justify-content: center; padding: 0.75rem; }
  .sidebar-logo-text { display: none; }
}

/* Mobile */
@media (max-width: 767px) {
  .sidebar {
    transform: translateX(-100%);
    transition: transform 0.25s ease;
  }
  .sidebar.is-open {
    transform: translateX(0);
  }
  .main-content {
    margin-left: 0;
  }
  .sidebar-overlay {
    display: block;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.4);
    z-index: 99;
  }
}
```

In App.vue `<script setup>`, add:

```js
import { ref } from 'vue'
const isSidebarOpen = ref(false)
```

Bind `isSidebarOpen` to the hamburger button click and to the `.is-open` class on `.sidebar`. Add a `.sidebar-overlay` div (visible only at mobile) that closes the sidebar on click.

## Phase 3 — Implementation

**MANDATORY: delegate all `.vue` file edits to the `vue-expert` subagent.** Do not edit `.vue` files directly — pass the full plan including the design tokens, layout structure, and code snippets above to vue-expert with explicit instructions on which files to change and what to change.

Pass vue-expert the following task summary:
1. Rewrite the layout shell in `client/src/App.vue`: add `<aside class="sidebar">` with logo, `<router-link>` nav items for all 6 routes (Dashboard `/`, Inventory `/inventory`, Orders `/orders`, Finance `/spending`, Demand `/demand`, Reports `/reports`), and sidebar footer with `ProfileMenu` and `LanguageSwitcher`. Add `<main class="main-content">` wrapping a `.topbar` div and `<FilterBar />` and `<div class="page-content"><router-view /></div>`. Remove the existing `.app-header`. Add all CSS from Phase 2 into the App.vue global `<style>` block (not scoped). Add `:root` CSS variables. Add mobile `isSidebarOpen` reactive ref and hamburger button in `.topbar`.
2. Update `client/src/components/FilterBar.vue`: change the sticky `top` value from `70px` to `var(--topbar-height)`.

Supply vue-expert with the exact design token values and CSS snippets from Phase 2 so it doesn't invent different values.

## Phase 4 — Visual Verification

Use Playwright MCP tools (`mcp__playwright__*`) to verify the result against `http://localhost:3000`.

Start the dev server first if not running:
```bash
cd /Users/jln/inventory-management/client && npm run dev
```

Then run these checks in order:

1. **Desktop layout** — Navigate to `http://localhost:3000`. Take a screenshot. Confirm: sidebar visible on left at ~240px, top bar at top of main content, FilterBar sticky below top bar, Dashboard content in remaining space.

2. **Nav routing** — Click each of the 6 nav links. Confirm: URL changes, active link has left blue border highlight, page content updates.

3. **Sticky behavior** — Scroll down on a long page (e.g., Dashboard). Confirm: sidebar stays fixed, top bar stays pinned, FilterBar stays pinned below top bar, page content scrolls underneath.

4. **Tablet breakpoint** — Resize viewport to 768px width (`mcp__playwright__browser_resize`). Take screenshot. Confirm: sidebar collapses to 64px icon-only rail, labels hidden, main content fills remaining width, no horizontal scroll.

5. **Mobile breakpoint** — Resize to 375px width. Take screenshot. Confirm: sidebar hidden (not visible), hamburger toggle button visible in top bar. Click hamburger. Confirm sidebar slides in as overlay.

6. **No regressions** — Navigate to Dashboard, Inventory, and Orders. Screenshot each. Confirm: existing cards, tables, and charts render correctly within the new layout.

Report any visual issues found and fix before marking the task complete.
