## Stable diffusion webui
### 여러가지 계속 시도를 해봤는데, 분명 작업 관리자 - GPU에서 cuda를 보면 100%로 사용해서 이미지를 생성하는데 매우매우 느리다.
#### step 20인 이미지 한 장 생성하는데 6분, 길면 10분 걸림
### <br/>

### cuda, cudnn, torch, xformers 버전 바꾸고 checkpoint, lora, VAE 등의 모델 문제인가 하고도 바꾸고, 로컬 python 문제인지 venv 쪽 문제인지인가 해서 python 쪽도 바꿔보고 여러가지 다 바꿔봤는데 결국 느렸다.
### UI와 img2img 등 부가 기능이 많다는 것의 장점은 참 많지만 너무 느려서 결국 다른 걸 쓰기로 결정하였다.
### <br/>

### comfyui를 써보니 step 20인 이미지 한 장 12초 걸렸다. 여러가지 삽질을 하면서 배운 것도 많았지만 지치는 건 어쩔 수 없나보다.
### <br/><br/><br/>

## comfyui vs webui
### 속도
### compyui 완승이다. webui는 너무 느리다. 최적화가 확실히 필요하다.
### <br/>

### 패키지, python 버전
### webui가 훨씬 구버전이다. 240910 기준 python은 3.10.6이고, torch에서 cudnn은 8.x 버전을 쓴다.
### 물론 comfyui도 구버전인 게 있지만 수정하면 될 듯 하다. python version은 3.11, torch에서 cudnn은 9.x을 쓴다.
### <br/>

