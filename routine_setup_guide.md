# 🇰🇷 국산화 TF 주간 대시보드 — Claude Code Routine 설정 가이드

이 파일은 매주 자동 실행되는 Claude Code 루틴의 명세서입니다.  
하나의 HTML 산출물(End Picture)을 Notion 데이터에서 자동 생성해 Google Drive에 업로드하고 GitHub Pages에 배포합니다.  
Weekly Action Review는 End Picture 하단 섹션으로 통합합니다.

---

## §1. Routine 기본 설정

| 항목 | 값 |
|------|-----|
| 이름 | 국산화TF-주간대시보드 |
| 스케줄 | 주간 미팅 일정에 따라 매주 조정 필요 |
| 커넥터 | Notion, Google Drive, humanize-korean |
| 산출물 | `{YYMMDD}_EndPicture.html` |
| 하단 섹션 | End Picture 내 Weekly Action Review 섹션 |
| 업로드 폴더 | `1QKfKMUeiRVbnSRtbe7sAiP7Ses9AXktm` |
| GitHub Pages | https://kunjoopark-jpg.github.io/droneparts_endpicture/ |

---

## §2. 지침 (Prompt)

→ `CLAUDE.md`의 "루틴 프롬프트 (전문)" 참조

---

## §3. 평가 기준 (Quick Reference)

| 단계 | 조건 | 색상 |
|------|------|------|
| 🟢 목표 달성 | (Under Review ≥ 1곳) and (75%+ ≥ 1곳) | green |
| 🟡 검토 진행 | (Under Review ≥ 1곳), but (75%+ 없음) | yellow |
| 🔴 Critical Gap | Under Review = 0 | red |

**Under Review 정의**: 해당 부품군에서 컨택한 업체 수.  
Notion 단계가 NDA, 미팅 완료, 샘플 Inquiry, 샘플 테스트, 공급사 선정 중 하나이고 종료/부적합이 아닌 업체만 포함한다.

**목표 달성 카드 detail**: 부품군과 달성 업체를 함께 적는다. 예: `GPS(비스타컴 75%), 모터(모터이엔지 75%)`. Notion에 없는 정보는 추정하지 않는다.

---

## §4. End Picture 양식 명세

### 4.1 구조 (위에서 아래로)

```
┌─────────────────────────────────────────────────┐
│ [A] Header                                      │
│     🇰🇷 드론 부품 국산화 — End Picture           │
│     서브타이틀 + 기준일 badge (우측)              │
├─────────────────────────────────────────────────┤
│ [B] Summary Strip — 4칸 (평가 기준 카드 내 병합)  │
│  [10] [🟢 목표 달성 N] [🟡 검토 진행 N] [🔴 Gap N] │
│  각 상태 카드 안에 .summary-criteria 점선 줄 표시  │
├─────────────────────────────────────────────────┤
│ [C] Matrix Table                                │
│  부품군 | 평가 | Under Review | 업체1 | 업체2 | 기타 | 결론 │
│  10개 부품군을 한눈에 보는 핵심 테이블             │
├─────────────────────────────────────────────────┤
│ [D] Weekly Action Review                        │
│  전 주 대비 핵심 변동 / 이번 주 action / 다음 주 일정 │
└─────────────────────────────────────────────────┘
```

### 4.2 Summary Strip 카드 4종

| # | 카드 | 클래스 | 숫자 | 디테일 | criteria line |
|---|------|--------|------|--------|---------------|
| 1 | 전체 부품군 | `.total` | 10 | 부품군 목록 나열 | (없음) |
| 2 | 🟢 목표 달성 | `.green` | N | 달성 부품군(업체명 %) | 조건: Under Review ≥ 1 AND 75%+ ≥ 1 |
| 3 | 🟡 검토 진행 | `.yellow` | N | 해당 부품군 목록 | 조건: Under Review ≥ 1, but 75%+ 없음 |
| 4 | 🔴 Critical Gap | `.red` | N | 해당 부품군 목록 | 조건: Under Review = 0 |

