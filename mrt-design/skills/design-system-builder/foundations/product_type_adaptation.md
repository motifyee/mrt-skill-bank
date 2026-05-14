# Product Type Adaptation

How the design system adapts its defaults, density, component behavior, and visual
priorities for different product types. Use this when the interview reveals a primary
surface type or when generating UI kits for specific surfaces.

Each product type has different constraints on information density, interaction
patterns, motion expectations, and visual hierarchy. The base design system tokens
remain constant; this document specifies how they are applied and prioritized per
product type.

---

## Marketing

Landing pages, public websites, campaign pages, and conversion-focused surfaces.

### Shell
- **Container width:** 1200-1440px max, generous side margins
- **Navigation:** Top horizontal nav with logo, 4-7 links, and a primary CTA button. Sticky on scroll is common but not required.
- **Footer:** Multi-column footer with links, social icons, and legal text.
- **Grid:** 12-column grid. Marketing pages often use asymmetric layouts (7fr/5fr, 8fr/4fr) for hero sections, then symmetric grids for feature sections.

### Density
- **Tier:** Spacious to comfortable (multiplier 0.9-1.1)
- **Section padding:** 64-128px vertical, creating clear content rhythm
- **Card density:** 2-3 cards per row with generous gap (24-32px)
- **Typography scale:** Use the full scale from display through body. Hero text gets the display or h1 size. Feature descriptions use body or body_lg.

### Components
- **Hero:** Full-width or asymmetric split. Contains headline (h1 or display), subtext (body_lg), and a primary CTA. Often includes a hero image, illustration, or atmospheric background treatment.
- **Feature grid:** 2-4 column grid of cards. Each card has an icon, heading (h3 or h4), and short description (body_sm). Signature DNA appears here in hover states or borders.
- **Testimonials:** Quote block with attribution. May use a carousel or masonry layout.
- **Pricing table:** 2-4 plan cards side-by-side. Featured plan gets visual emphasis (accent border, scale, or elevation).
- **CTA sections:** Full-width banner with headline, description, and button. Often uses accent or inverse background.

### Color Priority
- Primary emphasis on accent color for CTAs and interactive elements
- Neutral backgrounds with one or two accent-tinted sections for rhythm
- Dark mode often uses a single dark hero section against light body sections (or full dark if dark-first)
- Signature color moment typically lives in the hero section

### Motion
- **Entry animations:** Sections fade/slide in on scroll (IntersectionObserver)
- **Hover effects:** Cards lift or reveal accent borders. Buttons have scale or color transitions.
- **Micro-interactions:** CTA buttons pulse subtly or have animated borders
- **Restriction:** Avoid continuous animation. Marketing pages should feel static until the user interacts or scrolls. Motion should support content discovery, not distract from it.

### Content
- **Headlines:** Short, punchy, benefit-oriented. 4-8 words maximum.
- **Body:** Concise paragraphs. 2-3 sentences per section block.
- **Buttons:** Action verbs. 1-3 words. "Get started", "See pricing", "Start free trial".
- **Voice alignment:** Marketing copy is the most brand-voice-forward surface. Ensure the voice profile from the context packet is applied consistently.

### Navigation
- Horizontal top nav as default
- Mobile: hamburger menu with slide-in or overlay panel
- Active state on current page link
- CTA button in nav is common (top-right position in LTR)

### Imagery
- Photography style follows imagery.photo_style from the packet
- Hero image/illustration is the largest visual element on the page
- Feature sections may use icons (from the specified icon library) or small illustrations
- Avoid generic stock photos. Use product screenshots, abstract illustrations, or brand-specific imagery per the imagery guidelines.

---

## Dashboard

Internal tools, admin panels, analytics interfaces, and data-heavy applications.

### Shell
- **Container width:** Full-width. No max-width constraint. Content flows to viewport edges with a fixed sidebar.
- **Navigation:** Sidebar navigation (240-280px width) with icon + label items. Collapsible to icon-only (56-64px) on smaller screens or by user toggle.
- **Header bar:** Top bar (56-64px height) with page title, breadcrumbs, search, notifications, and user avatar.
- **Grid:** CSS Grid with sidebar + main content area. Internal content uses flexible grids (auto-fill or defined columns based on widget count).

