### 250521
## 작업 (Task) 사용 방법
### 먼저 API를 호출할 수 있는 웹사이트가 필요하다.
### 그럴려면 HTTPS 설정이 되어 있어야 한다.
### 나는 https://www.cognimosyne.com/ 으로 도메인 발급을 해놓은 상태이기 때문에 이걸로 간단하게 테스트를 진행하였다.
### 도메인 구매 및 SSL cert 설정 참고
#### https://github.com/Shin-jongwhan/nginx/tree/main/publish_domain_and_ssl_cert
### <br/>

### fastAPI로 간단하게 website 하나 띄웠다.
#### gpts_task_test.py
```
from fastapi import FastAPI
from fastapi.responses import JSONResponse
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

@app.get("/ping", summary="핑 테스트", description="서버가 응답 가능한지 확인하기 위한 간단한 GET 요청입니다.")
async def ping_general():
    return JSONResponse(content={"message": "pong"})

```
#### <br/>

### web app 실행
```
python -m uvicorn gpts_task_test:app --reload --host 0.0.0.0 --port 443 \
--ssl-certfile="all.crt.pem" --ssl-keyfile="key.pem"
```
#### <br/>

### 접속해보면 이렇게 단순하게 json으로 return만 해준다.
#### ![image](https://github.com/user-attachments/assets/d824689e-79ca-4ff0-a97e-be7cc665a095)
### <br/>

### 그 다음 작업에 인증 없이 그냥 스키마만 적어본다.
#### ![image](https://github.com/user-attachments/assets/eac5ded7-ba0e-4383-9651-4417f56037a3)
### 아래 보면 url에 base url이 적혀있고, paths 하위에 api 호출할 주소가 각각 적혀 있다.
```
openapi: 3.1.0
info:
  title: Ping API
  description: 서버 응답 테스트용 단순 API입니다.
  version: 1.0.0
servers:
  - url: https://www.cognimosyne.com
    description: 기본 서버

paths:
  /ping:
    get:
      operationId: ping
      summary: 핑 테스트
      description: 서버가 응답 가능한지 확인하기 위한 간단한 GET 요청입니다.
      responses:
        '200':
          description: "pong 메시지를 포함한 응답"
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string
                    example: pong
```
### <br/>

### 테스트 버튼을 누르면 chatGPT가 테스트를 해준다.
#### ![image](https://github.com/user-attachments/assets/357ab0ae-2e7b-43e5-8388-6ab6d0e9b29f)
#### ![image](https://github.com/user-attachments/assets/e5f17bef-437c-47fe-840b-16161b1e0a5b)
