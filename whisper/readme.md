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

### <br/><br/>

## whisper 명령어
### whisper 명령어를 이용하면 오디오 파일에서 자막으로 추출할 수 있다. 꽤 정확하고 빠르다. 20분 짜리 오디오인데, 1분에 5분 정도는 처리하는 것 같다.
#### 그런데 오디오가 크면 중간중간에 빈번하지는 않지만 빼먹는 구간이 생긴다. 그래서 좋은 방법으로는 audio 파일이 크면 일부 겹치게 해서 쪼갠 뒤에 whisper로 output을 출력하고, 나중에 병합하는 방식으로 해도 좋을 것 같다.
### 잘리는 것을 해결하려면 다음 옵션을 사용해본다.
- --temperature : 0.5 정도. 높이면 덜 보수적으로 인식해서 누락된 구간을 더 잘 포착한다.
- language ko : 언어를 지정
```
whisper test_audio.mp3 --model medium
# 정확도를 높이는 추천 옵션들
whisper vocals.wav --model large --language ko --temperature 0.4 --output_format all --beam_size 5 --best_of 5 --condition_on_previous_text False --fp16 False
# 음성 인식 threshold 조절
whisper louder.wav --model large --language ko --temperature 0.7 --output_format all --beam_size 5 --best_of 5 --condition_on_previous_text False --fp16 False --no_speech_threshold 0.3 --logprob_threshold -2.0 --compression_ratio_threshold 4.0
```
#### ![image](https://github.com/user-attachments/assets/5ee363c1-4e31-4b07-a2f8-7c838a42af58)
### <br/>

### 다 실행되면 이렇게 output으로 파일을 만들어준다.
#### ![image](https://github.com/user-attachments/assets/c83f70c3-3df3-4327-9e34-c1b2d717e21f)
### <br/><br/>

## 오디오에 대한 추가 작업
### 목소리를 구분하는 데에 뒷배경 소리가 들리면 구분을 잘 못 하기 때문에 소리를 나눌 필요성이 있다. 이때 사용할 수 있는 게 demucs이다. 
```
pip install demucs
```
### <br/>

### 실행
```
demucs test_audio_001856_001935.mp3
```
### <br/>

### 그러면 이렇게 목소리랑 배경 소리들을 구분해서 추출할 수 있다. 
#### ![image](https://github.com/user-attachments/assets/cab3b11b-43f1-40dd-bc42-edc46369c52b)
### <br/>

### 오디오 볼륨 크기 키우기
### 볼륨이 작으면 잘 인식을 못 할 수도 있음.
```
ffmpeg -i vocals.wav -filter:a "volume=3.0" louder.wav
```
