# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

개인 포트폴리오 웹사이트. [Andrej Karpathy의 karpathy.ai](https://karpathy.ai/)를 레퍼런스로 삼아 순수 HTML + CSS 단일 파일 구조로 제작한다. 프레임워크, 빌드 툴, JavaScript 없음.

**AI 워크플로**: Garry Tan의 gstack + gbrain을 적용하여 기획 → 설계 → 개발 → QA → 배포 전 과정을 진행한다.

## Assets

- `profile-photo.jpg` — 프로필 사진
- `2601(송인섭)-이력서.pdf` — 이력서 (경력/프로젝트 콘텐츠 원본)

## 목표 페이지 구조 (karpathy.ai 기준)

```
index.html
├── Header: 프로필 사진 + 이름 + 태그라인 + SNS 링크
├── 경력 타임라인 (역순)
├── Talks / Presentations
├── Writing / 블로그
├── Projects
└── Publications (해당 시)
```

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
/office-hours   → 코딩 전 6가지 강제 질문 (목적/독자/성공지표 정의)
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
- [ ] `/office-hours` → 포트폴리오 기획
- [ ] `/autoplan` → 설계 리뷰
- [ ] `/design-html` → HTML/CSS 생성
- [ ] 콘텐츠 채우기 (이력서 기반)
- [ ] `/qa` → QA
- [ ] GitHub Pages 배포

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
