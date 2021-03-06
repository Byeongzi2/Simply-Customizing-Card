# Simply Customizing Card
Future Finance A.I. Challenge 2020
### 주제 : 내 카드를 GAN단하게 커스터마이징하자
##### Idea : [Face2Malnyun](https://github.com/bryandlee/malnyun_faces)


## 1. 데이터 크롤링 ( 여신작가 / 복학왕 ) + 데이터 전처리 ( 얼굴만 따로 캡쳐 )
##### - 다양한 사람이면 좋음
##### - 이말년 그림체는 500장 / 다른 작가님들은 3 channel이여서 더 많이 필요할 수도 있음.
##### - 크기는 상관 없습니다 ( 어차피 다 256으로 Resize 하고 다시 64 or 128로 줄일 예정이지만 크면 클 수록 좋음 )
##### - 만일 웹툰에서 데이터 허락을 받지 못하면, 다른 데이터셋을 찾아봐야 할지도.. ex) [Simpson1](https://www.kaggle.com/alexattia/the-simpsons-characters-dataset), [Simpson2](https://www.kaggle.com/kostastokis/simpsons-faces), [Anime](https://www.kaggle.com/splcher/animefacedataset), [Cartoon](https://google.github.io/cartoonset/), [Selfie2Anime](https://www.kaggle.com/arnaud58/selfie2anime)

## 2. 모델구현 
##### - 모델 1 : StyleGAN v2 < FFHQ dataset Pre-trained Model >
##### - 모델 2 : U-GAT-IT


## 3. PPT작성
##### - 작년 수상자(https://github.com/ukiKwon/voice-separater-with-ripTracking)
##### - PPT를 어떻게 만들것인지 구상

------------------------------

<details>
<summary> 
  Part 1. Pre-trained Model  
  </summary>
<div markdown="1">


## 2020.08.16 Review

1. 데이터셋 정리 ( 심슨 데이터셋 / 웹툰 데이터셋 문의 )
2. 모델에 대한 이해
3. GPU서버 대여 ( Ubuntu / GPU 15TFLOPS / Keras, PyTorch / Python)

## 2020.08.17 Review

1. StyleGAN / U-GAT-IT 구현완료 ( 파라미터 튜닝은 조금 살펴봐야됨, 모델 이해 X, FFHQ Pre-trained 모델 확인 )

