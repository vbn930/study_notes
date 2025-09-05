락이 걸렷을때 대기 상태 처리 방법
1. 락이 종료 될때 까지 대기 -> spin lock
2. sleep 하다가 랜덤한 시간에 다시 락이 걸렸는지 확인 -> 확실성이 낮음
3. 옵저버를 통해서 락이 풀리면 알림을 받는 형태로 대기 -> 추가적인 리소스를 사용해야함

## 컨텍스트 스위칭
user level <-> kernel level 이동이 부하가 있다
하지만 컨텍스트 스위칭은 반드시 발생한다

spin lock -> user 레벨에서만 대기-> 컨텍스트 스위칭이 줄어든다

## 스핀락 구현
```cpp
class SpinLock {
public:
	void lock() {
		// CAS (Compare And Swap) -> 원자적 연산
		// _locked.compare_exchange_strong(expected, desired) -> 만약, _locked가 expected와 같다면 desired로 변경.
		// _locked가 expected와 같다면, true를 반환, 다르다면 false를 반환.
		/*
		if (_locked == expected) {
			expected = _locked;
			_locked = desired;
			return true;
		}
		else {
			expected = _locked;
			return false;
		}
		*/

		// lock이 걸려있는 상태라면, 계속 반복문을 순회하면서 lock이 풀리기를 기다린다.
		// lock이 풀리는 순간, _locked가 false로 바뀌므로, desired가 true이므로, _locked는 true로 바뀌게 된다.
		bool expected = false;
		bool desired = true;
		while (_locked.compare_exchange_strong(expected, desired) == false) {
			expected = false;
		}

		// 이 방식이 효율적인 경우는, lock이 걸려있는 시간이 매우 짧은 경우이다. 
		// 이 경우엔 유저 모드와 커널 모드를 오가는 컨텍스트 스위칭 비용을 낭비하기 보단, 
		// 단순하게 유저 모드에서 계속 반복문을 통해 대기하는 것이 더 효율적이기 때문이다.
	}

	void unlock() {
		_locked.store(false);
	}

private:
	atomic<bool> _locked = false; // -> 컴파일러 최적화를 방지하는 키워드
	// 컴파일러가 코드 자체를 자동으로 최적화 해준다. 이때, volatile 키워드를 사용하면 컴파일러가 해당 변수를 최적화하지 않도록 지시할 수 있다.
};
```

## sleep
```cpp
bool expected = false;
bool desired = true;
while (_locked.compare_exchange_strong(expected, desired) == false) {
	expected = false;
	this_thread::sleep_for(std::chrono::milliseconds(0)); // time_slice 포기하고 커널 모드에서 대기
	// this_thread::sleep_for(100ms);
	// this_thread::yield(); // -> 다른 스레드에게 양보
}
```

## event
유저 모드 뿐만 아니라 커널 모드 또한 사용하는 동기화 방식
event 라는 커널 오브젝트를 handle을 통해서 사용
auto reset event -> 시그널이 자동으로 초기화
manual reset event -> 시그널을 감지 후 수동으로 초기화 해 주어야함

```cpp
mutex m;
queue<int32> q;
HANDLE handle;

void Producer() {
	while (true) {
		{
			unique_lock<mutex> lock(m);
			q.push(100);
		}

		::SetEvent(handle); // 이벤트 신호 상태로 변경
		this_thread::sleep_for(chrono::milliseconds(10000));
	}
}

void Consumer() {
	while (true) {
		{
			// 이벤트가 발생 하는 경우에만 해당 스레드를 깨워서 행동을 실행
			::WaitForSingleObject(handle, INFINITE); // 이벤트가 신호 상태가 될 때까지 대기
			// 이벤트가 신호 상태가 되면 자동으로 리셋됨
			// Manual-Reset 이벤트는 수동으로 리셋해야 함
			// ::ResetEvent(handle); // 수동 리셋
			unique_lock<mutex> lock(m);
			if (!q.empty()) {
				int32 v = q.front();
				q.pop();
				cout << "Consumed: " << v << endl;
			}
		}
	}
}


int main()
{
	// 커널 오브젝트 -> 커널에서 관리하는 객체
	// Usage count
	// Singnal, None-Singnal
	// Auto -Reset, Manual-Reset
	handle = ::CreateEvent(NULL /*보안 속성*/, FALSE /*리셋 방법*/, FALSE /*초기 시그널 상태*/, NULL);
	thread t1(Producer);	
	thread t2(Consumer);

	t1.join();
	t2.join();

	::CloseHandle(handle);
}
```