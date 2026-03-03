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

## 레포지토리 구조

```
instagram-card-news/
├── CLAUDE.md                  # AI 오케스트레이터 지침 (이 파일)
├── README.md                  # 사용자용 설명서
├── BOOTSTRAP_PROMPT.md        # 빈 폴더 부트스트랩 프롬프트
├── config.json                # 직무별 렌더링 기본값
├── package.json               # Node.js 의존성 (puppeteer ^23)
│
├── scripts/
│   ├── render.js              # Puppeteer HTML→PNG 렌더러 (CLI + 모듈)
│   └── generate-samples.js   # 8개 스타일 × 14개 타입 샘플 일괄 생성
│
├── templates/                 # 8개 스타일 × 14개 슬라이드 타입 HTML 템플릿
│   ├── blueprint/             # 블루프린트 프레젠테이션형 (소프트블루)
│   ├── bold/                  # 강렬한 임팩트형 (그라디언트 배경)
│   ├── clean/                 # 클린 에디토리얼형 (라이트그레이)
│   ├── elegant/               # 고급스러운 감성형 (다크 골드)
│   ├── magazine/              # 매거진/SNS형 (포토+화이트)
│   ├── minimal/               # 깔끔한 정보 전달형 (화이트)
│   ├── premium/               # 다크 프리미엄형 (딥 다크)
│   └── toss/                  # 토스 스타일 미니멀 (다크 플랫)
│
├── skills/                    # Claude Code 스킬 파일 (SKILL.md)
│   ├── card-news-design/      # 디자인 스타일 가이드 (텍스트 밀도, 폰트 위계)
│   ├── readability-checker/   # 가독성 검토 체크리스트 (1~10점 평가)
│   ├── source-verifier/       # 출처 검증 기준 (연도·비율·URL)
│   └── target-audience/       # 직무별 타겟 오디언스 정의 (페르소나)
│
├── .claude/
│   └── commands/              # Claude Code 슬래시 커맨드
│       ├── card-news-developer.md
│       ├── card-news-pmpo.md
│       ├── card-news-designer.md
│       └── card-news-marketer.md
│
├── skill-package/             # Claude Code 스킬 배포 패키지 (card-news-setup)
│   ├── SKILL.md               # /card-news-setup 커맨드 정의
│   ├── install.sh             # curl 설치 스크립트
│   └── bin/install.js         # Node.js 설치 스크립트
│
├── workspace/                 # 작업 중간 파일 (git 추적, output은 .gitignore)
│   ├── slides.json            # 렌더링 대상 슬라이드 데이터 (AI가 직접 작성)
│   └── research.md            # 트렌드 리서치 결과 메모
│
├── output/                    # 최종 PNG 출력 (.gitignore에 포함)
└── jobkorea-card-news/        # 잡코리아 전용 서브프로젝트 (동일 구조)
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

# npm 단축키
npm run render -- --slides workspace/slides.json --style premium --output output/ --accent "#4A90E2"

# 샘플 생성 (슬라이드 타입 확인용, 8개 스타일 × 14개 타입)
node scripts/generate-samples.js
npm run sample

# 특정 슬라이드만 재렌더링
node scripts/render.js --slides workspace/slides.json --style [template] --output output/ --slides-only 3,5
```

---

## 템플릿 스타일 (8종)

