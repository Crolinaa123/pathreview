## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/148

**Issue title:** Skill extractor fails to detect JavaScript and TypeScript

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The `extract_skills()` method in `ingestion/parsers/skill_extractor.py` doesn't recognize the JavaScript/TypeScript language family. Text that clearly describes JavaScript work (e.g. arrow functions, async/await) returns zero skill detections, and TypeScript-specific signals like `.tsx`/`.ts` files are only picked up as "React" instead of TypeScript. Other language families (Python, DevOps, databases) detect correctly, which points to a gap specifically in the JS/TS matching rules or keyword patterns in the skill extraction logic. A successful fix would update the extractor so it correctly identifies both JavaScript and TypeScript skills, and would make the related failing tests in `tests/unit/test_skill_extractor.py` pass.

**Branch name:** fix/148-js-typescript-detection

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger

