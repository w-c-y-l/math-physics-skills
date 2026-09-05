---
name: math-physics-complete
description: "Use for mathematics, theoretical physics, engineering physics, control theory, mechanics, electromagnetism, quantum/statistical/continuum physics, formula-heavy literature reading, derivations, proofs, dimensional analysis, and any answer where rigorous formulas, symbol definitions, assumptions, units, coordinate systems, boundary/initial conditions, or step-by-step physical reasoning are required. Also use for producing Overleaf-ready LaTeX source and writing derivations directly into Overleaf."
category: physics
---

# Math Physics Complete

Use this skill to make formula-heavy answers complete, explicit, and checkable. The default
delivery is: derive, then route the output to the user's chosen destination — command line or
Overleaf (see "Delivery Workflow" below).

## Formula Rendering Rules

- Write all mathematical symbols and expressions using renderable LaTeX math delimiters.
- Use `\(...\)` for short inline mathematical symbols only when needed inside prose.
- Use display math for all nontrivial formulas:

  \[
  \text{formula here}
  \]

- Do not use dollar-delimited math such as `$x$`, `$\hat{y}$`, or `$$...$$`.
- Do not put formulas in fenced code blocks unless the user explicitly asks for raw LaTeX source
  (see "Delivery Workflow" below for destination routing).
- Do not leave formulas as plain text such as `x_dot = Ax + Bu` when a mathematical form is intended. Write:

  \[
  \dot{x}=Ax+Bu
  \]

- Preserve all indices, superscripts, subscripts, hats, bars, tildes, primes, transpose marks, tensor indices, summation bounds, integration limits, domains, and boundary or initial conditions.
- Prefer `\begin{aligned}...\end{aligned}`, `\begin{cases}...\end{cases}`, matrices, and named operators when they improve readability.

## Required Answer Contract

For math, theoretical physics, engineering physics, control, mechanics, and formula-heavy paper explanations:

1. State the problem in words before manipulating formulas.
2. Declare coordinate system, basis, sign convention, unit system, and domain when relevant.
3. State assumptions and applicability conditions before the derivation.
4. Define every symbol when it first appears, including indices, constants, operators, functions, fields, and parameters.
5. Keep formulas complete. Do not omit bounds, limits, domains, initial conditions, boundary conditions, or normalization conditions.
6. Show intermediate steps for derivations unless the user asks for a short answer.
7. Separate long derivations into numbered sections and continue stepwise. Do not collapse steps with "after simplification" unless the skipped algebra is trivial and unrelated to the user's confusion.
8. Check the final result: dimensions/units, sign conventions, limiting cases, and consistency with assumptions.
9. Identify unresolved ambiguities, missing definitions, or necessary context instead of inventing them silently.

## Symbol Table

For substantial answers, include a compact symbol table before or after the main derivation.

Minimum columns:

- Symbol
- Meaning
- Unit or dimension
- Notes or assumptions

If many symbols appear, group them by category: coordinates, fields, parameters, operators, indices, and boundary data.

## Literature Formula Explanations

When explaining formulas from a paper:

- Quote or reproduce the formula with the original equation number if available.
- Use this format:

  **Original equation (n-m)**

  \[
  \text{complete formula}
  \]

  **Meaning:** define every symbol.

  **Role in the paper:** explain why the equation is introduced and how it connects to previous and following equations.

  **Checks:** units, signs, assumptions, and special cases.

- If a paper uses inconsistent notation, state the conflict clearly and choose a local convention for the explanation.
- Do not renumber paper equations unless creating a separate derivation; if adding local equations, label them as "local equation".

## Derivation Structure

Use this default structure for long derivations:

1. Setup and assumptions
2. Coordinate system and units
3. Symbol definitions
4. Governing equations
5. Step-by-step derivation
6. Boundary or initial conditions
7. Final result
8. Checks and interpretation

For very long derivations, stop at a natural checkpoint and say what the next section will continue with. Do not compress the remainder into an omitted statement.

## Quality Checks

Before finalizing, verify:

- Every formula delimiter is renderable LaTeX: `\(...\)` or `\[...\]`.
- No dollar-delimited math remains.
- Every major symbol has been defined.
- Units or dimensions are stated for physical quantities.
- Boundary and initial conditions are present when solving differential equations.
- Summation and integration ranges are explicit.
- The final answer has a dimensional or limiting-case check when applicable.
- Keep limiting-case checks compact: name the limit and the resulting known form (e.g.
  "\(Q\to0\) → Schwarzschild–AdS"); do NOT restate the full formula in every case.

## Delivery Workflow (command-line / Overleaf)

The default delivery: derive, then route the output to the user's chosen destination — command
line or Overleaf. Do NOT re-print the full derivation to chat when the destination is Overleaf.

### 门禁总则 — 决策门逐个必问（先于所有 Step，优先级最高）

