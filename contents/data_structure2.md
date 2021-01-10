### [issue]에 대한 정리
#### [#issue1] Disjoint-set(서로소 집합 자료구조)의 개념과 사용 예제
    * 개념
        * Union-Find 자료구조라고도 한다.
        * 서로 중복되지 않는 부분 집합들(공통 원소가 없는, 즉 "상호 배타적" 인 부분 집합들)로 나눠진 원소들에 대한 정보를 저장하고 조작하는 자료 구조
        * 주로 트리 구조를 이용하여 구현한다.
    * 연산
        * make-set(x) : x를 유일한 원소로 하는 새로운 셋을 만든다.
        * union(x, y) : x가 속한 셋과 y가 속한 셋을 합친다. 즉, x와 y가 속한 두 집합을 합치는 연산
        * find(x) : x가 속한 셋의 대표값(루트노드 값)을 반환한다. 즉, x가 어떤 집합에 속해 있는지 찾는 연산
    * 사용 예제
        * 전체 집합이 있을 때 구성 원소들이 겹치지 않도록 분할(partition)하는 데 자주 사용된다.
        1. 초기에 {0}, {1}, {2}, ... {n} 이 각각 n+1개의 집합을 이루고 있다. 여기에 합집합 연산과, 두 원소가 같은 집합에 포함되어 있는지를 확인하는 연산을 수행하려는 경우
        2. 어떤 사이트의 친구 관계가 생긴 순서대로 주어졌을 때, 가입한 두 사람의 친구 네트워크에 몇 명이 있는지 구하는 프로그램을 작성하는 경우
       
       
#### [#issue1-1] Disjoint-set 구현 방법
* 기본적인 구현 방법
~~~java
/* make-set() */
// 부모 노드 번호 저장
int[] parents = new int[MAX_SIZE];
// union을 위한 트리 높이 저장
int[] rank = new int[MAX_SIZE];

for (int i = 0; i < MAX_SIZE; i++) {
    parent[i] = i;
    rank[i] = 1; 
}

/* find(x) */
int find(int x) {
    // 루트 노드는 부모 노드 번호로 자기 자신을 가진다.
    if (parents[x] == x) {
        return x;
    } 
    // 각 노드의 부모 노드를 찾아 올라간다.
    return find(parents[x]);
}

/* union(x, y) */
void union(int x, int y){
    // 각 원소가 속한 트리의 루트 노드를 찾는다.
    x = find(x);
    y = find(y);
    
    if (rank[x] > rank[y]) {
        parents[y] = x;
    } else if (rank[x] < rank[y]) {
        pranets[x] = y;
    } else {
        parents[x] = y;
        rank[y]++;
    }
    
}
~~~

#### [#issue2] 비트마스크의 개념과 사용 이유
    * 개념
        * 정수의 이진수 표현을 자료구조로 쓰는 기법을 비트마스크(bitmask)라고 부른다.
        * bit 연산을 이용해서 집합을 만들 수 있다.
        * bit가 1이면 "켜져 있다.", 0이면 "꺼져 있다."
        * 부호 없는 N bit의 값: 2^0 ~ 2^(n-1)
        
    * 비트마스크 사용 이유
        1. 더 빠른 수행 시간
        2. 더 간결한 코드
        3. 적은 메모리 사용량
        4. 연관 배열을 배열로 대체 가능: 
            * 연관 배열 객체 map<vector,int>을 비트마스크를 이용해 정수 변수로 나타내면 단순한 배열 int[]를 사용할 수 있다.
        
    * 비트마스크 사용 예제
        * 집합 구현
            * N bit 정수는 0 ~ (N-1)까지의 정수 원소를 가질 수 있는 집합
            * 집합 {1, 4, 5, 6, 7, 9}: 2^1 + 2^4 + 2^5 + 2^6 + 2^7 + 2^9 = 754
            * 집합 {1, 3, 4, 5, 9}: 2^1 + 2^3 + 2^4 + 2^5 + 2^9 = 570

