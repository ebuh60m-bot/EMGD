# Security Specification for UH-60M Training System

## Data Invariants
1. `EmergencyEntry`: Must have a valid `date` (YYYY-MM-DD), non-empty `content`, and a server-side `createdAt`.
2. `LimitationEntry`: Must have a valid `date`, `filled`, `correct`, and `wrong` must be non-negative integers. `createdAt` must be server-side.
3. Access: Submissions are anonymous. For this public training tool, any user can `create` a result. `read` access is allowed for lists to show statistics and anonymous feedback.

## The "Dirty Dozen" Payloads (Denial Expected)
1. Missing `date` in EmergencyEntry.
2. `content` size > 10,000 characters in EmergencyEntry.
3. `filled` as a string in LimitationEntry.
4. `correct` > `filled` in LimitationEntry (Integrity breach).
5. Attempting to update an existing result (Results should be immutable).
6. Attempting to delete a result.
7. Injection of "Ghost Fields" (e.g., `isVerified: true`).
8. `date` in a format other than YYYY-MM-DD.
9. `createdAt` provided as a client string instead of server timestamp.
10. `wrong` being negative.
11. `id` poisoning with extremely long document IDs.
12. Blanket listing without a date filter (if we want to enforce focused queries).

## Test Assertions
- `create` permitted for valid schema.
- `update` denied (immutable entries).
- `delete` denied.
- `list` permitted for statistical viewing.
