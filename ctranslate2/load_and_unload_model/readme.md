### 250530
## 모델을 RAM, GPU memory 간 이동 방법
### fastapi로 모델을 미리 로드하기 위한 fastapi 코드
```
from fastapi import FastAPI, Body
from pydantic import BaseModel
import ctranslate2
from transformers import AutoTokenizer
import time

app = FastAPI()

# ----- 1. 모델 및 토크나이저 로드 -----
MODEL_PATH = "/root/.cache/huggingface/hub/models--sionic-ai--nllb-200-ko-gec-3.3B/snapshots/413b34e43ffe8c7c5c431d9eb843e7102b0c994d/ct2_model"
TOKENIZER_PATH = "/root/.cache/huggingface/hub/models--sionic-ai--nllb-200-ko-gec-3.3B/snapshots/413b34e43ffe8c7c5c431d9eb843e7102b0c994d"

print("🔄 모델 및 토크나이저 로딩 중...")
tokenizer_load_start = time.time()
tokenizer = AutoTokenizer.from_pretrained(TOKENIZER_PATH)
tokenizer_load_time = time.time() - tokenizer_load_start

translator_load_start = time.time()
translator = ctranslate2.Translator(MODEL_PATH, device="cuda")
translator_load_time = time.time() - translator_load_start

print(f"✅ 토크나이저 로드 시간: {tokenizer_load_time:.4f}초")
print(f"✅ CTranslate2 모델 로드 시간: {translator_load_time:.4f}초")

# ----- 2. 요청 모델 정의 -----
class CorrectionRequest(BaseModel):
    text: str

@app.post("/correct")
def correct_text(data: CorrectionRequest):
    start_time = time.time()
    if not translator.model_is_loaded:
        print("📦 모델이 언로드 상태이므로 다시 로드합니다.")
        translator.load_model()

    text = data.text
    inputs = tokenizer.convert_ids_to_tokens(tokenizer(text)["input_ids"])
    results = translator.translate_batch(
        [inputs],
        beam_size=3,
        max_decoding_length=128,
    )
    output_tokens = results[0].hypotheses[0]
    output_ids = tokenizer.convert_tokens_to_ids(output_tokens)
    output_text = tokenizer.decode(output_ids, skip_special_tokens=True)

    elapsed_time = time.time() - start_time
    return {
        "input": text,
        "corrected": output_text,
        "unload_time_sec": round(elapsed_time, 4)
    }

# ----- 4. 모델을 CPU로 이동 -----
@app.post("/unload")
def unload_model_to_cpu():
    start_time = time.time()
    translator.unload_model(to_cpu=True)
    elapsed_time = time.time() - start_time
    return {
        "message": "모델이 GPU에서 언로드되고 CPU로 이동했습니다.",
        "unload_time_sec": round(elapsed_time, 4)
    }
```
### <br/>

### app 실행
```
uvicorn app_test:app --host 0.0.0.0 --port 8000
```
### 초기 모델 로드 시간이 걸린다.
#### ![image](https://github.com/user-attachments/assets/53d7678c-02d2-45b5-a5b8-3d83eb77332e)
### 다음과 같이 GPU memory에 로드된다.
#### ![image](https://github.com/user-attachments/assets/d53c7f49-2b70-4b50-af0b-68b01f6124f2)
### <br/>

### 모델 호출
### 거의 실시간으로 호출된다.
```
curl -X POST http://localhost:8000/correct -H "Content-Type: application/json" -d '{"text": "이 문장을 교정해 주세요"}'
```
#### ![image](https://github.com/user-attachments/assets/bd421bef-25d2-477b-b725-479bf0da58e3)
### <br/.

### GPU memory -> RAM으로 이동
```
curl -X POST http://localhost:8000/unload
```
### 시간은 별로 안 걸린다. 그리고 gpu memory에서 RAM으로 이동한다.
```
{"message":"모델이 GPU에서 언로드되고 CPU로 이동했습니다.","unload_time_sec":2.6011}
```
#### ![image](https://github.com/user-attachments/assets/a2c2966f-a7f4-402c-96d3-5c1c3daa6c05)
### <br/>

### 다시 호출하면 자동으로 GPU memory로 올라온다.
### 빠르게 호출할 수 있다.
#### ![image](https://github.com/user-attachments/assets/c2179b76-dd47-438d-96bd-a70418b89a76)
### <br/>

## VS code remote 접속 문제
### 여기서 문제가 왜 계속 CPU 리소스를 사용하고 있다.
### vscode에서 remote로 접속하면 뭔가 리소스 상에서 충돌이 있다. 
### 그래서 그냥 docker exec로 접속하니까 괜찮다(리소스 계속 잡고 있는 것 없음). 코드 상에는 문제 없는 것을 확인했다.

