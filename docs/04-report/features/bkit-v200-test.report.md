# bkit v2.0.5 Comprehensive Test Report

> Generated: 2026-08-16T04:09:40.537Z
> Total: 1967 TC, 1935 PASS, 25 FAIL, 7 SKIP
> Pass Rate: 98.4%

---

## Summary

| Category | Total | Passed | Failed | Skipped | Rate |
|----------|:-----:|:------:|:------:|:-------:|:----:|
| Unit Tests | 1967 | 1935 | 25 | 7 | 98.4% FAIL |
| **Total** | **1967** | **1935** | **25** | **7** | **98.4%** |

## Version Comparison: v1.6.2 → v2.0.0

| Metric | v1.6.2 | v2.0.0 | Delta |
|--------|:------:|:------:|:-----:|
| Categories | 8 | 1 | +-7 |
| Total TC | 1151 | 1967 | +816 |
| Unit Tests | 450 | 1967 | +1517 |

## Failures

### Unit Tests

- **unit/evals-runner-wrapper.test.js**: Execution error: Command failed: node "C:\Users\Igor\Desktop\Projekt\0_Rastaran\test\unit\evals-runner-wrapper.test.js"
- **unit/i18n-translator.test.js**: Execution error: Command failed: node "C:\Users\Igor\Desktop\Projekt\0_Rastaran\test\unit\i18n-translator.test.js"
- **unit/quality-gates-m1-m10.test.js**: Execution error: Command failed: node "C:\Users\Igor\Desktop\Projekt\0_Rastaran\test\unit\quality-gates-m1-m10.test.js"
FAIL quality-gates M1-M10 invariant violations:
  [catalog_unreadable] Cannot read C:\Users\Igor\Desktop\Projekt\0_Rastaran\docs\reference\quality-gates-m1-m10.md: ENOENT: no such file or directory, open 'C:\Users\Igor\Desktop\Projekt\0_Rastaran\docs\reference\quality-gates-m1-m10.md'

- **unit/token-report.test.js**: Execution error: Command failed: node "C:\Users\Igor\Desktop\Projekt\0_Rastaran\test\unit\token-report.test.js"
- **unit/worktree-detector.test.js**: Execution error: Command failed: node "C:\Users\Igor\Desktop\Projekt\0_Rastaran\test\unit\worktree-detector.test.js"

## Verdict

**25 TESTS FAILED** - Issues must be resolved before release.
