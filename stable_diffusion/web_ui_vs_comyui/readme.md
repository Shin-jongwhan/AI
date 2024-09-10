# webui 이용 시도...
### 여러가지 계속 시도를 해봤는데, 분명 작업 관리자 - GPU에서 cuda를 보면 100%로 사용해서 이미지를 생성하는데 매우매우 느리다.
#### step 20인 이미지 한 장 생성하는데 6분, 길면 10분 걸림
### <br/>

### cuda, cudnn, torch, xformers 버전 바꾸고 checkpoint, lora, VAE 등의 모델 문제인가 하고도 바꾸고, 로컬 python 문제인지 venv 쪽 문제인지인가 해서 python 쪽도 바꿔보고 여러가지 다 바꿔봤는데 결국 느렸다.
### UI와 img2img 등 부가 기능이 많다는 것의 장점은 참 많지만 너무 느려서 결국 다른 걸 쓰기로 결정하였다.
### <br/>

### comfyui를 써보니 step 20인 이미지 한 장 12초 걸렸다. 여러가지 삽질을 하면서 배운 것도 많았지만 지치는 건 어쩔 수 없나보다.
### <br/><br/><br/>

# comfyui vs webui
## 속도
### compyui 완승이다. webui는 너무 느리다. 최적화가 확실히 필요하다.
### 유튜브 등 여러 영상을 참고해봤지만 webui는 보통 편집해서 짧게 보여주는 경우가 많았다. 그런데 다른 영상을 보더라도 아주 좋은 GPU가 아닌 이상 webui는 몇 분 씩 걸리는 경우가 많았다. 
### comfyui는 유튜브에서 편집 안 하는 화면을 보여주는 경우가 많았다. 어떤 영상에서는 0.7초에 한 장씩 생성하는 것도 보았다. 이 케이스 같은 경우 lcm lora를 활용하여 step 수를 4~8 정도로 줄였다.
### 자세한 건 lcm lora를 참고
#### [블로그 - LCM-LoRA - 초고속 스테이블 디퓨전](https://www.internetmap.kr/entry/LCM-LoRA-very-fast-stable-diffusion)
#### [다운로드 링크](https://huggingface.co/latent-consistency)
### <br/>

### webui는 새로고침과 모델 불러오기도 느렸다. 모델 불러오는 데에만 짧으몇 몇 십초에서 수 분이 걸렸다.
### comfyui는 새로고침은 거의 실시간, 불러오는 것도 수 초 ~ 수십 초면 불러와졌다.
### <br/><br/>

## 패키지, python 버전
### webui가 훨씬 구버전이다. 240910 기준 python은 3.10.6이고, torch에서 cudnn은 8.x 버전을 쓴다.
### 물론 comfyui도 구버전인 게 있지만 수정하면 될 듯 하다. python version은 3.11, torch에서 cudnn은 9.x을 쓴다.
### <br/><br/>

## 기능
### webui가 기본적으로 제공되는 기능은 훨씬 많다.
### 다만 comfyui가 workflow 구성을 하고 customizing 하는 데에는 훨씬 좋다.
### comfyui는 노드로 연결해서 사용하기 때문에 어떠한 프로세스로 흘러가는지 한 눈에 볼 수 있다. stable diffusion의 workflow를 이해하는 데에도 큰 도움이 된다.
### 언리얼 비주얼 스크립팅을 사용해보았다면 익숙할 것이다.
### <br/><br/>

## 느낀 점
### webui는 너무 무겁다. 그리고 최적화가 확실히 안 되어 있다.
### comfyui는 반대로 가볍고 최적화가 잘 되어 있다고 느꼈다. 
### 앞으로 comfyui를 사용할 듯 하다.
