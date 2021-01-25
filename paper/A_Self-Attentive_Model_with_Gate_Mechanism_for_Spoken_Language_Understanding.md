<h3>A Self-Attentive Model with Gate Mechanism for Spoken Language Understanding</h3>

 - URL: https://www.aclweb.org/anthology/D18-1417.pdf     

--------------
 - 목적
   - slot과 intent가 서로 공유하도록 되어 있다는 점을 감안할 때 SLU에서 효과적임
   - slot과 intent 사이의 의미적 상관관계를 활용하기 위해 gate mechanism의 self-attention model 제안

<br>

 - 구조   
![image](https://user-images.githubusercontent.com/54783292/105689068-a7ee7b00-5f3d-11eb-81f4-e30f3edd781f.png)
   - intent sequence를 word-level과 character-level(n-gram)로 embedding해서 매핑
   - 문맥을 인지하기위해 self-attention을 사용함(그림은 self-attention layer구조)     
![image](https://user-images.githubusercontent.com/54783292/105689113-b472d380-5f3d-11eb-88f1-8833a039b063.png)
     - 각 time step에서 embedding은 여러 부분으로 구성됨(그림에서 파란색 차이)
     - embedding의 다른 부분들은 상대적으로 독립적이며 다른 역할을 함
     - 그림 속 빨간 사각형은 input을 매핑하는 matrices이고, 변환된 벡터는 self-attention을 계산하기 위해 여러 부분으로 나뉨
   - bidirectional recurrent layer은 embedding input과 context-aware vector를 input으로 함   
![image](https://user-images.githubusercontent.com/54783292/105689149-bdfc3b80-5f3d-11eb-89f8-97d52334241a.png)
     - ![image](https://user-images.githubusercontent.com/54783292/105689188-c8b6d080-5f3d-11eb-97a0-bd8d32ced412.png)는 context-aware vector
     - x 는 recurrent layer의 input
   - slot label 을 맞추기위해 self-attention과 intent-augmented gating mechanism 사용
     - gate는 self-attention에서 계산된 context-aware representation과 intent embedding vector를 MLP로 계산
   - gate의 output과 각각의 BiLSTM의 output을 dot-product 적용
   - gate layer위에 softmax layer를 두어 slot label 분류
   - 단순성을 위해?, input label을 예측할 때는 BiLSTM output에 가중평균만을 함

<br>

 - 실험
   - 데이터
     - ATIS: training set 4978, test set 893, slot 127, intent 18
   - 성능측정: F1-score(slot filling), prediction error rate(intent detection)

<br>
   
 - 결과   
   - slot filling F1-score  
![image](https://user-images.githubusercontent.com/54783292/105689249-dc623700-5f3d-11eb-9cc3-47768742ed54.png)
   - intent detection error rate   
![image](https://user-images.githubusercontent.com/54783292/105689283-e5530880-5f3d-11eb-900e-30c36530fd01.png)
   - joint training   
![image](https://user-images.githubusercontent.com/54783292/105689323-f13eca80-5f3d-11eb-8a18-b7b99c5e0967.png)
