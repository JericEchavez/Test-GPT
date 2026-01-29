# Filter + Focus Dropdown UX (Cloud Architecture Canvas)

## A) Information architecture (sections of dropdown)
1. **Header / Mode switcher**
   - Title: “Filters & Focus”
   - Two tabs or segmented control: **Filters** | **Focus**
   - Inline microcopy explaining each
2. **Applied section (per tab)**
   - Summary header with count badge
   - List of applied items with readable summaries
   - Actions: Remove item, Clear all
3. **Builder section (per tab)**
   - Simple mode default (Match: **ALL** / **ANY**)
   - Rule rows with [Field] [Operator] [Value] [⋯] [x]
   - Primary actions: Apply, Reset
4. **Advanced mode (optional)**
   - Toggle: “Advanced (Groups)”
   - Grouped logic builder with nested “Match ALL/ANY” per group
   - Per-group actions: add rule, add group
5. **Footer / system states**
   - Empty state (no applied rules)
   - No-results state (rules exclude all components)
   - Loading state (values/metadata)

---

## B) Wireframe-style layout (ASCII)
```
┌──────────────────────────────────────────────────────────────┐
│ Filters & Focus                                               │
│ [ Filters ] [ Focus ]                                         │
│ Filter: Hide non-matching components.                         │
│ Focus: Dim/grayscale non-matching components.                 │
├──────────────────────────────────────────────────────────────┤
│ Applied Filters  [2]                               Clear all  │
│ • Name contains “prod”  AND  Region = us-east-2      [x]      │
│ • Type = RDS  AND  State = available                 [x]      │
├──────────────────────────────────────────────────────────────┤
│ Build Filter                                                   │
│ Match: (● ALL) (○ ANY)                                       │
│ [Field ▼] [Operator ▼] [Value …] [⋯] [x]                      │
│ [Field ▼] [Operator ▼] [Value …] [⋯] [x]                      │
│ + Add condition                                               │
│ Advanced (Groups) ▸                                           │
│                                                              │
│ [Reset]                                       [Apply Filter]  │
├──────────────────────────────────────────────────────────────┤
│ Empty state: No filters applied.                              │
│ Tip: Use Filters to hide noise; use Focus to dim context.     │
└──────────────────────────────────────────────────────────────┘

(Advanced open)
┌──────────────────────────────────────────────────────────────┐
│ Advanced (Groups)                                             │
│ Group A — Match: (● ALL) (○ ANY)                              │
│  [Field ▼] [Operator ▼] [Value …] [⋯] [x]                     │
│  [Field ▼] [Operator ▼] [Value …] [⋯] [x]                     │
│  + Add condition     + Add group                              │
│ Group B — Match: (○ ALL) (● ANY)                              │
│  [Field ▼] [Operator ▼] [Value …] [⋯] [x]                     │
│ AND/OR between groups: (○ AND) (● OR)                         │
└──────────────────────────────────────────────────────────────┘
```

---

## C) Detailed list of actions and controls per section
### 1) Header / Mode switcher
- **Tabs:** Filters | Focus
- **Microcopy:**
  - Filters: “Hide non‑matching components.”
  - Focus: “Dim non‑matching components to keep context.”
- **Behavior:** Switching tabs preserves draft state for each tab separately.

### 2) Applied Filters section
- **Count badge**: number of applied filters
- **Applied item list**: human-readable summaries
- **Actions**:
  - **Remove** (x) removes single applied item
  - **Clear all** removes all applied items in this tab

### 3) Applied Focus section
- Same structure as filters; separate count badge and list

### 4) Builder (Simple mode)
- **Match ALL/ANY toggle** always visible
- **Rule row controls**:
  - Field picker
  - Operator picker
  - Value input
  - Overflow “⋯” for advanced row options (scope/ancestry)
  - Remove row “x”
- **Actions**:
  - **Add condition**
  - **Apply**
  - **Reset** (clears current draft only)

### 5) Advanced mode (Groups)
- **Advanced toggle** (collapsed by default)
- **Group header** with Match ALL/ANY
- **Group actions**: Add condition, Add group
- **Between-groups logic**: AND/OR selector
- **Row overflow (⋯)** exposes Scope (ancestry/dependencies)

### 6) States
- **Empty**: “No filters applied.” or “No focus rules applied.”
- **No results**: “No components match. Try loosening conditions.”
- **Loading**: skeleton rows and “Loading values…” for async metadata

---

## D) Dropdown option lists
### Field/property picker (grouped)
- **Identity**
  - Name
  - Type (e.g., EC2, RDS, S3)
  - Category (Compute, Storage, Database, Networking)
  - Product
- **Location**
  - Region
  - Account
  - Environment
- **Tags**
  - Tag (key)
  - Tag (value)
  - Tag count
- **State**
  - State (available, stopped, error)
- **Relationships**
  - Relationship type (depends on, connected to)
  - Connected to (component)

### Operator picker (by data type)
- **Text (name, product, tag value)**
  - equals / not equals
  - contains / does not contain
  - starts with / ends with
  - is empty / is not empty
- **Enum (type, category, region, environment, state)**
  - is / is not
  - is one of / is none of
- **Numeric (tag count)**
  - =, ≠, >, ≥, <, ≤
- **Relationship**
  - is related to
  - is not related to
  - relationship type is

### Value picker behaviors
- **Searchable list** for enums (type, region, environment)
- **Multi-select** for “is one of”
- **Recent values** shown first
- **Free text** for name/product/tag value
- **Async loading** for large lists; show inline spinner and “Loading…”
- **Empty option** for “is empty” (value field disabled)

---

## E) Applied summary formatting rules
- **Simple mode**: single summary line per applied item
  - Format: “Field Operator Value”
  - Join with **ALL** = “AND” or **ANY** = “OR”
  - Example: `Name contains “prod” AND Region is us-east-2`
- **Advanced mode**:
  - Group summaries: “(Group A: Name contains “prod” AND Type is RDS)”
  - Group connectors: “OR (Group B: Environment is prod OR State is available)”
- **Display chips**:
  - Each rule row becomes a pill: `Name · contains · “prod”`
  - Group pills are wrapped with labeled prefix

---

## F) Interaction rules
- **Live preview**: default ON for simple changes (shows immediate canvas update).
- **Apply button**: required when advanced groups are open or when user disables live preview.
- **Reset**: clears draft rules in the current tab; does not affect applied items.
- **Close dropdown with unapplied changes**:
  - Prompt: “Discard changes?” [Discard] [Keep editing]
  - If live preview is on, closing keeps applied changes.
- **Clear all**: removes all applied rules and restores full canvas.

---

## G) UX examples
### 1) Simple Filter (ALL) with 2 conditions
- **Match: ALL**
- Rules:
  1. Name contains “prod”
  2. Region is us-east-2
- Applied summary:
  - `Name contains “prod” AND Region is us-east-2`

### 2) Focus (ANY) with 2 conditions
- **Match: ANY**
- Rules:
  1. Type is RDS
  2. State is available
- Applied summary:
  - `Type is RDS OR State is available`

### 3) Advanced grouped logic
- **Group A (Match ALL)**
  - Category is Database
  - Environment is prod
- **Group B (Match ANY)**
  - Region is us-east-2
  - Tag value contains “pci”
- **Between groups**: OR
- Applied summary:
  - `(Group A: Category is Database AND Environment is prod) OR (Group B: Region is us-east-2 OR Tag value contains “pci”)`
