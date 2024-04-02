# **1. 액터와 액터 컴포넌트의 차이**

### 1) 유니티의 오브젝트와 언리얼의 액터 비교

| 특성 | Unity 오브젝트 | Unreal 액터 |
| --- | --- | --- |
| 기본 단위 | Scene상의  Hierachy에서GameObject 존재. | 레벨에 배치되는 오브젝트. AActor 를 베이스로 함. |
| 컴포넌트 시스템 | 컴포넌트를 추가하여 기능 확장 가능. 
추가 기능은 C#으로 작성 된 스크립트를 추가하여 확장 | 액터 컴포넌트를 부착하여 기능 확장 가능.
블루프린트나 액터 컴포넌트를 상속받아 확장
- UActorComponent
- USceneComponent
- UPrimitiveComponent |
| 정리 | 빈 그릇과 같은 상태로, 여러 기능을 컴포넌트를 부착하거나 스크립트를 확장하여 구현해야 함. | 고유의 라이프사이클이 있으며, 프레임 별 로직을 적용할 수 있는 틱 함수를 가지고 있음. |

<aside>
💡 언리얼 엔진에서,
액터란 트랜스폼(위치, 회전, 스케일)과 같은 컴포넌트를 부착할 수 있는 있는 엔티티.
액터 컴포넌트는 액터에 추가하여 액터의 동작을 확장할 수 있는 기능.
컴포넌트는 생성시 자신을 포함하고 있는 액터에 할당.

</aside>

- **UActorComponent**
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/d49cae90-1ad8-4f21-9a8e-99ff5e014b48/Untitled.png)
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/61c3af3e-ce65-4173-b2a2-67dd5619191c/Untitled.png)
    
    기본적인 기능을 제공하는 베이스 컴포넌트로, 액터의 일부로 포함되며(Register로 등록), 액터에 부착되어 activation/deactivation 상태 관리가 가능하고, 틱이 가능.
    
    - **UMovementComponent**
        
        액터에 특정한 이동 기능을 추가해 주는 컴포넌트. 예를 들어, ProjectileComponent는 투사체로서의 움직임을 구현함.
        
    - **USceneComponent**
        
        트랜스폼(위치, 회전, 크기 등)을 가지는 액터 컴포넌트로, 게임 월드 내에서의 공간적 위치를 액터에 부여.  오직 씬 컴포넌트(`USceneComponent` 및 그 자손 클래스)만이 서로 어태치될 수 있습니다
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/d97e8c19-05b1-4741-a983-78eeb7a279db/Untitled.png)
        
        계층 구조 형태로 부모 자식 관계로 서로에게 붙일 수 있음.  [USceneComponent](https://docs.unrealengine.com/4.26/en-US/API/Runtime/Engine/Components/USceneComponent/index.html) 
        
        - **UPrimitiveComponent**
            
            ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/76d12e48-b2c8-4d0f-b4c8-99a6bc5e676e/Untitled.png)
            
            월드에 렌더링되는 메시 컴포넌트나 쉐이프 컴포넌트 등, 주된 용도는 렌더링이나 콜리전. 가장 흔한 것은 **박스 컴포넌트(Box Component),** **캡슐 컴포넌트(Capsule Component),** **스태틱 메시 컴포넌트(Static Mesh Component)** 및 **스켈레탈 메시 컴포넌트(Skeletal Mesh Component).**
            
        - **UCameraConponent**
            
            카메라 뷰와 관련된 설정(FOV, 프로젝션 타입 등)을 관리하는 기능 제공.
            
- **AActor**
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/6a4fe2e8-8a56-4270-b208-97ffaed32301/Untitled.png)
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/9eae17ff-0f65-440e-b8a5-5cb0c200a170/Untitled.png)
    
    고유의 라이프사이클이 있으며, 프레임 별 로직을 적용할 수 있는 틱 함수를 가지고 있음.
    

### 2) 액터와 액터 컴포넌트의 차이

