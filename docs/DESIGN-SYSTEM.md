# Console Design System

Working record of the Kumbuka design system, built in dispatch://kumbuka/sprint/218.2 and
218.16. It states what has been ratified with the operator and what is still open. The
values live in `tokens/tokens.css`, the components in `css/components.css`;
`showcase/index.html` renders both for review. The unified console
(`platform-specs/design/unified-console/`) is the concrete application: it keeps the shell
and the product pages and builds on this design system. The JBAConsult
design system stays the basis of all paper output (PDF, documents) and is not changed
by this work.

Status: in progress, state of 2026-11-24 midday. Continued in dispatch://kumbuka/sprint/218.16.
Sections marked "proposed" are built in the showcase and await ratification; "ratified"
sections are binding; "open" sections are not yet worked.

## 1. Mixing rule (ratified)

| Area | Source | Rule |
|---|---|---|
| Token architecture | Forgenesis | Primitives, then semantic tokens per theme, then components. Components read semantic tokens only. Status as fg/bg triple. |
| Themes | new | Light and dark, both complete. Light is default; `data-theme` on `<html>` overrides `prefers-color-scheme`. High contrast is reserved for 2027. |
| Brand colours | Kumbuka logo | Navy `#15233F` is the primary colour; orange `#FF5B1F` is the create action and the logo mark only. No JBAConsult paper tone, no Forgenesis steel or copper. |
| Typography | mixed | Inter for the UI, Space Grotesk for the page title only, JetBrains Mono for addresses, identifiers and code. Base 14 px. |
| Density | Forgenesis | Row, input and button tokens with a comfortable and a compact step. |
| Shape | mixed | Radius 4 / 6 / 10 px. Hairlines carry the structure; shadows only for floating layers. |
| Components | Forgenesis as quarry | Rewritten, not copied. |
| Icons | new | Lucide (ISC licence). Fixed action vocabulary. Icon-only buttons always carry a tooltip and `aria-label`. |

## 2. Themes (ratified)

The same recipe in both themes: neutral surfaces, navy as the only marking colour in
three stages, orange only for creating. Per token only the value changes.

| Token | Light | Dark |
|---|---|---|
| `--bg-canvas` | `#f4f5f7` | `#13161d` |
| `--bg-frame` | `#ffffff` | `#0e1218` |
| `--bg-panel` | `#ffffff` | `#1a202c` |
| `--bg-hover` (stage 1) | `#eeeff1` | `#212835` |
| `--bg-selected` + `--edge-selected` (stage 2) | `#e7e8e9` + `#15233F` | `#252e3f` + `#7386ab` |
| `--fill-anchor` (stage 3, one per level) | `#15233F` | `#35476b` |
| `--fg-1` / `--fg-2` | `#15233F` / `#505b6f` | `#dfe1e5` / `#a4a8af` |

Rejected on the way: a navy canvas in dark mode (the brand colour as wallpaper carries no
meaning and overwhelms), saturated navy cards with a bright pastel anchor (too loud), and
any baby-blue selection tint in light mode.

## 3. Colour ring and user palette (ratified)

Twelve hues on one OKLCH lightness and chroma grid (Ottosson, Oklab 2020; CSS Color 4),
so that all hues carry the same perceived weight. Every hue has these stops: tint (chip
ground), deep (text on tint), vivid (light theme only: syntax colours, coloured icons,
chart lines; OKLCH 0.52 / 0.17), strong and strong-fg (user palette), and the dark
counterparts. All 48 tint/text pairs measure between 5.7:1 and 9.1:1.

Reserved hues: crimson 15° error, orange 45° brand and create, ochre 80° warning,
green 150° success, navy 263° anchor and info. Error and orange are 30° apart; warning
moved to ochre so that nothing status-like sits next to the brand orange.

User palette for scopes and tags: ten hues (all except crimson and orange) in two
expressions, soft and strong, giving 20 colours. A new tag starts with a colour derived
from its name (stable hash), which the user can change before creating it. Free hex
values are not allowed; a later option is a customer-chosen hue angle from which the
system derives all stops on the same grid.

