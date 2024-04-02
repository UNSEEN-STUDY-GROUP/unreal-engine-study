# Custom Sky Sphere : 언리얼에서 예쁜 하늘을 만들어보자

상태: 진행 중

## 예쁜 하늘을 직접 만들어보자

### 0. Overview




기본적으로 레벨 생성 시 포함되는 하늘 구성

![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%201.png)

### 1. 태양

- **Directional Light**

![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%202.png)

![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%203.png)

Unreal Engine에서 태양은 주로 Level의 Directional Light로 표현된다.

Directional Light의 각도를 조절하여 태양의 높이를 조절하거나,

크기, 강도를 조절하여 원하는 모습의 태양을 레벨에 배치할 수 있다. 

[언리얼 엔진의 디렉셔널 라이트](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/directional-lights-in-unreal-engine?application_version=5.3)

- **Sun And Sky Actor**
    
    Sun Position Calculator라는 Plugin을 사용하면 Sun And Sky Actor의 활용이 가능
    
    ![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%204.png)
    
    ![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%205.png)
    
    ![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%206.png)
    
    - 지리적 위치와 날짜를 기반으로 태양의 위치를 세밀하게 조정이 가능하다.
    
    [Geographically Accurate Sun Positioning Tool in Unreal Engine](https://dev.epicgames.com/documentation/en-us/unreal-engine/geographically-accurate-sun-positioning-tool-in-unreal-engine?application_version=5.3)
    

### 2. 하늘

- **Sky Sphere**
    
    
    Level에서 사용하는 Landscape(지형)을 감싸고 있는 구형 하늘
    
    ![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%207.png)
    
    이 Sky Sphere는 BP의 형태로 다양한 변수들을 포함하고 있다. 
    
    이 Sky Sphere에 Material을 적용시키는 형태로 하늘의 모습을 변경할 수 있다. 
    
    ![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%208.png)
    
    해당 사진을 통해 Material 생성 후 
    
    ![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%209.png)
    
    Sky Sphere에 적용 시키는 형태
    
    ![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%2010.png)
    
    변경하여 적용한 하늘
    

### 3. 안개 (빛의 산란효과)

[Exponential Height Fog User Guide](https://docs.unrealengine.com/4.27/en-US/BuildingWorlds/FogEffects/HeightFog/)

- Exponential Height Fog를 적용하지 않았을 때,
    
    ![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%2011.png)
    
- Exponential Height Fog를 Single Layer로 적용했을 때,
    
    ![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%2012.png)
    
- Exponentuial Height Fog를 2개 Layer로 구성했을 때,
    
    ![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%2013.png)
    

이처럼 Exponential Height  Fog를 이용해 안개를 구현할 수 있다. 

또한, Exponential Height Fog를 통해 빛의 산란에 영향을 준다.

![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%2014.png)

![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%2015.png)

### 4. 구름

- Volumetric Clouds

움직이는 구름의 구현이 가능하고, 디테일한 구름 모양의 조정이 가능하다.

![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%2016.png)

특히, 마우스를 통해 페인팅을 하듯이 구름을 만들고 지우는 기능을 제공한다.

[Volumetric Clouds](https://docs.unrealengine.com/4.26/en-US/BuildingWorlds/LightingAndShadows/VolumetricClouds/)

### 5. 대기

- Sky Atmosphere
    
    ![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%2017.png)
    
    Unreal에서 대기를 나타내는 컴포넌트 
    
    빛의 산란에 대한 물리적 특성 구현에 가장 큰 역할을 차지
    
    ![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%2018.png)
    
    Directional Light가 지면과 얼마나 평행하게 있는지에 따라,
    
    어떻게 산란되며 어떤 질감을 가질 것인지 설정을 통해 대기 특성을 표현이 가능   
    
    ![Untitled](Custom%20Sky%20Sphere%20%E1%84%8B%E1%85%A5%E1%86%AB%E1%84%85%E1%85%B5%E1%84%8B%E1%85%A5%E1%86%AF%E1%84%8B%E1%85%A6%E1%84%89%E1%85%A5%20%E1%84%8B%E1%85%A8%E1%84%88%E1%85%B3%E1%86%AB%20%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%201f5d9d676cd64bea83636d92e1cae341/Untitled%2019.png)
    
    Sky Atmosphere를 잘 이용하면 앞서 이야기 한 Exponential Height Fog를 사용하지 않고도 안개의 표현이 가능하다. 
    
    [Sky Atmosphere](https://docs.unrealengine.com/4.26/ko/BuildingWorlds/FogEffects/SkyAtmosphere/)
    
- 레퍼런스
    
    [UE4 : Sky Sphere 해 사이즈 조절하기 (SunDisk Size)](https://m.blog.naver.com/serbell_11/221950043600)
    
    언Directional Light 크기
    
    [How to create a CUSTOM SKY SPHERE | Community tutorial](https://dev.epicgames.com/community/learning/tutorials/Epme/unreal-engine-how-to-create-a-custom-sky-sphere?locale=ko-kr)
    
    Custom Sky Sphere
    
    [https://www.youtube.com/watch?v=8t97W8Ibq04](https://www.youtube.com/watch?v=8t97W8Ibq04)
    
    [Unreal에서 사실적인 하늘 구현하기](https://m.blog.naver.com/intenseardor/222058310795)
    
    [Unreal Engine 5) 하늘, (움직이는)구름 만들기 (Env. Light Mixer)](https://alpaca-code.tistory.com/155)