### Density
- **Tier:** Compact (multiplier 0.8-0.9)
- **Section padding:** 16-32px between widget groups
- **Card density:** Dense. Tables and stat cards use minimal padding (8-16px internal).
- **Typography scale:** Emphasizes h2-h4 for section headings, body for data, and label/mono for metadata. Display sizes are rarely used.

### Components
- **Sidebar nav:** Icon + text links, collapsible sections, active state with accent indicator (left border or bg tint). Badge counts for notification items.
- **Data tables:** Sortable columns, row hover states, pagination or infinite scroll. Header row uses label typography. Data cells use body or mono.
- **Stat cards:** Number (large, often h2 or h3 weight), label (label typography), and optional sparkline or trend indicator. Arranged in a responsive grid.
- **Charts:** Bar, line, area, or pie charts. Chart colors derived from the accent palette with distinct hues for multi-series. Chart tooltips use the card component style.
- **Filters:** Search input, dropdown selectors, date range picker. Typically grouped in a filter bar above the data table or chart area.
- **Modals/drawers:** For detail views, forms, and confirmations. Use the card elevation and border system.

### Color Priority
- Neutral-dominant. The accent color is used sparingly for active states, CTAs, and data highlights.
- Data visualization uses a derived palette from the accent hue, not random colors.
- Status colors (success, warning, error, info) are heavily used for state indicators, badges, and alerts.
- Background layers use bg and bg_alt to create depth without heavy shadows.

### Motion
- **Transitions:** Fast and functional. Page transitions use duration_fast (200ms). Data updates use duration_micro (100ms).
- **Loading states:** Skeleton screens preferred over spinners. Use bg_alt colored placeholders matching the expected content shape.
- **Chart animations:** Brief entrance animation (300-500ms), then static. Data updates transition smoothly (morph, not jump).
- **Restriction:** Dashboard motion should be invisible unless the user is looking for it. No decorative animation.

### Content
- **Headings:** Descriptive, functional. "Pipeline Status", "Recent Deployments", "Team Activity".
- **Body:** Data-dense. Prefer tables and stat summaries over prose.
- **Labels:** Extensive use of label typography for column headers, stat labels, metadata.
- **Empty states:** Friendly but concise. "No deployments yet. Run your first pipeline to get started."
- **Error states:** Clear, actionable. "Build failed at step 3. View logs for details."

### Navigation
- Sidebar navigation as default
- Mobile: bottom tab bar (max 5 items) or hamburger with slide-in sidebar
- Breadcrumbs for page hierarchy within the main content area
- Quick actions (keyboard shortcuts, command palette) for power users

### Imagery
- Minimal to none. Dashboards use data visualization and icons, not photography.
- User avatars for identity features.
- Status icons and illustrative icons from the icon library.
- If imagery is used (empty states, onboarding), use the illustration_style from the packet (usually diagrammatic or geometric).

---

## Documentation

Knowledge bases, API references, guides, and technical documentation.

### Shell
- **Container width:** 900-1100px for reading content. Sidebar navigation is separate.
- **Navigation:** Three-tier: (1) top bar with logo and search, (2) left sidebar with section navigation (240-280px), (3) right sidebar with page-level TOC (optional, desktop only).
- **Footer:** Minimal. Previous/Next page links, edit-on-GitHub link.
- **Grid:** Sidebar + main content. Reading column is narrow enough for comfortable line length (60-80 characters).

### Density
- **Tier:** Comfortable to spacious (multiplier 0.9-1.1)
- **Section padding:** 32-48px between content sections
- **Typography priority:** Body text readability is paramount. Line-height >= 1.6 for body. Generous paragraph spacing.
- **Code blocks:** Monospace with distinct background (bg_alt or darker). Generous padding (16-24px). Language label in top-right corner.

### Components
- **Callouts:** Info, warning, tip, and danger boxes with left accent border and icon. Use semantic colors from the palette.
- **Code blocks:** Syntax-highlighted code with copy button. Inline code uses mono typography with subtle background tint.
- **Tables:** Used for API parameters, configuration options, and comparison data. Horizontal scroll on mobile. Header row with label typography.
- **Tabs:** For switching between code languages, framework examples, or configuration approaches.
- **Breadcrumbs:** Top of page, showing section hierarchy.
- **Search:** Global search in the top bar. Results dropdown with section titles and highlighted matches.