本工作流里有若干"决策门"：内容计划书（Step 0）、目的地（Step 1）、新写/改（Step 3）、
选档（Step 4）。规则：

- **每个门 = 一次独立的 AskUserQuestion**，逐个抛、用户答完一个再抛下一个。禁止把多个门
  合并成一次提问、禁止漏抛其中任何一个。
- **禁止以任何理由跳过或简化某门**：题目简单、是短问答、会话已选过、主模型已是最强档、
  内容"看起来不言自明"，全都不是省略某个门的理由。跳过与否只能由用户在选项里决定。
- **禁止替用户默认或沿用**：上一轮选的档位、上一轮的目的地都不能自动沿用到本轮；每个门
  每次都重新问。
- 顺序固定，不要跳步：内容计划 → 目的地 →（Overleaf 才走连接）→ 新写/改 → 选档 → 撰写/修改
  → 交付。

### Step 0 — Content plan (ask first)

Ask whether the user wants a 内容计划书. If yes, enter the one-question-one-answer mode below; if
no, skip.

### Content plan — one-question-one-answer mode

Ask key questions ONE AT A TIME (AskUserQuestion), coarse not fine: topic/goal; language
(Chinese→ctexart / English→article); metric signature & units; main assumptions; which sections
of the Derivation Structure to include; which checks to keep. Stop when the user says enough.

### Step 1 — Output destination (ask)

Ask where to put the formula (AskUserQuestion):

- **仅命令行** → produce the content inline or via `deriver`, then render it in chat with
  `\(...\)` / `\[...\]` per the Formula Rendering Rules. No `.tex` wrapper, no Overleaf, no
  connection check, no Steps 2/3/5–7. The derivation-model tier is still asked in Step 4 for EVERY
  derivation — there is NO short-answer / light-question exemption.
- **仅 Overleaf** → proceed to Step 2.

### Step 2 — (Overleaf) Confirm connection

First `browser_tabs list` (cheap, authoritative re-sync). Confirm the Overleaf state. Require the
active tab to be `/project/<id>` (not `/project/owned`, not `docs.overleaf.com`). If `browser_tabs`
is blocked by a "modal state" or the state conflicts with what the user reports, STOP and ask them
to dismiss the dialog — never loop `browser_file_upload`.

TIMEOUT RULE: if `browser_tabs list` has not returned after roughly 1 minute (the MCP call gets
moved to a background task and hangs), STOP and ask the user what is blocking it (modal dialog /
not logged in / wrong tab). Kill the hung task with TaskStop; do NOT keep waiting on it or retry
it repeatedly. A hung call does not burn model tokens, but its eventual result would flood the
context, so cut it off early.

### Step 3 — Fresh write vs. modify (ask)

Ask whether this is a fresh write or a modification of existing content. ANY change to an existing
document — including appending a brand-new section, continuing a document written in an earlier
turn, or editing an Overleaf project that already has derived content — counts as 修改, NOT 新写.
新写 applies ONLY when the target body is still empty / a plain untouched template. The tier is
asked again in Step 4 regardless of which branch is chosen:

- **新写** → Step 4（选档）→ Step 5 整篇撰写。
- **修改** → Step 4（选档）→ Step 6 定向修改。

### Step 4 — Choose derivation model tier (选档)

Fresh writes (→ Step 5), modifications (→ Step 6), and ANY later edit of a document — including
appending a new section or revising after an earlier choice — all produce derived content. Ask the
tier question EVERY time such content will be produced (same session or later; command line or
Overleaf). NEVER reuse an earlier tier without asking again. AskUserQuestion, ONE question:

- **当前会话模型（不映射）** — derive inline on the main model (cheap). No subagent, no extra
  overhead. Cheaper path; still offer it EVERY time, never treat it as an automatic default.
- **最高档 fable（映射 pro）** — package the whole derivation core to the `deriver` subagent
  (frontmatter `model: fable`); the harness resolves `fable` to the current provider's most
  capable/reasoning model via env. Use for heavy derivations, long algebra, proofs.

The semantic target is ALWAYS the **fable slot**, never a hardcoded model id — this file must not
contain concrete model ids. Provider/model swaps happen only in each cc-switch provider preset's
env slot mapping; do not edit this file for that.

If the user picks **最高档 fable**:

1. **Pre-flight (once, before dispatching)**: read the current env —
   `printenv ANTHROPIC_BASE_URL ANTHROPIC_DEFAULT_FABLE_MODEL`. If the FABLE mapping is missing or
   clearly mismatched with the base URL (e.g. the switching tool wrote only base/token), STOP and
   ask the user to fix the slot mapping or fall back to 当前会话模型. Never silently send `fable`
   as the built-in Claude model to a non-Anthropic endpoint.
2. **Package ONE brief (single call — do not drip-feed)**:
   - the problem restated in words; chosen coordinate system / basis / symbols / units / signature;
     assumptions and applicability conditions;
   - which Derivation-Structure sections to include and which checks to keep;
   - source equation(s) with their numbers when explaining a paper; for a modification, the target
     section's CURRENT content (read via browser on the main model first) plus the intended change
     and the document's existing conventions.
