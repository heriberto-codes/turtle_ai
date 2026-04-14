## 🐢 🐢 Turtle AI 🐢 🐢
>Go slower. Understand more. Break less.

Turtle AI is an AI coding workflow where progress is gated by engineer comprehension, not just code generation.

It is designed to preserve engineer understanding, codebase context, and long-term ownership while still using AI to move faster.

Most AI coding tools optimize for speed. Turtle AI optimizes for control.

Its key distinction is the **⚙️ IMPLEMENTATION LOOP (Controlled Development Step-by-Step)**:

`EXECUTE → VERIFY → ENGINEER CHECKPOINT → TEST → DEBUG → PLAN STEP UPDATE`

At the center of that loop is **ENGINEER CHECKPOINT**.

This is the comprehension gate. Before work can move forward, the engineer must be able to explain:
- what was implemented
- why it works
- how it fits into the system

That changes the role of AI completely.

Instead of turning the engineer into a passive reviewer, Turtle AI keeps the engineer as the source of truth and uses AI as a constrained implementation partner.

This prevents the most common failure mode of AI-assisted development:

➡️ the engineer gradually losing real understanding of their own codebase

Turtle AI keeps changes small, reviewable, and aligned with actual architecture so knowledge compounds instead of decays.

>Turtle AI is not about writing less code. It is about retaining control while using AI to move faster — safely.

## 🧠 Turtle AI Workflow
A structured, human-tethered development flow that preserves engineer understanding while leveraging AI.

**🚀 How to Use Turtle AI (Quick Start)**

    1️⃣ Run FOUNDATION once:
    AGENTS
    ARCHITECTURE
    REPO_MAP

    2️⃣ Choose or generate work:
    IDEATE (optional)
    BACKLOG (select a feature)

    3️⃣ Understand the system (if needed):
    ANALYZE

    4️⃣ Plan the feature:
    PLAN → creates docs/plans/<feature_slug>_plan.md

    5️⃣ Run the IMPLEMENTATION LOOP until complete:
    EXECUTE → VERIFY → ENGINEER CHECKPOINT → TEST → DEBUG (if needed) → PLAN STEP UPDATE
    Repeat until no unchecked steps remain

    6️⃣ Finalize:
    SECURITY → PERFORMANCE → BACKLOG UPDATE → DOCUMENT → COMMIT
---

**🧱 FOUNDATION (Static Context)**

    1️⃣ AGENTS
    Project rules and safety constraints. Prevents AI from violating conventions.

    2️⃣ ARCHITECTURE
    System blueprint.
    Defines how the system is structured and what patterns should be preserved.

    3️⃣ REPO_MAP
    High-level navigation of the repo, protected paths, critical modules,  
    and important file locations.

**💡 DISCOVERY (Problem + Opportunity)**

    4️⃣ IDEATE
    What should we build?

    5️⃣ BACKLOG
    Store and track feature ideas in: docs/backlog.md

    6️⃣ ANALYZE
    AI learns the repository. Why this matters: without it, AI re-learns the repo every task.


**🧭 PLANNING (Architecture + Scope)**

    7️⃣ PLAN
    Architect chosen feature → docs/plans/<feature_slug>_plan.md

**⚙️ IMPLEMENTATION LOOP (Controlled Development Step-by-Step)**
This loop runs repeatedly until all plan steps are complete.

    8️⃣ EXECUTE
    Implement only the next unchecked task from the plan

    9️⃣ VERIFY
    AI performs code review of correctness and scope

    🔟 ENGINEER CHECKPOINT
    Engineer comprehension checkpoint

    1️⃣1️⃣ TEST
    Two-phase step:

    A. TEST REVIEW
    - Determine the smallest correct set of tests required for the current active plan step

    B. TEST IMPLEMENTATION
    - Implement ONLY the tests identified in TEST REVIEW

    1️⃣2️⃣ DEBUG
    Diagnose issues and route fixes to the correct layer (EXECUTE or TEST). 
    only if something fails

    1️⃣3️⃣ PLAN STEP UPDATE (REQUIRED) ✅
    This step is NOT optional. MUST run after every successful loop.  
    A step is NOT complete until this executes


**🛡️ HARDENING (Quality Gates)**

    1️⃣4️⃣ SECURITY
    Run focused security review for new or changed behavior

    1️⃣5️⃣ PERFORMANCE
    Run targeted performance review when the feature affects performance-sensitive paths

**🚀 FINALIZATION (Ship + Record)**

    1️⃣6️⃣ BACKLOG UPDATE
    Mark feature complete

    1️⃣7️⃣ DOCUMENT
    Capture architectural decisions inside the repo
    Note: Run after the feature is complete, not after every step

    1️⃣8️⃣ COMMIT
    Prepare commit message only

***

**🌐 GLOBAL RULES**

