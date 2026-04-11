# CampVote Portal — CLAUDE.md

## 프로젝트 개요

**CampVote** 투표 모니터링 시스템의 홍보·제안서 포털 사이트.  
순수 HTML/CSS/JS로 구성된 정적 사이트이며, Vercel로 배포한다.

## 디렉토리 구조

```
campvote-portal/
├── index.html                          # 포털 메인 (탭 네비게이션 + iframe 컨테이너)
├── vercel.json                         # Vercel 배포 설정
├── 투표모니터링시스템소개.html            # 시스템 소개 페이지 (intro 탭)
├── 투표모니터링시스템공중파_대형유투브제안서.html  # 공중파·종편 / 대형유튜브 제안서 (bc, yt 탭 공용)
├── 투표모니터링시스템방송국홍보기획서.html   # 방송국 홍보기획서 (broadcast 탭)
├── 투표모니터링시스템시연영상.html          # 시연영상 (demo 탭)
├── party-proposal.html                 # 정당 제안서 (party 탭)
├── party-execution-plan.html           # 정당 실행 계획서 (execplan 탭)
└── html-file/                          # 서브 HTML 파일 모음 (미사용 또는 임시 파일)
```

## 아키텍처

- `index.html`이 **포털 셸(shell)** 역할: 상단 탭 네비게이션 + iframe 영역으로 구성
- 각 탭은 별도 HTML 파일을 iframe으로 로드 (`frame-{탭키}` id 패턴)
- 탭 키: `intro`, `bc`, `yt`, `broadcast`, `party`, `execplan`, `demo`
- `bc`와 `yt` 탭은 동일 HTML 파일(`투표모니터링시스템공중파_대형유투브제안서.html`)을 로드하되, onload 시 `contentWindow.switchPage()` 호출로 내부 탭을 전환
- 각 iframe 로드 후 `hideInnerNav()`로 내부 nav를 숨김 처리

## 디자인 시스템

| 항목 | 값 |
|------|----|
| 배경색 | `#07090E` (거의 검정) |
| 골드 포인트 | `#C9A84C` |
| 폰트 | Noto Sans KR, Bebas Neue (Google Fonts) |
| 탭 하단 라인 색 | 탭별 상이 (intro·broadcast·execplan=골드, bc=파랑, yt=빨강, party=파랑, demo=녹색) |

## 주요 기능

- **탭 전환** (`switchTab(tab)`): iframe display 전환, active 클래스 관리
- **PDF 저장** (`downloadPDF()`): 현재 탭 iframe에 print CSS 주입 후 `contentWindow.print()`
- **도입 문의** (`openContact()`): 내부 문의 섹션 스크롤 or `mailto:voiceofpusan@gmail.com` fallback
- **로딩 오버레이**: intro iframe 로드 완료 후 숨김

## 배포

- 플랫폼: **Vercel**
- `vercel.json`에 프로젝트명 `campvote-portal` 설정

## 작업 규칙

- 별도 빌드 도구 없음 — HTML/CSS/JS 직접 수정
- 새 탭 추가 시: ① `index.html` nav에 버튼 추가 ② iframe 태그 추가 ③ `PDF_TITLES` 객체에 키 추가
- 내부 HTML 파일들은 독립 페이지로도 동작 가능하게 유지 (자체 nav 포함)
- `hideInnerNav()`가 iframe 내부 nav를 숨기므로, 내부 파일의 nav는 제거하지 않는다
- 한글 파일명 유지 (기존 파일 네이밍 규칙 준수)
