---
name: feature-spec
description: >-
  Use this skill to help a developer author a new feature specification document
  for an application (any platform, any language), AND to keep an existing spec
  in sync whenever the feature or screen it documents changes. Trigger on the
  obvious phrasing — "write a
  feature spec", "create a spec doc", "document a feature", "scaffold a spec" —
  but also trigger any time a developer describes changing, fixing, extending,
  or redesigning a feature or screen, even if they never mention documentation
  at all: "I added a new parameter to the top-up endpoint", "redesigned the
  login error state", "fixed a bug in the card screen". In those cases, ask
  whether the existing spec (and any of its screenshots) should be updated to
  match — don't wait for the developer to bring docs up. For new specs, the
  skill scans docs/templates, asks which template fits, asks where the spec
  should live, then walks the developer through the chosen template one heading
  at a time. For updates, it reads the existing spec, asks which sections and
  screenshots the change affects, and updates them in place. Both paths
  cross-link anything that already exists in the docs instead of duplicating
  it, and keep the navigation index wired up.
version: 1.4.0
---

# Feature Spec Author

This skill turns the templates in `docs/templates/` into a finished feature
specification under `docs/feature-specs/`, by interviewing the developer one
section at a time — and it also keeps existing specs honest by asking to
update them whenever the feature they describe changes. The goal is a spec
that reads like the existing ones in the repo — same structure, same
conventions, no leftover template boilerplate, and no drift from the code it
describes. While a spec is being written, two temporary files sit beside it:
`plan.md` holds the concluded requirements and progress checklist, and
`context.md` holds the ongoing discussion and where you left off. Both are
deleted together once the spec is complete, leaving the spec as the single
source of truth.

