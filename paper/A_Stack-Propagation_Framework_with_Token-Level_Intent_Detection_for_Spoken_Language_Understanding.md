<h3>A Stack-Propagation Framework with Token-Level Intent Detection for Spoken Language Understanding</h3>

 - URL: https://arxiv.org/pdf/1909.02188.pdf     
 - Github: https://github.com/LeePleased/StackPropagation-SLU

--------------
 - 목적
   - intent detection과 slot filling은 Spoken language understanding(SLU) system 구축을 위한 주요 작업임
   - slot filling을 돕고, intent information을 잘 포함하는 framework 제안

<br>

 - 구조   
![image](https://user-images.githubusercontent.com/54783292/104870372-b4963080-598b-11eb-9e6f-facdd49e7c81.png)   
 - Self-Attention Encoder
   - intent detection과 slot filling은 한개의 encoder를 공유함
   - encoder는 self-attentive decoder이고, self-attention BiLSTM을 사용함
   - 각 token으로부터 문맥정보를 추출하기위해 사용
   - input vector X의 matrix에 서로 다른 linear projection을 사용하여 queries(Q), keys(K), values(V)를 맵핑함
   - self-attention output: C, hidden states: H, self-attention output: C, 최종 encoding representations: E   
![image](https://user-images.githubusercontent.com/54783292/104870403-c8da2d80-598b-11eb-953f-8e55b59293af.png) ![image](https://user-images.githubusercontent.com/54783292/104870420-d42d5900-598b-11eb-82d9-667d8967b218.png)
 - Token-Level Intent Detection Decoder
   - training때는 문장의 intent label을 모든 token의 gold intent label로 설정함
   - intent detection network로 uni-directional LSTM 사용
   - decoder stat ![image](https://user-images.githubusercontent.com/54783292/104870643-5ddd2680-598c-11eb-97d0-456d831ec90f.png), 이전 decoding step의 decoder state: ![image](https://user-images.githubusercontent.com/54783292/104874087-6423d080-5995-11eb-9007-6613ac7c81d0.png)
   - 이전 decoding step의 intent label distribution: ![image](https://user-images.githubusercontent.com/54783292/104874170-9a615000-5995-11eb-90a5-484c1a8efc1e.png), aligned encoder hidden state: ![image](https://user-images.githubusercontent.com/54783292/104874179-9f260400-5995-11eb-8560-915d96105ba3.png), token의 intent label: ![image](https://user-images.githubusercontent.com/54783292/104874179-9f260400-5995-11eb-8560-915d96105ba3.png)   
![image](https://user-images.githubusercontent.com/54783292/104870491-02129d80-598c-11eb-9765-109b0d2f0262.png), ![image](https://user-images.githubusercontent.com/54783292/104870519-148cd700-598c-11eb-8815-f2c615ab884d.png), ![image](https://user-images.githubusercontent.com/54783292/104870560-2d958800-598c-11eb-9eed-f45195319143.png)
   - final utterance result ![image](https://user-images.githubusercontent.com/54783292/104874243-cc72b200-5995-11eb-8c02-3c67508b66d0.png)는 모든 token intent 결과의 투표에 의해 생성, utterance 길이: m, intent label의 수: n    
![image](https://user-images.githubusercontent.com/54783292/104874252-d2689300-5995-11eb-99bf-e4d7d7a051a9.png)
 - Stack-propagation for Slot Filling
   - slot filling decoder로 uni-directional LSTM 사용
   - decoder state: input unit: ![image](https://user-images.githubusercontent.com/54783292/104874294-ee6c3480-5995-11eb-9a83-74af6f10cc51.png), intent output distribution: ![image](https://user-images.githubusercontent.com/54783292/104874301-f330e880-5995-11eb-99a8-3bbc4d944f7d.png), aligned encoder hidden state: ![image](https://user-images.githubusercontent.com/54783292/104874307-f6c46f80-5995-11eb-9047-a84eb57bb82d.png), i번째 단어의 slot label: ![image](https://user-images.githubusercontent.com/54783292/104874313-faf08d00-5995-11eb-9746-6f0b83735ce0.png)   
![image](https://user-images.githubusercontent.com/54783292/104874318-ff1caa80-5995-11eb-80eb-eedbeaac29bf.png)![image](https://user-images.githubusercontent.com/54783292/104874323-0348c800-5996-11eb-8af0-e57351a61d5c.png)![image](https://user-images.githubusercontent.com/54783292/104874330-06dc4f00-5996-11eb-9873-8f212ac79133.png)
 - Joint Training 
   - 기존 연구와 다르게 intent detection을 위한 학습 방법임
   - sentence-level classification task를 token-level prediction으로 전환하여, slot filling을 위해 token-level intent 정보를 직접 활용함
   - intent detection objection, slot filling objection, final joint objective   
![image](https://user-images.githubusercontent.com/54783292/104874406-3f7c2880-5996-11eb-8c6a-3607721cc3c4.png)![image](https://user-images.githubusercontent.com/54783292/104874411-42771900-5996-11eb-8c44-fb3e8a80824f.png)![image](https://user-images.githubusercontent.com/54783292/104874418-45720980-5996-11eb-809f-ce6e000dd5ee.png)

<br>

 - 실험
   - 데이터
     - ATIS, SNIPS: Slot-Gated Modeling 과 같은 형식
   - 모델
     - [Joint Seq](https://www.csie.ntu.edu.tw/~yvchen/doc/IS16_MultiJoint.pdf), [Attention BiRNN](https://arxiv.org/pdf/1609.01454.pdf), [Slot-Gated Atten](https://www.aclweb.org/anthology/N18-2118.pdf), [Self-Attentive Model](https://www.aclweb.org/anthology/D18-1417.pdf), [Bi-Model](https://arxiv.org/pdf/1812.10235.pdf), [CAPSULE-NLU](https://arxiv.org/pdf/1812.09471.pdf), [SF-ID Network](https://arxiv.org/pdf/1907.00390.pdf)
   - 성능 측정
     - F1 score (slot filling), accuracy(intent prediction), overall accuracy(sentence-level semantic frame parsing)

<br>
   
 - 결과   
![image](https://user-images.githubusercontent.com/54783292/104874430-515dcb80-5996-11eb-819a-f91fdf3a344e.png)   
![image](https://user-images.githubusercontent.com/54783292/104874439-5589e900-5996-11eb-9ef0-2fba321182ed.png)   
![image](https://user-images.githubusercontent.com/54783292/104874448-57ec4300-5996-11eb-942c-441fa519b937.png)
