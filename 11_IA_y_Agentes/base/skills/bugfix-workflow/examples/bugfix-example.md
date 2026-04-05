# Example

Task: fix a lock-related bug where an action is allowed after the documented lock window.

Expected workflow:

1. Reproduce the failing behavior.
2. Read the lock-owning documentation.
3. Confirm the system is violating documented behavior.
4. Identify whether the root cause is boundary validation, time authority, or state handling.
5. Apply the smallest fix that restores documented lock enforcement.
6. Validate regression impact on adjacent pick flows.
