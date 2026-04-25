# DET Tool Suite — 통합 방향서

## 개요
Duolingo English Test(DET) 준비를 위한 자체 제작 웹 툴 모음.
현재 개별 프로젝트로 분리되어 있으나, 최종적으로 하나의 웹앱으로 통합 예정.

---

## 툴 목록

| 툴 | 로컬 경로 | 기능 | 형태 | GitHub | 배포 URL |
|----|----------|------|------|--------|---------|
| **ReadSpeak** | `D:\MyProject\ReadSpeak` | DET Read Then Speak — 지문 읽고 말하기 + 7문장 템플릿 + 타이머 | HTML 단일파일 | [저장소](https://github.com/hanaman100-ui/ReadSpeak) | [GitHub Pages](https://hanaman100-ui.github.io/ReadSpeak) |
| **RandomPhoto** | `D:\MyProject\RandomPhoto` | 사진 보고 말하기 연습 + 타이머 | HTML 단일파일 | [저장소](https://github.com/hanaman100-ui/RandomPhoto) | [GitHub Pages](https://hanaman100-ui.github.io/RandomPhoto) |
| **RandomWord** | `D:\MyProject\RandomWord` | AWL 영단어 퀴즈 (adaptive level) | Flask 웹앱 | [저장소](https://github.com/hanaman100-ui/RandomWord) | [Render](https://randomword-a5ma.onrender.com/) |
| **FillBlank** | `D:\MyProject\FillBlank` | 빈칸 채우기 문장 연습 | HTML 단일파일 ← 현재 | [hanaman100-ui/FillBlank](https://github.com/hanaman100-ui/FillBlank) | https://hanaman100-ui.github.io/FillBlank |
| **ReadComplete** | `D:\MyProject\ReadComplete` | DET Read & Complete — 단어 빈칸 채우기 (100개 지문, 문제당 3분, 2문제씩) | HTML 단일파일 | [hanaman100-ui/ReadComplete](https://github.com/hanaman100-ui/ReadComplete) | https://hanaman100-ui.github.io/ReadComplete |
| **ListenType** | `D:\MyProject\ListenType` | DET Listen & Type — 음성 듣고 타이핑 (300문장 DB, adaptive 난이도, 문제당 60초) | HTML 단일파일 | [hanaman100-ui/ListenType](https://github.com/hanaman100-ui/ListenType) | https://hanaman100-ui.github.io/ListenType |

---

## 원형 기준: ReadSpeak

> 모든 툴의 UI/UX 원형은 **ReadSpeak(`index.html`)**을 따른다.

- 순수 HTML 단일파일 → 서버 불필요, GitHub Pages / Netlify Drop 배포 즉시 가능
- 외부 라이브러리 없음 (CDN 포함 금지) → 오프라인에서도 동작
- 모바일 우선 레이아웃 (max-width 480px 중앙 정렬)

---

## 디자인 시스템

### CSS 변수 (모든 툴 공통)
```css
:root {
  --blue:   #3B82F6;
  --blue-d: #2563EB;
  --orange: #F59E0B;
  --red:    #EF4444;
  --green:  #10B981;
  --text:   #111827;
  --dim:    #9CA3AF;
  --border: #E5E7EB;
  --panel:  #F9FAFB;
}
```

### 레이아웃 규칙
```css
/* 기본 화면 컨테이너 */
.screen {
  position: fixed;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* 데스크톱에서 모바일처럼 보이게 */
@media (min-width: 520px) {
  .screen {
    max-width: 480px;
    left: 50%;
    transform: translateX(-50%);
    border-left:  1px solid var(--border);
    border-right: 1px solid var(--border);
  }
}
```

### 공통 컴포넌트 스펙
| 컴포넌트 | 스펙 |
|---------|------|
| 주 버튼 | height 52px, border-radius 14px, font-size 16px, font-weight 700, bg `--blue` |
| 보조 버튼 | height 48px, border-radius 12px, bg `--panel`, border `--border` |
| 카드 | border-radius 12px, border 1.5px `--border`, padding 18px 20px |
| 타이머 | font-size 52px, font-weight 800, color `--text` |
| 프로그레스바 | height 5px, bg `--orange`, transition 0.9s linear |
| 폰트 | system-ui 스택 (`-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`) |

### 탭 하이라이트 제거 (모바일)
```css
-webkit-tap-highlight-color: transparent;
```

---

## 화면 전환 패턴
- 멀티 화면은 `class="screen hidden"` / JS로 `.hidden` 토글
- `position: fixed; inset: 0` 기반 전환 (페이지 이동 없음)
- 화면 이름 예: `#home`, `#prep`, `#rec`, `#result`

---

## 통합 시 방향
1. 단일 `index.html` 안에 탭 또는 하단 네비로 툴 전환
2. RandomWord(Flask) → 순수 JS로 포팅 (서버 제거)
3. 데이터 파일(단어, 토픽, 사진 목록)은 `data/` 폴더에 JSON으로 관리
4. 서비스명: **DET Tool** (미정)

---

## 배포 기준
- GitHub Pages 또는 Netlify → 정적 HTML만으로 전체 동작
- 별도 서버/백엔드 없음
- 사진 리소스는 Unsplash 무작위 URL 활용 (RandomPhoto 현행 방식 유지)
