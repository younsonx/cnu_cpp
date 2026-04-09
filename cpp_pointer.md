## 포인터

변수 : 값을 담는 상자  
포인터 : 그 상자가 어디 있는지 주소를 담는 변수

```cpp
int n = 3;
int* p = &n;
```
n에는 3이 들어 있고  
&n은 n의 주소  
p는 그 주소를 저장하는 변수  

p는 n을 직접 저장하는게 아니라 n이 있는 위치를 저장.  

#### &: 주소를 구하는 기호
여기서 &n은  
“변수 n이 메모리 어디에 있나요?”  
라는 뜻  
즉, &n = n의 주소  

#### *: 그 주소에 있는 실제 값을 꺼내는 기호

```cpp
int n = 10;
int* p = &n;

cout << *p;
```
p는 주소를 들고있고, *p는 그 주소에 들어있는 실제 값.  
*p를 출력하면 10이 나옴 / p를 출력하면 10의 주소값이 나옴  
=> 역참조 라고 함  
*는 "포인터가 가리키는 실제 값을 가져오는 연산"  

p : 주소
*p : 그 주소에 있는 값

#### p = p2 와 *p = *p2 는 다르다

p1 = p2;
주소를 복사  
즉, 같은 곳을 가리키게 됨

```cpp
int main() {
    int a = 5, b = 9;
    int* p1 = &a;
    int* p2 = &b;

    *p1 = *p2;

    cout << p1 << endl;
    cout << p2 << endl;
    cout << endl;
    cout << *p1 << endl;
    cout << *p2 << endl;
}
```
이렇게 했었을때의 실행결과가
```cpp
0x16b11b0b8
0x16b11b0b8

9
9
```
이렇게 나오며 똑같은 주소를 가리키기 때문에 값과 주소값이 같은 값으로 출력된다.  

*p1 = *p2;  
값을 복사  
즉, 가리키는 대상의 값만 같아짐  
```cpp
int main() {
    int a = 5, b = 9;
    int* p1 = &a;
    int* p2 = &b;

    *p1 = *p2;

    cout << p1 << endl;
    cout << p2 << endl;
    cout << endl;
    cout << *p1 << endl;
    cout << *p2 << endl;
}
```
이렇게 했었을떄의 실행 결과가
```cpp
0x16d1930bc
0x16d1930b8

9
9
```
이렇게 나오게 되는데 이는  
가리키는 값만 복사하며, 주소값은 복사하지 않는다 따라거 p1의 주소값이 원래 5의 주소값을 가리킨다.  

#### 포인터와 배열
```cpp
int arr[10];
```
arr은 사실상 &arr[0]처럼 동작  
arr[i]를 포인터로 작성하면 *(arr + i)로 작성할 수 있다.  
```cpp
arr : 첫 번째 칸 주소
arr + i : i칸 뒤 주소
*(arr + i) : 그 칸 안의 값
```
배열은 연속된 칸이기 때문에 주소 이동으로 접근할 수 있다.  

#### 실습 코드: 포인터와 배열
```cpp
#include <iostream>
using namespace std;

int main() {
    int n[10];
    int i;
    int* p;

    for (i = 0; i < 10; i++)
        *(n + i) = i * 3;

    p = n; // p = &n[0]과 거의 같은 뜻, 즉 p가 배열의 첫 칸을 가리킨다.
    for (i = 0; i < 10; i++) {
        cout << *(p + i) << ' ';
    }
    cout << '\n';

    for (i = 0; i < 10; i++) {
        *p = *p + 2;
        p++;
    }

    for (i = 0; i < 10; i++)
        cout << n[i] << ' ';
    cout << "\n";
}
```
p = &n[0]과 거의 같은 뜻, 즉 p가 배열의 첫 칸을 가리킨다.  

#### 실습코드: 포인터와 함수
```cpp
#include <iostream>
using namespace std;

void sneaky(int* temp) {
    *temp = 99;
    cout << "inside funtion call *temp == " << *temp << endl;
}
// temp는 주소를 저장하는 포인터.
// *temp는 그 주소에 들어있는 실제 값
// temp가 가리키는 곳의 값을 99로 바꿔라 라는 뜻
// temp가 n의 주소를 받으면 결국 n = 99가 된다.

int main() {
    int n = 11;
    int* p;

    p = &n;
    *p = 77;
    cout << "before call to funtion *p == " << *p << endl;

    sneaky(p);

    cout << "after call to funtion *p == " << *p << endl;

    return 0;
}
```
p = &n; 을 통해 p에 n의 주소값을 대입  
*p = 77; 을 통해 n이 가리키는 값과 주소값을 바꿀 수 있음  

#### 클래스와 포인터
객체 포인터: 클래스 타입 객체의 주소를 저장하는 포인터 변수  
-> 연산자: 포인터를 통한 멤버 접근(객체에 대한 포인터가 있을때 멤버 접근)    
. 연산자: 객체가 있을때 멤버 접근  
```cpp
• obj.member // 객체 자체에서 멤버 접근
• ptr->member // 객체 포인터에서 멤버 접근
```
```cpp
▪ * 연산자와, . 연산자의 조합
• ptr->member 는 사실상 (*ptr).member 와 같은 뜻
```

실습코드: 클래스와 포인터
```cpp

Circle.h

#pragma once
class Circle {
    int radius;
public:
    Circle(); // 기본생성자
    Circle(int r);
    double getArea();
    void setRadius(int r);
};
```

```cpp

Circle.cpp

#include "Circle.h"

Circle::Circle() {
    radius = 1;
}

Circle::Circle(int r) {
    radius = r;
}

double Circle::getArea() {
    return 3.14 * radius * radius;
}

void Circle::setRadius(int r) {
    radius = r;
}
```

```cpp

main.cpp

#include <iostream>
#include "Circle.h"
using namespace std;

int main() {
    Circle dount;
    Circle pizza(30);

    cout << dount.getArea() << endl;

    Circle* p;
    p = &dount;
    cout << p -> getArea() << endl;
    cout << (*p).getArea() << endl;

    p = &pizza;
    cout << p -> getArea() << endl;
    cout << (*p).getArea() << endl;
}
```


