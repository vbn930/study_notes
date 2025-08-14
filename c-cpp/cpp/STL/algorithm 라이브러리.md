# algorithm
STL 라이브러리의 컨테이너들이 어떻게 데이터를 저장하는지에 대한 라이브러리였다면, algorithm은 데이터를 어떻게 사용할지에 대한 함수들을 담은 파일이다. 
```cpp
#include <algorithm>
```
위 선언으로 사용 할 수 있다.
## find
find 함수는 find(begin iter, end iter, value)의 형식으로 사용 가능하다. 해당 범위 내부에 value 값이 있는지 찾아준다. 만약, 해당 범위에 value와 같은 데이터가 존재하지 않는다면, end() 반복자를 반환한다.
## find_if
범위 내의 조건을 만족하는것을 값을 찾는 함수이다. find_if(begin iter, end iter, condition)의 형태로 사용 가능하다. condition은 bool을 반환값으로 가지고 value 하나를 인자로 받는 함수 포인터, 함수 객체, 람다 함수등을 대입 할 수 있다.
## count
find와 같은 형식으로 사용 가능하다. 해당 범위에 value와 같은 값을 가진 데이터의 개수를 반환한다.
## count_if
find_if 와 같은 형식으로 사용된다.해당 범위에 조건에 맞는 값 개수를 반환한다.
## all_of
해당 범위의 모든 데이터가 조건에 맞는지를 검사한다. bool을 반환값으로 가진다.
## any_of
조건에 맞는 데이터가 하나라도 있는지 검사한다.
## none_of
모든 데이터가 조건에 맞지 않는지를 검사한다.
## for_each
for_each(begin iter, end iter, task)의 형태로 사용한다. 해당 범위의 데이터에 task 인자로 넘겨준 작업을 범위 내의 데이터에 모두 수행한다. task는 condition과 마찬가지로 함수 포인터, 함수 객체, 람다 함수등을 대입 할 수 있다.
## remove
remove(begin iter, end iter, value)의 형태로 사용 가능하다. remove 함수를 사용 할 때 중요한점은, remove함수는 컨테이너의 크기를 줄이지 않는다는 것이다. 즉, 컨테이너 내부의 값을 삭제하는 함수가 아니다.

remove함수는 삭제 할 값을 가진 데이터를 뒤로 밀고, 삭제 되지 않을 값들을 모두 앞으로 당긴다. remove 함수는 삭제 될 범위의 제일 첫 원소를 가리키는 반복자를 반환한다.
```cpp
vector<int> v = {1, 2, 3, 4}; // 이렇게 값을 가진 벡터가 존재한다.
auto iter = remove(v.begin(), v.end(), 1); // 이렇게 1을 remove 하면 아래와 같은 형태로 vector 내부 구조가 변경된다.
[1, 2, 3, 4] -> [2, 3, 4, 1] // -> 삭제 할 값이 맨 뒤로 밀리고 나머지가 앞으로 복사된다.

erase(iter, v.end()); // 이렇게 반환된 반복자부터 vector의 end 반복자까지의 범위를 삭제 해 주어야 한다.

erase(remove(v.begin(), v.end(), 1), v.end()); // 일반적으로 이러한 형태로 사용한다.
```
remove 함수가 위와 같은 방식으로 동작 하기 때문에, 반드시 반환된 반복자를 통해 해당 범위를 erase 함수를 통해 실제로 삭제 해주어야 한다.
## remove_if
: remove_if(begin iter, end iter, condition)의 형태로 사용한다. condition의 조건에 맞는 데이터를 컨테이너 맨 뒤로 이동 시킨다. remove 함수와 동작 방식은 같기 때문에 remove와 같은 방법으로 사용하여 데이터를 삭제 해 주어야 한다.