The most important idea: **the templates are scaffolding, not output.** Each
template heading carries *guidance prose* ("A short paragraph explaining what
this feature is and why it exists…"). That prose is for you — it tells you what
to ask the developer. The finished spec replaces it with the developer's real
answers. Never leave the instructional text in the final document.

## The conventions to honour

Before you start, internalise how specs are laid out in this repo so what you
produce matches:

- A spec is a **folder** named after the feature (kebab-case), containing an
  `index.md`. e.g. `docs/feature-specs/auth/login/index.md`.
- Folders nest by section. There is a `common/` section today; features may live
  at the top level or inside a section like `auth/`. The developer chooses.
- Each section has its own `index.md` that lists links to its children. When you
  add a new spec, add a link to it from its parent's `index.md` so navigation
  stays consistent.
- Screenshots live in an `assets/` folder beside the `index.md`, referenced with
  relative paths like `![…](assets/screen-name.png)`.
- A spec folder may temporarily contain a `plan.md` — the concluded
  requirements and progress checklist for the feature (see Step 3) — and a
  `context.md` — the running discussion and memory for the spec (see Step 3).
  Both are scaffolding, not documentation: neither is linked from an index, and
  both are deleted together when the spec is complete.
- Shared code (services, widgets, utilities, data models) is documented **once**
  under `common/` and linked to from feature specs — never copy-pasted. The
  feature templates have a "Common" section that does exactly this.

## Which mode applies?

This skill covers two situations, and it's worth pausing to notice which one
you're in before doing anything else:

1. **Creating a new spec** — the feature has no documentation yet and the
   developer wants to write one from scratch. Handled by *Workflow: creating a
   new spec* below.
2. **Updating an existing spec because the feature changed** — the developer
   is fixing a bug, adding a parameter, redesigning a screen, or otherwise
   changing behaviour that's already documented. Handled by *Workflow:
   updating an existing spec* below.

The second case is easy to miss because the developer is usually focused on
the code, not the docs — that's exactly why it's worth catching. Specs go
stale when nobody thinks to update them mid-change, so whenever a request
describes changing, fixing, extending, or redesigning a feature or screen,
ask whether its spec should be updated too — even if documentation was never
mentioned. If the developer says no, or the feature has no spec yet, just
help with the code as normal (or, if they'd rather write one now, that's
Create mode).

## Resuming from `context.md`

Whenever you start work on a spec — create or update — check its folder for a
`context.md` **first**. If one exists, the work is already underway (possibly
in a session whose conversation was cleared): read `context.md`, then
`plan.md`, then the spec, and pick up at the "Where we left off" entry. Briefly
tell the developer where you're resuming from and confirm before continuing,
rather than restarting the interview or re-asking settled questions. If the
developer points you at a spec folder (or the spec path is otherwise known)
and the conversation has no history of it, look for `context.md` before asking
anything.

## Workflow: creating a new spec

Work through these steps in order. Steps 3–6 are the heart of it.

### Step 1 — Choose the template

List the templates by scanning `docs/templates/` (each subfolder is one template,
defined by its `index.md`). Present what you find to the developer with a
one-line summary of each, and ask which one fits. Today there are two:

- **feature** — a feature with no dedicated UI (shared logic, a service layer, a
  background capability).
- **feature-screen** — a feature the user sees and interacts with on one or more
  screens. Choose this whenever the work involves any screen, dialog, or visible
  flow.

If the developer describes something with screens or user interaction, recommend
**feature-screen** and confirm.

**Where the templates come from.** Prefer the repo's `docs/templates/` so the
spec tracks whatever the team has evolved there. But this skill also bundles a
copy of both templates under `assets/templates/` — use them as a fallback if
`docs/templates/` is missing or empty, and as worked examples to show the
developer what a good section looks like. The bundled copies are the safety net
that keeps the skill working when the repo has nothing to start from.

Read the chosen template file fully — it is the source of truth for the headings
and the guidance under each one. Don't rely on the inline examples in this skill;
templates may change.

### Step 2 — Name the feature and choose where it lives

Ask for the feature name, then derive the folder name as kebab-case (e.g.
"Card Top-Up" → `card-top-up`).

Then ask **where** the spec should live. Scan the existing folders under
`docs/feature-specs/` and offer them as choices, plus "a new section folder" and
"top level". For example: top level, inside `common/`, or inside an existing
section like `auth/`. The final path is
`docs/feature-specs/<section?>/<feature-folder>/index.md`.

If the developer picks a new section that doesn't exist yet, you'll create that
folder and give it its own `index.md` in Step 7.

Confirm the full path before writing anything.

### Step 3 — Capture requirements in `plan.md`

Create `plan.md` in the spec's folder (`<spec-folder>/plan.md`, beside where
`index.md` will go) **before the interview begins**. It holds every
requirement for the feature and is the context that drives the rest of the
workflow.

1. Ask the developer for the feature's requirements: goals, behaviours,
   constraints, open design decisions, and any source files that need to be
   created or changed. Accept rough notes and shape them into items.
2. Write them as `- [ ]` checkbox items, grouped under short bold area
   headings that fit the feature (e.g. **Payload & delivery**, **Screens**,
   **Routing**). Open the file with a blockquote noting that it is temporary —
   a checklist for completing the feature, to be deleted once the spec is
   done.
3. End with a **Documentation** group: replace placeholder wireframes with
   real screenshots, resolve the spec's remaining `TODO`s, delete `plan.md`.

`plan.md` and the spec inform each other:

- If the spec already has content (a draft in progress, or an existing spec),
  read it and use it to propose or refine requirements — a screen described in
  the spec implies items for its constants, data types, and screenshots.
- Requirements gathering can be revisited at any point. As the spec grows,
  add or refine `plan.md` items from what the spec now says.
- Items may link to the spec heading they relate to (derive the anchor from
  the real heading, per Step 5) so progress maps to a section.

If the feature is already fully implemented and there is nothing left to
track, skip `plan.md`.

