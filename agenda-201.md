# TAP Hands-On-Lab 201
![](./images/taphol_log.jpeg)

## Objective

## Overview

## 설치 제품
본 가이드에서 다루는 Tanzu 설치 솔루션 및 버전은 다음과 같습니다.
|제품명|Version|
|---|---|
|TAP|1.4.2|

## 0. 사전 준비 사항
- [Lab 환경을 위한 사전 준비사항](./install/lab_prepare.md)

## 1. TAP GUI
- [catalog 등록](./tap/catalog.md)
- [Postgres 연결해서 TAP GUI 메타 정보 저장](./tap/gui-meta.md)
- [Application Live View 둘러보기](./tap/alv.md)

## 2. Supply Chain 구성
OOTB Basic Supply Chain으로 설치된 TAP를 Testing and Scanning으로 변경합니다. 
- [Supply Chain - Testing and Scanning](./tap/ootb-testing-and-scanning.md)

## 3. 멀티클러스터 환경을 위한 GitOps 구성 
구성 정보를 자동적으로 git repository에 push하도록 gitops 환경을 구성합니다.
- [GitOps 구성하기](./tap/gitops.md)

## 4. 워크로드 배포를 위한 네임스페이스 설정
- [워크로드 배포를 위한 네임스페이스 설정하기](./install/dev-namespace.md)

## 5. 개발 환경 구성
- [개발 환경 구성하기](./tap/ide.md)

## 6. 개발 환경에서 동적배포와 원격 디버깅
- [동적 배포와 원격 디버깅](./tap/hotdeploy_debug.md)

## 7. Accelerator 커스터마이징
- [Accelerator 커스터마이징](./tap/accelerator.md)

