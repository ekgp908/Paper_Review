<h3>EmotionX-HSU: Adopting Pre-trained BERT for Emotion Classification</h3>

 - URL: https://arxiv.org/pdf/1907.09669.pdf   

--------------
 - 목적
   - emotion classification 성능 개선  

<br>
    
 - 방법
   - tokenize: WordPiece tokenization
   - BERT-base  사용
     - Encoder: 12-head self-attention layer, 768-dimensional hidden layer, 110M parameters
     - 입력 문장의 시작엔 [CLS], 끝 부분엔 [SEP]인 토큰 삽입
     - 랜덤으로 문장에 [MASK]로 대체할 단어 선택
     - BERT에 standard softmax layer를 추가해서 label을 예측함   
![image](https://user-images.githubusercontent.com/54783292/93869427-be25c980-fd06-11ea-92be-aef19c4a7379.png)


<br>
    
 - 실험
   - 데이터   
     - Friends, EmotionPush (https://sites.google.com/view/emotionx2019/datasets)   
![image](https://user-images.githubusercontent.com/54783292/93869459-c7af3180-fd06-11ea-8a47-cc84baad031f.png)
     - train: 8개의 emotion중에 3개(joy sadness, anger), neutral은 폐기   
![image](https://user-images.githubusercontent.com/54783292/93869485-cf6ed600-fd06-11ea-9fc7-60c5491f3a1f.png)
     - test: 240개의 대화

   - 성능측정
     - Friends   
![image](https://user-images.githubusercontent.com/54783292/93869510-d72e7a80-fd06-11ea-9201-e567471bcadb.png)
     - EmotionPush   
![image](https://user-images.githubusercontent.com/54783292/93869526-dd245b80-fd06-11ea-919d-5130d2b9a09f.png)

   - 결과
     - 각각의 utterance를 의미를 대표하는 벡터의 시퀀스로 인코딩함
     - softmax classifier를 사용하여 4개의 감정 예측