# 필터

Next2D Player는 DisplayObject에 적용할 수 있는 다양한 비주얼 필터를 제공합니다.

## 필터 적용 방법

```typescript
const { Sprite } = next2d.display;
const { BlurFilter, DropShadowFilter, GlowFilter } = next2d.filters;

const sprite = new Sprite();

// 단일 필터
sprite.filters = [new BlurFilter(4, 4)];

// 여러 필터
sprite.filters = [
    new DropShadowFilter(4, 45, 0x000000, 0.5),
    new GlowFilter(0xff0000, 1, 8, 8)
];

// 필터 제거
sprite.filters = null;
```

## 사용 가능한 필터

| 필터 | 설명 |
|-----------|------|
| BlurFilter | 흐림 효과 |
| DropShadowFilter | 드롭 섀도우 효과 |
| GlowFilter | 글로우 효과 |
| BevelFilter | 베벨 효과 |
| ColorMatrixFilter | 컬러 매트릭스 변환 |
| ConvolutionFilter | 컨볼루션 효과 |
| DisplacementMapFilter | 변위 맵 효과 |
| GradientBevelFilter | 그라디언트 베벨 효과 |
| GradientGlowFilter | 그라디언트 글로우 효과 |

---

## BlurFilter

흐림 효과를 적용합니다. 소프트 포커스부터 가우시안 블러까지 생성할 수 있습니다.

```typescript
const { BlurFilter } = next2d.filters;

new BlurFilter(blurX, blurY, quality);
```

### 프로퍼티

| 프로퍼티 | 타입 | 기본값 | 설명 |
|-----------|------|----------|------|
| blurX | number | 4 | 수평 방향 흐림량 (0~255) |
| blurY | number | 4 | 수직 방향 흐림량 (0~255) |
| quality | number | 1 | 흐림 실행 횟수 (0~15) |

---

## DropShadowFilter

드롭 섀도우 효과를 적용합니다. 내부 섀도우, 외부 섀도우, 녹아웃 모드 등의 스타일 옵션이 있습니다.

```typescript
const { DropShadowFilter } = next2d.filters;

new DropShadowFilter(
    distance, angle, color, alpha,
    blurX, blurY, strength, quality,
    inner, knockout, hideObject
);
```

### 프로퍼티

| 프로퍼티 | 타입 | 기본값 | 설명 |
|-----------|------|----------|------|
| alpha | number | 1 | 섀도우의 알파 투명도 (0~1) |
| angle | number | 45 | 섀도우의 각도 (-360~360도) |
| blurX | number | 4 | 수평 방향 흐림량 (0~255) |
| blurY | number | 4 | 수직 방향 흐림량 (0~255) |
| color | number | 0 | 섀도우의 색상 (0x000000~0xFFFFFF) |
| distance | number | 4 | 섀도우의 오프셋 거리 (-255~255픽셀) |
| hideObject | boolean | false | 오브젝트를 숨길지 여부 |
| inner | boolean | false | 내부 섀도우로 할지 여부 |
| knockout | boolean | false | 녹아웃 효과 적용 여부 |
| quality | number | 1 | 흐림 실행 횟수 (0~15) |
| strength | number | 1 | 임프린트 강도 (0~255) |

---

## GlowFilter

글로우 효과를 적용합니다. 내부 글로우, 외부 글로우, 녹아웃 모드 등의 스타일 옵션이 있습니다.

```typescript
const { GlowFilter } = next2d.filters;

new GlowFilter(
    color, alpha, blurX, blurY,
    strength, quality, inner, knockout
);
```

### 프로퍼티

| 프로퍼티 | 타입 | 기본값 | 설명 |
|-----------|------|----------|------|
| alpha | number | 1 | 글로우의 알파 투명도 (0~1) |
| blurX | number | 4 | 수평 방향 흐림량 (0~255) |
| blurY | number | 4 | 수직 방향 흐림량 (0~255) |
| color | number | 0 | 글로우의 색상 (0x000000~0xFFFFFF) |
| inner | boolean | false | 내부 글로우로 할지 여부 |
| knockout | boolean | false | 녹아웃 효과 적용 여부 |
| quality | number | 1 | 흐림 실행 횟수 (0~15) |
| strength | number | 1 | 임프린트 강도 (0~255) |

---

## BevelFilter

베벨 효과를 적용합니다. 오브젝트를 3차원적으로 표현할 수 있습니다.

```typescript
const { BevelFilter } = next2d.filters;

new BevelFilter(
    distance, angle, highlightColor, highlightAlpha,
    shadowColor, shadowAlpha, blurX, blurY,
    strength, quality, type, knockout
);
```

### 프로퍼티

