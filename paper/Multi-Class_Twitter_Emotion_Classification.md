<h3>Multi-Class Twitter Emotion Classification: A New Approach</h3>

 - URL: https://www.researchgate.net/publication/269670995_Multi-Class_Twitter_Emotion_Classification_A_New_Approach   

--------------
 - 목적
   - Emotion analysis과 opinion mining

<br>
    
 - 방법
   - 모델 학습
     - multi-class SVM kernel을 기반으로 함
   - feature set
     - Stanford Corenlp Package로 전처리
     - 분류할 때도 SVM 사용
     - syntactic, semantic, contextual 모두 고려함   
     - Porter Stemmer 사용, 불용어 제거   
![image](https://user-images.githubusercontent.com/54783292/95409667-1c84b600-095d-11eb-9519-bb62e2a64e95.png)
     - 3, 4, 8, 9: Stanford Penn-Bank POS-tagger로 태깅
     - 6: 문맥 파악, 앞 뒤 문장 확인
     - 7: Word-net Effect emotion lexicon + Stanford POS-tagger
     - 11: 이모티콘 사용, 앞뒤 문장에 기여

<br>
    
 - 실험
   - 데이터
     - 305,310 multi-lingual tweets(happiness, sadness, anger, disgust, surprise, fear)
     - Leave-One-Out Cross-validation 사용   
   - 성능측정   
     - accuracy
   - 결과   
![image](https://user-images.githubusercontent.com/54783292/95409685-26a6b480-095d-11eb-8dce-bebf6ddcaf0f.png)
![image](https://user-images.githubusercontent.com/54783292/95409696-2ad2d200-095d-11eb-9d62-1269eda18adc.png)
     - feature를 전부 사용했을 때의 성능이 가장 좋음