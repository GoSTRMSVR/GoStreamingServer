# 1:1 스트리밍 서버 프로젝트

Go, gRPC, WebRTC를 사용하여 **1:1 동영상 스트리밍 서버**를 구축하는 프로젝트입니다. 이 프로젝트는 송신자와 수신자 간에 실시간으로 동영상 스트림을 전송하고, 재생 및 일시정지와 같은 제어 기능을 제공합니다.

## 목차

- [소개](#소개)
- [기능](#기능)
- [개발 환경 설정](#개발-환경-설정)
- [프로젝트 구조](#프로젝트-구조)
- [설치 및 실행 방법](#설치-및-실행-방법)
- [사용 예시](#사용-예시)
- [구성 요소 설명](#구성-요소-설명)
- [추가 개선 사항](#추가-개선-사항)
- [라이선스](#라이선스)

## 소개

이 프로젝트는 Go 언어와 gRPC, WebRTC를 활용하여 송신자와 수신자 간의 **1:1 동영상 스트리밍**을 구현합니다. gRPC를 통해 신호 교환과 제어 채널을 관리하며, WebRTC를 사용하여 실제 동영상 데이터를 전송합니다.

## 기능

- **실시간 동영상 스트리밍**: 송신자와 수신자 간의 실시간 동영상 전송
- **제어 채널 지원**: 재생, 일시정지 등의 제어 명령 송수신
- **신호 교환(Signaling)**: gRPC를 통한 WebRTC 연결 설정
- **확장 가능한 구조**: 기능 추가 및 확장이 용이한 모듈식 구조

## 개발 환경 설정

### 필수 요구 사항

- **Go 언어**: 버전 1.16 이상
- **Protocol Buffers 컴파일러 (`protoc`)**
- **Git**: 버전 관리 및 코드 클론을 위해 필요

### 의존성 패키지

- **gRPC 및 Protocol Buffers 라이브러리**
- **Pion WebRTC**: Go용 WebRTC 구현체

## 프로젝트 구조

```
project-root/
├── go.mod
├── go.sum
├── README.md
├── cmd/
│   ├── server/
│   │   └── main.go
│   ├── sender/
│   │   └── main.go
│   └── receiver/
│       └── main.go
├── pkg/
│   ├── signaling/
│   │   ├── server.go
│   │   └── client.go
│   ├── control/
│   │   ├── server.go
│   │   └── client.go
│   └── webrtc/
│       ├── peer.go
│       └── utils.go
├── proto/
│   └── control_channel.proto
├── pb/
│   ├── control_channel.pb.go
│   └── control_channel_grpc.pb.go
└── scripts/
    └── generate_proto.sh
```

## 설치 및 실행 방법

### 1. 의존성 설치

```bash
# 프로젝트 클론
git clone https://github.com/your_username/streaming-server.git
cd streaming-server

# Go 모듈 초기화 및 의존성 다운로드
go mod tidy

# Protocol Buffers 컴파일러 설치
# macOS
brew install protobuf
# Ubuntu/Debian
sudo apt-get install -y protobuf-compiler

# Go용 protoc 플러그인 설치
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest

# GOPATH/bin을 PATH에 추가
export PATH="$PATH:$(go env GOPATH)/bin"
```

### 2. 프로토콜 버퍼 코드 생성

```bash
./scripts/generate_proto.sh
```

### 3. 실행

```bash
# 서버 실행 (기본 포트: 50051)
go run cmd/server/main.go

# 송신자 실행
go run cmd/sender/main.go

# 수신자 실행
go run cmd/receiver/main.go
```

## 사용 예시

1. 서버를 먼저 실행하여 클라이언트의 연결을 대기합니다.
2. 송신자 클라이언트를 실행하여 미디어 소스(웹캠, 동영상 파일)를 설정합니다.
3. 수신자 클라이언트를 실행하여 스트림을 수신하고 출력합니다.
4. 제어 명령을 사용하여 재생, 일시정지 등을 제어합니다.

## 구성 요소 설명

### 1. 신호 서버 (pkg/signaling/)
- WebRTC 연결 설정을 위한 신호 메시지 중계
- Offer, Answer, ICE Candidate 처리

### 2. 제어 채널 (pkg/control/)
- 재생, 일시정지 등의 제어 명령 처리
- 클라이언트-서버 간 제어 통신

### 3. WebRTC 처리 (pkg/webrtc/)
- WebRTC 피어 연결 설정 및 관리
- 미디어 스트림 처리

### 4. 프로토콜 정의 (proto/)
- gRPC 서비스와 메시지 정의
- Protocol Buffers 스키마

## 추가 개선 사항

- **보안 강화**: TLS 적용, 인증 및 권한 관리 도입
- **에러 처리 개선**: 다양한 예외 상황 처리 로직 추가
- **다중 시청자 지원**: SFU 도입으로 다수 수신자 지원
- **사용자 인터페이스**: GUI 추가
- **성능 최적화**: 네트워크 상태 기반 품질 조정, 대역폭 관리

## 라이선스

이 프로젝트는 MIT 라이선스를 따릅니다.
