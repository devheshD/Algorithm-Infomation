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

