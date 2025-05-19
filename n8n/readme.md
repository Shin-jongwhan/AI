### 250519
## n8n in docker
### 매우 간단하게 실행할 수 있다.
##### https://docs.n8n.io/hosting/installation/docker/#prerequisites
### docker volume은 따로 만들어도 되고, 지정해줘도 된다. 나는 지정해줬고, /home/node/.n8n 경로는 고정이다.
```
docker run -it --rm --name n8n -p 5678:5678 -v c:/docker_volume/n8n:/home/node/.n8n docker.n8n.io/n8nio/n8n
```
### <br/>

### 실행하면 localhost url로 접속해본다.
#### ![image](https://github.com/user-attachments/assets/d4241bee-b40d-442a-b21b-f089ae411d30)
#### <br/>

### 나는 ollama docker를 띄어둔 상태이기 때문에 해당 경로를 인식할 수 있게 만들어주었다.
#### ![image](https://github.com/user-attachments/assets/4e4539ff-721f-46c6-ad83-4f1f061b4485)
### 연결이 안 되어 있으면 이렇게 뜨는데, 여기서 ollama를 찾아서 선택하면 된다.
#### ![image](https://github.com/user-attachments/assets/e56782cc-d602-4af7-827d-0fd465c574ab)
#### ![image](https://github.com/user-attachments/assets/819a92f2-8eed-49bb-ad4c-093e5257dc54)
#### <br/>

### docker url로 적어준다.
#### http://host.docker.internal:11434/
#### ![image](https://github.com/user-attachments/assets/768e01d4-0921-473a-9267-8c7d1433e5dd)
### <br/>

### 다운로드한 모델 목록이 나온다.
#### ![image](https://github.com/user-attachments/assets/6ae0786d-2b82-459c-921a-7f780b1ab4b7)
### <br/>

### 같은 방식으로 agent에 memory를 연결해주는데, 나는 simple memory를 연결해주었다.
#### ![image](https://github.com/user-attachments/assets/4058511c-7251-433f-ba29-a58d5120de60)
### <br/>

### chat box를 열 수 있다. 여기서 대화를 입력하면 output이 출력된다.
#### ![image](https://github.com/user-attachments/assets/54a035c8-0f99-4380-9611-2f37d5f56d65)
### <br/>

### 여기까지가 LLM만 연결한 것까지만 해본 것이고, 이보다 더 복잡한 workflow를 짤 수 있다.
### 참고 1. 메일, 캘린더 보내기
#### ![image](https://github.com/user-attachments/assets/0b86251b-68dd-458a-90c9-e17327f2ce13)
#### ![image](https://github.com/user-attachments/assets/67c3176c-e9f0-44e2-8b53-8d2bfe91c993)
#### https://www.youtube.com/watch?v=n34gMNovWks&t=852s
#### <br/>

### 참고 2. 유튜브 영상을 요약하고, 관련 링크를 몇 개 찾아서 해당 링크도 각각 요약 후 discord로 형식 맞춰서 보내기
#### ![image](https://github.com/user-attachments/assets/eaa78299-2678-4410-9219-0efdd44d2da1)
#### https://www.youtube.com/watch?v=ZUwWpNEu8-k
#### <br/>

### 참고 3. RAG기반 AI 챗봇 직접 만들어쓰기. superbase 이용해서 데이터 저장하기
#### 아래 예시는 법률 정보를 데이터베이스에다가 저장하고, 법률 정보인지 판단이 되면 데이터베이스에서 검색해서 가져오는 방식이다.
#### ![image](https://github.com/user-attachments/assets/0d115deb-d6a4-4e9d-a926-098c5a42defd)
#### https://www.youtube.com/watch?v=Fk5pQ0fQkJ0&t=203s
