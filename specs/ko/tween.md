# Tween 애니메이션

Next2D Player에서는 프로그램으로 만드는 애니메이션(Tween)을 구현할 수 있습니다. 위치, 크기, 투명도 등의 프로퍼티를 부드럽게 변화시킬 수 있습니다.

## Tween의 기본 개념

```mermaid
flowchart LR
    Start["시작값"] -->|이징 함수| Progress["진행도 0→1"]
    Progress --> End["종료값"]

    subgraph Easing["이징"]
        Linear["Linear"]
        EaseIn["EaseIn"]
        EaseOut["EaseOut"]
        EaseInOut["EaseInOut"]
    end
```

## 기본적인 Tween 클래스

```typescript
class Tween {
    private _target;
    private _properties = {};
    private _duration;
    private _easing;
    private _startTime = 0;
    private _isPlaying = false;
    private _onUpdate;
    private _onComplete;

    constructor(target, options) {
        this._target = target;
        this._duration = options.duration;
        this._easing = options.easing || Easing.linear;
        this._onUpdate = options.onUpdate;
        this._onComplete = options.onComplete;
    }

    to(properties) {
        for (const key in properties) {
            this._properties[key] = {
                start: this._target[key],
                end: properties[key]
            };
        }
        return this;
    }

    play() {
        this._startTime = Date.now();
        this._isPlaying = true;
        this._update();
        return this;
    }

    private _update = () => {
        if (!this._isPlaying) return;

        const elapsed = Date.now() - this._startTime;
        let progress = Math.min(1, elapsed / this._duration);
        progress = this._easing(progress);

        // 프로퍼티 업데이트
        for (const key in this._properties) {
            const prop = this._properties[key];
            this._target[key] = prop.start + (prop.end - prop.start) * progress;
        }

        if (this._onUpdate) {
            this._onUpdate();
        }

        if (elapsed < this._duration) {
            requestAnimationFrame(this._update);
        } else {
            this._isPlaying = false;
            if (this._onComplete) {
                this._onComplete();
            }
        }
    };

    stop() {
        this._isPlaying = false;
    }
}
```

## 이징 함수

```typescript
const Easing = {
    // 선형
    linear: (t) => t,

    // 가속
    easeInQuad: (t) => t * t,
    easeInCubic: (t) => t * t * t,
    easeInQuart: (t) => t * t * t * t,

    // 감속
    easeOutQuad: (t) => t * (2 - t),
    easeOutCubic: (t) => (--t) * t * t + 1,
    easeOutQuart: (t) => 1 - (--t) * t * t * t,

    // 가속→감속
    easeInOutQuad: (t) =>
        t < 0.5 ? 2 * t * t : -1 + (4 - 2 * t) * t,
    easeInOutCubic: (t) =>
        t < 0.5 ? 4 * t * t * t : (t - 1) * (2 * t - 2) * (2 * t - 2) + 1,

    // 바운스
    easeOutBounce: (t) => {
        if (t < 1 / 2.75) {
            return 7.5625 * t * t;
        } else if (t < 2 / 2.75) {
            return 7.5625 * (t -= 1.5 / 2.75) * t + 0.75;
        } else if (t < 2.5 / 2.75) {
            return 7.5625 * (t -= 2.25 / 2.75) * t + 0.9375;
        } else {
            return 7.5625 * (t -= 2.625 / 2.75) * t + 0.984375;
        }
    },

    // 백 (지나쳐서 돌아옴)
    easeOutBack: (t) => {
        const c1 = 1.70158;
        const c3 = c1 + 1;
        return 1 + c3 * Math.pow(t - 1, 3) + c1 * Math.pow(t - 1, 2);
    },

    // 엘라스틱 (고무 같은 움직임)
    easeOutElastic: (t) => {
        if (t === 0 || t === 1) return t;
        return Math.pow(2, -10 * t) * Math.sin((t * 10 - 0.75) * (2 * Math.PI) / 3) + 1;
    }
};
```

## 사용 예제

### 기본적인 이동 애니메이션

```typescript
const { Sprite } = next2d.display;

const sprite = new Sprite();
sprite.x = 0;
sprite.y = 100;
stage.addChild(sprite);

// 오른쪽으로 이동
new Tween(sprite, { duration: 1000, easing: Easing.easeOutQuad })
    .to({ x: 400 })
    .play();
```

### 여러 프로퍼티 동시 애니메이션

```typescript
// 이동 + 확대 + 페이드인
new Tween(sprite, {
    duration: 500,
    easing: Easing.easeOutCubic
})
    .to({
        x: 200,
        y: 150,
        scaleX: 2,
        scaleY: 2,
        alpha: 1
    })
    .play();
```

