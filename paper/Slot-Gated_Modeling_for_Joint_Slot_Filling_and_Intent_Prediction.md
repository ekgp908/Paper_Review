<h3>Slot-Gated Modeling for Joint Slot Filling and Intent Prediction</h3>

 - URL: https://www.aclweb.org/anthology/N18-2118.pdf   

--------------
 - 목적
   - slot과 intent의 관계가 강하다는 점을 감안하여, intent와 slot attention vectors 사이의 관계를 집중적으로 학습하는 slot gate 제안

<br>

![image](https://user-images.githubusercontent.com/54783292/102040437-2371ee00-3e10-11eb-9ae4-417eb6c7eab7.png)
 - Attention-Based RNN Model
   - Slot filling
     - attention weight![image](https://user-images.githubusercontent.com/54783292/102040451-3258a080-3e10-11eb-90d4-e2c9a4cb6606.png)로 학습된 LSTM hidden states![image](https://user-images.githubusercontent.com/54783292/102040485-400e2600-3e10-11eb-877b-e8d821e14fb6.png)의 weighted sum으로 slot context vector![image](https://user-images.githubusercontent.com/54783292/102040498-4e5c4200-3e10-11eb-9fdd-66123ec23679.png)를 계산함
     - σ는 activation function, ![image](https://user-images.githubusercontent.com/54783292/102040510-5ae09a80-3e10-11eb-8797-e3bb39c2b0de.png)는 fee-forward neural network의 weight matrix   
![image](https://user-images.githubusercontent.com/54783292/102040538-6d5ad400-3e10-11eb-8db3-96560738020c.png)   ,  ![image](https://user-images.githubusercontent.com/54783292/102040552-76e43c00-3e10-11eb-94c8-706c47eade79.png)
     - hidden state와 slot context vector은 slot filling에 활용됨, ![image](https://user-images.githubusercontent.com/54783292/102040587-91b6b080-3e10-11eb-9f22-b3789d53fe84.png)은 slot label sequence   
![image](https://user-images.githubusercontent.com/54783292/102040595-9c714580-3e10-11eb-919d-dde71f54a831.png)
   - Intent prediction
     - intent context vector ![image](https://user-images.githubusercontent.com/54783292/102040606-a72bda80-3e10-11eb-81ec-51e5d4d0e897.png)는 ![image](https://user-images.githubusercontent.com/54783292/102040641-b1e66f80-3e10-11eb-9fa9-f45879db25c0.png)
와 같은 방법으로 계산될 수 있지만 intent detection에서는 BLSTM의 최종 hidden state만 사용함   
![image](https://user-images.githubusercontent.com/54783292/102040664-c9255d00-3e10-11eb-9d68-4c36692d8bfd.png)
 - Slot-Gated Mechanism   
![image](https://user-images.githubusercontent.com/54783292/102040704-dcd0c380-3e10-11eb-91cc-23f30575916b.png)
   - slot gated model는 slot filling 성능을 향상시키기 위해 slot-intent 관계를 모델링하여 intent context vector를 활용하는 추가 gate 도입함
   - slot context vector![image](https://user-images.githubusercontent.com/54783292/102040714-e5c19500-3e10-11eb-872b-c9434059f5e1.png)과 intent context vector![image](https://user-images.githubusercontent.com/54783292/102040732-f40fb100-3e10-11eb-8757-911394221edb.png)은 결합됨, g는 joint context vector의 weighted feature임      
![image](https://user-images.githubusercontent.com/54783292/102040747-025dcd00-3e11-11eb-8c11-302ae2d601b8.png)   
![image](https://user-images.githubusercontent.com/54783292/102040769-11dd1600-3e11-11eb-8e5b-d628aa23d21c.png)   
   - attention mechanism의 slot gate을 비교하기 위해, 오직 intent attention만을 가지고 slot-gated model을 제안함(b)   
![image](https://user-images.githubusercontent.com/54783292/102040784-23beb900-3e11-11eb-80fe-7a017db74d7d.png)   
 - Joint Optimization
   - ![image](https://user-images.githubusercontent.com/54783292/102040796-2c16f400-3e11-11eb-8406-7c5200b4f6c5.png)는 input word sequence에 대한 understanding result(slot filling과 intent prediction)의 조건부 확률임, 최대화됨   
![image](https://user-images.githubusercontent.com/54783292/102040809-36d18900-3e11-11eb-8d15-109073777ba9.png)   
![image](https://user-images.githubusercontent.com/54783292/102040823-3e912d80-3e11-11eb-9ea4-5488a413e8b5.png)

<br>

 - 데이터   
![image](https://user-images.githubusercontent.com/54783292/102040832-4650d200-3e11-11eb-9010-8d63cb813d21.png)   
   - ATIS (Airline Travel Information Systems) data set
     - 비행기 예약할 때, 사람들의 오디오 녹음 파일
   - Snips
     - Snips 개인 음성 비서로 부터 수집
 - 실험
   - baselines: joint model using bidirectional LSTM, attention-based model
   - model: slot-gated model with full attention, slot-gated model with intent attention
 - 성능 측정
   - F1 score (slot filling), Accuracy (intent classification)

<br>
   
 - 결과   
![image](https://user-images.githubusercontent.com/54783292/102040850-4ea90d00-3e11-11eb-9981-a37fb2625d01.png)