| 구분 | 액터 | 액터 컴포넌트 |
| --- | --- | --- |
| 정의 | 게임 월드 내 배치되어 상호작용 가능한 모든 객체. Transform을 비롯한 여러 기능 컴포넌트를 가짐. | 액터에 부착되어 기능을 확장하는 요소. 렌더링, 사운드, 물리적 상호작용 등 다양한 형태로 존재. |
| 상속 클래스 | AActor에서 상속받음. 기본적인 게임 월드 내의 모든 액터의 기반 클래스. | UActorComponent에서 상속받아, 다양한 하위 클래스가 존재함. |

### 3) **컴포넌트 등록**

액터 컴포넌트가 각 프레임을 업데이트하고 씬에 영향을 미치도록 하려면, 엔진이 액터 컴포넌트를 **등록(register)** 해야 함. `RegisterComponent` 함수가 이러한 기능을 제공.

| 함수 | 설명 |
| --- | --- |
| OnRegister | 컴포넌트를 등록할 때 이 함수를 오버라이드하여 코드를 추가할 수 있습니다. |
| CreateRenderState | 이 함수는 컴포넌트의 https://dev.epicgames.com/documentation/404를 초기화합니다. |
| OnCreatePhysicsState | 이 함수는 컴포넌트의 https://dev.epicgames.com/documentation/404를 초기화합니다. |

# 2. 액터의 라이프사이클

## 1-1. 준비단계

액터가 생성될 때, 초기화 작업을 수행함.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/4560b4b3-06b9-4944-a2d6-23758666671a/Untitled.png)

## - Disk Load

이미 레벨에 배치된 액터가 LoadMap이 발생하였을 때나 AddToWorld가 호출되었을 때 로드됨. 디스크에서 액터가 로드되고, PostLoad에서 액터가 제대로 로드되었는지 확인함.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/5a612512-953b-47dd-b42d-a5c19315ccd8/Untitled.png)

## - Editor Play

에디터에 있는 액터를 복제하여 사용함. PostDuplicate을 호출함.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/693efb61-f9e5-433c-86d1-15f833a63047/Untitled.png)

## - 스폰

액터를 실시간으로, 동적으로 생성할 때의 과정으로, SpawnActor 함수를 호출하여 액터의 새 인스턴스를 생성함. 이후 PostSpawnInitialize, PostActorCreated, ExecuteConstruction을 순차적으로 실행. OnConstruction 생성시 액터의 생성 지점으로 액터의 생성 지점에서 블루프린트 액터가 컴포넌트를 만들고 변수를 초기화함.

- SpawnActor 호출
    
    ```cpp
    AActor* UWorld::SpawnActor
    (
        UClass*         Class,
        FName           InName,
        FVectorconst*  Location,
        FRotatorconst* Rotation,
        AActor*         Template,
    bool            bNoCollisionFail,
    bool            bRemoteOwned,
        AActor*         Owner,
        APawn*          Instigator,
    bool            bNoFail,
        ULevel*         OverrideLevel,
    bool            bDeferConstruction
    )
    ```
    
    Actor의 새 인스턴스를 생성. UWorld::SpawnActor(), 반환값은 Actor
    

## 디퍼드 스폰

 기존의 SpawnActor는 생성과 동시에 BeginPlay를 호출. 하지만 BeginPlay 이전에 개발자가 어떠한 액터에 추가 작업을 해야 할 경우 SpawnActorDeferred를 사용.

 Expose On Spawn (스폰시 노출)로 설정된 프로퍼티가 있으면 개발자가 초기화하려는 값을 미리 설정 한 후에 나중에 몰아서 스폰이 됨. 다만 초기화로직을 수행한 이후에 액터가 게임 월드에 완전히 추가되고, 모든 초기화 작업이 완료된 것을 알리기 위해 FinishSpawning을 호출해 주어야 함.

## 2. 액터 컴포넌트 초기화

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/75a9177b-bf05-4102-a0d8-482d49a17991/Untitled.png)

