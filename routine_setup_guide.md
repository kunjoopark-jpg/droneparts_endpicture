# 🇰🇷 국산화 TF 주간 대시보드 — Claude Code Routine 설정 가이드

이 파일은 매주 자동 실행되는 Claude Code 루틴의 명세서입니다.
하나의 HTML 산출물(End Picture)을 Notion 데이터에서 자동 생성해 Google Drive에 업로드하고 GitHub Pages에 배포합니다.
Weekly Action Review는 End Picture 하단 섹션으로 통합합니다.

---

## §1. Routine 기본 설정

| 항목 | 값 |
|---|---|
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

## §3. 판정 체계 및 평가 기준

### 3.1 업체 판정 (업체 DB `판정` 필드)

| 판정 | 의미 | 조건 |
|---|---|---|
| **Pass** | 현 기준 추가 발주 대상 | 테스트 적합 + 국산화 🟢 |
| **Hold** | 추가 확인 필요 | 검증 중 or 조건부 |
| **Fail** | 종료 | 부적합 or 국산화 불가 |
| **—** | 판정 전 | 리드 발굴 / 미팅 완료 단계 |

### 3.2 부품군 평가 (Summary Strip + Matrix 색상)

| 등급 | 조건 | 색상 |
|---|---|---|
| 🟢 목표 달성 | Under Review ≥ 1곳 **AND** Pass ≥ 1곳 | green |
| 🟡 검토 진행 | Under Review ≥ 1곳, but Pass 없음 | yellow |
| 🔴 Critical Gap | Under Review = 0 | red |

**Under Review 정의**: 해당 부품군에서 컨택한 업체 수.
Notion `단계`가 미팅 완료, 샘플 Inquiry, 샘플 테스트 중 하나이고, `판정`이 Fail이 아닌 업체만 포함한다.
리드 발굴 단계는 Under Review에 포함하지 않는다.

**목표 달성 카드 detail**: 부품군과 Pass 업체를 함께 적는다.
예: `GPS (비스타컴 Pass) · 모터 (모터이엔지 Pass)`.
Notion에 없는 정보는 추정하지 않는다.

### 3.3 샘플테스트 데이터 활용

🧪 샘플테스트 DB의 정보는 Matrix Table에서 다음과 같이 활용한다:
- **vendor-note**: 해당 업체의 최신 테스트 레코드에서 `상태`, `결과`, `결과요약` 반영
- **결론/리스크**: 테스트 결과가 있으면 결론에 반영 (예: "LTE 테스트 적합 확인")
- **기체별 테스트 현황**: vendor-note에 기체명 명시 (예: "스왈로 PASS / 토브 테스트 예정")

---

## §4. End Picture 양식 명세

### 4.1 구조 (위에서 아래로)

```
┌─────────────────────────────────────────────────┐
│ [A] Header                                      │
│     🇰🇷 드론 부품 국산화 — End Picture           │
│     서브타이틀 + 기준일 badge (우측)              │
├─────────────────────────────────────────────────┤
│ [B] Summary Strip — 4칸                          │
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
|---|---|---|---|---|---|
| 1 | 전체 부품군 | `.total` | 10 | 부품군 목록 나열 | (없음) |
| 2 | 🟢 목표 달성 | `.green` | N | 달성 부품군(업체명 Pass) | 조건: Under Review ≥ 1 AND Pass ≥ 1 |
| 3 | 🟡 검토 진행 | `.yellow` | N | 해당 부품군 목록 | 조건: Under Review ≥ 1, but Pass 없음 |
| 4 | 🔴 Critical Gap | `.red` | N | 해당 부품군 목록 | 조건: Under Review = 0 |

카드 구조:

```html
<div class="summary-card green">
  <div class="summary-label">🟢 목표 달성</div>
  <div class="summary-number">N</div>
  <div class="summary-detail">GPS (비스타컴 Pass) · 모터 (모터이엔지 Pass)</div>
  <div class="summary-criteria">조건: Under Review ≥ 1 AND Pass ≥ 1</div>
</div>
```

**규칙**:
- 별도의 Criteria 패널 블록을 만들지 않는다. 평가 기준은 각 카드 내 `.summary-criteria` 점선 줄로만 표시한다.
- 카드 detail은 한 줄 또는 최대 두 줄로 제한한다.
- 카드 색상(green/yellow/red)은 Matrix 평가 pill 색상과 정확히 매칭한다.
- green/yellow 카드에는 gradient 배경을 적용한다.

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
|---|---|---|
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
- 판정 Fail 또는 단계 종료인 업체는 기타 파이프라인에 표시하지 않는다 (생략).

### 4.5 업체 정보 표시 명세

포맷: `[업체명] | [단계] | [판정]`

```html
<div class="vendor-line">
  <span class="v-name">비스타컴</span>
  <span class="v-sep">|</span>
  <span class="v-stage">샘플 테스트</span>
  <span class="v-sep">|</span>
  <span class="v-judgment pass">Pass</span>
