<h3>Attention-Based Recurrent Neural Network Models for Joint Intent Detection and Slot Filling</h3>

 - URL: https://arxiv.org/pdf/1609.01454.pdf   

--------------
 - 목적
   - Spoken language understanding(SLU) system
     - 일반적으로 화자의 의도를 식별하고 의미론적 구성요소를 추출하는 것을 포함함
     - 두 작업을 intent detection과 slot filling이라고 함
     - intent detection과 slot filling 보통 별도로 처리됨
   - Attention-based encoder-decoder neural network models은 최근 번역과 음성 인식에서 유망한 결과를 보임

<br>

 - Encoder-Decoder Model with Aligned Inputs
   - Joint intent detection과 slot filling을 위한 encoder-decoder model임
   - Encoder는 bidirectional RNN 사용
     - Speech recognition과 spoken language에 잘 사용됨   
![image](https://user-images.githubusercontent.com/54783292/100572257-61531a80-3318-11eb-8673-dafc7ecc3e39.png)
   - Slot filling에서 encoder는 source word sequence forward와 source word sequence backward를 읽음
     - Forward RNN는 word sequence를 순서대로 읽고 각 단계마다 hidden states를 생성함
     - Backward RNN은 word sequence를 역순으로 읽고 hidden states의 sequence를 생성함
     - 각 단계의 최종 encoder의 hidden state는 forward와 backward의 결합임
     - 마지막 state는 전체 source sequence의 정보를 전달함
     - backward encoder RNN의 마지막 hidden state는 decoder RNN state를 초기화하는 데 사용됨
   - Intent detection task를 하기 위해 slot filling decoder와 동일한 encoder를 공유하는 decoder를 추가함
 - Attention-Based RNN Model
   - Joint intent detection과 slot filling을 위한 attention-based RNN model   
![image](https://user-images.githubusercontent.com/54783292/100572289-6e700980-3318-11eb-8ff8-eb54d04d403f.png)
   - alignment-based RNN sequence labeling model는 encoder-decoder model을 사용
   - source word sequence forward와 source word sequence backward를 읽음
   - basic RNN unit을 LSTM cell로 사용
   - encoder-decoder architecture와 유사하게, 각 단계에서 hidden state는 forward state와 backward state의 결합임
   - 각각의 hidden state는 전체 input word sequence에 대한 정보를 포함함
   - Intent detection과 slot filling의 joint modeling을 위해, bidirectional RNN의 hidden state를 사용하여 intent    - class의 분포를 생성함
   - Attention을 사용하지 않을 경우, hidden states에 대해 mean-pooling 적용
   - Attention을 사용할 경우, 시간 경과에 따라 hidden states에 weighted average 적용

<br>

 - 데이터
   - ATIS (Airline Travel Information Systems) data set
     - 비행기 예약할 때, 사람들의 오디오 녹음 파일
     - 127 slot labels, 18 intents
   - Train set: ATIS-2 와 ATIS-3 corpora의 4978 utterances
   - Test set: ATIS-3 NOV93과 DEC94의 893 utterances
   - SLU 평가를 위해 ATIS text corpus를 사용
     - 5138 utterances, 110 slot labels, 21 intents

<br>
   
 - 실험
   - Slot Filling, Intent Detection, Joint training(slot filling and intent detection)
 - 성능 측정
   - F1 score (slot filling), Classification Error Rate (intent detection)

<br>
   
 - 결과
   - Slot Filling   
![image](https://user-images.githubusercontent.com/54783292/100572318-7fb91600-3318-11eb-8e3e-bcf6b656c464.png)
     - (a), (b), (c)는 encoder-decoder 변형 모델 비교
     - 두번째는 BiRNN에 대한 결과
     - Attentions을 사용하는 모델에서 F1 점수가 개선됨   
![image](https://user-images.githubusercontent.com/54783292/100572327-88115100-3318-11eb-9ab8-12d5bc0cac7b.png)
     - Slot filling model에 대한 비교
     - 두 개의 모델이 이전 모델보다 앞지름
   - Intent Detection   
![image](https://user-images.githubusercontent.com/54783292/100572339-90698c00-3318-11eb-8634-54806dca982f.png)
     - 이 논문의 intent models과 이전 접근법 사이의 intent classification error rate 비교
     - 제안한 모델이 다른 모델보다 훨씬 능가함
     - Attention-based Encoder-decoder intent model은 bidirectional RNN model을 발전시킴을 알 수 있음
   - Joint training    
![image](https://user-images.githubusercontent.com/54783292/100572352-97909a00-3318-11eb-9962-9e7bca2e9457.png)
     - Intent detection과 slot filling의 Joint training model 성능임
     - Attention-based RNN모델은 joint training에서 더 좋은 성능을 보임
