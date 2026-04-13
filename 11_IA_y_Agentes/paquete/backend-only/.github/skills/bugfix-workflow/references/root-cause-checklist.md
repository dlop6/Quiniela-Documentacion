# Root Cause Checklist

Before calling a cause “root cause,” check:

1. Is the expected behavior documented?
2. Is the issue happening at the correct layer, or is it only a symptom?
3. Is the bug caused by missing validation, wrong ownership, stale assumptions, or undocumented fallback?
4. Would the proposed fix alter adjacent documented behavior?
5. What regression-sensitive flows should be rechecked after the fix?
