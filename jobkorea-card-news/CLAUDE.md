# 잡코리아 카드뉴스 자동화 오케스트레이터

## 프로젝트 개요
잡코리아 오픈카톡방(개발자·PMPO·디자이너·마케터 4개 방) 멤버 이탈 방지 및 신규 유입을 위해
직무별 맞춤 카드뉴스를 주 1회 자동 발행하는 시스템.

- 출력: `output/` 디렉토리에 1080×1350px PNG (Instagram 세로형)
- 슬라이드 수: 본문 6~8장 + 출처 1장 (총 7~9장)
- 렌더링: `node scripts/render.js` (Puppeteer 기반)

---

## 빠른 시작 커맨드

```
/card-news-developer   → 개발자 방 카드뉴스 제작
/card-news-pmpo        → PMPO 방 카드뉴스 제작
/card-news-designer    → 디자이너 방 카드뉴스 제작
/card-news-marketer    → 마케터 방 카드뉴스 제작
```

---

## 전체 워크플로우 (7단계)

> 사람 개입 필요: **Step 1(시작)**, **Step 3(주제 승인)**, **Step 4(구성안 승인)**
> 나머지 Step 2·5·6·7은 완전 자동 실행

### Step 1 — 직무 & 콘텐츠 타입 선택 (사람 입력)
슬래시 커맨드 실행 시 다음을 확인:
- 직무: 개발자 / PMPO / 디자이너 / 마케터
- 콘텐츠 타입: 이직팁 / 커리어개발 / AI활용 / 업무스킬

### Step 2 — 트렌드 리서치 (자동)
`target-audience` 스킬과 `source-verifier` 스킬을 반드시 로드한 후 실행.
아래 **채널 탐색 순서**를 반드시 준수:

#### 공통 필수 탐색 채널 (전 직무 동일)
**채용 플랫폼** — 해당 직무 최신 Q&A·인기 글·키워드 탐색:
- 사람인(saramin.co.kr), 원티드(wanted.co.kr), 자소설닷컴(jasoseol.com)
- 링커리어(linkareer.com), 캐치(catch.co.kr), 리멤버(rememberapp.co.kr), 블라인드(teamblind.com)

**테크·IT 아티클** — 트렌드 파악:
- 긱뉴스(news.hada.io), 요즘IT(yozm.wishket.com), 요즘 것들(yozm.wishket.com)
- 커리어리(careerly.co.kr), 브런치(brunch.co.kr)

#### 직무별 전문 채널
| 직무 | 전문 채널 |
|------|-----------|
| 개발자 | velog.io, okky.kr, inflearn.com, github.com/trending, 개발자 블라인드 |
| PMPO | disquiet.io, yozm.wishket.com, careerly.co.kr, brunch.co.kr/@pm, surfit.io |
| 디자이너 | design-compass.co.kr, yozm.wishket.com, careerly.co.kr, figma.com/community, brunch.co.kr/@design |
| 마케터 | openads.co.kr, badak.biz, nasmedia.co.kr(보고서), mezzomedia.co.kr(보고서), careet.net, whistle.to |

#### 리서치 결과 출력 형식
주제 후보 3개를 다음 형식으로 제시:

```
후보 1: [주제 제목]
- 선정 이유: [왜 이 직무·이 시점에 좋은가]
- 트렌드 점수
  · 검색량 추정: ★★★★☆ (근거: ...)
  · 최근성: ★★★★★ (근거: 최근 N개월 내 ...)
  · 오카방 관련성: ★★★★☆ (근거: ...)
- 참고 출처: [URL 또는 채널명]
```

### Step 3 — 주제 확정 (사람 승인)
- 후보 3개 제시 후 선택 대기
- 반려 시 → 다른 채널에서 재탐색하여 새 후보 3개 제시
- 선택 완료 시 → Step 4 진행

### Step 4 — 카드 구성안 설계 (사람 승인)
`card-news-design` 스킬을 반드시 로드.
확정 주제 기반으로 슬라이드 구성안을 먼저 제안. **반드시 승인 후** 본문 작성 진행.

구성안 출력 형식:
```
슬라이드 구성안 — [주제 제목]

1장 (cover)     : [커버 제목 초안] / [부제목 초안]
2장 (content-?) : [핵심 메시지 한 줄]
3장 (content-?) : [핵심 메시지 한 줄]
4장 (content-?) : [핵심 메시지 한 줄]
5장 (content-?) : [핵심 메시지 한 줄]
6장 (content-?) : [핵심 메시지 한 줄]
7장 (cta)       : [CTA 문구 초안]
8장 (출처)      : 출처 링크 모음 [고정]

※ 타입 선택 근거: ...
```

사용 가능한 슬라이드 타입: cover / content / content-badge / content-stat / content-quote /
content-steps / content-list / content-split / content-highlight / content-grid /
content-bigdata / content-fullimage / cta

### Step 5 — 콘텐츠 초안 + 렌더링 (자동)
**Team 모드**: 카피라이터 에이전트와 후킹 전문가 에이전트가 협업.

