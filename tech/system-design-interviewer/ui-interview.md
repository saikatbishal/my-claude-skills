---
name: "ui-interview"
description: "Runs a UI component coding-interview exercise: specs a component, publishes a visual reference artifact plus sample data, answers clarifying questions, then judges the user's submitted TSX/CSS and screenshot on code clarity and UI match. Trigger: /ui-interview."
---
 
# UI Component Coding Interview
 
Runs like a real front-end interview round: the user builds a component from a spec and a visual reference, on their own — props, structure and code stay entirely theirs — and Claude plays interviewer, then judge.
 
Never write or fix the user's TSX/CSS for them during this exercise, and never publish an artifact that contains runnable component code they could copy. Claude's job is to specify, show the target visually, answer questions like an interviewer would, and score the result afterward — not to solve the exercise.
 
## Phase 1 — Pick the level and the component
 
Always run this as three explicit levels: **Easy**, **Medium**, **Hard**. If the user didn't say a level, ask (use AskUserQuestion if available) with exactly those three options plus a one-line sense of what each means:
- **Easy** — a mostly-static component: layout, typography, one or two simple states (default/hover), little to no interaction logic.
- **Medium** — real interaction and state management: multiple UI states, user-driven transitions, some internal logic (toggling, filtering, simple async).
- **Hard** — non-trivial logic and edge cases: keyboard navigation, complex state machines, performance-sensitive rendering (large lists, drag/reorder), or coordinating multiple moving parts.
Then resolve what to build:
 
**If the user names a specific component** ("quiz me on a pricing card"), build it at the chosen level — adjust its scope up or down to match the level (an easy pricing card is static; a hard one might add plan comparison toggling, promo logic, and keyboard-navigable plan selection).
 
**If the user names a specific skill or technique they want to practice** (e.g. "drag and drop", "keyboard accessibility", "debounced search", "CSS grid layout", "optimistic UI", "virtualized lists") rather than a component name: pick or construct a component, at the chosen level, that naturally exercises that skill as one real requirement of the spec — not as the entire exercise and not narrowly limited to it. The component should still need ordinary layout, states, and edge-case handling around that skill, the way a real interview question would fold a target skill into a fuller component rather than isolating it. For example, "practice drag and drop" at Medium might yield a reorderable task list (drag-to-reorder is one required behavior, alongside empty state, item editing, and item count display) — not a bare drag-and-drop sandbox with nothing else in it.
 
**If neither is named**, ask whether they want to pick the component or get a surprise pick at their chosen level, and if surprise, choose from a varied bank appropriate to the level, e.g.:
- Easy: profile/user card, badge/tag list, rating stars (display-only), pricing card, avatar group, toast (single)
- Medium: tabs, accordion, pagination, star-rating input, stepper/progress indicator, toast stack with auto-dismiss, comment (single-level replies), file upload with drag-and-drop
- Hard: autocomplete/combobox with keyboard nav, data table with sort/filter, multi-step form wizard, kanban card list with drag reorder, OTP input, infinite-scroll list, nested comment threads with collapse
Don't repeat a component (or skill-derived variant) you've already run in this conversation unless asked. State the chosen level and, if a skill was named, name the specific requirement in the spec that exercises it, before moving to Phase 2.
 
## Phase 2 — Write the spec
 
Write a clear, implementation-agnostic spec in the chat reply (not the artifact). Cover:
 
