
# 한국어 임배딩 1,2장 정리

## 1.1 임배딩이란

"만약 컴퓨터가 인간을 속여 자신을 마치 인간인 것처럼 믿게 할 수 있다면 컴퓨터를 '인텔리전트' 히디고 부를 만한 가치가 충분히 있다"<br>
> 엘런 튜링

* 컴퓨터는 어디까지나 빠르고 효율적인 '계산기'일 뿐이다.<br> 
* 컴퓨터는 인간이 사용하는 자연어(natural language)를 있는 그대로 이해(x) 숫자를 계산한다. <br>

자연어 처리 분야에서 **임베딩**이란, 사람이 쓰는 자연어를 기계가 이해할 수 있는 숫자의 나열인 **벡터**로 바꾼 결과 혹은 일련의 과정을 의미한다. <br>
단어나 문장 가각을 벡터로 변환해 **벡터 공간**으로 '끼워 넣는다'는 의미에서 임베딩이라는 이름이 붙었다. <br>
<br>

## 1.2 임베딩의 역활
임베딩은 다음 역할을 수행할 수 있다.
* 단어/문장 간 관련도 계산
* 의미적/문법적 정보 함축
* **전이 학습**

### 1.2.1 단어/문장 간 관련도 계산
쿼리 단어별로 벡터간 유사도 측정 기법의 일종인 코사인 유사도 사용(Word2Vec)
<img width="1016" alt="스크린샷 2020-11-01 오후 9 39 29" src="https://user-images.githubusercontent.com/45013435/97803250-39768580-1c8c-11eb-9767-8900861a081f.png">

표에서 관찰할수 있는 것처럼 **희망**과 코사인 유사도가 가장 높은 것은 **소망** 이라는 단어다. 마찬가지로 **절망** 은 **체념** 과 관련성이 높다 자연어일 때는 불가능했던 코사인 유사도 계산이 임베딩 덕분에 가능해졌다. <br><br>
임베딩을 수행하면 벡터 공간을 기하학적으로 나타낸 시각화 역시 가능하다. 

![IMG_452B36CA3EC8-1](https://user-images.githubusercontent.com/45013435/97803397-01237700-1c8d-11eb-8be5-0f215837b07f.jpeg)

### 1.2.2 의미/문법정보 함축
임베딩은 벡터인 만큼 사친연산이 가능하다. 단어 벡터 간 덧셈/뺄셈을 통해 단어들 사이의 의미적, 문법적 관계를 도출해낼 수 있다. <br>
구체적으로 **첫 번째 단어 벡터 - 두 번째 단어벡터 + 세 번째 단어 벡터를** 계산할 수 있다. -> ex) 아들 - 딸 + 소녀 = 소년
<br>
위의 예시가 성립하면 성공적인 임베딩이라고 볼 수 있다. 이렇게 단어 임베딩을 평가하는 방법을 **단어 유추 평가(word analogy test)** 라고 부른다.  

### 1.2.3 전이 학습

임베딩은 다른 딥러닝 모델의 입력값으로 자주 쓰인다. <br>
품질이 좋은 임베딩을 쓰면 문서 분류 정확도와 학습 속도가 올라간다.
* 임베딩을 다른 딥러닝 모델의 입력값으로 쓰는 기법을 **전이 학습** 이라고 한다.
&nbsp; 전이 학습은 사람의 학습과 비슷한 점이 있다. 사람은 무언가를 배울 때 제로에서 시작하지 않는다. 평생 쌓아온 지식을 바탕으로 새로운 사실을 빠르게 이해해한다 전이 학습 모델 역시 제로(0)부터 시작하지 않는다.
대규모 말뭉치를 활용해 임베딩을 미리 만들어 놓는다. 임베딩에는 의미적, 문법적 정보 등이 녹아 있다. 이 임베딩을 입력값으로 쓰는 전이 학습 모델은 문서 분류라는 task를 빠르게 잘 할 수 있게 된다.
&nbsp; 예시로 하나의 모델을 만든다(양방향 LSTM에 어텐션 메커니즘) 이 딥러닝 모델의 입력값은 단어 임베딩 기법의 일종인 FastText임베딩(100차원)을 사용했다. FastText임베딩은 59만 건에 이르는 한국어 문서를 미리 학습했다. 이 모델의 작동 방식을 간단하게 설명하면
* 이 영화 꿀잼 + 긍정(positive)
* 이 영화 노잼 + 부정(negative)
&nbsp; 이 전이 학습 모델은 문장을 입력받으면 해당 문장이 긍정인지 부정인지를 출력한다. 첫 번째 문장의 경우 이, 영화, 꿀잼 임베딩 세개가 순차적으로 입력된다. 두 번째 문장은 이, 영화, 노잼 임베딩이 차례로 들어간다. 이후 모델은 이 입력 벡터들을 계산해 문장의 극성을 예측한다.
<br>
e(이)  = [-0.20195.....-0.0674]<br>
e(영화) = [-0.0795.....0.11161]<br>
e(꿀잼) = [-0.08885.....0.02243]<br>
e(노잼) = [-0.009156.....0.01423]<br>

