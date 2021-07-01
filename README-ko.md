# DirectorViewMV

DirectorViewMV는 간단한 카메라 플러그인입니다.

만든 이 : [Creta Park](https://creft.me/cretapark)  
<a href="https://www.buymeacoffee.com/CretaPark" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" width="120" height="26"></a>

이 플러그인은 RPG 만들기(쯔꾸르) 생태계에 기여하는 Creta Park의 프로젝트 중 하나입니다.

[인과율 (因果律)](https://store.steampowered.com/app/1158720/Causality)에서 사용됐습니다.

> 원래 쓰이던 환경의 제약으로 인해 원하는 기능이 지원되지 않거나 없을 수 있습니다.  
> 사용하기 전에 사양 항목을 잘 읽어주세요.

# 사양

- 앞서가는 카메라 뷰 지원  
  플레이어가 움직이기 시작하면, 카메라가 부드럽게 앞서나가 보여주는 기능입니다.  
  이 기능은 플레이어가 설정에서 끌 수 있습니다.

- 앵커 기반 카메라 뷰  
  특정 이벤트나 장소가 앵커로 등록되면, 카메라가 모든 앵커를 담기 위해 움직입니다.

- 카메라 트위닝 이동  
  원하는 위치를 향해 카메라를 부드럽게 움직이는 등의 연출 기능이 있습니다.

- 지도 반복을 지원하지 않음  
  인과율의 대부분의 맵은 단 하나의 맵(엔딩)을 제외하고 반복되지 않는 구조를 가지고 있습니다.  
  이것은 부분적으로 작동하는데, 이는 플레이어가 이동하고 있으면 부드러운 움직임이 내부적으로 점점 없애기 때문입니다.

- 확대 및 축소 기능을 지원하지 않음  
  저희가 인과율을 개발했을 땐, 제약이 심한 모바일 출시도 고려해야 했었습니다.  
  이 때문에 저희 게임의 해상도는 320 : 180픽셀이 되었기에, 확대 및 축소 기능을 고려하지 않았습니다.

- Yanfly 옵션 코어를 지원하지 않음  
  저희는 당시에 기본 옵션 창을 사용했었기 때문에, 아직은 지원하지 않습니다.

# 목차

- 1\. [준비하기](#1-준비하기)
  - 2\. [사용하는 법](#2-사용하는-법)
    - 2.1\. [앵커 관리하기](#21-앵커-관리하기)
      - 2.1.1\. [매개변수](#211-매개변수)
      - 2.1.2\. [고정 앵커(Fixed anchor) 만들기](#212-고정-앵커fixed-anchor-만들기)
      - 2.1.3\. [이벤트 앵커(Event anchor) 만들기](#213-이벤트-앵커event-anchor-만들기)
      - 2.1.4\. [앵커 제거하기](#214-앵커-제거하기)
    - 2.2\. [카메라워크 전환](#22-카메라워크-전환)
      - 2.2.1\. [매개변수](#221-매개변수)
      - 2.2.2\. ['위치'에서 '위치'로 이동](#222-위치에서-위치로-이동)
      - 2.2.3\. ['위치'로 이동](#223-위치로-이동)
      - 2.2.4\. ['앵커의 위치'로 이동](#224-앵커의-위치로-이동)
      - 2.2.5\. ['이벤트 위치'로 이동](#225-이벤트-위치로-이동)
      - 2.2.6\. ['플레이어 위치'로 이동](#226-플레이어-위치로-이동)
    - 2.3\. [카메라 시스템 관리](#23-카메라-시스템-관리)
      - 2.3.1\. [오프셋](#231-오프셋)
      - 2.3.2\. [앞서가는 카메라 오프셋](#232-앞서가는-카메라-오프셋)
      - 2.3.3\. [앞서가는 카메라 사용](#233-앞서가는-카메라-사용)
      - 2.3.4\. [어색하지 않게 플레이어 위치 변경하기](#234-어색하지-않게-플레이어-위치-변경하기)
      - 2.3.5\. [초기 값으로 돌려놓기](#235-초기-값으로-돌려놓기)
- 3\. [플러그인 설정](#3-플러그인-설정)
  - 3.1\. [Default offset X, Y](#31-default-offset-x-y)
  - 3.2\. [Default lookahead view](#32-default-lookahead-view)
  - 3.3\. [Default lookahead X, Y](#33-default-lookahead-x-y)
  - 3.4\. [Lookahead delay](#34-lookahead-delay)
  - 3.5\. [Lookahead duration](#35-lookahead-duration)
  - 3.6\. [Anchor transition duration](#36-anchor-transition-duration)
  - 3.7\. [Use accurate anchor check](#37-use-accurate-anchor-check)
  - 3.8\. [Option label : Use fixed camera](#38-option-label--use-fixed-camera)
- [4. 타사 라이브러리 / 소스 고지](#4-타사-라이브러리--소스-고지)

# [1.][toc] 준비하기

이 플러그인을 사용하고 싶은 프로젝트에 추가하세요.  
정말 쉽죠?

이 문서에서의 모든 예시는 맵의 크기가
RPG 만들기 MV의 기본 맵 크기의 두배인 상황으로 가정합니다.  
(가로 32, 세로 26)

# [2.][toc] 사용하는 법

DirectorView는 앵커 중심의 카메라 시스템입니다,
각 앵커에는 고유한 `앵커 id`와 `활성화 범위` 데이터를 가집니다.  
플레이어 캐릭터는 기본적으로 0번 id를 가진 앵커를 가집니다.

## [2.1.][toc] 앵커 관리하기

여러가지 방법으로 앵커들을 관리할 수 있습니다.  
앵커는 이벤트에 할당하거나 특정 위치에 만들 수 있습니다.

### [2.1.1.][toc] 매개변수

* `앵커 id`  
   - 각 앵커를 구분하는 고유한 번호 입니다.
   - 앵커들은 이 값을 통해 만들거나 제거할 수 있습니다.
   - 앵커를 만들 때, 이미 존재하는 id를 사용해 만들 경우, 해당하는 앵커는 덮어씌워집니다.
   - 카메라는 기본적으로 항상 0번 앵커를 `활성화 범위`에 상관없이 따릅니다.

* `활성화 범위`  
   - 플레이어 캐릭터가 앵커의 `활성화 범위` 이내로 들어오면 활성화가 되어  
     카메라가 해당 앵커를 화면에 같이 담기 위해 움직입니다.
   - 반대로, 이 범위를 벗어나면 앵커는 비활성화 상태가 되며,
     카메라가 해당 앵커를 더 이상 같이 화면에 담지 않습니다.

### [2.1.2.][toc] 고정 앵커(Fixed anchor) 만들기

고정 앵커는 특정 위치에서 움직이지 않는 앵커입니다.  
원하는 풍경을 집중해서 담거나 할 때 유용합니다.

1. 맵의 메모를 이용해서 만들기
   
   > *템플릿 코드 : 메모*
   > ```
   > DV_Anchor[fixed, <앵커 id>, <x>, <y>, <활성화 범위>]
   > ```
   
   고정 앵커를 추가하고 싶은 맵을 우클릭 한 후, 편집 창을 엽니다.  
   그리고 문법을 따라 아래와 같은 메모를 추가하세요.
   ```
   DV_Anchor[fixed, 1, 8.5, 6.5, 3]
   ```
   
   > 맵의 메모를 통해 만들게 되면, 플레이어가 해당 맵에 진입했을 때
   > 항상 앵커가 만들어집니다.
   
2. 스크립트 이벤트를 이용해서 만들기
   
   > *템플릿 코드 : 스크립트*
   > ```
   > DirectorView.AddFixedAnchor(<앵커 id>, <x>, <y>, <활성화 범위>);
   > ```
   
   스크립트 이벤트를 만들고, 문법을 따라 아래와 같은 코드를 추가하세요.
   ```js
   DirectorView.AddFixedAnchor(1, 8.5, 6.5, 3);
   ```
   
   > 이것을 이용하면 특정한 시점에 앵커를 만들 수 있습니다.  
   > 단, 맵에서 벗어나게 되면 해당 앵커는 제거됩니다.

> 두 방법 모두 '`앵커 id`가 `1`이고, `활성화 범위`가 `3`인
> `고정 앵커`를 (`8.5`, `6.5`)에 해당하는 위치에 만든다'를 의미합니다.

### [2.1.3.][toc] 이벤트 앵커(Event anchor) 만들기

이벤트 앵커는 이벤트와 함께 움직이는 앵커입니다.  
캐릭터나 상호작용이 가능한 요소에 집중하고 싶거나 할 때 사용할 수 있습니다.

1. 이벤트 메모를 이용해서 만들기
   
   > *템플릿 코드 : 메모*
   > ```
   > DV_Anchor[<앵커 id>, <활성화 범위>]
   > ```
   
   앵커를 추가하고 싶은 이벤트를 열고, 메모 칸에 문법을 따라 아래와 같은 메모를 추가하세요.
   ```
   DV_Anchor[1, 2]
   ```
   
   > 자주하는 실수가 여기에서 발생합니다, 꼭 이벤트의 메모 칸에 작성해야 됩니다.  
   > 실행 내용에 들어가는 메모(노트 이벤트)에서는 작동하지 않습니다!
   
   > 이벤트의 메모를 통해 만들어지는 앵커는,
   > 플레이어가 맵에 진입했을 때 항상 만들어집니다.  
   > 단, 모든 이벤트 페이지가 활성화 되는 조건이 없을 경우에는 예외입니다.

2. 스크립트 이벤트를 이용해서 만들기
   
   > *템플릿 코드 : 스크립트*
   > ```
   > DirectorView.AddMapEventAnchor(<앵커 id>, <이벤트 id>, <활성화 범위>);
   > ```
   
   스크립트 이벤트를 만들고, 문법을 따라 아래와 같은 코드를 추가하세요.
   ```js
   DirectorView.AddMapEventAnchor(1, 1, 2);
   ```
   
   > 이것을 이용하면 특정한 시점에 앵커를 만들 수 있습니다.  
   > 단, 맵에서 벗어나게 되면 해당 앵커는 제거됩니다.

3. 맵의 메모를 이용해서 만들기
   
   > *템플릿 코드 : 메모*
   > ```
   > DV_Anchor[<앵커 id>, <이벤트 id>, <활성화 범위>]
   > ```
   
   앵커를 추가하고 싶은 이벤트를 열고, 메모 칸에 문법을 따라 아래와 같은 메모를 추가하세요.
   ```
   DV_Anchor[1, 1, 2]
   ```
   
   > 맵의 메모를 통해 만들게 되면, 플레이어가 해당 맵에 진입했을 때
   > 항상 앵커가 만들어집니다.

> 이 예시들은 '`앵커 id`가 `1`이고, `활성화 범위`가 `2`인
> `이벤트 앵커`를 `이벤트 id`가 1인 이벤트에 추가한다' 는 것을 의미합니다.  
> 단, **첫번째 방식 (이벤트 메모)**에서는 추가하고 싶은 이벤트에
> 직접적으로 만드는 것이기 때문에, `이벤트 id`를 쓰는 과정을 생략합니다.

### [2.1.4.][toc] 앵커 제거하기

앵커를 추가하다 보면, 앵커들을 제거해야 할 필요가 있음을 깨닫게 됩니다.

> *템플릿 코드 : 스크립트*
> ```
> DirectorView.RemoveAnchor(<앵커 id>);
> ```

스크립트 이벤트를 만들고, 문법을 따라 아래와 같은 코드를 추가하세요.
```js
DirectorView.RemoveAnchor(1);
```

이 스크립트 이벤트는 실행됐을 때, `앵커 id`가 1인 앵커를 제거 할 겁니다.

> 이 기능은 일시적으로 앵커를 지웁니다.  
> 맵의 메모나 이벤트의 메모를 통해 만든 앵커는
> 플레이어가 맵에 진입 할 때마다 만들어지기 때문에, 주의해서 사용하세요.

## [2.2.][toc] 카메라워크 전환

DirectorView는 부드럽게 특정 지점에서 원하는 위치까지
부드럽게 움직이는 기능을 가지고 있습니다.

이곳의 모든 예시는 `앵커 id`가 1인 앵커가 만들어져 있고,
두 개의 이벤트가 있다고 가정합니다.  
또한, 모든 예시는 두번째 이벤트에서 실행하는 것으로 가정합니다.

> 여기에서 소개되는 모든 기능은 카메라가 이동을 끝내기까지 이벤트 실행을 멈추지 않습니다.  
> 카메라가 움직이는 동안 제작자가 원하는 카메라 연출이 있을 수 있기 때문입니다.  
> 주의해서 사용하세요.

### [2.2.1.][toc] 매개변수

* `~에서`  
  카메라워크 전환이 시작되어야 하는 위치입니다.

* `~으로`  
  카메라워크 전환이 도착해야 하는 위치입니다.

* `길이`  
  카메라 워크가 움직임을 완료하기까지 걸리는 시간입니다.  
  RPG 만들기의 시간 단위와 동일하기 때문에, 60은 1초가 걸린다는 것을 의미합니다.

* `전환 방식`  
  카메라 워크가 어떻게 움직여야 하는 지의 동작을 정합니다.  
  대부분의 경우 부드럽게 카메라가 움직이는 용도로 사용합니다.  
  [easings.net](https://easings.net/ko)에서 각 전환방식들이 어떤 식으로 움직이는 지 확인하세요.
  
  > *지원되는 전환 방식들*
  > ```
  > Linear
  > EaseInQuad
  > EaseOutQuad
  > EaseInOutQuad
  > EaseInCubic
  > EaseOutCubic
  > EaseInOutCubic
  > EaseInQuart
  > EaseOutQuart
  > EaseInOutQuart
  > EaseInQuint
  > EaseOutQuint
  > EaseInOutQuint
  > EaseInExpo
  > EaseOutExpo
  > EaseInOutExpo
  > ```
  
  만약에 어떤 `전환 방식`이 좋은 지 모르겠다면 `EaseInOutQuart`를 추천합니다.  
  인과율에서는 거의 모든 전환에 이 방식을 사용했습니다.

### [2.2.2.][toc] '위치'에서 '위치'로 이동

> *템플릿 코드 : 스크립트*
> ```
> DirectorView.StartTween(<x에서>, <y에서>, <x으로>, <y으로>, <전환 방식>, <길이>);
> ```

카메라를 `x, y`에서 `x, y`로 전환합니다.

스크립트 이벤트를 만들고, 문법을 따라 아래와 같은 코드를 추가하세요.
```js
DirectorView.StartTween(8.5, 6.5, 25.5, 19.5, 'EaseInOutQuart', 60);
```

그리고 그 다음에 60인 대기 이벤트를 만들어주세요.  
이벤트를 실행하면, 카메라가 (`8.5`, `6.5`)에서 (`25.5`, `19.5`)으로
1초간 부드럽게 움직이는 것을 볼 수 있습니다.

### [2.2.3.][toc] '위치'로 이동

> *템플릿 코드 : 스크립트*
> ```
> DirectorView.StartTweenFromCamera(<x으로>, <y으로>, <전환 방식>, <길이>);
> ```

카메라를 현재 위치에서 `x, y`로 전환합니다.

스크립트 이벤트를 만들고, 문법을 따라 아래와 같은 코드를 추가하세요.
```js
DirectorView.StartTweenFromCamera(25.5, 19.5, 'EaseInOutQuart', 60);
```

그리고 그 다음에 60인 대기 이벤트를 만들어주세요.  
이벤트를 실행하면, 카메라가 현재 위치에서 (`25.5`, `19.5`)으로
1초간 부드럽게 움직이는 것을 볼 수 있습니다.

### [2.2.4.][toc] '앵커의 위치'로 이동

> *템플릿 코드 : 스크립트*
> ```
> DirectorView.StartTweenToAnchor(<앵커 id>, <전환 방식>, <길이>);
> ```

카메라를 현재 위치에서 `앵커의 위치`로 전환합니다.

스크립트 이벤트를 만들고, 문법을 따라 아래와 같은 코드를 추가하세요.
```js
DirectorView.StartTweenToAnchor(1, 'EaseInOutQuart', 60);
```

그리고 그 다음에 60인 대기 이벤트를 만들어주세요.  
이벤트를 실행하면, 카메라가 현재 위치에서 지정된 `앵커 id`를 가진 앵커의 위치로
1초간 부드럽게 움직이는 것을 볼 수 있습니다.

### [2.2.5.][toc] '이벤트 위치'로 이동

> *템플릿 코드 : 스크립트*
> ```
> DirectorView.StartTweenToEvent(<이벤트 id>, <전환 방식>, <길이>);
> ```

카메라를 현재 위치에서 `이벤트의 위치`로 전환합니다.

스크립트 이벤트를 만들고, 문법을 따라 아래와 같은 코드를 추가하세요.
```js
DirectorView.StartTweenToEvent(1, 'EaseInOutQuart', 60);
```

그리고 그 다음에 60인 대기 이벤트를 만들어주세요.  
이벤트를 실행하면, 카메라가 현재 위치에서 지정된 `이벤트 id`를 가진 이벤트의 위치로
1초간 부드럽게 움직이는 것을 볼 수 있습니다.

### [2.2.6.][toc] '플레이어 위치'로 이동

> *템플릿 코드 : 스크립트*
> ```
> DirectorView.StartTweenToPlayer(<전환 방식>, <길이>);
> ```

카메라를 현재 위치에서 `플레이어 캐릭터의 위치`로 전환합니다.

스크립트 이벤트를 만들고, 문법을 따라 아래와 같은 코드를 추가하세요.
```js
DirectorView.StartTweenToPlayer('EaseInOutQuart', 60);
```

그리고 그 다음에 60인 대기 이벤트를 만들어주세요.  
이벤트를 실행하면, 카메라가 현재 위치에서 플레이어 캐릭터의 위치로
1초간 부드럽게 움직이는 것을 볼 수 있습니다.

## [2.3.][toc] 카메라 시스템 관리

DirectorView는 여러가지 시스템 기능을 제공합니다.  
*앞서가는 카메라 뷰를 키거나 끄기, 오프셋 등...*

> 여기에서 소개되는 기능들은 각 게임의 세이브에 보관되고 불러와집니다.  
> 주의해서 사용하세요.

### [2.3.1.][toc] 오프셋

오프셋은 카메라가 집중 대상으로부터 얼만큼 이동한 위치에서 보고 있을 지 지정합니다.

> RPG 만들기의 지도 스크롤 이벤트의 기능이 이 오프셋을 사용하도록 바뀌었습니다.  
> 주의해서 사용하세요.

스크립트 이벤트에서 아래와 같은 코드를 통해 오프셋을 지정할 수 있습니다.
```js
DirectorView.Offset.X = 1;
DirectorView.Offset.Y = 1;
```

이 코드는 곧바로 카메라의 오프셋의 X Y를 1로 지정합니다.  
오프셋의 단위는 타일 기반이기 때문에 1은 1칸을 의미합니다.

또한, 위에서 언급했듯이, 지도 스크롤 이벤트를 통해 조정할 수 있습니다.

이걸 아래의 코드를 통해 지도 스크롤 이벤트 없이
직접적으로 부드럽게 오프셋을 전환할 수 있습니다.
> *템플릿 코드 : 스크립트*
> ```js
> DirectorView.StartOffsetMove(<x>, <y>, <길이>);
> ```

```js
DirectorView.StartOffsetMove(1, 1, 60);
```

이 코드는 카메라 오프셋에 X + 1, Y + 1만큼의 오프셋으로 1초 동안 전환하도록 만듭니다.

### [2.3.2.][toc] 앞서가는 카메라 오프셋

앞서가는 카메라 오프셋은 플레이어가 움직일 때,
카메라가 해당하는 방향만큼 얼만큼 더 앞으로 나아갈 것인지 지정합니다.

스크립트 이벤트에서 아래와 같은 코드를 통해 앞서가는 카메라 오프셋을 지정할 수 있습니다.
```js
DirectorView.LookaheadOffset.H = 0;
DirectorView.LookaheadOffset.V = 5;
```
*\* H 는 가로를, V 는 세로를 의미합니다.*

이 코드는 가로 오프셋을 0으로, 세로 오프셋을 5로 지정합니다.  
가로로 움직이면 카메라는 앞서가지 않지만,
세로로 움직이면 카메라가 앞서가는 것을 볼 수 있습니다.

> 이 기능은 플레이어가 설정에서 `고정된 카메라 사용`을 킨 경우, 시각적으로 보이지 않습니다.

### [2.3.3.][toc] 앞서가는 카메라 사용

플레이어가 움직일 때 카메라가 앞서갈 지 지정합니다.

스크립트 이벤트에서 아래와 같은 코드를 통해 앞서가는 카메라 사용 여부를 지정할 수 있습니다.
```js
DirectorView.UseLookahead = false;
//또는
DirectorView.UseLookahead = true;
```
*\* true는 활성화, false 는 비활성화를 의미합니다.*

비활성화를 하게 되면, 카메라는 플레이어를 앞서가지 않습니다.  
다만, 여전히 부드럽게 따라갑니다.

> 이 기능은 플레이어가 설정에서 `고정된 카메라 사용`을 킨 경우, 시각적으로 보이지 않습니다.

### [2.3.4.][toc] 어색하지 않게 플레이어 위치 변경하기

DirectorView는 플레이어 캐릭터의 위치를 변경하면 내부 값들을 모두 리셋해
스크린 중심에 플레이어 캐릭터가 보이도록 합니다.

다만, 특정 시나리오에서는 좋지 않을 수 있습니다.  
예를 들어, 제작자가 조용히 플레이어가 알 수 없게 다른 장소로 옮기거나 하고 싶을 때
이러한 동작은 원치 않을 수 있기 때문입니다.

> *템플릿 코드 : 스크립트*
> ```
> $gamePlayer.setPosition(<x>, <y>);
> $gamePlayer.center(<x>, <y>, true);
> ```

이 코드 조각을 스크립트 이벤트에 넣어 원하는 지점으로 플레이어 캐릭터를
카메라와 함께 위치를 변경할 수 있습니다.

> 저희가 인과율을 개발했을 때, 파티 멤버를 사용 할 필요가 없었습니다.  
> 따라서 저는 이 기능에 대해서 작업하지 않았기 때문에
> 파티 멤버가 있다면 어색하게 보일 수 있습니다.

### [2.3.5.][toc] 초기 값으로 돌려놓기

위의 설정값들을 게임 내에서 조정하다 보면, 원래의 설정값을 필요로 할 때가 있습니다.

스크립트 이벤트에 아래의 코드를 넣으세요.
```js
DirectorView.ResetToInitialPluginValue();
```

이 코드가 실행되면 조정된 모든 설정 값이 초기 값으로 돌아옵니다.

# [3.][toc] 플러그인 설정

## [3.1.][toc] Default offset X, Y

초기 카메라의 오프셋 값입니다.

기본 값은 `0` 입니다.

> 이 값은 타이틀 화면으로 돌아오거나 `ResetToInitialPluginValue`를 실행하면 적용됩니다.

## [3.2.][toc] Default lookahead view

초기 카메라의 앞서가기를 사용할 것인지 지정하는 값입니다.

기본 값은 `true (ON)` 입니다.

> 이 값은 타이틀 화면으로 돌아오거나 `ResetToInitialPluginValue`를 실행하면 적용됩니다.

## [3.3.][toc] Default lookahead X, Y

초기 카메라의 앞서가기 오프셋 값입니다.

기본 값은 `2.5` 입니다.

> 이 값은 타이틀 화면으로 돌아오거나 `ResetToInitialPluginValue`를 실행하면 적용됩니다.

## [3.4.][toc] Lookahead delay

카메라의 앞서가기가 시작하기 전에 기다리는 시간입니다.  
카메라의 앞서가기는 플레이어가 이동하자마자 바로 시작하지 않습니다,
플레이어가 이 값만큼의 프레임을 이동하면 앞서가기가 시작됩니다.

RPG 만들기의 시간 단위와 동일하며 (60 = 1초), 기본 값은 `15` 입니다.

## [3.5.][toc] Lookahead duration

카메라의 앞서가기가 시작되어 끝까지 도달하는 데 걸리는 시간입니다.

RPG 만들기의 시간 단위와 동일하며 (60 = 1초), 기본 값은 `30` 입니다.

## [3.6.][toc] Anchor transition duration

활성화 된 앵커의 개수가 변경되면 카메라는 부드럽게
현재 위치에서 새 앵커들을 화면에 다 담을 수 있는 위치로 전환합니다.  
이 설정은 전환하는 데 걸리는 시간을 지정합니다.

If turns into only player's anchor left then it'll use half of this value for transition.

RPG 만들기의 시간 단위와 동일하며 (60 = 1초), 기본 값은 `60` 입니다.

## [3.7.][toc] Use accurate anchor check

DirectorView는 기본적으로 앵커의 활성화 범위를 사각형 충돌 검사 (AABB)를 사용합니다.  
이 기능을 사용하면, 실제 거리에 따라 활성화를 하도록 변경합니다.  
활성화 범위가 사각형에서 원형으로 변경됩니다.

기본 값은 `false (OFF) 입니다`

## [3.8.][toc] Option label : Use fixed camera

게임 내 옵션 메뉴에서 표시될 `고정된 카메라 사용` 설정에 표시될 텍스트입니다.

이 옵션은 카메라 움직임으로 인해 멀미를 느낄 수 있는 플레이어를 위한 접근성 옵션입니다.  
게임 내에서 활성화되면 카메라가 기본적으로 완전한 RPG Maker 스타일을 사용한 방식으로 움직입니다.

# [4.][toc] 타사 라이브러리 / 소스 고지

 - [gre/easing.js](https://gist.github.com/gre/1650294)


[toc]: #목차