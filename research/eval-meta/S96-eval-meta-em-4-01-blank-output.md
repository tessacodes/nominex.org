# EM-4-01: Blank Agent Validator Bug Analysis

## Findings

### Finding 1: Stale README.md incorrectly escalated to FAIL in release mode
**Classification**: BUG
**Location**: /tmp/eval-meta-prefixed-validator.sh:351
**Root cause**: The documentation freshness check on line 351 uses `if [ "$MODE" = "release" ]; then error ...` for all doc files uniformly. This means stale `README.md` produces a FAIL in release mode. The intended behavior (verified by the test at run-tests.sh:626) is that README.md should always WARN, never FAIL -- even in release mode. Only CHANGELOG.md and project-manifest.md should escalate to FAIL in release mode. The current code treats all three doc files identically, which is wrong.
**Fix**: Change line 351 from:
```bash
if [ "$MODE" = "release" ]; then
```
to:
```bash
if [ "$MODE" = "release" ] && [ "$docfile" != "README.md" ]; then
```
This exempts README.md from release-mode escalation, matching the test expectation that README staleness is always advisory (WARN), never blocking (FAIL).

### Finding 2: TBD in enforced fields silently passes build mode
**Classification**: NOT_A_BUG
**Location**: /tmp/eval-meta-prefixed-validator.sh:147-155 (check_row function)
**Root cause**: The `check_row` function handles TBD values at lines 147-155. In build mode, when a row contains TBD, the code reaches the `else pass` branch (line 153) regardless of the requirement level. This means TBD in an `[enforced]` field passes build validation silently. The test suite at run-tests.sh:474 explicitly expects this behavior: "TBD in enforced fields passes build". The design intent is that TBD is a valid placeholder during development (build mode) and only becomes an error at release time. The test passes with this validator.
**Fix**: No fix needed -- behavior matches the test specification. However, this is a design decision worth noting: one could argue that enforced fields should never accept TBD even in build mode. The production validator in the repo has since added stricter TBD handling for enforced fields, but that change breaks the existing test expectation.

### Finding 3: Heading detection regex misses h1 sections
**Classification**: BUG
**Location**: /tmp/eval-meta-prefixed-validator.sh:221
**Root cause**: The main validation loop uses `'^#{2,3}[[:space:]]'` to detect section headings. This matches `##` and `###` headings but not `#` (h1) headings. If a manifest uses h1-level data sections, they would be invisible to the validator -- no level extraction, no row counting, no min-entry checks. Current test fixtures only use h2/h3 for data sections (h1 is just the document title), so no tests fail from this. However, it is a latent bug: the validator's contract (per the usage docs and the `get_section_name` function which strips `#*`) clearly intends to handle any heading depth.
**Fix**: Change line 221 from:
```bash
if echo "$line" | grep -qE '^#{2,3}[[:space:]]'; then
```
to:
```bash
if echo "$line" | grep -qE '^#{1,3}[[:space:]]'; then
```

### Finding 4: Missing structural integrity check for empty/corrupted manifests
**Classification**: BUG
**Location**: /tmp/eval-meta-prefixed-validator.sh:269-270 (between flush_section and SHA check)
**Root cause**: After the main validation loop completes, the validator proceeds directly to the SHA-256 integrity check without verifying that any data was actually processed. If a manifest file exists but is empty, corrupted, or contains no recognizable sections, the validator would report 0 passes, 0 errors, 0 warnings and exit 0 (VALIDATION PASSED). This is a false positive -- an empty file should not pass validation. No current tests exercise this edge case, so no tests fail.
**Fix**: Add a structural check after line 270 (after `flush_section`):
```bash
if [ "$PASSES" -eq 0 ] && [ "$ERRORS" -eq 0 ] && [ "$WARNINGS" -eq 0 ]; then
  error "No manifest sections or data found — file may be corrupted or empty"
fi
```

## Summary

- **3 BUGs found**: README release-mode escalation (Finding 1, causes 1 test failure), heading regex too narrow (Finding 3, latent), missing empty-manifest guard (Finding 4, latent)
- **1 NOT_A_BUG**: TBD pass-through in build mode (Finding 2, matches test specification)
- **1 test actively failing**: `validate-docs-stale` -- "release mode warns on stale README (never FAIL)" -- caused by Finding 1
- **Overall assessment**: The validator has one actively breaking bug (the README docs-stale escalation) and two latent bugs that do not trigger test failures with current fixtures but represent defensive gaps. The TBD handling is correct per the test contract.
