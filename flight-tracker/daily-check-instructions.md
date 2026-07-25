# 신혼여행 항공권 매일 추적 — 실행 지침

이 문서는 매일 자동으로 깨어나는 세션이 따르는 지침입니다.

## 여행 조건 (고정)
- 출발: 인천(ICN)
- 목적지: 호주 시드니(SYD) **또는** 체코 프라하(PRG) — 둘 다 조사
- 출발일: 2026-03-20 / 귀국일: 2026-03-30 (왕복, 약 10일)
- 좌석: **비즈니스**
- 인원: 2명 (신혼여행) — 가격은 **1인 왕복 기준**으로 기록
- 중요: 사용자는 공황장애가 있어 10시간+ 장거리 비행이 부담됨.
  가능하면 **직항 또는 경유 1회 이하**, 좌석이 넓고 프라이버시가 좋은 기체를 우대해서 코멘트에 남길 것.

## 매일 할 일
1. 이 저장소 브랜치 체크아웃:
   `git fetch origin claude/honeymoon-flight-price-tracker-df0d95`
   `git checkout claude/honeymoon-flight-price-tracker-df0d95`
2. `flight-tracker/price-history.json` 읽기. 마지막 기록가와 `targets` 확인.
3. WebSearch로 오늘 시세 조사 (각 노선별로 2~3개 쿼리):
   - "인천 시드니 비즈니스 항공권 2026년 3월 왕복 가격"
   - "ICN SYD business class March 2026 round trip price skyscanner kayak"
   - "인천 프라하 비즈니스 항공권 2026년 3월 왕복 대한항공"
   - "ICN PRG business class March 2026 korean air prestige price"
   - 필요 시 skyscanner / kayak / trip.com / 항공사 페이지를 WebFetch로 열어 확인.
4. 각 노선의 **최저 비즈니스 왕복가(1인, KRW)** 와 항공사/경유/신뢰도를 뽑는다.
   - 값이 애매하면 confidence를 "low"로 표기하고 범위로 남긴다.
5. `history` 배열 끝에 오늘 날짜 항목을 추가한다 (기존 형식과 동일하게).
6. 직전 기록 대비 변화 계산:
   - 각 노선별 **전일 대비 증감액/증감률**.
   - `targets`가 설정돼 있으면 목표가 이하 도달 여부.
7. 변경된 JSON을 커밋 & 푸시:
   `git add flight-tracker/price-history.json`
   `git commit -m "chore(flight-tracker): YYYY-MM-DD 시세 업데이트"`
   `git push -u origin claude/honeymoon-flight-price-tracker-df0d95`
   (푸시 실패 시 2s/4s/8s/16s 백오프로 최대 4회 재시도)

## 사용자에게 보낼 요약 (푸시 알림 본문)
- 한국어로, 200자 이내 한 줄 위주. 이모지 최소.
- 형식 예:
  `[항공권] 시드니 비즈 315만(▼12만/-3.7%) · 프라하 560만(→). 시드니 어제보다 하락!`
- **하락 시 강조**: 전일 대비 떨어졌으면 "하락"/"↓"를 앞쪽에 넣어 눈에 띄게.
- 목표가 이하로 내려가면 "🎯 목표가 도달" 문구를 최우선으로.
- 조사 실패(네트워크 등)면 솔직하게: `[항공권] 오늘 시세 조회 실패 — 스카이스캐너에서 직접 확인 필요`.

## 주의
- 가격은 웹 검색 기반 추정치. 최종 결제 전 항공사/스카이스캐너에서 재확인해야 함을 잊지 말 것.
- 절대 사용자에게 결제/예약을 대신 진행하지 말 것. 정보 제공까지만.
