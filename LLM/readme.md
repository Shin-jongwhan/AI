### 250515
## LLM API server 만들기
### python API server 코드
```
# llama_api.py

from fastapi import FastAPI, Request
from pydantic import BaseModel
from transformers import AutoTokenizer, AutoModelForCausalLM
import torch

app = FastAPI()

# 모델 경로
model_path = "/data/Llama-3.2-1B"

# 모델 & 토크나이저 로드 (최초 1회)
print("Loading model...")
tokenizer = AutoTokenizer.from_pretrained(model_path)
model = AutoModelForCausalLM.from_pretrained(model_path, torch_dtype=torch.float16)
device = "cuda" if torch.cuda.is_available() else "cpu"
model.to(device).eval()
print("Model loaded to", device)

# 요청 데이터 구조 정의
class PromptRequest(BaseModel):
    prompt: str
    max_tokens: int = 512

# POST 요청 처리
@app.post("/generate")
def generate(req: PromptRequest):
    inputs = tokenizer(req.prompt, return_tensors="pt").to(device)
    with torch.no_grad():
        outputs = model.generate(
            **inputs,
            max_new_tokens=req.max_tokens,
            do_sample=True,
            top_k=50,
            top_p=0.95,
            temperature=0.7
        )
    result = tokenizer.decode(outputs[0], skip_special_tokens=True)
    return {"response": result}
```
### <br/>

### 아래의 명령어로 server를 실행한다.
```
uvicorn test_apiserver:app --host 0.0.0.0 --port 8000
```
### <br/>

### 새로운 쉘 터미널에 접속해서 curl로 API 호출
```
curl -X POST http://localhost:8000/generate   -H "Content-Type: application/json"   -d '{"prompt": "Hello, how are you today?", "max_tokens": 300}'
```
### 출력 결과
```
{"response":"Hello, how are you today? I’m happy that you’re here with me and I hope that I’m doing something that will make you feel happy, so I would like to thank you for your time. As we know, there are a lot of people who are struggling in life and have a hard time finding happiness. This is why I decided to write a post about how to be happy and how you can find happiness in life.\nI know that there are a lot of people who are looking for some way to be happy and to find happiness in life. I know that there are a lot of people who are struggling with their lives and they’re not able to find happiness in life. That’s why I want to share some tips with you today. I want to show you how to be happy and how you can find happiness in life.\nI know that there are a lot of people who are struggling with their lives and they’re not able to find happiness in life. That’s why I want to share some tips with you today. I want to show you how to be happy and how you can find happiness in life.\nI know that there are a lot of people who are looking for some way to be happy and to find happiness in life. I know that there are a lot of people who are struggling with their lives and they’re not able to find happiness in life. That’s why I want to share some tips with you today. I want to show you how to be happy and how you can find happiness in"}
```
