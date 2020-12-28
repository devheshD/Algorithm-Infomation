## 자료구조1(스택, 큐, 덱, 문자열)

### [issue]에 대한 정리
#### [#issue1] 스택(Stack)의 개념
    * 스택(Stack)이란
        * 한 쪽 끝에서만 자료를 넣고 뺄 수 있는 자료구조
        * public class Stack<E> extends Vector<E>
        * 마지막에 넣은 것이 가장 먼저 나오기 때문에 LIFO(Last In First Out)
    
    * 스택(Stack)의 사용 예제
        * 웹 브라우저 방문기록 (뒤로가기)
        * 실행취소 (undo)
        * 역순 문자열 만들기
        * 수식의 괄호 검사 (연산자 우선순위 표현을 위한 괄호 검사)
        * 후위표기법 계산
           
* Ex) 올바른 괄호 문자열(VPS, Valid Parenthesis String) 판단하기
~~~java
/* 1. '(' ')'의 개수가 동일, 2. '('의 개수가 더 많으면 안됨 */
int cnt = 0;
for (int i=0; i<string.length(); i++){
    if(string.charAt(i) == '('){
        cnt++;
    } else {
        cnt--;
        if (cnt == -1)
            break;
    }
}
System.out.println(cnt == 0 ? "YES" : "NO");
~~~


#### [#issue1-1] 스택(Stack) 관련 메서드
    * push(E item)
        * 해당 item을 Stack의 top에 삽입
        * Vector의 addElement(item)과 동일
    * pop()
        * Stack의 top에 있는 item을 삭제하고 해당 item을 반환
    * peek()
        * Stack의 top에 있는 item을 삭제하지않고 해당 item을 반환
    * empty()
        * Stack이 비어있으면 true를 반환 그렇지않으면 false를 반환 
    * search(Object o)
        * 해당 Object의 위치를 반환
        * Stack의 top 위치는 1, 해당 Object가 없으면 -1을 반환

* Methods inherited from class ***java.util.Vector***
    * add, add, addAll, addAll, addElement, capacity, clear, clone, contains, containsAll, copyInto, elementAt, elements, 
    ensureCapacity, equals, firstElement, get, hashCode, indexOf, indexOf, insertElementAt, **isEmpty,** iterator, lastElement, 
    lastIndexOf, lastIndexOf, listIterator, listIterator, remove, remove, removeAll, removeAllElements, removeElement, 
    removeElementAt, removeRange, retainAll, set, setElementAt, setSize, **size,** subList, toArray, toArray, toString, trimToSize

* Methods inherited from class ***java.lang.Object***
    * finalize, getClass, notify, notifyAll, wait, wait, wait
    
    
#### [#issue2] 큐(Queue)의 개념
    * 큐(Queue)란
        * 컴퓨터의 기본적인 자료 구조의 한가지로, 먼저 집어 넣은 데이터가 먼저 나오는 FIFO(First In First Out)구조로 저장하는 형식
        * public interface Queue<E> extends Collection<E>
        * 나중에 집어 넣은 데이터가 먼저 나오는(LIFO) 스택과는 반대되는 개념
    
    * 큐(Queue)의 사용 예제
        * 우선순위가 같은 작업 예약 (인쇄 대기열)
        * 선입선출이 필요한 대기열 (티켓 카운터)
        * 콜센터 고객 대기시간
        * 프린터의 출력 처리
        * 윈도 시스템의 메시지 처리기
        * 프로세스 관리
        * 즉, 데이터가 입력된 시간 순서대로 처리해야 할 필요가 있는 상황에 이용
    
    * Implementing Class
        * 제네릭 형태로 사용할 때, 아래와 같이 큐를 구현한 클래스를 생성하여 사용
        * LinkedList
            * 끝에 요소를 추가하기에 용이하다.
            * List interface 구현
            * 요소에 null을 허용한다.
        * priorityQueue
            * 우선순위 큐
            * PIPO(Priority-In Priority-Out)
            * 정렬된 순서에 의해 반복
            * 요소에 null을 허용하지 않는다.           
        * priorityBlockingQueue
            * priorityQueue의 동기화된 버전

