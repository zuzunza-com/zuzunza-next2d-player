# 이벤트 시스템

Next2D Player는 Flash Player와 동일한 이벤트 모델을 채택하고 있습니다.

## EventDispatcher

모든 이벤트 발행 가능한 오브젝트의 기본 클래스입니다.

### addEventListener(type, listener, useCapture, priority)

이벤트 리스너를 등록합니다.

```typescript
displayObject.addEventListener("click", (event) => {
    console.log("클릭되었습니다");
});

// 캡처 페이즈에서 받기
displayObject.addEventListener("click", handler, true);

// 우선순위 지정
displayObject.addEventListener("click", handler, false, 10);
```

### removeEventListener(type, listener, useCapture)

이벤트 리스너를 삭제합니다.

```typescript
displayObject.removeEventListener("click", handler);
```

### hasEventListener(type)

지정 타입의 리스너가 등록되어 있는지 확인합니다.

```typescript
if (displayObject.hasEventListener("click")) {
    console.log("클릭 리스너가 등록되어 있습니다");
}
```

### dispatchEvent(event)

이벤트를 발행합니다.

```typescript
const { Event } = next2d.events;

const event = new Event("customEvent");
displayObject.dispatchEvent(event);
```

## Event 클래스

### 프로퍼티

| 프로퍼티 | 타입 | 설명 |
|-----------|------|------|
| `type` | String | 이벤트 타입 |
| `target` | Object | 이벤트 발행원 |
| `currentTarget` | Object | 현재 리스너 등록처 |
| `eventPhase` | Number | 이벤트 페이즈 |
| `bubbles` | Boolean | 버블링 여부 |
| `cancelable` | Boolean | 취소 가능 여부 |

### 메서드

| 메서드 | 설명 |
|----------|------|
| `stopPropagation()` | 전파 정지 |
| `stopImmediatePropagation()` | 전파를 즉시 정지 |
| `preventDefault()` | 기본 동작 취소 |

## 표준 이벤트 타입

### 표시 리스트 관련

| 이벤트 | 설명 |
|----------|------|
| `added` | DisplayObjectContainer에 추가됨 |
| `addedToStage` | Stage에 추가됨 |
| `removed` | DisplayObjectContainer에서 삭제됨 |
| `removedFromStage` | Stage에서 삭제됨 |

```typescript
sprite.addEventListener("addedToStage", (event) => {
    console.log("스테이지에 추가되었습니다");
});
```

### 타임라인 관련

| 이벤트 | 설명 |
|----------|------|
| `enterFrame` | 각 프레임에서 발생 |
| `frameConstructed` | 프레임 구축 완료 |
| `exitFrame` | 프레임 이탈 시 |

```typescript
movieClip.addEventListener("enterFrame", (event) => {
    // 매 프레임 실행되는 처리
    updatePosition();
});
```

### 로드 관련

| 이벤트 | 설명 |
|----------|------|
| `complete` | 로드 완료 |
| `progress` | 로드 진행 |
| `ioError` | IO 에러 |

```typescript
const { Loader } = next2d.display;
const { URLRequest } = next2d.net;

const loader = new Loader();

// async/await를 사용한 읽기
await loader.load(new URLRequest("animation.json"));
const content = loader.content;
stage.addChild(content);

// 프로그레스 이벤트를 사용하는 경우
loader.contentLoaderInfo.addEventListener("progress", (event) => {
    const percent = (event.bytesLoaded / event.bytesTotal) * 100;
    console.log(`${percent}% 로드 완료`);
});
```

## 마우스 이벤트

| 이벤트 | 설명 |
|----------|------|
| `click` | 클릭 |
| `doubleClick` | 더블 클릭 |
| `mouseDown` | 마우스 버튼 누름 |
| `mouseUp` | 마우스 버튼 놓음 |
| `mouseMove` | 마우스 이동 |
| `mouseOver` | 마우스 오버 |
| `mouseOut` | 마우스 아웃 |
| `rollOver` | 롤 오버 |
| `rollOut` | 롤 아웃 |

```typescript
sprite.addEventListener("click", (event) => {
    console.log("클릭 위치:", event.localX, event.localY);
});

sprite.addEventListener("mouseMove", (event) => {
    console.log("마우스 위치:", event.stageX, event.stageY);
});
```

## 키보드 이벤트

| 이벤트 | 설명 |
|----------|------|
| `keyDown` | 키 누름 |
| `keyUp` | 키 놓음 |

```typescript
stage.addEventListener("keyDown", (event) => {
    console.log("키 코드:", event.keyCode);

    switch (event.keyCode) {
        case 37: // 왼쪽 화살표
            player.x -= 10;
            break;
        case 39: // 오른쪽 화살표
            player.x += 10;
            break;
    }
});
```

## 커스텀 이벤트

```typescript
const { Event } = next2d.events;

// 커스텀 이벤트 정의
const customEvent = new Event("gameOver", true, true);

// 이벤트 발행
gameManager.dispatchEvent(customEvent);

// 이벤트 리스닝
gameManager.addEventListener("gameOver", (event) => {
    showGameOverScreen();
});
```

## 이벤트 전파

이벤트는 3개의 페이즈로 전파됩니다:

1. **캡처 페이즈**: root에서 target으로
2. **타겟 페이즈**: target에서 처리
3. **버블링 페이즈**: target에서 root로

```typescript
// 캡처 페이즈에서 처리
parent.addEventListener("click", handler, true);

// 버블링 페이즈에서 처리 (기본값)
child.addEventListener("click", handler, false);
```

## 관련 항목

- [DisplayObject](/ko/reference/player/display-object)
- [MovieClip](/ko/reference/player/movie-clip)
