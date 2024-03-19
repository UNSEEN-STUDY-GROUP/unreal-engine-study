## 부제 - Actor의 생성자, PostInitializeComponents(), BeginPlay()의 호출 시점

### 요약

1. **Actor의 생성자**는 엔진 초기화 이전에, FEngineLoop::PreInit()에서 모듈이 로드될 때 호출된다. 따라서, 생성자에는 게임 플레이와 관련된 코드는 포함하면 안된다.
→생성자 호출이 끝난 뒤에 바로 InitProperties(), PostInitProperties()가 호출된다.
2. **PostInitializeComponents()**는 맵 패키지의 로드가 끝난 뒤에 호출된다.
3. **BeginPlay()**는 게임을 시작하기 위한 모든 준비가 끝났을 때 호출된다.

## 전개

엔진 버전 : 5.3.2

엔진이 어떻게 시작 되는지 궁금해서 찾아봤는데, 너무 먼 길을 와버렸다.
내용이 너무 많아서 다 설명하기엔 시간이 부족하기 때문에, 파트 4_2강에서 사용한 생성자, PostInitializeComponent(), BeginPlay() 세 함수의 호출 시점을 찾아보는 것으로 주제를 정하고, 요약했다.
그 외에 찾아본 내용은 토글을 활용해 접어서 기록해 두기로 했다.

<aside>
<img src="/icons/snippet_gray.svg" alt="/icons/snippet_gray.svg" width="40px" /> **GuardedMain()**

- Main() 이전
    
    F11로 시작하면 처음 걸리는 위치 - 윈도우 환경이라서 여기서 시작한다. 
    
    ![Source\Runtime\Launch\Private\Windows\LaunchWindows.cpp](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/56fe2bfc-3aec-437a-9ba1-22031e67af8a/Untitled.png)
    
    Source\Runtime\Launch\Private\Windows\LaunchWindows.cpp
    
    해당 함수 안에 있는 GuardedMain이 진짜 시작이라고 보면 된다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/956625df-0600-4b91-b7b6-9c3042fd0632/Untitled.png)
    

GuardedMain()을 보면 기본적인 루프를 확인할 수 있다.
시작부터, BeginPlay()까지 알아볼 것이기 때문에, EnginePreInit(), EngineInit()만 들어갔다.
전체 흐름만 볼 수 있도록 하고, 아래에 EnginePreInit() 과 EngintInit() 에서 수행되는 과정을 간단하게 정리했다.

![Source\Runtime\Launch\Private\Launch.cpp](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/71bc12c5-3ad1-4f50-853a-12a333ee169c/Untitled.png)

Source\Runtime\Launch\Private\Launch.cpp

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/0311b177-f8ab-4e14-a655-aac197832706/Untitled.png)

- EditorInit() 과 EnginInit()의 차이 (별거 없다)
    
    EditorInit()을 타고 들어가보면, UnrealEd.Globals.cpp로 가지만, 그 안에서 EngineLoop.Init()을 호출해 결국 FEngineLoop::Init()으로 간다
    
    ![Source\Editor\UnrealED\Private\UnrealEdGlobals.cpp](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/a1d24ca3-854a-46f6-ae7c-40eee223981c/Untitled.png)
    
    Source\Editor\UnrealED\Private\UnrealEdGlobals.cpp
    
    ![Source\Runtime\Launch\Private\Launch.cpp](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/801184f2-23e0-4ad3-b1ba-1f0551f7c31f/Untitled.png)
    
    Source\Runtime\Launch\Private\Launch.cpp
    

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/c8576a57-ae0c-40f4-842a-a1c1db84ba3a/Untitled.png)

[PreInit()에서는 low-level 엔진 모듈을 로드해서 필수 시스템이 초기화되고 필수 유형이 정의된다.](https://youtu.be/IaU2Hue-ApI?si=N7oco_i8u7q4P1Na&t=219)

EnginePreInit()에서 로드하는 모듈은 아래와 같다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/afc106dc-69b6-4bcf-8f59-4d22208a663b/Untitled.png)

- 생성자 호출 시점을 확인한 과정
    
    흐름을 따라가면서 LoadPreInitModules() 에서 Engine 모듈을 로드할 때 Actor의 생성자가 호출되는 것으로 추측했다. (Actor.cpp가 Engine\Private 안에 있으니까)
    
    파트 1_5 강에서 생성자에 디버그를 걸었을 때 에디터 로딩 75%에서 걸린다는 것이 떠올랐고, 디버깅으로 위치를 확인해봤다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/5021a233-7925-4934-8493-8d376cde04a7/Untitled.png)
    
    PreInit() 안이긴 한데, 추측한 LoadPreInitModules()가 아니라 그 이후에 호출되는 LoadStartupModules()에서 호출됐다. 직접 만든 액터는 시스템에 필수적인 요소는 아니라서 필수 요소가 갖춰진 뒤에 호출되는 것 같다.
    
    어쨌든 이 시점에는 게임 플레이를 위한 준비는 되어있지 않은 상태이기 때문에, 생성자에는 게임 플레이 관련 코드를 포함하면 안되는 이유는 확인할 수 있었다.
    

