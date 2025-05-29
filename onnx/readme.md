# ONNX
### ONNX는 Open Neural Network Exchange의 줄인 말로서 이름과 같이 다른 DNN 프레임워크 환경(ex Tensorflow, PyTorch, etc..)에서 만들어진 모델들을 서로 호환되게 사용할 수 있도록 만들어진 공유 플랫폼이다.
#### https://beeny-ds.tistory.com/entry/%EC%86%8C%EA%B0%9C-ONNX-%EB%9E%80
- “한 프레임워크에서 학습한 모델을 다른 곳에서 사용할 수 있도록 하는 공통 언어(포맷)”
### 🛠 왜 필요할까?
#### 기존에는 이렇게 불편했어요:
- PyTorch에서 모델 학습 → TensorRT에서 사용 불가
- TensorFlow 모델을 Web에서 실행하려면 구조 변경 필요
#### 그래서 등장한 게 ONNX:
- 학습은 어디서든 → 저장은 ONNX → 실행은 어디서든
### <br/>

### 📦 ONNX는 무엇을 저장하나?
| 구성 요소         | 설명                   |
| ------------- | -------------------- |
| **모델 구조**     | 연산자(operator) 계산 그래프 |
| **모델 파라미터**   | weight, bias 등       |
| **입출력 텐서 정의** | 어떤 입력을 받고 어떤 출력을 내는지 |
#### ✔️ .onnx 파일 하나에 모든 정보 포함됨 → "포터블 모델"
### <br/>

### 🎮 어디서 쓸 수 있나?
#### ONNX로 변환된 모델은 아래처럼 다양하게 활용됩니다:
| 환경      | 실행 엔진                         |
| ------- | ----------------------------- |
| 서버 추론   | **ONNX Runtime**, TensorRT    |
| 엣지 디바이스 | Jetson, OpenVINO              |
| 모바일     | CoreML (Apple), Android NNAPI |
| 웹       | WebDNN, ONNX.js 등             |
### <br/>

### ⚙️ opset이란?
- ONNX 연산자의 정의 버전 (예: Add, MatMul, Gelu 등)
- 버전이 높을수록 최신 연산자 사용 가능
- 예: scaled_dot_product_attention은 opset 14부터 가능

### <br/><br/><br/>

## 모델 변환
### 필요한 패키지
```
pip install transformers[onnx]
```
### <br/>

### 이 명령어는 PyTorch 기반의 사전학습 모델을 ONNX 형식으로 변환하는 작업이다.  
### ONNX의 `opset`은 연산자(operator) 정의의 버전을 나타내며,  
### 버전이 높을수록 최신 연산 기능이 지원되고, 성능 및 호환성이 개선된다.  
### 이 변환 과정은 `scripting` 방식이 아닌 `tracing` 방식을 사용하며,  
### 실제 실행한 연산 경로를 기준으로 ONNX 계산 그래프를 생성한다.
```
python -m transformers.onnx --model=sionic-ai/nllb-200-ko-gec-3.3B --opset=17 /root/.cache/huggingface/hub/models--sionic-ai--nllb-200-ko-gec-3.3B/onnx
```
