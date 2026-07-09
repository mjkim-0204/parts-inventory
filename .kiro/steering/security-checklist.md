# 보안 체크리스트 (부품 도감 프로젝트)

## 외부 서비스 ID 관리
- DRIVE_FOLDER_ID, SPREADSHEET_ID 등 외부 서비스 식별자를 소스 코드에 직접 넣지 말 것
  → Apps Script 속성(PropertiesService) 또는 환경 설정으로 분리
  → Git에 올라가면 폴더/시트에 무단 접근 가능

## API 엔드포인트 인증
- 공개 배포된 Apps Script 웹앱에는 최소한의 토큰 검증 필수
  → doPost에서 secret 파라미터를 확인하는 로직 추가
  → URL만 알면 누구나 bulkSync로 시트 전체 덮어쓰기 가능하므로 방어 필요

## 외부 CDN 의존성
- esm.sh 등 외부 CDN에서 동적 import 시 SRI(Subresource Integrity) 적용 권장
  → CDN 탈취 시 악성 코드 주입 가능성 존재
  → 불가능한 경우 버전 고정(pinning)이라도 할 것

## 클라이언트 저장소
- localStorage에 민감 정보(API URL, 토큰) 저장 시 XSS 공격에 노출됨을 인지할 것
  → 가능하면 서비스 워커나 httpOnly 쿠키로 대체
  → 현 구조에서는 최소한 XSS 방어(esc 함수)를 빠짐없이 적용

## 사용자 입력 이스케이프
- onclick 핸들러에 동적 값 삽입 시 따옴표 이스케이프 확인
  → `onclick="fn('${id}')"` 형태에서 id에 `'`가 들어가면 인젝션 가능
  → 이벤트 위임(delegation) 패턴으로 전환하면 근본적 해결
