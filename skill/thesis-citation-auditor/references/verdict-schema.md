# Verdict schema

- `직접 근거`: The source passage supports every material part of the claim at the same scope and strength.
- `부분 근거`: At least one material clause, qualifier, scope, causal link, or number remains unsupported.
- `근거 불일치`: The source contradicts the claim or discusses a materially different subject.
- `판단 자료 부족`: Full text, relevant passage, or enough context is unavailable.

For `부분 근거`, list supported and unsupported clauses separately and search for the gaps.
For an existing citation, verify the cited source itself before proposing alternatives.
Treat a PDF title/DOI match and a claim-support judgment as separate checks.

Citation-necessity scores are triage heuristics, not probabilities. Give higher priority to numbers,
named studies, mechanisms, causal claims, comparisons, and technical limitations. Penalize authorial
purpose, document organization, and vague transition sentences.