Deferred: mapping the hues to memory types (decided with the memory block, once the set
of types is final).

## 4. Typography (ratified)

| Role | Font | Size / weight |
|---|---|---|
| Page title | Space Grotesk | 24 / 600, tracking -0.02em, once per page |
| Section, subsection | Inter | 16 / 600, 14 / 600 |
| Body | Inter | 14 / 400, line height 1.5 |
| Table | Inter | 13, tabular figures |
| Label, meta | Inter | 12 / 500, 12 / 400 |
| Address, inline code | JetBrains Mono | 12.5 |
| Id in tables, hash, timestamp | JetBrains Mono | 12 |
| Code block | JetBrains Mono | 13 |
| Eyebrow | JetBrains Mono | 11 / 500, uppercase, 0.08em, prefixed `// ` |

An address that leads somewhere is a link (link colour plus underline); an address that
only identifies is shown in secondary colour with a copy icon. Amounts and dates stay in
Inter with tabular figures; mono is for what people copy, not for numbers they read.

## 5. Buttons and states (ratified)

| Level | Look | Use |
|---|---|---|
| Primary create | orange fill, navy text | new item, start process |
| Primary confirm | green fill | save, apply, OK |
| Primary danger | crimson fill | delete, withdraw; destructive only |
| Secondary | neutral, border | cancel, back, reset |
| Tertiary | no border, icon plus text | side functions: save filter, add condition |

Split button: the main part runs the most likely action (e.g. accept), the chevron opens
the alternatives with icon and one-line description (e.g. rework, reject). Menu button:
the whole button opens a menu and the choice is the action (e.g. new item from template).
More actions: icon-only button with an ellipsis; destructive entries sit last, after a
separator, in error colour. Menus close on Escape and outside click; arrow keys move.

Cancel is never red: red keeps its warning value only if it is reserved for destructive
actions, and a green/red pair is indistinguishable under deuteranopia (measured ΔE 4.7).

Disabled controls use `aria-disabled="true"`, never the `disabled` attribute, so that they
stay focusable and can explain themselves. The tooltip appears instantly for disabled
controls (Floating UI, MIT) and on focus as well as on hover. "No permission" shows a lock
icon and a dashed border; "not possible yet" is only dimmed.

## 6. Form field (ratified)

- Label on top. Required fields carry `*` in error colour, optional fields the word
  "optional".
- Exactly one line below the control: help text, or once invalid the error message in its
  place. Errors carry an icon and a sentence that says what to do, and are linked to the
  control with `aria-describedby`.
- Error presentation (ratified 2026-11-24): a single field gets a 2 px border in error colour,
  a warning icon inside the field on the right and the message below (variant C). A group
  (checkbox group, choice cards, composite field, repeatable group) gets a bar on the left
  of the whole group and the message above its options (variant D). No red tint: it turns
  a form with several errors into a Christmas tree and reads badly in dark mode.
- Validation on blur, never while typing. On save, an error summary appears at the top of
  the form, takes focus and links every entry to its field.
- Units are a fixed suffix inside the control; the read state formats the value.
- Grouping in fieldsets with a heading and a trailing rule, inside a two-column grid;
  long fields span both columns.
- Read state B: label and value without a frame at rest; on hover the field shows a frame
  (`--read-hover-border`, `--read-hover-bg`; lifted, never sunken, in dark mode). This
  prepares in-place editing, whose mechanics belong to the pattern round (sprint/218.3).
- Every field type defines its read representation. Checkbox: the sentence stays, with a
  read-only box before it.

## 7. Combined controls, round 1 (ratified)

- Dual list selector (PatternFly "Dual list selector", Salesforce "Dueling Picklist") with
  search on the left, move buttons, and reordering on the right by drag and drop
  (implementation: dnd-kit, keyboard capable) and up/down buttons. Use from about 20
  options upward; below that, a tag input or select.
- Tag input with suggestions, creation of new tags and colour choice at creation.
- Filter builder: one joined bar per condition (field, operator, value), "all/any" logic,
  addressable filter string, remove on hover.
