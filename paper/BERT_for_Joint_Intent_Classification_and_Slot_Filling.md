<h3>BERT for Joint Intent Classification and Slot Filling</h3>

 - URL: https://arxiv.org/pdf/1902.10909.pdf   

--------------
 - 목적
   - Spoken language understanding(SLU)
     - 일반적으로 intent detection과 slot filling을 포함함
     - 사용자 발언을 위한 의미론적 구문을 형성하는 것을 목표로 함
   - NLU를 위한 BERT 연구는 큰 노력이 없음
     - NLU의 열악한 능력을 해결하기 위해 BERT pre-trained model을 탐구함
     - BERT를 기반으로 joint model을 제안하며, 제안 모델이 상당히 개선됨을 입증하기 위함
<br>

 - Joint Intent Classification and Slot Filling
   - BERT는 joint intent classification과 slot filling으로 쉽게 확장할 수 있음
   - intent는 첫번째 special token([CLS])의 hidden state(h1)로 예측 가능함   
![image](https://user-images.githubusercontent.com/54783292/101301165-497d1880-387b-11eb-8a73-c6575f054c47.png)
   - slot filling label을 분류하도록, 다른 token(h2,..,hT)의 마지막 hidden states를 softmax layer에 넣어줌   
![image](https://user-images.githubusercontent.com/54783292/101301214-75989980-387b-11eb-9862-80b794b401fa.png)
   - 각 tokenized input word를 WordPiece tokenizer에 넣어주고, softmax classifier의 input으로 첫번째 sub-token에 해당하는 hidden state를 사용함
   - joint model을 위해 해당 공식 사용   
![image](https://user-images.githubusercontent.com/54783292/101301232-81845b80-387b-11eb-84d3-124f030e85f7.png)   
   - 조건부 확률을 최대화하는 것이 학습의 목표
   - 모델은 cross-entropy loss를 최소화하여 end-to-end로 fine-tuning함

<br>

 - Conditional Random Field
   - slot label prediction은 주위의 단어 예측에 의존함
   - conditional random fields (CRF)와 같은 구조화된 예측 모델은 slot filling 성능을 개선할 수 있음
     - BiLSTM encoder를 CRF 레이어에 추가하면서 semantic role labeling을 향상시킨다는 연구가 있음
   - Joint BERT 모델에 slot label 종속성을 모델링하기 위해 CRF를 추가의 유효성을 연구함

<br>

 - 데이터
   - ATIS (Airline Travel Information Systems) data set
     - 비행기 예약할 때, 사람들의 오디오 녹음 파일
     - 120 slot labels, 21 intents
     - train : development : test = 4478 : 500 : 893 utterances
   - Snips
     - Snips 개인 음성 비서로 부터 수집
     - 72 slot labels, 7 intents
     - train : development : test = 13,084 : 700 : 700 utterances
 - 실험
   - 모델: BERT-Base
     - pre-training : BooksCorpus(800M words), Wikipedia(2500M words)
     - fine-tuning : development set에서 tuning
 - 성능 측정
   - F1 score (slot filling), Accuracy (intent classification)

<br>
   
 - 결과
   - BiLSTM을 사용한 sequence based joint RNN, attention-based BiRNN, slot-gated model과 비교   
![image](https://user-images.githubusercontent.com/54783292/101301258-95c85880-387b-11eb-87e1-1de3f47e9421.png)   
   - epochs에 따른 성능 비교   
![image](https://user-images.githubusercontent.com/54783292/101301269-a082ed80-387b-11eb-8a24-38d77eafc38a.png)
