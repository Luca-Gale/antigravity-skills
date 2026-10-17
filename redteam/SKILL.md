---
name: redteam
description: >-
  Adversarial architectural validation and ideation workflow for new projects or features.
  Use this skill when the user wants to brainstorm, validate, or stress-test a project idea
  through a structured Blue Team (Architect) vs. Red Team (Skeptic) live debate, cross-examination,
  and technical specification generation, or when invoked via /redteam.
---

# Red-Team Ideation & Architectural Validation (`/redteam`)

This skill orchestrates a structured, multi-perspective architectural workflow in a **single chat timeline**. It pairs a constructive **Blue Team (The Architect)** with an adversarial **Red Team (The Skeptic)** to stress-test ideas, surface blind spots, and educate the user on trade-offs before writing production code.

---

## Core Operating Principles

1. **Single-Chat Timeline**: Every phase—discovery, the 2-round debate, cross-examination, and final sign-off—takes place in the main chat thread so the user has full visibility without switching windows.
2. **Protect Creative Momentum in Phase 1**: Never turn Phase 1 into a 20-minute interrogation. Limit discovery to **2–3 quick conversational turns** focused strictly on the **4 Core Pillars**. Leave minor details, edge cases, and failure modes for Phase 3.
3. **360° Gap Detection**: While keeping questions brief, the Blue Team proactively identifies missing architectural components, unstated constraints, and structural blind spots to assemble a complete v1 draft.
4. **Pedagogical Cross-Examination**: Never expect the user to vote on complex trade-offs from a two-line summary. Always open the **Cross-Examination Chamber** when deadlocks occur so the user can freely question both teams from first principles.

---

## Workflow State Machine

### Phase 1: Lean 360° Discovery (🔵 Blue Team — The Architect)

Adopt the persona defined in [blue_team_persona.md](./references/blue_team_persona.md).

1. **Check the Idea Across the 4 Core Pillars**:
   - **Pillar 1 — Purpose & Problem**: What core problem does this solve and for whom?
   - **Pillar 2 — Surface & Environment**: Where does it run? (CLI, web, mobile, desktop, local-first, cloud, background daemon?)
   - **Pillar 3 — Core Flow & Components**: What are the 2–3 primary user actions, and what essential system components are needed to support them?
   - **Pillar 4 — Hard Constraints**: Any specific tech stack preferences, scale expectations, budget limits, or maintenance appetite (e.g., zero-ops vs. managed cloud)?
2. **Proactive Gap Filling**:
   - If the user's initial pitch leaves out critical components (e.g., authentication, data persistence, ingestion pipeline, sync mechanism), point them out concisely and propose sensible defaults rather than asking open-ended trivia.
   - Ask at most **3–4 high-leverage questions per turn** across a maximum of **2–3 turns**.
3. **Exit Criterion**:
   - Once the 4 Core Pillars are clear, announce that the baseline design is ready and proceed immediately to Phase 2.

---

### Phase 2: Baseline Technical Specification (`TECH_SPEC.md` v1)

1. Determine the target project directory:
   - If a workspace is open, write files to the workspace root.
   - If no workspace is open, create a clean project directory under `C:\Users\lucag\.gemini\antigravity\scratch\<project-slug>\`.
2. Create `TECH_SPEC.md` (Status: `DRAFT v1 — Pre-Red-Team`) using [tech_spec_template.md](./resources/tech_spec_template.md).
3. Briefly confirm to the user that `TECH_SPEC.md` (v1) has been saved, and immediately launch **Phase 3** in the same response or upon user confirmation.

---

### Phase 3: The 2-Round Live Debate (🔴 Red Team vs. 🔵 Blue Team)

Load the adversarial rubric from [red_team_persona.md](./references/red_team_persona.md).

> [!TIP]
> **Preventing Confirmation Bias**: To ensure genuine adversarial rigor, you may optionally invoke an isolated `self` or `research` subagent that reads *only* `TECH_SPEC.md` and `red_team_persona.md` (without the Phase 1 chat history) to generate the Red Team attacks, then render its verbatim output directly in the main chat.

Render both rounds clearly in the chat using distinct headers:

#### Round 1: Broadside Critique & Initial Defense
- **`## 🔴 Red Team — Round 1: Adversarial Audit & Counter-Proposal`**
  - Attack `TECH_SPEC.md` across four axes:
    1. **Premature Complexity & Over-Engineering**
    2. **Missing Edge Cases, Security & Failure Modes** (surfacing the details deferred from Phase 1)
    3. **Scalability & Maintenance Traps**
    4. **The Counter-Proposal** (propose a concrete, radically simpler or superior alternative architecture)