모델의 학습 과정별 **단순정확도**를 나타낸 것이다. 예시로 네이버 영화 리뷰를 사용했을 경우

<img width="737" alt="스크린샷 2020-11-01 오후 10 27 57" src="https://user-images.githubusercontent.com/45013435/97804182-8446cc00-1c91-11eb-9faa-1b75376d4648.png">

파란색 선으로 나타나 FastText는 모델 입력값으로 FastText임베딩을 사용한 것이다. Random은 입력 벡터를 랜덤 초기화한 뒤 모델을 학습한 방식이다. 즉 Random은 학습을 제로부터 시작한 모델이다.<br>
FastText임베딩을 사용한 모델의 성능이 시종일관 좋다 임베딩의 품질이 좋으면 수행하려는 task의 성능 역시 올라간다. **임베딩이 중요한 이유**이다.
<br>

다음은 문서 극성 분류 모델의 학습 과정별 **학습손실** 을 그래프로 나타낸 것이다.  
<img width="745" alt="스크린샷 2020-11-01 오후 10 33 00" src="https://user-images.githubusercontent.com/45013435/97804294-38e0ed80-1c92-11eb-9ae5-47ea8f4ae050.png">



FastText임베딩을 사용한 모델의 학습 손실이 Random보다 작고 

FastText임베딩을 사용한 모델의 성능이 시종일관 좋다 임베딩의 품질이 좋으면 수행하려는 task의 성능 역시 올라간다. **임베딩이 중요한 이유**이다..






## 4.5 Glove

Gloce는 미국 스탠포드대학교연구팀에서 개발한 단어 임베딩 기법이다. Word2Vec과 잠재의미 분석 두 기법의 단점을 극복하고자 했다. 담재 의미 분석은 말뭉치 전체의 통계량을 모두 활용할 수 있지만, 그 결과물로 단어 간 유사도를 측정하기는 어렵다.<br>
Word2Vec 기법이 단어 벡터 사이의 유사도를 측정하는 데는 LSA 보다 유리하자만 사용자가 지정한 위도우 내의 로컬 문백만 학습하기 때문에 말뭉치 전체의 통계 정보는 반영되기 어렵다는 단점을 지닌다.

### 4.5.1 모델 기본 구조

임베딩된 두 단어 벡터의 내적이  말뭉치 전체에서의 둥시 등장 빈도의 로그 값이 되도록 **목적함수** 를 정의했다.<br>
**'임배딩된 단어 벡터 간  유사도 측정을 수월하게 하면서도 말뭉치 전체의 통계 정보를 좀더 잘 반영해보자'** Glove가 지향하는 핵심 목표라 말할 수 있을 것이다.<br>
Glocve는 우선 학습 말뭉치를 대상으로 **단어-문백 행렬 A**를 만드는 것에서부터 학습을 시작한다.

## 4.6 Swivel

Swivel은 구글 연구 팀이 발표한 행렬 분해 기반의 단어 임베딩 기법이다.

### 4.6.1 모델 기본 구조

Swivel은 PMI 행렬을 분해한다는 점에서 단어-문백 행렬을 분해하는 Glove와 다르다
Swivel은 목적함수를 PMI의 단점을 극복할 수 있도록 설계했다는 점 또한 눈에 띈다.



## 4.8 가중 임베딩
단어 임베딩을 문장 수준 임베딩으로 확장하는 방법을 설명한다.

### 4.8.1 모델 개요
문서 내 단어의 등장은 저자가 생가가한 주제에 의존한다고 가정했다. 다시 말해 주제에 따라 단어의 사용 양상이 달라진다는 것이다. 이를 위해 **주제 벡터라**는 개념을 도입했다.


# 5. 문장 수준 임베딩
5장에서는 문장 수준 임베딩을 다룬다. 행렬 분해, 확률 모형, 누럴 네트워크 기반 모델 등 세 종류를 소개한다.

## 5.1 잠재의 분석
5장에서 소개할 잠재 의미 분석은 단어-문서 행렬이나 TF-IDF 행렬ㅇ에 SVD를 시행하고, 축소된 이 행렬에서 문서에 대응하는 벡터를 취해 문서 임베딩을 만드는 방식이다. 단어-행렬과 TF-IDF등 주요 개념은 관련 장을 
참고한다

## 5.3 잠재 디리크레 할당
잠재 디리클레 할당이란 주어진 문서에 대하여 각 문서에 어떤 토픽들이 존재하는지에 대한 확률 모형이다. 말뭉치 이면에 잠재된 토픽을 추출한다는 의미에서 토픽 모델링이라 부르기도 한다. 문서를 토픽 확률 분포로 나타내 각각을 벡터화한다는 점에서 LDA를 임베딩 기법의 일종으로 이해할 수도 있다.

### 모델 개요
LDA는 토픽별로 단어의 분포, 문서별 토픽의 분포를 모두 추정해낸다.
<br> 우선 LDA는 특정 토픽에 특정 단어가 나타날 확률을 내어 준다.LDA는 문서가 생성되는 과정을 확률 모형으로 모델링한 것이기 때문이다. 
LDA는 망물치 이면에 존재하는 정보를 추론해낼 수 있다. LDA에 잠재라는 이름이 붙은 이유다










