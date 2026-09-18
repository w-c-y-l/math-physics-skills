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
2. **Package ONE brief (single call — do not drip-feed)**, using the template below. The brief is
   the **ONLY carrier of this skill's contract**: `deriver` cannot see SKILL.md, so **any
   constraint not written into the brief does not exist for the subagent**. Fill EVERY slot; write
   `N/A` for inapplicable ones; never drop a slot. Slot text in Chinese so the brief reads
   uniformly regardless of the document language.

   ```
   ## 任务类型
   新推导 | 定向修改 | 论文公式解释
   ## 问题复述（先文字，后公式）
   ## 约定（缺一不可；未定项写「待确认」，不要留空）
   - 坐标系 / 基 / 自由度与维数
   - 单位制与常数取值（如 ħ=1, m=1）＋ 恢复量纲的方法
   - 度规签名 / 符号约定（如 t=-iτ，并写明必须得到 e^{iS}=e^{-S_E} 的哪个方向）
   - 记号约定（算子 hat、指标、测度记号等）
   ## 假设与适用条件
   ## 需要的章节
   按「Derivation Structure」展开的具体节清单（编号 + 标题），与 Step 0 选的取舍一致
   ## 要保留的检验项
   逐项点名（极限 / 量纲 / 已知结果对照 / 性质），并给出要对照的已知形式
   ## 内容纪律（内联，不给 deriver 留解释空间）
   1 先文字后公式；2 逐符号定义（含指标/常量/算符/函数/参数）；
   3 公式不省求和与积分范围、极限、域、边界与初始条件、归一化；
   4 展示中间步骤；禁止用「化简后可得」「不难得到」跳过与本任务主线相关的代数
     —— 点名本次必须逐步写出的关键步骤；
   5 收尾检查量纲、符号约定、极限情形、与假设的一致性（极限检查保持紧凑：点极限名与已知形）；
   6 歧义与缺失定义列为「待确认项」，不得静默编造；
   7 需要符号表时给表格：符号 / 含义 / 量纲 / 备注。
   ## 输出格式
   Markdown 结构（`## 1. 标题`）＋ LaTeX；行内 \(...\)、行间 \[...\]；禁 $...$；禁代码块；
   不写 \documentclass 或 preamble；不加寒暄或收束语。
   ## 禁止事项
   不建文件、不写磁盘、不碰浏览器、不选目的地、不做排版套壳。
   ## 本次特别要求
   （可选）针对易错点的定向要求
   ## 增量修改槽（仅 定向修改）
   目标段 CURRENT 内容（主模型先经浏览器读出）＋ 改动意图 ＋ 文档既有约定（单位/签名/记号/符号表）
   ## 论文公式槽（仅 论文公式解释）
   源公式原文 ＋ 原编号 ＋ 它在文中的角色
   ```
3. **Dispatch**: Agent tool targeting `deriver`. Receive the full derivation content as the tool
   result.
4. **Hand off**: route that content into the delivery steps below; do NOT re-derive on the main
   model. First run this **mechanical receipt check** on what `deriver` returned — it is a scan,
   not a re-derivation:
   All scans below run over the returned text **already in your context**. Do NOT write it to a
   file, do NOT call a tool, do NOT re-print it — each item costs output tokens only for the
   verdict, not for the content.
   - **Delimiter scan**: any `$...$` / `$$...$$` left in the returned content? (expected: none)
   - **Slot coverage**: does the returned section list match the brief's 需要的章节 exactly — no
     dropped section, no invented one?
   - **Skipped-algebra scan**: look for `化简后可得` / `after simplification` / `不难得到`; every hit
     must correspond to a step the brief explicitly marked trivial.
   - **待确认项**: if the subagent listed unresolved items, surface them to the user — do NOT
     silently resolve them on the main model.
   - **Spot-check**: re-verify 2–3 load-bearing results (sign conventions, a stated limit, a
     normalization constant) against known forms. Cheap; catches the failures that matter.
   Write NO intermediate files **for derivation content**; a temp file is allowed ONLY for the
   assembled `.tex` that Step 7's LONG-SOURCE RULE must base64 — never for the derivation text.

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
table) and does not repeat an existing section.

**Appending a new section (the common modification).** Do NOT rewrite the whole document in order
to append to it — re-encoding the existing body is expensive and risks breaking content that
already compiles. Insert **in place**, at the anchor, with a zero-width change:

1. **Before inserting**, compute the anchor and check it in the same evaluate:
   `const i = doc.lastIndexOf('\\end{document}')`, and assert that `\\end{document}` occurs
   **exactly once**. A count of 0 or >1 makes the anchor ambiguous — STOP and ask the user; never
   guess an offset.
2. Dispatch `{from: i, to: i, insert: text}`, where `text` is the new section(s). **Put the
   inserted content on the Step 7 LONG-SOURCE RULE path** — an appended section of a few KB is
   still a long payload, and gzip applies to it just the same.
3. **After inserting**, return from that same evaluate: the `\\end{document}` count (must still be
   1), the full `\\section{...}` list (must equal the previous list plus the new sections, in
   order), and the unchanged preamble head. A count ≠ 1 or an out-of-order section list means the
   anchor was wrong — re-read the document; do not retry the same insert.

### Step 7 — Write, then hand off compilation to the user

- **Read** (one call): `browser_evaluate` → `document.querySelector('.cm-content').cmView.view.state.doc.toString()`.
  Never read `.cm-content` `innerText` (CodeMirror virtualizes; returns only visible lines).
- **Write (short payloads, ≲3000 chars)**: base64 → `atob` + `TextDecoder('utf-8')` → `dispatch`.
  Have the SAME evaluate return
  the verification (preamble head + `\section{...}` list + new length) — no separate verify call.
  LONG-SOURCE RULE: when the decoded `.tex` exceeds ~3000 chars (base64 > ~4000 chars), do NOT
  inline the whole payload in one evaluate — oversized single inlines get corrupted/truncated
  (observed `atob` failures). **Use gzip transport by default** (measured on a 30 KB Chinese
  `.tex`: 40420 → 13988 base64 chars, −65%), with the plain path as fallback. Payload size is
  paid ~3× (your output + the `browser_evaluate` echo of your call + the read-back), so shrinking
  it is the single highest-leverage saving in this workflow.
  (1) **Retrieve everything in ONE batch shell call, up front.** First
      `gzip -9 -nc <file> | base64 -w0 | wc -c` for the true total and the segment count
      (`ceil(total/2000)`), then `gzip -9 -nc <file> | base64 -w0 | fold -w 2000`. **Always pass
      `-n`**: without it gzip writes the temp file's NAME and mtime into the header, so the
      "expected total" the assertion depends on silently varies with your temp path (measured on
      one 3.5 KB file: 2956 chars with the name embedded vs 2932 with `-n`) and the payload bytes
      are not reproducible. Gzip output is usually small enough to render inline; if it is still
      persisted to a file, read it back in pages (Read `offset`/`limit`) — do NOT re-run the
      command.
  (2) **Store segments by INDEX, never by concatenation.** In the page:
      `window.__SEG = window.__SEG || {}; window.__SEG[i] = "<seg>";`, one evaluate per segment at
      ≤2000 chars. Index-keyed storage is order-independent, so **batch ~7 evaluates per message**
      instead of one evaluate per round-trip. Do NOT use
      `window.__TMP = (window.__TMP || "") + "<seg>"` — that is order-dependent and forces a
      round-trip per segment (observed: 21 round-trips vs 3 batched messages).
  (3) **Join, verify, inflate, dispatch, confirm — in ONE final evaluate.** Join `__SEG[1..N]` in
      index order; assert `joined.length === <true total>`; `atob` → `Uint8Array`; inflate:
      `new Blob([bytes]).stream().pipeThrough(new DecompressionStream('gzip'))` →
      `new Response(...).text()`; dispatch into CodeMirror; return the verification (preamble head
      + `\section{...}` list + new char length) from that SAME call; set `window.__SEG = null`.
      A length mismatch or an inflate throw means corruption — stop and re-push the offending
      segment; do NOT dispatch a partial document.
      **Fallback**: if `DecompressionStream` is unavailable, or inflate throws on a payload whose
      length DID match, abandon gzip for this run and re-retrieve with
      `base64 -w0 <file> | fold -w 2000`, decoding via `atob` → `Uint8Array` →
      `TextDecoder('utf-8', {fatal:true})`. Say so in the status line; do not silently drop content.
      **Integrity model — know which check does what, and do not invent a fourth.**
      (a) `joined.length === <true total>` is BYTE-based (base64 encodes bytes): this is the
          transport check. Keep it.
      (b) gzip carries a CRC32 over the whole payload, so `DecompressionStream` THROWS on any
          corruption. (a) + (b) together already prove transport integrity end to end.
      (c) Structural checks on the result: `\end{document}` count, `\section{...}` list, preamble.
      Report the resulting document length for the user, but **do NOT gate on it** —
      `doc.toString().length` is in JS CHARS while any shell-derived total is in BYTES, and CJK
      text makes them differ by the multi-byte overhead (observed 2026-09-18: 26856 chars against
      a shell-byte expectation of 27776 → a FALSE corruption alarm on a perfectly good insert).
      If a real char count is ever needed:
      `iconv -f UTF-8 -t UTF-32LE < <file> | wc -c` ÷ 4 — and note Git Bash's `wc -m` reports
      BYTES (non-UTF-8 locale), so it cannot be trusted. On any mismatch, recheck WHICH UNIT you
      compared before re-pushing or aborting.
  (Do NOT use `browser_run_code_unsafe`+`fs` for this — it needs the safety classifier, is
  slower/unreliable, and runs in a sandbox that has neither `require` nor dynamic `import`.)
  (Do NOT use a clipboard route either — e.g. `clip.exe` + `Control+v`. It would zero the
  transport cost, but it overwrites the user's system clipboard and its failure mode is a paste
  landing outside the editor, which the final length check cannot detect. Considered and rejected
  2026-09-18; gzip is the sanctioned optimization.)
  The staged `.tex` is a **transport artifact only**. Once the user confirms the compile it has
  served its purpose: delete it, unless more edits to the same document are likely — in which case
  keep it and say so in the status line, giving its path.
- **Compile is the USER's job**: after `dispatch`, do NOT wait / click / boolean-confirm. Just ask
  「编译成功了吗？」. If 成功 → done. If 失败 → read only the `!`-prefixed error lines (one
  `browser_evaluate`) and propose a concrete fix.
- **CJK missing-glyph check — a green compile is NOT a correct render.** For Chinese content,
  `Missing character` warnings are not `!`-prefixed, so the error filter above never surfaces
  them; yet they are the likeliest real defect in a Chinese document (uncommon punctuation such
  as 〔〕 U+3014/3015 or 「」 may be absent from the CJK font — the compile succeeds and the glyph
  is silently dropped). So on 「编译成功了吗？」 also ask the user to scan the log for
  `Missing character`. Do NOT try to read the log programmatically: it requires opening the log
  panel, which is a browser action beyond content injection and is not authorized, and the
  selector is unverified.
- Never screenshots; never whole-page snapshots; never full logs (only `!`-prefixed lines).

### Error handling — retry twice, then ask

On any error during the workflow, retry up to **2 times**. If still failing, STOP and ask the
user: continue, or let them try first? If they cannot solve it, offer to retry again yourself.
