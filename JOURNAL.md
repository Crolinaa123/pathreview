## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/148

**Issue title:** Skill extractor fails to detect JavaScript and TypeScript

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The `extract_skills()` method in `ingestion/parsers/skill_extractor.py` doesn't recognize the JavaScript/TypeScript language family. Text that clearly describes JavaScript work (e.g. arrow functions, async/await) returns zero skill detections, and TypeScript-specific signals like `.tsx`/`.ts` files are only picked up as "React" instead of TypeScript. Other language families (Python, DevOps, databases) detect correctly, which points to a gap specifically in the JS/TS matching rules or keyword patterns in the skill extraction logic. A successful fix would update the extractor so it correctly identifies both JavaScript and TypeScript skills, and would make the related failing tests in `tests/unit/test_skill_extractor.py` pass.

**Branch name:** fix/148-js-typescript-detection

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger



## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/Crolinaa123/pathreview/commit/d310d984a555b259476db7a5f69f11edd86f2b6a

**Reproduction summary:**
Ran the repro script from issue #148 in the Python REPL. extract_skills('Wrote index.js using const arrow functions and async/await callbacks') returned []. extract_skills('Built app.tsx and types.ts with strict TypeScript interfaces') returned only ['React'], confirming JS gets no detection and TypeScript is misclassified as React.


**PLAN.md link:** (https://github.com/Crolinaa123/pathreview/blob/fix/148-js-typescript-detection/PLAN.md)



**Blockers or open questions:**
[leave blank or note anything uncertain]

## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
All 5 PLAN.md sub-tasks complete. Identified the root cause in ingestion/parsers/skill_extractor.py: JS/TS detection relied on filename extension and a broken regex (require\s+ never matched require('fs')), and Docker detection only matched the literal word "docker," which never appears in real Dockerfile/docker-compose syntax. Added JavaScript detection (require() calls, ES6 import/export, const/let/var, console.log), separated TypeScript detection from JavaScript/React (interfaces, type annotations, Promise<T>), and added a new _detect_docker method for structural Dockerfile/docker-compose detection. Also fixed a mypy type-annotation issue the pre-commit hook caught along the way.

**Next steps:**
Run make check and the full make test-unit suite to confirm no regressions beyond the one known pre-existing failure, then open the PR for review.

**Blockers:**
None currently — resolved earlier issues with commits accidentally landing on main instead of the feature branch, and an indentation bug that pulled _detect_docker outside the class.


---

### Check-in 2 (end of week)

**PR link:** https://github.com/ascherj/pathreview/pull/838

**Branch:** fix/148-js-typescript-detection

**What you built:**
Fixed the skill extractor to correctly detect JavaScript, TypeScript, and Docker/Docker Compose usage, replacing filename-only and literal-keyword matching with evidence-based structural detection.

**Tests added or updated:**
No new tests added — the existing test_javascript_detection, test_text_with_typescript_files, test_devops_tool_detection, and test_docker_compose_detection in tests/unit/test_skill_extractor.py now all pass against the updated implementation.


**Self-review confirmation:** [ ] make check passes  [ ] make test-unit passes

**Draft PR feedback received from:** none
