---
name: thesis-citation-auditor
description: Audit thesis or academic drafts in DOCX, Markdown, or text; identify externally verifiable claims that may need citations, score citation necessity, verify existing numbered references against local PDF full text, find lawful external full text when local evidence is insufficient, and propose evidence-backed revisions and reference renumbering. Use for 졸업논문, 논문 각주 검증, reference 확인, 인용 필요 문장 탐지, 근거 논문 검색, or citation audit requests.
---

# Thesis Citation Auditor

Use the existing runner at `C:\Users\jinu\Desktop\AI\thesis_assistant\thesis_assistant.py`.
Treat its output as retrieval material, not as the final semantic judgment.

## Inputs

- At the start of every invocation, obtain and confirm these two paths before auditing:
  1. the draft file (`.docx`, `.md`, or `.txt`);
  2. the folder containing reference PDF files.
- If either path is already explicit in the user's message, do not ask for it again. Resolve and verify it directly.
- Derive the output folder as `<draft-folder>\citation_audit_output` unless the user specifies another location.
- Preserve source files. Never require users to copy them into a fixed project directory.
- For DOCX or PDF inspection, use the corresponding document/PDF skill and follow its verification workflow.

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
10. Propose a minimally revised sentence. If inserting a new numbered reference at `[n]`, shift the existing `[n]` and later references instead of appending an arbitrary final number.
11. Generate the scan report and run tests. Require zero pending verdicts for the selected claims, valid UTF-8, and a clean test run.

## Reporting Rules

- Show only user-relevant fields: claim, existing citation, citation-necessity score and reason, verdict, supported/unsupported parts, clean source passage and page, revision, numbering plan, and user decision.
- Keep labels in Korean throughout one report.
- Remove watermarks, navigation text, raw retrieval scores, bibliography-match scores, and unrelated PDF text.
- Do not alter the draft or accept a proposal without the user's decision.
- State limitations plainly; never claim exhaustive detection or fabricate references, page numbers, or performance values.

## Commands

Use the bundled workspace Python when available:

```powershell
$py = 'C:\Users\jinu\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe'
& $py 'C:\Users\jinu\Desktop\AI\thesis_assistant\thesis_assistant.py' index --papers-dir '<reference-folder>' --output-dir '<output-folder>'
& $py 'C:\Users\jinu\Desktop\AI\thesis_assistant\thesis_assistant.py' prepare-ai '<draft-path>' --papers-dir '<reference-folder>' --output-dir '<output-folder>'
& $py 'C:\Users\jinu\Desktop\AI\thesis_assistant\thesis_assistant.py' scan '<draft-path>' --papers-dir '<reference-folder>' --output-dir '<output-folder>'
& $py -m unittest -v 'C:\Users\jinu\Desktop\AI\thesis_assistant\test_assistant.py'
```
