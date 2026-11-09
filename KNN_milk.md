
# 우유 품질 등급 분류를 위한 KNN(K-Nearest Neighbor) 알고리즘 실습

## 1. 우유 데이터셋 확인하기

먼저 제공한 `milknew.csv` 파일을 살펴보면 총 **1,059개의 데이터와 8개의 열**로 구성되어 있다.

| 열 이름    | 의미             | 데이터 형태       |
| :--------- | :--------------- | :---------------- |
| pH         | 우유의 산도      | 숫자              |
| Temprature | 우유의 온도      | 숫자              |
| Taste      | 맛 특성          | 0 또는 1          |
| Odor       | 냄새 특성        | 0 또는 1          |
| Fat        | 지방 특성        | 0 또는 1          |
| Turbidity  | 탁도 특성        | 0 또는 1          |
| Colour     | 우유의 색상 수치 | 숫자              |
| Grade      | 우유 등급        | high, medium, low |

# 우유 품질 등급 분류를 위한 KNN(K-Nearest Neighbor) 알고리즘 실습

## 1. 우유 데이터셋 확인하기

먼저 제공한 `milknew.csv` 파일을 살펴보면 총 **1,059개의 데이터와 8개의 열**로 구성되어 있다.

| 열 이름    | 의미             | 데이터 형태       |
| :--------- | :--------------- | :---------------- |
| pH         | 우유의 산도      | 숫자              |
| Temprature | 우유의 온도      | 숫자              |
| Taste      | 맛 특성          | 0 또는 1          |
| Odor       | 냄새 특성        | 0 또는 1          |
| Fat        | 지방 특성        | 0 또는 1          |
| Turbidity  | 탁도 특성        | 0 또는 1          |
| Colour     | 우유의 색상 수치 | 숫자              |
| Grade      | 우유 등급        | high, medium, low |

여기서 가장 중요한 속성은 우유 등급을 나타내는 `Grade`이다. 우유의 품질 특성을 입력하면 KNN이 가장 비슷한 우유들의 등급을 참고하여 새로운 우유의 등급을 예측하게 된다.

---

## 2. 첫 번째 단계: 데이터 다운로드하기

교재의 노트북 첫 번째 코드이다.

```python
import os
import urllib.request

# 1. data 폴더 생성
os.makedirs("./data", exist_ok=True)

# 2. CSV 파일 다운로드
url = "[https://raw.githubusercontent.com/seongui2030/Section2/main/milknew.csv](https://raw.githubusercontent.com/seongui2030/Section2/main/milknew.csv)"
urllib.request.urlretrieve(url, "./data/milknew.csv")
print("다운로드 완료!")
```

여기서 가장 중요한 속성은 우유 등급을 나타내는 `Grade`이다. 우유의 품질 특성을 입력하면 KNN이 가장 비슷한 우유들의 등급을 참고하여 새로운 우유의 등급을 예측하게 된다.

---

## 2. 첫 번째 단계: 데이터 다운로드하기

교재의 노트북 첫 번째 코드이다.

```python
import os
import urllib.request

# 1. data 폴더 생성
os.makedirs("./data", exist_ok=True)

# 2. CSV 파일 다운로드
url = "[https://raw.githubusercontent.com/seongui2030/Section2/main/milknew.csv](https://raw.githubusercontent.com/seongui2030/Section2/main/milknew.csv)"
urllib.request.urlretrieve(url, "./data/milknew.csv")
print("다운로드 완료!")
```
