---
published: false
id: common
slug: common
title: Common
summary: common
toc: true
tags: ["react-dom-components"]
categories: ["react-dom-components"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# React Dom components

input, select, textarea는 value prop을 받아 controlled 할 수 있습니다.
props 이름은 camelCase를 사용합니다.
online convert를 활용할 수 있습니다.
- [html to jsx](https://transform.tools/html-to-jsx)
- [svg to jsx](https://transform.tools/)

colon 또한 camelCase로 변환해야합니다.
ex) xlink:href => xlinkHref

[Codesandbox](https://codesandbox.io/s/react-dom-api-test-5rkw39)

[Unitless 속성 값](https://github.com/facebook/react/blob/81d4ee9ca5c405dce62f64e61506b8e155f38d8d/packages/react-dom-bindings/src/shared/CSSProperty.js#L8-L57)이 아니라면 px이 자동으로 붙는다.

## div Props
accesskey
- Mac Chrome에서는 Control + Alt + accesskey로 실행

aria-*
- 웹접근성을 향상 시켜줌

autoCapitalize
- 입력값을 대문자로 변환시켜주는데, 실제로 동작하지는 않았음

contentEditable
- true이면, element의 값을 즉각 수정할 수 있음

data-*
- element에 string형태의 추가 데이터를 줄때 사용함, React에서는 일반적으로 props와 state를 활용하므로 사용하지 않음

enterKeyHint
- 가상 키보드(특히 모바일)의 엔터키 형태를 바꿔줌

hidden
- element를 숨기고 싶을때 사용

is
- custom element로 동작하도록 할때 사용

inputMode
- (가상)키보드의 형태를 지정할 때

itemProp
- 추가적인 meta data를 제공할 때

onAnimationIteration
- animation이 종료되고 다음 animation이 실행될때 마다 실행됨 (infinite에서도 계속 실행됨)

onAuxClick
- non-primary pointer button이 아닌 경우에 실행됨 (주로 primary pointer button은 마우스 좌클릭)

onBeforeInput
- input에 값이 수정되기 전에 실행됨 (아직은 beforeinput의 native를 사용하지는 않음)

onCompositionStart
- 문자 합성이 일어날때 (한글 입력할 때)

onCompositionEnd
- 문자 합성이 업데이트 될때 (한글 입력할 때)

onCompositionEnd
- 문자 합성이 끝날때 (한글 입력할 때)

onContextMenu
- context 메뉴가 열릴때 (주로 마우스 우클릭으로 열림)

onCopy
- 클립보드에 복사가 될때

onCut
- 클립보드에 오려두기가 될때

onPaste
- 클립보드에서 붙여넣기 될때

onDoubleClick
- 더블클릭 할때

onDrag
- 드래그 하고 있는 동안 계속 발생

onDragStart
- 동적으로 드래그가 안되게 막으려면 onDragStart에서 e.preventDefault()를 하면 됨

onDragEnter
- valid drop target에 drag element가 올려졌을때 한번실행 (Enter 했을때)

onDragOver
- valid drop target에 drag element가 올려져 있는동안 계속실행
- drop을 하기위해서는 onDragOver에 e.preventDefault() 처리를 해줘야함
- 기본적으로, 대부분의 요소는 드롭 타겟으로 설정되지 않음. 즉, 드래그된 요소를 놓을 수 없음
  ondragover 이벤트에서 event.preventDefault()를 호출하면, 이 기본 동작을 무시하고 그 요소가 드롭을 받을 수 있게 설정하는 것

onDrop
- valid drop target에 어떤것이 drop 되었을 때

onGotPointerCapture
- pointer 이벤트가 정상적으로 캡쳐되었을 때, 즉 pointer 이벤트를 성공했을 때

onLostPointerCapture
- pointer 캡쳐가 중단되었을 때

onPointerCancel
- pointer 이벤트가 취소되었을때

onPointerDown
- pointer가 active 상태가 되었을 때

onPointerUp
- pointer가 더 이상 active 상태가 아니게 되었을 때

onPointerMove
- pointer가 움직일때

onPointerEnter
- pointer가 element안에 들어갔을때

onPointerLeave
- pointer가 element 밖으로 나올때 (버블링 되지 않는다)

onPointerOut
- pointer가 element 밖으로 나올때 (버블링 된다)

onScroll
- 스크롤 발생했을 때 (버블링 되지 않는다)

onSelect
- input, textarea에서 텍스트 선택했을 때 발생 (e.target.selectionStart, e.target.selectionEnd로 선택한 위치 잡을 수 있음)
- div에서도 사용할 수는 있는데 contentEditable이 true인 경우에 발생한다.
- 단, div에서는 selectionStart, selectionEnd 가 없으므로, window.getSelection()을 사용해야한다.

onTouchCancel
- touch 동작이 취소되었을 때 발생하는데, 개발자가 의도적으로 발생시키는 것은 아니고 일반적으로 앱이 느려지면 운영체제가 발생시킴

onTransitionEnd
- transition이 끝날때마다 발생한다. 각 속성마다 한번씩 발생하므로 `border-radius: 50%`로 하면 각 모서리마다 발생하여 4번이 발생함

role
- HTML 요소의 역할을 명시하는 문자열로 웹 접근성을 향상시킴
- 웹표준에서 기본적으로 role이 정의되어있으므로, 기본역할이 아닌 다른 역할을 주고 싶을 때 사용

slot
- shadow DOM의 슬롯 이름을 명시하는 문자열 (정확히 뭔지 모르겠음)

spellCheck
- input, textarea에서 철자검사를 해줌, true인 경우 단어가 틀리면 빨간줄이 그어짐(근데 나는 안됨, 이유는 모름)
- 보안과 관련된 단어들에는 false를 주는것이 좋음

tabIndex
- Tab키의 동작을 재정의하는 숫자로 기본적으로 -1, 0은 피해야

title
- 툴팁 텍스트를 명시하는 문자열 (hover over하면 텍스트가 표시됨)

translate
- yes 혹은 no로 no를 전달하면 번역에서 제외됨, 일반적으로 자동 번역 시스템에 의한 번역을 방지하는 경우 사용



































## form Props

onReset
- form에서 reset 버튼이 실행되었을 때

onSubmit
- formd에서 submit 버튼이 실행되었을 때

## dialog Props

onCancel
- dialog를 사용자가 무시하려는 경우에 발생하는데, 보통 esc를 누르면 dialog가 닫히면서 발생함

onClose
- dialog를 닫는 경우 발생 (close 메소드 사용) 

## details Props

onToggle
- details가 toggle될때 실행됨, e.target.open으로 현재 toggle 상태를 알 수 있음


## img, iframe, object, embed, link, image(svg) Props
버블링이 발생함

onLoad
- 리소스가 정상적으로 로드가 완료되었을 때

onError
- 리소스가 로드되지 못했을 때

## audio, video Props

### 최초에 비디오 로드될때
- LoadStart
- Progress
- Suspend
- DurationChange
- Resize
- LoadedMeta
- LoadedData
- CanPlay
- CanPlayThrough

### 비디오 링크가 잘못되었을 때(빈 문자열로 넣었을 때)
- Abort
- Emptied
- TimeUpdate
- LoadStart
- Error

### 비디오 링크가 다시 셋팅 되었을 때
- Emptied
- TimeUpdate
- LoadStart
- Progress
- Suspend
- DurationChange
- Resize
- LoadedMeta
- LoadedData
- CanPlay
- CanPlayThrough

### 재생 위치를 변경하였을 때
- Seeking
- Progress
- Suspend
- TimeUpdate
- Seeked
- CanPlay
- CanPlayThrough

onAbort
- 리소스가 완전히 로드되지 않았을 때 발생합니다. 이는 오류 때문이 아닌 다른 이유로 인해 발생합니다.

onCanPlay
- 충분한 데이터가 로드되어 재생을 시작할 수 있을 때 발생합니다. 하지만 버퍼링 없이 끝까지 재생할 수 있을 만큼의 데이터는 아닙니다.

onCanPlayThrough
- 충분한 데이터가 로드되어 버퍼링 없이 끝까지 재생 가능할 것으로 예상될 때 발생합니다.

onDurationChange
- 미디어의 총 재생 시간이 변경되었을 때 발생합니다.

onEmptied
- 미디어가 비워졌을 때 (모든 데이터가 제거되었을 때) 발생합니다.

onEncrypted
- 브라우저가 암호화된 미디어를 발견했을 때 발생합니다.

onEnded
- 재생이 끝났을 때 발생합니다. 이는 미디어를 더 이상 재생할 수 없을 때 발생합니다.

onError
- 리소스를 로드하는 데 실패했을 때 발생합니다.

onLoadedData
- 현재 재생 프레임이 로드되었을 때 발생합니다.

onLoadedMetadata
- 메타데이터가 로드되었을 때 발생합니다.

onLoadStart
- 리소스 로드가 시작되었을 때 발생합니다.

onPause
- 미디어가 일시 중지되었을 때 발생합니다.

onPlay
- 미디어가 일시 중지 상태에서 벗어났을 때 발생합니다.

onPlaying
- 미디어 재생이 시작되거나 재시작되었을 때 발생합니다.

onProgress
- 리소스 로드 중에 주기적으로 발생합니다.

onRateChange
- 재생 속도가 변경되었을 때 발생합니다.

onResize
- 비디오 크기가 변경되었을 때 발생합니다.

onSeeked
- 탐색(Seek) 작업이 완료되었을 때 발생합니다.
- "탐색작업이 시작되었다"는 표현은 비디오 또는 오디오 콘텐츠에서 재생 위치를 변경하는 동작을 의미합니다. 이것은 사용자가 재생 바를 드래그하거나, 특정 시간으로 건너뛰는 등의 동작을 했을 때 발생합니다.
- 예를 들어, 사용자가 비디오의 특정 부분을 보기 위해 진행바를 드래그하면, 이러한 동작은 '탐색'으로 분류됩니다. 이 때 'onSeeking' 이벤트가 발생하며, 사용자가 탐색을 시작한 시점을 알 수 있습니다.
- 그리고 이 '탐색' 동작이 완료되면, 즉 사용자가 드래그를 끝내고 새로운 재생 위치가 확정되면 'onSeeked' 이벤트가 발생합니다. 이를 통해 탐색이 완료된 시점을 알 수 있습니다.
- 따라서 'onSeeking'과 'onSeeked' 이벤트는 사용자가 재생 위치를 변경하는 동작의 시작과 끝을 감지하는데 사용됩니다.

onSeeking
- 탐색(Seek) 작업이 시작되었을 때 발생합니다.

onStalled
- 브라우저가 데이터를 기다리고 있지만 계속 로드되지 않을 때 발생합니다.

onSuspend
- 리소스 로드가 일시 중지되었을 때 발생합니다. 

onTimeUpdate
- 현재 재생 시간이 업데이트될 때 발생합니다.

onVolumChange
- 볼륨이 변경되었을 때 발생합니다.

onWaiting
- 일시적으로 데이터가 부족하여 재생이 중지되었을 때 발생합니다.


## Caveat
- children와 dangerouslySetInnerHTML은 동시에 사용할 수 없습니다.
- 특정 이벤트들은 브라우저에서는 버블링 되지 않지만, 리액트에서는 버블될 수 있습니다. (예를들어, onAbort와 onLoad)

## ref callback function
ref에 함수를 직접 넣으면 랜더링 될때 마다 callback이 실행되게 됩니다.
리랜더될때마다 callback 함수가 다르다면, detach와 re-attach를 매 랜더링마다 하게 될 것입니다.

```ts
<div ref={(node) => console.log(node)} />
```
node에는 DOM node나 null이 들어갑니다.
detach가 될때는 null이 들어가게 됩니다.

ref callback에는 return 값이 없어야 합니다.

## React event object

리액트에서는 event 객체를 Synthetic event라고 합니다.
nativeEvent와 동일하지는 않고 일부 브라우저 비일관성을 고치기 위해서 수정되었습니다.
예를들어, onMouseLeave는 e.nativeEvent에서는 mouseout 이벤트를 가리킵니다.
상세한 매핑은 공개적인 API가 아니므로 미래에 변경될 수 있습니다.

리액트 event 객체는 아래 표준 event 속성을 포함합니다.
- bubbles : boolean, 버블링이 되었는지 여부
- cancelable: boolean, 이벤트 취소 여부
- currentTarget: DOM node, 리액트 트리에서 현재 핸들러가 실행된 노드
- target: DOM node, 실제로 이벤트를 발생시킨 노드(자식 노드인 경우에는 자식노드로 나옴)
- defaultPrevented: boolean, preventDefault의 실행여부
- eventPhase: 현재 이벤트의 [Phase](https://developer.mozilla.org/en-US/docs/Web/API/Event/eventPhase)
- isTrusted: boolean, 유저에 의해 실행되었는지 여부
- timeStamp: number, 이벤트 실행된 시각

추가적으로
- nativeEvent: DOM event로 오리지널 브라우저의 이벤트 객체

리액트는 아래 추가 메소드를 포함합니다.
- isDefaultPrevented(): boolean을 리턴, preventDefault가 실행되었는지 여부
- isPropagationStopped(): boolean을 리턴, stopPropagation이 실행되었는지 여부

리액트의 이벤트 객체와 네이티브 객체는 다를 수 있습니다.
예를들어, e.currentTarget과 e.nativeEvent.currentTarget은 다를 수 있고,
polyfill된 이벤트에서는 e.type과 e.nativeEvent.type이 다를 수 있습니다.

## AnimationEvent Parameters

추가 속성
- animationName
- elapsedTime
- pseudoElement





























