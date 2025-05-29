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
### <br/>

### 테스트
```
import onnxruntime as ort
import numpy as np
from transformers import AutoTokenizer
import time

print("🚀 실행 디바이스:", ort.get_device())  # "GPU" 출력되면 성공

# 모델 및 토크나이저 경로
model_path = "/root/.cache/huggingface/hub/models--sionic-ai--nllb-200-ko-gec-3.3B/onnx/model.onnx"
tokenizer_path = "/root/.cache/huggingface/hub/models--sionic-ai--nllb-200-ko-gec-3.3B/snapshots/413b34e43ffe8c7c5c431d9eb843e7102b0c994d"

# 입력 텍스트
input_text = "나는 어제 도서관에 갔습니다 그리고 책을 읽었다"

# 🔹 1. 토크나이저 로드
start = time.time()
tokenizer = AutoTokenizer.from_pretrained(tokenizer_path)
print(f"⏱️ Tokenizer 로드 시간: {time.time() - start:.2f}초")

# 🔹 2. 입력 텍스트 토크나이즈
start = time.time()
inputs = tokenizer(input_text, return_tensors="np")
print(f"⏱️ 토크나이징 시간: {time.time() - start:.2f}초")

# 🔹 3. ONNX 세션 초기화
start = time.time()
# 초기화 성능 분석 가능 + 멀티스레딩 조절
so = ort.SessionOptions()
so.enable_profiling = True
so.intra_op_num_threads = 4  # CPU 연산 병렬 수 조절
# 세션 초기화
session = ort.InferenceSession(model_path, sess_options=so, providers=["CUDAExecutionProvider"])
print(f"⏱️ ONNX 세션 초기화 시간: {time.time() - start:.2f}초")

# 🔹 4. 추론
start = time.time()
onnx_inputs = {k: v for k, v in inputs.items()}
outputs = session.run(None, onnx_inputs)
print(f"⏱️ ONNX 추론 시간: {time.time() - start:.2f}초")

# 🔹 5. 결과 디코딩
start = time.time()
output_ids = np.argmax(outputs[0], axis=-1)
decoded = tokenizer.batch_decode(output_ids, skip_special_tokens=True)
print(f"⏱️ 디코딩 시간: {time.time() - start:.2f}초")

# 출력
print("\n📝 원본 문장:", input_text)
print("✅ 교정된 문장:", decoded[0])
```
### <br/>

### 테스트 결과... torch model보다 모델 로딩 시간(90초)이 느리다. 다른 건 빠른데 모델 로딩 시간을 어떻게 해결하고 싶다.
- ⏱️ ONNX 세션 초기화 시간: 351.14초
