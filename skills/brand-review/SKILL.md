---
name: brand-review
description: Audits a frontend project for compliance with @adarsh_goswami/brand. Fixes any hardcoded style values that have brand token equivalents, and lists values with no token coverage as vulnerabilities requiring a human decision. Use when reviewing or cleaning up styling in any project that uses @adarsh_goswami/brand.
disable-model-invocation: true
---

You are reviewing a frontend project for compliance with the `@adarsh_goswami/brand` design system. Your job is to:
1. Audit all source files for hardcoded style values that should use brand tokens
2. Fix every violation that has a brand token equivalent
3. List anything that has NO brand equivalent as a **vulnerability** requiring a human decision

---

## Step 1 — Locate the brand package

Find `node_modules/@adarsh_goswami/brand/dist/theme.css` and `tailwind.config.css` in the current project. Read both files to build your authoritative token reference for this audit.

If the package is not installed, stop and report: "❌ `@adarsh_goswami/brand` is not installed. Run `bun add @adarsh_goswami/brand` first."

---

## Step 2 — Build the token lookup table

From `theme.css`, extract every CSS custom property defined in `:root`. These are all available as `var(--token-name)` in inline styles.

From `tailwind.config.css`, extract every `--color-*`, `--font-*`, `--text-*`, `--radius-*`, `--shadow-*`, `--leading-*`, `--tracking-*`, `--duration-*`, `--ease-*`, `--z-*` entry. These map to Tailwind utility classes:
- `--color-accent` → `text-accent`, `bg-accent`, `border-accent`, `stroke-accent`, `fill-accent`
- `--color-success` → `text-success`, `bg-success`, `border-success`, `stroke-success`, `fill-success`
- `--font-display` → `font-display`
- `--text-xs` → `text-xs` (brand scale, overrides Tailwind default)
- `--radius-lg` → `rounded-lg`
- `--shadow-md` → `shadow-md`
- etc.

CSS vars that exist in `theme.css` but are NOT in `tailwind.config.css` as `--color-*` must be used as Tailwind **arbitrary values**: `bg-[var(--accent-subtle)]`, `border-[var(--accent-glow)]`.

---

## Step 3 — Scan all source files

Find all `.tsx`, `.jsx`, `.ts`, `.js`, `.css`, `.scss` files under `src/` (exclude `node_modules`, `dist`, `build`).

For each file, scan for the following violation patterns:

### A — Hardcoded hex colors
Pattern: `#[0-9A-Fa-f]{3,8}` in:
- JSX `style={{ color: '#...' }}`, `style={{ background: '#...' }}`, etc.
- JSX `className="... text-[#...] ..."` arbitrary Tailwind values
- CSS/SCSS property values

### B — Hardcoded rgba/rgb/hsl
Pattern: `rgba?\(` or `hsl\(` in style attributes or CSS files.

### C — Hardcoded font families
Pattern: `fontFamily:` or `font-family:` values that are not `var(--brand-font-*)`.

### D — Hardcoded font sizes (in style attributes, not Tailwind classes)
Pattern: `fontSize:` values with px or rem literals that are not `var(--brand-text-*)`.

### E — Hardcoded border radius (in style attributes)
Pattern: `borderRadius:` values with px literals that are not `var(--brand-radius-*)`.

### F — Hardcoded shadows (in style attributes)
Pattern: `boxShadow:` values that are not `var(--brand-shadow-*)`.

### G — Hardcoded transition durations/easings (in style attributes)
Pattern: `transitionDuration:`, `animationDuration:`, `transition:` values that are not `var(--brand-duration-*)` / `var(--brand-ease-*)`.

---

## Step 4 — Classify each violation

For every violation found, classify it as one of:

**FIXABLE** — A brand token exists with the same or equivalent value. Fix it immediately.

