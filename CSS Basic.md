## Animation

### __요소선정__
- 버벅거리거나 잘못 선택한 요소에서의 애니메이션은 사용자에게 거부감을 줄 수 있다. 성능과 적합성을 갖추어야 한다.
- 사용자의 활동을 불필요하게 방해하는 애니메이션을 피한다.
- 비용이 많이 드는 애니메이션 속성을 피한다. 
    - <span style="background-color:gray;">&nbsp;**box-shadow**&nbsp;</span> 을 변경하는 것은 텍스트 색상의 변경보다 비용이 더 크다.
    - 요소의 width를 변경하는 것은 transform을 변경하는 것보다 비용이 더 크다.

### __성능__
1. 애니메이션을 만들때마나 60fps를 유지해야 한다. 애니메이션이 버벅이거나 정지하면 사용자에게 거슬릴 수 밖에 없다. 
1. 레이아웃 변경이나 페인트를 유발하는 애니메이션 속성은 비용이 많이 든다. 
1. 가급적 변형 및 불투명도 변경을 고수하라
1. will-change를 사용하여 애니메이션 적용 대상을 브라우저가 알도록 하라.

요소의  <span style="background-color:gray;">&nbsp;**width, height**&nbsp;</span> 를 변경하는 레이아웃(Gecko는 리플로우)애니메이션은 가급적 피해야 하며, 최신 브라우저의 경우 애니메이션을 opacity 또는 transform으로 제한한다. 

### CSS 와 JS 애니메이션
CSS와 JS의 애니메이션을 구분하는 규칙은 다음과 같다.
- UI 요소 상태전환과 같은 Simple한 One-shot 전환에 CSS를 사용한다. 
- 바운스, 중지, 일시중지, 되감기 또는 감속과 같은 고급 효과는 JS를 사용한다. 
- JS 애니메이션을 사용하는 경우 최신 Web F/W 의 API를 사용한다. 

### CSS vs JS 성능비교
- CSS 기반 애니메이션 및 기본적인 웹 애니메이션은 Composite Thread에서 치리된다. 
- 이는 스타일 지정, 레이아웃, 페인트 및 자바스크립트가 실행되는 브라우저의 Main Thread와는 다르다. 즉 메인스레드가 실행중이어도 Composite Thread는 계속 실행된다. 그렇기 때문에 Composite Thread를 활용하는 Animation을 사용하는 것이 이득이다. (ex. jQuery .animate)

### will-change 속성 사용
<span style="background-color:gray;">&nbsp;**will-change**&nbsp;</span> 속성을 사용하여 어떤 요소의 속성을 변경하려 하는지 브라우저가 알도록 한다. 그러면 개발자가 변경을 수행하기 전에 브라우저가 최적화를 수행한다. 다만, 남용할 시 브라우저가 리소스를 낭비하여 더 많은 성능문제를 야기한다. 

일반적으로, 사용자 상호작용 및 어플리케이션의 상태로 인해 그 다음 200ms 이내에 애니메이션이 트리거 되는 경우 모든 요소에 적용하는게 좋다. 

현재 <span style="background-color:gray;">&nbsp;**will-change**&nbsp;</span> 속성은 [Chrome, Firefox, Opera](https://caniuse.com/#feat=will-change) 브라우저 에서 지원한다.

사용 예는 다음과 같다
~~~
.box {
  will-change: transform, opacity;
}
~~~

### easing
한 지점에서 다른 지점으로 이동할때 본질적으로 선형적으로 이동하지 않는다. 이러한 관점에서 easing은 애니메이션을 더욱 부드럽고 자연스럽게 만드는 프로세스 이다. UI 요소에 대해서 ease-out 애니메이션을 사용하고, ease-in, ease-in-out 애니메이션 사용은 사용자에게 굼뜬 느낌을 주므로 사용을 피해야 한다. 
easing을 사용하지 않은 애니메이션을 linear라고 한다. 

CSS에서 사용할 수 있는 일부는 다음과 같다.
- linear
~~~
transition: transform 500ms linear;
~~~
- ease-in: Slow Start, Fast End(부자연스러운 느낌으로 권장하지 않음)
~~~
transition: transform 500ms ease-in;
~~~
- ease-out: Fast Start, Slow End(사용자 인터페이스에 가장 적합)
~~~
transition: transform 500ms ease-out;
~~~
- ease-in-out: Slow Start, Fast Middle, Slow End
~~~
transition: transform 500ms ease-in-out;
~~~

### 적합한 easing 선택
- UI 요소에는 ease-out 애니메이션을 사용한다. [Quintic ease-out](http://easings.net/ko#easeOutQuint) 은 매우 빠르고 멋진 ease 이다.
- ease-out은 200 ~ 500ms 여야하고, elastic ease 는 800 ~ 1200ms 여야 한다. 
    - elastic ease는 적합한 곳에 드물게 사용하는게 맞다. 
- easing 유형의 전체 목록과 데모는 [easings.net](http://easings.net/ko) 을 참조한다.

### 비대칭 애니메이션 타이밍
비대칭 애니메이션 타이밍이란, 어떤 영역이 나타나고 사라지는 타이밍이 서로 다르게 설정되는 것을 의미한다. 
이 경우의 규칙은 다음과 같다. 
- 사용자의 상호작용으로 트리거되는 UI 애니메이션의 경우 Fast Show, Slow Hide를 설정한다. 
- 오류 또는 모달 뷰 등 코드로 트리거는되는 UI 애니메이션의 경우, Slow Show Fast Hide를 설정한다.




### 참고자료
[애니메이션 및 성능](https://developers.google.com/web/fundamentals/design-and-ux/animations/animations-and-performance)
[CSS 트리거](https://csstriggers.com/)
[High Performance Animations](https://www.html5rocks.com/en/tutorials/speed/high-performance-animations/)

## SVG <path>

[SVG <path>](https://www.w3schools.com/graphics/svg_path.asp)

timeupdate event 가 끝난 후 svg의 사이즈를 변경하기 위해서 path selector를 통해 바로 바꿔 줄 수 있을까 ?