**Current Step Detector Pattern**

    Use the plan file as the source of truth for workflow state:

    docs/plans/<feature_slug>_plan.md

    Step state rules
    - The active loop step = the first unchecked step:
    - - [ ]
    - The last completed step = the most recently checked step:
    - the last - [x]
    - The plan is complete when no unchecked steps remain

    Detection rules

    Every prompt that needs step state must determine. 
    it from the plan file instead of asking for manual step input.

    Use this pattern:
    1. Read docs/plans/<feature_slug>_plan.md
    2. Find all checklist items
    3. For EXECUTE, VERIFY, ENGINEER CHECKPOINT, TEST, and DEBUG on active work:
    - use the first unchecked step - [ ]
    4. For prompts that operate on already-completed work after loop completion:
    - use the most recently checked step - [x]
    5. If no unchecked steps remain:
    - treat the plan as complete
    - stop or move to FINALIZATION as appropriate

    Manual input rule
    - Do NOT ask for current step name or number if it can be derived from the plan file
    - The plan file is the single source of truth for step progression

    🗄️ STATE MODEL:
    This workflow is plan-driven. The plan file controls all execution state.
    Use the CURRENT STEP DETECTOR PATTERN to derive workflow state from:
    docs/plans/<feature_slug>_plan.md

**Step State Invariant (CRITICAL)**

    Throughout the execution loop:

    - The active step ALWAYS remains unchecked (- [ ])
    - A step is ONLY marked complete (- [x]) after:
    EXECUTE → VERIFY → ENGINEER CHECKPOINT → TEST → DEBUG (if needed)

    This means:
    - EXECUTE, VERIFY, ENGINEER CHECKPOINT, and TEST ALL operate on:
    → the FIRST unchecked step (- [ ])

    - PLAN STEP UPDATE is the ONLY step allowed to:
    → change [ ] → [x]

    Violation of this rule breaks workflow state consistency.


**Debug Routing Rule**
    DEBUG does NOT automatically mean "fix the code".

    Always route based on root cause:
    → EXECUTE (DEBUG FIX) — code issue
    → EXECUTE (VERIFY FIX MODE) — plan mismatch
    → TEST — test issue

    Always fix the correct layer. Never mix layers.

    ┌─────────────────────────────────────────────────────┐
    │ Root Cause              → Route To                  │
    ├─────────────────────────────────────────────────────┤
    │ Code is wrong           → EXECUTE (DEBUG FIX)       │
    │ Code doesn't match plan → EXECUTE (VERIFY FIX MODE) │
    │ Tests are wrong         → TEST                      │
    │ Unclear / mixed         → EXECUTE (DEBUG FIX)       │
    └─────────────────────────────────────────────────────┘

    Never fix the wrong layer.

**Engineer Checkpoint Rule (CRITICAL)**

    ENGINEER CHECKPOINT is a required comprehension gate before moving forward.

    The engineer must be able to:
    - explain what was implemented
    - explain why it works
    - explain how it fits into the system

    If the engineer cannot confidently answer:
    - STOP the workflow
    - do NOT proceed to TEST or PLAN STEP UPDATE
    - revisit the implementation

    Rules:
    - This step enforces understanding, not correctness
    - Passing VERIFY does NOT guarantee passing ENGINEER CHECKPOINT
    - The engineer is the source of truth, not the AI

    Purpose:
    Prevent passive approval of code and ensure long-term ownership of the system.

**🔁 CORE EXECUTION LOOP (Step-by-Step)**
    This loop runs repeatedly until all plan steps are complete.

    1. EXECUTE — implement one scoped step
    2. VERIFY — review correctness and scope
    3. ENGINEER CHECKPOINT — confirm understanding
    4. TEST — validate behavior
    5. DEBUG — only if something fails
    6. PLAN STEP UPDATE (REQUIRED) ✅
    - This step is NOT optional
    - MUST run after every successful loop
    - A step is NOT complete until this executes

***

**📂 FOLDER STRUCTURE** 

    project-root
    │
    ├── agents.md
    ├── architecture.md
    ├── repo_map.md
    │
    ├── prompts/
    │   ├── foundation/
    │   │   ├── agents_prompt.md
    │   │   ├── architecture_prompt.md
    │   │   └── repo_map_prompt.md
    │   │
    │   ├── discovery/
    │   │   ├── ideate_prompt.md
    │   │   ├── backlog_prompt.md
    │   │   └── analyze_prompt.md
    │   │
    │   ├── planning/
    │   │   └── plan_prompt.md
    │   │
    │   ├── loop/
    │   │   ├── execute_prompt.md
    │   │   ├── verify_prompt.md
    │   │   ├── engineer_checkpoint_prompt.md
    │   │   ├── test_prompt.md
    │   │   ├── debug_prompt.md
    │   │   └── plan_step_update_prompt.md
    │   │
    │   ├── hardening/
    │   │   ├── security_prompt.md
    │   │   └── performance_prompt.md
    │   │
    │   └── finalization/
    │       ├── backlog_update_prompt.md
    │       ├── document_prompt.md
    │       └── commit_prompt.md
    │
    ├── docs/
    │   ├── analysis/
    │   │   └── repo_analysis.md
    │   │
    │   ├── system/
    │   │   └── current_step_detector.md
    │   │
    │   ├── backlog.md
    │   │
    │   ├── plans/
    │   │   └── <feature_slug>_plan.md
    │   │
    │   └── features/
    │       └── <feature_slug>.md

Folder purposes:
- prompts/ = reusable workflow behavior
- docs/analysis/ = repo understanding
- docs/system/ = shared workflow rules
- docs/plans/ = active feature execution state
- docs/features/ = final feature records