- No native `select`: menus and listboxes are custom, built on an accessible headless
  component (Radix Select or React Aria).

## 8. Further field types (in review)

Each type defines an edit and a read representation.

| Type | Edit | Read |
|---|---|---|
| Radio | dot, row or column; optional description line per option | selected option as text |
| Segmented | 2 to 4 equal options, takes effect at once | selected option as text |
| Toggle | switch with sentence; takes effect at once. Inside a form with save it is a checkbox instead | state chip (an/aus) plus sentence |
| Date | typed input (TT.MM.JJJJ, "12.11.", "heute") plus calendar; ISO 8601: week starts Monday, week number column, today outlined, weekend in secondary colour | date, weekday and week number |
| Date with bounds | as date; days outside the bound are struck through and not selectable in the calendar; a typed date outside the bound fails on blur with the reason ("not in the past" / "not in the future") | as date |
| Date and time | date plus time field side by side; time typed in any form (14:30, 1430, 14) or picked from a 15-minute list; 24-hour clock | "26.11.2026, 14:30 Uhr", weekday and week number |
| Time only | time field with 15-minute list, free entry to the minute | "08:00 Uhr" |
| Date range | calendar with presets (today, last 7 / 30 days, this month, last quarter), start then end | range and number of days |
| Text with counter | input with "n / max" counter; warning colour from 90 %, error above the limit | text |
| Number | stepper (minus, value, unit, plus), bounds and step; typed value checked on blur | value with unit |
| Secret / token | masked (prefix and last four), reveal and copy; shown in full once at creation only | masked, with creation date |
| E-mail, URL | prefix icon, format check on blur; URLs https only | as link |
| File upload | drop zone (drag and drop or choose), type and size hint, list with icon, size, progress and remove | file list |
| Markdown text | source editor (CodeMirror 6 in the product) with toolbar (bold, italic, heading, list, code, link), shortcuts ⌘B ⌘I ⌘K, write / split / preview | rendered |
| Integer | right-aligned, tabular figures, thousands separators on blur | formatted number |
| Decimal, percent | fixed decimals; percent as suffix with bounds 0 to 100 | formatted number, "%" |
| Currency | currency picker joined to the amount; accepts "7.400,50", "7400,5" and "7400.50"; formats on blur | Intl currency format, e.g. "7.400,50 €" |
| Duration | free entry ("1 h 30 min", "90 min", "1:30", "2 T 4 h") | "1 Std 30 Min" |
| Timestamp (logs) | TT.MM.JJJJ HH:MM:SS.mmm in mono, milliseconds optional; local or UTC switch; stored in UTC | date, time, milliseconds in secondary colour, zone name |
| Person and role picker | search with avatars; a role includes all its members | avatars with names |
| Amount with threshold | joined bar: comparison, amount, unit | "ab 5.000,00 €" with the field it applies to |
| Key-value editor | rows of key (mono), type, value; add and remove | key/value list |
| Tree selection | tri-state boxes (partial = dash), expandable | selected paths as chips |

Times are stored in UTC and shown in the user's time zone; the zone is written out only
where it differs from the user's or in evidence views (audit, K8).

Later, not in 218.2: a location field (geodata). Direction without Google: OpenStreetMap
tiles with MapLibre GL (BSD) and a geocoder such as Photon or Nominatim, self-hostable.

Open, later: a WYSIWYG mode for business users without an IT background. Explaining
Markdown to them is a marketing problem; the source editor stays the basis because the
text is a source for people and agents alike and must be stored exactly as written.

## 9. Grouping building blocks (in review)

| Block | Rule |
|---|---|
| Checkbox group | one label for the group, "select all" with tri-state and count, one or two columns, minimum/maximum checked at change |
| Choice cards | radio or checkbox as a card with icon, title and description; for options that need explaining (e.g. four-eyes kind) |
| Composite field | several inputs forming one value (address, name, phone); one label, sub-labels below each input, one shared error line |
| Repeatable group | block with header (handle, number, remove), "+ further item" as tertiary button; new block focuses its first field |
| Conditional fields | revealed fields hang off their trigger with an indented navy rule; when the trigger changes they disappear; their values are kept while editing, so an accidental switch loses nothing, but are not saved while hidden |
| Disclosure section | rarely used fields behind a summary line with chevron and field count |
| Computed value | dashed frame, "berechnet" chip, tooltip with the formula and its inputs; never editable, updates live |