</div>
<div class="vendor-note">• GPS 수신성능 적합 확인 / 양산 계약 단계</div>
```

**판정 표시 클래스**:

| 판정 | 클래스 | 색상 | 배경 |
|---|---|---|---|
| Pass | `.pass` | green | green-bg |
| Hold | `.hold` | gold (amber) | gold-bg |
| Fail | `.fail` | red | red-bg |
| — | `.pending` | text-muted | 없음 |

**vendor-note 작성 규칙**:
- 🧪 샘플테스트 DB에 해당 업체의 레코드가 있으면, 최신 테스트의 `상태`와 `결과요약`을 반영한다.
- 1줄 이내. "•" prefix.
- Notion에 없는 정보는 추정하지 않는다.

### 4.6 메인 슬롯 배치 우선순위

업체 1(Best)과 업체 2 슬롯에 배치할 업체 선정 기준:

1. **Pass** 업체 최우선
2. **Hold** 업체 중 `단계`가 높은 순 (샘플 테스트 > 샘플 Inquiry > 미팅 완료)
3. 동일 단계면 🧪 샘플테스트 DB에 테스트 레코드가 있는 업체 우선
4. **Fail / —** 업체는 메인 슬롯에 배치하지 않음 (기타 파이프라인으로)

---

## §5. Weekly Action Review 명세

### 5.1 구조

3컬럼 그리드:
- **전 주 대비 핵심 변동 사항**: 태그 (NEW / 상태 변경 / 완료 / Carry) + 날짜 + 이름 + 설명
- **이번 주 주요 Action Item**: 날짜 + 이름 + 상태 + 설명
- **다음 주에 다가오는 일정**: 예정 태그 + 날짜 + 이름 + 설명

### 5.2 태그 클래스

| 태그 | 클래스 | 배경 |
|---|---|---|
| NEW | `.tag.new` | blue-bg |
| 상태 변경 | `.tag.status` | yellow-bg |
| 완료 | `.tag.complete` | green-bg |
| Carry | `.tag.carry` | surface3 |
| 예정 | `.tag.schedule` | purple |

### 5.3 변동 사항 추적 규칙

- **NEW**: 이번 주에 새로 생성된 액션 아이템 또는 샘플테스트 레코드
- **상태 변경**: 단계, 판정, 또는 테스트 상태가 변경된 항목
- **완료**: 이번 주에 완료 처리된 항목
- **Carry**: 전 주에도 오픈이었고 이번 주에도 오픈인 항목

### 5.4 오픈 액션

- 하단 Weekly Action Review에는 전체 오픈 액션 테이블을 만들지 않음
- 해당 주/다음 주 일정에 포함되는 액션만 요약

---

## §6. 디자인 시스템

### 6.1 CSS 변수

```css
:root {
  --bg: #f6f8fb;
  --surface: #ffffff;
  --surface2: #f0f3f8;
  --surface3: #e6ebf2;
  --border: #d8dee8;
  --border-light: #e3e8f0;

  --text: #1a2233;
  --text-dim: #586377;
  --text-muted: #8a94a8;

  --green: #0f9d58;        --green-soft: #34c77c;   --green-bg: #e6f7ee;
  --yellow: #d97706;       --yellow-soft: #f59e0b;  --yellow-bg: #fff4e0;
  --red: #dc2626;          --red-soft: #ef4444;     --red-bg: #fde7e7;
  --blue: #1d6fed;         --blue-soft: #3b82f6;    --blue-bg: #e3edff;
  --purple: #7c3aed;
  --orange: #ea580c;
  --accent: #5b6cff;

  --gold: #c47e00;
  --gold-bright: #e8a200;
  --gold-bg: #fff4d6;
  --gold-border: #e8c870;
}
```

### 6.2 폰트

```css
font-family: 'Noto Sans KR', sans-serif;  /* 본문 */
font-family: 'JetBrains Mono', monospace;  /* 숫자, 코드, 판정 pill */
```

Google Fonts import:
```
https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700;900&family=JetBrains+Mono:wght@400;500;600;700&display=swap
```

### 6.3 핵심 컴포넌트 클래스

- `.pill.green / .pill.yellow / .pill.red` — Matrix 평가 badge
- `.summary-card.total / .green / .yellow / .red` — Summary Strip 카드
- `.cat-strip.green / .yellow / .red` — Matrix 좌측 색상 strip
- `.vendor-line` — 업체 정보 한줄 (monospace)
- `.vendor-note` — 업체 부가 설명 (11px, dim)
- `.v-judgment.pass / .hold / .fail / .pending` — 판정 표시
- `.pipeline-item.dead` — 종료/부적합 업체 (취소선)
- `.conclusion .risk` — 리스크 강조 (red, bold)
- `.conclusion .next` — 다음 스텝 (dim)

### 6.4 판정 pill 스타일

```css
.v-judgment.pass {
  color: var(--green);
  background: var(--green-bg);
  padding: 1px 6px;
  border-radius: 4px;
}
.v-judgment.hold {
  color: var(--gold);
  background: var(--gold-bg);
  padding: 1px 6px;
  border-radius: 4px;
  border: 1px solid var(--gold-border);
}
.v-judgment.fail {
  color: var(--red);
  background: var(--red-bg);
  padding: 1px 6px;
  border-radius: 4px;
}
.v-judgment.pending {
  color: var(--text-muted);
}
```

---

## §7. 루틴 설정 방법

### Claude Code에서 루틴 등록

```bash
claude routine add "국산화TF-주간대시보드" \
  --schedule "weekly" \
  --prompt-file CLAUDE.md \
  --connectors notion,google-drive
```

### 수동 실행

```bash
claude routine run "국산화TF-주간대시보드"
```

### GitHub push (수동)

```bash
cd droneparts_endpicture
cp {YYMMDD}_EndPicture.html index.html
git add index.html
git commit -m "{YYMMDD} EP"
git push origin main
```