3. **Dispatch**: Agent tool targeting `deriver`. Receive the full derivation content as the tool
   result.
4. **Hand off**: route that content into the delivery steps below. Write NO intermediate files; do
   NOT re-derive on the main model.

If the user picks **当前会话模型**: derive inline on the main model following the Formula Rendering
Rules and the Required Answer Contract. This step ends.

### Step 5 — Write the whole document (fresh write)

Use the derivation content settled in Step 4 — inline on the main model, or returned by `deriver`.
This step only scaffolds and typesets that content into the target destination; it must NOT
re-derive. If the content came from `deriver` (delimiters `\(...\)` / `\[...\]`), normalize inline
math to the Overleaf dollar style per the rules below.

Compose ONE complete, self-contained `.tex` following the **Derivation Structure** above, laid
out as real `\title` / `\section` / `\subsection`. Write it in a single CodeMirror `dispatch`.
Reply with only a short status line (structure + section list + compile result).

Format rules for the `.tex`:

- Chinese content: `\documentclass[11pt]{ctexart}`, compile with **XeLaTeX** (pdfLaTeX cannot
  render Chinese).
- English-only content: `\documentclass{article}`.
- Preamble: `amsmath, amssymb, booktabs, geometry`.
- Use Overleaf-native math delimiters `$...$`, `\[...\]`, `\begin{equation}`, `\begin{aligned}`
  (dollar signs are required in the `.tex` source but forbidden in chat prose).

### Step 6 — Modify existing content (targeted retrieval, three-tier scope)

The model tier was already chosen in Step 4. If **最高档 fable**: the main model first reads the
target section's current content (per the scope below), then packages {current content + the
intended change + the document's conventions} into ONE brief for `deriver`; deriver returns the
revised/new derivation content, and the main model applies it. deriver must NOT re-read the
document itself. If **当前会话模型**: revise inline as before.

Ask which key parts the user has already changed, to decide how much to read:

- **关键部分未改**（语言、文档类、文件等都没动）→ do NOT read the whole document. One
  `browser_evaluate` returns (a) the preamble and (b) the `\section{...}` index; locate the target
  section named in the prompt, read/edit ONLY that section.
- **关键部分改过** → read those key parts (language / documentclass / packages) first, adjust the
  output to match, then modify.
- **大幅改过** → re-read the WHOLE article, re-understand it, then re-locate and modify.

Whatever the scope, **understand the article independently** before editing: revise the output and,
if needed, change direction to avoid duplicate / incorrect / mismatched content — confirm the new
text is consistent with the document's existing conventions (units, signature, notation, symbol
table) and does not repeat an existing section. Then `dispatch` the edit and compile.

### Step 7 — Write, then hand off compilation to the user

- **Read** (one call): `browser_evaluate` → `document.querySelector('.cm-content').cmView.view.state.doc.toString()`.
  Never read `.cm-content` `innerText` (CodeMirror virtualizes; returns only visible lines).
- **Write**: base64 → `atob` + `TextDecoder('utf-8')` → `dispatch`. Have the SAME evaluate return
  the verification (preamble head + `\section{...}` list + new length) — no separate verify call.
  LONG-SOURCE RULE: when the decoded `.tex` exceeds ~3000 chars (base64 > ~4000 chars), do NOT
  inline the whole payload in one evaluate — oversized single inlines get corrupted/truncated
  (observed `atob` failures). Instead: (1) split the base64 into segments of ≤2000 chars and
  retrieve ALL segments in ONE batch shell call up front (`base64 -w0 file | fold -w 2000`), so no
  repeated read round-trips later; (2) accumulate them in the page across separate evaluates, one
  per segment, in order (`window.__TMP = (window.__TMP || "") + "<seg>"`) — no per-segment length
  round-trip; (3) after the LAST segment only, verify the accumulated length ONCE against the file's
  true base64 total (`base64 -w0 file | wc -c`), then do ONE final decode + `dispatch` +
  verification. (Do NOT use `browser_run_code_unsafe`+`fs` for this — it needs the safety
  classifier, is slower/unreliable, and runs in a sandbox that has neither `require` nor dynamic
  `import`.)
- **Compile is the USER's job**: after `dispatch`, do NOT wait / click / boolean-confirm. Just ask
  「编译成功了吗？」. If 成功 → done. If 失败 → read only the `!`-prefixed error lines (one
  `browser_evaluate`) and propose a concrete fix.
- Never screenshots; never whole-page snapshots; never full logs (only `!`-prefixed lines).

### Error handling — retry twice, then ask

On any error during the workflow, retry up to **2 times**. If still failing, STOP and ask the
user: continue, or let them try first? If they cannot solve it, offer to retry again yourself.
