<h3>Emotional Chatting Machine: Emotional Conversation Generation with Internal and External Memory</h3>

 - URL: https://arxiv.org/abs/1704.01074    
 - github: https://github.com/tuxchow/ecm

--------------
 - 목적
   - 인간과 의사소통이 가능한 챗봇을 만들기 위해서는 인간의 감정을 인지하고 표현하는 능력을 갖추는 것이 필요함    

<br>
    
 - 방법 (emotional chatting machine)
![image](https://user-images.githubusercontent.com/54783292/93037849-5a542e80-f67e-11ea-9ef5-fbcbbda3df01.png)
   
   - training: 각 responses에 대한 emotion label 생성하기 위해, post-response쌍의 말뭉치를 emotion classifier에 넣음
   - inference: 다른 emotion categories를 조건으로하는 emotional responses를 생성하기위해 post를 넣음
   - Encoder-Decoder: seq2seq의 encoder-decoder를 기반으로 함, GRU 사용
   - Emotion Category Embedding   
![image](https://user-images.githubusercontent.com/54783292/93037870-69d37780-f67e-11ea-8209-d018ddf07383.png)
      - emotion category embedding과 context vector은 decoder의 state를 업데이트 하기위해 decoder에 공급됨   
   - internal memory module을 사용함   
![image](https://user-images.githubusercontent.com/54783292/93037883-7526a300-f67e-11ea-9f97-d49f3b5330ca.png)   
      - decoding 시작 전에 각 category에 대한 internal emotion state가 있음
      - 각 단계에서 emotion state는 감소함   
      - decoding 과정이 끝나면 emotion state는 0으로 감소해야 함
    - external memory module에 의한 word의 명확한 선택을 통해, emotion의 명확한 표현을 모델링함   
![image](https://user-images.githubusercontent.com/54783292/93037895-7c4db100-f67e-11ea-819c-266b282399c8.png)   
      - final decoding probability은 emotion softmax와 generic softmax 사이에서 가중되며, type selector에 의해 가중치가 계산됨
      - 다음 단어 yt는 가중된 두개의 probability의 결합으로 추출됨

<br>
    
 - 실험
   - 데이터
     - NLPCC emotion classification dataset를 이용하여 emotion classifier를 학습시킨 후, 해당 classifier를 사용해 STC conversation dataset에 주석을 달아 생성
     - NLPCC dataset의 분류 정확도   
![image](https://user-images.githubusercontent.com/54783292/93037911-853e8280-f67e-11ea-8257-578717136366.png)
     - Emotional STC   
![image](https://user-images.githubusercontent.com/54783292/93037921-8b346380-f67e-11ea-826a-66686698d28c.png)

   - 성능측정
     - 자동 평가
       - content가 문법에 맞고 목적에 적합한지 평가하기 위해, perplexity를 평가지표로 함   
![image](https://user-images.githubusercontent.com/54783292/93037946-a010f700-f67e-11ea-9a82-f0e908c0d02e.png)
     - 수동 평가
       - content과 emotion으로 생성된 responses가 잘 이해하기 위해, 생성된 responses를 세명의 주석자에게 줌
       - 주석자들이 내용(0,1,2), 감정(0,1)으로 점수를 매김   
![image](https://user-images.githubusercontent.com/54783292/93037938-94bdcb80-f67e-11ea-8254-44952efffe75.png)

   - 결과
     - emotion influence를 모델링하기 위해 ECM 제안
     - emotion factor를 모델링하기 위한 3가지 메커니즘 제시
     - content뿐만 아니라 emotion에서도 적절한 reponses를 생성함