다음으로 EngintInit()에서는 엔진을 생성, 초기화하고, 프로젝트와 플러그인 모듈을 로드하고 엔진을 시작한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/767819bd-0b87-496d-8553-63c0c51f0811/Untitled.png)

**GEngine→Init()** 에서는 엔진을 초기화하고, GameInstance, GameViewportClient, LocalPlayer 등을 생성 및 초기화한다.
**GEngine→Start()** 에서는 **LoadMap()** 을 통해 World, GameMode, GameState, PlayerController 등이 생성 및 초기화되어 게임을 시작할 준비를 마치고, **BeginPlay()** 를 호출한다.

**LoadMap()** 안에서 **BeginPlay()** 뿐 아니라 **PostInitializeComponents()** 도 호출된다.

아래 그림에서 ENGINE OBJECTS 파트를 **GEngine→Init()** 가, GAME OBJECTS 파트를 **GEngine→Start()** 가 생성한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/48995e2a-0022-4b58-8acf-bc43c754221c/Untitled.png)

</aside>

### FEngineLoop::PreInit() 내부

표시된 두 함수를 타고 들어가 봤다.

![Source\Runtime\Launch\Private\LaunchEngineLoop.cpp](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/a97aaaae-23e6-4b8e-97e6-7136fa32d7ad/Untitled.png)

Source\Runtime\Launch\Private\LaunchEngineLoop.cpp

먼저 PreInitPreStartupScreen() 에서는 [위에서 확인한 그림](https://www.notion.so/BeginPlay-dde211ce76704302b62e1c7fd29c3669?pvs=21)의 **LoadCoreModules()**, **LoadPreInitModules()** 를 호출하는 것을 확인했다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/600bfefe-3cc8-4702-9d3a-67dd44a4ab27/Untitled.png)

다음으로 PreInitPostStartupScreen() 에서는 [위에서 확인한 그림](https://www.notion.so/BeginPlay-dde211ce76704302b62e1c7fd29c3669?pvs=21)의 **LoadStartupCoreModules()** 를 호출한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/94de4e79-00ea-4a70-a18c-8e7ab30c584b/Untitled.png)

**LoadStartUpCoreModules()** 내부로 들어가보면 Core 모듈을 로드하는 것을 확인할 수 있다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/41909b2b-dea2-4f25-b617-7144a05e2e78/Untitled.png)

$\footnotesize\color{666666}{아래는~PreInitPreStartupScreen()~함수~내부의~코드}$
이 외에도 게임 이름을 설정하거나, 

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/2e61ef77-5517-44db-90bd-4614698edb2b/Untitled.png)

셰이더 코드 라이브러리를 준비하거나,

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/9c1fce09-75f7-4214-88c1-82aea7a56203/Untitled.png)

게임 스레드를 준비하거나,

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/e475e8f4-9503-4aeb-8067-42530dce69dc/Untitled.png)

난수 생성을 위한 준비하는 모습도 확인할 수 있었다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/0819b144-5205-45c8-9d4c-98e1d93d61b6/Untitled.png)

### FEngineLoop::Init() 내부

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/10360588-3f8f-44f9-9af8-2c1a7b55fdb6/Untitled.png)

UGameEngine의 Init()을 들어가 봤다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/b6d86b4e-81f7-4d3b-b33b-50642dd795ef/Untitled.png)

- **GEngine→Init()**
    
    **GameInstance, GameViewportClient, LocalPlayer** 생성 및 초기화를 확인할 수 있다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/fddd0794-2b55-417c-9665-932af62e1b5d/Untitled.png)
    
    추가로 UEngine::Inti() 을 들어가보면 플러그인 관련된 코드를 확인할 수 있는데,
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/b77a48e7-6304-4e9f-942b-61899e50a932/Untitled.png)
    
    .uplugin 파일을 열면 유사한 내용을 확인할 수 있다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/f140d46e-e385-4ae4-81f2-4345c520ca28/Untitled.png)
    

GEngine→Init() 이후에는 프로젝트와 플러그인 모듈을 로드한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/eee8310e-1d78-4f1a-b412-f1ff1aab962f/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/59a1622b-ea07-4608-a596-c5e7999e8189/Untitled.png)