#### [#issue2-1] 비트연산의 종류와 사용법
   | A | B | ~A | A & B (AND) | A ㅣ B (OR) | A ^ B (XOR) |
   | :---: | :---: | :---: | :---: | :---: | :---: |
   | 0 | 0 | 1 | 0 | 0 | 0 |
   | 0 | 1 | 0 | 0 | 1 | 1 |
   | 1 | 0 | 0 | 0 | 1 | 1 |
   | 1 | 1 | 0 | 1 | 1 | 0 |  
   
    * shift left(<<), shift right(>>)
        * A << B
            * A를 B bit만큼 왼쪽으로 민다.
            * A * 2^B
        * A >> B
            * A를 B bit만큼 오른쪽으로 민다.
            * A / 2^B
            * Ex) (A + C) / 2는 (A + C) >> 1과 동일
            
    * A & B (AND)
        * Ex) 어떤 수 N이 홀수 인지 판단하는 if(N % 2 == 1)는 if(N & 1)과 동일
       
    * 비트 연산 사용법
        * int S = 0; // 공집합
        * int num = Integer.parseInt(st.nextToken()); // 임의의 숫자
        1. add:  
            * S = S | (1 << num)
        2. remove:
            * S = S & ~(1 << num)
        3. check: 
            * S & (1 << num)
            * num이 집합에 없으면 return 0
        4. toggle:
            * S = S ^ (1 << num);
        5. all: 
            * NUM SIZE: 집합에 들어갈 수 있는 num의 최대 수 + 1
            * S = ((1 << NUM_SIZE) - 1)
          
#### [#issue3] 이진 트리의 개념과 종류
    * 이진 트리의 개념    
        * 각각의 노드가 최대 두 개의 자식 노드를 가지는 트리 자료 구조
     
    * 이진 트리 표현 방법
    1. 1차원 배열 표현
        * 이진 트리의 i번째 노드를 배열의 i번째 요소에 저장하는 방법
        * 장점: 노드의 위치를 index에 의하여 쉽게 접근할 수 있다.
        * 단점: 특정 트리에서 기억 공간의 낭비가 심할 수 있다.
    2. 연결리스트 표현
        * 왼쪽 자식을 가리키는 포인터 필드와 오른쪽 자식을 가리키는 포인터 필드를 포함하는 노드로 표현하는 방법
        * 장점: 기억 장소를 절약할 수 있고, 노드의 삽입과 삭제가 용이하다.
        * 단점: 이진 트리가 아닌 일반 트리의 경우에는 각 노드의 차수만큼 가변적인 포인터 필드를 가져야 하기 때문에 접근상의 어려움이 따른다.
     
    * 이진 트리의 종류
        * Full Binary Tree(정 이진 트리, Strictly Binary Tree)
            * 모든 노드가 0개 또는 2개의 자식 노드를 갖는 트리
        * Complete Binary Tree(완전 이진 트리)
            * 마지막 레벨을 제외하고 모든 레벨이 완전히 채워져 있다. 
            * 마지막 레벨의 모든 노드는 가능한 한 가장 왼쪽에 있다. 
            * 마지막 레벨 h 에서 1부터 2h-1 개의 노드를 가질 수 있다. 
            * 또 다른 정의는 가장 오른쪽의 잎 노드가 (아마도 모두) 제거된 포화 이진 트리다. 
            * 완전 이진 트리는 배열을 사용해 효율적으로 표현 가능하다.
        * Perfect Binary Tree(포화 이진 트리)
            * 모든 내부 노드가 두 개의 자식 노드를 가진다. 
            * 모든 잎 노드가 동일한 깊이 또는 레벨을 갖는다.

![](/contents/images/binary-tree.png)

#### [#issue3-1] 이진 트리와 관련된 용어들
    * left child node(왼쪽 자식 노드): 왼쪽 부트리의 노드
    * right child node(오른쪽 자식 노드): 오른쪽 부트리의 노드
    * parent node(부모 노드): 해당 노드를 자식으로 하는 노드
    * sibling node(형제 노드): 부모가 같은 두 노드
    * external/leaf node(잎 노드): 차수가 0인 노드. 즉, 자식이 없는 노드
    * internal/branch node(내부 노드): 차수가 0이 아닌 노드. 즉, 자식이 있는 노드
    
    * degree(차수): 자식의 수
    * depth(노드의 깊이): 루트 노드에서 자신까지 가는 경로의 길이. 루트 노드의 깊이는 0
    * level(노드의 레벨): 루트 노드에서 자신까지 가는 (경로의 길이 + 1). 루트 노드의 레벨은 1
    * height(노드의 높이): 그 노드와 단말 노드 사이의 경로의 최대 길이
    * size(노드의 크기) 자기 자신 및 모든 자손 노드의 수
    
    * 트리의 깊이와 레벨과 높이와 크기는 각각 노드의 길이와 레벨과 높이와 크기의 최댓값이다. 
    * 특히, 트리의 높이는 그 루트 노드의 높이와 같으며, 트리의 크기는 그 루트 노드의 크기와 같다.

