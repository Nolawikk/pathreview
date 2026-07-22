## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/88

**Issue title:** `POST /reviews` endpoint has no test for when the profile has no ingested documents

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The `POST /reviews` endpoint currently has no test coverage for the case where a user's profile exists but has no ingested documents attached to it. It's unclear whether the endpoint fails gracefully with a proper error response or crashes unexpectedly in this scenario, since nothing exercises that code path today. This affects the API layer, specifically the review-creation flow tested in `tests/unit/test_review_routes.py`. A successful fix will add a test that calls the endpoint under this condition and asserts it returns an appropriate error response rather than an unhandled exception, closing a gap in the test suite around edge-case handling.

**Branch name:** test/88-reviews-no-documents-test

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [ ] Issue added to cohort ledger