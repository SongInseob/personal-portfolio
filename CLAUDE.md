# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

개인 포트폴리오 웹사이트. [Andrej Karpathy의 karpathy.ai](https://karpathy.ai/)를 레퍼런스로 삼아 순수 HTML + CSS 단일 파일 구조로 제작한다. 프레임워크, 빌드 툴, JavaScript 없음.

**AI 워크플로**: Garry Tan의 gstack + gbrain을 적용하여 기획 → 설계 → 개발 → QA → 배포 전 과정을 진행한다.

## Assets

- `profile-photo-resized.jpg` — 프로필 사진 (400×405px, 44KB, 리사이즈 완료)
- `2026(InSong)-Resume.md` — 영문 이력서 (경력/프로젝트 콘텐츠 원본)
- `myinfo.md` — 태그라인, 포트폴리오 링크, 소셜 링크 정리본

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

## 현재 index.html 구성 (확정)

### 섹션 구조
- **Header**: profile-photo-resized.jpg + "Inseob Song" + 태그라인 + Email/LinkedIn 링크
- **Career**: 6개 타임라인 항목 (역순)
- **Selected Portfolios**: Infludeo, Turing, Oprimed
- **Notable Achievements**: 4개 항목 (World first × 2, Korea first, National project)
- **Publications & Patents**: 통계 5개 (63 patents, 89 apps, 21 journals, 46 conf, 5 keynotes)
- **Awards**: 3개 항목 (2020/2025 통합, 2010, 2001–2002)

### 주요 설계 결정 (확정)
- 2차 텍스트 색상: `#767676` (WCAG AA 4.54:1 통과)
- 외부 링크: `rel="noopener noreferrer"` 전체 적용
- `<main>` 랜드마크, `a:focus-visible` outline 적용
- 2020/2025 경기도지사 표창 → 한 항목으로 통합 표시

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
- [x] git init + GitHub CLI(`gh`) 설치 및 로그인 (SongInseob)
- [x] `/office-hours` → 포트폴리오 기획 완료 (설계 문서 APPROVED)
- [x] 콘텐츠 준비 완료 (2026(InSong)-Resume.md + myinfo.md)
- [x] `profile-photo-resized.jpg` 생성 (400×405px, 44KB)
- [x] `/design-html` → `index.html` 생성 완료 (2026-05-01)
      - 디자인 아티팩트: `~/.gstack/projects/personal-portfolio/designs/portfolio-20260501/`
- [x] `/review` → WCAG AA 색상 수정, focus-visible 추가
- [x] `/qa` → health score 97/100, PASS (P3 only: OG tags 없음, skip-link 없음)
      - QA 리포트: `.gstack/qa-reports/qa-report-localhost-2026-05-01.md`
- [x] GitHub Pages 배포 완료 (2026-05-01)
      - 라이브 URL: https://songinseob.github.io/personal-portfolio/
      - 레포: https://github.com/SongInseob/personal-portfolio
      - `git push` → 1~2분 자동 반영
- [x] 사소한 수정 (2026-05-01): Selected Portfolios 표기, 수상 2020/2025 통합
- [x] 콘텐츠 보완 (2026-05-02):
      - OG / Twitter Card 메타태그 추가
      - 포트폴리오 회사 설명 정확도 반영 (Infludeo/Turing/Oprimed)
      - Invited Talks 섹션 신설 (7개, 2004–2013)
      - Publications & Patents 통계: 5 Keynotes → 7 Invited Talks
      - Notable Achievements 연도 태그 일관화, National project SOFC/DMFC 분리
      - 포트폴리오 회사 설명 추가 수정 (K-POP, Turing 문구 간결화)
- [x] 추가 수정 (2026-05-02 계속):
      - Notable Achievements 연도 일관화 완료 (MEMS 2002 태그 추가, 본문 연도 제거)
      - National project SOFC(2008–2012, $30.3M) / DMFC(2011–2014, $9.8M) 분리
      - Turing 설명 간결화 (Global 제거, elites 제거, worldwide 제거)
- [x] Notion 포트폴리오 Markdown 생성 (2026-05-02)
      - 파일: `notion-portfolio.md`
      - Notion에서 `/import` → Markdown & CSV 로 업로드하거나 복사-붙여넣기

## 다음 세션에서 할 일 (선택)

- [ ] `/canary` → 라이브 GitHub Pages URL 기준 배포 후 모니터링
- [ ] `/design-review` → 시각적 폴리싱 (Invited Talks 섹션 추가 후 전체 흐름 재점검)
- [ ] `/learn` → 프로젝트 설계 결정사항 gbrain 저장
- [ ] Skip-to-content 링크 추가 (키보드 접근성)
- [ ] 추가 콘텐츠 여부 확인 (AI 부트캠프 프로젝트 등)
- [ ] Notion 페이지 직접 연동 (Notion API MCP 설정 시)

## 참고 사항

- 로컬에서 `index.html` 직접 열면 Chrome 보안 정책으로 이미지 안 보임
  → 로컬 미리보기: `python3 -m http.server 8080` 후 `localhost:8080` 접속
  → 평소엔 GitHub Pages URL 사용 권장

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
