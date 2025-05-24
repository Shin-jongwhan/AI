### 250518
## parakeet-tdt
### 오디오에서 자막을 추출하는 기능, Speech Transcription 이다.
### hugging face
- https://huggingface.co/nvidia/parakeet-tdt-0.6b-v2
### github
- https://github.com/NVIDIA/NeMo
### 테스트 할 수 있는 demo 페이지
- https://huggingface.co/spaces/nvidia/parakeet-tdt-0.6b-v2
### <br/>

### 데모 페이지에서 테스트하면 다음과 같이 나온다.
### 오디오에서 자막을 시간 순으로 추출해준다.

https://github.com/user-attachments/assets/7943c918-8c41-443b-9d35-730c92a8e9c5

### <br/>

### csv로도 다운로드할 수 있다.
```
Start (s),End (s),Segment
0.96,3.76,DAIA is an open weights text-to-dialog model.
4.00,6.00,You get full control over scripts and voices.
6.24,6.56,Wow.
6.80,7.60,Amazing.
7.92,10.32,Try it now on GitHub or Hugging Face.
```
### <br/><br/>

## docker
### docker를 하나 만들었다. 최신이라 docker hub에 push 한지 얼마 안 됐는데 벌써 pull이 일어나고 있다. 꽤 인기가 많다.
### docker 실행 방법은 아래 페이지 참고
#### https://hub.docker.com/r/shinejh0528/parakeet-tdt-0.6b-v2
### <br/>

### 데모 페이지에 좀 더 긴 오디오는 아래 스크립트를 활용하라고 나와 있는데, 이건 nvidia 공식 nemo git repo에 있는 script이다.
#### ![image](https://github.com/user-attachments/assets/ba984094-9fb0-4597-9d7f-8c10ec7a1a0c)
### <br/>

### 사용 방법은 일단 model_path를 등록하고 2가지로 구분된다.
- manifest.json을 만들어서 사용. 이걸 사용하는 것을 추천한다.
- audio_dir 옵션 사용. audio dir를 하나 만들고 그 안에 오디오 파일들을 넣으면 된다. 별도 정리는 안 하겠음.
### <br/>

### manifest.json 사용
#### 1. manifest.json 작성
```
{"audio_filepath": "/root/audio/2086-149220-0033.wav", "duration": 7.0, "text": "dummy"}
```
#### <br/>

#### 2. 실행
#### 참고로 timestamps=True 로 해야 문장이나 char 별로 출력이 된다.
```
python transcribe_speech.py   model_path=/usr/local/src/nvidia/parakeet-tdt-0.6b-v2.nemo   dataset_manifest=/root/manifest.json   output_filename=output.json   clean_groundtruth_text=True   langid='en'   batch_size=32   timestamps=True   compute_langs=False   cuda=0   amp=True
```
#### <br/>