#### 카피라이터 역할
- 승인된 구성안 기반으로 `workspace/slides.json` 초안 작성
- `card-news-design` 스킬의 텍스트 밀도 기준 반드시 준수:
  - 커버 제목: 10자 이하
  - 본문 헤드라인: 15자 이하
  - 본문 내용: 3~4줄 이하
  - 강조 카드: 핵심 메시지 1개만

#### 후킹 전문가 역할
초안 완성 후 다음 기준으로 평가 (7점 이상 → 합의, 미달 → 수정 요청):
- 스크롤 스톱 파워: 첫 2초 안에 멈추게 하는가?
- 호기심 유발: 다음 장을 넘기고 싶은가?
- CTA 클릭 유도력: 마지막 장에서 행동하고 싶은가?

최대 3라운드 토론 후 합의된 `slides.json`으로 렌더링:
```bash
node scripts/render.js \
  --slides workspace/slides.json \
  --style [직무별 템플릿] \
  --output output/ \
  --accent "[직무별 색상]" \
  --account "jobkorea_career"
```

### Step 6 — 3종 병렬 검증 (자동)
다음 3개 에이전트를 **동시에** 실행:

#### 팩트체커 에이전트
`source-verifier` 스킬 로드 후 실행:
1. URL 생존 확인: 본문에 인용된 링크 HTTP 상태 코드 체크
2. 내용 정확성: 수치·주장이 출처 내용과 일치하는지 교차 검증
3. 출처 비율: 한국 80~90% / 해외 10~20% 준수 여부
4. 연도 범위: 2024~2026년 출처만 사용 여부
→ 수정 필요 항목 리스트업

#### 가독성 검토자 에이전트
`readability-checker` 스킬 로드 후 실행:
렌더링된 PNG를 기반으로 슬라이드별 평가 (1~10점):
1. 텍스트 밀도: 모바일 기준 본문 5줄 이하
2. 폰트 위계: 제목→소제목→본문 3단계 구분
3. 색상 대비: WCAG AA 기준 (일반 4.5:1↑, 대형 3:1↑)
4. 3초 가독: 슬라이드당 핵심 메시지 1개
5. 카드 흐름: 앞→다음 카드 논리적 연결
6. CTA 명확성: 마지막 장 행동 유도 문구 명확
→ 슬라이드별 점수 + 수정 방향 + 종합 평가

#### 페르소나 리뷰어 에이전트
`target-audience` 스킬 로드 후 해당 직무 3명 페르소나로 평가:
- 페르소나 A: 저연차(1~3년차) — 실용성·이해도
- 페르소나 B: 고연차(7년차 이상) — 깊이·차별성
- 페르소나 C: HR 담당자 — 시장 정확성·신뢰도

각 페르소나별 평가 항목:
1. 도움이 되는가? (Y/N + 이유)
2. 반드시 유지해야 할 부분
3. 추가하면 좋은 내용
4. 불필요하거나 삭제할 부분
5. 가독성·구성 피드백

### Step 7 — 피드백 통합 & 최종 수정 (자동)
3종 검증 결과를 통합하여 우선순위 판단:
1. 팩트체커 지적 사항 → 반드시 수정 (출처 오류는 신뢰도 직결)
2. 가독성 5점 이하 슬라이드 → 반드시 수정
3. 페르소나 2명 이상이 공통 지적한 항목 → 수정
4. 1명만 지적한 항목 → 선택적 반영

수정 완료 후 재렌더링 → `output/` 최종 PNG 저장.

---

## 텍스트 밀도 기준 (필수 준수)

| 슬라이드 타입 | 제목 | 본문 |
|--------------|------|------|
| cover | 10자 이하 | 20자 이하 |
| content 계열 | 15자 이하 | 3~4줄 이하 |
| content-stat / bigdata | 숫자+단위 1개 | 2줄 이하 |
| cta | 10자 이하 | CTA 문구 1개 |

---

## 출처 기준 (필수 준수)
- 연도: 2024~2026년만 사용
- 비율: 한국 출처 80~90% / 해외 10~20%
- 출처 슬라이드: 항상 마지막 1장 고정 (본문 슬라이드 수와 무관)

---

## 직무별 렌더링 설정

| 직무 | 템플릿 | 악센트 색상 | 슬라이드 수 | 톤 |
|------|--------|------------|------------|-----|
| 개발자 | premium | #4A90E2 | 7장 | professional |
| PMPO | toss | #6C5CE7 | 7장 | professional |
| 디자이너 | elegant | #D4AF37 | 8장 | casual |
| 마케터 | bold | #F5A623 | 6장 | energetic |

---

## 주요 명령어

```bash
# 렌더링
node scripts/render.js --slides workspace/slides.json --style [template] --output output/ --accent "[color]"

# 샘플 생성 (슬라이드 타입 확인용)
node scripts/generate-samples.js

# 특정 슬라이드만 재렌더링
node scripts/render.js --slides workspace/slides.json --style [template] --output output/ --slides-only 3,5
```
