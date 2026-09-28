# PaperSpine 분석 대화 정리

- 작성일: 2026-09-28
- 분석 대상 저장소 (포크): https://github.com/bmshin94/PaperSpine
- 원본 저장소: https://github.com/WUBING2023/PaperSpine
- 제품 페이지: https://wubing2023.github.io/PaperSpine/v5/
- 릴리스: https://github.com/WUBING2023/PaperSpine/releases/tag/v0.4.0-alpha.1-dev

---

## 1. 전수조사 분석

### 한 줄 요약
PaperSpine은 AI 에이전트(Claude Code, Codex 등)가 논문 한 편을 처음부터 끝까지 쓰도록 만드는
**작업 매뉴얼 + 검증 도구 묶음**이다. 연구 주제, 자료, 실험 데이터를 주면 문헌 조사 → 논점 정리 →
개요 → 본문 → 그림 → 인용 검증 → 리뷰·수정 → 조판까지 진행하고 Word / LaTeX / PDF로 넘겨준다.

### 기본 정보
| 항목 | 내용 |
|---|---|
| 원본 | `WUBING2023/PaperSpine` (중국 개발자 프로젝트) |
| 이 저장소 | `bmshin94/PaperSpine` (원본 포크, CLAUDE.md 페르소나 추가) |
| 버전 | V4 스킬 `4.0.0` + PaperSpine5 `v0.4.0-alpha.1-dev` (알파 프리릴리스) |
| 라이선스 | MIT (상업적 사용·수정·재배포 가능, 저작권 고지 유지) |
| 지원 호스트 | Claude Code, Codex, OpenClaw, Hermes CLI |
| 인기 | 2026-06-11 기준 스타 2,846개, 209개 학교·58개 회사 (`atlas/data.js`) |

### 동작 흐름 (7단계, `src/skill/SKILL.md`)
1. **접수·설정**: 로컬 웹 화면에서 작업 방식, 목표 저널, 언어, 결과물 형식 선택
2. **리서치**: 목표 저널·학회의 비슷한 논문 3+3편(깊게 하면 6+6편)을 읽고 학습, Crossref/Semantic Scholar로 인용 검증
3. **기여점 확정**: 기여점 후보를 제시하고 사용자가 직접 선택 (Contribution-First)
4. **개요·집필·그림**: 본문, 데이터 그래프, 메커니즘 도식, 방법론 그림 생성 (편집 가능한 SVG/PPTX)
5. **렌더링·검사**: LaTeX/PDF/DOCX 생성, 페이지별 빈 공간·잘린 그림·깨진 인용 검사
6. **독립 리뷰**: 방법론·증거·글쓰기 리뷰어 에이전트 3명이 독립적으로 심사한 뒤 수정
7. **전달·수정**: 같은 작업 안에서 피드백 반영을 반복

작업 모드: `build_from_materials`, `rewrite_existing`, `audit`, `review`, `revise`, `transfer`

### 핵심 철학
- 데이터, p-value, 인용, 저자, 윤리 승인 등을 지어내지 않음
- 3가지 게이트: `contribution_check.py`, `results_validation_check.py`, `reviewer_audit_check.py`
- 로컬 우선: 연구 자료는 로컬에 보관하고 투고·업로드·결제는 자동으로 하지 않음

### 폴더 구성
| 폴더/파일 | 내용 |
|---|---|
| `src/skill/SKILL.md` | 총괄 오케스트레이터 지침 (246줄) |
| `src/skill/references/` | 단계별 상세 매뉴얼 85개 |
| `src/skill/agents/` | 서브에이전트 역할카드 6개 (리서치 3, 리뷰어 3) |
| `src/scripts/` | 파이썬 검증 스크립트 52개 (표준 라이브러리만 사용, 웹 UI·MCP 포함) |
| `src/adapters/` | 호스트별 명령어·설정 보정 |
| `dist/` | `src/`에서 자동 생성된 호스트별 배포본 4종 (직접 수정 금지) |
| `paperspine5/core/` | V5 본체: 글쓰기(01), 그림 엔진 FigMirror(02), 글+그림 통합·웹 UI(03), 패키징(06) |
| `website/` | 제품 페이지, 그림 예시, 홍보 영상, 후원 QR |
| `atlas/` | 스타 누른 사람 세계지도 |
| `tests/` | 테스트 24개 파일 |
| `.github/workflows/` | 검증, 번들 빌드, 릴리스 검증 CI |
| `install.sh` / `install.ps1` | 설치 스크립트 |
| `.claude-plugin/` | Claude Code 플러그인 마켓플레이스 설정 |
| `README.*.md` | 9개 언어 README |

