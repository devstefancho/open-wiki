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
updatedDate: 2024-05-07
---

# Feconf 지원
- https://docs.google.com/forms/d/e/1FAIpQLSdcp-7HKQnHIg64QQ--dwe07djNzSgTOwvnth7JFLGzhoFc0A/viewform

## 공유 주제 혹은 제목


## 공유 내용
대략적인 목차와 상세 내용을 가급적 자세하게 적어 주세요.
참고할 수 있는 소스 코드나 결과물이 있다면 좋습니다. 발표 시간은 25-30분이며 최대 40분입니다.


### 목차 작성

- 자기소개 (2)
- Webrtc를 하게 된 계기 (2)
- Webrtc 기초 (5)
- 화상회의의 기본기능 (2)
- 확장된 화상회의의 기능 (2)
- Webrtc를 하면서 겪은 어려움 (2)
- 간단한 예시 코드 (통신방식 설명) (5)
- 마무리 (2)


### 목차별 상세 내용 작성

#### 자기소개
- 이름 : 조성진
- 소속 : LG U+
- 역할 : Frontend Developer
- 취미 : 러닝, 만화보기
- 연락처 : 전화번호, 이메일, 링크드인, 깃헙, QR 코드

#### Webrtc를 공유해야겠다고 생각한 이유
- feconf에 webrtc가 소개된적이 없음
- 한국어로 된 webrtc관련 frontend 자료가 많이 없다고 생각함
- webrtc로 화상회의를 만들면서 겪은 어려움과 해결방법을 공유하고 싶었음

#### Webrtc를 선택하게 된 계기
- 화상회의 서비스를 신규개발하게 되었음
- 1:N 방식의 화상회의 서비스를 만들어야 했음
- webrtc라는 용어정도만 아는 상태였고, Youtube에서 webrtc example이라고 검색해서 예제를 따라해봄

#### Webrtc 기초

Web API와 미디어 공유를 위해서 알아야하는 것들
- getUserMedia
- SDP 생성 및 교환
- ICE Candidate 교환
- PeerConnection (Peer to Peer 연결)


#### 화상회의의 기본기능

- 화상회의에 있는 기본적인 기능들은 무엇이 있는가?
- 대기실, 회의실, 종료페이지
- 카메라, 마이크, 스피커 설정
- 상대방 디바이스 상태 확인
- 상대방 디바이스 컨트롤
- 화면공유
- 채팅 혹은 손들기와 같은 의사표현 기능
- 화면 레이아웃 변경 (디바이스 별로 다르고, pin하면 또 다르고)
- 녹화
- 화이트 보드


#### 확장된 화상회의의 기능

- 교육쪽에서 사용하면서 추가된 기능들
- 선생님이 변경한 모드에 맞게 학생들의 화면 레이아웃 변경
- 그루핑하여 여러명의 학생들의 마이크, 카메라를 컨트롤
- ROLE(선생님, 학생)에 따른 화면 레이아웃 구성
- ROLE(선생님, 학생)에 따른 기능 제한(화면공유 기능제한)


#### Webrtc를 하면서 겪은 어려움

- 오디오 테스트의 어려움 (하울링 이슈)
- 다중 접속자 테스트의 어려움 (한번에 여러명으로 접속해야함)
- 여러가지 디바이스에 따라 테스트를 해야함 (OS별 Brower, Mobile Brower, Webview)
- 상태관리의 어려움 (상대방이 나의 상태를 변경시켰을때 비동기적 상태관리를 해야함)


#### 간단한 예시 코드 (통신방식 설명)

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
