---
published: false
id: webrtc
slug: webrtc
title: Webrtc
summary: webrtc
toc: true
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2024-03-17
updatedDate: 2024-03-20
---

# Webrtc

## Udemy

- 24m: Introduction to webRTC (3/17)
- 1h37m: Project - getUserMedia playground (3/17)
- 2h37m: rtcPeerConnection (~ 3/22 매일 4개)
- 5h54m: webrtc and React (3/23, 3/24)
- 2h5m: Deploying app to aws (optional)

## Note
webrtc 정의 : MDN에서 확인

- getUserMedia: camera, mic, screen capture
- RTCPeerConnection: stream

webrtc vs zoom sdk
- webrtc: p2p(fast), control, flexibility, pricing, security
- zoom sdk: stability

video tag에서 autoplay, playsinline은 필수로 있어야함, 그래야 바로 재생이 됨

WebRTC has two part
1. Media - getUserMedia
2. Connection - RTCPeerConnection

Signaling
signaling server's job : two browser can not find each other and talk to same language without help
1. Find : ip address, ICE Candidate
2. Exchange Info : SDP
are the two main tasks of signaling
[signaling][1] is the process of coordinating communication between WebRTC clients

debugging
- chrome://webrtc-internals/

webrtc protocol
- https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Protocols

### How to set sdp

- createOffer --> setLocalDescription with this offer --> send this offer to other peer by signaling server
1. Someone must getUserMedia() - CLIENT1/Init/Caller/Offerer
2. CLIENT1 creates RTCPeerConnection
3. peerConnection needs STUN servers
  - we will need ICE candidates later
4. CLIENT1 add local stream tracks to peerConnection
  - we need to associate CLIENT1 feed with peerConnection
5. CLIENT1 creates an offer
  - needed peerConnection with tracks
  - offer = RTCSessionDescription
    1. SDP - codec/resolution information
    2. Type (offer)
6. CLIENT1 hands offer to peerConnection.setLocalDescription(offer)

### API

enumerateDevices
- get all devices
- https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/enumerateDevices

applyConstraints
- change the constraints of the media track
- https://developer.mozilla.org/en-US/docs/Web/API/Media_Capture_and_Streams_API/Constraints#applying_constraints

getSupportedConstraints
- get the supported constraints
- https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getSupportedConstraints

MediaRecorder.start
- start recording
- https://developer.mozilla.org/en-US/docs/Web/API/MediaRecorder/start

PeerConnection.addTrack
- add a track to the connection (this is needed after creating the connection)
- https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/addTrack

[1]: https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Signaling_and_video_calling