| 프로퍼티 | 타입 | 기본값 | 설명 |
|-----------|------|----------|------|
| angle | number | 45 | 베벨의 각도 (-360~360도) |
| blurX | number | 4 | 수평 방향 흐림량 (0~255) |
| blurY | number | 4 | 수직 방향 흐림량 (0~255) |
| distance | number | 4 | 베벨의 오프셋 거리 (-255~255픽셀) |
| highlightAlpha | number | 1 | 하이라이트의 알파 투명도 (0~1) |
| highlightColor | number | 0xFFFFFF | 하이라이트의 색상 (0x000000~0xFFFFFF) |
| knockout | boolean | false | 녹아웃 효과 적용 여부 |
| quality | number | 1 | 흐림 실행 횟수 (0~15) |
| shadowAlpha | number | 1 | 섀도우의 알파 투명도 (0~1) |
| shadowColor | number | 0 | 섀도우의 색상 (0x000000~0xFFFFFF) |
| strength | number | 1 | 임프린트 강도 (0~255) |
| type | string | "inner" | 베벨 배치 ("inner", "outer", "full") |

---

## ColorMatrixFilter

4x5 컬러 매트릭스 변환을 적용합니다. 밝기, 대비, 채도, 색조 등을 조정할 수 있습니다.

```typescript
const { ColorMatrixFilter } = next2d.filters;

new ColorMatrixFilter(matrix);
```

### 프로퍼티

| 프로퍼티 | 타입 | 기본값 | 설명 |
|-----------|------|----------|------|
| matrix | number[] | 단위 행렬 | 4x5 컬러 변환용 20개 요소를 가진 배열 |

### 매트릭스 기본값 (단위 행렬)

```typescript
[
    1, 0, 0, 0, 0,
    0, 1, 0, 0, 0,
    0, 0, 1, 0, 0,
    0, 0, 0, 1, 0
]
```

---

## ConvolutionFilter

매트릭스 컨볼루션 필터 효과를 적용합니다. 흐림, 엣지 검출, 샤프, 엠보스, 베벨 등의 효과를 구현할 수 있습니다.

```typescript
const { ConvolutionFilter } = next2d.filters;

new ConvolutionFilter(
    matrixX, matrixY, matrix, divisor, bias,
    preserveAlpha, clamp, color, alpha
);
```

### 프로퍼티

| 프로퍼티 | 타입 | 기본값 | 설명 |
|-----------|------|----------|------|
| alpha | number | 0 | 범위 외 픽셀의 알파 투명도 (0~1) |
| bias | number | 0 | 매트릭스 변환 결과에 가산하는 바이어스량 |
| clamp | boolean | true | 이미지를 클램프할지 여부 |
| color | number | 0 | 범위 외 픽셀의 교체 색상 (0x000000~0xFFFFFF) |
| divisor | number | 1 | 매트릭스 변환 중의 제수 |
| matrix | number[] \| null | null | 매트릭스 변환에 사용하는 값의 배열 |
| matrixX | number | 0 | 매트릭스의 X 차원 (열 수, 0~15) |
| matrixY | number | 0 | 매트릭스의 Y 차원 (행 수, 0~15) |
| preserveAlpha | boolean | true | 알파 채널을 유지할지 여부 |

---

## DisplacementMapFilter

BitmapData 오브젝트의 픽셀 값을 사용하여 오브젝트의 변위를 실행합니다.

```typescript
const { DisplacementMapFilter } = next2d.filters;

new DisplacementMapFilter(
    bitmapBuffer, bitmapWidth, bitmapHeight,
    mapPointX, mapPointY, componentX, componentY,
    scaleX, scaleY, mode, color, alpha
);
```

### 프로퍼티

| 프로퍼티 | 타입 | 기본값 | 설명 |
|-----------|------|----------|------|
| alpha | number | 0 | 범위 외 변위의 알파 투명도 (0~1) |
| bitmapBuffer | Uint8Array \| null | null | 변위 맵 데이터를 포함하는 버퍼 |
| bitmapHeight | number | 0 | 변위 맵 데이터의 높이 |
| bitmapWidth | number | 0 | 변위 맵 데이터의 너비 |
| color | number | 0 | 범위 외 변위에 사용하는 색상 (0x000000~0xFFFFFF) |
| componentX | number | 0 | X 변위에 사용하는 컬러 채널 |
| componentY | number | 0 | Y 변위에 사용하는 컬러 채널 |
| mapPointX | number | 0 | 맵 포인트의 X 오프셋 |
| mapPointY | number | 0 | 맵 포인트의 Y 오프셋 |
| mode | string | "wrap" | 필터 모드 ("wrap", "clamp", "ignore", "color") |
| scaleX | number | 0 | X 변위 결과의 승수 (-65535~65535) |
| scaleY | number | 0 | Y 변위 결과의 승수 (-65535~65535) |

---

## GradientBevelFilter

그라디언트 베벨 효과를 적용합니다. 그라디언트 컬러로 강조된 비스듬한 엣지로 오브젝트를 3차원적으로 보이게 합니다.

```typescript
const { GradientBevelFilter } = next2d.filters;

new GradientBevelFilter(
    distance, angle, colors, alphas, ratios,
    blurX, blurY, strength, quality, type, knockout
);
```

