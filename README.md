# Easy Read PDF (PWA)

PDF 파일을 큰 글자로 편하게 읽기 위한 웹 앱. iOS Safari에서 "홈 화면에 추가" 하면 진짜 앱처럼 동작합니다.

## 파일 구조

```
easy-read-pdf-pwa/
├── index.html              # 메인 HTML
├── app.js                  # 앱 로직 (PDF 추출, UI, 테마)
├── sw.js                   # 서비스 워커 (오프라인 캐시)
├── manifest.webmanifest    # PWA 매니페스트
├── icons/
│   ├── icon-192.png        # 일반 아이콘
│   ├── icon-512.png        # 큰 아이콘
│   └── icon-maskable-512.png  # 안드로이드 적응형 아이콘용
└── README.md
```

## 배포: GitHub Pages 사용 (추천)

### 1. 새 저장소 만들기

GitHub에서 `easy-read-pdf` (또는 원하는 이름) 저장소 생성. **public** 으로 만들어야 GitHub Pages가 무료로 됨.

### 2. 파일 푸시

```bash
git init
git add .
git commit -m "Initial PWA"
git branch -M main
git remote add origin https://github.com/<USERNAME>/<REPO>.git
git push -u origin main
```

기존 Jekyll 사이트가 있으면 거기에 서브폴더로 둬도 됩니다 (예: `/reader/`).

### 3. Pages 활성화

저장소 → **Settings** → **Pages** → Source: `Deploy from a branch` → Branch: `main` / `(root)` → Save.

몇 분 기다리면 `https://<USERNAME>.github.io/<REPO>/` 에서 접속 가능. **HTTPS는 자동**이라 PWA 요구사항 충족됩니다.

### 4. 동작 확인

- 데스크탑 크롬에서 URL 열기
- 개발자 도구 → Application 탭 → Manifest 정상 로드 확인
- Service Workers 항목에 `sw.js` activated 되어 있는지 확인
- Lighthouse → Progressive Web App 카테고리 100점 가까이 나와야 함

## 사용자 안내

### iPhone 사용자에게

1. **Safari** 에서 URL 열기 (크롬 X)
2. 화면 아래 **공유 버튼** (네모+위쪽 화살표) 누르기
3. 메뉴에서 **"홈 화면에 추가"** 선택
4. **"추가"** 누르면 끝

설치 후엔 일반 앱처럼 홈 화면 아이콘으로 실행됩니다. 풀스크린이고 사파리 주소창도 안 보여요.

### Android 사용자에게

이미 네이티브 앱이 있으면 그것 추천. PWA로도 가능 — 크롬에서 URL 열고 메뉴 → "홈 화면에 추가" 또는 "앱 설치".

## 기능

- ✅ PDF 텍스트 추출 (PDF.js 사용, 모든 처리는 기기에서만)
- ✅ 단락 자동 분리 (번호 매김 항목 인식)
- ✅ 글자 크기 조절 (14~48px) — 다음 방문 시 기억
- ✅ 라이트 / 다크 / 시스템 테마 — 다음 방문 시 기억
- ✅ 오프라인 동작 (한 번 방문 후엔 인터넷 없어도 OK)
- ✅ iOS Safari 전용 설치 안내 자동 표시 (한 번 닫으면 다시 안 뜸)
- ✅ 안드로이드 앱과 동일한 시각 디자인

## 한계

- 스캔 PDF (이미지 기반)는 텍스트 추출 불가
- 다단 레이아웃은 단락 분리가 부정확할 수 있음
- iOS는 PWA 정책상 푸시 알림 같은 일부 기능 제한 (이 앱엔 해당 없음)

## 로컬 테스트

PWA는 `file://` 로는 제대로 안 돌아가요 (서비스 워커 등록 안 됨). 간단히 로컬 서버 돌리려면:

```bash
# Python 3
python -m http.server 8000

# 또는 npx
npx serve

# 또는 PHP
php -S localhost:8000
```

브라우저에서 `http://localhost:8000` 열기.

## 업데이트 배포

서비스 워커 캐시 때문에 사용자가 옛 버전을 계속 볼 수 있어요. 변경사항 배포 시:

1. `sw.js` 의 `CACHE_NAME` 을 `'easy-read-pdf-v2'`, `v3` ... 식으로 올리기
2. 푸시
3. 사용자가 다음에 접속하면 새 서비스 워커가 옛 캐시를 자동 삭제
