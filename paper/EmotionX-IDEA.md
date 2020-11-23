<h3>EmotionX-IDEA: Emotion BERT – an Affectional Model for Conversation</h3>

 - URL: https://arxiv.org/pdf/1908.06264.pdf   

--------------
 - 목적
   - 맥락을 고려하고, 모든 발음에 대한 감정 인식  

<br>
    
 - 방법   
![image](https://user-images.githubusercontent.com/54783292/96392005-9e929b80-11f5-11eb-9e1b-7cd9a41b8471.png)
   - Causal Utterance Modeling
      - sequence of utterance u, ![image](https://user-images.githubusercontent.com/54783292/96392029-b4a05c00-11f5-11eb-8c16-fab98f8a0854.png)   
      - 연속된 ![image](https://user-images.githubusercontent.com/54783292/96392043-c08c1e00-11f5-11eb-846b-cd663bfd9773.png)을 재배열한 singel sentence representation ![image](https://user-images.githubusercontent.com/54783292/96392051-c8e45900-11f5-11eb-8cfa-037b23c49734.png)
      - sentence representation corpus ![image](https://user-images.githubusercontent.com/54783292/96392060-d4378480-11f5-11eb-95c8-fffff85e979c.png)
      - Freinds: Personality tokenization(주연 6명의 토큰을 만들어, 화자가 6명중 한명일 경우 utterance 전에 토큰 출력)
      - EmotionPush: Unifying tokenization(속어, 약어, 오자, 하이퍼링크, 이모지가 포함되어 있어 이것들의 토큰을 통일시킴)
   - Model Pre-training
      - FriendsBERT: emorynlp1 으로 Friends TV Shows의 10개 시즌 대본으로 학습
      - ChatBERT: Twitter emotion dataset으로 사전 학습(twitter streaming API), twitter의 hashtag는 fine tuning 하는데 emotion label로 사용   
![image](https://user-images.githubusercontent.com/54783292/96392015-a8b49a00-11f5-11eb-9176-6b3ea0d897c0.png)
   - Fine-tuning
      - encoder의 final hidden state에서 토큰 [CLS]에 해당하는 첫 embedding vector 사용
      - emotion prediction probability: ![image](https://user-images.githubusercontent.com/54783292/96392087-e5809100-11f5-11eb-90fb-8607dab49d85.png)
      - emotion label 불균형을 해결하기 위해 NLL loss function에 weighted balanced warming를 적용함 
![image](https://user-images.githubusercontent.com/54783292/96392091-eadddb80-11f5-11eb-9e58-fbd8efbb8dae.png)

<br>
    
 - 실험
   - 데이터   
     - Friends, EmotionPush (https://sites.google.com/view/emotionx2019/datasets)
     - emotions: Joy, Sadness, Anger, Neutral

   - 성능측정
     - precision, recall, f1-score

   - 결과   
![image](https://user-images.githubusercontent.com/54783292/96392098-f29d8000-11f5-11eb-8206-214f88f4d08a.png)