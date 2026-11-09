
KNN 교재 그림	우유 등급 분류
빨간색 네모와 노란색 별	우유 등급(BAD, MEDIUM, GOOD)
새로운 입력 데이터	등급을 예측할 새로운 우유
가까운 이웃 K개	품질 특성이 비슷한 우유 K개
다수결	가장 많이 나온 우유 등급 선택

이번에는 단순히 코드를 설명하는 데 그치지 않고, 데이터 준비 → 학습 데이터와 테스트 데이터 분리 → KNN 모델 생성 → 학습 → 예측 → 정확도 평가 순서로 이해하면 좋아.

1. 우유 데이터셋 확인하기
   먼저 제공한 milknew.csv 파일을 살펴보면 총 1,059개의 데이터와 8개의 열로 구성되어 있어.
   GitHub+1

각 열의 의미는 다음과 같아.
열 이름	의미	데이터 형태
pH	우유의 산도	숫자
Temprature	우유의 온도	숫자
Taste	맛 특성	0 또는 1
Odor	냄새 특성	0 또는 1
Fat	지방 특성	0 또는 1
Turbidity	탁도 특성	0 또는 1
Colour	우유의 색상 수치	숫자
Grade	우유 등급	high, medium, low

여기서 가장 중요한 것은 Grade 속성으로 우유 등급이 있어.

우유의 품질 특성을 입력하면 KNN이 가장 비슷한 우유들의 등급을 참고하여 새로운 우유의 등급을 예측하는 것이야.

2. 첫 번째 단계: 데이터 다운로드하기
   교재의 노트북 첫 번째 코드야.
   import osimport urllib.request# 1. data 폴더 생성os.makedirs("./data", exist_ok=True)# 2. CSV 파일 다운로드url = "https://raw.githubusercontent.com/seongui2030/Section2/main/milknew.csv"urllib.request.urlretrieve(url, "./data/milknew.csv")print("다운로드 완료!")

코드의 역할

1. os와 urllib.request를 불러온다.
2. ./data라는 폴더를 만든다.
3. GitHub에 저장된 milknew.csv 파일을 다운로드한다.
4. 다운로드한 파일을 data 폴더에 저장한다.
   즉, KNN 분석에 사용할 우유 데이터를 준비하는 단계야.
5. 두 번째 단계: 데이터 읽어오기
   import pandas as pddf = pd.read_csv('/content/data/milknew.csv')df.head(3)

코드의 역할

- pandas: 표 형태의 데이터를 처리하는 라이브러리
- read_csv(): CSV 파일을 읽어오는 기능
- df: 읽어온 데이터를 저장하는 데이터프레임 변수
- head(3): 처음 3개 행을 확인하는 기능
  실제 데이터의 일부는 다음과 같아.
  pH	온도	맛	냄새	지방	탁도	색상	등급
  6.6	35	1	0	1	0	254	high
  6.6	36	0	1	0	1	253	high
  8.5	70	1	1	1	1	246	low

여기서 확인할 점은 우유의 품질을 나타내는 7개의 특성과 최종 등급 1개가 있다는 것이야.
4. 세 번째 단계: 입력 데이터(X)와 정답(y) 분리하기
교재의 핵심 코드야.
X = df.drop(columns=['Grade'])y = df['Grade']

이 부분은 KNN을 이해할 때 매우 중요해.

머신러닝에서는 일반적으로 데이터를 다음 두 가지로 나눠.

① X: 입력 데이터(독립변수)
X = df.drop(columns=['Grade'])

Grade 열을 제외한 나머지 7개 특성을 입력 데이터로 사용한다는 뜻이야.

즉,

- pH
- Temprature
- Taste
- Odor
- Fat
- Turbidity
- Colour
  이 7가지 정보를 이용해 우유 등급을 예측하는 거야.

② y: 정답 데이터(종속변수)
y = df['Grade']

우유의 실제 등급을 정답으로 지정하는 코드야.

정답은 세 종류야.

- high
- medium
- low
  X와 y의 관계 이해하기
  X : 우유의 품질 특성 7개

pH, 온도, 맛, 냄새, 지방, 탁도, 색상

y : 실제 우유 등급

HIGH / MEDIUM / LOW

