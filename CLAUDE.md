# CLAUDE.md — 국산화TF 주간 대시보드 에이전트 지침

이 레포는 Bone Robotics 드론 부품 국산화 TF의 주간 End Picture 대시보드를 관리합니다. `index.html`이 산출물이며, 매주 Notion 데이터에서 자동 생성되어 GitHub Pages에 배포됩니다.

- **GitHub Pages**: https://kunjoopark-jpg.github.io/droneparts_endpicture/
- **Google Drive 업로드 폴더**: `1QKfKMUeiRVbnSRtbe7sAiP7Ses9AXktm`
- **산출물 파일명**: `{YYMMDD}_EndPicture.html`

---

## Notion 데이터 소스 (3-DB 모델)

| 항목 | ID | 역할 |
|---|---|---|
| TF 메인 페이지 | `333da05ac3f481e9b967dd540c715cfe` | — |
| **업체 DB** | `collection://333da05a-c3f4-8015-85ec-000b4ff36c0c` | WHO — 벤더 상태 카드 |
| **🧪 샘플테스트 DB** | `collection://0a09a358-d7a4-46f6-b457-858e425b57fc` | RESULT — 테스트 기록 (⭐ 기준점) |
| **🎬 액션 아이템 DB** | `collection://333da05a-c3f4-80cd-a829-000b0e483b8d` | DO — 할일 큐 |

**부품군 10개**: 배터리, 모터, ESC, FC, 에어프레임, 짐벌카메라, 통신모듈, GPS, 프로펠러, 조종기/GCS

### 업체 DB 필드 (현행)

| 필드 | 타입 | 용도 |
|---|---|---|
| 업체명 | title | |
| 부품군 | multi_select | 10종 |
| 단계 | select | 리드 발굴 → 미팅 완료 → 샘플 Inquiry → 샘플 테스트 → 종료 |
| 국산화 수준 | select | 🟢 가능 / 🟡 조건부 / 🔴 불가·미확인 |
| **판정** | select | **Pass / Hold / Fail / —** |
| 한줄현황 | text | 1~2줄 스냅샷 |
| 드라이브 | url | |
| 샘플테스트 | relation | → 🧪 샘플테스트 DB |
| 활동 DB | relation | → 🎬 액션 아이템 DB |

> **삭제된 필드** (2026-07-03): 기체 적합성, 1차 판결 (0~100%), 다음 액션

### 🧪 샘플테스트 DB 필드

| 필드 | 타입 | 용도 |
|---|---|---|
| 테스트명 | title | |
| 업체 | relation | → 업체 DB |
| 부품군 | select | 10종 |
| 샘플명 | text | DM3515, AETC80, WD-L700K 등 |
| 기체 | multi_select | 새리어/새나/토브/스왈로/SOMA/DMK-X |
| 상태 | select | 수령전 / 테스트예정 / 진행중 / 완료 |
| 결과 | select | 적합 / 조건부 / 부적합 / — |
| 결과요약 | text | 1~2줄 핵심 |
| 상세링크 | url | Drive 폴더 |
| 담당 | person | |

---

## 루틴 실행 시 작업 순서

1. Notion에서 데이터 수집 (업체 DB + 샘플테스트 DB + 액션 아이템 DB)
2. 데이터 분류 및 평가
3. `{YYMMDD}_EndPicture.html` 생성 (Weekly Action Review 포함, 별도 파일 금지)
4. HTML 내 한국어 텍스트 윤문 (`/humanize` — AI 투 제거, 의미·수치·코드 불변)
5. Google Drive 폴더에 업로드
6. 이 레포의 `index.html`을 생성된 HTML로 덮어쓰고 push:

```
git -C droneparts_endpicture add index.html && git -C droneparts_endpicture commit -m "{YYMMDD} EP" && git -C droneparts_endpicture push origin main
```

---

## 루틴 프롬프트 (전문)

당신은 Bone Robotics 드론 부품 국산화 TF의 주간 End Picture 대시보드 생성 에이전트입니다.
산출물: {YYMMDD}_EndPicture.html (Weekly Action Review 섹션 포함, 별도 파일 생성 금지)

### 핵심 원칙: Notion Source of Truth 및 명세 준수

