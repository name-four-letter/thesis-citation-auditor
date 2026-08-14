# Thesis Citation Auditor

논문 초안에서 인용이 필요할 가능성이 높은 주장을 탐지하고, 기존 참고문헌 PDF가 실제로 주장을 뒷받침하는지 검증하는 Codex 기반 도구다.

## 주요 기능

- DOCX, Markdown, TXT 초안 지원
- 인용 필요도 점수와 탐지 이유 표시
- 논문 목적·구성·모호한 전환 문장 보수적 제외
- 기존 번호 인용과 PDF를 DOI·제목으로 연결
- `직접 근거 / 부분 근거 / 근거 불일치 / 판단 자료 부족` 판정
- 관련 원문 구절과 페이지 표시
- 부족한 근거의 로컬 재검색 및 합법적 외부 원문 탐색
- 수정 문장과 참고문헌 번호 변경안 제안
- 사용자 승인 전 초안 미수정

검색 결과는 근거 후보일 뿐이다. 최종 의미 판정과 참고문헌 채택에는 원문 확인이 필요하다.

## 실행

Python 패키지 `pdfplumber`, `python-docx`가 필요하다.

```powershell
python -m pip install pdfplumber python-docx
```

초안과 레퍼런스는 임의의 위치에 둘 수 있다.

```powershell
python thesis_assistant.py index `
  --papers-dir "C:\path\to\references" `
  --output-dir "C:\path\to\citation_audit_output"

python thesis_assistant.py prepare-ai "C:\path\to\draft.docx" `
  --papers-dir "C:\path\to\references" `
  --output-dir "C:\path\to\citation_audit_output"

python thesis_assistant.py scan "C:\path\to\draft.docx" `
  --papers-dir "C:\path\to\references" `
  --output-dir "C:\path\to\citation_audit_output"
```

## Codex 스킬

`skill/thesis-citation-auditor` 폴더를 개인 Codex 스킬 폴더에 복사한 뒤 새 작업에서 호출한다.

```text
$thesis-citation-auditor 논문 초안의 인용 필요 문장과 기존 각주를 검증해줘.
```

스킬은 작업 시작 시 다음 두 경로를 확인한다.

1. 작성 중인 논문 파일
2. 레퍼런스 PDF 폴더

## 테스트

```powershell
python -m unittest -v test_assistant.py
```

## 주의사항

- 유료벽 우회 경로를 사용하지 않는다.
- 공개 원문을 확보하지 못한 자료는 검증 완료로 처리하지 않는다.
- 논문 PDF, 작성 중인 초안, 생성 보고서는 저장소에 포함하지 않는다.
- 인용 필요도 점수는 확률이 아니라 검토 우선순위를 위한 규칙 기반 점수다.
