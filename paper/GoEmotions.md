<h3>GoEmotions: A Dataset of Fine-Grained Emotions</h3>

 - URL: https://arxiv.org/pdf/2005.00547.pdf

--------------
 - 목적
   - 27개의 감정으로 분류되는 수동 주석 데이터 소개

<br>
    
 - 데이터   
![image](https://user-images.githubusercontent.com/54783292/97144422-bfd02a80-17a7-11eb-8358-ca086f21b0a7.png)
   - 58k개의 Reddit comments, 27개의 emotion + neutral
   - 최소 10k개의 comments가 있는 하위 Reddit에, 삭제된 comment와 영어가 아닌 comment제거
   - 욕설 제거
     - 10%이상의 불쾌한 욕/성인 토큰을 포함하는 comment를 제거
   - 수동으로 comment 검토
     - 불쾌감을 주는 comment 최대한 제거
   - 길이 필터링
     - nltk의 word tokenize 사용
     - 3~30개 토큰을 포함한 comment 선택
     - token count를 12개로 제한해 다운 샘플링함
   - Sentiment balancing
     - positive, negative, ambiguous, neutral인 감정이 아닌 subreddits 제거
   - Emotion balancing
     - 각 comments에 감정 부여
     - 약한 label이 지정된 데이터를 다운 샘플링
   - Subreddit balancing
     - 중앙 Subreddit을 기준으로 다운 샘플링, 무작위 추출
   - masking
     - BERT-based Named Entity Tagger을 사용하여 사람을 가리키는 이름을 [NAME] 마스킹
     - 종교용어는 [RELIGION]을 마스킹

<br>
   
 - 데이터 분석
   - 감정 카테고리와 평가자들의 상관 관계(색상)   
![image](https://user-images.githubusercontent.com/54783292/97144522-f27a2300-17a7-11eb-945d-ab88473997db.png)
     - gratitude, admiration, amusement가 가장 높은 상관 관계를 가짐
     - grief, nervousness는 가장 낮은 상관 관계를 가짐
   - 감정간의 상관 관계   
![image](https://user-images.githubusercontent.com/54783292/97144503-ed1cd880-17a7-11eb-958d-135348691bd8.png)
     - 강도와 관련된 감정들이 강한 양의 상관 관계를 가짐
     - 감정이 반대인 감정끼리는 음의 상관 관계를 가짐
     - 계층적 군집화를 통해 분류체계의 중첩된 구조를 알 수 있음
   - PPCA로 차원 축소

<br>
   
 - 실험
   - 원본 데이터에서 label이 1개 이상인 데이터 93%(train 80%, dev 10%, test 10%)
   - label을 positive, negative, ambiguous, neutral 4가지 범주로 나눔
   - 6개의 그룹으로 나눔
     - anger(anger, annoyance, disapproval)
     - disgust(disgust)
     - fear(fear, nervousness)
     - joy(all positive emotions)
     - sadness(sadness, disappointment, embarrassment, grief, remorse)
     - surprise (all ambiguous emotions)
   - BERT-base 사용

<br>
   
 - 결과   
![image](https://user-images.githubusercontent.com/54783292/97144545-fa39c780-17a7-11eb-8e7a-d0dce5ec71d6.png)