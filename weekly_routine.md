# 국산화 TF 주간 루틴

매주 목요일 미팅 전, Cowork에서 아래 프롬프트를 실행합니다.

---

## 실행 프롬프트

```
국산화 TF 주간 대시보드 루틴을 실행합니다.

## STEP 1: Notion 데이터 수집
아래 3개 DB에서 최신 데이터를 전부 읽어주세요.
- 업체 DB: collection://333da05a-c3f4-8015-85ec-000b4ff36c0c
- 🧪 샘플테스트 DB: collection://0a09a358-d7a4-46f6-b457-858e425b57fc
- 🎬 액션 아이템 DB: collection://333da05a-c3f4-80cd-a829-000b0e483b8d

## STEP 2: End Picture HTML 생성
GitHub 레포의 가이드 파일을 참조하여 HTML을 생성해주세요.
- 레포: https://github.com/kunjoopark-jpg/droneparts_endpicture
- CLAUDE.md: 에이전트 지침 (데이터 원칙, 실행 순서)
- routine_setup_guide.md: HTML 양식 명세 (§3 평가기준, §4 구조, §5 WAR, §6 디자인)
- 파일명: {YYMMDD}_EndPicture.html
- index.html로도 복사하여 GitHub Pages 배포

## STEP 3: 주간 변동 + Next Plan 정리
STEP 1에서 수집한 데이터를 바탕으로 아래 내용을 정리해주세요.

### 3-A. 전 주 대비 변동 사항
- 업체 DB: 단계/판정 변경된 벤더
- 샘플테스트 DB: 상태/결과 변경된 항목
- 액션 아이템: 완료된 것, 새로 생긴 것

### 3-B. 이번 주 주요 Action
- 마감일이 이번 주인 액션 아이템
- 테스트 예정/진행 중인 샘플

### 3-C. Next Plan (다음 주 이후)
- 입고 예정 샘플
- 예정된 미팅/방문
- 판정 대기 중인 벤더

### 3-D. 공유 아젠다 (미팅용)
위 3-A ~ 3-C를 바탕으로 팀 미팅 아젠다를 작성해주세요.
- Slack #5-1-temporary-communication 채널에 보낼 수 있는 형태로
- 드래프트로 먼저 보여주세요. 내가 수정한 뒤 발송 지시합니다.

## STEP 4: GitHub Push
생성된 HTML 파일을 GitHub에 push해주세요.
- 레포: kunjoopark-jpg/droneparts_endpicture
- index.html + {YYMMDD}_EndPicture.html
- 커밋 메시지: "{YYMMDD} EP"

## STEP 5: Google Drive 업로드
{YYMMDD}_EndPicture.html을 아래 폴더에 업로드해주세요.
- 폴더 ID: 1QKfKMUeiRVbnSRtbe7sAiP7Ses9AXktm
```

---

## 산출물 체크리스트

| # | 산출물 | 확인 |
|---|---|---|
| 1 | End Picture HTML 생성 | ☐ |
| 2 | GitHub Pages 배포 (index.html push) | ☐ |
| 3 | Google Drive 업로드 | ☐ |
| 4 | 주간 변동 + Next Plan 정리 | ☐ |
| 5 | 미팅 아젠다 Slack 드래프트 | ☐ |

---

## 참고 링크

- Notion TF 페이지: https://app.notion.com/p/TF-333da05ac3f481e9b967dd540c715cfe
- GitHub Pages: https://kunjoopark-jpg.github.io/droneparts_endpicture/
- GitHub 레포: https://github.com/kunjoopark-jpg/droneparts_endpicture
- Drive (샘플테스트 데이터): https://drive.google.com/drive/folders/1Vu3V8p8ZiT1-vwcwN0FF-PpgywjSSFWI
- Drive (EP 업로드): https://drive.google.com/drive/folders/1QKfKMUeiRVbnSRtbe7sAiP7Ses9AXktm
- Slack 채널: #5-1-temporary-communication (C0AE96F8J8J)