#### 결과
```
{"audio_filepath": "/root/audio/2086-149220-0033.wav", "duration": 7.0, "text": "dummy", "pred_text": "Well, I don't wish to see it any more, observed Phebe, turning away her eyes. It is certainly very like the old portrait,", "char": [{"char": ["Well"], "start_offset": 5, "end_offset": 8, "start": 0.4, "end": 0.64}, {"char": [","], "start_offset": 8, "end_offset": 8, "start": 0.64, "end": 0.64}, {"char": ["I"], "start_offset": 9, "end_offset": 11, "start": 0.72, "end": 0.88}, {"char": ["don"], "start_offset": 11, "end_offset": 12, "start": 0.88, "end": 0.96}, {"char": ["'"], "start_offset": 12, "end_offset": 12, "start": 0.96, "end": 0.96}, {"char": ["t"], "start_offset": 14, "end_offset": 14, "start": 1.12, "end": 1.12}, {"char": ["w"], "start_offset": 14, "end_offset": 15, "start": 1.12, "end": 1.2}, {"char": ["ish"], "start_offset": 15, "end_offset": 16, "start": 1.2, "end": 1.28}, {"char": ["to"], "start_offset": 16, "end_offset": 18, "start": 1.28, "end": 1.44}, {"char": ["see"], "start_offset": 18, "end_offset": 20, "start": 1.44, "end": 1.6}, {"char": ["it"], "start_offset": 20, "end_offset": 22, "start": 1.6, "end": 1.76}, {"char": ["any"], "start_offset": 22, "end_offset": 24, "start": 1.76, "end": 1.92}, {"char": ["more"], "start_offset": 24, "end_offset": 25, "start": 1.92, "end": 2.0}, {"char": [","], "start_offset": 25, "end_offset": 25, "start": 2.0, "end": 2.0}, {"char": ["ob"], "start_offset": 28, "end_offset": 29, "start": 2.24, "end": 2.32}, {"char": ["s"], "start_offset": 29, "end_offset": 30, "start": 2.32, "end": 2.4}, {"char": ["er"], "start_offset": 30, "end_offset": 31, "start": 2.4, "end": 2.48}, {"char": ["ved"], "start_offset": 31, "end_offset": 33, "start": 2.48, "end": 2.64}, {"char": ["P"], "start_offset": 33, "end_offset": 34, "start": 2.64, "end": 2.72}, {"char": ["he"], "start_offset": 34, "end_offset": 34, "start": 2.72, "end": 2.72}, {"char": ["be"], "start_offset": 36, "end_offset": 37, "start": 2.88, "end": 2.96}, {"char": [","], "start_offset": 37, "end_offset": 37, "start": 2.96, "end": 2.96}, {"char": ["t"], "start_offset": 42, "end_offset": 43, "start": 3.36, "end": 3.44}, {"char": ["ur"], "start_offset": 43, "end_offset": 44, "start": 3.44, "end": 3.52}, {"char": ["ning"], "start_offset": 44, "end_offset": 46, "start": 3.52, "end": 3.68}, {"char": ["a"], "start_offset": 46, "end_offset": 48, "start": 3.68, "end": 3.84}, {"char": ["way"], "start_offset": 48, "end_offset": 50, "start": 3.84, "end": 4.0}, {"char": ["her"], "start_offset": 50, "end_offset": 52, "start": 4.0, "end": 4.16}, {"char": ["e"], "start_offset": 52, "end_offset": 53, "start": 4.16, "end": 4.24}, {"char": ["y"], "start_offset": 53, "end_offset": 55, "start": 4.24, "end": 4.4}, {"char": ["es"], "start_offset": 55, "end_offset": 58, "start": 4.4, "end": 4.64}, {"char": ["."], "start_offset": 58, "end_offset": 58, "start": 4.64, "end": 4.64}, {"char": ["It"], "start_offset": 62, "end_offset": 64, "start": 4.96, "end": 5.12}, {"char": ["is"], "start_offset": 64, "end_offset": 67, "start": 5.12, "end": 5.36}, {"char": ["c"], "start_offset": 67, "end_offset": 68, "start": 5.36, "end": 5.44}, {"char": ["ert"], "start_offset": 68, "end_offset": 69, "start": 5.44, "end": 5.5200000000000005}, {"char": ["ain"], "start_offset": 69, "end_offset": 70, "start": 5.5200000000000005, "end": 5.6000000000000005}, {"char": ["ly"], "start_offset": 70, "end_offset": 72, "start": 5.6000000000000005, "end": 5.76}, {"char": ["very"], "start_offset": 72, "end_offset": 75, "start": 5.76, "end": 6.0}, {"char": ["like"], "start_offset": 75, "end_offset": 78, "start": 6.0, "end": 6.24}, {"char": ["the"], "start_offset": 78, "end_offset": 79, "start": 6.24, "end": 6.32}, {"char": ["o"], "start_offset": 79, "end_offset": 80, "start": 6.32, "end": 6.4}, {"char": ["ld"], "start_offset": 80, "end_offset": 82, "start": 6.4, "end": 6.5600000000000005}, {"char": ["p"], "start_offset": 82, "end_offset": 83, "start": 6.5600000000000005, "end": 6.640000000000001}, {"char": ["ort"], "start_offset": 83, "end_offset": 84, "start": 6.640000000000001, "end": 6.72}, {"char": ["ra"], "start_offset": 84, "end_offset": 85, "start": 6.72, "end": 6.8}, {"char": ["it"], "start_offset": 85, "end_offset": 86, "start": 6.8, "end": 6.88}, {"char": [","], "start_offset": 86, "end_offset": 86, "start": 6.88, "end": 6.88}], "word": [{"word": "Well,", "start_offset": 5, "end_offset": 8, "start": 0.4, "end": 0.64}, {"word": "I", "start_offset": 9, "end_offset": 11, "start": 0.72, "end": 0.88}, {"word": "don't", "start_offset": 11, "end_offset": 14, "start": 0.88, "end": 1.12}, {"word": "wish", "start_offset": 14, "end_offset": 16, "start": 1.12, "end": 1.28}, {"word": "to", "start_offset": 16, "end_offset": 18, "start": 1.28, "end": 1.44}, {"word": "see", "start_offset": 18, "end_offset": 20, "start": 1.44, "end": 1.6}, {"word": "it", "start_offset": 20, "end_offset": 22, "start": 1.6, "end": 1.76}, {"word": "any", "start_offset": 22, "end_offset": 24, "start": 1.76, "end": 1.92}, {"word": "more,", "start_offset": 24, "end_offset": 25, "start": 1.92, "end": 2.0}, {"word": "observed", "start_offset": 28, "end_offset": 33, "start": 2.24, "end": 2.64}, {"word": "Phebe,", "start_offset": 33, "end_offset": 37, "start": 2.64, "end": 2.96}, {"word": "turning", "start_offset": 42, "end_offset": 46, "start": 3.36, "end": 3.68}, {"word": "away", "start_offset": 46, "end_offset": 50, "start": 3.68, "end": 4.0}, {"word": "her", "start_offset": 50, "end_offset": 52, "start": 4.0, "end": 4.16}, {"word": "eyes.", "start_offset": 52, "end_offset": 58, "start": 4.16, "end": 4.64}, {"word": "It", "start_offset": 62, "end_offset": 64, "start": 4.96, "end": 5.12}, {"word": "is", "start_offset": 64, "end_offset": 67, "start": 5.12, "end": 5.36}, {"word": "certainly", "start_offset": 67, "end_offset": 72, "start": 5.36, "end": 5.76}, {"word": "very", "start_offset": 72, "end_offset": 75, "start": 5.76, "end": 6.0}, {"word": "like", "start_offset": 75, "end_offset": 78, "start": 6.0, "end": 6.24}, {"word": "the", "start_offset": 78, "end_offset": 79, "start": 6.24, "end": 6.32}, {"word": "old", "start_offset": 79, "end_offset": 82, "start": 6.32, "end": 6.5600000000000005}, {"word": "portrait,", "start_offset": 82, "end_offset": 86, "start": 6.5600000000000005, "end": 6.88}], "segment": [{"segment": "Well, I don't wish to see it any more, observed Phebe, turning away her eyes.", "start_offset": 5, "end_offset": 58, "start": 0.4, "end": 4.64}, {"segment": "It is certainly very like the old portrait,", "start_offset": 62, "end_offset": 86, "start": 4.96, "end": 6.88}], "wer": 23.0, "tokens": 1, "ins_rate": 22.0, "del_rate": 0.0, "sub_rate": 1.0}
```
### <br/>

### audio_dir 옵션 사용
#### 내용은 manifest와 같다.
```
python transcribe_speech.py   model_path=/usr/local/src/nvidia/parakeet-tdt-0.6b-v2.nemo   audio_dir=/root/audio   output_filename=/root/output.json   clean_groundtruth_text=True   langid='en'   batch_size=32 
  timestamps=False   compute_langs=False   cuda=0   amp=True
```