### 프로퍼티

| 프로퍼티 | 타입 | 기본값 | 설명 |
|-----------|------|----------|------|
| alphas | number[] \| null | null | 컬러 ��열의 각 색상에 대응하는 알파값 배열 (각 값 0~1) |
| angle | number | 45 | 베벨의 각도 (-360~360도) |
| blurX | number | 4 | 수평 방향 흐림량 (0~255) |
| blurY | number | 4 | 수직 방향 흐림량 (0~255) |
| colors | number[] \| null | null | 그라디언트에서 사용하는 RGB 16진수 컬러 값의 배열 |
| distance | number | 4 | 베벨의 오프셋 거리 (-255~255픽셀) |
| knockout | boolean | false | 녹아웃 효과 적용 여부 |
| quality | number | 1 | 흐림 실행 횟수 (0~15) |
| ratios | number[] \| null | null | 컬러 배열의 각 색상에 대응하는 색 분포 비율 배열 (각 값 0~255) |
| strength | number | 1 | 임프린트 강도 (0~255) |
| type | string | "inner" | 베벨 배치 ("inner", "outer", "full") |

---

## GradientGlowFilter

그라디언트 글로우 효과를 적용합니다. 제어 가능한 컬러 그라디언트로 현실적인 빛남을 표현할 수 있습니다.

```typescript
const { GradientGlowFilter } = next2d.filters;

new GradientGlowFilter(
    distance, angle, colors, alphas, ratios,
    blurX, blurY, strength, quality, type, knockout
);
```

### 프로퍼티

| 프로퍼티 | 타입 | 기본값 | 설명 |
|-----------|------|----------|------|
| alphas | number[] \| null | null | 컬러 배열의 각 색상에 대응하는 알파값 배열 (각 값 0~1) |
| angle | number | 45 | 글로우의 각도 (-360~360도) |
| blurX | number | 4 | 수평 방향 흐림량 (0~255) |
| blurY | number | 4 | 수직 방향 흐림량 (0~255) |
| colors | number[] \| null | null | 그라디언트에서 사용하는 RGB 16진수 컬러 값의 배열 |
| distance | number | 4 | 글로우의 오프셋 거리 (-255~255픽셀) |
| knockout | boolean | false | 녹아웃 효과 적용 여부 |
| quality | number | 1 | 흐림 실행 횟수 (0~15) |
| ratios | number[] \| null | null | 컬러 배열의 각 색상에 대응하는 색 분포 비율 배열 (각 값 0~255) |
| strength | number | 1 | 임프린트 강도 (0~255) |
| type | string | "outer" | 글로우 배치 ("inner", "outer", "full") |

---

## 사용 예제

### 버튼의 호버 효과

```typescript
const { Sprite } = next2d.display;
const { GlowFilter } = next2d.filters;

const button = new Sprite();

button.addEventListener("rollOver", () => {
    button.filters = [
        new GlowFilter(0x00ff00, 0.8, 10, 10)
    ];
});

button.addEventListener("rollOut", () => {
    button.filters = null;
});
```

### 그림자가 있는 텍스트

```typescript
const { TextField } = next2d.text;
const { DropShadowFilter } = next2d.filters;

const textField = new TextField();
textField.text = "Hello World";
textField.filters = [
    new DropShadowFilter(2, 45, 0x000000, 0.5, 2, 2)
];
```

### 복합 필터

```typescript
const { GlowFilter, DropShadowFilter, BlurFilter } = next2d.filters;

sprite.filters = [
    // 외부 글로우
    new GlowFilter(0x0088ff, 0.8, 15, 15, 2, 1, false),
    // 드롭 섀도우
    new DropShadowFilter(4, 45, 0x000000, 0.6, 4, 4),
    // 가벼운 흐림
    new BlurFilter(1, 1, 1)
];
```

### 컬러 매트릭스로 그레이스케일

```typescript
const { ColorMatrixFilter } = next2d.filters;

// 그레이스케일 변환 매트릭스
const grayscaleMatrix = [
    0.299, 0.587, 0.114, 0, 0,
    0.299, 0.587, 0.114, 0, 0,
    0.299, 0.587, 0.114, 0, 0,
    0,     0,     0,     1, 0
];

sprite.filters = [new ColorMatrixFilter(grayscaleMatrix)];
```

### 그라디언트 글로우 효과

```typescript
const { GradientGlowFilter } = next2d.filters;

sprite.filters = [
    new GradientGlowFilter(
        4, 45,
        [0xff0000, 0x00ff00, 0x0000ff], // colors
        [1, 1, 1],                       // alphas
        [0, 128, 255],                   // ratios
        10, 10, 2, 1, "outer", false
    )
];
```

---

## 관련 항목

- [DisplayObject](/ja/reference/player/display-object)
- [MovieClip](/ja/reference/player/movie-clip)
