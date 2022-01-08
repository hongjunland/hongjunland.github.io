---
title: "[algorithm] 최단 경로"
last_modified_at: 2021-12-07 23:12
categories:
    - algorithm
tags:
    - algorithm
    - 최단경로
---

## 1. 최단 경로 문제란?

* 간선의 가중치가 있는 그래프에서 두 정점 사이의 경로들 중에 간선의 가중치 합이 최소인 경로를 찾는 문제이다.

## 2. 알고리즘 분류

* 단일-출발 최단 경로

    * 다익스트라(dijkstra) 알고리즘 : 양의 가중치만 허용
    * 벨만-포드(Bellman-Ford) 알고리즘 : 음의 가중치도 허용

* 전체-쌍 최단 경로

    * 플로이드-워셜(Floyd-Warshall) 알고리즘