### 언제 쓰나
- 실험 데이터를 논문으로 엮을 때, 초고를 저널 스타일로 다듬을 때
- 투고 전 자체 리뷰, 리뷰어 답변서 작성, 중↔영 번역
- 학회·공모전·보고서 작성

### 나에게 주는 도움
1. 논문·보고서 작성 도구로 바로 사용
2. AI 에이전트 스킬 설계의 교과서 (오케스트레이터 + 매뉴얼 + 서브에이전트 + 검증 스크립트 + 테스트)
3. MIT 라이선스라 제품화 가능

### 주의점
- 알파 버전이라 불안정할 수 있음
- `install.sh`는 원본(WUBING2023) 릴리스에서 번들을 받음 (포크를 수정해도 설치에는 반영 안 됨)
- 문서·폴더명 일부가 중국어
- 버전 표기 혼재 (`plugin.json` 4.0.0 / README V5 알파)

---

## 2. 쉬운 설명 (비유)

PaperSpine은 **논문 요리 레시피북 + 주방 검수관**이다.

- 요리사 = AI 에이전트 (Claude Code 등)
- 레시피북 = `SKILL.md`와 매뉴얼 85개
- 재료 = 실험 데이터, 메모, 참고논문
- 검수관 = 파이썬 스크립트 52개
- 심사위원 = 가상 리뷰어 3명
- 완성 요리 = Word / LaTeX / PDF 논문

| 일반 AI 챗봇 | PaperSpine |
|---|---|
| 인용을 지어낼 수 있음 | 인용이 실제로 있는지 DB로 확인 |
| 한 번에 작성 | 기여점을 사용자와 먼저 합의 |
| 리뷰 없음 | 독립 리뷰어 3명 심사 |
| 텍스트만 | 그림 + LaTeX + PDF + Word |
| 대화 끝나면 끝 | 같은 작업에서 계속 수정 |

---

## 3. 질문별 답변

### 설치 및 사용법
**A. 공식 설치 스크립트 (V5 전체 번들)**
```sh
sh ./install.sh --target claude-code --clean-legacy   # Claude Code
sh ./install.sh --target codex                        # Codex
sh ./install.sh --target both                         # 둘 다
sh ./install.sh --check-only                          # 업데이트 확인
```
```powershell
powershell -ExecutionPolicy Bypass -File .\install.ps1 -Target claude-code -CleanLegacy
```
- OS별 번들(26~56MB, 파이썬 런타임 포함) 다운로드 → 크기·SHA-256 검증 → `~/.claude/skills/paper-spine` 설치 → `~/.paperspine5/profiles/default` 프로필 생성
- `curl`, `unzip` 필요. 설치 후 호스트 재시작

**B. Claude Code 플러그인 마켓플레이스 (V4 스킬)**
```
/plugin marketplace add WUBING2023/PaperSpine
/plugin install paper-spine
```

**C. 수동 복사**: `dist/claude/skills/paper-spine`를 `~/.claude/skills/`에 복사

**사용**: 자료 폴더에서 `/paperspine` 입력 또는 "이 폴더 자료로 논문 써줘" → 웹 설정 화면에서 선택 → 결과는 `paper_rewriting_output/`

### 플러그인? 스킬? MCP?
| 구분 | 해당 여부 | 근거 |
|---|---|---|
| 스킬 | 본체 | `SKILL.md` + `references/` + `scripts/` 표준 구조 |
| 플러그인 | 포장 | `.claude-plugin/plugin.json`, `marketplace.json` |
| MCP | 내부 부품 | V5가 로컬 웹 서버 + MCP-stdio 창구로 작업 상태 기록 (사용자가 별도 등록할 필요 없음) |

### API 토큰 필요 여부
- PaperSpine 자체로는 별도 API 키가 필요 없음
- 글쓰기는 호스트(Claude Code/Codex)가 담당하므로 기존 구독이나 API 과금으로 비용 발생
- Crossref, Semantic Scholar는 무료이며 키 없이 호출
- 다만 논문 여러 편 정독, 서브에이전트, 반복 리뷰로 토큰 사용량이 큼 (`usage_ledger.py`로 기록 가능)

### 깃허브에서 유명한 이유
1. 전 세계 대학원생·연구자라는 거대한 수요
2. AI 가짜 인용 문제를 검증 게이트로 해결
3. 조사부터 그림, 조판까지 끝까지 처리
4. 편집 가능한 과학 그림 생성
5. 멀티 호스트 지원, 9개 언어 README
6. 제품 페이지, 홍보 영상, 커뮤니티 지도 등 마케팅
7. 에이전트 스킬 붐과 타이밍이 맞음

