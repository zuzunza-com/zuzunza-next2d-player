# DisplayObject

DisplayObject는 Next2D Player에서 모든 표시 오브젝트의 기본 클래스입니다.

## 프로퍼티 (Properties)

### 읽기 전용 프로퍼티

| 프로퍼티 | 타입 | 설명 |
|-----------|------|------|
| `instanceId` | number | DisplayObject의 고유 인스턴스 ID |
| `isSprite` | boolean | Sprite 기능을 소유하고 있는지 반환 |
| `isInteractive` | boolean | InteractiveObject 기능을 소유하고 있는지 반환 |
| `isContainerEnabled` | boolean | 컨테이너 기능을 소유하고 있는지 반환 |
| `isTimelineEnabled` | boolean | MovieClip 기능을 소유하고 있는지 반환 |
| `isShape` | boolean | Shape 기능을 소유하고 있는지 반환 |
| `isVideo` | boolean | Video 기능을 소유하고 있는지 반환 |
| `isText` | boolean | Text 기능을 소유하고 있는지 반환 |
| `concatenatedMatrix` | Matrix | 루트 레벨까지 결합된 변환 행렬 |
| `dropTarget` | DisplayObject \| null | 스프라이트의 드래그 대상 또는 드롭된 표시 오브젝트 |
| `loaderInfo` | LoaderInfo \| null | 이 표시 오브젝트가 속한 파일의 로딩 정보 |
| `mouseX` | number | 대상 DisplayObject 기준점에서 마우스의 X 좌표(픽셀) |
| `mouseY` | number | 대상 DisplayObject 기준점에서 마우스의 Y 좌표(픽셀) |
| `root` | MovieClip \| Sprite \| null | DisplayObject의 루트인 DisplayObjectContainer |

### 읽기/쓰기 프로퍼티

| 프로퍼티 | 타입 | 설명 |
|-----------|------|------|
| `name` | string | 이름. getChildByName()에서 사용됨 (기본값: "") |
| `startFrame` | number | 시작 프레임 (기본값: 1) |
| `endFrame` | number | 종료 프레임 (기본값: 0) |
| `isMask` | boolean | 마스크로 DisplayObject에 설정되어 있는지 표시 (기본값: false) |
| `parent` | Sprite \| MovieClip \| null | 이 DisplayObject의 부모 DisplayObjectContainer |
| `alpha` | number | 알파 투명도 값 (0.0~1.0, 기본값: 1.0) |
| `blendMode` | string | 사용할 블렌드 모드 (기본값: BlendMode.NORMAL) |
| `filters` | Array \| null | 표시 오브젝트에 연결된 각 필터 오브젝트의 배열 |
| `height` | number | 표시 오브젝트의 높이 (픽셀 단위) |
| `width` | number | 표시 오브젝트의 너비 (픽셀 단위) |
| `colorTransform` | ColorTransform | 표시 오브젝트의 ColorTransform |
| `matrix` | Matrix | 표시 오브젝트의 Matrix |
| `rotation` | number | DisplayObject 인스턴스의 회전 각도 (도 단위) |
| `scale9Grid` | Rectangle \| null | 현재 유효한 확대/축소 그리드 |
| `scaleX` | number | 기준점에서 적용되는 오브젝트의 수평 스케일 값 |
| `scaleY` | number | 기준점에서 적용되는 오브젝트의 수직 스케일 값 |
| `visible` | boolean | 표시 오브젝트가 가시적인지 여부 (기본값: true) |
| `x` | number | 부모 DisplayObjectContainer의 로컬 좌표를 기준으로 한 X 좌표 |
| `y` | number | 부모 DisplayObjectContainer의 로컬 좌표를 기준으로 한 Y 좌표 |

## 메서드 (Methods)

