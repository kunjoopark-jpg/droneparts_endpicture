# 🇰🇷 드론 부품 국산화 — End Picture Dashboard

Bone Robotics 드론 부품 국산화 TF의 주간 현황 대시보드입니다.

**GitHub Pages**: https://kunjoopark-jpg.github.io/droneparts_endpicture/

---

## 개요

매주 Notion 업체 DB와 액션 아이템 DB에서 데이터를 읽어 자동 생성되는 HTML 대시보드입니다.
10개 부품군(배터리, 모터, ESC, FC, 에어프레임, 짐벌카메라, 통신모듈, GPS, 프로펠러, 조종기/GCS)의
국산화 진행 상태를 한눈에 확인할 수 있습니다.

## 파일 구조

| 파일 | 설명 |
|------|------|
| `index.html` | GitHub Pages 배포 산출물 (매주 자동 업데이트) |
| `CLAUDE.md` | 루틴 에이전트 지침 및 Notion 데이터 소스 명세 |
| `routine_setup_guide.md` | HTML 산출물 양식 명세, 디자인 시스템, 설정 가이드 |

## 업데이트 방식

Claude Code 루틴이 매주 실행되어:
1. Notion DB에서 최신 데이터 수집
2. `{YYMMDD}_EndPicture.html` 생성
3. Google Drive 업로드
4. 이 레포의 `index.html` 덮어쓰기 → GitHub Pages 자동 배포

루틴 설정 방법은 `routine_setup_guide.md` §7 참조.