> 주요 함수
> 
> - **PreInitializeComponents**
>     
>     ```cpp
>     void AActor::PreInitializeComponents()
>     {
>     	if (AutoReceiveInput != EAutoReceiveInput::Disabled)
>     	{
>     		const int32 PlayerIndex = int32(AutoReceiveInput.GetValue()) - 1;
>     
>     		APlayerController* PC = UGameplayStatics::GetPlayerController(this, PlayerIndex);
>     		if (PC)
>     		{
>     			EnableInput(PC);
>     		}
>     		else
>     		{
>     			GetWorld()->PersistentLevel->RegisterActorForAutoReceiveInput(this, PlayerIndex);
>     		}
>     	}
>     }
>     ```
>     
>     PlayerController를 받아와 EnableInput에 넘기는 등, 액터의 컴포넌트를 설정하기 전에 호출됨.
>     
> - **InitializeComponents**
>     
>     ```cpp
>     void AActor::InitializeComponents()
>     {
>     	QUICK_SCOPE_CYCLE_COUNTER(STAT_Actor_InitializeComponents);
>     
>     	TInlineComponentArray<UActorComponent*> Components;
>     	GetComponents(Components);
>     
>     	for (UActorComponent* ActorComp : Components)
>     	{
>     		if (ActorComp->IsRegistered())
>     		{
>     			if (ActorComp->bAutoActivate && !ActorComp->IsActive())
>     			{
>     				ActorComp->Activate(true);
>     			}
>     
>     			if (ActorComp->bWantsInitializeComponent && !ActorComp->HasBeenInitialized())
>     			{
>     				// Broadcast the activation event since Activate occurs too early to fire a callback in a game
>     				ActorComp->InitializeComponent();
>     			}
>     		}
>     	}
>     ```
>     
>     컴포넌트들을 초기화하는 단계임.
>     
> - PostInitializeComponents
>     
>     ```cpp
>     void AActor::PostInitializeComponents()
>     {
>     	QUICK_SCOPE_CYCLE_COUNTER(STAT_Actor_PostInitComponents);
>     
>     	if(IsValidChecked(this) )
>     	{
>     		bActorInitialized = true;
>     		
>     		UpdateAllReplicatedComponents();
>     	}
>     }
>     ```
>     
>     컴포넌트 초기화 이후, 액터가 완전히 초기화된 상태임을 나타내는 단계임.
>     
> 

액터의 생성과 초기화 과정을 통해 게임 플레이에 필요한 다양한 설정이 준비되고, BeginPlay는 플레이가 시작될 때 호출되며, 이 시점부터 액터는 게임 로직에 따라 다양한 행동을 취함.

<aside>
💡 디스크로드는 디스크에서 직접 액터와 게임 데이터를 로드하는 과정으로 보이고, PIE는 에디터 플레이 시 에디터에 로드 되어 있는 액터를 복사하는 과정, 스폰 액터는 동적으로 액터를 인스턴스화 하여 생성하는 과정으로 보인다.

</aside>

| 구분 | Deferred Spawn | Spawn | Disk Load | PIE (Play In Editor) |
| --- | --- | --- | --- | --- |
| 정의 | 액터를 게임 월드에 추가하기 전에 추가적인 초기화 작업을 수행할 수 있도록 유예시키는 방법. SpawnActorDeferred 함수 사용 | 게임 월드에 액터를 동적으로 생성하고 즉시 추가하는 방법. SpawnActor 함수 사용 | 레벨이 로드될 때 디스크에서 직접 액터와 게임 데이터를 로드하는 과정 | 에디터에서 직접 게임을 실행하여 테스트하는 기능. 에디터에 있는 액터를 새 월드로 복제하여 사용 |
| 주요 특징 | - BeginPlay 호출 전에 액터에 대한 추가 작업 가능Expose On Spawn으로 설정된 프로퍼티를 초기화할 수 있음 FinishSpawning 호출 필요 | - 생성과 동시에 BeginPlay가 호출됨 동적으로 게임 로직에 따라 액터 생성 필요 시 사용 | - 레벨 로딩 시 액터 로드LoadMap이 발생하거나 AddToWorld가 호출될 때 실행 | - 개발 중인 게임을 실시간으로 테스트 에디터 내 액터 복제 후 새 월드에서 사용 PostDuplicate 호출 |
| 사용 시나리오 | 액터 생성 전에 특정 설정이나 초기화가 필요한 경우. 예: 네트워크 게임에서 클라이언트 동기화 전 초기화 필요 | 게임 도중 동적으로 액터를 생성해야 할 때. 예: 아이템이나 적 생성 | 게임 레벨이 시작될 때 기본적으로 존재해야 하는 액터들을 로드할 때 | 게임 개발 과정에서 실시간으로 변경 사항을 테스트하고 싶을 때 |
| 이점 | - 세밀한 초기화 제어 가능 초기화 과정에서 조건부 로직 적용 용이 | - 즉각적인 액터 생성과 사용간단한 사용 사례에 적합 | - 레벨과 함께 필요한 모든 액터를 한 번에 로드 게임 시작 시 필요한 환경 자동 구성 | - 개발 중인 게임의 실시간 테스트에디터 상태에서 게임의 동작 확인 가능 |

