<h3>ConveRT: Efficient and Accurate Conversational Representations from Transformers</h3>

 - URL: https://arxiv.org/pdf/1911.03688.pdf

--------------
 - 목적
   - 효과적이고 저렴하고 학습이 빠른 모델 제안

<br>
  
 - 모델
   - Single-Context Model    
![image](https://user-images.githubusercontent.com/54783292/99364190-07565c00-28f9-11eb-992b-a90cae75d18c.png)
     - 두 개의 다른 feed-forward network (FFN) layer
       - feed-forward 1은 standard FFN layer 로 구성
       - Feed-forworward 2 는 3개의 fully-connected nonlinear feed-forward layer와 그 뒤에 최종 linear layer 로 구성
     - Self-attention blocks전에 subword embedding inputs에 positional enconding 존재
     - 6개의 layer는 long sequences와 distant dependencies를 일반화하는데 도움이 됨
     - 다른 네트워크 계층에서 학습된 인코딩을 intent detection 이나 value extraction과 같은 다른 작업으로 보낼 수 있음
     - 사용되는 모든 activation function은 fast GeLU approximation 
   - Multi-Context Model    
![image](https://user-images.githubusercontent.com/54783292/99364227-11785a80-28f9-11eb-859f-a5fd87625472.png)
     - 즉각적인 context와 이전 대화 내역을 결합한 multi-context dual-encoder model
       - 즉각적인 context와 그에 따르는 response의 상호작용
       - 대화 내역에서 최대 10개의 이전 context와 response의 상호작용
       - 전체 context와 response의 상호작용
     - Transformer layer는 singe context encoder 모델을 참조함
     - feed-forward 2 blocks은 singe context encoder 와 동일

<br>
    
 - 데이터  
   - Response Selection
     - Reddit Data: 727M 개의 input-response 쌍, 654M train 나머지 test
     - AMAZONQA: Amazon 제품에 대한 정보를 질문-응답 쌍으로 된 전자 상거래 데이터셋
     - DSTC7- UBUNTU: 1M+ 개의 Ubuntu v2 corpus, 10K train/10K validation/5K test
   - Intent Classification   
![image](https://user-images.githubusercontent.com/54783292/99364249-1806d200-28f9-11eb-9e1b-5902bb9dc931.png)
     - train : validation : test = 8 : 1 : 1

<br>

- 실험
  - Response Selection
    - 대화 내역이 주어지면 가장 적절한 응답 선택하기
    - 문맥과 응답 컬렉션을 인코딩한 후, 각 후보 응답의 인코딩에 대해 쿼리 표현을 일치시켜 가장 관련성이 높은 응답을 검색함
    - label이 없는 대화 데이터를 사용하여 학습한 후, 적은 양의 데이터를 사용해 레이어를 추가하여 모델을 fine-tuning함
    - 비교 모델: USE-LARGE, BERT-LARGE, USE-QA, POLYAI-DUAL
  - Intent Classification
    - encoding 𝑟_𝑥 를 input으로 사용
    - encoder를 고정하고, sentence encoding위에 classifier layer를 두어 학습함
    - 비교 모델: USE-LARGE, BERT-LARGE
<br>

- 성능 측정
  - Response Selection: Recall@k

<br>
   
 - 결과
   - 기존 encoder에 비해 cost 절감, model size 작음, training time 빠름
![image](https://user-images.githubusercontent.com/54783292/99364276-205f0d00-28f9-11eb-82f0-9a5b6d9d9267.png)
   - Response Selection (Reddit,AmazonQA / DSTC7-UBUNTU)   
![image](https://user-images.githubusercontent.com/54783292/99364312-294fde80-28f9-11eb-9967-88ee6ac69f2c.png)   
![image](https://user-images.githubusercontent.com/54783292/99364361-38369100-28f9-11eb-8457-a8f2b2dd9031.png)
  - Intent Classification   
![image](https://user-images.githubusercontent.com/54783292/99364382-3d93db80-28f9-11eb-8f2c-1ff171be0476.png)
