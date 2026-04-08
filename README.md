# zuzunza-next2d-player — Next2D WebGL/WebGPU Player

> **ZUZUNZA Waterscape 6.0** · Next2D WebGL/WebGPU Player  
> ZUZUNZA 플랫폼 전용으로 포크된 Next2D 플레이어.  
> WebGL 하드웨어 가속 + OffscreenCanvas 멀티스레드 처리로 고성능 애니메이션·게임·인터랙티브 콘텐츠를 브라우저에서 구동합니다.

---

## 목차

1. [기술 스택](#1-기술-스택)
2. [패키지 구조](#2-패키지-구조)
3. [렌더러 아키텍처](#3-렌더러-아키텍처)
4. [설계 원칙](#4-설계-원칙)
5. [플랫폼 통합](#5-플랫폼-통합)
6. [빌드 및 실행](#6-빌드-및-실행)

---

## 1. 기술 스택

| 항목 | 기술 / 버전 |
|------|-------------|
| 패키지 이름 | `@zuzunza-com/zuzunza-next2d-player` |
| 버전 | 3.0.0 |
| 언어 | TypeScript |
| 번들러 | Vite + Rollup |
| 모듈 형식 | ESM |
| 렌더러 | WebGL (하드웨어 가속), WebGPU (차세대) |
| 스레드 | OffscreenCanvas + WorkerThread |
| 의존성 | Node.js 22+ |
| 패키지 매니저 | pnpm (workspace) |
| 테스트 | Vitest |
| 린트 | ESLint |

---

## 2. 패키지 구조

모노레포 구조 — `packages/` 하위의 각 패키지가 독립 책임을 가집니다.

```
zuzunza-next2d-player/
├── src/                      # 최상위 플레이어 진입점
│   └── index.ts
│
├── packages/                 # 모노레포 패키지
│   ├── cache/                # 리소스 캐시 관리
│   ├── core/                 # 플레이어 코어 엔진
│   ├── display/              # 디스플레이 오브젝트 트리 (Stage, Sprite 등)
│   ├── events/               # 이벤트 시스템 (EventDispatcher)
│   ├── filters/              # 비주얼 필터 (Blur, ColorMatrix, DropShadow 등)
│   ├── geom/                 # 기하 연산 (Matrix, Point, Rectangle)
│   ├── media/                # 미디어 재생 (Video, Sound)
│   ├── net/                  # 네트워크 로더 (URLLoader, URLRequest)
│   ├── render-queue/         # 렌더 커맨드 큐
│   ├── renderer/             # 렌더러 메인 (WebGL·WebGPU 드라이버)
│   ├── text/                 # 텍스트 렌더링 (TextField, TextFormat)
│   ├── texture-packer/       # 텍스처 아틀라스 패킹
│   ├── ui/                   # UI 인터랙션 (Button, SimpleButton)
│   ├── webgl/                # WebGL 렌더러 구현
│   └── webgpu/               # WebGPU 렌더러 구현
│
├── e2e/                      # E2E 테스트
├── specs/                    # 단위 테스트 스펙
├── scripts/                  # 빌드 자동화
│   ├── rollup.renderer.worker.config.js  # 렌더러 워커 번들
│   └── rollup.unzip.worker.config.js     # Unzip 워커 번들
│
├── index.html                # 개발 서버 진입점
├── vite.config.ts            # Vite 설정
└── pnpm-workspace.yaml       # pnpm workspace 설정
```

---

## 3. 렌더러 아키텍처

### 멀티스레드 렌더링

```
메인 스레드 (DOM, 입력 이벤트)
  │  postMessage (렌더 커맨드)
  ▼
OffscreenCanvas WorkerThread
  │
  ├── WebGL 렌더러 (packages/webgl/)
  │     └── 하드웨어 가속 벡터·텍스처 렌더링
  │
  └── WebGPU 렌더러 (packages/webgpu/)
        └── 차세대 GPU API 기반 렌더링
```

### 렌더 파이프라인

```
콘텐츠 로드 (net/)
  → 텍스처 패킹 (texture-packer/)
  → 디스플레이 트리 구성 (display/)
  → 렌더 커맨드 생성 (render-queue/)
  → GPU 렌더링 (webgl/ 또는 webgpu/)
  → OffscreenCanvas 합성
  → 화면 출력
```

---

## 4. 설계 원칙

각 클래스의 메서드는 **usecase** 또는 **service** 패턴으로 구현됩니다.

| 규칙 | 설명 |
|------|------|
| `service` → `service` 호출 금지 | 서비스 간 직접 호출 없이 usecase를 경유 |
| 단순 메서드 | `service` 직접 호출 |
| 복합 메서드 | 여러 service를 조합하는 `usecase` 구현 |
| 메서드 역할 | `private`·`protected` 클래스 변수 값 설정까지만 |
| 로직 책임 | `usecase` 또는 `service`에 집중 |

### 의존성 다이어그램

```
case 1:  class → method → service
case 2:  class → method → usecase → service(s)
```

---

## 5. 플랫폼 통합

### wscp-frontend 통합

- `wscp-frontend`의 `webrgss/` 모듈에서 Next2D 플레이어를 iframe으로 임베드.
- Next2D 네이티브 JSON 포맷을 통해 애니메이션·게임 콘텐츠를 로드.

### wscp-studio 통합

- wscp-studio의 미리보기 패널(`preview/[publishId]`)이 Next2D 플레이어를 iframe으로 임베드.
- 스튜디오에서 퍼블리시된 콘텐츠를 실시간 미리보기.

### 콘텐츠 로드 예시

```javascript
// JSON 경로로 콘텐츠 로드
next2d.load("https://cdn.zuzunza.com/content/animation.json");
```

---

## 6. 빌드 및 실행

```bash
# 의존성 설치 (Node.js 22+ 필요)
npm install

# 개발 서버 시작
npm start

# 단위 테스트
npm test

# 린트
npm run lint

# 프로덕션 빌드
npm run build
```

---

*ZUZUNZA Waterscape 6.0 — Next2D WebGL/WebGPU Player*