## 수명의 마지막

액터의 소멸 과정은 여러 방식으로 진행되지만, 공통적으로 액터는 레벨에서 제거되고, 가비지 컬렉션에 의해 최종적으로 메모리에서 해제됨. 

### **Destroy 소멸 과정**

- **Destroy() 함수 호출**: 액터를 소멸시키기 위해 수동으로 호출. 이때 액터는 'Pending Kill' 상태로 마킹되며, 레벨의 액터 목록에서 제거됨.
- **Pending Kill 상태**: 액터가 가비지 컬렉션에 의해 처리될 준비가 됨. 가비지 컬렉터가 다음 주기에 실행될 때, Pending Kill로 마킹된 액터는 메모리에서 해제됨.

### **EndPlay 종료 과정**

액터의 수명이 끝나는 것을 명확히 하기 위해, **`EndPlay`** 함수는 액터가 더 이상 필요하지 않을 때 다양한 상황에서 호출됨.

- **Destroy 호출**: 액터를 명시적으로 소멸시키기 위해 Destroy() 함수가 호출되면 EndPlay가 실행됨.
- **에디터 플레이 종료**: 개발 중 에디터에서 게임을 실행한 후 종료할 때 EndPlay가 호출됨.
- **레벨 전환**: 게임 중 레벨 변경이 일어날 때(예: 심리스 트래블, 맵 로드) EndPlay가 실행됨.
- **스트리밍 레벨 언로드**: 게임 플레이 중 액터가 포함된 스트리밍 레벨이 언로드되면 EndPlay가 호출됨.
- **애플리케이션 종료**: 게임 종료 시 모든 액터에 대해 EndPlay가 실행됨.

### **메모리 관리**

- **RF_PendingKill 마킹**: 액터가 Pending Kill로 마킹되면, 가비지 컬렉션 주기에 따라 메모리에서 해제됨.
- **FWeakObjectPtr 사용**: 액터의 유효성을 검사할 때, 직접적인 Pending Kill 상태를 검사하기보다는 FWeakObjectPtr을 사용하는 것이 권장됨.

## 참고자료

https://www.youtube.com/watch?v=iQ3c-lrHO7o&t=175s

[액터의 수명 주기 | 언리얼 엔진 문서 (unrealengine.com)](https://docs.unrealengine.com/4.27/ko/ProgrammingAndScripting/ProgrammingWithCPP/UnrealArchitecture/Actors/ActorLifecycle/)

[Actor Lifecycle (tistory.com)](https://bonggon.tistory.com/50)

[[언리얼 엔진] 액터의 수명 주기 (tistory.com)](https://wecandev.tistory.com/91)

[Unreal Engine 4 - Game Flow and Actor Lifecycle Overview - bright developers](https://www.brightdevelopers.com/unreal-engine-4-game-flow-actor-lifecycle-overview/)

[컴포넌트 | Epic Developer Community (epicgames.com)](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/components-in-unreal-engine?application_version=5.0)