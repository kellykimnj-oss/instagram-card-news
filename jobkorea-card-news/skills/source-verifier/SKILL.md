---
name: source-verifier
description: 카드뉴스 출처 검증 기준. 연도·비율·URL 유효성·내용 정확성을 검증. Use when fact-checking sources, verifying statistics, or building the reference slide for card news content.
---

# 출처 검증 기준

## 기본 규칙

| 항목 | 기준 |
|------|------|
| 허용 연도 | 2024~2026년 발행 출처만 사용 |
| 한국 출처 비율 | 80~90% |
| 해외 출처 비율 | 10~20% |
| 출처 슬라이드 | 본문 마지막 +1장 고정 |

---

## 검증 절차

### 1단계: URL 생존 확인
본문에 인용된 모든 링크에 대해:
- HTTP 200 응답 확인
- 리다이렉트 최종 URL 확인
- 404/403/접근불가 → 대체 출처 탐색 후 교체

### 2단계: 내용 정확성 교차 검증
통계·수치·주장이 포함된 경우:
- 원문 출처에서 해당 수치 직접 확인
- 인용 맥락이 원문과 일치하는지 확인
- 수치 변형(반올림·단위 변경 등) 시 원문 병기

### 3단계: 출처 비율 확인
한국/해외 출처 수 집계 → 80/20 비율 준수 여부 확인
미준수 시 → 부족한 비율의 출처 추가 탐색

### 4단계: 연도 확인
2024년 미만 출처 발견 시 → 동일 주제 최신 출처로 교체
단, 업계 표준 보고서(연간 리포트 등)는 2023년까지 예외 허용

---

## 직무별 우선 한국 출처

### 공통 (전 직무)
- 사람인 리서치·뉴스룸 (saramin.co.kr)
- 원티드 인사이트 (wanted.co.kr/insight)
- 자소설닷컴 트렌드 (jasoseol.com)
- 링커리어 커뮤니티 (linkareer.com)
- 캐치 콘텐츠 (catch.co.kr)
- 리멤버 아티클 (rememberapp.co.kr)
- 긱뉴스 (news.hada.io)
- 요즘IT (yozm.wishket.com)
- 커리어리 (careerly.co.kr)
- 브런치 (brunch.co.kr)

### 개발자 특화
- velog 조회수 높은 글
- OKKY 인기 게시글
- 인프런 강의 후기·블로그
- 개발자 블라인드 핫 게시글

### PMPO 특화
- 디스콰이엇 (disquiet.io)
- 서핏 (surfit.io)
- 브런치 PM/기획 카테고리

### 디자이너 특화
- 디자인 컴퍼스 (design-compass.co.kr)
- 브런치 디자인 카테고리
- Figma Community (한국어 콘텐츠)

### 마케터 특화
- 오픈애즈 (openads.co.kr)
- 바닥 (badak.biz)
- 나스미디어 보고서 (nasmedia.co.kr)
- 메조미디어 보고서 (mezzomedia.co.kr)
- 캐릿 (careet.net)

---

## 허용 해외 출처 (10~20% 한정)

- LinkedIn Talent Insights
- Harvard Business Review
- Nielsen Norman Group (디자이너)
- McKinsey Global Institute 보고서
- Stack Overflow Developer Survey (개발자)
- Product Coalition (PMPO)
- Think with Google (마케터)

---

## 출처 슬라이드 작성 형식

```json
{
  "type": "cta",
  "headline": "참고 출처",
  "body": "① [출처명] — [URL 또는 발행처] (연도)\n② [출처명] — [URL 또는 발행처] (연도)\n③ ...",
  "cta_text": "더 알아보기",
  "tag1": "#잡코리아오카방",
  "tag2": "#[직무]커리어",
  "tag3": "#이직팁"
}
```

출처는 최대 8개, 번호 순으로 정렬. 