### Color Priority
- Neutral-dominant with minimal accent usage
- Accent color used for links, active nav items, and interactive elements
- Code block backgrounds use bg_alt (distinct from bg but not jarring)
- Callout colors: info (blue tint), warning (amber tint), tip (green tint), danger (red tint)
- Dark mode is common for developer documentation -- ensure full dark mode token support

### Motion
- **Minimal to none.** Documentation should be static. No scroll animations or decorative transitions.
- **Interactions:** Expand/collapse for code blocks, sidebar sections, and detail panels. Use duration_fast (200ms).
- **Search:** Instant results with no perceptible delay. Debounced input with dropdown results.
- **Restriction:** Documentation motion should never distract from reading. If in doubt, omit the animation.

### Content
- **Headings:** Descriptive, scannable. "Installation", "Configuration", "API Reference".
- **Body:** Clear, instructional prose. Short paragraphs. Steps use numbered lists.
- **Code:** Inline code for function names, parameters, and CLI commands. Code blocks for multi-line examples.
- **Voice:** Follows the voice profile from the context packet, but leans toward clarity over personality. Technical documentation prioritizes being understood over being clever.

### Navigation
- Left sidebar with section headings and page links
- Active page highlighted in sidebar
- Right sidebar with on-page TOC (h2 and h3 headings)
- Mobile: sidebar becomes a slide-in drawer, triggered by hamburger menu
- Previous/Next navigation at the bottom of each page

### Imagery
- Diagrams and screenshots only. No decorative photography.
- Architecture diagrams use the illustration style from the packet.
- Screenshots are annotated with callouts when showing UI features.
- Icons from the icon library for UI element references.

---

## Settings/Admin

Configuration panels, user settings, admin interfaces, and internal management tools.

### Shell
- **Container width:** 800-1000px for forms and settings. Full-width for tables (user management, logs).
- **Navigation:** Embedded within a dashboard sidebar (admin section) or standalone vertical nav for settings-only interfaces.
- **Header:** Page title with optional description text. Breadcrumbs if nested.
- **Grid:** Single column for form-heavy pages. Multi-column for dashboards with settings widgets.

### Density
- **Tier:** Comfortable (multiplier 1.0)
- **Section padding:** 24-40px between setting groups
- **Form density:** Generous spacing between form fields (16-24px). Group related fields with section headings.
- **Typography:** Body for labels and descriptions. Label typography for field names and small helper text. h2 or h3 for section headings.

### Components
- **Form sections:** Grouped by category with section headings. Each group has a heading, optional description, and 3-8 form fields.
- **Form fields:** Label, input/select/textarea, and helper text. Error states appear below the field with error color. Required fields marked with visual indicator.
- **Toggle/switch:** For boolean settings. Clear on/off states with label text.
- **Danger zones:** Destructive actions (delete account, reset data) in a distinctly bordered section (often with error-tinted border or background).
- **Save/cancel:** Sticky bottom bar or inline buttons per section. Primary action (Save) is accent-colored. Cancel is ghost/outline.
- **Tables:** For list management (users, roles, API keys). Inline actions (edit, delete) in the last column. Bulk actions toolbar when items are selected.

### Color Priority
- Neutral-dominant, similar to dashboard
- Accent for primary actions (Save, Create, Add)
- Semantic colors for status: success for saved state, warning for unsaved changes, error for validation errors
- Danger zone uses error color for borders or background tint
- Active/hover states use subtle bg_alt tint, not accent flood

### Motion
- **Minimal.** Settings pages should feel instant and reliable.
- **Save confirmation:** Brief success toast or inline "Saved" indicator (duration_base, 300ms fade).
- **Validation:** Inline error messages appear with duration_fast (200ms). Red border on invalid fields.
- **Restriction:** No decorative animation. Settings are a trust surface -- stability signals reliability.

