# Style Transfer Benchmarking

### 📄프로젝트 소개
---
&ensp;사진을 특정 스타일의 느낌으로 표현하는 **Style Transfer** 기술 탐구 및 다양한 Style Transfer 모델의 결과물 비교.
</br></br></br></br>

### 🧠개요
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="45%"/>
  <img src="https://github.com/user-attachments/assets/16f82010-876e-4fe2-b722-000dccec9f53" width="45%"/>
</p>
</br>

&ensp;AI의 발전을 날로 체감하는 요즘, 특히 눈길을 끈 건 위의 사진과 같이 **실제 인물을 특정 그림체로 그려주는 기술** 이었다. 자신의 사진을 지브리풍 그림체로 생성해서 프로필 사진으로 쓰는 게 유행이 될 만큼 인기를 끌었기에 호기심이 생길 수밖에 없었다. </br></br>
&ensp;[KSH Drawing AI](https://github.com/HLife15/CD_KSHD-AI) 프로젝트에서 텍스트 프롬프트를 바탕으로 내 그림체의 이미지를 생성해주는 프로그램을 제작한 적이 있었다. 그래서 이번엔 실제 인물 사진을 바탕으로 내 그림체의 이미지를 생성해주는 프로그램을 목적으로 직접 구현을 시도해보았지만 시작부터 쉽지 않았다.</br></br>
&ensp;지난 프로젝트에서는 Input은 이미지에 대한 설명(Text), Output은 설명에 맞는 이미지(Image)로 설정해 이들을 한 쌍으로 묶어 Text-to-Image 학습을 진행했었다. 그와 다르게 이번 작업은 일종의 Image-to-Image이므로 이론 상으로 Input은 실제 사람 이미지, Output은 Input 이미지를 특정 그림체로 그린 이미지로 설정해 학습을 진행해야 할 것이다. 하지만 Diffusers 라이브러리에 학습 코드가 제공되는 Text-to-Image와는 다르게 Image-to-Image는 마땅한 학습 방법이 제공되지 않았고, 무엇보다 학습에 쓰일 데이터셋을 구하는 것부터 사실상 불가능했다. 그렇게 방법을 고민하던 중, **Style Transfer**라는 기술에 대해 알게 되었고 내 목적에 부합하는 결과물을 생성해낼 수 있을 것 같아 자세히 알아보았다.
</br></br></br></br>

### 🖌️Style Transfer
---

<img width="1170" height="485" alt="Image" src="https://github.com/user-attachments/assets/5d3a66ad-fbec-4295-b239-00d19c2289ec" />
</br></br>

&ensp;**Style Transfer**는 **한 이미지의 ‘내용(무엇이 보이는가)’은 유지하면서 다른 이미지의 ‘스타일(색감, 질감, 붓터치 등 시각적 특성)’을 가져와 합성하는 기술**이다. 즉, 사진의 내용은 그대로 둔 채, 다른 그림의 화풍을 입히는 것이다. </br></br>
&ensp;Style Transfer는 크게 **내용 (content)** 과 **스타일 (Style)** 로 구성된다. 내용은 사진에서 **무엇**이 보이는 지에 관한 정보이다. 사람 사진을 예로 들었을 때 사람의 얼굴형, 배경의 건물 형태, 배경 나무의 종류와 위치 등이 내용에 해당된다. 스타일은 색감, 선 형태, 질감 등 **어떻게** 보이는가에 관한 정보다. 반 고흐풍, 지브리풍 등 특정 그림체가 스타일에 해당된다. Style Transfer는 이 둘을 분리해서 '내용은 A에서 따오고, 스타일은 B에서 따와 합친다'는 개념으로 설명이 가능하다. </br></br>
&ensp;Style Transfer에 대해 조사하면서 매우 다양한 모델들이 배포되어 있었다. 그래서 내 목표인 **'실제 인물을 내 그림체로 그려주는 것'** 에 가까운 모델이 있는 지 알아보기 위해 배포되어 있는 모델들을 비교해보고자 한다.
</br></br></br></br>

#### 1. InstantStyle
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="45%"/>
  <img src="https://github.com/user-attachments/assets/49ccb9a3-e2ac-493e-a875-0345d69302e8" width="45%"/>
</p>
</br>
설명 : 텍스트 및 참조 이미지를 입력으로 받아 스타일을 보존하면서 새로운 이미지를 생성할 수 있도록 설계된 프레임워크 </br>
배포일 : 2024.04.09.</br>
텍스트 프롬프트 : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 2. Flux.1-dev-IP-Adapter
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="45%"/>
  <img src="https://github.com/user-attachments/assets/393dd3e8-9d21-4637-9882-810c2657e85a" width="45%"/>
</p>
</br>
설명 : Text-to-Image 생성 모델인 FLUX.1-dev에 이미지를 참조로 해서 생성할 수 있는 기능을 추가한 프레임워크 </br>
배포일 : 2024.11.01.</br>
텍스트 프롬프트 : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 3. FlowEdit
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="45%"/>
  <img src="https://github.com/user-attachments/assets/53b2615c-0986-4e2e-b4a5-be0acc5959a8" width="45%"/>
</p>
</br>
설명 : 기존 모델들과 달리 이미지 역변환 등 추가 단계를 거치지 않고 작업을 수행할 수 있는 프레임워크 </br>
배포일 : 2024.12.11.</br>
텍스트 프롬프트 : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 4. OminiControl_Art
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="18%"/>
  <img src="https://github.com/user-attachments/assets/3af968b3-d363-48ca-8f1f-0bd01e4a4486" width="18%"/>
  <img src="https://github.com/user-attachments/assets/16528d23-925e-4085-88c1-33a9953ca22d" width="18%"/>
  <img src="https://github.com/user-attachments/assets/7d772161-262a-4be1-b0e9-15fe7bf6fefc" width="18%"/>
  <img src="https://github.com/user-attachments/assets/472ce276-2e89-46a9-83d6-490d38d6af17" width="18%"/>
</p>
</br>
설명 : OminiControl 프레임워크의 Art Version으로 입력된 이미지의 스타일을 4가지의 스타일 (Irasutoya Illustration, Snoopy, Studio Ghibli, The Simpsons) 중 선택한 옵션으로 변환해준다.  </br>
배포일 : 2025.04.09.</br>
</br></br>

#### 5. InstantCharacter
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="22%"/>
  <img src="https://github.com/user-attachments/assets/a36cca11-77ae-4a9e-9dc2-e2f5468f7a48" width="22%"/>
  <img src="https://github.com/user-attachments/assets/69f25d5c-e72b-4caa-aca4-05841ff3f5e0" width="22%"/>
  <img src="https://github.com/user-attachments/assets/0e31894b-b824-429d-9742-7580ebad5227" width="22%"/>
</p>
</br>
설명 : 단 한 장의 이미지로부터 인물을 인식하고 그 인물을 다른 포즈, 다른 스타일 (None, Makoto Shinkai, Ghibli), 다른 장면으로 만들어주는 생성 모델  </br>
배포일 : 2025.04.10.</br>
텍스트 프롬프트 : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 6. USO FLUX
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="30%"/>
  <img src="https://github.com/user-attachments/assets/6d59e9ec-d339-4fc1-abf5-00388337be71" width="30%"/>
  <img src="https://github.com/user-attachments/assets/4f782cc9-cea9-42f0-9354-f3c74c91ee92" width="30%"/>
</p>
</br>
설명 : 스타일과 주제 두 가지를 한 번에 제어할 수 있는 이미지 생성 모델. 인물(위 좌측 사진)을 유지하면서 스타일(위 중앙 사진)을 입혀주는 것.  </br>
배포일 : 2025.08.28.</br>
텍스트 프롬프트 : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>