### 순차적 애니메이션

```typescript
// 연속된 애니메이션
function sequentialAnimation(sprite) {
    new Tween(sprite, {
        duration: 500,
        onComplete: () => {
            new Tween(sprite, {
                duration: 300,
                onComplete: () => {
                    new Tween(sprite, { duration: 500 })
                        .to({ alpha: 0 })
                        .play();
                }
            })
                .to({ scaleX: 1.5, scaleY: 1.5 })
                .play();
        }
    })
        .to({ y: 100 })
        .play();
}
```

### 게임에서의 활용 예제

#### 캐릭터 점프

```typescript
function jump(character) {
    const startY = character.y;
    const jumpHeight = 100;

    // 상승
    new Tween(character, {
        duration: 300,
        easing: Easing.easeOutQuad,
        onComplete: () => {
            // 하강
            new Tween(character, {
                duration: 300,
                easing: Easing.easeInQuad
            })
                .to({ y: startY })
                .play();
        }
    })
        .to({ y: startY - jumpHeight })
        .play();
}
```

#### 데미지 이펙트

```typescript
function damageEffect(target) {
    const originalX = target.x;
    let shakeCount = 0;

    // 깜빡임 + 흔들림
    const shake = () => {
        if (shakeCount >= 6) {
            target.x = originalX;
            target.alpha = 1;
            return;
        }

        const offset = shakeCount % 2 === 0 ? 5 : -5;
        target.x = originalX + offset;
        target.alpha = shakeCount % 2 === 0 ? 0.5 : 1;
        shakeCount++;

        setTimeout(shake, 50);
    };

    shake();
}
```

#### 코인 획득 이펙트

```typescript
function coinCollectEffect(coin, targetY) {
    // 위로 날아가서 페이드아웃
    new Tween(coin, {
        duration: 500,
        easing: Easing.easeOutQuad,
        onUpdate: () => {
            // 회전
            coin.rotation += 15;
        },
        onComplete: () => {
            coin.parent?.removeChild(coin);
        }
    })
        .to({
            y: targetY,
            alpha: 0,
            scaleX: 0.5,
            scaleY: 0.5
        })
        .play();
}
```

#### UI 표시 애니메이션

```typescript
function showPopup(popup) {
    popup.scaleX = 0;
    popup.scaleY = 0;
    popup.alpha = 0;

    new Tween(popup, {
        duration: 400,
        easing: Easing.easeOutBack
    })
        .to({ scaleX: 1, scaleY: 1, alpha: 1 })
        .play();
}

function hidePopup(popup, onComplete) {
    new Tween(popup, {
        duration: 200,
        easing: Easing.easeInQuad,
        onComplete
    })
        .to({ scaleX: 0, scaleY: 0, alpha: 0 })
        .play();
}
```

## enterFrame를 사용한 경량 Tween

```typescript
// 간단한 enterFrame 기반 Tween
function tweenTo(target, property, endValue, speed = 0.1) {
    const handler = (event) => {
        const current = target[property];
        const diff = endValue - current;

        if (Math.abs(diff) < 0.1) {
            target[property] = endValue;
            stage.removeEventListener("enterFrame", handler);
        } else {
            target[property] = current + diff * speed;
        }
    };

    stage.addEventListener("enterFrame", handler);
}

// 사용 예제
tweenTo(sprite, "x", 300, 0.15);  // x를 300으로 이동
tweenTo(sprite, "alpha", 0, 0.05);  // 페이드아웃
```

## 커스텀 이징

```typescript
// 베지어 곡선 기반 이징
function bezierEasing(x1, y1, x2, y2) {
    return (t) => {
        // 간이적인 3차 베지어 보간
        const cx = 3 * x1;
        const bx = 3 * (x2 - x1) - cx;
        const ax = 1 - cx - bx;

        const cy = 3 * y1;
        const by = 3 * (y2 - y1) - cy;
        const ay = 1 - cy - by;

        const sampleCurveY = (t) =>
            ((ay * t + by) * t + cy) * t;

        return sampleCurveY(t);
    };
}

// CSS cubic-bezier 상당
const customEase = bezierEasing(0.25, 0.1, 0.25, 1.0);
```

## 성능 힌트

1. **requestAnimationFrame 사용**: setTimeout보다 부드러움
2. **프로퍼티 변경 최소화**: 필요한 프로퍼티만 업데이트
3. **오브젝트 풀**: 대량의 Tween은 풀로 재사용
4. **완료 후 클린업**: 불필요한 리스너는 삭제

## 관련 항목

- [DisplayObject](/ja/reference/player/display-object)
- [이벤트 시스템](/ja/reference/player/events)
