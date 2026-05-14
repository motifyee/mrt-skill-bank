# Content Design Foundations

Voice, tone, copy patterns, microcopy, error messages, and content strategy
for professional design systems. Content is not decoration — it is design.

## Design Principles

1. **Voice is constant; tone shifts by context:** Lock the brand personality (voice) and adapt only the emotional register (tone) for onboarding, errors, empty states, and destructive actions.
2. **Every element has a length target:** Enforce word counts per element type (5-9 words for hero headlines, 1-3 words for buttons, 1-2 sentences for tooltips) and cut anything that exceeds them.
3. **Write imperatives, not descriptions:** Button labels and CTAs always start with a verb ("Start a project", not "Project starter"); error messages always state what happened and how to fix it.
4. **Match density to trust level:** Serve sparse, proof-oriented copy to first-time visitors and denser, data-rich copy to authenticated daily users.

---

## Voice and Tone

### What voice means in a design system

Voice is the consistent personality behind every word. Tone is how that personality
adapts to context. A design system documents voice (constant) and provides tone
guidance for common situations (variable).

### Establishing voice

Voice emerges from three inputs:
1. **Product identity** — what the product does and for whom
2. **Audience expectations** — how the audience expects to be spoken to
3. **Brand personality** — the human characteristics the brand embodies

Common professional voice profiles:

| Profile      | Characteristics                           | Example brands       |
|-------------|-------------------------------------------|----------------------|
| Direct/Technical | Precise, specific, confident. No fluff. | Vercel, Linear, Stripe |
| Warm/Approachable | Friendly but not casual. Helpful. | Airbnb, Notion       |
| Authoritative | Expert, trustworthy, definitive. | Bloomberg, The Economist |
| Playful/Creative | Witty, surprising, human. | Mailchimp, Dollar Shave Club |
| Premium/Restrained | Elegant, minimal, exclusive. | Apple, Aesop        |

### Tone adaptation by context

| Context             | Tone shift                              |
|---------------------|-----------------------------------------|
| Onboarding          | Encouraging, clear, low-pressure        |
| Success states      | Brief, confirming, no celebration needed |
| Error states        | Empathetic, specific, solution-oriented |
| Empty states        | Helpful, inviting, action-oriented      |
| Destructive actions | Serious, clear, requiring confirmation  |
| Marketing/landing   | Confident, specific, benefit-driven     |

---

## Copy Length Rules

### By element type

| Element            | Target length          | Rules                                    |
|--------------------|------------------------|------------------------------------------|
| Hero headline      | 5-9 words              | Never a full sentence. Statement or phrase. |
| Hero subhead       | 1 sentence, 1 clause   | If it needs a comma, cut it.              |
| Section headline   | 2-6 words              | Describes the section. No verbs needed.   |
| Body paragraph     | 2-4 sentences          | Whitespace frames it; copy doesn't fill it. |
| Button label       | 1-3 words              | Verb + optional noun. Always imperative.   |
| Form label         | 1-3 words              | Noun phrase. What the field is.            |
| Error message      | 1-2 sentences          | What happened + how to fix it.             |
| Tooltip            | 1-2 sentences          | Brief, contextual, non-obvious info only.  |
| Navigation item    | 1-2 words              | Noun or verb. Consistent part of speech.   |
| Badge/tag          | 1-2 words              | Status or category. Title case.            |

### What to avoid in all copy
- Startup hype words: synergy, revolutionary, disruptive, passionate, game-changing
- Empty intensifiers: truly, really, incredibly, amazing, very
- Exclamation marks (almost never justified in professional UI)
- Emojis in product or marketing copy (unless the brand is explicitly playful)
- Placeholder text left in production (no "Lorem ipsum" in deliverables)
- Vague language: "various", "multiple", "several" — be specific

---

## Casing Rules

Choose one and enforce everywhere:

| Style          | Use when                                   | Example              |
|----------------|--------------------------------------------|----------------------|
| Sentence case  | Default for most professional brands       | "Start a project"    |
| Title case     | Traditional/enterprise brands              | "Start A Project"    |
| All caps       | Labels and eyebrows only (with tracking)   | "FEATURES"           |
| Lowercase      | Deliberate casual brands                   | "start a project"    |