### Content
- **Labels:** Clear, specific. "Display name", "Email notifications", "Default timezone".
- **Helper text:** Brief, actionable. "Used for project invitations. Visible to team members."
- **Section headings:** Category-based. "Profile", "Notifications", "Security", "Billing".
- **Button labels:** Specific actions. "Save changes", "Delete account", "Generate API key".
- **Confirmation dialogs:** Clear consequence language. "This will permanently delete your account and all associated data."

### Navigation
- Vertical section navigation (left sidebar or in-page anchor links)
- Horizontal tabs for top-level categories if sidebar is not available
- Breadcrumbs for nested settings
- Mobile: stacked sections with accordion or scroll-spy navigation

### Imagery
- None. Settings and admin interfaces are pure UI surfaces.
- Icons from the icon library for section indicators and field decorations.
- User avatars for profile sections.
- Status icons for success/warning/error states.

---

## Signature DNA for Dense Surfaces

Marketing surfaces get natural signature moments: hero sections, feature grids, and
testimonial layouts give designers clear canvas. Dense surfaces — dashboards, data tables,
settings panels — have almost no whitespace for decoration, making signatures easy to omit.

This section maps signature moments to dense surface components so agents never generate
generic internal tools. The signature DNA from `components.signature_propagation` must
reach every surface, including the ones with no room for flourish.

### Mapping Common Signature Types to Dense Components

| Signature type | Marketing expression | Dashboard expression | Settings/Admin expression |
|---------------|---------------------|---------------------|--------------------------|
| **Active state indicator** (e.g., 2px accent underline, left border rule) | Active nav link | Selected sidebar item, active filter chip, selected table row | Active settings section, selected radio/checkbox accent color |
| **Accent glow or radiance** (e.g., cyan glow, indigo halo) | Hero grid overlay | Chart active data point, focus ring on primary action button, selected stat card border | Focus ring on save/primary button, validation success state |
| **Border accent micro-rule** (e.g., top colored border on cards) | Feature card hover | Stat card category indicator, priority badge left rule | Section group header left rule, danger zone border treatment |
| **Typography tension** (e.g., serif display + sans body) | Hero headline | Widget header serif label vs. tabular mono data | Section heading serif vs. field label sans |
| **Texture or pattern overlay** (e.g., dot grid, line pattern) | Hero background panel | Empty state placeholder background | Confirmation dialog subtle background texture |
| **Neutral family warmth/coolness** (e.g., sandstone vs. ink neutrals) | Section background alternation | Table row stripe color, sidebar background, hover fill | Form section group background, destructive zone tint |

### Rules for Dense Surface Signature Propagation

1. **Every signature component category must touch the dashboard.** If `signature_dna`
   specifies three propagation sites, at least one must be a dashboard component (sidebar
   nav, table, stat card, or chart). If it cannot reach a dashboard component, the
   signature is too delicate and needs to be strengthened.

2. **Signatures on dense surfaces must not add visual noise.** The goal is coherence, not
   decoration. A 2px accent border on a selected table row adds zero noise while visually
   connecting the dashboard to the marketing surface. A glow on every table cell is noise.

3. **Data visualization is a signature opportunity.** Chart accent colors, active state
   highlights, and tooltip style are the highest-visibility signature expressions on data
   surfaces. The chart accent must be derived from `colors.accent`, not an independent
   choice.

4. **Empty states are premium signature moments.** Dashboard empty states (no data, first
   run, error) are full-canvas surfaces that agents frequently leave generic. Apply
   illustration style, brand voice, and signature texture here.

5. **Don't omit the signature from settings.** Settings pages look identical across
   products if no signature DNA is applied. The section header style, active state on
   the settings nav, and the primary save button treatment are the minimum three sites.

### Agent Checklist for Dense Surfaces

Before submitting a dashboard or settings UI kit, verify:

- [ ] Active sidebar nav item uses the signature state indicator from `signature_dna`
- [ ] Selected table row or active filter state carries the signature mark
- [ ] Chart accent colors derived from `colors.accent` + a systematically shifted secondary
- [ ] At least one empty state uses the brand illustration style (not generic grey blob)
- [ ] Settings section headers use the correct typography tension from the system
- [ ] Danger/destructive zone uses a consistent error treatment (not ad-hoc red)
- [ ] Focus rings everywhere are the branded `--focus-ring` color, not the browser default