Lookup table of known brand token values (from `theme.css`):
```
Colors:
  #0A0A0B        → var(--bg-base)         / bg-bg-base / text-bg-base
  #111113        → var(--bg-surface)      / bg-bg-surface / fill-bg-surface
  #18181C        → var(--bg-raised)       / bg-bg-raised / fill-bg-raised
  #1F1F25        → var(--bg-overlay)      / bg-bg-overlay
  #1A1A1F        → var(--bg-hover)        / bg-bg-hover
  #212127        → var(--bg-active)       / bg-bg-active
  #1E1E24        → var(--border-subtle)   / border-border-subtle
  #2A2A34        → var(--border-soft)     / border-border-soft
  #3A3A48        → var(--border-mid)      / border-border-mid
  #454558        → var(--border-hover)    / border-border-hover
  #7C6EFA        → var(--accent) / var(--border-focus) / text-accent / bg-accent / stroke-accent / fill-accent
  #F0EEF8        → var(--text-primary)    / text-text-primary / stroke-text-primary / fill-text-primary
  #9997AA        → var(--text-secondary)  / text-text-secondary
  #5C5A6E        → var(--text-muted)      / text-text-muted
  #3A3848        → var(--text-disabled)   / text-text-disabled
  #9B8FFB        → var(--accent-bright)   / text-accent-bright / stroke-accent-bright
  #8D80FB        → var(--accent-hover)    / bg-accent-hover
  #6B5CE8        → var(--accent-active)   / bg-accent-active
  #4A3FCC        → var(--accent-dim)      / text-accent-dim / stroke-accent-dim
  rgba(124,110,250,0.15) → var(--accent-glow)   / bg-[var(--accent-glow)] / border-[var(--accent-glow)]
  rgba(124,110,250,0.08) → var(--accent-subtle) / bg-[var(--accent-subtle)]
  rgba(124,110,250,0.2)  → var(--accent-border) / border-[var(--accent-border)]
  #3DD68C        → var(--success)  / text-success / bg-success / stroke-success
  #F5A623        → var(--warning)  / text-warning / bg-warning / stroke-warning
  #F2546A        → var(--error)    / text-error / bg-error / stroke-error
  #4AA8FF        → var(--info)     / text-info / bg-info / stroke-info
  rgba(61,214,140,0.08)  → var(--success-bg)     / bg-[var(--success-bg)]
  rgba(245,166,35,0.08)  → var(--warning-bg)     / bg-[var(--warning-bg)]
  rgba(242,84,106,0.08)  → var(--error-bg)       / bg-[var(--error-bg)]
  rgba(74,168,255,0.08)  → var(--info-bg)        / bg-[var(--info-bg)]
  rgba(61,214,140,0.2)   → var(--success-border) / border-[var(--success-border)]
  rgba(245,166,35,0.2)   → var(--warning-border) / border-[var(--warning-border)]
  rgba(242,84,106,0.2)   → var(--error-border)   / border-[var(--error-border)]
  rgba(74,168,255,0.2)   → var(--info-border)    / border-[var(--info-border)]

Font families:
  'Syne', sans-serif   → var(--brand-font-display) / font-display
  'DM Sans', sans-serif → var(--brand-font-body)   / font-body
  'DM Mono', monospace  → var(--brand-font-mono)   / font-mono

Font sizes (in style attributes):
  0.6875rem / 11px → var(--brand-text-xs)
  0.8125rem / 13px → var(--brand-text-sm)
  0.9375rem / 15px → var(--brand-text-base)
  1.0625rem / 17px → var(--brand-text-md)
  1.25rem   / 20px → var(--brand-text-lg)
  1.5rem    / 24px → var(--brand-text-xl)
  2rem      / 32px → var(--brand-text-2xl)
  2.75rem   / 44px → var(--brand-text-3xl)
  3.75rem   / 60px → var(--brand-text-4xl)
  5rem      / 80px → var(--brand-text-5xl)

Border radius (in style attributes):
  4px    → var(--brand-radius-sm)
  8px    → var(--brand-radius-md)
  12px   → var(--brand-radius-lg)
  16px   → var(--brand-radius-xl)
  24px   → var(--brand-radius-2xl)
  9999px → var(--brand-radius-full)

Shadows:
  0 1px 2px rgba(0,0,0,0.4)      → var(--brand-shadow-sm)     / shadow-sm
  0 4px 16px rgba(0,0,0,0.5)     → var(--brand-shadow-md)     / shadow-md
  0 12px 40px rgba(0,0,0,0.6)    → var(--brand-shadow-lg)     / shadow-lg
  0 0 24px rgba(124,110,250,0.25) → var(--brand-shadow-accent) / shadow-accent

Transitions:
  120ms → var(--brand-duration-fast) / duration-fast
  220ms → var(--brand-duration-base) / duration-base
  400ms → var(--brand-duration-slow) / duration-slow
  cubic-bezier(0.16, 1, 0.3, 1) → var(--brand-ease-out)
  cubic-bezier(0.4, 0, 0.2, 1)  → var(--brand-ease-in-out)
```

**Fix strategy** — prefer Tailwind classes over inline `style` where possible:
- If the value is on an HTML/JSX element, use a Tailwind utility class
- If the value is on an SVG element (`<path>`, `<rect>`, `<line>`, etc.), use `stroke-*` / `fill-*` Tailwind classes
- If there's no matching Tailwind utility (token not in `--color-*` mapping), use `bg-[var(--token)]` arbitrary syntax in className
- Only use `style={{ ... }}` when the value is genuinely dynamic, is part of a visual demo (color swatch showing itself), or is a complex expression like `radial-gradient()` or `clamp()` that has no Tailwind equivalent

**SKIP (do not flag)** — these are intentional and correct:
- Color swatch `background` values in design system documentation components (where the component's purpose IS to display that exact color)
- Type specimen `fontSize` style values (where the element IS demonstrating that size)
- Spacing bar `width` values that ARE the spacing scale demo
- Radius demo `borderRadius`, `width`, `height` on elements that ARE the radius demo boxes
- SVG `radialGradient` / `linearGradient` `stopColor` values that don't match any brand token
- `clamp()` expressions in `fontSize` style
- `backdrop-filter` CSS (use Tailwind `backdrop-blur-*` instead — flag only if not already done)

**VULNERABILITY** — value is hardcoded AND no brand token covers it. Do not change. Add to the report instead.

---

## Step 5 — Apply fixes

Read each file that has FIXABLE violations, apply all fixes, save. Batch all fixes for a file in a single edit pass.

---

## Step 6 — Output the report

After all fixes are applied, print:

```
## Brand Compliance Report

### ✅ Fixed (N violations)
- `src/path/file.tsx` line 42: `#7C6EFA` → `stroke-accent`
- `src/path/file.tsx` line 88: `rgba(124,110,250,0.08)` → `bg-[var(--accent-subtle)]`

### ⚠️ Vulnerabilities (N items — no brand token exists)
- `src/path/file.tsx` line 61: `background: '#0D0C1A'`
  → No token. Reason: glow logo card specific background, dark-first system has no equivalent.
  → Options: (a) add --bg-glow token to @adarsh_goswami/brand, (b) keep as intentional one-off, (c) remove

### 📋 Summary
- Files scanned: N
- Files modified: N
- Violations fixed: N
- Vulnerabilities requiring decision: N
```

Be precise. Do not guess — if you are not sure a value matches a token, put it in vulnerabilities, not fixes.
