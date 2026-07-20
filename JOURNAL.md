# Journal

## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/151

**Issue title:** Bias detector patterns are too narrow to match common phrasings

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Why this issue fits:**
This is my first contribution to this codebase, so I deliberately looked for a Tier 1
issue rather than something touching the RAG pipeline or agent orchestration I'm not
yet familiar with. Before picking it, I cross-checked the live GitHub issue tracker
(not just the course's static issue list) and found that most other Tier 1 "good
first issue" bugs already had 3-5 competing open PRs from classmates — this one had
none. I also verified the bug myself locally by running
`tests/unit/test_bias_detector.py` rather than trusting the issue title, which
confirmed 9 of 29 tests currently fail. The fix is scoped to a single file
(`safety/bias_detector.py`, a ~40-line class with two regex-pattern lists) with an
existing, thorough test file that already encodes the expected behavior, so I have a
clear, bounded definition of "done" without needing to design new test cases from
scratch — a good match for a first issue where I'm still learning the codebase's
conventions.

**Problem summary:**
`safety/bias_detector.py` flags biased feedback language using a fixed list of regex
patterns for two categories: dismissive comments about educational background (e.g.
bootcamp vs. university) and demographic assumptions (age, immigration status,
socioeconomic background). The patterns only match a handful of rigid phrasings, so
common real-world variations of the same biased statements slip through undetected.
I confirmed this by running the existing test suite: 9 of 29 tests in
`tests/unit/test_bias_detector.py` currently fail, including cases like "self-taught
developers are not equal to university graduates" and "bootcamp attendance means
inadequate training" that should be flagged but aren't. A successful fix broadens the
regex patterns (or replaces them with more flexible matching) so the detector catches
these variations without introducing false positives on neutral/positive feedback,
which the test file already has coverage for. This affects the `safety` module, which
sits between the RAG-generated feedback and what's shown to the end user.

**Branch name:** fix/151-bias-detector-narrow-patterns

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger
