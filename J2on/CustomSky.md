# Custom Sky Sphere : 언리얼에서 예쁜 하늘을 만들어보자

상태: 진행 중

## 예쁜 하늘을 직접 만들어보자

### 0. Overview

![Untitled](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/1a9add5c-1f84-4dfd-b4a0-16d734167e54)

기본적으로 레벨 생성 시 포함되는 하늘 구성

![Untitled 1](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/925d74a0-eb06-43b5-9976-9525eb61446d)


### 1. 태양

- **Directional Light**

![Untitled 2](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/fc58eeed-7807-4c58-a809-c972c3815e12)


![Untitled 3](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/e1621da2-436d-4874-89aa-ba1f5cc269fc)


Unreal Engine에서 태양은 주로 Level의 Directional Light로 표현된다.

Directional Light의 각도를 조절하여 태양의 높이를 조절하거나,

크기, 강도를 조절하여 원하는 모습의 태양을 레벨에 배치할 수 있다. 

[언리얼 엔진의 디렉셔널 라이트](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/directional-lights-in-unreal-engine?application_version=5.3)

- **Sun And Sky Actor**
    
    Sun Position Calculator라는 Plugin을 사용하면 Sun And Sky Actor의 활용이 가능
    
![Untitled 4](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/168a12f7-82c3-48b6-8e2c-0553c94e81f5)

![Untitled 5](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/ccf8f0cd-1194-4796-9e6c-2aa20793c7e6)
![Untitled 6](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/3a00d0b4-e5ae-4668-97cb-c0af44042a5b)

    
    - 지리적 위치와 날짜를 기반으로 태양의 위치를 세밀하게 조정이 가능하다.
    
    [Geographically Accurate Sun Positioning Tool in Unreal Engine](https://dev.epicgames.com/documentation/en-us/unreal-engine/geographically-accurate-sun-positioning-tool-in-unreal-engine?application_version=5.3)
    

### 2. 하늘

- **Sky Sphere**
    
    
    Level에서 사용하는 Landscape(지형)을 감싸고 있는 구형 하늘
    
    ![Untitled 7](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/bbdd661a-9451-4538-876c-50edaf13bf7f)

    이 Sky Sphere는 BP의 형태로 다양한 변수들을 포함하고 있다. 
    
    이 Sky Sphere에 Material을 적용시키는 형태로 하늘의 모습을 변경할 수 있다. 
    
    ![Untitled 8](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/a0855dc4-b5c8-46f8-b483-88a8b7100be3)

    
    해당 사진을 통해 Material 생성 후 
    
    ![Untitled 9](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/cc9b6011-025b-41ab-8c44-31636085b757)

    
    Sky Sphere에 적용 시키는 형태
    
    ![Untitled 10](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/73255a46-f02a-4895-9dd5-c54178c03fe3)

    
    변경하여 적용한 하늘
    

### 3. 안개 (빛의 산란효과)

[Exponential Height Fog User Guide](https://docs.unrealengine.com/4.27/en-US/BuildingWorlds/FogEffects/HeightFog/)

- Exponential Height Fog를 적용하지 않았을 때,
    
    ![Untitled 11](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/95f15548-df37-4b58-a2c6-8005c485c08b)

    
- Exponential Height Fog를 Single Layer로 적용했을 때,
    
    ![Untitled 12](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/6d25390f-859a-49ad-bdf3-a8fcb7cf9d6b)

    
- Exponentuial Height Fog를 2개 Layer로 구성했을 때,
    
    ![Untitled 13](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/a24b313c-33c8-44fa-a02b-94ee826583d2)

    

이처럼 Exponential Height  Fog를 이용해 안개를 구현할 수 있다. 

또한, Exponential Height Fog를 통해 빛의 산란에 영향을 준다.

![Untitled 14](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/b2b213a1-5d66-4f1e-99b0-75cc39e57264)


![Untitled 15](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/b449823c-28bb-46fc-af44-3bf3690eaaac)


### 4. 구름

- Volumetric Clouds

움직이는 구름의 구현이 가능하고, 디테일한 구름 모양의 조정이 가능하다.

![Untitled 16](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/fa98ca09-296f-4ad1-a3be-2e85ebca1935)


특히, 마우스를 통해 페인팅을 하듯이 구름을 만들고 지우는 기능을 제공한다.

[Volumetric Clouds](https://docs.unrealengine.com/4.26/en-US/BuildingWorlds/LightingAndShadows/VolumetricClouds/)

### 5. 대기

- Sky Atmosphere
    
    ![Untitled 17](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/f211a0f4-f32b-4ade-9e55-8d469106dedc)

    
    Unreal에서 대기를 나타내는 컴포넌트 
    
    빛의 산란에 대한 물리적 특성 구현에 가장 큰 역할을 차지
    
    ![Untitled 18](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/838be15a-3e7b-4b9a-bda1-676f10cbf463)

    
    Directional Light가 지면과 얼마나 평행하게 있는지에 따라,
    
    어떻게 산란되며 어떤 질감을 가질 것인지 설정을 통해 대기 특성을 표현이 가능   
    
    ![Untitled 19](https://github.com/UNSEEN-STUDY-GROUP/unreal-engine-study/assets/63008127/9a3f6687-0ea4-49a7-bf11-8c8eb7c8c0c0)

    
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
