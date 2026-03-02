---
name: card-news-design
description: 잡코리아 오카방 카드뉴스 디자인 스타일 가이드. 카드뉴스 제작 요청 시 항상 자동 로드. 직무별 템플릿·색상·폰트·텍스트 밀도 기준을 정의. Use when creating, rendering, or reviewing card news slides for jobkorea kakao openchat rooms.
---

# 카드뉴스 디자인 스타일 가이드

## 핵심 원칙
- 모바일 우선: 1080×1350px, 세로형, 한 손으로 스크롤하며 보는 환경
- 3초 룰: 슬라이드당 핵심 메시지 1개, 3초 안에 파악 가능해야 함
- 여백의 미: 텍스트를 줄이고 여백을 늘릴수록 좋음

---

## 직무별 템플릿 & 색상

| 직무 | 템플릿 | 악센트 색상 | 슬라이드 수 | 톤 |
|------|--------|------------|------------|-----|
| 개발자 | `premium` | `#4A90E2` | 7장 | professional |
| PMPO | `toss` | `#6C5CE7` | 7장 | professional |
| 디자이너 | `elegant` | `#D4AF37` | 8장 | casual |
| 마케터 | `bold` | `#F5A623` | 6장 | energetic |

---

## 텍스트 밀도 기준 (엄격 적용)

### 슬라이드 타입별 제한

**cover (표지)**
- headline: **10자 이하** (예: "이직 전 꼭 봐야 할 5가지")
- subtext: 20자 이하
- headline_label: 태그 형식, 10자 이하

**content 계열 (본문)**
- headline: **15자 이하**
- body: **3~4줄 이하**, 줄당 20자 내외
- badge_text: 8자 이하

**content-stat / content-bigdata (숫자 강조)**
- 숫자+단위 1개만 (예: "72%" 또는 "3배")
- body: 2줄 이하 보조 설명

**content-list (목록형)**
- 항목: 최대 5개
- 각 항목: 15자 이하

**content-steps (단계형)**
- 단계: 3단계 고정
- 각 단계 제목: 10자 이하

**cta (마지막 행동 유도)**
- headline: 10자 이하, 질문형 권장
- cta_text: 15자 이하, 동사로 시작

---

## 폰트 위계

```
제목(headline)   : Pretendard Bold, 28px 이상
소제목(badge)    : Pretendard SemiBold, 14~16px
본문(body)       : Pretendard Regular, 16~18px
출처/태그        : Pretendard Light, 12~14px
```

한국어: **Pretendard 필수**
영문 보조: Inter

---

## 텍스트 하이라이트 사용법

핵심 단어 강조 시 highlight span 사용:
```json
{
  "headline": "이직 성공률을 <span class='highlight'>3배</span> 높이는 법"
}
```

과도한 사용 금지: 슬라이드당 최대 1~2개 단어만 강조

---

## 슬라이드 구성 패턴

### 기본 구성 (7장)
```
1장: cover         → 주제 제시, 호기심 유발
2장: content-stat  → 핵심 통계/수치로 문제 제기
3장: content-list  → 원인 또는 체크리스트
4장: content-steps → 3단계 해결책
5장: content       → 심화 인사이트
6장: content-highlight → 핵심 요약
7장: cta           → 행동 유도
+1장: 출처 (고정)
```

### 이직팁 특화 구성
```
1장: cover         → "○○ 이직 전 꼭 알아야 할 것들"
2장: content-stat  → 업계 이직률/연봉 데이터
3장: content-list  → 이직 시 체크리스트
4장: content-split → Do vs Don't
5장: content-quote → 현직자 인사이트
6장: content       → 실행 팁
7장: cta           → "당신의 이직 준비도는?"
```

### AI활용 특화 구성
```
1장: cover         → "○○이 AI를 쓰는 진짜 방법"
2장: content-grid  → AI 툴 4종 비교
3~5장: content     → 실무 활용 사례 (직무별)
6장: content-stat  → 생산성 향상 수치
7장: cta           → "지금 바로 써보기"
```

---

## 금지 사항
- ❌ 슬라이드당 2개 이상의 핵심 메시지
- ❌ 8줄 이상의 본문 텍스트
- ❌ 출처 없는 통계 수치 사용
- ❌ 영어 전용 슬라이드 (한국어 필수)
- ❌ 5개 이상의 하이라이트 강조
