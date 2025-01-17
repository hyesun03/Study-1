# 트리(Tree)

## 1. 구조

트리는 부모 노드 밑에 자식 노드가 연결되고, 자식 노드 각각에 다시 자식 노드가 연결되는 재귀적 형태의 자료구조다. 만약 자식 노드의 자식 노드가 부모 노드로 연결되는 경우는 트리로 인정하지 않는다. 

## 2. 종류

### 1. 이진 트리(Binary Tree)

트리의 가장 간단한 형태로, 부모 노드 밑에 자식 노드가 최대 2개밖에 오지 않는 형태 이다. 부모 노드는 데이터와 오른쪽 노드, 왼쪽 노드를 각각 가리킬 두 개의 포인터를 가지고 있다. 왼쪽에 자식 노드, 오른쪽에 형제 노드를 배치(Left-Child, Right-Sibling)해 모든 트리를 이진 트리 형태로 재구성할 수 있다.

- #### 정 이진 트리(Full Binary Tree)

    모든 트리의 자식은 0개나 2개인 이진 트리

- #### 포화 이진 트리(Perfect Binary Tree)

    모든 리프 노드(자식이 없는 노드)의 높이가 같은 이진 트리

- #### 완전 이진 트리(Complete Binary Tree)

    트리의 원소를 왼쪽에서 오른쪽으로 하나도 빠짐없이 채워나간 형태의 이진 트리

### 2. 이진 탐색 트리(Binary Search Tree, BST)

각 노드의 왼쪽 가지에는 노드의 값보다 작은 값들만 있고, 오른쪽 가지에는 큰 값들만 있도록 구성된 이진 트리다. 이러한 구조 덕분에 이진 탐색을 하기에 적합한 구성이 된다.

- #### AVL 트리

    자가 균형 이진 탐색 트리로 이상적인 상황에서나 최악으 상황에서 탐색, 삽입, 삭제 모두가 시간 복잡도가 O(log N)이다. 균형이 매우 
    좋은 트리다.

- #### 레드블랙 트리(Red-Black Tree)

    자가 균형 이진 탐색 트리의 일종으로, 노드에 검정 또는 빨강인 색깔 속성이 붙은 트리다. 자식이 하나도 없는 노드 끝에는 널 리프 노
    드가 붙는다.

- #### 힙(Heap)

    힙은 최댓값 및 최솟값을 찾아내는 연산을 빠르게 하기 위해 고안된 완전 이진 트리를 기본으로 한 자료구조다. 부모노드의 키값이 자식
    노드의 키값보다 항상 작은 힙을 '최소힙', 반대의 경우를 '최소힙'이라고 부른다.

### 3. B-Tree

이진 트리를 확장한 트리로 자식 노드의 수가 2개 이상을 가질 수 있다. 노드에 접근 하는 시간 자체가 노드에서 연산하는 시간보다 훨씬 길 경우, B-tree를 쓴다.

- #### 2-3-4 tree

    B-tree of order 4(모든 노드가 가질 수 있는 자식 노들의 최대 수가 4개인 트리)의 일종이다.

- #### B+ tree

    루트 노드와 중간의 노드응 키를 이용하여 위치를 찾아가는 인덱스 역할만을 하며 데이터 자체는 모두 리프 노드에 저장하는 트리다.

### 3. R 트리(R Tree)

R 트리는 B 트리와 비슷하며 다차원의 공간 데이터를 저장하는 색인이다.

### 4. 포레스트(Forest)

하나 이상의 트리로 이루어진 집합을 포레스트 라고 한다.

## 3. 용어(구현)

### insert()

트리에 데이터를 넣는다.

### find()

트리에서 특정 데이터를 찾는다.

### remove()

트리에서 특정 데이터를 삭제한다.

## 4. 용도

#### 1. 컴퓨터의 디렉토리(Directory)구조에 이용
#### 2. 리스트를 정렬하기 위해 이진트리를 이용
#### 3. 명제식을 이진 트리로 표현하여 최종 결과를 내는데 이용
#### 4. 스레드를 이진 트리에 이용