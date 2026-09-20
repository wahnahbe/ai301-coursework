# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69

**Verdict output**

Live-mode run of `issue-select` on 2026-09-20 against three open Path Review issues (#68, #69, #72), pasted verbatim from the skill's output, ending with the fenced JSON verdict block. The chosen issue, #69, is the first item and records `accept`.

````
## Ranked read-out

All three candidates are inside the scoped repo (`codepath/pathreview-ai301-fa26-s3`), and all three **accept**: every required check passes on each. Shared repo-level evidence: archived = no, last push 2026-09-16 (4 days ago), last 5 `main` commits all by human Andrew Burke (Aburke225) on 2026-08-24 and 2026-09-16, zero PRs in the repo, and `docs/CONTRIBUTING.md` says nothing about AI (it does require green CI, xfail-marker removal, and Conventional Commits — conditions, not bans).

**Accepted, in fit order:**

1. **#69 — Output parser crashes on top-level JSON array fallback** (`rag/generator/output_parser.py`). Best fit: it's squarely in the RAG pipeline the profile wants to learn (handling LLM output), pure Python with a covering unit test to un-xfail, and the thread is completely empty — nobody else is on it yet.
2. **#68 — Keyword search `ZeroDivisionError` on empty index** (`rag/retriever/keyword_search.py`). Also RAG + Python + tests, and BM25 will feel familiar to someone with a data-science background. One classmate (acordero4852) posted a claim on 2026-09-19; under the Path Review house rule that doesn't block, but it's why #69 edges it out.
3. **#72 — `verify_password` raises `UnknownHashError`** (`core/security.py`). Smallest job (1–2 h), Python API code — fits, but it's auth plumbing rather than RAG/API-endpoint work, and two classmates' fork commits already reference it.

**Rejected:** none.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
    "checks": [
      {"name": "maintainer-responds", "grade": "pass", "evidence": "Opened by Aburke225 (COLLABORATOR); last 5 main commits are by human Andrew Burke, newest 2026-09-16 (4 days ago); sample issues #52/#43 got maintainer replies within 6 days"},
      {"name": "repo-active", "grade": "pass", "evidence": "archived: false; pushed_at 2026-09-16T21:50:20Z, within 90 days of 2026-09-20"},
      {"name": "not-claimed", "grade": "pass", "evidence": "No assignees; repo has 0 PRs (open or closed); comment thread is empty"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Bug opened by a COLLABORATOR, labeled bug/good first issue/rag/tier-1; one bounded fix in output_parser.py plus removing the H-02 xfail marker; no linked PRs"},
      {"name": "ai-policy-ok", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no statement on AI; conditions are CI green, xfail removal, Conventional Commits"},
      {"name": "fresh-issue", "grade": "pass", "evidence": "Opened 2026-09-10, 10 days before today"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "maintainer-responds", "grade": "pass", "evidence": "Opened by Aburke225 (COLLABORATOR); last 5 main commits are by human Andrew Burke, newest 2026-09-16 (4 days ago)"},
      {"name": "repo-active", "grade": "pass", "evidence": "archived: false; pushed_at 2026-09-16T21:50:20Z, within 90 days of 2026-09-20"},
      {"name": "not-claimed", "grade": "pass", "evidence": "No assignees; repo has 0 PRs; one claim comment by acordero4852 (NONE) on 2026-09-19, which the Path Review house rule says does not block"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Bug opened by a COLLABORATOR, labeled bug/good first issue/rag/tier-1; one bounded fix in keyword_search.py plus removing the H-01 xfail marker"},
      {"name": "ai-policy-ok", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no statement on AI; only workflow conditions"},
      {"name": "fresh-issue", "grade": "pass", "evidence": "Opened 2026-09-10, 10 days before today"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "maintainer-responds", "grade": "pass", "evidence": "Opened by Aburke225 (COLLABORATOR); last 5 main commits are by human Andrew Burke, newest 2026-09-16 (4 days ago)"},
      {"name": "repo-active", "grade": "pass", "evidence": "archived: false; pushed_at 2026-09-16T21:50:20Z, within 90 days of 2026-09-20"},
      {"name": "not-claimed", "grade": "pass", "evidence": "No assignees; repo has 0 PRs; empty comment thread; only timeline refs are commits in two students' fork repos (ai301-coursework), not PRs here"},
      {"name": "scope-fits", "grade": "pass", "evidence": "Bug opened by a COLLABORATOR, labeled bug/good first issue/api/tier-1; one bounded fix in core/security.py (catch UnknownHashError, return False) plus removing the H-05 xfail marker"},
      {"name": "ai-policy-ok", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no statement on AI; only workflow conditions"},
      {"name": "fresh-issue", "grade": "pass", "evidence": "Opened 2026-09-10, 10 days before today"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

**Run history**

1. 2/3 — `--limit 3` smoke run with the first rubric (issue-01 rejected on maintainer-responds and scope-fits)
2. 3/3 — `--limit 3` smoke run after revising those two checks
3. 19/20 — first full run (issue-20 accepted, gold reject)
4. 19/20 — second full run after tightening scope-fits for unacknowledged feature requests and bot-opened issues (issue-20 fixed; issue-15 accepted, gold reject)
5. 20/20 — final full run after adding the abandoned-attempts condition to scope-fits; this is the run committed as `eval-run.txt` (`agreement: 20/20 scored items  (bar: 18/20: PASS)`)

**Issue analysis**

issue-01 (conda/conda#16475, adding permanent docs for installing PyPI packages with `conda install`). My final rubric says accept; gold says accept. On my first smoke run it was a reject, and both failing checks were my wording, not the label.

maintainer-responds failed because I had written it as response time only. conda's five sampled issues had one maintainer reply at 32.9 days and four with none, and the opener was a CONTRIBUTOR, so the check saw nothing — even though the repo had five human commits in the two days before capture and a release five days old. My group's own definition of "maintainer alive" in class included commits and releases, so I added a human default-branch commit within 30 days as a passing signal.

scope-fits failed because the issue asks for a new docs page plus updates to three or four existing pages, and my first wording said "one bounded change that names a specific file" and "not a list of sub-items." That read a detailed spec as an umbrella. The evidence guide's test is narrower — fail only when the issue is explicitly a tracking issue meant to be split into separate PRs — so I rewrote the check to say that, and to treat a long, detailed spec as evidence of bounded scope rather than against it. After those two changes the rubric accepts it for the right reason: one coherent docs change with a clear end state, in a repo that is clearly alive.

**Check rationale**

Quoted as it is currently written in `tools/issue-select/rubric.md` (the full `scope-fits` table row):

```
| scope-fits | The issue header (the opener's author association and the labels line), the issue body, the full comment thread, and the "linked PRs:" line under Repo facts | The issue describes one coherent piece of work with a clear end state, even if it touches several files. Fail only if one of these is true: the issue is explicitly an umbrella or tracking issue whose sub-items are meant to become separate PRs; it is a usage or support question; the thread shows the design is still being debated with no maintainer decision; a maintainer states the fix touches core internals; the issue is a new-feature request that no maintainer has acknowledged (not opened by an OWNER, MEMBER, or COLLABORATOR, no comment from one, and no label applied), because silence on a feature is not a settled design; the issue was opened by a bot account (opener name ending in [bot]); or the issue has 2 or more closed, unmerged linked PRs, because repeated abandoned attempts mean the work is harder than the label says. A terse body still passes if the work asked for is bounded, and a long, detailed spec counts as evidence of bounded scope | required |
```

This is the check I revised the most — three times, each time because the harness showed me a specific issue my wording got wrong.

It started as "one bounded change naming a specific file, not an umbrella list, no live design debate, no maintainer saying core internals." issue-01 showed that "not a list of sub-items" rejected a well-specified multi-file docs change, so I narrowed it to *explicitly* umbrella or tracking issues and made a long spec count for scope, not against it.

issue-20 (excalidraw, "add company logo shape to the toolbar," opened by cursor[bot], no maintainer comment, no labels) showed the gap on the other side: my "design still being debated" clause needs a debate to fire, and this issue had silence, so it passed. I added two conditions — a new-feature request that nobody with merge rights has acknowledged fails, and a bot-opened issue fails — because silence on a feature is not a settled design.

issue-15 (zulip, open since 2021, 97 comments, two closed unmerged PRs) rejected on one full run and accepted on the next with no change to anything that touched it; the grader was inferring "unsettled" from the thread on one run and not the other. I added "2 or more closed, unmerged linked PRs" so that case is a stated condition instead of an inference.

Two things I considered and rejected: failing on issue age alone, which would have sunk issue-09, a 2018 conda issue that gold accepts; and failing any multi-file change, which is exactly what broke issue-01 in the first place.

**Trade-offs**

Two issues changed result because of this check: issue-20 went from accept to reject after the feature-request and bot-opener conditions, and issue-15 went from accept to reject after the abandoned-attempts condition.

Nothing else moved, and I know because I checked before each re-run rather than after. Before the second revision I looked at the header line of every issue my rubric accepted (using Claude Code to pull the lines) — all eight were either opened by a COLLABORATOR or MEMBER or carried at least one label, so the new conditions could only touch issue-20. Before the third revision I looked at the linked-PRs line of the same eight — only issue-09 had a closed linked PR, and exactly one, so a threshold of two could only touch issue-15. The final full run confirmed it: 20/20, every category matched.

What the check gives up: a feature request that a maintainer labeled but never actually decided on will pass, and an issue with exactly one abandoned PR that is nonetheless too hard will pass. I accept both. The first would need me to read intent into a label, and the second would sink real first issues like issue-09.

---

## Selection rationale

**Selection rationale**

1. Fit and time. #69 is a crash in `rag/generator/output_parser.py` when the model returns a top-level JSON array instead of an object. It is pure Python, in the RAG code I said in my fit profile I want to get better at, and it comes with an xfail test to un-mark, so I will know when I am done. The skill estimated 2–4 hours, which fits a weekend session around my weekday job.

2. What the verdict got right, and what I weighed beyond it. The rubric was right on every check: the repo is alive (human commits four days before I ran it), nobody is assigned, no PR references the issue, it is one bounded bug opened by a collaborator, and CONTRIBUTING.md has no AI ban. All three candidates passed, so the choice came down to things the rubric cannot see: #68 already has a classmate's claim comment on it, and #72 is auth plumbing in `core/security.py` — the smallest job, but not the part of the codebase I want to learn. #69 had a completely empty thread and is the one I would actually choose to read the code for.

3. Difficulty in claiming it. Path Review is a classroom and the house rule says other students' claims do not block, so I cannot be locked out — but that cuts both ways: by the time I post my claim in Unit 2, several classmates may be on the same issue, and my fix has to be my own rather than a copy of someone else's open PR. I have not commented on the issue this week; the claim comment is Unit 2's job.

---

Related paths: `eval-run.txt` in this directory; your skill's files in `tools/issue-select/`.
