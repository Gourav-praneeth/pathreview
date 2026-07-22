# Journal

## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/147

**Issue title:** Resume section detection fails on text with leading whitespace

**Tier:** Tier 1

**Selection reasoning:**
I'm comfortable with Python and testing generally, but this is my first time working
in the pathreview codebase specifically, so I chose a Tier 1 issue on purpose rather
than defaulting to it out of caution. My goal for this first issue is to learn the
repo's contribution workflow end-to-end — branch naming, commit conventions, test
layout, and the PR process — on a change that's small and well-scoped, before taking
on a Tier 2/3 issue that touches more of the architecture. This issue in particular
is a good fit: it's isolated to a single method (`_detect_sections()`) in one parser
file, has three existing failing tests that already define "done," and doesn't
require touching the API, RAG pipeline, or agent layer, so I can focus on
understanding the ingestion module in depth rather than juggling multiple subsystems.

**Problem summary:**
The `_detect_sections()` method in `ingestion/parsers/resume_parser.py` uses regex
patterns anchored with `^` and `\n` to find section headers like "Education" or
"Skills". When PDF text extraction preserves leading indentation/whitespace on
each line, those patterns no longer match, so `detected_sections` comes back
empty even though the sections are clearly present in the text. A successful
fix will make section detection tolerant of leading whitespace so resumes with
indented text (a common PDF extraction artifact) are parsed correctly, fixing
the related failing tests `test_parse_single_column_resume_text`,
`test_parse_resume_no_work_experience`, and `test_detect_sections`.

**Branch name:** fix/147-resume-parser-whitespace

**Setup confirmation:** Yes, the app runs locally at localhost:5173

**Cohort ledger:** I've added the issue to cohort ledger

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/Gourav-praneeth/pathreview/commit/90f7ed5

**Reproduction summary:**
Reproduced the bug two ways: ran the three failing unit tests named in the issue
(all failed with `AssertionError` on empty/missing detected sections), and called
`ResumeParser._detect_sections()` directly on indented vs. unindented versions of
the same text — the indented version returned `[]` while the unindented version
correctly returned `['Education', 'Skills']`, confirming leading whitespace is
what breaks the regex anchors.

**PLAN.md link:** https://github.com/Gourav-praneeth/pathreview/blob/fix/147-resume-parser-whitespace/PLAN.md

**Blockers or open questions:**
Not yet sure how much leading whitespace real PDF extraction actually produces
(plain spaces vs. tabs vs. non-breaking spaces) — planning to keep the fix scoped
to spaces/tabs unless a real fixture surfaces something wider.