| 메서드 | 반환값 | 설명 |
|---------|--------|------|
| `getBounds(targetDisplayObject)` | Rectangle | 지정한 DisplayObject의 좌표계를 기준으로 표시 오브젝트의 영역을 정의하는 사각형 반환 |
| `globalToLocal(point)` | Point | point 오브젝트를 스테이지(글로벌) 좌표에서 표시 오브젝트의(로컬) 좌표로 변환 |
| `localToGlobal(point)` | Point | point 오브젝트를 표시 오브젝트의(로컬) 좌표에서 스테이지(글로벌) 좌표로 변환 |
| `hitTestObject(targetDisplayObject)` | boolean | DisplayObject의 드로잉 범위를 평가하여 중복 또는 교차 여부 조사 |
| `hitTestPoint(x, y, shapeFlag)` | boolean | 표시 오브젝트를 평가하여 x, y 파라미터로 지정된 포인트와 중복 또는 교차 여부 조사 |
| `getLocalVariable(key)` | any | 클래스의 로컬 변수 공간에서 값 취득 |
| `setLocalVariable(key, value)` | void | 클래스의 로컬 변수 공간에 값 저장 |
| `hasLocalVariable(key)` | boolean | 클래스의 로컬 변수 공간에 값이 있는지 판단 |
| `deleteLocalVariable(key)` | void | 클래스의 로컬 변수 공간의 값 삭제 |
| `getGlobalVariable(key)` | any | 글로벌 변수 공간에서 값 취득 |
| `setGlobalVariable(key, value)` | void | 글로벌 변수 공간에 값 저장 |
| `hasGlobalVariable(key)` | boolean | 글로벌 변수 공간에 값이 있는지 판단 |
| `deleteGlobalVariable(key)` | void | 글로벌 변수 공간의 값 삭제 |
| `clearGlobalVariable()` | void | 글로벌 변수 공간의 값을 모두 클리어 |
| `remove()` | void | 부모-자식 관계 해제 |

## 블렌드 모드

| 상수 | 설명 |
|------|------|
| `BlendMode.NORMAL` | 일�� 표시 |
| `BlendMode.ADD` | 가산 |
| `BlendMode.MULTIPLY` | 곱셈 |
| `BlendMode.SCREEN` | 스크린 |
| `BlendMode.DARKEN` | 어둡게 |
| `BlendMode.LIGHTEN` | 밝게 |
| `BlendMode.DIFFERENCE` | 차분 |
| `BlendMode.OVERLAY` | 오버레이 |
| `BlendMode.HARDLIGHT` | 하드라이트 |
| `BlendMode.INVERT` | 반전 |
| `BlendMode.ALPHA` | 알파 |
| `BlendMode.ERASE` | 소거 |

## 사용 예제

```typescript
const { Sprite } = next2d.display;
const { BlurFilter } = next2d.filters;

const sprite = new Sprite();

// 위치와 크기
sprite.x = 100;
sprite.y = 200;
sprite.scaleX = 1.5;
sprite.scaleY = 1.5;
sprite.rotation = 30;

// 표시 제어
sprite.alpha = 0.8;
sprite.visible = true;
sprite.blendMode = "add";

// 필터
sprite.filters = [
    new BlurFilter(4, 4)
];

// 스테이지에 추가
stage.addChild(sprite);
```

### 좌표 변환 예제

```typescript
const { Point } = next2d.geom;

// 글로벌 좌표를 로컬 좌표로 변환
const globalPoint = new Point(100, 100);
const localPoint = displayObject.globalToLocal(globalPoint);

// 로컬 좌표를 글로벌 좌표로 변환
const localPos = new Point(0, 0);
const globalPos = displayObject.localToGlobal(localPos);
```

### 충돌 판정 예제

```typescript
// 바운딩 박스로 판정
const hit1 = displayObject.hitTestPoint(100, 100, false);

// 실제 형상으로 판정
const hit2 = displayObject.hitTestPoint(100, 100, true);

// 다른 DisplayObject와의 충돌 판정
if (obj1.hitTestObject(obj2)) {
    console.log("충돌했습니다");
}
```

### 변수 조작 예제

```typescript
// 로컬 변수 조작
displayObject.setLocalVariable("score", 100);
const score = displayObject.getLocalVariable("score");
if (displayObject.hasLocalVariable("score")) {
    displayObject.deleteLocalVariable("score");
}

// 글로벌 변수 조작
displayObject.setGlobalVariable("gameState", "playing");
const state = displayObject.getGlobalVariable("gameState");
displayObject.clearGlobalVariable(); // 모두 클리어
```

## 관련 항목

- [MovieClip](/ko/reference/player/movie-clip)
- [Sprite](/ko/reference/player/sprite)
