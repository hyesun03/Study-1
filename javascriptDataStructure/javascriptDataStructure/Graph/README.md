# 그래프(Graph)

## 1. 구조

그래프는 정점(Vertex)과 정점을 연결하는 간선(Edge)으로 구성된 자료구조다.

## 2. 종류

### 1. 유향 그래프(Directed Graph)

간선을 화살표로 나타내서 해당 방향으로만 이동할 수 있는 그래프

### 2. 무향 그래프(Undirected Graph)

간선을 직선으로 나타내서 양방향으로 이동할 수 있는 그래프

### 3. 가중 그래프(Weight Graph)

간선에 가중치가 할당된 그래프(Network)

### 4. 다중 그래프(Multi Graph)

두 정점 사이에 두 개 이상의 간선이 있을수 있는 그래프

### 5. DAG(Directed Acyclic Graph)

한 정점에서 출발해 자기 자신으로 돌아오는 경로가 존재하지 않는 방향 그래프

## 3. 구현 방법

### 1. 인접 행렬(Adjacency Matrix)

배열을 이용한 그래프의 구현. 무향 그래프에서는 행렬이 대각선을 중심으로 대칭이다. 공간복잡도(필요한 기억장소의 크기)는 S(n) = n^2 다.

### 2. 인접 리스트(Adjacency Lists)

연결 리스트를 이용한 그래프의 구현. n개의 연결리스트로 그래프를 표현한다.

## 4. 용어(구현)

### 1. initGraph()

그래프를 초기화 시킨다.

### 2. isEmpty()

그래프가 비어있다면 참을 반환하고, 그렇지 않으면 거짓을 반환한다.

### 3. insertVertex()

정점을 삽입한다.

### 4. insertEdge()

간선을 삽입한다.

### 5. deleteVertex()

정점을 삭제한다.

### 6. deleteEdge()

간선을 삭제한다.

## 5. 용도

#### 1. 최단경로를 구하는데 이용
#### 2. 컴퓨터 통신망에 이용
#### 3. 물리적인 모델링에 이용
#### 4. 추상적인 모델링에 이용