| 스타일 | 설명 | 기본 악센트 | 배경 |
|--------|------|------------|------|
| `premium` | 다크 프리미엄형 (개발자) | `#4A90E2` 블루 | 딥 다크 (#0D0D1A) |
| `toss` | 토스 스타일 미니멀 (PMPO) | `#6C5CE7` 퍼플 | 다크 플랫 |
| `elegant` | 고급스러운 감성형 (디자이너) | `#D4AF37` 골드 | 다크 |
| `bold` | 강렬한 임팩트형 (마케터) | `#F5A623` 오렌지 | 그라디언트 |
| `clean` | 클린 에디토리얼형 | `#8BC34A` 라임그린 | 라이트그레이 (#F0F0F0) |
| `minimal` | 깔끔한 정보 전달형 | `#2D63E2` 블루 | 화이트 |
| `magazine` | 매거진/SNS형 | `#3B82F6` 블루 | 포토+화이트 |
| `blueprint` | 블루프린트 프레젠테이션형 | `#7BA7CC` 소프트블루 | 라이트블루그레이 |

---

## 슬라이드 타입 레퍼런스 (14종)

모든 슬라이드는 `type` 필드로 템플릿을 지정합니다.
`\n`은 줄바꿈으로 처리됩니다 (렌더러가 `<br>`로 변환).

### 공통 필드 (모든 타입)
| 필드 | 설명 |
|------|------|
| `type` | 슬라이드 타입 (필수) |
| `slide` | 슬라이드 번호 (선택, 문서화용) |

### 타입별 필드 상세

#### `cover` — 표지
```json
{
  "type": "cover",
  "headline": "AI 시대의\n생존법",
  "subtext": "살아남는 사람들은 이미 알고 있다",
  "headline_label": "카드뉴스",
  "tag1": "#개발자커리어",
  "tag2": "#AI활용",
  "tag3": "#잡코리아오카방"
}
```

#### `content` — 일반 본문
```json
{
  "type": "content",
  "headline": "핵심 메시지",
  "body": "본문 내용\n줄바꿈 가능",
  "badge_number": "01",
  "subtext": "보조 설명"
}
```

#### `content-badge` — 카테고리 태그형
```json
{
  "type": "content-badge",
  "badge_text": "TREND",
  "badge_number": "01",
  "headline": "AI가 판도를 바꾼다",
  "body": "설명 텍스트",
  "subtext": "보조 설명"
}
```

#### `content-stat` — 숫자/통계 강조
```json
{
  "type": "content-stat",
  "headline": "AI 도입률",
  "emphasis": "78%",
  "body": "통계 설명\n2줄 이하",
  "subtext": "출처명"
}
```

#### `content-bigdata` — 대형 숫자 강조
```json
{
  "type": "content-bigdata",
  "headline": "시장 규모",
  "bigdata_number": "4,340",
  "bigdata_unit": "억 달러",
  "body": "보조 설명 2줄 이하",
  "subtext": "출처명"
}
```

#### `content-quote` — 인용구/명언
```json
{
  "type": "content-quote",
  "headline": "— Seth Godin",
  "body": "\"마케팅은 더 이상\n만든 것을 파는 기술이 아니다.\"",
  "subtext": "인사이트 설명",
  "badge_number": "03"
}
```

#### `content-steps` — 3단계 프로세스
```json
{
  "type": "content-steps",
  "headline": "성공 3단계",
  "step1": "1단계 설명",
  "step2": "2단계 설명",
  "step3": "3단계 설명"
}
```

#### `content-list` — 항목 나열 (최대 5개)
```json
{
  "type": "content-list",
  "headline": "핵심 채널 5선",
  "item1": "Instagram Reels — 숏폼 최강자",
  "item2": "YouTube Shorts — 검색 연동",
  "item3": "TikTok — Z세대 플랫폼",
  "item4": "LinkedIn — B2B 필수",
  "item5": "Threads — 신흥 채널"
}
```

#### `content-split` — 비교/대조 (좌우)
```json
{
  "type": "content-split",
  "headline": "Before vs After",
  "left_title": "Before",
  "left_body": "수동 작업\n긴 시간\n높은 비용",
  "right_title": "After",
  "right_body": "AI 자동화\n즉각 결과\n비용 절감",
  "subtext": "출처명"
}
```

#### `content-highlight` — 핵심 강조 박스
```json
{
  "type": "content-highlight",
  "headline": "진짜 문제",
  "emphasis": "기술이 아니라\n실행력",
  "body": "보조 설명",
  "subtext": "출처명"
}
```

#### `content-grid` — 2×2 그리드
```json
{
  "type": "content-grid",
  "headline": "4대 핵심 전략",
  "grid1_icon": "🎯",
  "grid1_title": "타겟 정밀화",
  "grid1_desc": "AI 기반 세그먼트",
  "grid2_icon": "📱",
  "grid2_title": "숏폼 콘텐츠",
  "grid2_desc": "15초 영상 제작",
  "grid3_icon": "🤖",
  "grid3_title": "AI 자동화",
  "grid3_desc": "콘텐츠 생성",
  "grid4_icon": "📊",
  "grid4_title": "데이터 분석",
  "grid4_desc": "실시간 측정"
}
```

#### `content-image` — 이미지+텍스트
```json
{
  "type": "content-image",
  "headline": "비주얼 콘텐츠",
  "body": "이미지와 영상은\n텍스트 대비 65% 높은 기억 유지율",
  "image_url": "https://...",
  "badge_number": "02"
}
```

#### `content-fullimage` — 풀 배경 이미지 오버레이
```json
{
  "type": "content-fullimage",
  "headline": "AI 협업 시대의 생존법",
  "badge_text": "핵심 인사이트",
  "body": "파일럿은 끝났다\n성과를 증명할 때",
  "badge2_text": "2026년 목표",
  "body2": "실험 → 확산 → 성과",
  "image_url": "https://..."
}
```

#### `cta` — 행동 유도 (항상 마지막)
```json
{
  "type": "cta",
  "headline": "매주 10분으로\nAI 경쟁력 완성",
  "cta_text": "지금 팔로우하기",
  "subtext": "매주 새로운 인사이트",
  "tag1": "#잡코리아오카방",
  "tag2": "#개발자커리어",
  "tag3": "#이직팁"
}
```

---

## 인라인 텍스트 강조 문법

`headline`, `body` 등 텍스트 필드 안에서 HTML span을 사용해 단어를 강조할 수 있습니다.

```json
{
  "headline": "이직 성공률을 <span class='highlight'>3배</span> 높이는 법",
  "body": "AI 시장, <span class='accent'>폭발적 성장</span>"
}
```

| 클래스 | 효과 |
|--------|------|
| `highlight` | 형광펜 마커 스타일 (악센트 색상 반투명 밑줄) |
| `accent` | 악센트 색상으로 텍스트 강조 |
| `bar-highlight` | 굵은 밑줄 강조 (일부 템플릿) |

**주의**: 슬라이드당 최대 1~2개 단어만 강조. 과도한 사용 금지.

---

## config.json 구조

```json
{
  "defaults": {
    "template": "premium",
    "accent_color": "#4A90E2",
    "account_name": "jobkorea_career",
    "slide_count": 7,
    "tone": "professional"
  },
  "per_job": {
    "developer": { "template": "premium", "accent_color": "#4A90E2", "slide_count": 7, "tone": "professional", "hashtags": [...] },
    "pmpo":      { "template": "toss",    "accent_color": "#6C5CE7", "slide_count": 7, "tone": "professional", "hashtags": [...] },
    "designer":  { "template": "elegant", "accent_color": "#D4AF37", "slide_count": 8, "tone": "casual",       "hashtags": [...] },
    "marketer":  { "template": "bold",    "accent_color": "#F5A623", "slide_count": 6, "tone": "energetic",    "hashtags": [...] }
  },
  "output": { "width": 1080, "height": 1350, "format": "png", "directory": "output/" },
  "sources": { "year_range": [2024, 2026], "korean_ratio_min": 0.8, "korean_ratio_max": 0.9 }
}
```

`config.json`에서 `dimensions.width`/`dimensions.height` 또는 `output.width`/`output.height`로
뷰포트 크기를 지정할 수 있습니다. `render.js`는 `config.dimensions`를 우선 참조합니다.

---

## 렌더러 상세 (`scripts/render.js`)

### CLI 옵션
| 옵션 | 설명 | 기본값 |
|------|------|--------|
| `--slides` | slides.json 경로 | `workspace/slides.json` |
| `--style` | 템플릿 스타일 | `config.defaults.template` |
| `--output` | 출력 디렉토리 | `output/` |
| `--accent` | 악센트 색상 hex | `config.defaults.accent_color` |
| `--account` | 계정명 | `config.defaults.account_name` |

### 모듈로 사용
```javascript
const { render } = require('./scripts/render.js');
await render({
  slidesPath: 'workspace/slides.json',
  style: 'premium',
  outputDir: 'output/',
  accent: '#4A90E2',
  account: 'jobkorea_career',
});
```

### 출력 파일 명명 규칙
`slide_01.png`, `slide_02.png`, ... (2자리 제로패딩)

### 템플릿 플레이스홀더
렌더러는 HTML 템플릿에서 `{{필드명}}` 패턴을 슬라이드 데이터로 치환합니다.
지원되는 플레이스홀더: `{{headline}}`, `{{subtext}}`, `{{body}}`, `{{emphasis}}`,
`{{cta_text}}`, `{{slide_number}}`, `{{total_slides}}`, `{{accent_color}}`,
`{{account_name}}`, `{{image_url}}`, `{{badge_text}}`, `{{badge_number}}`,
`{{step1~3}}`, `{{item1~5}}`, `{{left_title}}`, `{{left_body}}`,
`{{right_title}}`, `{{right_body}}`, `{{grid1~4_icon/title/desc}}`,
`{{bigdata_number}}`, `{{bigdata_unit}}`, `{{headline_label}}`,
`{{tag1~3}}`, `{{badge2_text}}`, `{{body2}}`

---

## 스킬 레퍼런스

| 스킬 | 파일 | 로드 시점 |
|------|------|-----------|
| `card-news-design` | `skills/card-news-design/SKILL.md` | Step 4, Step 5 |
| `target-audience` | `skills/target-audience/SKILL.md` | Step 2, Step 6(페르소나) |
| `source-verifier` | `skills/source-verifier/SKILL.md` | Step 2, Step 6(팩트체크) |
| `readability-checker` | `skills/readability-checker/SKILL.md` | Step 6(가독성) |

각 스킬은 해당 단계 실행 전 반드시 SKILL.md 내용을 읽고 기준을 적용해야 합니다.

---

## 슬래시 커맨드 구현 위치

`.claude/commands/` 디렉토리의 각 `.md` 파일이 `/card-news-*` 커맨드를 정의합니다.
커맨드 파일은 직무별 설정값(템플릿, 악센트 색상, 슬라이드 수, 탐색 채널)을 내장하고
CLAUDE.md Step 3~7 실행을 지시합니다.

---

## 개발 규칙

### slides.json 작성 시
- JSON 배열, 각 원소가 슬라이드 1장
- 순서가 렌더링 순서와 동일
- `\n`으로 줄바꿈 (렌더러가 `<br>` 변환)
- 마지막 슬라이드는 항상 `cta` 타입
- 출처 슬라이드는 `cta` 타입으로 작성 (`headline: "참고 출처"`)

### 템플릿 HTML 수정 시
- `{{accent_color}}` 플레이스홀더는 CSS와 SVG 모두에서 사용 가능
- 모든 템플릿은 `width: 1080px; height: 1350px` 고정 (뷰포트 크기와 일치)
- `overflow: hidden` 필수 (캡처 영역 밖 내용 잘림 방지)
- 외부 폰트 로드 시 `waitUntil: 'networkidle0'` 조건으로 렌더링 대기 (이미 적용됨)

### 새 템플릿 스타일 추가 시
1. `templates/[style-name]/` 디렉토리 생성
2. 14개 슬라이드 타입 HTML 파일 모두 생성 (누락 시 해당 슬라이드 스킵)
3. `scripts/generate-samples.js`의 `templateStyles` 배열에 추가
4. `config.json`에 필요 시 설정 추가