## 10. Breadcrumb and tabs (in review)

Breadcrumb for the Kumbuka hierarchy (start, scopes, scope, item, dispatch, child
dispatch). Every element leads up; an element with siblings (the scope) carries a chevron
to switch sideways; long paths collapse their middle into an ellipsis menu; the current
page is not a link and is followed by a copy-address button. How breadcrumb, side
navigation, search and jump by address work together is the navigation concept of K1
(sprint/218.4).



Tabs structure blocks within one page (file tabs, settings), at most six. More sections
need a side navigation, not tabs; there is no overflow menu. Optional count badge and an
error dot that shows a tab contains invalid fields. Arrow keys move between tabs
(WAI-ARIA tabs pattern, roving tabindex). Page-level error box, wizard and page layouts
belong to the pattern round (sprint/218.3).

## 11. Review of 2026-11-24 (fixed)

Automated check with axe-core 4.10 (WCAG 2.0/2.1/2.2 A and AA) over the showcase, both
themes, edit and read mode. Before: four rule violations (contrast, labels, tree roles,
keyboard access to an empty listbox). After: none.

- `--fg-3` was 4.36:1 on sunken light surfaces. Now `#5f6776` light (at least 4.6:1 on
  every light surface, selection included) and `#949aa4` dark (4.8:1 on selection).
- Every control is linked to its label (`label for` or `aria-labelledby`); a field without
  a linked label must not exist, and the component enforces it.
- Tree selection implements the WAI-ARIA tree pattern (treeitem, level, expanded, checked
  with mixed; arrow keys, space). Listboxes implement the listbox pattern (focusable,
  active descendant, arrow keys, space).
- Locked exists for every field type (select, date, radio, checkbox, toggle, tag input):
  dashed frame or dimmed control, lock mark at the label, focusable, reason in the
  tooltip. In read mode a locked field looks like any other.
- System values (set by the platform, e.g. item number, created at) are never inputs:
  read form with a "System" chip and a tooltip naming the source.
- One focus style everywhere: 2 px outline in `--focus-color` with 2 px offset.
- Loading and failure: async search shows a spinner with "Suche läuft …" as status;
  an empty result says what to check; an upload that fails stays in the list with its
  reason (e.g. too large) instead of disappearing.
- Markdown headings in user content are shifted below the heading level of their
  context (product rule; the showcase renders them as h3).

## 12. Table (ratified 2026-11-24, sprint/218.16)

Large data sets are the normal case in Kumbuka (worklist, dispatch, memory, audit), so
the table, not the card, is the main way to show many objects.

- One table core (`k-table`) with variants by switch, never separate components:
  list (built), grouped and ordered, expandable; card view and tree table follow as
  further variants over the same core (same data, filter, sort, selection). Order of
  work: list, grouped/ordered, expandable, card view, tree table.
- Native table semantics, no `role="grid"`. Sorting is a button in the header cell;
  `aria-sort` sits on the primary sort column only; a polite status announces the order.
- Sort: a click sorts by one column, two states (ascending, descending), never unsorted,
  because a server list always needs a defined order. Multi-sort is a per-instance
  switch: shift-click adds a column, the header shows its rank, and the "Sortierung"
  popover lists the sort keys as a sortable list with direction, up/down and remove.
  The popover is the real path (keyboard, discoverable); shift-click is the shortcut.
- Expandable rows (to be built): a fixed detail form with an upper bound: at most one
  text block clamped to four lines, at most six key-value pairs in two or three columns,
  a footer with "Öffnen". No nested tables, tabs, forms or further expanders. Several rows
  may be open; "collapse all" in the header; the open state is not stored. Only the
  chevron expands; the row's own link stays the entry to the detail (sprint/218.3).
- Row actions: the more-actions button is always visible, never hover-only; at most one
  direct action icon, also always visible. Destructive entries last, after a separator.