**Also create `context.md`** beside it, at the same moment. Where `plan.md` is
the *concluded* record (decisions already made, work to tick off),
`context.md` is the *ongoing* one: the memory for this spec, so that if the
conversation is cleared, reading it gets you back to where you left off. Open
it with a blockquote noting it is temporary working memory, deleted with
`plan.md` once the spec is complete. Keep it short and current — rewrite
entries rather than appending a transcript. Use these sections:

- **Where we left off** — the single next action: which heading or plan item
  is in progress, and the question currently open with the developer. Always
  overwrite this; it is the first thing read on resume.
- **Spec setup** — template chosen, feature name, folder path, any new section
  folder or assets filenames agreed.
- **Decisions in flight** — points discussed but not yet concluded (options
  weighed, leanings, things the developer wants to think about). Once settled,
  move the outcome into `plan.md` (or the spec) and delete the entry here.
- **Open questions** — things to ask the developer later, including sections
  they said "skip" to.
- **Notes** — anything else a fresh session would need: source files already
  inspected or created, common docs found for cross-linking, constraints the
  developer mentioned in passing.

Nothing is permanent in `context.md`: concluded items graduate to `plan.md` or
the spec, and the file shrinks as the spec nears completion.

### Step 4 — Fill in the spec, one heading at a time

Read `context.md` and `plan.md` first and use them as context: propose
answers for each heading from the requirements rather than asking from a blank
slate, and confirm them with the developer.

Create the spec file on disk **before the interview begins** — write the title
and a skeleton of the top-level headings so the file exists from the start.
After each section is answered, **immediately update the file** with the
developer's answer. Don't batch writes until the end; the developer should be
able to open the file at any point and see exactly how far the spec has
progressed. Keep `plan.md` in step: tick (`[x]`) the items each answer
completes, and add newly discovered work and any `TODO` you leave in the spec
as unchecked items. Keep `context.md` in step too: after each exchange, update
**Where we left off** (and move any concluded decision out of **Decisions in
flight** into `plan.md`/the spec) so a cleared conversation can resume from
the file alone.

For **each** heading in order:

1. Show the developer the heading and the template's guidance for it (paraphrase
   if that's clearer), so they know exactly what belongs there.
2. Ask what to put. Accept prose, bullet points, code — whatever the section
   calls for. Let them say "skip" (leave a short `TODO` marker) or "not
   applicable" (omit the section) so the interview never stalls.
3. Write their answer into the file in place of the guidance prose, then
   continue to the next heading.

Keep the developer oriented — tell them roughly how many sections remain so the
interview feels bounded rather than endless.

**Repeating sections.** The `feature-screen` template's "Screens" section repeats
per screen (Screen Description, API Interaction, Screen Arguments, Data /
Networking, Constants, …). Ask how many screens there are and their names, then
walk each screen through that block. Same for multiple API endpoints within a
screen, or multiple shared widgets. Duplicate the sub-structure once per item.

**Always ask about per-screen Constants and Data classes — even with no API.**
A screen can carry screen-specific constants (argument/parameter keys, route
codes) and data/state types regardless of whether it calls an endpoint. A
screen driven entirely by a shared mechanism (e.g. listening to an emitted
result rather than calling REST) still often defines a state type, an args
type, or a constants group. So for every screen, explicitly ask "any constants
for this screen?" and "any data or state types for this screen?" rather than
skipping them just because API Interaction was N/A. Link shared types to their
common doc; document genuinely screen-specific ones inline, or mark `TODO` if
not yet defined.

**API Interaction — check existence before documenting.** When filling in an API
Interaction sub-section, ask the developer: "Does this endpoint already exist in
the codebase?" Then act on the answer:
- **Exists** — search the codebase for the API client interface and its
  implementation, using whatever networking pattern the project already uses.
  Read the actual method signatures and document them faithfully. Ask whether
  the existing endpoint needs to be *updated* (new parameters, changed response
  shape) and, if so, offer to make that change to the source file now.
