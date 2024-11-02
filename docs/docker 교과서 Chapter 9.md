---
aliases: 
tags: 
description:
title: docker 교과서 Chapter 9
created: 2024-11-02T16:54:14
updated: 2024-11-02T17:08:20
---

## What is Prometheus?

프로메테우스는 도커 컨테이너의 투명성을 확보하기 위해 컨테이너의 상태를 수집 및 수치화 하여 공통 API를 통해 통신하도록 만들어진 모니터링 툴이다. 따라서 어느 컨테이너건 프로메테우스가 정의한 API 규칙만 준수하면 개별 컨테이너는 물론 분산환경에서의 상태 추적 및 관리가 용이해진다.

[docs.docker.com # Collect Docker metrics with Prometheus](https://docs.docker.com/engine/daemon/prometheus/) 이 문서는 도커 컨테이너가 아닌, 도커 엔진 자체를 프로메테우스를 통하여 모니터링하는 방법에 대해 설명한다.

## What is Grafana?
