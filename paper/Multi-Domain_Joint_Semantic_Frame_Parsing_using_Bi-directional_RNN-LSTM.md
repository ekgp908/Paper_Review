<h3>Multi-Domain Joint Semantic Frame Parsing using Bi-directional RNN-LSTM</h3>

 - URL: https://www.csie.ntu.edu.tw/~yvchen/doc/IS16_MultiJoint.pdf

--------------
 - 목적
   - 대화 시스템으로 부터 생성되는 모든 사용자의 utterances의 완벽한 semantic frames 추정
   - holistic multi-domain, multi-task(slot filling, domain/intent detection) modeling 하기 위함

<br>

 - RNN with LSTM cells for slot filling
   - input layer, single hidden layer, output layer (Elman RNN architecture)
   - input: 1-hot vector 또는 word level embedding로 표현   
   ![image](https://user-images.githubusercontent.com/54783292/102732703-c382c680-437e-11eb-8e91-7ff5f24ef47e.png)
   - xt: input, ht-1: t-1시점의 hidden state, pt: hidden layers, yˆt: output layers
   - Wxh. Why: 가중치, φ: 활성함수(tanh, sigm)
   - 모델의 가중치는 training set의 조건부 확률을 최대로 하기위해 back-propagation을 사용함   
![image](https://user-images.githubusercontent.com/54783292/102732727-d1384c00-437e-11eb-9a09-a403a36e7a1e.png)![image](https://user-images.githubusercontent.com/54783292/102732753-ddbca480-437e-11eb-8340-c8b848ec2998.png)
 - Integration of context
   - word tags는 맥락에 의해서도 결정됨
   - 의존성을 알아내기위해 RNN-LSTM의 두가지 확장 버전 사용: look-around LSTM(LSTM-LA), Bi-directional LSTM(bLSTM)   
![image](https://user-images.githubusercontent.com/54783292/102732766-ea40fd00-437e-11eb-8b0b-f2e4bb75455b.png)

<br>

 - 데이터
   - ATIS (Airline Travel Information Systems) data set
     - 비행기 예약할 때, 사람들의 오디오 녹음 파일
     - Domain: alarm, calendar, communication, technical   
![image](https://user-images.githubusercontent.com/54783292/102732796-fc22a000-437e-11eb-84f8-029ba9528ddf.png)
 - 실험
   - Slot Filling
     - 모델: RNN, LSTM, LSTM-LA, bLSTM   
   - Multi-Domain, Joint Model
     - |D|: 도메인의 수
     - SD-Sep: 각 domain에 대해, intent detection model과 slot filling model이 사용되는 2x|D|개의 classifier를 생성
     - SD-Joint: 각 domain에 대해, intent와 slot의 sequence를 맞추는 single model이 사용되며, |D|개의 classifier를 생성
     - MD-Seq: 모든 domain으로 학습한 intent detection model과 slot filling model이 사용되는 2개의 classifier를 생성
     - MD-Joint: 모든 데이터로 학습한 domain, intent, slots을 맞추기 위한 single classifier를 생성
 - 성능 측정
   - Slot Filling: maximum F-measure(best.F), average F-measure(avg.F)
   - Multi-Domain, Joint Model: Slot F-measure(SLOT F), intent accuracy(INTENT A), overall frame error rate(OVERALL E)

<br>
   
 - 결과
   - Slot Filling   
![image](https://user-images.githubusercontent.com/54783292/102732820-0a70bc00-437f-11eb-82a3-afe48118813f.png)
   - Multi-Domain, Joint Model   
     - intent detection accuracy는 joint training에서 향상되지만 slot filling에서는 저하됨   
![image](https://user-images.githubusercontent.com/54783292/102732830-10ff3380-437f-11eb-932a-7579ec777934.png)   
![image](https://user-images.githubusercontent.com/54783292/102732846-178dab00-437f-11eb-87fd-5889da61c167.png)