|Model|Paper|Code|
|:---|:---|:---|
|   U-GAT-IT   | [Paper](https://arxiv.org/pdf/1907.10830.pdf) | [Code](https://github.com/znxlwm/UGATIT-pytorch) |
|   StyleGAN   | [Paper](https://arxiv.org/pdf/1812.04948.pdf) | [Code](https://github.com/rosinality/stylegan2-pytorch) |
| StyleTransfer| [Paper](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Gatys_Image_Style_Transfer_CVPR_2016_paper.pdf) | [Code](https://pytorch.org/tutorials/advanced/neural_style_tutorial.html) |
| WBCartoonization| [Paper](https://github.com/SystemErrorWang/White-box-Cartoonization/blob/master/paper/06791.pdf) | [Code](https://github.com/SystemErrorWang/White-box-Cartoonization) |

2. FreezeD / ADA 논문 리뷰

|Method|Paper|Review|
|:---|:---|:---|
| FreezeD(FreezeDiscriminator) | [Paper](https://arxiv.org/pdf/2002.10964.pdf) | Pretrained된 Discriminator의 low-layer을 Freeze시키고 High-layer만 학습시키는 방법 |
| ADA(AdaptiveDataAugmentation) | [Paper](https://research.nvidia.com/sites/default/files/pubs/2020-06_Training-Generative-Adversarial/karras2020-limited-data.pdf )| 5가지 방법으로 Adaptive하게 Data Augmentation하는 방법 논문 p.19-20 참조 |


## 2020.08.18 Review

1. 데이터 수집
2. Docker 시스템 구축


## 2020.08.19 Review

1. U-GAT-IT 모델구조 파악
- Local Discriminator (n_layer = 5) 와 Global Discriminator (n_layer = 7) 으로 (2개의 Discriminator + 1개의 Generator) x 2
- Loss : Adverserial Loss (MSE) + Cycle Loss (MAE) + Identity Loss (MAE) + CAM Loss(Class Activation Map) (BCE) (+ VGG Loss)
- Discriminator에 Spectral Normalization 사용
- CAM (Class Activation Map) : class classification에 영향을 주는 feature map / global adaptive maxpool + global adaptive avgpool
- AdaLIN (Adaptive Layer Instance Normalization) : Instance (channel-wise), Layer (layer-wise) 을 adaptive하게 normalization

2. Dataset 추가
- Selfie2Anime


## 2020.08.20 Review

1. github 세팅 / docker 생성
### -------해야할일-------
2. (지헌)     파이프라인 구성
3. (주성/병지) GAN구조 파악 / DCGAN / ADA / PyTorch tutorial


## 2020.08.21 Review

1. Main 파이프라인 구현 완료
2. 학습중 ( face dataset : [face](https://github.com/JingchunCheng/All-Age-Faces-Dataset)에서 3500개 추출해서 사용중)
3. 하이퍼파라피터 조정중


## 2020.08.22 Review

1. 학습중 - 120K후에 Linear하게 lr->0으로 감소 ( 실수한거 발견 - 210K후에 실험 중단)


## 2020.08.23 Review

1. 학습중
2. Selfie2Anime 데이터셋으로 Pretrained한 모델 발견 ( FreezeD 이용해서 Transfer Learning 해볼 예정 )

## 2020.08.24 Review

1. Dataset 의 문제인지 학습이 잘 안됨
- [ ] Transfer Learning ( FreezeD )
- [ ] Dataset ( Selfie2Anime )
- [ ] ADA 추가


## 2020.08.25 Review

1. 데이터셋 변화( Selfie2Anime ), 모델 감량( 6res -> 4res ), 전처리 추가 -> 학습 잘됨.


## 2020.08.26 Review

1. 이미지 사이즈 증가 -> 96에서 192
2. Cycle Coefficieint, Identity Coefficieint (10,10) -> (15,15)
3. 주성님 서버 받으면 transfer learning 해볼 예정
4. 병지님 인물데이터 받으며 웹툰데이터셋 구성

</div>
</details>


------------------------------
------------------------------

<details>
<summary> 
  Part 2. Parameter Customizing  
  </summary>
<div markdown="1">

## 2020.08.31 Review

1. Transfer Learning 시작 (webtoon dataset) Cycle, Identity -> (10,10), lr = 0.00001
2. AIHub에서 받은 인물 데이터셋은 잠시 기각

## 2020.09.01 Review

1. VGG Loss, TV Loss 추가
2. Transfer / fine-tuning 시작
3. 연놈 데이터 추가.

## 2020.09.06 Review

1. VGG Loss 삭제 -> 이유는 모르겠지만 CAM Loss랑 충돌이 일어나는 것 같음
2. CAM Loss coefficient 2000을 증가
3. Transfer보다 fine-tuning이 더 성능이 잘나오는 거 같음
4. TV Loss : 15, lr = 0.0002

## 2020.09.07 Review
1. White-box-Cartoonization 모델 추가
2. White-box-Cartoonization 구조 파악 및 정리
3. pretrain된 모델 데이터로 실행이 잘됨. transfer를 통해 추가 학습 필요없다고 판단.

4. Server1 에서 여신강림 mode collapse발생 -> 재학습 (TV Loss : 20, CAM Loss : 1500, lr = 0.0001)
5. Server2 에서 연놈, 학습 잘 되던 중 80epoch 넘어가면서 overfitting 발생

## 2020.09.10 Review
1. 이미지사이즈 256, light=False로 YSGR/YN 두 개 학습.
2. YSGR이미지가 256이고 YN이 192라 YN은 잘 안됨.
3. 서버2에 이미지 사이즈 128, light=False, res_block = 6으로 설정하고 다시 학습중

</div>
</details>


------------------------------

# 최총본
1. U_GAT_IT + TV Loss (여신강림)
2. U_GAT_IT + TV Loss (연놈)
3. White-Box-Cartoonization (Pretrained)
4. Style Transfer