- Selection: checkbox column with a tri-state header for the page. While anything is
  selected, the bulk bar replaces the toolbar in place. When the whole page is selected,
  the bar offers "select all n matches".
- Density follows the page (`data-density`) and can be overridden per user and table in
  the table's settings menu. Density tokens may be set on any subtree.
- Pagination is a per-instance switch: pages with size 25, 50 or 100, total count and
  page jump. No "load more" (it loses the position when returning from the detail).
  Virtualisation is decided with audit and logs (K8), not built here.
- Height (fixed 2026-11-24 after a double scroll in the showcase): embedded is the default;
  the table grows with its rows, the page scrolls vertically, the table only sideways.
  Fill is for the list page: the table takes the content area, the page does not scroll,
  and the table is the one vertical scroll with a sticky head. Never a fixed height with
  an inner scroll inside a scrolling page. Scrollbars take theme colours.
- Wide tables: column chooser (the dual list) and horizontal scroll with selection,
  number and title fixed on the left. The head is sticky in fill mode. The scroll container is
  focusable and labelled; a re-render keeps its scroll position.
- Alignment: amounts and counts right-aligned with tabular figures, dates with tabular
  figures, identifiers in mono; the header follows its column's alignment.
- States: first load shows skeleton rows under the real head; reload keeps the data with
  a thin progress bar and `aria-busy`. Empty comes in two kinds: nothing exists yet
  (invitation with the create action) and nothing matches (reset the search or filter).
  A load failure shows inside the table body with "Erneut versuchen"; the page-level
  error box belongs to sprint/218.3.
- Sortable list outside the table: drop marker is a line, position number per instance
  (on where order carries meaning), up/down buttons as the single-pointer alternative
  (WCAG 2.2, 2.5.7), keyboard as in dnd-kit.
- Reordering only in an explicit order mode: while a column sort is active there is no
  drag and drop; the order mode locks column sorting.

### Grouped and ordered variant (ratified 2026-11-24, built)

- Grouping is a per-instance switch over exactly one field, chosen in the toolbar
  ("Gruppiert nach"). No second level: that would be a tree table.
- Each group is its own `tbody` with a group head (`th scope="rowgroup"`): chevron to
  collapse, name, count, an optional sum aligned under its own column, an optional limit
  ("4 / 5", warning tone once full; the limit replaces the count).
- Groups follow the field's business order (for a status: the process order, never
  alphabetical); column sorting acts within each group.
- Pagination runs over the whole list; a group torn across a page boundary repeats its
  head on the next page as continuation; the count always counts the whole group.
  No paging per group (that would be "load more" through the back door).
- Order mode ("Reihenfolge bearbeiten", off by default): grip column and rank number
  appear; column sorting is locked (headers are plain text, a note says why);
  pagination is off; groups of 25 or more start collapsed. Rank is kept per status, so
  the order mode is offered only when grouped by status, otherwise the button explains
  itself.
- Dragging within a group changes the rank; dragging into another group changes the
  field value, is saved at once and confirmed with "Rückgängig" (a status line until the
  toast exists). A full target refuses: the drop line turns red and names the reason,
  and the refusal stays visible after the drag. Dropping on a group head appends to
  that group.
- Single-pointer alternative (WCAG 2.2, 2.5.7): "Verschieben nach …" in the row menu and
  as a bulk action, with status and position (start, end, after a given row). Keyboard:
  focus the grip, space lifts, arrows move within and across groups (a full group is
  skipped with a message), space drops, escape restores.
- One row is dragged at a time; several rows move through the bulk action.
- Implementation note for the product: a grip that resets styles with `all: unset`
  must set `-webkit-user-drag: element` again, otherwise the browser never starts the
  drag (measured).

Checked 2026-11-24 with axe-core 4.10 (more than 60 runs over the day, all proposals included) (WCAG 2.0/2.1/2.2 A and AA): both themes, edit and
read mode, and every table state and overlay (load, reload, empty, no match, error, sort
popover, settings menu, row menu, bulk bar; grouped, order mode, lifted row, move panel,
refusal, confirmation with undo, grouping by another field): no violations.

