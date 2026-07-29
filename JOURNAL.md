## Week 7 - Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/88

**Issue title:** `POST /reviews` endpoint has no test for when the profile has no ingested documents

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The `POST /reviews` endpoint currently has no test coverage for the case where a user's profile exists but has no ingested documents attached to it. It's unclear whether the endpoint fails gracefully with a proper error response or crashes unexpectedly in this scenario, since nothing exercises that code path today. This affects the API layer, specifically the review-creation flow tested in `tests/unit/test_review_routes.py`. A successful fix will add a test that calls the endpoint under this condition and asserts it returns an appropriate error response rather than an unhandled exception, closing a gap in the test suite around edge-case handling.

**Branch name:** test/88-reviews-no-documents-test

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [ ] Issue added to cohort ledger

## Week 8 - Reproduction & solution planning

**Reproduction commit link:** https://github.com/Nolawikk/pathreview/commit/b5dc331

**Reproduction summary:**
I traced the bug by reading through `api/routes/reviews.py` and `core/services/review_service.py`. I found that `_run_agent_orchestration` and `_run_rag_retrieval_generation` are placeholder functions that ignore their input entirely, so I wrote a test calling them directly with an empty ingestion results list (simulating a profile with no documents). The test confirmed both functions still return fabricated, non-empty feedback and a normal-looking score, even with zero real input data.

**PLAN.md link:** https://github.com/Nolawikk/pathreview/blob/test/88-reviews-no-documents-test/PLAN.md

**Walkthrough video (recommended):** [not recorded]

**Blockers or open questions:**
Still unsure whether the correct fix is a new "no_data" status or reusing the existing "failed" status - need to check the Review model and how the frontend displays failure states before finalizing the plan.