#### [#issue4] 최대힙의 삽입과 삭제
    * 최대힙(Max Heap)
        * 부모 노드의 키 값이 자식 노드의 키 값보다 크거나 같은 완전 이진 트리
        * key(부모 노드) >= key(자식 노드)
        * 즉, 가장 큰 값은 루트 노드
        * N개가 힙에 들어가 있으면 높이는 log(N)
    
    * 최대힙 삽입
        1. 힙에 새로운 요소가 들어오면, 일단 새로운 노드를 힙의 마지막 노드에 이어서 삽입한다.
        2. 새로운 노드를 부모 노드들과 교환해서 힙의 성질을 만족시킨다.
        
    * 최대힙 삭제
        1. 최대 힙에서 최댓값은 루트 노드이므로 루트 노드가 삭제된다.
            * 최대 힙(max heap)에서 삭제 연산은 최댓값을 가진 요소를 삭제하는 것이다.
        2. 삭제된 루트 노드에는 힙의 마지막 노드를 가져온다.
        3. 힙을 재구성한다.
           
           
~~~java
int[] maxHeap;
int heapSize;

/* 최대힙 삽입 */
void insert_max_heap(int x){
    maxHeap[++heapSize] = x; // 힙 크기를 하나 증가하고 마지막 노드에 x를 넣는다.

    for (int i=heapSize; i>1; i/=2) {
        // 마지막 노드가 자신의 부모 노드보다 크면 swap
        if (maxHeap[i/2] < maxHeap[i]) {
            swap(i/2, i);
        } else {
            break;
        }
    }
}

/* 최대힙 삭제 */
int delete_max_heap(){
    if (heapSize == 0) // 배열이 빈 경우
        return 0;

    int item = maxHeap[1]; // 루트 노드의 값을 저장한다.
    maxHeap[1] = maxHeap[heapSize]; // 마지막 노드의 값을 루트 노드에 둔다.
    maxHeap[heapSize--] = 0; // 힙 크기를 하나 줄이고 마지막 노드를 0으로 초기화한다.

    for (int i=1; i*2<=heapSize;) {
        // 마지막 노드가 왼쪽 노드와 오른쪽 노드보다 크면 반복문을 나간다.
        if (maxHeap[i] > maxHeap[i*2] && maxHeap[i] > maxHeap[i*2+1]) {
            break;
        }
        // 왼쪽 노드가 더 큰 경우, 왼쪽 노드와 마지막 노드를 swap
        else if (maxHeap[i*2] > maxHeap[i*2+1]) {
            swap(i, i*2);
            i = i*2;
        }
        // 오른쪽 노드가 더 큰 경우, 오른쪽 노드와 마지막 노드를 swap
        else {
            swap(i, i*2+1);
            i = i*2+1;
        }
    }
    return item;
}
~~~       
      
* 그림 참고: [data-structure-heap](https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html)
* PriorityQueue, Comparator interface를 이용한 방법: [https://gist.github.com/Baekjoon/5b1349b9159e7548bb54](https://gist.github.com/Baekjoon/5b1349b9159e7548bb54)


#### [#issue5] 이진 탐색 트리의 개념 


### Reference
> - [http://bowbowbow.tistory.com/26](http://bowbowbow.tistory.com/26)
> - [https://ratsgo.github.io/data%20structure&algorithm/2017/11/12/disjointset/](https://ratsgo.github.io/data%20structure&algorithm/2017/11/12/disjointset/)
> - [https://groovypark.github.io/2017/10/04/%EB%B9%84%ED%8A%B8%EB%A7%88%EC%8A%A4%ED%81%AC/](https://groovypark.github.io/2017/10/04/%EB%B9%84%ED%8A%B8%EB%A7%88%EC%8A%A4%ED%81%AC/)
> - [https://ko.wikipedia.org/wiki/%EC%9D%B4%EC%A7%84_%ED%8A%B8%EB%A6%AC](https://ko.wikipedia.org/wiki/%EC%9D%B4%EC%A7%84_%ED%8A%B8%EB%A6%AC)
> - [https://stackoverflow.com/questions/12359660/difference-between-complete-binary-tree-strict-binary-tree-full-binary-tre](https://stackoverflow.com/questions/12359660/difference-between-complete-binary-tree-strict-binary-tree-full-binary-tre)
> - [https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html](https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html)

### :house: [Go Home](https://github.com/heshAlgo/Algorithm-Information) 