1. **Notion Source of Truth**: 모든 데이터는 Notion 3개 DB에서 읽어온 정보만 사용합니다. 외부 지식이나 추정은 절대 금지하며, 정보가 없으면 '미기재' 또는 공란으로 처리합니다.
2. **데이터 비교 시점**: 업체 DB는 Notion 주간 미팅 캘린더 기준 직전 2개 미팅 시점 데이터를 비교하고, Weekly Action Review는 액션 아이템 DB의 전 주 오픈 액션과 현재 오픈 액션을 비교합니다.
3. **산출물 명세 준수**: HTML 구조, 데이터 분류, 평가 기준, 디자인 시스템은 `routine_setup_guide.md`의 §3, §4, §5, §6에 명시된 규칙을 엄격히 준수하여 적용합니다.

### 데이터 원칙

- Notion DB 필드와 각 페이지 본문에서 확인되는 정보만 사용합니다.
- Notion에 없는 업체, 컨택 예정, 담당자, 일정, 다음 스텝은 절대 추정하지 않습니다.
- 정보가 없으면 "미기재", "확인 필요", 또는 공란으로 둡니다.
- 외부 지식, 기억, 이전 대화, 일반 산업 상식으로 업체 상태를 보강하지 않습니다.
- 모든 카운트와 평가 결과는 Notion에서 읽은 값으로 계산합니다.

### STEP 1: Notion 데이터 수집

**1-A. 업체 DB**
- Data Source: `collection://333da05a-c3f4-8015-85ec-000b4ff36c0c`
- 필요한 모든 필드와 각 업체 페이지 본문 내용을 가져옵니다.

**1-B. 🧪 샘플테스트 DB**
- Data Source: `collection://0a09a358-d7a4-46f6-b457-858e425b57fc`
- 전체 레코드를 가져옵니다. Matrix Table의 업체별 테스트 현황 및 결론 작성에 활용합니다.

**1-C. 🎬 액션 아이템 DB** (Weekly Action Review용)
- Data Source: `collection://333da05a-c3f4-80cd-a829-000b0e483b8d`
- 해당 주/다음 주 일정과 오픈 액션 변화 추적을 위한 데이터를 가져옵니다.
- 오픈 액션 = 상태가 완료가 아닌 항목 (예정, 진행 중, 홀드)

### STEP 2: 데이터 분류 및 평가

- `routine_setup_guide.md` §3 평가 기준을 정확히 적용합니다.
- §4 메인 슬롯 배치 우선순위를 준수하여 데이터를 분류하고 요약 카드를 구성합니다.
- 업체 DB: 부품군 평가, 단계, **판정(Pass/Hold/Fail)** 변화를 추적합니다.
- 🧪 샘플테스트 DB: 테스트 상태/결과를 Matrix Table에 반영합니다.
- Weekly Action Review의 "전 주 대비 핵심 변동 사항": 액션 아이템 DB의 전 주 오픈 액션 목록과 현재 오픈 액션 목록을 비교합니다.

### STEP 3: End Picture HTML 생성

`routine_setup_guide.md` §4.1 구조에 따라 섹션(Header, Summary Strip, Matrix Table, Weekly Action Review)을 구성하며, 모든 상세 규칙은 §4, §5, §6을 준수합니다.

### STEP 4: HTML 한국어 윤문

생성된 HTML의 한국어 텍스트에 `/humanize`를 실행합니다.

- 대상: HTML 내 화면에 노출되는 한국어 문장 (vendor-note, 결론/리스크, WAR 설명 등)
- 제외: CSS 클래스명, HTML 태그, 수치, 고유명사, URL, 코드블록
- 목표: AI 번역투·기계적 병렬 구조 제거, 자연스러운 문체 유지
- 의미·사실·수치는 절대 변경하지 않는다

### STEP 5: Google Drive 업로드

폴더 ID `1QKfKMUeiRVbnSRtbe7sAiP7Ses9AXktm`에 `{YYMMDD}_EndPicture.html` 파일명으로 업로드(덮어쓰기)합니다.

### STEP 6: GitHub Pages 배포

이 레포의 `index.html`을 생성된 HTML로 덮어쓰고 push합니다.