- **`## 🔵 Blue Team — Round 1: Defense & Concessions`**
  - Address every Red Team point explicitly using status tags:
    - `[CONCEDED]` — Red Team exposed a genuine flaw or better approach; Blue Team adopts it.
    - `[DEFENDED]` — Blue Team rebuts with concrete rationale tied to the user's goals.
    - `[DEADLOCK]` — A genuine architectural trade-off exists with valid arguments on both sides.

#### Round 2: Focused Pressure-Test & Final Stances
- **`## 🔴 Red Team — Round 2: Focused Rebuttal`**
  - Drop all `[CONCEDED]` points.
  - Drill down exclusively on `[DEFENDED]` and `[DEADLOCK]` items, plus any new risks introduced by Round 1 concessions.
- **`## 🔵 Blue Team — Round 2: Final Reconciliation`**
  - Finalize what is agreed upon and isolate the remaining irreconcilable forks for the user.

---

### Phase 4: The Cross-Examination Chamber (Interactive Deposition)

At the conclusion of Round 2:

1. **Present the Post-Debate Briefing**:
   - **✅ Agreed Improvements**: Bullet list of changes both teams already aligned on.
   - **⚖️ Open Architectural Forks (Deadlocks)**: Numbered list of unresolved decisions where Blue and Red disagree.
2. **Open the Cross-Examination Chamber**:
   - Do **not** force an immediate blind vote.
   - Explicitly invite the user into **Cross-Examination Mode**:
     - Explain each fork in plain, accessible terms (what is at stake in practice).
     - Prompt the user:
       > *"We are now in the **Cross-Examination Chamber**. Before you vote on the open forks, you can freely question either team right here in the chat:*
       > - *Ask `Blue: ...` to explore the Architect's approach*
       > - *Ask `Red: ...` to explore the Skeptic's counter-proposal*
       > - *Ask `Both: ...` to have both sides compare a specific aspect (e.g., code examples, learning curve, debugging, cost)*
       > *Or, if you're already confident, tell me your decision on each fork."*
3. **Handling Cross-Examination Turns**:
   - Whenever the user questions `Blue`, `Red`, or `Both`, respond in character following the **Pedagogical Mandate**:
     - Explain from **first principles** without unexplained jargon.
     - Use concrete real-world analogies, day-in-the-life developer scenarios, and short code/architecture snippets when helpful.
     - Honestly acknowledge the downside of one's own position ("Here is the honest price you pay if you choose my approach...").
   - Stay in Cross-Examination Mode for as many turns as the user wants until they issue their ruling.

---

### Phase 5: Final Synthesis & Deliverables

Once the user delivers their ruling on the open forks:

1. **Update `TECH_SPEC.md`**:
   - Promote status to `FINAL — Red-Team Validated`.
   - Integrate all Round 1/2 concessions and the user's executive rulings into a cohesive, implementation-ready specification.
2. **Create `RED_TEAM_AUDIT.md`**:
   - Use [audit_log_template.md](./resources/audit_log_template.md) to record:
     - Initial blind spots uncovered
     - Concessions made during the debate
     - Discard pile (rejected ideas and why)
     - User's executive rulings and rationale
3. **Present the Final Hand-Off**:
   - Provide clickable links to both `TECH_SPEC.md` and `RED_TEAM_AUDIT.md`.
   - Summarize the finalized architecture and ask if the user would like to begin implementation.
