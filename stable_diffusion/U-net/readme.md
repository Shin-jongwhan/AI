### 240909
## 참고
- https://velog.io/@lighthouse97/UNet%EC%9D%98-%EC%9D%B4%ED%95%B4
### <br/><br/><br/>

## Image segmentation
### segmentation이라는 뜻은 분류(classification)와 유사하다. 분류는 한 이미지가 어떤 카테고리에 들어갈지를 맞추는 것이면, segmentation은 픽셀 단위(또는 픽셀 집합)로 분류를 한다. 좀 더 세세하게 분류를 하는 것이라 생각하면 된다.
### <br/><br/><br/>

## U-net
### U-net은 image segmentation을 하는 알고리즘이다.
### 'U-Net: Convolutional Networks for Biomedical Image Segmentation' 이라는 논문에서 제안한 구조로서 매우 적은 수의 학습 데이터로도 정확한 이미지 세그멘테이션 성능을 보여주었으며 ISBI 세포 추적 챌린지 2015에서 큰 점수 차이로 우승했다고 한다.
### <br/>

### 이런 식으로 U 자형을 그리면서 encoding - decoding을 해서 U-net이라고 부른다. 
#### 자세한 내용은 [참고](https://velog.io/@lighthouse97/UNet%EC%9D%98-%EC%9D%B4%ED%95%B4)
#### ![image](https://github.com/user-attachments/assets/99eb1dc9-9399-4199-86cf-0d99da36ece3)