- **New** — ask whether to create the API client interface and its
  implementation now. If yes, scaffold them in the correct location (mirror the
  conventions of neighbouring API service files) and document the generated
  interface in the spec. Mark the implementation as not-yet-complete where
  details are still unknown.

**Code snippets — offer to create the source files.** When a section calls for
an interface, data type, constants group, or repository, ask: "Do you want me
to create this file now?" Creating the file first has two benefits: the
developer can review the real signature immediately, and copying it into the spec
is then just a paste rather than writing from scratch. If the developer says yes,
create the file in the correct location (infer the convention from neighbouring
files), then read it back and paste the interface or type into the spec. If the
code already exists, read the file and paste it directly — never transcribe from
memory. Mark anything still undecided as a `TODO` comment.

**Documenting code in specs — doc comments in source, full block in spec.**
When a spec references a class, interface, or enum from the code, the
field/property descriptions belong directly in the source file, using whatever
in-code documentation convention the project's language uses (KDoc for Kotlin,
Javadoc for Java, docstrings for Python, JSDoc for JavaScript/TypeScript, XML
doc comments for C#, etc.) — not as a separate markdown table in the spec. The
spec then copies the **full, documented code block** (including those doc
comments) verbatim into a fenced code block tagged with the source language.
Two rules follow from this:

- **Never link to a source file path** in place of the code block. Paths change
  when files move; the spec would silently point nowhere. The code block is
  self-contained and readable without navigating to source.
- **Keep the spec block in sync with the source.** When the source file changes
  (new field, renamed property, updated doc comment), update the spec's code
  block at the same time. The spec is a snapshot of the documented code, not a
  live reference.

### Step 5 — Cross-link instead of duplicating

This is what keeps the docs maintainable. Whenever the developer mentions
something that **already has a doc** — a common service, a shared widget, a
utility, a data model, or even another feature's screen — link to that existing
section rather than writing the content again.

How to do it:

1. Search the existing docs for the referenced thing. Start with
   `docs/feature-specs/common/` (grep the `index.md` files and headings), then the
   rest of `docs/feature-specs/`.
2. If you find it, insert a Markdown link to that heading using a **relative path**
   from the new spec's location, plus the heading's anchor.
3. **Derive the anchor from the real heading, don't guess it.** Read the target
   file and look at the exact heading line, because anchors are fussier than they
   look — these docs use emoji headings like `#### 🦾 Common Utility / Service
   Classes`, and GitHub's generated anchor for that is *not* a clean slug. GitHub
   builds the anchor by lower-casing, dropping the emoji and other non-alphanumeric
   characters, and replacing runs of spaces with single hyphens — which for that
   heading yields a leading hyphen (`#-common-utility--service-classes`-style),
   not `#common-utility-service-classes`. Rather than risk a broken link, prefer a
   clean text heading nearby (e.g. `#### NFC Service Provider` →
   `#nfc-service-provider`), or link to the file root with no anchor. When in
   doubt, open the file and confirm the exact heading text before forming the
   anchor.
4. Compute the relative path correctly from the new file's folder. For a spec at
   `docs/feature-specs/auth/login/index.md` linking to a common doc at
   `docs/feature-specs/common/index.md`, the link is
   `../../common/index.md#exampleservice`. Double-check the number of `../` hops
   against the actual depth.
5. If it doesn't exist anywhere yet, ask the developer two things:
   - Is this **shared** (used by multiple features, belongs in `common/`) or
     **local** (specific to this feature only)?
   - If shared: offer to create a stub doc for it right now. A stub is a new
     folder under the appropriate `common/` sub-section (e.g.
     `common/data/remote/multi-factor-authentication-api/`) with a minimal
     `index.md` — title, a one-line description, and a `TODO:` body. Wire it
     into its parent `index.md` immediately, then link to it from the new spec.
     Don't wait until the end — create it as soon as the developer confirms.
   - If local: document it inline in the spec under the relevant screen or
     section.