* [Queue Implementations 참고](https://docs.oracle.com/javase/tutorial/collections/implementations/queue.html)


#### [#issue2-1] 큐(Queue) 관련 메서드       
    * 두 가지 형태의 메서드 
        1. 수행이 실패했을 때 exception을 발생
            * add(e), element(), remove() 
        2. 수행이 실패했을 때 null 또는 false를 반환
            * offer(e), peek(), poll() 
        
    * boolean add(E item)
        * 해당 item을 Queue에 삽입
        * 삽입에 성공하면 true를 반환, 삽입할 공간이 없는 경우는 예외(IllegalStateException)를 발생
    * E element()
        * Queue의 Head에 있는 item을 삭제하지않고 해당 item을 반환
        * 만약 Queue가 비어있으면 예외를 발생
    * E remove()
        * Queue의 Head에 있는 item을 삭제하고 해당 item을 반환
        * 만약 Queue가 비어있으면 예외를 발생
        
    * boolean offer(E item)
        * add(e)와 동일한 기능
        * 삽입에 성공하면 true를 반환, 실패하면 false를 반환
    * E peek()
        * element()와 동일한 기능
        * 만약 Queue가 비어있으면 null을 반환
    * E poll()
        * remove()와 동일한 기능
        * 만약 Queue가 비어있으면 null을 반환

* Methods inherited from interface ***java.util.Collection***
    * addAll, clear, contains, containsAll, equals, hashCode, **isEmpty,** iterator, remove, removeAll, retainAll, **size,** 
    toArray, toArray
    

#### [#issue3] 덱(Deque, Double-ended Queue)의 개념
    * 덱(Deque, Double-ended Queue)이란
        * 양쪽 끝에서 삽입과 삭제가 모두 가능한 자료 구조의 한 형태
        * public interface Deque<E> extends Queue<E>
        * 큐와 스택을 합친 형태로 생각할 수 있다.
        
    * Implementing Class
        * ArrayDeque
            * Deque 인터페이스를 구현한 Resizable-Array. 
            * external synchronization이 되어있지 않아서 thread-safe하지 않음
        * ConcurrentLinkedDeque
            * Linked node로 이루어진 Concurrent deque
        * Concurrent - : 
            * multiple thread 환경에서 Element(node)를 삽입, 제거, 접근을 병렬적으로 처리할 수 있도록 하는 컬렉션들         
        * LinkedBlockingDeque
            * Linkded node로 이루어진 Deque. 
            * Integer.MAX_VALUE의 크기까지만 생성이 가능
        * LinkedList
            * List와 Deque를 구현한 Doubly-Linked List

* [Deque Implementations 참고](https://docs.oracle.com/javase/tutorial/collections/implementations/deque.html)
        
        
#### [#issue3-1] 덱(Deque, Double-ended Queue) 관련 메서드
![](/contents/images/deque-method.png)

* Methods inherited from interface ***java.util.Collection***
    * addAll, clear, containsAll, equals, hashCode, **isEmpty,** removeAll, retainAll, toArray, toArray
    
* [https://docs.oracle.com/javase/7/docs/api/java/util/Deque.html 참고](https://docs.oracle.com/javase/7/docs/api/java/util/Deque.html)
        
        
        
#### [#issue4] String indexOf()의 사용법 
    * indexOf(inc ch)
        * String에서 첫 번째 ch index 반환
        * default : -1
    * indexOf(String s)
        * String에서 첫 번째 s의 시작 index 반환
    * indexOf(int ch, int beginIndex)
        * String에서 beginIndex이상의 첫 번째 ch index 반환
    * indexOf(String s, int beginIndex)
        * String에서 beginIndex이상의 첫 번째 s의 시작 index 반환

* Ex) 알파벳 소문자로만 이루어진 단어 s에서 각각의 알파벳에 대해서, 단어에 포함되어 있는 경우에는 처음 등장하는 위치를, 포함되어 있지 않은 경우에는 -1을 출력하기
~~~java
String s = "dohee";
for(int i='a'; i<='z'; i++) {
    System.out.print(S.indexOf(i)+" ");
}
// 출력 : -1 -1 -1 0 3 -1 -1 2 -1 -1 -1 -1 -1 -1 1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 
~~~



#### [#issue5] String substring()의 사용법
    * substring(int beginIndex)
        * beginIndex 부터 끝까지 모든 문자열 반환
    * substring(int beginIndex, int endIndex)
        * beginIndex 부터 endIndex까지의 문자열 반환
  
* Ex) 문자열 s가 주어졌을 때, 모든 접미사를 사전순으로 정렬한 다음 출력하기      
~~~java
String s = "dohee";
String[] suffix = new String[s.length()];

for (int i=0; i<s.length(); i++){
    suffix[i] = s.substring(i);
}
Arrays.sort(suffix);
for (int i=0; i<s.length(); i++){
    System.out.println(suffix[i]);
}

/* 출력
   dohee
   e
   ee
   hee
   ohee
*/
~~~



### Reference
> - [https://docs.oracle.com/javase/7/docs/api/java/util/Stack.html](https://docs.oracle.com/javase/7/docs/api/java/util/Stack.html)
> - [https://docs.oracle.com/javase/7/docs/api/java/util/Queue.html](https://docs.oracle.com/javase/7/docs/api/java/util/Queue.html)
> - [https://docs.oracle.com/javase/7/docs/api/java/util/Deque.html](https://docs.oracle.com/javase/7/docs/api/java/util/Deque.html)
> - [http://dreamzelkova.tistory.com/entry/JAVA-%EC%BB%AC%EB%A0%89%EC%85%98%ED%81%90Queue](http://dreamzelkova.tistory.com/entry/JAVA-%EC%BB%AC%EB%A0%89%EC%85%98%ED%81%90Queue)
> - [https://ko.wikipedia.org/wiki/%ED%81%90_(%EC%9E%90%EB%A3%8C_%EA%B5%AC%EC%A1%B0)](https://ko.wikipedia.org/wiki/%ED%81%90_(%EC%9E%90%EB%A3%8C_%EA%B5%AC%EC%A1%B0))
> - [https://wayhome25.github.io/cs/2017/05/28/algorithm/](https://wayhome25.github.io/cs/2017/05/28/algorithm/)
> - [https://ko.wikipedia.org/wiki/%EB%8D%B1_(%EC%9E%90%EB%A3%8C_%EA%B5%AC%EC%A1%B0)](https://ko.wikipedia.org/wiki/%EB%8D%B1_(%EC%9E%90%EB%A3%8C_%EA%B5%AC%EC%A1%B0))
> - [http://opensourcedev.tistory.com/3](http://opensourcedev.tistory.com/3)
> - [https://docs.oracle.com/javase/tutorial/collections/implementations/queue.html](https://docs.oracle.com/javase/tutorial/collections/implementations/queue.html)
> - [http://jamesdreaming.tistory.com/81](http://jamesdreaming.tistory.com/81)


### :house: [Go Home](https://github.com/Do-Hee/algorithm-study) 
