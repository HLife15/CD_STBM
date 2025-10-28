# Style Transfer Benchmarking

### 📄프로젝트 소개
---
&ensp;사진을 특정 스타일의 느낌으로 표현하는 Style Transfer 기술 탐구 및 다양한 Style Transfer 모델의 결과물 비교.
</br></br></br></br>

### 🧠개요
---
<p align="center">
  <img src="https://github.com/user-attachments/assets/0e2ff06a-4b20-4c50-9a4f-4fb6a09e5115" width="45%"/>
  <img src="https://github.com/user-attachments/assets/16f82010-876e-4fe2-b722-000dccec9f53" width="45%"/>
</p>
</br>
&ensp;AI의 발전을 날로 체감하는 요즘이다. 특히 눈길을 끈 건 위의 사진과 같이 **실제 인물을 특정 그림체로 그려주는 기술** 이었다. 자신의 사진을 지브리풍 그림체로 생성해서 프로필 사진으로 쓰는 게 유행이 될 만큼 인기를 끌었기에 호기심이 생길 수밖에 없었다. [KSH Drawing AI](https://github.com/HLife15/CD_KSHD-AI) 프로젝트에서 텍스트 프롬프트를 바탕으로 내 그림체의 이미지를 생성해주는 프로그램을 제작한 적이 있었다. 그래서 이번엔 실제 인물 사진을 바탕으로 내 그림체의 이미지를 생성해주는 프로그램을 목적으로 직접 구현을 시도해보았지만 시작부터 쉽지 않았다. 지난 프로젝트에서는 Input은 이미지에 대한 설명(Text), Output은 설명에 맞는 이미지(Image)로 설정해 이들을 한 쌍으로 묶어 Text-to-Image 학습을 진행했었다. 그와 다르게 이번 작업은 일종의 Image-to-Image이므로 이론 상으로 Input은 실제 사람 이미지, Output은 Input 이미지를 특정 그림체로 그린 이미지로 설정해 학습을 진행해야 할 것이다. 하지만 Diffusers 라이브러리에 학습 코드가 제공되는 Text-to-Image와는 다르게 Image-to-Image는 마땅한 학습 방법이 제공되지 않았고, 무엇보다 학습에 쓰일 데이터셋을 구하는 것부터 사실상 불가능했다. 그렇게 방법을 고민하던 중, **Style Transfer**라는 기술에 대해 알게 되었고 내 목적에 부합하는 결과물을 생성해낼 수 있을 것 같아 자세히 알아보았다..</br></br></br></br>
<img width="1170" height="485" alt="Image" src="https://github.com/user-attachments/assets/5d3a66ad-fbec-4295-b239-00d19c2289ec" />
</br>
&ensp;**Style Transfer**는 **한 이미지의 ‘내용(무엇이 보이는가)’은 유지하면서 다른 이미지의 ‘스타일(색감, 질감, 붓터치 등 시각적 특성)’을 가져와 합성하는 기술**이다.
