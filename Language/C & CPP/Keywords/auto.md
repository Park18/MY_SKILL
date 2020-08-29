# auto

## 1. 기능
```auto``` 키워드는 C++11 이전에는 다른 의미로 쓰였지만 C++11 이후에는 선언된 변수나 람다식의 타입을 컴파일러에게 추론하도록 지시한다.

## 2. 변수

### 2.1. 지역, 전역 변수
지역, 전역 변수로 ```auto``` 키워드를 사용할 때는 ```int```, ```char```, 등의 자료형을 사용할 때처럼 사용하면 된다.

> 예)
```c++
#include <iosatream>
#include <typeinfo>

using namespace std;

auto a = 1;                     // int

int main()
{
    auto b = 1;                 // int
	auto c = 1.2;               // double
	auto d = 'c';               // char
	auto e = "abc";             // char const *
	auto f = string("abc");     // string
	string g("abc");

	cout << typeid(a).name() << endl;
	cout << typeid(b).name() << endl;
	cout << typeid(c).name() << endl;
	cout << typeid(d).name() << endl;
	cout << typeid(e).name() << endl;
	cout << typeid(f).name() << endl;
    cout << typeid(g).name() << endl;
}
```

> 출력 결과
```
int
int
double
char
char const *
class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> >
class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> >
```

### 2.1.2. 포인터와 레퍼런스

#### 2.1.2.1. 초기화 값이 포인터, 레퍼런스인 경우
포인터 변수는 변수가 저장된 메모리 위치를 저장하는 변수이고 레퍼런스 변수는 변수의 별칭을 만드는 것이다. 이

> 예)
```c++
#include <iostream>
#include <typeinfo>

using namespace std;

int main()
{
	int num = 1;
	int& ref_num = num;
	int* ptr_num = &num;

	auto auto_num1 = ref_num;
	auto_num1 = 2;
	cout << typeid(auto_num1).name() << ": " << num << " - ";

	auto auto_num2 = ptr_num;
	*auto_num2 = 3;
	cout << typeid(auto_num2).name() << ": " << num << endl;
}
```
> 출력 결과
```
int: 1 - int *: 3
```
출력 결과로 2, 3 을 예상 했지만 예상과 다르게 1, 3이라는 값이 나왔다. 그 이유는 레퍼런스 변수는 결국 원본 변수 자체를 뜻하기 때문에 ```auto auto_num1 = ref_num```은 ```int``` 형 변수를 초기화한 것이다. 

## 2.2. 함수

### 2.2.1. 매개 변수에서 auto
```auto``` 키워드는 함수 매개 변수로 선언 했을 때, 타입을 추론할 수 없기 때문에 매개변수의 자료형으로 ```auto``` 키워드를 선언할 수 없다.

> 예)
```c++
#include <iostream>

using namespace std;

void print_add(auto a, auto b)  // error
{
    cout << a + b << endl;    
}
```

### 2.2.2. 반환 값에서 auto
C++14 부터 ```auto``` 키워드가 함수의 반환 타입을 자도응로 추론할 수 있도록 확장되었다.

> 예)
```c++
auto add(int a, int b)
{
    return a + b;
}
```

위 문법이 편해보일 수 있지만 컴파일러가 타입을 잘못 추론하여 의도하지 않은 오류가 발생할 수 있으므로 사용하지 않는 것이 좋다.

## 3. 클래스 및 구조체
클래스와 구조체에서 클래스, 구조체 크기를 알 수 없기 떄문에 멤버 변수로 ```auto```를 사용할 수는 없다. 하지만 함수의 반환값으로 ```auto```는 사용할 수 있다.

> 예)
```c++
class A
{
private:
    auto auto_num;          // error

public:
    auto add(int a, int b);
};
```