카드 구조:
```html
<div class="summary-card green">
  <div class="summary-label">🟢 목표 달성</div>
  <div class="summary-number">N</div>
  <div class="summary-detail">GPS (비스타컴 75%)</div>
  <div class="summary-criteria">조건: Under Review ≥ 1 AND 75%+ ≥ 1</div>
</div>
```

**규칙**:
- 별도의 Criteria 패널 블록을 만들지 않는다. 평가 기준은 각 카드 내 `.summary-criteria` 점선 줄로만 표시한다.
- 별도의 ★ 75%+ 업체 카드는 만들지 않는다.
- 카드 detail은 한 줄 또는 최대 두 줄로 제한한다.
- 카드 색상(green/yellow/red)은 Matrix 평가 pill 색상과 정확히 매칭한다.
- green/yellow 카드에는 gradient 배경(`linear-gradient(180deg, {color-bg} 0%, var(--surface) 60%)`)을 적용한다.

### 4.3 Header 구조

```html
<header class="header">
  <div class="header-title">
    <h1>🇰🇷 드론 부품 국산화 — End Picture</h1>
    <div class="subtitle">Bone Robotics · TF Weekly Dashboard · {N}월 {N}주차 (W{NN})</div>
  </div>
  <div class="header-badge">
    <span class="badge-label">기준일</span>
    <span class="badge-date">YYYY-MM-DD</span>
  </div>
</header>
```

### 4.4 Matrix Table 컬럼 명세

정본은 아래 7컬럼 단일 표다.

| 컬럼 | 너비 | 내용 |
|------|------|------|
| 부품군 | 118px | 아이콘 + 명칭 + 좌측 4px 상태 strip (`.cat-strip.green/yellow/red`) |
| 평가 | 92px | 🟢/🟡/🔴 pill. Summary 카드 색상과 동일 |
| Under Review | 90px | 컨택한 업체 수. 큰 숫자(`.review-num`) + "컨택 업체" label |
| 업체 1 (Best) | flex | 최우선 업체 vendor-line + vendor-note 1줄 |
| 업체 2 | flex | 차순위 업체 vendor-line + vendor-note 1줄 |
| 기타 파이프라인 | flex | 리드/종료/부적합 등 보조 정보. 최대 2개 제한, 과밀하면 "+N" 요약 |
| 결론 / 리스크 | 240px | 1~2줄. `.conclusion` + `.risk`/`.next` 클래스 구분 |

**표 가독성 규칙**:
- `thead th`는 `font-weight: 800` 이상으로 Bold 처리한다.
- 한 셀에 여러 문장을 넣지 않는다. vendor-line + vendor-note 1줄을 기본으로 한다.
- Notion에 없는 리스크, 컨택 예정, 담당자, 일정은 쓰지 않는다.
- 업체 1/2에는 실제 컨택 또는 검토 중인 업체만 배치한다.
- 리드 발굴만 있는 업체는 메인 슬롯에 넣지 않고 기타 파이프라인에만 표시한다.
- 종료/부적합 업체는 `.pipeline-item.dead` (취소선 + opacity 0.7)로 낮은 시각 강도에 표시한다.

### 4.5 업체 정보 표시 명세

포맷: `[업체명] | [단계] | [1차 판결 %]`

```html
<div class="vendor-line">
  <span class="v-name">비스타컴</span>
  <span class="v-sep">|</span>
  <span class="v-stage">샘플 테스트</span>
  <span class="v-sep">|</span>
  <span class="v-judgment j75">75%</span>
</div>
<div class="vendor-note">• 핵심 1줄 내용</div>
```

**1차 판결 % 색상 클래스**:

| 클래스 | 조건 | 스타일 |
|--------|------|--------|
| `.j100` | 100% (목표 달성) | green 배경 + green 텍스트 |
| `.j75` | 75% (목표 달성) | gold 배경 + gold 텍스트 + gold border |
| `.j50` | 50% (검토 중) | blue 텍스트 |
| `.j25` | 25% (조건부/보류) | orange 텍스트 |
| `.j0` | 0% (종료/부적합) | red 텍스트 |
| `.jna` | 미기재 | muted 텍스트 |

- 구분자는 반드시 `" | "`(공백 포함)로 통일한다.
- 1차 판결 값이 없으면 `.jna "미기재"`로 표시한다.
- vendor-note: Notion의 '리스크/현황' 1줄 + '다음 액션' 핵심 1줄만 표시한다.

### 4.6 Under Review 셀 시각화

- 명칭은 **Under Review**로 통일한다.
- 의미: 해당 부품군에서 컨택한 업체 수.
- 리드 발굴만 있는 업체는 Under Review 수에 포함하지 않는다.
- 숫자 아래 label은 "컨택 업체"로 표기한다.
- 0인 경우 `.review-num.zero` (muted 색상)으로 표시한다.

### 4.7 결론 / 리스크 셀

```html
<td class="conclusion">
  <span class="risk">리스크 내용 (빨간 굵은 텍스트)</span>
  <span class="next">다음 판단 포인트 (dim 텍스트)</span>
</td>
```

- 1~2줄로 제한.
- `.risk` = 빨간 굵은 텍스트, `.next` = dim 텍스트.

### 4.8 하단 Weekly Action Review 구성

Sub-Header: **Weekly Action Review**

포함 섹션은 세 개만 허용한다:
1. 전 주 대비 핵심 변동 사항
2. 해당 주의 주요 action item
3. 다음 주에 다가오는 일정

나머지 평가 기준 반복, 중복 요약, 별도 캘린더, 별도 오픈 액션 테이블은 삭제한다.

**WAR tag 종류 (`.tag` 클래스)**:

| 클래스 | 의미 | 색상 |
|--------|------|------|
| `.tag.new` | 신규 진입 | blue |
| `.tag.carry` | 전 주에서 이월 (오픈 액션 유지) | gray |
| `.tag.status` | 상태 변경 (예정→진행 중 등) | yellow |
| `.tag.dead` | 종료/부적합 확정 | red (opacity 0.85) |
| `.tag.schedule` | 다음 주 예정 일정 | purple (white 텍스트) |

---

## §5. Weekly Action Review 섹션 명세

이 섹션은 별도 HTML 파일이 아니라 End Picture 하단에 포함한다.

### 5.1 Sub-Header

- Sub-Header 텍스트: **Weekly Action Review**
- End Picture 매트릭스 아래, Footer Note 위에 배치
- 별도의 WeeklyActionReview HTML 파일은 생성하지 않음

### 5.2 포함할 하단 섹션 3종

**1. 전 주 대비 핵심 변동 사항**
- 오픈 액션 = 상태가 완료가 아닌 항목 (예정, 진행 중, 홀드)
- 현재 오픈 액션에는 있지만 전 주 목록에 없던 항목 → NEW
- 전 주에도 있었고 현재도 완료되지 않은 항목 → Carry
- 전 주 대비 상태가 바뀐 항목 → 상태 변경
- 종료/부적합 확정 항목 → DEL
- 완료된 항목은 기본적으로 표시하지 않는다.
- 형식: `[tag] [마감일] 활동명 — 상태 / sub-line: summary 1줄`
- 비교 가능한 전 주 데이터가 없으면 "비교 기준 없음"으로 표시한다.

**2. 해당 주의 주요 action item**
- 액션 아이템 DB에 있는 이번 주 핵심 액션만 표시
- 형식: `[마감일] 업체/부품군 — 액션명 — 상태/담당(있을 때만)`
- 담당, 마감일, 다음 스텝이 Notion에 없으면 임의 작성하지 않음

