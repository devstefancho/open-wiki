---
published: false
id: feconf
slug: feconf
title: Feconf
summary: feconf
toc: true
tags: ["projects"]
categories: ["projects"]
createdDate: 2024-05-06
updatedDate: 2024-05-11
---

# Feconf 지원
- https://docs.google.com/forms/d/e/1FAIpQLSdcp-7HKQnHIg64QQ--dwe07djNzSgTOwvnth7JFLGzhoFc0A/viewform


- 이름 : 조성진
- 소속 : LG U+
- 역할 : Frontend Developer
- 취미 : 러닝, 만화보기
- 연락처 : 전화번호, 이메일, 링크드인, 깃헙, QR 코드

## 공유 주제 혹은 제목

## 공유 내용
대략적인 목차와 상세 내용을 가급적 자세하게 적어 주세요.
참고할 수 있는 소스 코드나 결과물이 있다면 좋습니다. 발표 시간은 25-30분이며 최대 40분입니다.


### 목차 작성

- 발표자 소개
- 주제선정의 이유
- Webrtc란 무엇인가?
- 어떤사람들에게 도움이 될까
- 어떤 서비스를 만들때 사용하는가?
- Webrtc와 관련된 기술들
- 간단한 데모 시연
- 화상회의를 위한 도메인 지식
- PeerConnection의 한계점
- 간단한 데모 시연
- 그러면 서버는 누가 만들어?
- 화상회의 만들면서 겪은 어려움들
- 나의 팁
- 기타내용


### 목차별 상세 내용 작성

#### 발표자 소개
- 이름, 소속, 역할, 취미, 소셜미디어


#### 주제선정의 이유
올 하반기에 화상회의 서비스를 런칭하기 위해 2월말부터 Webrtc를 공부하기 시작하였습니다.
Webrtc 기술을 사용하는게 맞는가 반신반의하며, 일단 시작하고 봤는데 생각보다 사용하기 어렵지 않았습니다.
프론트엔드 개발자는 Webrtc에 대해 어느정도까지 이해해야 화상회의를 만들 수 있을까 고민되는 사람들에게
최대한 내용을 쉽게 내용을 전달하여 많은 프론트개발자들이 다양한 서비스에 Webrtc 기술을 적용해보았으면 하는 바람입니다.


#### Webrtc란 무엇인가?
- Webrtc의 역사
- Webrtc 용어설명
- Webrtc의 강점


#### 어떤사람들에게 도움이 될까
- 화상회의 서비스를 만들어야하거나 만들어보고 싶은 개발자
- Webrtc 기술에 대해서 궁금했던 사람들


#### 어떤 서비스를 만들때 사용하는가?
- 여러사람이 카메라와 오디오 스트림을 공유하는 서비스
- 화면공유를 통해 영상을 공유하는 서비스
- Web 기반의 화상회의 서비스가 필요할 때


#### Webrtc와 관련된 기술들
프로토콜 및 프레임워크
- SDP(Session Description Protocol) 란 무엇인가?
- SDP를 교환하는 이유
- SDP를 교환하는 방법
- ICE(Interactive Connectivity Establishment)란  무엇인가?
- ICE Candidate는 언제 생성되는가
- STUN, TURN 서버

API
- 스트림 생성을 위한 getUserMedia
- 화면 공유를 위한 getDisplayMedia
- Peer간 연결을 위한 RTCPeerConnection 객체

#### 간단한 데모 시연
- SDP 교환 이해를 돕기 위한 데모 시연 (백엔드 없는 데모)


#### 화상회의를 위한 도메인 지식

화상회의를 구현하는데 어떤것들이 필요한가?

- 기본적인 페이지 구성
  - 대기실, 회의실, 회의종료 페이지
  - 대기실에서는 내 로컬 스트림을 테스트함
  - 회의실에서는 다른사람들과 스트림을 공유함
  - 회의종료 페이지로 보내면서 모든 연결을 끊음

- 스트림 조작하기
  - 로컬 카메라, 마이크 설정
  - 리모트 유저 디바이스 상태 설정 (Signaling Server)

- 화면 공유
- 메시지 전송하기 (Signaling Server)
- 미디어스트림 녹화


#### PeerConnection의 한계점

- 1:1 은 스트림 공유는 쉽다 그러나 N명은?
- N명의 그룹콜을 위한 Server 설계
  - Mesh 방식
  - SFU(Selective Forwarding Unit) 방식
  - MCU(Multi-point Control Unit) 방식


#### 간단한 데모 시연
- SFU 혹은 MCU 방식으로 데모 시연 (이 데모는 아직 못만들었는데 간단하게라도 만들어 볼 예정입니다)


#### 그러면 서버는 누가 만들어?
다행히도 중앙서버를 제공하는 서비스가 있음
- Agora
- Zoom
- Dyte


#### 화상회의 만들면서 겪은 어려움들

- 다중 접속자 테스트의 어려움
  - 오디오 테스트 어려움 (하울링 이슈)
- 디바이스 호환성 테스트의 어려움
  - iOS, iPadOS, Android, Windows, MacOS 등 여러OS 및 디바이스로 확인해야함
- 비동기적 상태관리의 어려움
  - 상대방 디바이스 상태를 변경했을때, 상대 화면에서 UI 업데이트가 되어야함


#### 나의 팁

- 데모페이지는 무조건 만들자
- 데모페이지는 단계적으로 만들자
- 테스트를 고려하여 기능을 추가하자
- 내가 본 Webrtc 학습에 도움이 되는 미디어 추천
  - MDN, Udemy Course 링크, Youtube 링크


#### 기타내용

교육 목적의 화상회의의 기능
- 선생님이 학생 UI를 변경할 수 있음 (비디오 위치변경)
- 참여자를 N 등분하여 여러명의 학생들의 마이크, 카메라를 제어하기


### 소스코드 작성

Webrtc 샘플코드 (업데이트 예정)

- Code: https://github.com/devstefancho/webrtc-react-example/tree/main/frontend
- Demo: https://webrtc-react-example.vercel.app/
  - Vite, React, Tailwindcss
  - SDP 교환을 직접 copy & paste로 해서 전달하는 방식으로 함
  - Mesh 방식으로 구현 (4명까지 연결되는 것 확인, 그 이상은 손이 많이가서 안함)


## 하고 싶은 말


## Memo
icecandidate 이벤트 발생전에 pc.localDescription 값을 가져오면, icecandidate 정보가 빠지게 된다.
SDP 정보에 `c=IN IP4 192.168.219.112`와 같이 ip 정보가 있어야 한다.
없다고 하더라도 에러는 나지 않기 때문에 주의가 필요하다. (3시간 동안 삽질함)
```js
peerConnection.onicecandidate = async (event) => {
    //Event that fires off when a new answer ICE candidate is created
    if(event.candidate){
        document.getElementById('answer-sdp').value = JSON.stringify(peerConnection.localDescription)
    }
};
```
