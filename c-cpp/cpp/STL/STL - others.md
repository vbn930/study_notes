# set, map, multimap, multiset
## set
set은 map과 비슷하지만, key와 value가 같은 하나의 값을 가지는 컨테이너이다. 
```cpp
std::set<int> s;
```
위와 같이 선언 할 수 있고, 사용 방법은 대부분 map과 똑같다. 하지만, map과는 다르게 [] 연산자를 사용할수 없다.
## multimap
multimap은 하나의 key에 여러 value를 할당 할 수 있는 컨테이너이다. map에선 key와 value가 1:1 관계였다면, multimap 에서는 key와 value가 1:N 관계이다.

multimap에서 erase를 통해 value를 삭제하면, 해당 key에 할당된 value가 모두 삭제된다.

또한, **equal_range** 함수를 통해 해당 key에 할당된 값들의 범위를 찾을 수 있다. **equal_range**는 해당 범위의 반복자 pair를 반환한다. 여기서 주의해야 할 점은, 반복자를 통한 순회를 쉽게 하기 위해서 반환된 pair의 second 값은 찾은 범위의 마지막 원소의 다음을 반환한다.
## multiset
multiset 또한 같은 값의 key를 여러개 할당 할 수 있는 컨테이너이다.
multimap과 대부분 같은 문법과 함수를 공유한다 (**equal_range**와 같은 함수를 multiset에서도 사용 할 수 있다).