<h3>Integrated Eojeol Embedding for Erroneous Sentence Classification in Korean Chatbots</h3>

 - URL: https://arxiv.org/pdf/2004.05744.pdf

--------------
 - 목적
   - intent가 올바르게 분류되면, 분류된 intent는 input sentence에 대해 entity를 추출하는 것을 쉽게 함
   - intent를 올바르게 분류하기 위해 Integrated Eojeol Embedding(IEE)를 제안함

<br>

- 어절(Eojeol)
   - 공백으로 구분된 한글 Sequence, 하나 이상의 형태소가 들어있음
   - input sentence는 어절 list {w_1, w_2,…w_s}를 얻기 위해 공백으로 tokenize됨
   - 어절 w로 4개의 subword unit list가 생성됨
     - jamo list
     - character list
     - byte-pair encoding (BPE) subunit list
     - morpheme list
- Integrated Eojeol Embedding(IEE)
  - subword unit merge(SUM) network
    - 어절 𝑤가 주어졌을 때 IEE vector를 계산하기 위한 network architecture   
![image](https://user-images.githubusercontent.com/54783292/99926994-fa1cef80-2d86-11eb-976b-855a788d68cc.png)   
     1. subword unit list이 입력됨
     2. SUM network는 각 list item을 embedding vector로 변환하여embedding matrix를 얻음
     3. embedding matrix는 one-dimensional depthwise separable convolution에 적용함
     4. Max-pooling과 LayerNorm을 수행함
     5. Integrated Eojeol Embedding vector는 subword unit-based Eojeol embedding vector를 통해 계산됨
  - IEE를 구성하기 위한 3가지 알고리즘
    - IEE by Concatenation
      - 모든 subword unit-based Eojeol embedding vector 들을 하나의 IEE vector를 형성하도록 concatenation함
    - IEE by Weighted Sum
      - subword unit-based Eojeol embedding vector의 weighted sum으로 정의함
    - IEE by Max Pooling
      - subword unit-based Eojeol embedding vector의 j번째 요소의 maximal value를 IEE의 j번째 요소로 설정함
 - Noise Insertion Methods
   - IEE vector의 성능을 향상시키는 두 가지 노이즈 삽입 방법
     - Jamo dropout
       - Training 단계에서 Jamo dropout probability를 통해 전체 Jamo embedding vector를 masking함
       - 다른 유형의 subword unit에 mask된 jamo가 포함된 경우, 해당 subword unit embedding vector도 masking함
       - input embedding vector의 일부 요소를 dropout으로 masking하는 대신, jamo dropout은 전체 jamo embedding vector를 masking함
       - masked jamo를 포함하는 다른 subword unit을 masking함으로써 subword unit vocab에서 input subword를 알 수 없는 경우 system은 jamo embedding에 집중하는 방법을 배울 수 있음
     - Space-missing sentence generation
       - Trining 단계 전에 한번만 적용됨
       - msp(missing space probability)를 갖는 training corpus를 무작위로 선택하고, 선택된 문장의 모든 공백을 제거한다
       - 공백을 기준으로 어절을 tokenize하기 때문에 morpheme embedding-based방식과 비교하여 eojeol embedding-based 방식은 공백이 없는 문장에 대해 성능이 떨어질 것으로 예상됨 그래서 이 방법 도입

<br>

 - 데이터
   - intent classification corpus
     - 48 intents
     - 127,322개의 문법적으로 정확한 한국어 문장
     - train:validation:test = 8:1:1
   - WF(Well-Formed) corpus
     - test dataset, 12,711개
   - KM(Korean Misspelling) corpus
     - 오류가 있는 문장의 성능을 측정하기 위해 수동으로 주석 추가
     - 46개의 intents (무의미한 문장의 의도인 ood, 응과 같은 짧은 답변 의도를 뜻하는 common 제외)
     - 2,070개의 잘못된 문장을 포함 (각 intent에 대해 annotator가 오류없는 45개의 문장을 작성하고 다음 annotator가 작성된 문장에 오류를 삽입함)
     - testing할 때 사용
   - SM (Space-missing) test corpus
     - WF와 KM corpus를 기반으로 생성됨
     - 필요한 공백이 없는 문장을 성능 평가함(단어 사이의 공백은 0.5 확률로 랜덤하게 제거되며 원래 문장에서 하나 이상의 공백이 제거됨)
     - 14,781개의 문장을 포함
     - testing할 때 사용

<br>
   
 - 실험
   - IEE-based Sentence Classification
     - IEE를 사용하는 sentence classification을 위한 network architecture   
![image](https://user-images.githubusercontent.com/54783292/99927012-0a34cf00-2d87-11eb-8542-b77488cfd873.png)   
      1. 주어진 문장 s의 어절 리스트에서 각 어절에 대한 IEE matrix 𝐸(𝑠)를 얻기 위해 IEE network로 계산함
      2. IEE matrix 𝐸(𝑠)가 계산되면 depthwise separable convolution에 적용함
      3. Max-pooling과 LayerNorm을 수행함
      4. 두 개의 fully connected feed-forward layer에서 final output score vector 𝑂를 얻음
 - 성능 측정
   - Sentence accuracy = (correct sentences) / (total sentences)

<br>
   
 - 결과   
   - IEE를 구성하는 3가지 알고리즘 비교 실험 결과   
![image](https://user-images.githubusercontent.com/54783292/99927033-2f294200-2d87-11eb-841f-fa0b7ed10544.png)   
     - IEEc(IEE by Concatenation), IEEm(IEE by Max-pooling), IEEw(IEE by Weighted Sum)
     - [m]: morpheme, [b]: BPE unit, [j]: Jamo, [c]: Character
     - 두 가지 노이즈 삽입 방법을 모두 사용했을 때, 그렇지 않은 경우보다 크게 향상됨
   - IME와 IBE에 대한 비교 실험 결과   
![image](https://user-images.githubusercontent.com/54783292/99927047-3bad9a80-2d87-11eb-939c-b5727f8be5ca.png)   
     - IME(integrated morpheme embedding), IBE(integrated BPE embedding)
       - IEE-based Sentence Classification과정에서 Eojeol embedding vector를 공급할때, morpheme embedding VECTOR와 BPE unit embedding vector를 공급한다는 점을 빼면 IEE와 동일
     - KM corpus의 경우 어절 기반 embeddin이 형태소와 BPE 기반의 방식에 비해 3% 더 높음
   - 다양한 subword unit으로 구성된 IEEc에 대한 실험 결과   
![image](https://user-images.githubusercontent.com/54783292/99927070-4831f300-2d87-11eb-9638-97efc8e12bdb.png)   
     - Jamo subword를 적용하면 KM corpus에선 성능이 크게 향상됨
     - Jamo subword unit은 pre-trained embedding이 없기 때문에 WF Corpus에서는 상대적으로 성능이 저하
