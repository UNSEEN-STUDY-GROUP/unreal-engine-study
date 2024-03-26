

# 언리얼 엔진의 포인터


언리얼 엔진에서 사용하는 Pointer에 대해 알아보자

크게 두 가지로 분류할 수 있다. 

## Managed Pointer ( 관리되는 포인터 )

  ![Untitled](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/1d663e00-a9f5-4f04-bfc3-de214fb69a5d)
  
  **Garbage Collector**에 의해 관리되는 포인터를 뜻한다.  
  
  UObject에서 활용이 가능한 경우
  
  크게  강력(소유하는)/ 약한(소유하지 않는) 포인터로 구분할 수 있다. 
  
  ### 1. Hard Object Pointer (강함)
  
  UObject에 대해 ‘hold’ 혹은 ‘dependent’ 
  
  즉, 점유하고 있는 상태를 말한다.
  
  이런 Hard Object Pointer가 UObject를 참조하고 있는 경우,
  
  GC가 Object를 참조하지 않는다. 
  
    
  
  ```cpp
  UPROPERTY() UObject* Pointer = nullptr;
  
  IsValid(Pointer) // 유효성 판별
  ```
  
  ```cpp
  TObjectPtr<UCapsuleComponent> CapsuleComponent;
  ```
  
  Unreal Engine 5부터 이러한 형태로 사용하는 것을 권장함
  
  ### 2. Weak Object Pointer (약함)
  
   UObject를 소유하지 않음. 
  
  그래서 GC(또는 리플렉션)가 해당 포인터가 객체를 참조하는 것을 알 수 없다. 
  
  그래서 **Weak Pointer가 Object를 참조하고 있어도, GC가 삭제할 수 있다.** 
  
  GC로 인해 **Object가 삭제된 경우에는 Null값을 참조**하게 된다.
  
   
  
  그렇기 때문에 WeakObjectPointer로 선언한 pointer에 접근할 때에는 
  
  **항상 유효성 판별이 필요**
  
  ```cpp
  TWeakObjectPtr<UObject> Pointer;
  
  IsValid(Pointer) // 유효성 판별
  ```
  
  ### 3. Soft Object Pointer
  
  ```cpp
  TSoftObjectPtr<UObject> Pointer;
  ```
    
  Weak Object Pointer 처럼 소유하지 않는 약한 참조를 한다. 
  
  그런데 Weak Pointer와 차이점이 무엇이냐 하면,
  
  얘는 어떤 instance를 pointing할 때, **메모리 주소 값 & asset의 주소** 를 저장한다.
  
  이것이 무슨 말이냐.
  
  **해당 UObject가 사용하는 Asset의 주소를 저장**한다는 것.
  
  때문에 Runtime에 에셋을 불러오는 것에 도움을 준다. 
  
  ```cpp
  TSoftObjectPtr<UTexture2D> ItemImage;
  ItemImage = "/Game/Textures/ItemTexture.ItemTexture";
  
  ItemImage.LoadSynchronous(); // 이미지 Texture를 로드
  ```
  


  ## Unmanaged Pointers
  
  기본적으로 C++이 제공하는 SmartPointer랑 거의 유사하다고 생각하면 될 것 같다. 
  
  reflection에 알리는 것이 아니라 자체적으로 동작하는 것.
  
  **UObject를 사용할 수 없는 포인터. 대신 FObject를 사용한다.** 
  
  ### 1. Unique Pointer
    
  ```cpp
  TUniquePtr<FObject> Pointer;
  ```
  
  일반적인 Unique Pointer의 역할을 한다. 
  
  **한 시점에 오직 하나의 Unique Pointer만 객체를 소유**함
  
  더 이상 객체를 소유하지 않거나, 다른 Unique Pointer가 객체를 소유하면 사라짐
  
  ### 2. Shared Pointer
  
  ```cpp
  TSharedPtr<FObject> Pointer;
  ```
  
  객체를 가리키는 Pointer가 0이 되면 객체를 삭제
  
  C++에서 제공하는 Shared Pointer와 유사
  
  ### 3. **Weak Pointer**
    
  ```cpp
  TWeakPtr<FObject> Pointer;
  ```
  
  C++에서 제공하는 Weak Pointer와 유사
  
  ### 4. Raw Pointer
  
  ```cpp
  FObject* Pointer = nullptr;
  ```
  
  원시적인 형태의 포인터 방식
  
  **안정성을 보장하지 않음**.
  
  때문에 조심해서 활용해야 한다.
  
  여기에 UPROPERTY 매크로만 붙이면 Hard Object Pointer가 된다. 
  
참조

[Pointer Types | Unreal Engine Community Wiki](https://unrealcommunity.wiki/pointer-types-m33pysxg)
