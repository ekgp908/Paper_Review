<h3>BERT(Bidirectional Encoder Representations from Transformers)</h3>

 - URL : https://keep-steady.tistory.com/19

--------------

 - Token Embeddings
   - Word Piece 임베딩 방식
     - 가장 긴 길이의 sub-word를 하나의 단위로 만듦
     - 자주 등장하지 않는 단어는 sub-word로 쪼개지게 됨
     - 모든 언어에 적용 가능
     - OOV 처리에 효과적, 정확도 상승
 - Sentence Embeddings
   - 두 개의 문장을 문장 구분자와 함께 합쳐 넣음
   - 입력 길이를 512subword 이하로 제한함: 길이가 길수록 학습 시간은 제곱으로 증가함
 - Position Embedding
   - Transformer 모델의 인코더 사용: CNN, RNN이 아닌 Self-Attention 모델 사용
      ![image](/uploads/cfe7d515b759f2a24ac2837e08057093/image.png)
     - 입력의 위치에 대해 고려하지 X -> 입력 토큰의 위치 정보를 줘야함
     - Sinusoid 함수한 Positional encoding을 변형해 Position encoding 사용
 - 위 3가지 임베딩을 취함하여 하나의 임베딩 값으로 만듦
--------------
 - 언어 모델링 구조(Pre-training Bert)
   - 거대 encoder가 입력 문장들을 임베딩하여 언어 모델링 함
   - MLM(Masked Language Model)과 NSP(Next Sentence Prediction) 사용
   - 말뭉치는 MLM. NSP를 적용하기 위해 스스로 라벨을 만들고 수헹함 -> 준지도학습
   - MLM
     - 입력 문장에서 임의로 Token을 masking하고 그 Token을 맞추는 방식
     - 입력의 15% 단어를 [MASK] Token으로 바꿔주어 마스킹함 -> [MASK]가 어떤 단어인지 예측
     - 미세 조정 시 올바른 예측을 돕도록 마스킹에 노이즈를 섞음
![image](/uploads/6a0cd1ae2d376f8c1f80052431cdbe0d/image.png)
   - NSP
     - 두 문장이 주어졌을때 두 번째 문장이 첫 번째 문장의 바로 다음에 오는 문장인지 여부를 예측하는 방식 -> 두 문장은 [SEP]로 구분
     - NLI와 QA의 파인튜닝을 위해 두 문장이 연관이 있는지를 맞추도록 학습
     - 문맥과 순서를 언어모델이 학습할 수 있음
<br>

 - 학습된 언어모델 전이학습(Transfer Learning)
   - 전이학습은 라벨이 주어지므로 지도학습
   - BERT의 출력에 추가적인 모델을 쌓아 만듦