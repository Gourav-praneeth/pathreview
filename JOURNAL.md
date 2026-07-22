# Journal

## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/147

**Issue title:** Resume section detection fails on text with leading whitespace

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

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

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [ ] Issue added to cohort ledger
