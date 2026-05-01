# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

개인 포트폴리오 웹사이트. [Andrej Karpathy의 karpathy.ai](https://karpathy.ai/)를 레퍼런스로 삼아 순수 HTML + CSS 단일 파일 구조로 제작한다. 프레임워크, 빌드 툴, JavaScript 없음.

**AI 워크플로**: Garry Tan의 gstack + gbrain을 적용하여 기획 → 설계 → 개발 → QA → 배포 전 과정을 진행한다.

## Assets

- `profile-photo.jpg` — 프로필 사진 (deploy 전 ≤400px width, <100KB로 리사이즈 필요)
- `2601(송인섭)-이력서.pdf` — 이력서 (경력/프로젝트 콘텐츠 원본)

## 확정된 설계 결정 (`/office-hours` 완료)

디자인 문서: `~/.gstack/projects/personal-portfolio/songinseob-main-design-20260501-093821.md`

| 항목 | 결정 |
|------|------|
| 파일 구조 | `index.html` 단일 파일, 인라인 `<style>` |
| 언어 | **영어 전용** |
| 레퍼런스 | karpathy.ai 시각 구조·간격 패턴 차용 (코드 복사 아님) |
| 비어있는 섹션 | **생략** (Talks/Writing 등 콘텐츠 없으면 HTML에서 제거) |
| 모바일 기준 | 375px viewport에서 읽기 편해야 함 |
| 배포 | GitHub Pages (`git push` = 즉시 반영) |

### 페이지 구조

```
index.html
├── Header: profile-photo.jpg + Name + Tagline + Social links
├── Career timeline (reverse chronological)
├── Talks / Presentations  [콘텐츠 있을 때만]
├── Writing / Blog         [콘텐츠 있을 때만]
└── Projects (3–5개)
```

## /design-html 실행 전 준비 체크리스트

아래 5가지가 준비되어야 `/design-html`을 실행할 수 있다:

- [ ] 태그라인 (영어 한 문장) — **이력서에 없음, 직접 작성 필요**
      예시: *"Startup seed investor and deep-tech engineer with 26 years at Samsung and a PhD from KAIST."*
- [x] 경력 타임라인 (회사명, 역할, 기간 — 역순) — 이력서에서 추출 가능
- [ ] 프로젝트 3–5개 (이름, 한줄 설명, GitHub/demo 링크) — **이력서에 없음, 직접 작성 필요**
      (AI 부트캠프 결과물, 투자한 스타트업, 공개 가능한 기술 성과 등)
- [ ] 소셜 링크 — Email `inseob.song@gmail.com` ✅ / **GitHub URL, LinkedIn URL 없음**
- [ ] `profile-photo.jpg` 리사이즈 완료 — **현재 4000×3000px, 7.9MB → ≤400px, <100KB 필요**
      ```bash
      sips --resampleWidth 400 \
        "/Users/songinseob/Library/Mobile Documents/com~apple~CloudDocs/WorkSpace/personal-portfolio/profile-photo.jpg" \
        --out "/Users/songinseob/Library/Mobile Documents/com~apple~CloudDocs/WorkSpace/personal-portfolio/profile-photo.jpg"
      ```

### 이력서에서 바로 쓸 수 있는 경력 타임라인

| 기간 | 소속 | 역할 |
|------|------|------|
| 2017.01–현재 | 에스큐빅엔젤스 (SQubic Angels) | 회장, 개인투자조합 GP |
| 2025.05–2026.01 | K-Digital Training | AI 부트캠프 14기 수료 |
| 2004.10–2016.05 | 삼성SDI | 연료전지 그룹장, 배터리시스템 개발팀장 |
| 1996.02–2004.09 | 삼성전자 종합기술원 | MEMS 센서 개발 과제리더 |
| 1993.10–1996.01 | 삼성중공업 | 자동차 차체 제조공정 설계 엔지니어 |
| 1990.03–1993.09 | 삼성전자 생산기술센터 | — |
| 1994.08 | KAIST | 정밀공학 박사 |

## AI 개발 환경 (gstack + gbrain)

### 설치 현황

| 도구 | 버전 | 위치 |
|------|------|------|
| Bun | 1.3.13 | `~/.bun/bin/bun` |
| gstack | latest | `~/.claude/skills/gstack` |
| gbrain | 0.23.0 | `~/.gbrain/brain.pglite` |

`~/.bun/bin`은 `~/.zshrc`에 PATH 등록 완료. 터미널 재시작 시 자동 인식.

### gstack 슬래시 커맨드 워크플로

```
/office-hours   → 코딩 전 강제 질문 (목적/독자/성공지표 정의)
/autoplan       → CEO + 디자인 + 엔지니어링 리뷰 자동화
/design-shotgun → 4~6가지 시각 변형 생성 (취향 학습)
/design-html    → 프레임워크 없는 프로덕션 품질 HTML/CSS 생성
/review         → 코드 리뷰 + 자동 수정
/qa             → 실제 Chromium 브라우저 테스트
/cso            → 보안 검토 (OWASP Top 10)
/ship           → 푸시 + PR
/canary         → 배포 후 모니터링
/learn          → 프로젝트별 패턴/선호 저장 (gbrain 연동)
```

### gbrain CLI 주요 명령

```bash
export PATH="$HOME/.bun/bin:$PATH"   # 현재 세션에서 gbrain 사용 시 필요

gbrain query "질문"                  # 하이브리드 검색 (벡터+키워드)
gbrain put <slug> < file.md          # 페이지 저장
gbrain import <dir>                  # 마크다운 디렉토리 임포트
gbrain doctor                        # 상태 확인
```

gbrain MCP 서버는 `~/.claude/settings.json`에 등록되어 있어 Claude Code 재시작 시 자동 연결된다.

## 진행 현황

- [x] gstack 설치 (`~/.claude/skills/gstack`)
- [x] gbrain 설치 및 초기화 (`~/.gbrain/brain.pglite`)
- [x] gbrain MCP 서버 등록 (`~/.claude/settings.json`)
- [x] git init
- [x] `/office-hours` → 포트폴리오 기획 완료 (설계 문서 APPROVED)
- [x] 콘텐츠 준비 완료 (2026(InSong)-Resume.md + myinfo.md)
- [x] `/design-html` → `index.html` 생성 완료 (2026-05-01)
      - 폰트: system sans-serif
      - 섹션: Career, Selected Portfolio, Notable Achievements, Publications & Patents, Awards
      - 모바일(375px) / 데스크톱(1440px) 검증 완료
      - 디자인 아티팩트: `~/.gstack/projects/personal-portfolio/designs/portfolio-20260501/`
- [x] `/qa` → Chromium 테스트 완료 (health score 97/100, PASS — P3 only)
- [x] `/ship` → GitHub Pages 배포 완료
      - URL: https://songinseob.github.io/personal-portfolio/
      - Repo: https://github.com/SongInseob/personal-portfolio
      - Branch: main → Pages 자동 빌드

## Skill routing

When the user's request matches an available skill, invoke it via the Skill tool. When in doubt, invoke the skill.

Key routing rules:
- Product ideas/brainstorming → invoke /office-hours
- Strategy/scope → invoke /plan-ceo-review
- Architecture → invoke /plan-eng-review
- Design system/plan review → invoke /design-consultation or /plan-design-review
- Full review pipeline → invoke /autoplan
- Bugs/errors → invoke /investigate
- QA/testing site behavior → invoke /qa or /qa-only
- Code review/diff check → invoke /review
- Visual polish → invoke /design-review
- Ship/deploy/PR → invoke /ship or /land-and-deploy
- Save progress → invoke /context-save
- Resume context → invoke /context-restore