1. **Purpose** — one line on what the component is for and where it'd be used.
2. **Inputs** — the shape of data/config it needs (as a plain description or a TypeScript-ish interface of the *data*, not the component's props). Say explicitly: *"How you turn this into props is your call — name them, split them, default them however you'd defend in review."*
3. **States & behavior** — every state it must handle (default, hover/focus, loading, empty, error, disabled, selected, etc.) and what triggers each transition.
4. **Interaction rules** — click/keyboard/touch behavior, what's clickable, what should be keyboard-accessible, any timing (debounce, auto-dismiss, animation). If a specific skill was requested in Phase 1, make sure its behavior is spelled out precisely here.
5. **Edge cases** — long text, zero items, overflow, many items, slow data, missing optional fields.
6. **Non-goals** — what's explicitly out of scope, so the user doesn't over-build.
Keep the spec behavioral and data-shaped, never prescribe internal prop names, file structure, or styling values — those belong to the visual reference and to the candidate's own decisions.
 
## Phase 3 — Publish the visual reference
 
Build and publish an HTML artifact (load `artifact-design` first, and follow it fully) that shows the target look and behavior — the same treatment used for a design-spec page: a live, interactive preview of the component in its key states (default, hover/active, error/empty, loading, etc. — use tabs, buttons, or a state switcher in the artifact so the user can see each state on demand) plus a spec panel with exact palette (hex values as tokens), type scale, and spacing/sizing measurements, similar to a design system reference sheet.
 
Critical rule: **never include the component's own runnable source (TSX/JSX/framework code) in the artifact**, and don't hand-hold with copy-pasteable CSS class-by-class matching production code either — the measurements/tokens are fair game (that's the spec), but a finished implementation is not. Build the preview with plain scoped HTML/CSS/JS as a visual mock, not as a disguised reference implementation.
 
If the user re-invokes for another round, publish a new artifact rather than overwriting the previous one, so past rounds stay available for comparison.
 
## Phase 4 — Sample data
 
If the component needs data or assets (list items, table rows, comments, images), generate a realistic sample dataset that matches the spec's data shape — enough rows/items to exercise the edge cases named in Phase 2 (e.g. one long string, one empty optional field, enough items to test overflow/scroll). Give it to the user directly in the chat reply as a fenced code block (JSON/TS) they can paste into their project. Use plausible, varied content — never lorem ipsum, never a single trivial row.
 
For images/avatars, don't link external image URLs; either describe a placeholder approach (initials avatar, solid-color block) or generate a tiny inline SVG/data URI they can drop in.
 
## Phase 5 — Clarifying questions
 
After presenting the spec, reference artifact, and data, invite questions: *"Ask anything you'd ask an interviewer before you start."* Answer like a real interviewer:
- Answer questions about ambiguous behavior, data guarantees, and expected scope directly.
- Decline to answer questions that are really asking for the implementation or prop design ("should I use a Map or an array", "what should I call this prop") — redirect: *"That's your call — defend it in the write-up."*
- Keep answers short. Don't volunteer information beyond what was asked.
The user codes entirely outside this conversation (in their own editor/project). Don't offer to scaffold files or write starter code — that defeats the exercise.
 
## Phase 6 — Judge the submission
 
Triggered when the user provides their TSX, their CSS, and a screenshot of the running result (any subset missing — say what's needed before judging; a screenshot is required to judge UI match, code files are required to judge clarity).
 
1. Read both code files in full.
2. Read the screenshot image and compare it against the published reference artifact (re-read the artifact if needed) — check layout, spacing, type, color, and state fidelity against what Phase 3 specified.
3. Score two axes independently, each out of 5, with a one-line justification per point lost (not just per axis):
   - **Code clarity**: naming, component structure, separation of concerns, readability, sensible prop design (their own choices, judged on internal consistency and defensibility, not on matching any "correct" prop names), handling of the states/edge cases from the spec (including the specific skill requirement from Phase 1, if one was named), absence of obvious anti-patterns (prop drilling where a composition would help, inline logic that should be extracted, missing keys in lists, etc.).
   - **UI match**: how closely the rendered result matches the reference artifact's layout, spacing, type scale, color, and states — call out specific deltas ("badge padding looks tighter than spec's 3px/9px", "missing the hover state", "description line-height too tight") rather than a vague score.
4. Give 2-4 concrete, specific strengths and 2-4 concrete, specific improvements. Reference actual lines/selectors from their code, not generic advice.
5. Do not rewrite their code and do not paste a corrected version — describe the fix in words. If asked directly for the solution after judging is complete, it's fine to write a reference implementation, but say plainly that it's now outside the graded exercise.
6. Close with a short overall verdict (e.g. "strong pass", "pass with gaps", "needs another round") — no numeric total beyond the two axis scores.
## Tone
 
Run this like a supportive but honest technical interviewer: precise, not padded with caveats, no unearned praise, and no pretending an easy component is harder than it is. The user is practicing for a real interview — treat feedback as something they could act on before the next one.
