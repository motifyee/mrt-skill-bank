## Design Principles

- **Errors appear inline, directly below the offending field.** Never hide validation feedback in a summary banner or require the user to scroll to find what failed.
- **Validation timing respects the user's flow.** Validate on blur (not during typing), then re-validate on input only after an error is shown -- never interrupt a user mid-keystroke.
- **Error messages describe the fix, not just the problem.** "Enter a valid email like name@company.com" is actionable; "Invalid input" is not.
- **Required fields are clearly marked; optional fields are unmarked.** An asterisk with a legend at the top establishes the convention without cluttering every label.
- **Focus rings and error rings are visually distinct.** The accent-colored focus ring tells the user where they are; the red error ring tells them something needs fixing.

### Text Input
```css
.input {
  width: 100%;
  padding: 10px 14px;
  font-family: var(--font-body);
  font-size: var(--fs-body);
  line-height: 1.5;
  color: var(--fg);
  background: var(--bg);
  border: 1.5px solid var(--border);
  border-radius: var(--radius-sm);
  transition: border-color var(--dur-base) var(--ease), box-shadow var(--dur-base) var(--ease);
  min-height: 44px;
}
.input::placeholder {
  color: var(--fg-subtle);
}
.input:focus {
  outline: none;
  border-color: var(--accent);
  box-shadow: 0 0 0 3px var(--accent-tint);
}
.input-error {
  border-color: var(--error);
}
.input-error:focus {
  /* Use token when available: var(--error-tint, rgba(220,38,38,0.15)) */
  box-shadow: 0 0 0 3px var(--error-tint, rgba(220,38,38,0.15));
}
```

### Label
```css
.label {
  display: block;
  margin-bottom: var(--space-2);
  font-family: var(--font-display);
  font-size: var(--fs-body-sm);
  font-weight: var(--fw-medium);
  color: var(--fg);
}
```

### Helper / Error Text
```css
.helper-text {
  margin-top: var(--space-1);
  font-size: var(--fs-body-sm);
  color: var(--fg-muted);
}
.error-text {
  margin-top: var(--space-1);
  font-size: var(--fs-body-sm);
  color: var(--error);
}
```

### Form Group Pattern
```html
<div class="form-group">
  <label class="label" for="email">Email address</label>
  <input class="input" type="email" id="email" placeholder="you@example.com"
         aria-describedby="email-help">
  <p class="helper-text" id="email-help">We'll never share your email.</p>
</div>
```

### Form Validation UX

**Validation timing:**
- **On blur:** Validate when the user leaves a field. Show errors immediately for that field.
- **On submit:** Validate all fields when the form is submitted. Focus the first invalid field.
- **On input (after error):** Once a field has an error, re-validate on each keystroke to clear the error as soon as the input becomes valid. Do NOT validate on input before the first blur.

**Error display rules:**
- Errors appear directly below the field, not in a summary at the top.
- Error messages are specific: "Email address is required" not "Invalid input".
- Error messages describe what to fix: "Enter a valid email like name@company.com".
- Pair error text with an error icon for visual reinforcement (color is not the only indicator).
- Never clear all errors at once; clear each field's error individually when valid.

**Accessible error markup:**
```html
<div class="form-group">
  <label class="label" for="password">Password</label>
  <input class="input input-error" type="password" id="password"
         aria-invalid="true" aria-describedby="password-error">
  <p class="error-text" id="password-error" role="alert">
    Password must be at least 8 characters
  </p>
</div>
```

**Required field indicators:**
- Mark required fields with a visual indicator (asterisk or "(required)" text).
- Explain the indicator at the top of the form: "* indicates a required field".
- Do NOT mark optional fields — required is the default signal.

**Multi-step form validation:**
- Validate each step before allowing progression.
- Show a step indicator with completion status.
- Allow backward navigation without validation.
- Preserve all entered data across steps.
- On final submit, re-validate all steps (data may have been invalidated by server-side checks).

**Inline validation patterns for common fields:**

| Field | Validation Rule | Error Message |
|-------|----------------|---------------|
| Email | `/.+@.+\..+/` pattern | "Enter a valid email address" |
| Phone | Length + digits only | "Enter a 10-digit phone number" |
| Password | Min 8 chars, 1 uppercase, 1 number | "Must be at least 8 characters with 1 uppercase letter and 1 number" |
| URL | Starts with http/https | "Enter a URL starting with https://" |
| Required text | Non-empty after trim | "This field is required" |
| Number range | min/max values | "Enter a value between {min} and {max}" |
