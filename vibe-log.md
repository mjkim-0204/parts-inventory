# Vibe Log — Parts Inventory

---

## 2026-07-03 | 재고관리 확장 Spec 보강 (발주/FIFO/권한/알림)

### 작업 내용
- 기존 requirements.md 11개 항목 리뷰 후, 사용자가 말한 핵심 그림("에러코드→필요부품 + 창고·작업자 재고추적 + 발주시점 + 선입선출") 대비 빠진 부분 확인
- 질문을 통해 방향 확정 후 Requirement 12~16 추가:
  - 12: 로트 단위 재고 및 선입선출(FIFO) — 입고 시마다 Lot(입고일+수량) 기록, 출고 시 오래된 Lot 우선 제안
  - 13: 발주서 초안 생성 — 최소재고 미달 부품을 거래처별로 그룹핑해 발주 수량 자동 계산 후 시트/CSV로 내보내기
  - 14: 인증 및 호스팅 전환 — GitHub Pages(정적) → Apps Script 웹앱으로 전환, 조직 구글계정 로그인만 허용
  - 15: 작업자/운영자 권한 분리 — Worker는 자기 차량 반출/반납만, Operator가 전체 관리
  - 16: Slack 알림 연동 — 최소재고 미달/발주서 생성 시 Slack Webhook 알림 (5분 배치)
- Glossary에 Lot, FIFO_Order, Purchase_Order_Draft, Role, Worker, Auth_Session, Slack_Webhook 추가

### 결정 사항
- 전사 직원(작업자 폰 + 운영자 PC)이 실데이터에 접근하게 되므로 정적 페이지 구조는 더 이상 유효하지 않음 → Apps Script 웹앱 전환이 필수 전제
- FIFO는 로트 단위까지 필요 (부품당 단순 총수량으로는 불가능)
- 발주는 "발주서 초안 생성"까지만 자동화, 실제 발주(전화/카톡)는 사람이 처리

### 다음 단계
- design.md 작성 (데이터 구조: Lot 테이블, Role/Auth 모델, Apps Script 웹앱 구조, Slack Webhook 연동)
- tasks.md 생성 후 구현 착수

---


## 2026-06-30 | 재고관리 확장 Spec 작성

### 작업 내용
- **inventory-management-enhancement** Spec 요구사항 문서(requirements.md) 작성 완료
- 11개 요구사항 정의:
  - UI 탭 구조 분리 (도감/재고/AS가이드/대시보드)
  - 부품 위치 관리 (창고 3곳 + 작업자 차량)
  - 반출/반납 추적 및 선지출 시트 연동
  - 작업자 프로필 및 차량 재고 관리
  - 총재고 수량 관리 (최소 재고 경고 포함)
  - 에러코드-부품 연동 AS 가이드
  - 자주 쓰는 품목 관리 (최대 30개)
  - 부품 코드 체계 개선 (WB-D-FM-01 형식)
  - 구글 시트 일괄 가져오기 (자동 컬럼 매핑)
  - 재고 데이터 동기화 및 영속성
  - 운영 효율 대시보드

### 결정 사항
- 기존 index.html 단일파일 구조에서 탭 기반 확장 구조로 전환 예정
- localStorage + Google Sheet 이중 저장 방식 유지
- Apps Script 백엔드(v3) 기반으로 선지출 시트 연동 구현 예정
- 레거시 부품 코드(KS-XX-NN)와 신규 코드 병행 운영

### 다음 단계
- Tech Design 문서 작성 (design.md)
- Task List 생성 (tasks.md)
- 구현 착수

---
