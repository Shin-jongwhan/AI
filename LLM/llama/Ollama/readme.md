### 250519
## docker 
### docker로 매우 간단하게 실행해볼 수 있다.
### 그리고 매우 빠르다 ! 내 GPU로도 chatgpt와 거의 같은 속도로 출력된다.
### 내가 직접 torch로 한 거랑 속도 차이가 나는 이유는 torch는 python 기반이라 어쩔 수 없이 느리고, Ollama는 cpp 기반이라 빠르다.
```
docker run -itd --gpus=all -v C:\docker_volume\llama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```
### <br/>

### docker ps를 입력해서 컨테이너가 실행 중인지 확인
#### ![image](https://github.com/user-attachments/assets/4c305191-478c-4221-9984-9306206b0ae3)
### <br/>

### 컨테이너 접속 후 Ollama에서 llama 모델 실행
```
docker exec -it ollama /bin/bash
ollama run llama3
```
#### ![image](https://github.com/user-attachments/assets/e39f3e26-aac5-4ef1-9eb2-151119b2483c)
