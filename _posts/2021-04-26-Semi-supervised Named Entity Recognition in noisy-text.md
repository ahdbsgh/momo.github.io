# Semi-supervised Named Entity Recognition in noisy-text

- [https://github.com/napsternxg/TwitterNER](https://github.com/napsternxg/TwitterNER)
- [https://www.aclweb.org/anthology/W16-3927.pdf](https://www.aclweb.org/anthology/W16-3927.pdf)

NER 할때 주로 쓰는 인코딩 방식은 Begin-Inside-Outside (BIO)방식이다. 

BIEOU = BILOU 두개 같은 표현

Begin-Inside-End-Outside-Unigram (BIEOU)

Begin-Inside-Last-Outside-Unigram (BILOU)

HMM 정리 [https://lovit.github.io/nlp/2018/09/11/hmm_based_tagger/](https://lovit.github.io/nlp/2018/09/11/hmm_based_tagger/)

CRF 정리 [https://lovit.github.io/nlp/2018/09/13/crf_based_tagger/](https://lovit.github.io/nlp/2018/09/13/crf_based_tagger/)

               [https://ratsgo.github.io/machine learning/2017/11/10/CRF/](https://ratsgo.github.io/machine%20learning/2017/11/10/CRF/)

int 

기존의 NER방식은 적절한 구문으로 이루어진 뉴스코퍼스 데이터를 기반으로 구축됨.

하지만 이렇게 하면 트위터같이 노이즈가 심한 데이터에대해서는 정확한 결과를 얻기 힘듬 

이 논문은 CRF를 기반으로한 BIEOU인코딩 체계를 사용하였고 

각종 Feature Engineering을 진행하였다. 

Workshop on “Noisy User-generated Text” (WNUT)

ST는 얘네가 전에 냈던 WNUT 2016 NER 대회에 제출했던거

SI는 얘네가 제시했던 semi-supervised 한 NER모델 → 이에대해 개선사항을 제시

rf → random feature

data 

WNUT 2016 Dataset

개체명을 가진 트위터 text데이터 

![Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled.png](Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled.png)

![Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%201.png](Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%201.png)

3. Feature Engineering

- Regex features [RF]

![Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%202.png](Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%202.png)

- Gazetteers [GZ]

![Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%203.png](Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%203.png)

- Word representations [WR]

![Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%204.png](Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%204.png)

- Word clusters [WC]

![Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%205.png](Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%205.png)

- Additional features

            lexical tokens [LT]

      global features [GF]

![Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%206.png](Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%206.png)

- Random up-sampling with feature dropout [RSFD]

![Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%207.png](Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%207.png)

4. NER classification algorithm

CRF 모델사용 모델은 L2 norm, SGD사용해서 훈련시킴 

[ST]

 lexical tokens [LT], Regex features [RF], Random up-sampling with feature dropout [RSFD] 사용 해서 supervised learning함 

[SI]

[SI]는 ST의 문제점을 개선한것 

ST는 한가지 단점이 있었는데 이는 overfitting 이다. 

이 overfitting을 막기 위해 semi-supervised를 사용했다. 

unlabeled data → train, dev, test병합해서 만든 레이블링되지 않은 데이터 

unlabeled data를 word embeding후 클러스터링 해서 기존의 데이터에 추가 학습

→ 트윗에 있는 토큰이 증가 →레이블이 지정되지 않은 새로운 테스트 데이터를 사용하여 단어 표현과 클러스터를 개선

→ unlabeled data는 regularization factor 역할을 하여 훈련 데이터에 과적합되는 것을 방지한다. 

[TDTE]는 train, dev, test의 text를 병합

[TD] 테스트 데이터를 뺀 나머지 train dev text 병합

[SI]에 대한 개선사항 

→gazetteer [GZ] 기능을 추가하면 분류 정확도가 크게 향상

[WCBTP] → brown clustesrs 

[WRFTC] → fine-tuned word representations based clusters

[WCCC] → Clark clusters 

위의 4개 추가 하니까 F1score 향상 

result

![Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%208.png](Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%208.png)

![Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%209.png](Semi-supervised%20Named%20Entity%20Recognition%20in%20noisy-%20be5cb9a7f78d447b97cf6c01764c293f/Untitled%209.png)