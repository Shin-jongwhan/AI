### 250518
## whisper 설치 방법
### venv로 구성하여 진행하였다.
### 내 GPU에 맞춰 torch를 설치하였다.
### webrtcvad는 음성 활동 감지 (VAD, Voice Activity Detection) 기능을 활용하게 만들어주는 기능이다. VAD는 마이크 입력에서 사람이 말하고 있는 구간과 무음(또는 잡음) 구간을 구분해줘서, 말이 시작되고 끝난 시점을 자동으로 잡아준다.
```
# venv 환경 실행
py -3.12 -m venv myenv
.\myenv\Scripts\activate

# 설치
git clone https://github.com/openai/whisper.git
cd whisper
pip install -r requirements.txt

# 추가 dependency 설치
pip uninstall torch torchvision torchaudio -y
pip cache purge
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
pip install openai-whisper
pip install webrtcvad
```
### <br/>

### ffmpeg
### 오디오를 변환할 수 있는지 툴이다. 아래 사이트에서 다운로드한다. 아래 블로그 글을 참고하였다.
#### https://velog.io/@tjdwjdgus99/ffmpeg-%EC%82%AC%EC%9A%A9%EB%B2%95
#### https://www.gyan.dev/ffmpeg/builds/
### <br/><br/>

## 테스트
### 아래 코드로 테스트 진행하였다. audio가 생성되는 경로를 지정하고 오디오 생성과 출력을 확인하였다.
```
import os
import time
import whisper
import speech_recognition as sr

# Whisper 모델 로드
model = whisper.load_model("base")
save_dir = "C:\\test\\speech_recognition\\whisper\\tests\\audio"

# Recognizer 초기화
recognizer = sr.Recognizer()

def listen_and_transcribe():
    with sr.Microphone() as source:
        print("잠시 대기 중... (말하면 녹음 시작)")
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)
        print("녹음 완료. 처리 중...")

        # 파일 저장
        filename = f"{save_dir}\\voice_{int(time.time())}.wav"
        with open(filename, "wb") as f:
            f.write(audio.get_wav_data())

        # Whisper로 인식
        result = model.transcribe(filename)
        print("🗣️ 인식 결과:", result["text"])

        # 필요 없으면 삭제
        #os.remove(filename)

# 반복적으로 실행
while True:
    try:
        listen_and_transcribe()
    except KeyboardInterrupt:
        print("\n종료합니다.")
        break
    except Exception as e:
        print("오류 발생:", e)

```
### <br/>

### 실행하면 이렇게 된다. 음성을 인식하고 내가 말한 걸 출력해주고, 오디오를 끊어서 저장한다.

https://github.com/user-attachments/assets/eacd06b1-805e-4270-8133-df9d74b86f10

