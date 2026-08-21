---
name: thesis-citation-auditor
description: Audit thesis or academic drafts in DOCX, Markdown, or text; identify externally verifiable claims that may need citations, score citation necessity, verify existing numbered references against local PDF full text, find lawful external full text when local evidence is insufficient, and propose evidence-backed revisions and reference renumbering. Use for 졸업논문, 논문 각주 검증, reference 확인, 인용 필요 문장 탐지, 근거 논문 검색, or citation audit requests.
---

# Thesis Citation Auditor

Use the repository's bundled `thesis_assistant.py` runner. Resolve the repository root from the installed source or the user's project; never assume a personal absolute path.
Treat its output as retrieval material, not as the final semantic judgment.

## Inputs

- At the start of every invocation, obtain and confirm these two paths before auditing:
  1. the draft file (`.docx`, `.md`, or `.txt`);
  2. the folder containing reference PDF files.
- If either path is already explicit in the user's message, do not ask for it again. Resolve and verify it directly.
- Derive the output folder as `<draft-folder>\citation_audit_output` unless the user specifies another location.
- Preserve source files. Never require users to copy them into a fixed project directory.
- For DOCX or PDF inspection, use the corresponding document/PDF skill and follow its verification workflow.

## Evidence and comparability rules

- Treat AI/external-review feedback and research summaries as issue candidates, not evidence. When they conflict, prefer the cited paper's full text, then other user-supplied primary material, over secondary summaries.
- Verify that each cited value and claim share the relevant measurement context: sample, device, operating condition, material composition, and metric definition. Do not combine a best value from one condition with a separate demonstration as if they describe one result.
- Preserve authors' reported metric, unit, and qualifier. Any cross-study conversion must state its formula and assumptions, be labeled as calculated, and be omitted when the available geometry or conditions do not justify it.
- For figures and tables, verify the caption, the accompanying text, and the cited source together. A figure used for cross-study comparison must state whether it is a correlation, an illustrative capability map, or another limited comparison.

## Revision-priority scoring

- Score every selected claim for **수정 필요도** on a 0–100 scale in addition to citation necessity. This is a triage priority, not a probability or a measure of writing quality.
- Use [revision-priority.md](references/revision-priority.md) for the scoring signals and bands. Base the score on the severity of the evidence or interpretation problem, not on stylistic preference alone.
- When the relevant evidence is unavailable, label the verdict `판단 자료 부족`, give a provisional score with that limitation, and recommend the next source to inspect or lawfully obtain rather than inventing a revision.

## Workflow

1. Verify that the draft exists and the reference folder contains PDFs. Report an empty or inaccessible folder instead of silently substituting another library.
2. Index every PDF in the selected reference folder with `--papers-dir` and keep its index in the selected output folder with `--output-dir`.
3. Generate the review packet for the selected draft using the same path options.
4. Detect externally checkable claims conservatively. Exclude the paper's own purpose, organization, and vague transition sentences. Show the numeric citation-necessity score and reasons.
5. For every existing citation, match the bibliography entry to a PDF by DOI or title, then inspect the cited paper's body. Do not use an abstract's missing citations as a reason to fail an otherwise supported abstract claim.
6. Judge semantic support using the labels in [verdict-schema.md](references/verdict-schema.md). A search hit is only a candidate until the claim and source passage have been compared.
7. If support is partial, isolate each unsupported clause and search other local PDFs.
8. If local evidence is insufficient, try lawful full-text routes: publisher OA, author/lab page, institutional repository or accepted manuscript, then public scholarly versions. Do not use Sci-Hub or bypass access controls.
9. Download lawful full text to the selected reference folder, verify title, authors, DOI, relevant body passage, and page, then re-index. If full text cannot be obtained, label it `원문 미확보·미검증`; do not call it verified.
10. Propose a minimally revised sentence. Before proposing reference renumbering, determine whether the required style orders references by first appearance. When it does, provide one whole-document mapping that updates the abstract, body, tables, figure captions, and bibliography without changing source-to-reference correspondence. Do not renumber merely to sort the bibliography.
11. Generate the scan report and run tests. A scan report may contain unresolved candidates; before calling selected claims verified, resolve each selected claim to a final verdict, confirm valid UTF-8, and require a clean test run.

## Reporting Rules

- Sort findings by 수정 필요도, highest first. For each finding show: 수정 필요도 and reasons, original sentence, existing citation, citation-necessity score and reason, verdict, supported/unsupported parts, clean source passage and page, revision, numbering plan, and user decision.
- Keep labels in Korean throughout one report.
- Remove watermarks, navigation text, raw retrieval scores, bibliography-match scores, and unrelated PDF text.
- Do not alter the draft or accept a proposal without the user's decision. If the user authorizes an edit, preserve non-text DOCX objects such as figures, drawings, equations, and embedded charts while changing text.
- State limitations plainly; never claim exhaustive detection or fabricate references, page numbers, or performance values.

## Commands

Use the bundled workspace Python when available:

```powershell
# Resolve this from the checked-out repository or installed skill package.
$repo = '<repository-root>'
$runner = Join-Path $repo 'thesis_assistant.py'
$py = '<bundled-python-path>'
& $py $runner index --papers-dir '<reference-folder>' --output-dir '<output-folder>'
& $py $runner prepare-ai '<draft-path>' --papers-dir '<reference-folder>' --output-dir '<output-folder>'
& $py $runner scan '<draft-path>' --papers-dir '<reference-folder>' --output-dir '<output-folder>'
& $py -m unittest -v (Join-Path $repo 'test_assistant.py')
```