### Group list beside the table (ratified approach, layout preview for sprint/218.3)

- In-table grouping works while the collapsed group heads fit one screen (about twelve).
  Fields with many values (workstreams, customers) get the group list beside the table
  instead, like folders in a mailbox: a searchable list of groups with counts on the left,
  the table of the chosen group on the right. The choice is made per grouping field and
  never switches automatically with the current number of values.
- Rows can be dragged onto a target outside the table (a group in the list); the move is
  confirmed with "Rückgängig".
- "Verschieben nach …" gets a searchable target field where the targets are many.
- No drop bar during dragging: below the threshold the group heads are reachable, above
  it the group list takes that role.
- Rejected: implicit collapsing of other groups when an expanded group exceeds the page
  size. The table would change state where the user did not act.
- The page layout itself (list beside table) is ratified in sprint/218.3.

### Iterations as groups (ratified approach)

- Several tables of the old system (one per iteration) become one grouped table; moving an
  item between iterations is a cross-group drag. Group heads are separators with a title,
  never draggable and never reorderable; the order of groups comes from the field.
- The separator is sharpened within the existing group head: a stronger rule before every
  group but the first, the title at section size. "Alle zuklappen / aufklappen" sits in
  the toolbar.

## 13. Card view (proposed)

- A further view over the table core, switched in the toolbar ("Tabelle / Kacheln"): same
  data, filter, sort, selection, pagination and states.
- Fixed card form: head (select, number, status), title clamped to two lines, three facts,
  foot (due date, more actions). Equal row heights; a real grid, never masonry.
- For few, visually strong objects (scopes, documents, process definitions, agents), not
  for thousands of items. Sorting happens through the sort popover (no column heads).

## 14. Tree table (proposed)

- For real hierarchies (milestone, item, sub-item; bracket and children). WAI-ARIA
  treegrid in row-focus mode: one tab stop; up and down move, right expands or enters the
  first child, left collapses or goes to the parent, Home and End, Enter opens.
- No controls inside rows and no dragging; re-parenting goes through "Verschieben nach …".
  Parents show the progress of their leaves (done / total). "Alle aufklappen / zuklappen".

## 15. Status chips (proposed)

- Never interactive; always dot plus text, never colour alone. Five tones: neutral, info,
  warning, success, error. Modifiers: strong (the state being worked in), dashed (draft),
  struck (closed without result).
- Mapping, worklist item: intake neutral "Eingang", open info "Offen", planned info strong
  "Geplant", closed successful success "Erledigt", closed discarded neutral struck
  "Verworfen". Dispatch task: draft neutral dashed "Entwurf", open info "Offen", active
  info strong "Aktiv", on_hold warning "Angehalten", delivered success "Geliefert", closed
  neutral "Geschlossen".
- Distinct from tags (user palette, removable) and counters.

## 16. Toast (proposed)

- One region bottom right (bottom centre on narrow screens), newest at the bottom, at most
  three. For outcomes of an action.
- Without action it leaves after six seconds; with an action ("Rückgängig") it stays until
  closed, so nobody is timed out of the action (WCAG 2.2.1). Hover and focus pause the timer.
- Errors that concern something on the page stay inline at that place; a toast carries
  only background failures (for example an export). The tables use the toast for undo.

## 17. Empty state (proposed)

- One block for tables, lists and pages. Kinds: first use (invitation with the create
  action), no match (reset), all done (success tone, no action), no permission (lock,
  who grants access), load failure (error tone, retry).
- Text: the heading names the space or the result, one sentence says what to do, at most
  one action. Never apologise, never "nothing here". First use and no match are never
  confused.

## 18. Dialog and side panel (proposed)

- Both are native modal dialogs (`showModal`): the page behind is inert, focus stays
  inside, Escape closes, focus returns to the trigger.
- Initial focus: the least harmful action in a confirmation, the first field in a form.
  The backdrop never closes. Never a dialog on a dialog: "discard changes?" appears in the
  footer. An irreversible act of large scope asks to type the name.