**3. 다음 주에 다가오는 일정**
- 다음 주에 마감 또는 진행 예정인 Notion 액션만 표시
- `.tag.schedule` (purple) 태그 사용
- 컨택할 예정이라는 추정성 문장은 금지
- 일정이 없으면 'Notion 기준 등록된 다음 주 일정 없음'으로 표시

### 5.3 제거할 항목

- 별도 Weekly Action Review HTML 파일
- 평가 기준 재확인 문장
- 중복 수치 요약
- Notion에 없는 다음 스텝/컨택 예정/외부 추론

---

## §6. 디자인 시스템 (CSS 변수)

Light Theme 확정. 어두운 테마 사용 금지. Vercel 디자인 시스템 참조 적용.

```css
:root {
  /* ── Surface ── */
  --bg: #fafafa;          /* page background */
  --surface: #ffffff;     /* card / table */
  --surface2: #f5f5f5;    /* inset region */
  --surface3: #ebebeb;    /* subtle divider fill */

  /* ── Border (hairline 방식) ── */
  --border: #ebebeb;
  --border-strong: #d4d4d4;

  /* ── Text ── */
  --text: #171717;        /* ink — primary */
  --text-dim: #4d4d4d;    /* body */
  --text-muted: #888888;  /* caption / label */

  /* ── Semantic — 상태 표시 전용, 변경 금지 ── */
  --green: #0f9d58;   --green-soft: #34c77c;  --green-bg: #e6f7ee;
  --yellow: #d97706;  --yellow-soft: #f59e0b; --yellow-bg: #fff4e0;
  --red: #dc2626;     --red-soft: #ef4444;    --red-bg: #fde7e7;
  --blue: #1d6fed;    --blue-soft: #3b82f6;   --blue-bg: #e3edff;
  --purple: #7c3aed;
  --orange: #ea580c;
  --accent: #171717;  /* CTA — ink */

  /* ── Gold: 75%+ 업체 j75 강조 전용 ── */
  --gold: #c47e00;
  --gold-bright: #e8a200;
  --gold-bg: #fff4d6;
  --gold-border: #e8c870;

  /* ── Elevation (stacked shadow) ── */
  --shadow-1: 0 1px 1px rgba(0,0,0,.03), 0 2px 2px rgba(0,0,0,.04);
  --shadow-2: 0 1px 1px rgba(0,0,0,.03), 0 2px 4px rgba(0,0,0,.05), 0 4px 8px rgba(0,0,0,.04);

  /* ── Radius ── */
  --radius-sm: 6px;   /* pill / badge */
  --radius-md: 8px;   /* card */
  --radius-lg: 12px;  /* table wrapper */
}
```

**폰트**:
- 본문: `'Inter', 'Noto Sans KR', sans-serif` (400/500/600) — Inter 우선, 한글 Noto KR 폴백
- 수치/코드/vendor-line: `'JetBrains Mono', monospace` (400/500/600)
- Google Fonts CDN: `Inter` + `Noto Sans KR` + `JetBrains Mono`
- Display 텍스트(h1): `font-weight: 600`, `letter-spacing: -0.5px`
- 본문: `font-weight: 400`, `letter-spacing: -0.1px`

**공통 레이아웃**:
- body padding: `32px 40px`
- font-size: `13.5px`, line-height: `1.6`
- card/table wrapper: `border-radius: var(--radius-lg)`, `box-shadow: var(--shadow-1)`
- pill/badge: `border-radius: var(--radius-sm)`
- Table header/th: `font-weight: 600`, `letter-spacing: 0.3px`, `text-transform: uppercase`, `font-size: 11px`
- border는 `1px solid var(--border)` 단일 hairline 방식 통일
- Summary/Matrix의 green/yellow/red 의미와 색상은 항상 동일하게 매칭

---

## §7. 설정 방법

### Cloud Routine (권장)

