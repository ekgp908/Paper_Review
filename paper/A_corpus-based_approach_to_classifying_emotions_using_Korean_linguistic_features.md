<h3>A corpus-based approach to classifying emotions using Korean linguistic features</h3>

 - URL: https://link.springer.com/content/pdf/10.1007/s10586-017-0777-8.pdf

--------------
 - 목적
   - 감정 분류 모델 제안

<br>
    
 - 데이터   
   - 25개 emotion keyword
   - stem: emotion keyword를 2글자로 변환
   - stem conjugation: 다양한 문법으로 stem을 수정한 것
![image](https://user-images.githubusercontent.com/54783292/98491970-d41d2880-2279-11eb-8e03-b0fde7146a15.png)   
   - tokenization
     - 토큰화 과정에서 글에 있는 예약 키워드는 역할에 맞게 변환되거나 제거함
     - 문장의 구성 요소 단순화(리트윗표시인 RT글 제거, 언급 표시인 @사용자 제거, 링크 제거)
   - Emoticon selection
     - 문장 중간에 기호가 있을때 파싱이 어려워 이모티콘으로 문장을 구분해 파싱하면 결과가 좋음
     - 다음과 같은 정규화를 이용하여 이모티콘 식별![image](https://user-images.githubusercontent.com/54783292/98492057-121a4c80-227a-11eb-8632-31b0f83d7b66.png)
   - Syntatic parsing
     - Berkeley Parser를 Sejong Treebank에 적용한 Improved inference for unlexicalized parsing를 사용
   - Message-level feature
     - 이모티콘이 바로 옆에 있는 문장에 대한 감정을 보완함

<br>
   
 - 실험   
![image](https://user-images.githubusercontent.com/54783292/98491991-e1d2ae00-2279-11eb-80b7-7df7e424416e.png)   
   - svm으로 학습
   - 10,500 개의 한글 트위터 메시지로 말뭉치를 구성함
   - 10-fold cross-validation 사용
   - Emotion corpus verification
     - emotion class마다 acceptable과 unacceptable 비율 비교: emotion 정보를 정확하게 표현하는지 확인
   - Emotion classification
     - emotion class마다 효과적인 feature 조합을 찾기
     - Word: 연속된 sequences 존재 유무, n-gram으로 표현
     - POS: 각 품사 태그의 발생 횟수, n-gram으로 표현
     - RR(Rewrite rules): 하위 트리로 분할되는지 
     - EK(Emotion keyword): 434개의 단어 목록에서 상위 25개의 단어가 있는지 유무
     - Character: 한글 모음 자모(Hangul vowel jamos) 사용, 문자를 n-gram으로 표현

- 성능 측정
   - precision, recall, F-measure
   
<br>
   
 - 결과   
   - Emotion corpus verification   
![image](https://user-images.githubusercontent.com/54783292/98492005-ebf4ac80-2279-11eb-8905-e75ff696c190.png)   
   - Emotion classification   
![image](https://user-images.githubusercontent.com/54783292/98492014-f0b96080-2279-11eb-81a4-c66d5d8d8070.png)