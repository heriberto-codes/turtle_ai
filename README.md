## 🐢 🐢 Turtle AI  🐢 🐢
    Avoid human context decay

> Turtle AI is a human-tethered coding workflow that slows AI down just enough to preserve engineer understanding, codebase context, and long-term ownership. Its purpose is to prevent the engineer from becoming a passive reviewer of AI-written code. This would apply in the loop phase of the workflow. 

**🧱 FOUNDATION (Static Context)**

    1️⃣ AGENTS
    Project rules and safety constraints. Prevents AI from violating conventions.

    2️⃣ ARCHITECTURE
    System blueprint.
    Defines how the system is structured and what patterns should be preserved.

    3️⃣ REPO_MAP
    High-level navigation of the repo, protected paths, 
    critical modules, and important file locations.

**💡 DISCOVERY (Problem + Opportunity)**

    4️⃣ IDEATE  
    What should we build?

    5️⃣ BACKLOG  
    Store and track feature ideas in: docs/backlog.md

    6️⃣ ANALYZE  
    AI learns the repository. Why this matters:  
    Without it, AI re-learns the repo every task

**🧭 PLANNING (Architecture + Scope)**

    7️⃣ PLAN  
    Architect chosen feature → docs/plans/<feature_slug>_plan.md

**⚙️ IMPLEMENTATION LOOP (Controlled Development)**

    8️⃣ EXECUTE  
    Implement ONLY the next unchecked task from PLAN   

    9️⃣ VERIFY  
    AI performs code review 

    🔟 ENGINEER CHECKPOINT
    Engineer comprehension checkpoint

    1️⃣1️⃣ TEST  
    Write or update tests for the current task  

    1️⃣2️⃣ DEBUG  
    Conditional step only run when EXECUTE, VERIFY, CHECKPOINT, or TEST reveals problems. 

    🔄 LOOP
    Repeat: EXECUTE → VERIFY → CHECKPOINT → TEST → DEBUG

**🛡️ HARDENING (Quality Gates)**

1️⃣3️⃣ SECURITY  
Run focused security review for new or changed behavior

1️⃣4️⃣ PERF  
Run targeted performance review when feature touches:

**🚀 FINALIZATION (Ship + Record)**

1️⃣5️⃣ BACKLOG UPDATE  
Mark feature complete

1️⃣6️⃣ DOCUMENT  
Capture architectural decisions inside the repo  
Note: Run AFTER feature is complete not after every step

1️⃣7️⃣ COMMIT  
Prepare commit message only 