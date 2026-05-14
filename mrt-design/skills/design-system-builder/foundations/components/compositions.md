## Design Principles

- **Compositions define data flow between atomic components.** Each composition specifies which components participate, how data moves between them, and what triggers state changes.
- **A composition's layout diagram is a contract.** The ASCII diagram shows the spatial relationship of components; implementers should preserve this structure unless the design system explicitly overrides it.
- **Compositions reuse atomic components without modification.** A Data Table composition uses the Table, Pagination, Input, and Badge components as-is -- it wires them together, it does not extend their internals.
- **Every composition has a defined trigger-response cycle.** User action on one component (e.g., typing in a filter Input) produces a deterministic response in another (e.g., the Table re-renders, Pagination resets to page 1).

Larger UI units built by combining the atomic components above. Each composition describes the components involved, a layout diagram, and data flow.

### Search Bar

**Components**: Input + Button + Dropdown (autocomplete suggestions)

```
+----------------------------------------------------------+
|  [🔍] [Search input_______________] [Search btn]         |
|  +----------------------------------------------------+  |
|  | Suggestion 1                          |  <- Dropdown |
|  | Suggestion 2 (highlighted)            |    panel     |
|  | Suggestion 3                          |    (hidden   |
|  +---------------------------------------+    when      |
                                                 empty)    |
+----------------------------------------------------------+
```

**Data flow**: User types into Input. On each keystroke (debounced 300ms), fetch suggestions and render them in the Dropdown panel. Arrow keys navigate suggestions (reuse Dropdown keyboard behavior). Enter selects the highlighted suggestion and populates the Input value. Clicking the Button submits the search with the current Input value.

---

### Data Table

**Components**: Table + Pagination + Input (filter) + Badge (status column)

```
+----------------------------------------------------------+
|  [Filter input____________] [Select: per page v]         |
|  +------------------------------------------------------+|
|  | Name          | Email              | Status           ||
|  |---------------|--------------------|------------------|
|  | Alice Chen    | alice@example.com  | [Active]  <-Badge||
|  | Bob Kumar     | bob@example.com    | [Pending] <-Badge||
|  | Carol Diaz    | carol@example.com  | [Active]  <-Badge||
|  +------------------------------------------------------+|
|  [< Prev]  1  [2]  3  ...  12  [Next >]     <- Pagination|
+----------------------------------------------------------+
```

**Data flow**: Filter Input narrows the dataset (client-side filter or API query). Table renders rows from the filtered dataset. Status column uses Badge variants (success for Active, warning for Pending, error for Inactive). Pagination slices the dataset into pages. Changing the filter resets to page 1. Changing per-page Select updates Pagination and re-renders Table.

---

### Form Section

**Components**: Input group + Toggle + Select + Button (submit) + Toast (confirmation)

```
+----------------------------------------------------------+
|  Account Settings                                         |
|  +------------------------------------------------------+|
|  | Name:   [_____________________]           <- Input    ||
|  | Email:  [_____________________]           <- Input    ||
|  | Role:   [Admin v]                         <- Select   ||
|  | Notifications: [O==] Enable              <- Toggle    ||
|  +------------------------------------------------------+|
|                          [Save changes]       <- Button  |
+----------------------------------------------------------+
                                        [Saved successfully] <- Toast
+----------------------------------------------------------+
```

**Data flow**: User fills form fields (Inputs, Select, Toggle). On "Save changes" Button click, validate all fields. If validation passes, submit data. On success, show a success Toast. On failure, show error states on invalid fields and an error Toast. Toggle state is read from `aria-checked`. Select value is read from the selected option's `data-value`.

---

### Card Grid

**Components**: Card x N + Pagination + Input (search filter)

```
+----------------------------------------------------------+
|  Browse Products                                          |
|  [Search input___________________________]                |
|  +------------+ +------------+ +------------+             |
|  | Card       | | Card       | | Card       |             |
|  | [Image]    | | [Image]    | | [Image]    |             |
|  | Title      | | Title      | | Title      |             |
|  | $49.99     | | $29.99     | | $79.99     |             |
|  | [Add to ^] | | [Add to ^] | | [Add to ^] |             |
|  +------------+ +------------+ +------------+             |
|  +------------+ +------------+ +------------+             |
|  | Card       | | Card       | | Card       |             |
|  | ...        | | ...        | | ...        |             |
|  +------------+ +------------+ +------------+             |
|  [< Prev]  1  [2]  3  [Next >]              <- Pagination|
+----------------------------------------------------------+
```

**Data flow**: Cards are rendered from a product data array. The search Input filters the array (by title, description). The filtered results are paginated. Pagination controls which slice of cards is visible. Each Card's "Add to cart" Button triggers a callback with the product ID and shows a confirmation Toast.

---

### Settings Panel

**Components**: Toggle switches + Select dropdowns + Button (save) + Toast (success)

```
+----------------------------------------------------------+
|  Preferences                                              |
|  +------------------------------------------------------+|
|  | Appearance                                              |
|  | Theme:       [Light v]                     <- Select   ||
|  | Dark mode:   [==O]  Disabled              <- Toggle    ||
|  |                                                        ||
|  | Notifications                                           |
|  | Email alerts: [O==]  Enabled              <- Toggle    ||
|  | Frequency:   [Weekly v]                   <- Select    ||
|  | Push alerts:  [==O]  Disabled             <- Toggle    ||
|  +------------------------------------------------------+|
|                          [Save preferences]    <- Button  |
+----------------------------------------------------------+
                                     [Preferences saved]    <- Toast
+----------------------------------------------------------+
```

**Data flow**: Settings are loaded into Toggle and Select components from a saved preferences object. User modifies Toggles (on/off) and Selects (dropdown choice). On "Save preferences" Button click, collect all current values and persist them. On success, show a success Toast. On error, show an error Toast with retry option. Toggles may trigger immediate preview (e.g., dark mode toggle changes theme without save).
