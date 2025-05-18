### 250518
## dia
### 요즘에 hugging face에서 핫한 TTS
### 설치 방법도 간단하다. 나는 windows에서 했다.
```
# venv 설치
py -3.12 -m venv myenv
.\myenv\Scripts\activate

git clone https://github.com/nari-labs/dia.git
pip install uv
cd dia && uv run app.py
```
### <br/>

### 실행하면 local에서 실행할 수 있는 web 주소가 나온다.
#### http://127.0.0.1:7860
#### ![image](https://github.com/user-attachments/assets/24b9f988-22e5-40ca-b663-f9af196197af)
### <br/>

### S1, S2는 각각 사람 1, 사람 2를 나타낸다. 매번 실행할 때마다 다른 목소리가 나온다. 목소리를 고정하는 방법도 있을 것 같다.

https://github.com/user-attachments/assets/2161d079-8bae-4f27-8d0f-81f543941480

