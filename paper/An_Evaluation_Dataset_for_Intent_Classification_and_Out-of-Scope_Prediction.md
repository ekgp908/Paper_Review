<h3>An Evaluation Dataset for Intent Classification and Out-of-Scope Prediction</h3>

 - URL: https://arxiv.org/pdf/1909.02027.pdf
 - github: https://github.com/clinc/oos-eval

--------------
 - 목적
   - 범위를 벗어난(out-of-scope) 쿼리 분류에 초점을 맞춤

<br>
    
 - 데이터   
![image](https://user-images.githubusercontent.com/54783292/97937014-c4f13300-1dc0-11eb-817b-b8d9afa4f179.png)
   - 23,700개의 쿼리로 구성된 crowdsourced dataset
   - 10개의 일반 도메인으로 그룹화 할수있는 150개의 intents
   - 1,200개의 범위를 벗어난 쿼리 포함
   - 전처리
     - 모든 토큰은 down cased됨
     - 문장 끝 구두점 제거
     - 중복된 쿼리 제거 및 대체
     - train, validation, test로 분할
   - 데이터 변형
     - Small: in-scope intent당 50개 학습
     - Imbalanced: intent마다 25, 50, 75, 100개 학습
     - OOS+: 250개의 out-of-scope 학습

<br>
   
 - 실험
   - 사용 모델: SVM, MLP, FastText, CNN, BERT, Platforms
   - 성능 측정: 150개의 intents accuracy, out-of-scope 쿼리에 대한 recall
   - Out-of-Scope 예측
     - oos-train: 
     - oos-threshold: 
     - oos-binary: in / out 분류후 intents 분류
       - under: in-scope 쿼리 언더샘플링
       - wiki aug: wikipedia로 out-of-scope 쿼리 추가

<br>
    
 - 결과   
   - oos-train, oos-threshould   
![image](https://user-images.githubusercontent.com/54783292/97937024-cd496e00-1dc0-11eb-8335-b9bf0b2497af.png)
   - oos-binary    
![image](https://user-images.githubusercontent.com/54783292/97937032-d76b6c80-1dc0-11eb-9553-845ddec792bf.png)
   - 대부분 모델에서 in-scope의 intent는 잘 분류했지만, out-of-scope는 분류하는데 어려움이 있음