Most modern design systems use sentence case for headings, buttons, and navigation.
Document the choice in the system and enforce it everywhere.

---

## Microcopy Patterns

### Button copy
- Primary CTAs: verb + noun — "Start a project", "Book a call", "See pricing"
- Secondary CTAs: shorter — "Learn more", "View details", "Contact"
- Never: "Click here", "Submit", "Click here to learn more!"

### Voice-transformed examples

Use these examples to keep marketing, docs, forms, and notifications in the
same dialect across agents.

| Intent | Direct/Technical | Warm/Approachable | Authoritative | Playful/Creative | Premium/Restrained |
|---|---|---|---|---|---|
| Hero headline | Deploy safely in minutes | Care moves with you | Proven systems for critical work | Make the messy part disappear | Designed for quiet confidence |
| Primary button | Start deploy | Get started | Request access | Try the magic | Begin |
| Form error | Email needs an @ sign. | Add an @ so we can reach you. | Enter a valid email address. | That email is missing its @. | Use a valid email. |
| Empty state | No runs match this filter. | Nothing here yet. Create your first one. | No records are available for this view. | Clean slate. Want to add the first? | No items yet. |
| Toast success | Changes saved. | Your changes are saved. | Changes were saved successfully. | Saved. Tiny victory. | Saved. |
| Docs intro | Use tokens to keep UI consistent. | Tokens help every screen feel familiar. | Tokens establish the system contract. | Tokens are the shared language of the interface. | Tokens define the visual standard. |

### Form validation
- Inline validation as the user types (not only on submit)
- Positive: show the field is valid before submission ("Looks good!")
- Error: explain what's wrong and how to fix ("Email needs an @ sign")
- Never: "Invalid input", "Error", "Field is required" (say which field)

### Empty states
- Explain why it's empty (first time? filtered to nothing? permission issue?)
- Provide a clear next action ("Create your first project")
- Use an illustration or icon, not just text

### Loading states
- Skeleton screens for known layouts
- Progress indicators for known durations
- Spinners for indeterminate, short waits only
- Never block the entire page for a partial load

### Error messages (page-level)
- What happened: "We couldn't save your changes."
- Why (if helpful): "Your session expired."
- What to do: "Sign in again and try."

### Confirmation dialogs
- State what will happen: "Delete this project?"
- List consequences: "All files and settings will be permanently removed."
- Clear action labels: "Delete project" (destructive) / "Keep project" (safe)
- Never: "Are you sure?" without context

---

## Pronouns and Person

| Person    | Use when                              | Example                     |
|-----------|---------------------------------------|-----------------------------|
| First person plural | Brand/company speaking    | "We ship in six weeks."     |
| Second person | Addressing the user directly  | "Your projects are here."   |
| Third person | Describing features neutrally | "Projects can be shared."   |

Choose based on the product type:
- B2B services and agencies: "we" for the company, "you" for the client
- Consumer products: "you" for the user, avoid "we"
- Technical tools: often avoid pronouns entirely — "Projects save automatically."

Document the choice and stick with it. Mixing persons in the same interface
creates an inconsistent voice.

---

## Content Density

### How much content per surface

| Surface type       | Content density    | Character count per viewport  |
|--------------------|--------------------|-------------------------------|
| Landing/hero       | Low (1 primary message + 1 CTA) | < 50 words       |
| Features section   | Medium (3-6 feature cards) | 150-300 words total  |
| Pricing page       | Medium (comparison + tiers) | 200-400 words      |
| Blog/article       | High (long-form)    | 500-2000+ words               |
| Dashboard          | Mixed (data + labels) | 50-100 words per panel      |
| Form               | Low (labels + helpers) | 20-50 words per form       |

### Content density derives from trust level

- **High trust** (logged in, daily use): denser, more data, less hand-holding
- **Medium trust** (considering purchase): balanced, benefit-driven, clear CTAs
- **Low trust** (first visit): sparse, focused, proof-oriented, one message at a time

Match content density to where the user is in their journey.