- **GEngine→Start()**
    
    **Start()→StartGameInstance()→Browse()→LoadMap()** 으로 들어갔다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/68402b59-42ce-4263-984b-80c02528f058/Untitled.png)
    
    **LoadMap()** 에서 먼저 전역 Delegate로 맵이 변경됨을 알리고, 현재 맵을 언로드한다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/430d8a69-40ec-40de-869f-d36b8e18cae8/Untitled.png)
    
    현재 맵을 닫는 작업이 끝나면, 새 맵을 로드한다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/aa9505eb-e641-4b10-99ba-26bd74f41feb/Untitled.png)
    
    .umap 파일에 월드, 레벨, 액터, 컴포넌트가 직렬화되어 저장되는데, 이 파일을 로드하는 것이다. (파트 1_13강에서 직렬화에 대해 설명한다.)
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/c539bd61-1cfe-436b-bd3e-4caf1ae62470/Untitled.png)
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/0e5a5594-2436-45e0-a8b9-d949abaea4d8/Untitled.png)
    
    이렇게 파일을 로드하고 나면, 새 맵을 현재 맵으로 등록한다. 이후 InitWorld() 를 수행한다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/17f07b92-7c87-4951-84f2-1ee9a288c634/Untitled.png)
    
    InitWorld() 에서는 물리, 네비게이션, AI, 오디오 등의 시스템을 초기화한다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/1f10f7ec-ae05-4848-a142-e3fb82cc63b4/Untitled.png)
    
    이어서 SetGameMode()를 호출한다. SetGameMode() 에서 GameMode 액터가 생성된다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/5f4378a5-3eb0-481b-bb0e-3bba4ddaa5a3/Untitled.png)
    
    GameMode를 생성하고, 맵을 완전히 로드한다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/9c4a714e-d841-4aa2-8948-132cd8d84ed6/Untitled.png)
    
    다음으로, InitializeActorsForPlay() 를 호출해 액터 초기화 작업을 시작한다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/a58a5189-8b18-4e4d-b4f5-ae00d7a5a2a5/Untitled.png)
    
    InitializeActorsForPlay() 에서는 먼저 UpdateWorldComponents() 로 액터의 컴포넌트들을 월드에 등록한다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/847317b8-45a6-4ca4-8a63-89b2e4f376de/Untitled.png)
    
    이후, GameMode→InitGame()에서 GameSession 액터를 생성한다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/72193618-4694-4255-94e1-f09d8f48247f/Untitled.png)
    
    이어서 레벨마다 루프를 돌면서 각 레벨의 액터를 초기화한다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/58497a9d-9b7c-4da2-9286-d0500f04ff08/Untitled.png)
    
    여기서 PreInitializeComponents(), InitializeComponents(), PostInitializeComponents() 가 호출되는 것을 확인할 수 있다. $\sixptsize\color{666666}{드디어!!}$
    이 과정에서 GameMode의 PreInitializeComponents() 또한 호출되고, 그 안에서 GameState가 생성 및 초기화된다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/091733a0-0279-4c9c-aa45-979b2d56e0c5/Untitled.png)
    
    이로써 월드가 완전히 초기화되고 게임 프레임워크가 생성됐다.
    다음은 플레이어 관련 작업이 진행되는데,,,,, $\sixptsize\color{666666}{이~부분은~체력과~정신력의~문제로~더~파고들지~못했다...}$
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/086e44d3-e8a3-4927-b756-fd14c9defd05/Untitled.png)
    
    플레이어 작업까지 진행되고 나면 비로소 **BeginPlay()** 와 마주하게 된다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/b3b7c8f3-e754-4a07-9c51-f57ea588b92e/Untitled.png)
    
    World는 GameMode에게 → GameMode는 GameState에게 → GameState는 모든 Actor에게 BeginPlay를 알리고, LoadMap() 의 처리가 끝나 GameLoop에 포함된다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/3a1b2268-c668-4acd-b28c-94869bdca84b/Untitled.png)
    
    $\sixptsize\color{666666}{정말~힘들었다..~그래도~오랜~시간을~들인~만큼~흐름~자체는~제대로~파악한~듯~하다..}$
    

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/398364ad-d361-4afc-a422-c2288c3d298c/6a32c9ba-5ad4-4eb3-bc3d-36ebf9901976/Untitled.png)

~~잠을 안자고 장시간 작성한 글이라서 형식이 고르지 못한 점 양해 부탁 드립니다.~~  ~~😂😂~~

**내용 관련해서 피드백 또는 질문은 환영합니다. 가독성이 좋지 못해 수정했으면 하는 부분이나, 자세한 설명이 필요하시면 언제든 말씀해주세요!**