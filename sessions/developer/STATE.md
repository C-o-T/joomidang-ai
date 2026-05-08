# developer 상태 파일

## 현재 상태

대기 중 — 2026-05-08 작업 완료

## 완료한 작업 이력

| 날짜 | 작업 | 핵심 결정 |
|------|------|-----------|
| 2026-05-08 | 최초 투입 + arXiv 수집기 구현 | requests 라이브러리 사용 (urllib TLS 재협상 타임아웃 우회), HTTPS 직접 연결 + verify=False |

## 현재 적용 중인 판단 기준

- dataverse mini-pipeline: Python 3.11, 이미 requirements.txt에 있는 라이브러리 우선 사용
- HTTP 헤더: ASCII 전용 (한글 금지 — 이전 버그 재발 방지)
- 반환 형식: wikipedia.py fetch_page()와 동일 키 구조 유지 (domain 키 추가는 호환 유지)
- arXiv API: verify=False 필수 (Windows Python + arXiv Schannel TLS 재협상 이슈)

## chief와 협의한 사항

없음 (최초 세션)

## 다음 작업 예상

- search_ui.py 개선 사항 적용 (chief 동의 후)
- 세 번째 데이터 소스 수집기 추가 가능성
- run_pipeline.py에 arXiv 수집 통합