X의 특성을 이용하여 y의 등급을 학습하고 예측한다.
쉽게 말하면 X는 문제이고, y는 정답이라고 생각하면 돼.
5. 네 번째 단계: 학습 데이터와 테스트 데이터 분리하기
교재 코드:
from sklearn.model_selection import train_test_splitX_train, X_test, y_train, y_test = train_test_split(X, y)print('train data 개수:', X_train.shape)print('test data 개수:', X_test.shape)

이 코드는 전체 데이터를 학습용과 평가용으로 나누는 과정이야.

왜 나눌까?

모델이 이미 학습한 데이터로만 평가하면 새로운 데이터에 대해서도 잘 예측하는지 확인하기 어렵기 때문이야.

이번 실습에서는 다음과 같이 나뉘었어.
구분	데이터 개수	비율
학습 데이터	794개	약 75%
테스트 데이터	265개	약 25%
전체	1,059개	100%

각 변수의 의미는 다음과 같아.

- X_train: 학습에 사용할 우유 품질 특성
- y_train: 학습 데이터의 실제 우유 등급
- X_test: 예측 성능을 평가할 우유 품질 특성
- y_test: 테스트 데이터의 실제 우유 등급
  즉, 794개로 KNN 모델을 준비하고, 나머지 265개로 예측 성능을 확인하는 거야.

6. 다섯 번째 단계: KNN 모델 생성하기
   교재의 코드:
   from sklearn.neighbors import KNeighborsClassifiermodel = KNeighborsClassifier(n_neighbors=5)

이 코드는 KNN 분류 모델을 생성하는 단계야.

각 부분의 의미를 살펴보자.
코드	의미
KNeighborsClassifier	KNN 분류 알고리즘 불러오기
model	생성한 모델의 이름
n_neighbors=5	가장 가까운 이웃 5개를 사용

여기서 가장 중요한 것은 n_neighbors=5야.

앞에서 교재 그림으로 공부했던 K=1, K=3과 같은 개념이야.

이번 우유 분류에서는 K=5, 즉 가장 가까운 우유 5개를 참고해서 새로운 우유의 등급을 결정하는 거야.

예를 들어 새로운 우유와 가장 가까운 5개의 우유 등급이 다음과 같다고 가정해 보자.
이웃 순서	우유 등급
1번째	HIGH
2번째	MEDIUM
3번째	HIGH
4번째	LOW
5번째	HIGH

가장 가까운 이웃 5개의 투표 결과
HIGH3표

MEDIUM
1표

LOW
1표

최종 예측: HIGH
가장 많은 표를 얻은 HIGH 등급으로 분류한다.

이것이 KNN의 다수결 분류 원리야.

※ 위 투표 예시는 KNN 원리를 설명하기 위한 가상 데이터이며, 실제 노트북의 개별 예측 결과는 아니야.
7. 여섯 번째 단계: KNN 모델 학습하기
교재의 코드:
model.fit(X_train, y_train)

이 코드는 앞에서 만든 KNN 모델에 학습 데이터를 입력하는 단계야.

실행하면 다음 작업이 이루어져.

1. 794개의 우유 품질 데이터를 준비한다.
2. 각 데이터에 연결된 실제 등급을 확인한다.
3. 새로운 우유와 기존 우유 사이의 거리를 비교할 수 있도록 학습 데이터를 저장한다.
4. 이후 새로운 데이터가 입력되면 가장 가까운 5개를 찾아 등급을 예측할 준비를 한다.
   여기서 중요한 차이점이 있어.

KNN은 K-means처럼 군집 중심점을 계산하는 알고리즘이 아니야.

기존 데이터와 정답을 바탕으로 새로운 데이터의 이웃을 찾아 분류하는 방식이야.
8. 일곱 번째 단계: 모델 성능 평가하기
교재의 마지막 코드:
model.score(X_test, y_test)

이 코드는 테스트 데이터 265개를 이용하여 KNN 모델의 예측 정확도를 평가하는 단계야.

작동 원리는 다음과 같아.

1. 테스트 데이터 265개를 모델에 입력한다.
2. KNN이 각 우유의 등급을 예측한다.
3. 예측 결과와 실제 등급을 비교한다.
4. 전체 테스트 데이터 중 정확하게 예측한 비율을 계산한다.
   교재 노트북에 기록된 실행 결과는 다음과 같아.
   GitHub