1. `claude.ai/code/routines` 접속 → New routine
2. `CLAUDE.md`의 루틴 프롬프트 전문을 Prompt 필드에 붙여넣기
3. Connectors: Notion + Google Drive 연결
4. Trigger: Schedule → 주간 미팅 일정에 맞춰 조정
5. Save & Enable

### Desktop Local Routine

1. Claude Code Desktop → Routines → + New routine → Local
2. 동일 지침 붙여넣기
3. Schedule: Weekly · 미팅 전날 · 설정 시각
4. Permission mode: Auto

---

## §8. 평가 기준 변경 이력

| 버전 | 기준 | 비고 |
|------|------|------|
| v1 | Active ≥ 2 (단순 수량) | 초기 |
| v2 | Active ≥ 2 AND 75%+ ≥ 1 | 너무 엄격 |
| v3 | Active ≥ 2 OR 75%+ ≥ 1 | OR 조건 |
| v4 (현재) | (Under Review ≥ 1) AND (75%+ ≥ 1) + Under Review 수(컨택 업체 수) 세부 지표 | 직관성·확장성 |

---

## §9. 유지보수

| 상황 | 대응 |
|------|------|
| 부품군 추가/삭제 | `CLAUDE.md` + §3 표 + §4.1 구조 모두 수정 |
| Notion 스키마 변경 (필드/옵션) | `CLAUDE.md` 1-A 필드 목록 수정 |
| Drive 폴더 변경 | §1 표 + `CLAUDE.md` STEP 5 폴더 ID 수정 |
| 평가 기준 변경 | `CLAUDE.md` 2-B + §3 + §8 이력 수정 (세 곳 모두) |
| 디자인 변경 | §6 CSS 변수 + §4·§5 양식 명세 수정. 기존 산출물을 reference로 Drive에 보관 |
| 신규 산출물 추가 | §1·`CLAUDE.md` STEP 추가 + §4·§5 같은 형식으로 새 양식 명세 작성 |
| GitHub Pages 반영 안 됨 | git push 인증 계정 확인 (kunjoopark-jpg). 자격증명 캐시 초기화 후 재시도 |

---

## §10. 검증 (수동)

루틴 실행 후 Drive 폴더에서 End Picture 산출물을 열어 다음을 점검:

- [ ] `{YYMMDD}_EndPicture.html` 한 개만 생성되었는가
- [ ] 별도 WeeklyActionReview HTML 파일이 생성되지 않았는가
- [ ] 부품군 10개가 모두 매트릭스에 표시되었는가
- [ ] Notion에 없는 업체·컨택 예정·담당자·일정이 임의로 추가되지 않았는가
- [ ] Summary Strip은 4개 카드만 표시되는가
- [ ] 각 카드 하단에 `.summary-criteria` 점선 줄이 있는가
- [ ] 목표 달성 카드 detail에 부품군(업체명 75%/100%) 형식으로 표시되는가
- [ ] 별도 Criteria 패널 블록이 없는가
- [ ] 별도 75%+ 업체 카드가 없는가
- [ ] Under Review 수가 실제 컨택한 업체 수와 일치하는가
- [ ] Under Review label이 "컨택 업체"로 표기되는가
- [ ] Matrix Table이 7컬럼 구조인가 (부품군/평가/Under Review/업체1/업체2/기타/결론)
- [ ] Matrix Table thead가 Bold(font-weight 800)인가
- [ ] vendor-line 포맷이 `[업체명] | [단계] | [%]`로 통일되었는가
- [ ] 75%+ 업체에 `.j75` gold 스타일이 적용되었는가
- [ ] 종료/부적합 업체가 `.pipeline-item.dead` (취소선)로 표시되는가
- [ ] 하단 Weekly Action Review에는 세 섹션만 있는가
- [ ] WAR tag가 new/carry/status/dead/schedule 5종 내에서 사용되었는가
- [ ] GitHub Pages에 최신 내용이 반영되었는가
