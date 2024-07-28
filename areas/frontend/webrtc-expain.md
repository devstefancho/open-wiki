---
published: false
id: webrtc-expain
slug: webrtc-expain
title: Webrtc Expain
summary: webrtc expain
toc: true
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2024-07-28
updatedDate: 2024-07-28
---

# WebRTC

## WebRTC 란?

서버없이 브라우저간에 통신으로 영상, 오디오 파일을 교환하는 방식
최초에는 ip 주소, 브라우저 정보가 없기 때문에 SDP, ICE를 교환이 필요하다.

**SDP**
- Codec 정보, 소스주소, video, audio 정보들

**ICE**
- A,B 간의 연결이 되지 못하는 여러가지 이유들이 있음
- 서로 연결을 가능하게 하는 정보들을 교환하는것
- 이런 연결을 해주기 위해서 STUN 서버라는 것을 사용한다.

## 사용자 카메라, 마이크 접근

**API**
- getUserMedia

```javascript
navigator.mediaDevices.getUserMedia(
  { audio: true, video:true
});
```

```javascript
// 해상도를 설정할 때
getUserMedia({
  audio: true,
  video: { width: 1280, height: 720 },
});
```

## 화면공유

**API**
- getDisplayMedia

```typescript
const handleClick = async () => {
  const stream = await navigator.mediaDevices.getDisplayMedia({
    video: true,
    audio: true,
  });

  const video = videoRef.current;
  if (!video) {
    return;
  }

  video.srcObject = stream;
};
```

## Peer간 WebRTC 연결

**API**
- RTCPeerConnection : 브라우저간 연결과정에서 사용

stream 정보(video, audio)인 SDP를 교환하고
SDP 교환이 완료되면, 연결(ip주소, 방화벽 통과 등)을 위해 ICE 후보를 생성하여 교환한다.
이런 과정을 Signaling이라고 부른다.


여러명을 연결해야하는 경우, 위 과정이 동시에 일어나야한다.
예를 들면, 10명이 접속한 경우 10명이 서로 SDP, ICE 교환을 해야한다.
이 과정을 안정성있게 처리해주는 역할을 하는게 Agora SDK (agora-rtc-sdk-ng) 이다.


## 다중연결 방식

- Mesh 방식
- SFU(Selective Forwarding Unit) 방식
- MCU(Multi-point Control Unit) 방식


## 화상회의를 만들때 주의해야할 점

- 랜더링 최적화
  - 여러 비디오를 play 하기 때문에 잦은 랜더링은 성능이슈로 이어질 수 있음 (ex. iPad에서 깜박임 현상)
- 지원 브라우저, 디바이스 선정
  - 화면공유 같은 api는 모바일 브라우저에서 지원하지 않는다.


## 추가 고려사항

- 단일 기능 테스트 페이지
  - 각 api 호출이 정상적으로 되는지 확인하기 위해 단일 기능들을 순차 테스트할 수 있는 페이지 필요
- 로그 수집
  - 많은 사람들이 참여하는 상황에서 접속시 에러상황을 탐지하기 위해 로그 수집
- 테스트 서버
  - 서비스페이지와 유사한 테스트전용 서버 구축 (ex. MSW를 Mock 서버로 사용)
- 테스트 계정
  - 테스트 서버에서 사용할 테스트 계정을 정해두기