KNN 모델의 예측 정확도
97.74%
교재 실행 결과
계산식:
\[
0.977358 \times 100
=97.7358\%
\]

소수점 둘째 자리까지 반올림하면 약 97.74%야.

즉, 테스트 데이터 265개를 대상으로 약 97.74%의 정확도를 기록했다는 의미야.

다만 데이터 분할에 random_state가 지정되지 않았기 때문에 다시 실행하면 학습·테스트 데이터 구성이 달라져 정확도도 달라질 수 있어.

9. 전체 KNN 알고리즘의 절차 정리
   지금까지 설명한 내용을 전체 흐름으로 연결해 보자.
   우유 등급 분류 KNN 처리 과정
   1단계. 데이터 다운로드

milknew.csv 파일 가져오기

2단계. 데이터 읽기

pandas로 CSV 데이터 불러오기

3단계. X와 y 분리

X = 우유 품질 특성 7개

y = 우유 등급 3종류

4단계. 데이터 분할

학습 794개 / 테스트 265개

5단계. KNN 모델 생성

KNeighborsClassifier(n_neighbors=5)

6단계. 모델 학습

model.fit(X_train, y_train)

7단계. 모델 평가

model.score(X_test, y_test)

약 97.74%
최종 정확도

10. 교재의 전체 실행 코드
    아래는 교재의 코드를 하나로 연결한 형태야.

# 1. 데이터 다운로드import osimport urllib.requestos.makedirs("./data", exist_ok=True)url = "https://raw.githubusercontent.com/seongui2030/Section2/main/milknew.csv"urllib.request.urlretrieve(url, "./data/milknew.csv")# 2. 데이터 읽기import pandas as pddf = pd.read_csv("./data/milknew.csv")# 3. 입력 데이터(X)와 정답(y) 분리X = df.drop(columns=["Grade"])y = df["Grade"]# 4. 학습 데이터와 테스트 데이터 분리from sklearn.model_selection import train_test_splitX_train, X_test, y_train, y_test = train_test_split(X, y)# 5. KNN 알고리즘 불러오기from sklearn.neighbors import KNeighborsClassifier# 6. KNN 모델 생성 (이웃 5개)model = KNeighborsClassifier(n_neighbors=5)# 7. 모델 학습

11. 실제 분석에서 주의할 점
    교재 코드를 이해하는 것과 실제로 좋은 모델을 만드는 것은 조금 달라.

특히 이번 데이터는 pH, 온도, 색상처럼 숫자의 범위가 서로 다르고, 맛·냄새·지방·탁도처럼 0과 1로 표현된 변수도 있어.

KNN은 거리 계산을 이용하기 때문에 값의 범위가 큰 온도나 색상 특성이 거리 계산에 더 큰 영향을 줄 수 있어.

따라서 실제 모델의 성능을 개선하려면 StandardScaler 또는 MinMaxScaler를 이용해 특성별 크기를 조정하는 방법을 검토하는 것이 좋아.

또한 테스트 데이터와 학습 데이터에 같은 우유가 중복되어 들어가지 않도록 데이터 분할과 중복 여부도 확인해야 해.

12. 최종 시험 핵심 정리
    코드	기억해야 할 의미
    pd.read_csv()	데이터 불러오기
    df.drop(columns=['Grade'])	입력 데이터 X 만들기
    df['Grade']	정답 데이터 y 만들기
    train_test_split()	학습용과 테스트용 데이터 분리
    KNeighborsClassifier(n_neighbors=5)	K=5인 KNN 분류 모델 생성
    model.fit()	학습 데이터 입력
    model.score()	예측 정확도 평가

가장 중요한 세 가지를 기억하자.

1. KNN은 지도 학습 알고리즘이다.
2. K는 가장 가까운 이웃의 개수를 의미한다.
3. 주변 K개 데이터의 다수결로 새로운 데이터의 등급을 예측한다.
   이번 우유 분류 문제에서는 K=5를 사용하므로 가장 가까운 우유 5개의 등급을 참고해 HIGH, MEDIUM, LOW 중 하나로 분류하는 것이 핵심이야.
