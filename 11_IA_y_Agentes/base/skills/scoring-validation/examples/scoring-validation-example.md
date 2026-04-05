# Example

Task: validate a change that modifies lock enforcement.

Expected workflow:

1. Read the official lock-related rules first.
2. Confirm the change does not depend on undocumented timing assumptions.
3. Check whether backend and UI stay subordinate to the same authority model.
4. Identify regression-sensitive flows around picks and score outcomes.
5. Report competitive impact and required validation.
