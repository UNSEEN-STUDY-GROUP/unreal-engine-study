# Smart Pointer


### 한 줄 요약 = 💡 자동으로 Memory 관리를 해주는 포인터




Smart Pointer – 추가기능을 가진 포인터다. 

● 일반적인 포인터에 비해 좋은점 몇가지 가지고 있음

● 포인터를 우리가 잡는다는 것은 메모리를 잡는 것. 

우리가 코드를 작성하며 동적으로 메모리를 할당할 때, 이를 잊어버릴 수 있다

**⇒ memory leak**인 상황
이런걸 막기 위해 **Smart Pointer**를 사용한다.

### Smart Pointer의 종류

- **Shared Pointer**
    - object가 **몇 개의 pointer에 의해 pointing** 되고 있는지 알려준다.
    - 참조되는 (pointing)의 수가 **0이 되면 해당 object를 지워버림**
  
  **순환 참조**
  
  ```cpp
  std::shared_ptr<MyClass> sharedPtr1 = std::make_shared<MyClass>();
  std::shared_ptr<MyClass> sharedPtr2 = std::make_shared<MyClass>();
  
  // sharedPtr1과 sharedPtr2가 서로를 가리키게 됨 (순환 참조)
  sharedPtr1->member = sharedPtr2;
  sharedPtr2->member = sharedPtr1;
  ```

- **Weak Pointer**
    - shared pointer를 이용할 때 두 object가 서로를 참조하고 있으면, 
    shared pointer에서는 영원히 지워지지 않음.
    - 이런 경우를 **Cyclic reference**라고 하고 이걸 방지하는 애가 Weak Pointer.
    - 객체를 소유하지 않는 약한 참조이기 떄문에 객체에 안전하게 접근함
    
    ```cpp
    // Weak pointer를 사용하여 순환 참조 방지
    std::weak_ptr<MyClass> weakPtr1 = sharedPtr1;
    std::weak_ptr<MyClass> weakPtr2 = sharedPtr2;
    ```
    

- **Unique Pointer**
    - 어떠한 object가 오직 한 개의 참조만이 가능한 포인터
    - **복사해서 사용이 불가능**
    - **이동해서 사용은 가능**!
        - A라는 unique_ptr에서 B라는 unique_ptr로 이동한다면, A는 더 이상 객체를 소유하지 않음
            
    ```cpp
    std::unique_ptr<Resource> ptrA(new Resource());
    std::unique_ptr<Resource> ptrB;
    
    ptrB = std::move(ptrA);
    ```
            

### 사용 예시

1. **std::unique_ptr 예시:**

```cpp
#include <memory>
#include <iostream>

class MyClass {
public:
    MyClass() {
        std::cout << "MyClass 생성자 호출\n";
    }

    ~MyClass() {
        std::cout << "MyClass 소멸자 호출\n";
    }

    void doSomething() {
        std::cout << "뭔가를 수행 중...\n";
    }
};

int main() {
    // std::unique_ptr을 사용하여 동적으로 할당된 객체를 관리
    std::unique_ptr<MyClass> uniquePtr = std::make_unique<MyClass>();

    // 객체에 접근
    uniquePtr->doSomething();

    // 메모리는 자동으로 관리되므로 따로 delete할 필요 없음

    return 0; // uniquePtr가 범위를 벗어나면 MyClass는 자동으로 삭제됨
}

```


2. **std::shared_ptr 예시:**

```cpp

#include <memory>
#include <iostream>
class MyClass {
public:
    MyClass() {
        std::cout << "MyClass 생성자 호출\n";
    }

    ~MyClass() {
        std::cout << "MyClass 소멸자 호출\n";
    }

    void doSomething() {
        std::cout << "뭔가를 수행 중...\n";
    }
};

int main() {
    // std::shared_ptr을 사용하여 여러 곳에서 동적으로 할당된 객체를 공유
    std::shared_ptr<MyClass> sharedPtr1 = std::make_shared<MyClass>();
    std::shared_ptr<MyClass> sharedPtr2 = sharedPtr1; // 참조 계수가 증가

    // 객체에 접근
    sharedPtr1->doSomething();
    sharedPtr2->doSomething();

    // 메모리는 참조 계수에 따라 자동으로 관리됨

    return 0; // sharedPtr1과 sharedPtr2가 범위를 벗어나면 MyClass는 자동으로 삭제됨
}

```
