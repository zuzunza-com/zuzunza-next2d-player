# 봐도 잘 몰라서 그냥 분석 중

## 코드 구조 이해하기

```시작 지점
index.html (루트 파일)

브라우저에서 처음 로드되는 HTML
<script> 태그에서 next2d.load("develop")로 콘텐츠 로드
src/index.ts (진입점)

Next2D Player의 JavaScript/TypeScript 진입점
전역 next2d 객체를 생성
캔버스/화면 표시 관련 파일
packages/core/

Canvas.ts: HTML Canvas 초기화, 포인터 이벤트 처리
Player.ts: 렌더링 루프, 화면 크기 조절, 재생/정지
Next2D.ts: 애플리케이션 부트스트랩
packages/display/

Stage.ts: 루트 디스플레이 컨테이너 (화면에 표시되는 모든 것의 부모)
Sprite.ts, Shape.ts: 화면에 그릴 수 있는 오브젝트들
DisplayObject.ts: 모든 디스플레이 오브젝트의 베이스 클래스
packages/renderer/ + packages/webgl/

WebGL을 사용해 실제로 GPU에 그리는 부분
Worker 스레드에서 렌더링 수행
권장 읽는 순서
Code
1. README.md (전체 구조 파악)
2. index.html (어떻게 실행되는지)
3. packages/core/README.md (코어 패키지 역할)
4. packages/display/README.md (화면에 뭔가 그리는 방법)
5. packages/core/Canvas.ts (Canvas 초기화 + 이벤트)
6. packages/core/Player.ts (렌더링 루프)
화면 표시 흐름 요약
Code
사용자 코드 (index.html) 
  → next2d.load() 
  → Player.boot (packages/core/Player.ts)
  → Canvas 초기화 (packages/core/Canvas.ts)
  → Stage 생성 (packages/display/Stage.ts)
  → RendererWorker 시작 (packages/core/RendererWorker.ts)
  → WebGL 렌더링 (packages/webgl/)
  → 화면에 표시
packages/core/Canvas.ts와 packages/display/Stage.ts를 보면 캔버스가 어떻게 생성되고 화면에 추가되는지 알 수 있습니다.
```
그렇다는데 모르겠음.