- Sizes: S 460 px (confirmation), M 640 px (short form), L 880 px (form with groups).
  Larger dialogs and their layout are a topic of the pattern round.
- Side panel from the right, 480 px: header with type, title and address, scrolling body,
  footer with actions that stays. A permanently docked panel is a page layout (218.3).

## 19. Icon vocabulary (ratified 2026-11-24)

One action, one Lucide icon (stroke 1.75; 16 px in buttons, 14 px in menus); an icon never
takes a second meaning; text and icon always appear together, so where two verbs share an
icon the text says which. A process is never reset: the trace is immutable; recall,
rework and withdraw cover the cases.

| Group | Action: icon |
|---|---|
| General | create `plus`, edit `pencil`, save `check`, delete `trash-2`, undo `undo-2`, reset filter or form `rotate-ccw`, retry `refresh-cw`, close or cancel `x`, open `square-arrow-out-up-right`, open in side panel `panel-right-open`, enlarge `maximize-2`, external link `globe`, copy `copy`, assign `user-plus`, move `folder-input`, search `search`, filter `filter`, sort `arrow-down-up`, settings `settings-2`, more `ellipsis`, drag handle `grip-vertical`, expand `chevron-down`, export `download`, upload `upload`, share `share-2`, comment `message-square`, history `history`, notifications `bell`, sign out `log-out` |
| Task (Dispatch) | send `send`, recall `file-pen-line`, claim `grab`, release `hand-helping`, defer `clock-arrow-up`, renew `timer-reset`, ask `message-circle-question`, answer `message-square-reply`, hold `pause`, resume `play`, deliver `package-check`, rework `wrench`, accept or approve `stamp`, reject `circle-x`, fail `octagon-x`, withdraw `archive-x` |
| Instance and definition | start `play`, conclude `square-check-big`, link `link-2`, unlink `unlink-2`, write version `file-plus-2`, activate version `gavel` |
| Worklist | plan `calendar-plus`, unplan `calendar-minus`, advance `step-forward`, close `archive`, validate `list-checks`, book in the fulfilment ledger `book-check`, submit `arrow-up-from-dot`, discard `circle-slash`, tag `tag` |
| States and views | no permission `lock`, system value `cpu`, help `circle-help`, info `info`, warning `triangle-alert`, error `circle-alert`, success `circle-check`, table view `table-2`, card view `layout-grid` |
| Reserved | switch an agent on or off `power`, reset an agent `skip-back` (never for processes) |

## 20. Density (proposed)

- One switch for the page (comfortable: 36 px row, 34 px controls; compact: 28 px). Every
  component reads the density tokens, fixed heights included; type sizes stay. A table may
  override the page in its settings menu.

## 21. State catalogue and pressed state (proposed)

- The showcase shows every state of every component (rest, hover, focus, pressed,
  selected or on, disabled, no permission, error, loading); a dash means the component
  has no such state.
- New: pressed. Filled buttons darken (brightness 0.88), neutral buttons take the
  selection surface.

## 22. Mobile round (open)

Measured on a phone (2026-11-24): the header eyebrow wraps into four lines; the filter
builder's joined bar is too narrow and must stack per condition; button rows wrap and
belong full width at the bottom. Tag suggestions now open on focus only. To add: tables
and cards on narrow screens, dialogs and the side panel at full width, reflow at 400 %
zoom (WCAG 1.4.10).

## 23. Implementation basis (open)

The showcase is hand-written to show behaviour; it is not the implementation. To decide:
the table core (TanStack Table or React Aria), the tree table (React Aria treegrid or own),
dialogs (native `dialog` or Radix), toasts, drag and drop (dnd-kit, ratified in section 7).

## 24. Open in 218.16

- Ratify sections 8 to 10 and 13 to 18, 20 and 21.
- Expandable row (ratified in section 12, not built).
- Implementation basis (section 23).
- Mobile round (section 22).
- Rebuild the console stylesheet (`unified-console/shared/console.css`) from this design
  system so that the empty shell renders.
- Design System artifact in Claude Design.
- Then: commit by the operator, foreign review (Kimi) against the commit.
