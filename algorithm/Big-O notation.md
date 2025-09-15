알고리즘의 실행 시간을 측정하는 방법에는 여러가지 방법이 존재한다.
1. 알고리즘을 실행하는데 걸리는 실제 시간을 측정 하는 방법 -> 해당 알고리즘을 실행하는 컴퓨터의 특징에 따라 시간이 달라지기 때문에 사용 하기 힘들다.
2. 알고리즘에서 assembly instruction 횟수를 측정하는 방법 -> 이 방법은 assmembly의 변환이 컴파일러 마다 다르기 때문에 범용적인 사용이 힘들다.
위의 방법들 외에도, 다양한 측정 방법이 존재 할 수 있다.

위와 같은 방법 외에 모든 환경에서 범용적으로 알고리즘의 복잡도를 표기하는 방법을 **Big-O** 표기법 이라고 한다.

일반적으로 Big-O 표기법을 사용해서 표기하는 복잡도에는 시간 복잡도, 공간 복잡도가 있다.
**시간 복잡도**란, 알고리즘의 시간적 효율성을 나타내는 방법이고, **공간 복잡도**란, 알고리즘의 공간의 효율성, 즉, 메모리를 얼마나 효율적으로 사용 하는 지를 나타내는 방법이다.

## Big-O 표기법의 특징
일반적으로 Big-O 표기법에서 데이터의 입력 값 N이 충분히 큰 수라고 가정한다. 즉, 반복 횟수가 10, 100과 같이 작은 수가 아니라, 엄청나게 큰 수라고 가정하는 것이다. 

위와 같은 특징으로 인해서 알고리즘 공식의 사소한 항들은 무시된다.
예를 들어, 특정 알고리즘의 공식이 f(x) =  2N^3+N+1 이라고 가정 해보자. 이때, N이 충분히 큰 수라면 최고 차항인 N^3을 제외한 나머지 상수항, 낮은 차수의 항들은 영향력이 크지 않다.

만약, N = 10,000,000,000 이라고 가정하고 공식에 대입 해보면, f(N) = 2 x 10,000,000,000^3 + 10,000,000,000 + 1 이 결과가 되는데, 이때, 10,000,000,000 + 1은 10,000,000,000^3에 비해서 상대적으로 매우 작은 값이기 때문에 무시 된다. 또한 같은 이유로 N^3의 상수 항인 2도 무시된다.

이러한 Big-O 표기법의 특징으로 인해, 알고리즘 코드의 반복문 (for-loop, while-loop)만 시간 복잡도 계산의 표기에 적용 된다.

### Big-O의 표기
Big-O 표기법을 표기 하는 방법은 **O(해당 알고리즘의 시간 복잡도)** 이다. 
예를 들어, 배열을 처음부터 순회 하면서 특정 원소를 찾는 알고리즘의 예시 코드는 아래와 같다.
```cpp
int arr[100]; // 100개의 원소가 있다고 가정
int target = 5; // 배열 내부에서 5라는 원소를 찾으려고 함
int idx = 0;
for(int i = 0; i < 100; i++){
	// 타켓 원소를 찾으면 해당 원소의 index를 저장하고 loop 빠져나옴
	if (target == arr[i]){ 
		idx = i;
		break;
	}
}
```
위 코드에선 for-loop가 한번 쓰였는데, 이는 배열을 배열의 크기만큼 한 번 순회한다는 의미이다. 그러므로 해당 알고리즘의 시간 복잡도를 Big-O로 표기하면, O(n)이 된다.
### Best/Worst/Average Case of algorithm
Big-O 표기법에서는 알고리즘의 worst-case의 경우를 표기한다. 이는, 알고리즘의 worst-case를 기준으로 표기하는 것이 해당 알고리즘의 성능 보장과 안정성을 평가하는데 가장 유용하기 때문이다.

worst-case를 기준으로 표기하게 되면, 어떤 입력 값이 들어오더라도, 적어도 해당 시간 이하로 실행 된다는 것을 보장한다. 이는 알고리즘을 사용 할 때 나, 성능 예측 시에 매우 중요하다.

만약 best/average-case를 사용한다면, 특정 예외적인 입력 값에서는 성능이 매우 저하되는 경우가 존재 할 수 있다. 

예를 들어, 배열을 순회하며 특정 값을 찾는 알고리즘의 경우, 찾는 배열이 첫 원소인 경우엔 O(1)의 시간이 소요 되지만, 만약 배열의 제일 마지막에 있다면, O(n) 만큼의 시간이 소요된다. 

만약 best-case인 O(1)로 알고리즘의 시간 복잡도를 표기한다면, 예외적인 입력 값이 입력 될 때, 알고리즘의 성능과 안정성을 보장하기 힘들어 진다.

## Algorithm Complexity Examples
### O(1) - Constant Time
```cpp
// Array access by index 
int getElement(int* arr, int index) 
{ 
	return arr[index]; 
} 
```

**Best Case** = O(1);
**Average Case** = O(1);
**Worst Case** = O(1);

### O(log n) - Logarithmic Time
```cpp
// Binary search on sorted array 
int binarySearch(int arr[], int n, int target) { 
	int left = 0, right = n - 1; 
	while (left <= right) { 
		int mid = left + (right - left) / 2; 
		if (arr[mid] == target) 
			return mid; 
		if (arr[mid] < target) 
			left = mid + 1; 
		else right = mid - 1; 
	} 
	return -1; 
}
```

**Best Case** = O(1);
**Average Case** = O(log n);
**Worst Case** = O(log n);

### O(n) - Linear Time
```cpp
// Linear search
int linearSearch(int arr[], int n, int target) { 
	for (int i = 0; i < n; i++){
		if (arr[i] == target){
			return i;
		}
	}
	return -1; 
}
```

**Best Case** = O(1);
**Average Case** = O(n/2) = O(n);
**Worst Case** = O(n);

![[Pasted image 20250914161807.jpg]]