### 로컬 에이전트 구축에 도움이 되는지
설계 패턴 교과서로 매우 유용하다.
- 오케스트레이터 패턴 (필요할 때만 매뉴얼을 읽어 컨텍스트 절약)
- 서브에이전트 팬아웃 (독립 병렬 작업)
- LLM 판단 + 결정론적 파이썬 게이트
- 작업 ID·이벤트 저장소로 상태 영속화
- 로컬 웹 UI + MCP 분리
- `src/` 하나로 멀티 호스트 `dist/` 자동 생성

단, 에이전트 프레임워크가 아니라 기존 에이전트 위에 얹는 스킬이다. 완전 로컬 LLM으로 돌리려면 호스트를
따로 붙여야 하고, 소형 모델은 긴 지침을 소화하기 어려울 수 있다.

### React나 PHP로 만들 수 있는지
가능하다.
| 부분 | 기술 | 가능성 |
|---|---|---|
| 프론트엔드 | React | 적합 (현재 웹 UI가 순수 HTML/JS) |
| 백엔드 | PHP (Laravel) | 가능 |
| AI 두뇌 | Claude API | `SKILL.md`와 매뉴얼을 시스템 프롬프트·도구로 이식 |
| 검증 스크립트 | Python 유지 + API로 감싸기 | 재작성보다 효율적 |
| 조판 | TeX Live, Pandoc | VPS나 도커 필요 |

추천 구조: `React` ↔ `PHP Laravel(API·결제·회원)` ↔ `Python 워커(검증·그림·조판)` ↔ `Claude API`

---

## 4. 수익화 아이디어

### 1) 한국어 논문 작성 SaaS "K-PaperSpine"
- 타깃: 국내 석·박사 과정생, 학부 졸업논문 작성자, 연구실
- 차별점: KCI 투고 규정, 한국어 학술 문체, RISS/KCI 인용 검증, HWP 출력
- 가격 예시: 무료(인용 검증 월 3회) / 학생 월 1.9~2.9만 원 / 연구실 월 15만 원
- API 원가를 고려해 크레딧제 권장, "대필"이 아닌 "연구자 주도 글쓰기 보조"로 포지셔닝

### 2) 정부과제·R&D 보고서 작성 도우미 (B2B)
- 타깃: 중소기업·스타트업 R&D 과제, 산학협력단
- 매년 반복되는 수요, 외주 컨설팅 단가가 높음
- 리뷰어 감사 구조를 "평가위원 관점 사전 심사"로 전환
- 가격: 건당 30~100만 원 또는 연간 구독

### 3) 논문 그림 생성 서비스 "FigStudio"
- FigMirror 엔진의 편집 가능한 SVG/PPTX 그림 활용
- CSV 업로드 → 저널 스타일 3안 → 선택 → 다운로드
- 가격: 그림당 3천~1만 원, 월 2~4만 원

### 4) 투고 전 AI 사전 심사 "Pre-Review"
- 리뷰어 3명 리포트 + 인용 진위 검사 + 형식 체크
- 기존 스크립트 재활용률이 높고 윤리 리스크가 낮음
- 가격: 건당 1~3만 원, 영문 교정 업체 제휴

### 5) 기업·연구소용 설치형 버전
- 병원, 제약사, 국책연구소 (데이터 외부 반출 불가)
- 구축비 + 연 유지보수 15~20%

### 6) 스킬 제작 대행·교육
- 법률, 특허, 기획서, 제안서 분야로 설계 패턴 복제
- 맞춤 스킬 제작, 강의·전자책

### 우선순위
| 순위 | 아이디어 | 난이도 | 수익성 |
|---|---|---|---|
| 1 | Pre-Review | 쉬움 | 중간 (MVP 추천) |
| 2 | K-PaperSpine | 중간 | 높음 |
| 3 | R&D 보고서 도우미 | 중간 | 매우 높음 |
| 4 | FigStudio | 어려움 | 중간 |
| 5 | 설치형 | 매우 어려움 | 높음 |

### 로드맵 (React + PHP)
1. 1~2개월: Pre-Review MVP (React + Laravel + Python 워커 + Claude API)
2. 3~4개월: 결제 연동, 대학원 커뮤니티 베타
3. 5~6개월: 초고 작성과 한국어·KCI 스타일 → K-PaperSpine
4. 이후: B2B 확장

### 유의사항
- MIT 저작권·라이선스 고지 유지
- 학술 윤리: 대학별 AI 사용 규정 확인, AI 사용 내역 리포트 제공
- API 원가 관리: 프롬프트 캐싱, 경량 모델 1차 처리, 크레딧제
- 개인정보: 연구·임상 데이터 보관 정책과 암호화
