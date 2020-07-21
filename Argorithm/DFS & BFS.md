# 1. 깊이 우선 탐색 (DFS, Depth-First-Search)

## 1.1. 알고리즘
DFS의 알고리즘은 트리나 그래프에서 한 루트로 탐색하다가 특정 상황에서 최대한 깊숙히 들어가서 확인한 후 다시 돌아가 다른 루트로 탐색하는 방식이다. 즉, 해당 분기에 대해 완벽하게 탐색한 후 다른 분기를 탐색하는 것이다. DFS 알고리즘은 ```스택(Stack)```을 이용한 방식으로 일반적으로 재귀호출을 사용하여 구현한다.   

갈림길이 나타날 때마다 '다른 길이 있다'는 정보만 기록하면서 자신이 지나간 길을 지워나간다. 그러다 막다른 곳에 도달하면 직전 갈림길까지 돌아가면서 '이 길은 아님'이라는 표식을 남긴다. 그렇게 분기를 순차적으로 탐색해 나가다 목적지에 도착하면 해답을 내고 종료하게 된다.

### 1.1.1 알고리즘 예시
![DFS 알고리즘 예시](https://upload.wikimedia.org/wikipedia/commons/7/7f/Depth-First-Search.gif)

## 1.2. 장단점
### 1.2.1. 장점
* 단지 현 경로상의 노드들만을 기억하면 되므로 저장공강의 수요가 비교적 적다.
* 목표 노드가 깊은 단계에 있을 경우 해를 빨리 구할 수 있다.

 ### 1.2.2 단점
 * 해가 없는 경로에 깊이 빠질 가능성이 있다. 때문에 실제의 경우 미리 저장한 임의의 깊이까지만 타맥하고 목표 노드를 발견하지 못하면 다음 경로를 따라 탐색하는 방법이 유용할 수 있다.
 * 얻어진 해가 최단 경로가 된다는 보장이 없다. 이는 목표에 이르는 경로가 다수인 문제에 대해 깊이 우선 탐색은 해에 다다르면 탐색을 긑내버리므로, 이 때 얻어진 해는 최적이 아닐 수 있다는 의미이다.

 # 2. 넓이 우선 탐색(Breadth-First_Search)

 ## 2.1. 알고리즘
 BFS의 알고리즘은 트리가 그래프에서 한 노드에서 시작하여 특정 상황에 만족하는 자식 노드들을 차례대로 방문하는 것으로 ```큐(Queue)```를 이용한다.   

### 2.1.1. 알고리즘 예시
![BFS 알고리즘 예시](https://upload.wikimedia.org/wikipedia/commons/5/5d/Breadth-First-Search-Algorithm.gif)

## 2.2. 장단점
### 2.2.1 장점
* 출발 노드에서 목표 노드까지의 최단 길이 경로를 보장한다.

### 2.2.2 단점
* 경로가 매우 길 경우에는 탐색 가지가 급격히 증가함에 따라 보다 많은 기억 공간을 필요하게 된다.
* 해가 존재하지 않으면 휴한 그래프의 경우에는 모든 그래프를 탐색한 후에 실패로 끝는다.
* 무한 그래프의 경우에는 결코 해를 찾지도, 끝내지도 못한다.

## 출처
* [위키토피아-DFS](https://ko.wikipedia.org/wiki/%EA%B9%8A%EC%9D%B4_%EC%9A%B0%EC%84%A0_%ED%83%90%EC%83%89)   
* [나무위키-DFS](https://namu.wiki/w/DFS)   
* [위키토피아-BFS](https://ko.wikipedia.org/wiki/%EB%84%88%EB%B9%84_%EC%9A%B0%EC%84%A0_%ED%83%90%EC%83%89)   
* [나무위키-BFS](https://namu.wiki/w/BFS)   
* [Kim Ju Hui-[그래프] BFS와 DFS ](https://velog.io/@kjh107704/%EA%B7%B8%EB%9E%98%ED%94%84-BFS%EC%99%80-DFS)   
* [TWpower-[Algorithm]DFS와 BFS 기본과 예제 문제](https://twpower.github.io/151-bfs-dfs-basic-problem)   
* [Yun Young-[알고리즘]깊이 우선 탐색(DFS) 과 너비 우선 탐색(BFS)](https://yunyoung1819.tistory.com/86)   
* [Wikimedia Commons-DFS 알고리즘 예시 그림](https://upload.wikimedia.org/wikipedia/commons/7/7f/Depth-First-Search.gif)   
* [Wikimedia Commons-BFS 알고리즘 예시 그림](https://upload.wikimedia.org/wikipedia/commons/5/5d/Breadth-First-Search-Algorithm.gif)
