<h3>논문 :: Deep Reinforcement Learning for Dialogue Generation</h3>

 - URL : https://arxiv.org/pdf/1606.01541.pdf
---------------------
 - 방법
   - SEQ2SEQ
   - mutual information model
     - mutual information responses를 극대화하는 encoder-decoder 모델을 적용
     - 최적화를 위한 policy gradient methods 사용
     1. 미리 학습된 SEQ2SEQ를 사용하여, policy model pRL을 초기화함
     2. input source에 따라 candidate list를 생성
     3. 각 candidate에 대해 사전에 학습된 SEQ2SEQ와 backward SEQ2SEQ로 부터 mutual information score를 얻음   
         : mutual information score는 reward로 사용   
     4. 더 높은 reward로 sequences를 생성하도록, encoder-decoder model에 back-propagated 함   
         : stochastic gradient descent를 사용하여 encoder-decoder model의 파라미터를 업데이트   
     5. gradient는 likelihood ratio trick를 사용해 추정
   - 제안된 RL 모델
<br>
 
 - 실험   
   - 데이터
      - OpenSubtitles dataset에서 “i don’t know what you are taking about”라고 응답할 확률이 가장 낮은 80만 개 시퀀스
   - 두 Agents의 대화 시뮬레이션
      1. 첫 번째 agent는 input message의 vector representation에 encoding을 하고 decoding을 시작하여 output을 생성함
      2. 두 번째 agent는 대화 이력으로 encoding하여 state를 업데이트하고, decoder RNN을 사용하여 응답을 생성함
      3. 해당 응답은 첫번째 agent로 다시 input됨
      4. 1~3 과정 반복
   - 성능측정
      - 대화 길이   
![image](/uploads/15627f662f73961d05a0598966319eb2/image.png)      
      - 다양성     
![image](/uploads/4f83c2fb7ac73c97f6a78053407f0aad/image.png)        
      - 사람이 직접 평가    
![image](/uploads/f8c3e7883e9b57f1650c4cebd8a4eef4/image.png)    
   - 결과
      - RL model이 mutual information model 보다 더 많은 대화와 지속적인 대화를 할 수 있음
      - 대화 중에 관련이 적은 주제로 넘어갈 수 있음
