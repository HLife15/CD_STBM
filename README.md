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
&ensp;Style Transfer에 대해 조사하면서 매우 다양한 모델들이 배포되어 있었다. 그래서 내 목표인 **'실제 인물을 내 그림체로 그려주는 것'** 에 가까운 모델이 있는 지 알아보기 위해 배포되어 있는 모델들을 비교해보고자 한다.</br></br>
&ensp;(사진 배치 순서는 **내용 이미지 - 스타일 이미지 (필요 시) - 결과물 (모델이 여러 개일 경우 모델 표기 순서대로 배치)** 이다)
</br></br></br></br>

#### 1. [InstantStyle](https://huggingface.co/spaces/InstantX/InstantStyle)
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="45%"/>
  <img src="https://github.com/user-attachments/assets/49ccb9a3-e2ac-493e-a875-0345d69302e8" width="45%"/>
</p>
</br>

**설명** : 텍스트 및 참조 이미지를 입력으로 받아 스타일을 보존하면서 새로운 이미지를 생성할 수 있도록 설계된 프레임워크 </br>
**배포일** : 2024.04.09.</br>
**텍스트 프롬프트** : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 2. [Flux.1-dev-IP-Adapter](https://huggingface.co/spaces/InstantX/flux-IP-adapter)
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="45%"/>
  <img src="https://github.com/user-attachments/assets/393dd3e8-9d21-4637-9882-810c2657e85a" width="45%"/>
</p>
</br>

**설명** : Text-to-Image 생성 모델인 FLUX.1-dev에 이미지를 참조로 해서 생성할 수 있는 기능을 추가한 프레임워크 </br>
**배포일** : 2024.11.01.</br>
**텍스트 프롬프트** : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 3. [FlowEdit](https://huggingface.co/spaces/fallenshock/FlowEdit)
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="45%"/>
  <img src="https://github.com/user-attachments/assets/53b2615c-0986-4e2e-b4a5-be0acc5959a8" width="45%"/>
</p>
</br>

**설명** : 기존 모델들과 달리 이미지 역변환 등 추가 단계를 거치지 않고 작업을 수행할 수 있는 프레임워크 </br>
**배포일** : 2024.12.11.</br>
**텍스트 프롬프트** : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 4. [OminiControl_Art](https://huggingface.co/spaces/Yuanshi/OminiControl_Art)
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="18%"/>
  <img src="https://github.com/user-attachments/assets/3af968b3-d363-48ca-8f1f-0bd01e4a4486" width="18%"/>
  <img src="https://github.com/user-attachments/assets/16528d23-925e-4085-88c1-33a9953ca22d" width="18%"/>
  <img src="https://github.com/user-attachments/assets/7d772161-262a-4be1-b0e9-15fe7bf6fefc" width="18%"/>
  <img src="https://github.com/user-attachments/assets/472ce276-2e89-46a9-83d6-490d38d6af17" width="18%"/>
</p>
</br>

**설명** : OminiControl 프레임워크의 Art Version으로 입력된 이미지의 스타일을 4가지의 스타일 (Irasutoya Illustration, Snoopy, Studio Ghibli, The Simpsons) 중 선택한 옵션으로 변환해준다.  </br>
**배포일** : 2025.04.09.</br>
</br></br>

#### 5. [InstantCharacter](https://huggingface.co/spaces/InstantX/InstantCharacter)
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="22%"/>
  <img src="https://github.com/user-attachments/assets/a36cca11-77ae-4a9e-9dc2-e2f5468f7a48" width="22%"/>
  <img src="https://github.com/user-attachments/assets/69f25d5c-e72b-4caa-aca4-05841ff3f5e0" width="22%"/>
  <img src="https://github.com/user-attachments/assets/0e31894b-b824-429d-9742-7580ebad5227" width="22%"/>
</p>
</br>

**설명** : 단 한 장의 이미지로부터 인물을 인식하고 그 인물을 다른 포즈, 다른 스타일 (None, Makoto Shinkai, Ghibli), 다른 장면으로 만들어주는 생성 모델  </br>
**배포일** : 2025.04.10.</br>
**텍스트 프롬프트** : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 6. [USO FLUX](https://huggingface.co/spaces/bytedance-research/USO)
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="30%"/>
  <img src="https://github.com/user-attachments/assets/6d59e9ec-d339-4fc1-abf5-00388337be71" width="30%"/>
  <img src="https://github.com/user-attachments/assets/4f782cc9-cea9-42f0-9354-f3c74c91ee92" width="30%"/>
</p>
</br>

**설명** : 스타일과 주제 두 가지를 한 번에 제어할 수 있는 이미지 생성 모델. 인물(위 좌측 사진)을 유지하면서 스타일(위 중앙 사진)을 입혀주는 것.  </br>
**배포일** : 2025.08.28.</br>
**텍스트 프롬프트** : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 7. [Omni Zero](https://huggingface.co/spaces/okaris/omni-zero)
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="30%"/>
  <img src="https://github.com/user-attachments/assets/6d59e9ec-d339-4fc1-abf5-00388337be71" width="30%"/>
  <img src="https://github.com/user-attachments/assets/4175dfcf-cb59-4a72-9c65-04bda48b4a12" width="30%"/>
</p>
</br>

**설명** : 얼굴 또는 인물 중심의 이미지 (위 좌측 사진)를 즉시 스타일(위 중앙 사진)화 할 수 있는 파이프라인  </br>
**배포일** : 2024.05.11.</br>
**텍스트 프롬프트** : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 8. [DreamO](https://huggingface.co/spaces/okaris/omni-zero)
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="30%"/>
  <img src="https://github.com/user-attachments/assets/6d59e9ec-d339-4fc1-abf5-00388337be71" width="30%"/>
  <img src="https://github.com/user-attachments/assets/448fcc2b-2517-4c72-b1d7-8624e3dccfaf" width="30%"/>
</p>
</br>

**설명** : 이미지 생성 및 편집 분야에서 스타일, 주제, 아이덴티티, 의상 등 다양한 조건을 한 번에 통합해서 제어할 수 있는 프레임워크  </br>
**배포일** : 2025.05.08.</br>
**텍스트 프롬프트** : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 9. Neural Style Transfer  ([georgescutelnicu](https://huggingface.co/spaces/georgescutelnicu/neural-style-transfer) / [astro189](https://huggingface.co/spaces/astro189/Neural_Style_Transfer) / [optimization-hashira](https://huggingface.co/spaces/optimization-hashira/neural-style-transfer))
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="18%"/>
  <img src="https://github.com/user-attachments/assets/6d59e9ec-d339-4fc1-abf5-00388337be71" width="18%"/>
  <img src="https://github.com/user-attachments/assets/1bc7c40e-70ad-4304-b735-ea0e57579470" width="18%"/>
  <img src="https://github.com/user-attachments/assets/ad86f715-4a02-41eb-bdab-ef2becdc6280" width="18%"/>
  <img src="https://github.com/user-attachments/assets/fe16b17f-39bf-4971-87db-3d8dc80573f2" width="18%"/>
</p>
</br>

**설명** : 한 이미지의 '스타일 (위 좌측 두번째 사진)'을 다른 이미지의 '내용 (위 좌측 사진)'에 입히는 기술 </br>
**배포일** : 2023.01.14. (georgescutelnicu), 2023.05.16. (astro189), 2024.10.16. (optimization-hashira)</br>
</br></br>

#### 10. [Simple Style Transfer](https://huggingface.co/spaces/1plus1/Simple-Style_Transfer)
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="30%"/>
  <img src="https://github.com/user-attachments/assets/6d59e9ec-d339-4fc1-abf5-00388337be71" width="30%"/>
  <img src="https://github.com/user-attachments/assets/a0167a7b-80e4-4c5b-b5a4-add0ef1aa0ce" width="30%"/>
</p>
</br>

**설명** : 두 이미지를 입력 받아 내용 이미지 (위 좌측 사진)의 구조, 형태는 유지하면서 스타일 이미지 (위 중앙 사진)의 색감, 질감, 붓 터치 분위기 등을 입히는 Style Transfer 도구  </br>
**배포일** : 2024.12.25.</br>
**텍스트 프롬프트** : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 11. Image Style Transfer ([01devManish](https://huggingface.co/spaces/01devManish/Image-Style-Transfer) / [TungDuong](https://huggingface.co/spaces/TungDuong/image_style_transfer))
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="22%"/>
  <img src="https://github.com/user-attachments/assets/6d59e9ec-d339-4fc1-abf5-00388337be71" width="22%"/>
  <img src="https://github.com/user-attachments/assets/ccd7eb38-b454-4776-a6d2-c684e068d9ee" width="22%"/>
  <img src="https://github.com/user-attachments/assets/a95673ce-57c3-47bd-ad06-a2b0187567da" width="22%"/>
</p>
</br>

**설명** : 내용 이미지(위 좌측 이미지) 와 스타일 이미지(위 좌측 두 번째 이미지) 두 장을 입력하면, 내용 이미지는 유지하면서 스타일 이미지의 색감·질감·붓터치 같은 느낌을 적용한 새로운 이미지를 만들어주는 style transfer 도구  </br>
**배포일** : 2022.03.03. (01devManish), 2025.01.07. (TungDuong) </br>
</br></br>

#### 12. [StyleFusion](https://huggingface.co/spaces/escapist413/StyleFusion)
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="45%"/>
  <img src="https://github.com/user-attachments/assets/af695260-6ff0-4d73-9c57-57d3ca357110" width="45%"/>
</p>
</br>

**설명** : 여러 스타일 특징의 융합(fusion)”을 수행하는 Style Transfer 시스템 </br>
**배포일** : 2024.01.08.</br>
</br></br>

#### 13. [Diffusion Style Transfer](https://huggingface.co/spaces/GandalfTheBlack/DiffusionStyleTransfer)
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="30%"/>
  <img src="https://github.com/user-attachments/assets/6d59e9ec-d339-4fc1-abf5-00388337be71" width="30%"/>
  <img src="https://github.com/user-attachments/assets/f21d0717-e8bb-4c61-bef4-b18aa1bac5da" width="30%"/>
</p>
</br>

**설명** : 스테이블 디퓨전(Stable Diffusion) 또는 유사한 디퓨전 모델을 사용해서 스타일 이미지의 느낌을 내용 이미지에 입히는 Style Transfer 작업을 수행하는 도구  </br>
**배포일** : 2025.02.14.</br>
**텍스트 프롬프트** : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 14. [Kolors IP-Adapter](https://huggingface.co/spaces/multimodalart/Kolors-IPAdapter)
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="45%"/>
  <img src="https://github.com/user-attachments/assets/20e735ac-8e62-4dad-b05f-36d0bee047bd" width="45%"/>
</p>
</br>

**설명** : Kolors라는 Text-to-Image 생성 모델 기반으로 설계된 IP-Adapter(image-prompt adapter) 형태의 모듈로, 특히 “참조 이미지 + 텍스트 프롬프트” 조합을 받아서 참조 이미지의 시각적 특성을 생성에 반영할 수 있도록 설계된 모델 </br>
**배포일** : 2024.07.12.</br>
**텍스트 프롬프트** : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br>

#### 15. [FLUX Style Shaping](https://huggingface.co/spaces/multimodalart/flux-style-shaping)
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="30%"/>
  <img src="https://github.com/user-attachments/assets/6d59e9ec-d339-4fc1-abf5-00388337be71" width="30%"/>
  <img src="https://github.com/user-attachments/assets/2ad947ce-d521-419d-8ada-8e9ddf16bee6" width="30%"/>
</p>
</br>

**설명** : 스타일 이미지 (위 중앙 이미지)와 텍스트 프롬프트를 바탕으로 내용 이미지 (위 좌측 이미지)의 스타일, 질감, 분위기를 조정하거나 바꾸는 데 초점을 맞추고 있는 모델  </br>
**배포일** : 2024.12.03.</br>
**텍스트 프롬프트** : A bespectacled boy smiling with a wine glass on the ocean view veranda
</br></br></br></br>

### ✅결론
---
&ensp;이번 프로젝트의 목적은 **실제 인물의 사진을 특정 그림체로 변환하는 데 가장 효과적인 Style Transfer 모델을 탐색**하는 것이다. 이를 위해 Hugging Face Spaces에 공개된 여러 Style Transfer 관련 모델을 벤치마킹하고, 그 결과물을 비교·분석하였다. 그럴듯한 결과물을 생성된 경우도 있었지만, 예상과 달리 아쉬운 퀄리티의 결과물도 다수 생성되었다.

&ensp;분석 결과, **FLUX Style Shaping**을 통해 프로젝트의 목적에 가장 부합하는 결과물을 생성할 수 있었다.
</br></br>
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="30%"/>
  <img src="https://github.com/user-attachments/assets/1d20369f-f27d-4305-b6bb-2bab5f05b2c7" width="30%"/>
  <img src="https://github.com/user-attachments/assets/53dc5e2c-8114-492b-9243-0c744de3d285" width="30%"/>
</p>
</br></br>

&ensp;물론 이 모델도 실제 인물을 특정 그림체로 그려냈다고 보기엔 애매하다. 정확하게 설명하자면, 내용 이미지의 인물의 자세와 비중이 큰 사물들을 인식해 이를 스타일 이미지의 그림체와 색감으로 그려낸 것이다. 하지만 그럼에도 이 모델이 다른 모델들과의 큰 차이점이 있다면 바로 **스타일 이미지의 특징을 거의 완벽하게 반영한 결과물을 생성**해냈다는 것이다. 대부분의 다른 모델들은 스타일 이미지의 특징을 거의 반영하지 못했지만, 이 모델은 마치 스타일 이미지의 캐릭터가 내용 이미지 인물의 포즈를 따라하는 듯한 이미지를 생성해냈다.</br></br>
&ensp;향후 이 FLUX Style Shaping 모델의 생성 원리를 분석하다 보면 내가 구현하지 못했던 '실제 인물 사진을 내 그림체로 변환'해주는 모델을 만들 수 있을 것이라 기대한다. 

