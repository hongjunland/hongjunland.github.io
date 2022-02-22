---
title: "[BlockChain]Block Chain"
last_modified_at: "2022-02-22 10:06"
categories:
    - blockchain
tags:
    - nft
    - react
    - javascript
    
toc: true
toc_sticky: true
toc_label: "Contents"
---

## 1. 블록체인

* 분산 컴퓨팅 기술 기반의 데이터 위변조 방지 기술
* P2P방식을 기반으로 소규모 데이터들이 체인 형태로 무수히 연결되어 형성된 블록이라는 분산데이터 저장환경에 관리 대상 데이터를 저장함으로써 임의 변경이 불가능하며, 누구나 변경의 결과를 알수 있게하는 기술.

## 2. 블록체인 분류

유형|특징|예시
---|---|---
public|누구나 네트워크에 참여가능|Bitcoin, Ethereum,...
private|하나의 조직 혹은 기관이 관장하는 네트워크, 승인된 주체만 자료를 읽고, 지정노드만 거래승인|Monax, Quorum,..
Consortium|이해관계자 간 컨소시엄을 구성해서, 네트워크를 구성한다. 네트워크참여자에 의해 접근 허용|Hyperledger Fabric, Tendermint

## 3. Public Network

* Main Net : 기존에 존재하는 플랫폼에 종속되어 있지 않고, 독립적으로 생태계를 구성하는 것
* Test Net : 독자적인 플랫폼으로 자리잡을 수 있는지 테스트하는 것

Hex|Dec|Network
--|--|--
0x1|1|Ethereum Main Network
0x3|3|Ropsten Test Network
0x4|4|Rinkely Test Network
0x5|5|Goerli Test Network
0x29|42|Kovan Test Network


## 4. MetaMask Wallet

* 블록체인 네트워크를 사용할 수 있도록 계정의 개인 키를 관리하는 프로그램
* 개인키로 sign하여 트랜잭션을 전송

### 4.1 계정생성 절차

1. 개인키 생성 : 256bit의 무작위 숫자 -> 64자리의 16진수로 인코딩
2. 타원곡선전자서명 알고리즘(ECDSA, secp256k1)을 사용해서 public key 생성
3. Keccak-256 Hashing 
4. 계정 주소(마지막 20Byte)


## 5. 이더리움 네트워크의 주요 용어

* Client : RPC 요청을 받고 결과를 반환하는 endpoint
* Provider : 클라이언트를 통해 이더리움에 대한 액세스를 제공하는 소비자가 사용할 수 있는 Provider javascript 객체 
* Faucet : 개인 키를 관리하고 서명 작업을 수행하며, provider와 클라이언트 사이의 미들웨어 역할을 수행하는 최종 사용자 어플리케이션
* Gas : 스마트 컨트랙트를 실행할때, 수수료를 책정하기 위해 만든 단위
* RPC : 원격 프로시저 요청,해당 지갑 또는 클라이언트가 처리해야 하는 일부 절차에 대해 공급자에게 제출된 모든 요청.