The feature templates already model this in their "Common" section — follow that
pattern for any reference, not just the Common list.

### Step 6 — Assets folder (screen specs)

For a `feature-screen` spec, create an empty `assets/` folder beside the
`index.md` and keep the template's image references as **placeholders** (e.g.
`![Login screen](assets/login.png)`), named after each screen/state the developer
described.

**Announce the filenames up front, before writing them in.** Don't silently
invent names — once the screens and states are known, tell the developer the
exact filenames you intend to use (e.g. `top-up-screen.png`,
`top-up-screen-loading.png`, `top-up-screen-error.png`) and ask whether that
matches any naming convention they already follow. This way the placeholders line
up with the files they'll actually drop in, instead of forcing a rename later.
Default to kebab-case `screen-name.png` and `screen-name-<state>.png`, but defer
to whatever convention they name. For a non-screen `feature` spec, there are no
assets.

**Screenshots vs wireframes.** When covering the User Interaction & Design
section for each screen, ask the developer: "Do you have existing screenshots for
this screen?" If yes, note the filenames they provide and reference them in the
spec. If no, generate a simple ASCII wireframe in a fenced code block inside the
spec as a visual placeholder — enough to convey the layout (title, key UI
elements, button positions) without being a finished design. Label it clearly as
a placeholder wireframe so it is easy to swap out when real screenshots are
available. The image reference (`![…](assets/…)`) should still be included below
the wireframe so the developer knows where to drop the real screenshot.

### Step 7 — Wire it into navigation

After writing the spec:

