<h3>Encoding Syntactic Knowledge in Transformer Encoder for Intent Detection and Slot Filling</h3>

 - URL: https://arxiv.org/pdf/2012.11689v1.pdf    

--------------
 - 모델  
![image](https://user-images.githubusercontent.com/54783292/107896030-9a467700-6f78-11eb-8d75-106393a054f5.png)   
   - Input Embedding
     - 첫번째 토큰은 [SOS]
   - intent는 utterance-level, slot은 token-level
   - Transformer encoder Layer X
     - POS tagging model
     - multi-head self-attention layer
     - residual connections과 layer 정규화의 feed forward layer
   - Syntactically-informed Transformer encoder Layer
![image](https://user-images.githubusercontent.com/54783292/107896069-a7fbfc80-6f78-11eb-99e0-a068f2c6a2f3.png) 
     - one attention head
     - dependency parsing tree에서 각 토큰에 대한 전체 조상을 예측함
   - Encoding POS Knowledge
     - 사전 학습한 POS tagger에 의해 생성된 POS 태그를 사용하여 pos tagging을 하도록 모델을 학습함
   - Intent detection
     - 마지막 encoder layer에서 출력되는 토큰 [SOS]의 임베딩 값에 선형 분류기에 적용
   - Slot filling 
     - 마지막 encoder layer의 임베딩 출력값을 MLP-based classifier에 적용
   - Multi-task Learning
     - multi-task learning으로 학습함

<br>

 - 실험
   - 데이터: ATIS, Snips
   - 성능측정
     - SNIPS: F1 score(SF), Accuracy(ID)
     - ATIS: F1 score(SF), 문장의 실제 라벨중 하나만 일치해도 됨(ID-S), 문장의 실제 모든 라벨과 일치해야 함(ID-M)

<br>
   
 - 결과   
![image](https://user-images.githubusercontent.com/54783292/107896091-b2b69180-6f78-11eb-8037-13837f2829f1.png)   
![image](https://user-images.githubusercontent.com/54783292/107896111-bf3aea00-6f78-11eb-911c-b8317d7277a9.png)