1. Add a link to the new `index.md` from its **parent** `index.md` (the section
   it lives in, or `docs/feature-specs/index.md` if it's top level), matching the
   existing list style in that file.
2. If you created a new section folder in Step 2, give that folder its own
   `index.md` (a title plus the link to this spec) and link the section from its
   parent in turn.

### Step 8 — Validate all local links

Before declaring the spec done, audit every relative Markdown link in **all
files you created or modified** during this session (the new spec's `index.md`,
any stub common docs, any parent `index.md` files you updated).

**How to check each link:**

1. Extract every `[text](path)` where the path does not start with `http` or `#`.
2. Strip the anchor fragment (`#…`) from the path — anchors can't be verified
   from the filesystem, so only verify the file/folder exists.
3. Resolve the path relative to the file that contains it using Python:
   ```bash
   python3 -c "import os; print(os.path.realpath('path/to/link'))"
   ```
   Run this from the directory of the containing file (not the repo root).
   Avoid `realpath --no-symlinks` — that flag is Linux-only and silently fails on macOS.
4. Check whether the resolved path exists on disk (`[ -e "$RESOLVED" ]`).

**For each broken link**, show the developer:
- The file that contains the link
- The broken link text and path
- Your best guess at the correct path (recount `../` hops from the file's
  actual depth, check whether the target folder exists, suggest the fix)

Then ask: "Should I fix this to `<suggested path>`, or mark it `TODO`?"
Apply their answer and move on to the next broken link. Do not silently drop or
leave broken links — every link in a freshly created spec should either resolve
or be an explicit `TODO`.

**Relative path depth — common source of errors.** Count the `../` levels
carefully from the *file's directory*, not the repo root.

Example: a spec at `docs/feature-specs/main/auth/login/index.md` is 5 levels
deep inside `docs/feature-specs/`. To reach `docs/feature-specs/common/`:
- `../` → `docs/feature-specs/main/auth/`
- `../../` → `docs/feature-specs/main/`
- `../../../` → `docs/feature-specs/` ← correct stopping point for `common/`

So the link should be `../../../common/…`, not `../../../../common/…`. One extra
`../` overshoots into `docs/` and every link silently breaks.

`plan.md` and `context.md` are scaffolding and aren't linked from anywhere, so
they need no link audit.

After all links resolve (or are explicitly `TODO`), show the developer the tree
of what you created/changed and the path to the finished spec.

### Step 9 — Finish and clean up `plan.md` and `context.md`

Once every box in `plan.md` is ticked and the spec's `TODO`s are resolved,
offer to delete `plan.md` **and `context.md` together** — they live and die as
a pair. The completed spec is then the new source of truth. If items remain
open, leave both files in place, make sure `context.md`'s **Where we left off**
is current, and tell the developer what is outstanding — they keep tracking
the work until the feature is done.

## Workflow: updating an existing spec

This is the path for a change to a feature or screen that's *already*
documented. Unlike creating a new spec, don't walk the whole template start to
finish — only touch what the change actually affects. Re-touching unrelated
sections just to be thorough risks introducing drift of its own.

### Step U1 — Confirm and locate

Ask the developer whether this feature has an existing spec and, if so, where
it lives — they usually know the path faster than a search would find it. If
there isn't one yet, this isn't an update; offer Create mode instead (Step 1
above), or just leave it if they'd rather not document it right now.

Read the full spec before changing anything, so you're editing in place
against its real current structure rather than guessing. Also check for a
`context.md` and `plan.md` beside it and read them as context for the change
(see *Resuming from `context.md`*); if there isn't a `plan.md` and the change
is substantial, offer to create both files (Create mode's Step 3).

**Migrate legacy requirements sections (one-time, every update run).** Older
specs carried their punch list inside the spec as a `## Requirements
(temporary)` section. Before going further, scan **every** `index.md` under
`docs/feature-specs/` (not just this spec) for that heading, or for a
`## Requirements` section that is a `- [ ]` checklist with a "temporary"
warning blockquote. For each one found:

1. Tell the developer which specs have one, and confirm before touching them.
2. Move the checklist items into a `plan.md` beside that spec's `index.md`,
   in the format from Create mode's Step 3 (temporary-file note, `- [ ]`
   items grouped under bold area headings, closing Documentation group). If a
   `plan.md` already exists, merge in only the items it doesn't have.
   Preserve ticked (`[x]`) state.
3. Remove the section from the spec and its entry from the Table of Contents.
4. If every item was already ticked, offer to skip `plan.md` and just delete
   the section.

A plain `## Requirements` section that is permanent prose (not a temporary
checklist) is not legacy scaffolding — leave it and ask the developer if
unsure. If nothing is found, say nothing and carry on.

### Step U2 — Inspect what's about to be committed

Before asking "what changed?", look at the actual pending change. Run `git
status`, `git diff` (unstaged), and `git diff --cached` (staged) so you see
everything the developer is about to commit, not just what's staged — a
change is often still unstaged mid-session. This surfaces things a developer
won't always think to mention out loud: a renamed field, a new error state, a
parameter added to an existing endpoint.

- Scope your reading to files plausibly related to this spec: source under
  the feature's package, screens/layouts, API service interfaces and DTOs,
  data/state classes — cross-reference the changed paths against classes,
  endpoints, and screens the spec already documents.
- Summarize the relevant hunks back to the developer in plain language (e.g.
  "This diff adds a `retryCount` field to `SubmitDataParametersRequest` and
  changes `SubmitPresenter` to show a new error state") rather than pasting
  raw diff output.
- Use that summary to drive the rest of the interview: it tells you which
  headings in Step U4 are likely affected, so you can propose them directly
  instead of making the developer enumerate every change from memory, and it
  flags which screens in Step U3's screenshot pass might need a new or
  updated image (a new error state suggests a new screenshot; a rename alone
  usually doesn't).
- Still confirm with the developer and fill gaps — the diff shows *what*
  changed in code, not *why*, and not whether a change is even user-visible.
  Treat it as a lead for the interview, not a replacement for it.
- If there's no pending diff (already committed, or the developer is
  describing work not yet started), skip this step and just ask directly.

### Step U3 — Screenshots

Ask: "Do any screenshots need to be updated for this change?" If no, skip to
Step U4.

If yes, don't ask them to recall filenames from memory — **list what's
already there first.** Scan the spec's `assets/` folder and the
`![…](assets/…)` references in the spec itself, and show the developer every
current screenshot with the filename and which screen/state it belongs to.
Picking off a concrete list is faster and less error-prone than the developer
trying to remember exact names, and it doubles as a sanity check that the
spec's existing references still match what's on disk.

Have the developer select which of those listed screenshots need updating —
they may pick more than one. Separately, ask whether the change also needs a
**new** screenshot for a state that doesn't exist yet (a new error case, an
added button). Then handle each one:

- **Selected from the list (replacement)** — the existing filename is already
  correct and stays as-is. Ask the developer to paste the new screenshot, then
  overwrite that file in `assets/`. No markdown change is needed since the
  existing reference already points at the right filename — just confirm the
  file itself was overwritten.
- **New (addition)** — derive a filename following the convention from Step 6
  of Create mode (kebab-case `screen-name-<state>.png`) and tell the developer
  the exact filename you intend to use *before* asking for the image, so they
  can sanity-check it against their own naming instincts. Ask them to paste
  the screenshot, save it into `assets/` under that filename, then insert a
  new `![…](assets/…)` reference into the spec next to the existing
  screenshots for that screen.

### Step U4 — Prose sections

Ask the developer what changed — using the diff summary from Step U2 to
propose likely-affected headings rather than starting from a blank slate —
then match that against the spec's existing headings (re-read them if the
file is large or it's been a while). For each affected heading:

- Show the current text for that section.
- Ask what should change.
- Update it in place, applying the same rules Create mode's Step 4 uses for
  this content — check whether an API already exists before documenting it,
  offer to create source files for new code, and keep doc comments in the
  source file (using the project's normal in-code documentation convention)
  with the full documented code block copied into the spec rather than a
  markdown field table.

**Maintain `plan.md`.** If a `plan.md` sits beside the spec (see Create mode's
Step 3), treat it as the live punch list for the change you're documenting:
tick off (`[x]`) items this change completes, and add newly discovered work as
unchecked items. It can also draw on the spec — use what the spec now says to
refine or add requirements. If a `context.md` exists, keep it current as well
(**Where we left off**, **Decisions in flight**). When the last box is ticked,
offer to delete `plan.md` and `context.md` together; the updated spec is then
the source of truth.

### Step U5 — Cross-links and validation

If the update introduces a new reference to a common service, widget, or
another feature's screen, follow Step 5's cross-linking rules (search
`common/` first, link rather than duplicate, offer a stub doc if nothing
exists yet). Before calling the update done, validate every link you added or
changed using Step 8's link-checking process — the same rules about relative
path depth and confirming anchors against real heading text apply here too.

Finish by showing the developer a summary of what changed: which sections,
which screenshots, and the path to the spec.

## Style reminders

- Match the voice of the existing specs: clear, present-tense, written so a QA
  tester can follow the user-facing parts without knowing the code.
- Preserve the template's heading hierarchy and Table of Contents — update the
  ToC links if headings changed.
- Don't invent detail. If the developer doesn't know something yet, a short
  `TODO:` is better than a plausible-sounding fabrication.
- For any type documented in the spec, descriptions go in the source file's
  normal in-code documentation format (KDoc, Javadoc, docstrings, JSDoc, etc. —
  whatever the project's language uses); the spec copies the full documented
  code block. Never use a markdown field table or a path link as a substitute
  for the code block.
- Never leave a requirements checklist inside the spec. `plan.md` is the only
  place for one, and it is deleted when the work is complete. Likewise, running
  discussion and "where we left off" notes belong in `context.md`, never in
  the spec; it is deleted together with `plan.md`.
- Keep the interview conversational. The developer is the source of truth; you're
  the scribe who knows the house style and the existing docs.
