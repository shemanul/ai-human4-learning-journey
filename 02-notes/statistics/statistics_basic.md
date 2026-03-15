## AI/머신러닝 위한 선행 지식
```
                데이터
                   │
                   │
        ┌──────────┴──────────┐
        │                     │
      통계                  선형대수
   (데이터 이해)           (모델 계산)
        │                     │
        │                     │
   평균 / 분산                 벡터
   상관계수                    행렬
   이상치                      내적
        │                     │
        └──────────┬──────────┘
                   │
                 확률
            (예측의 기반)
                   │
                   │
               머신러닝
```

**1. 통계**
    - 주요 항목
      -  평균, 중앙값, 분산, 표준편차, 최빈값
      -  공분산, 사분위, 산포도
      -  선형 비선형 데이터, 상관계수
      -  이상탐지
   - 목적 :데이터 구조 파악, 관계 찾기, 이상치 발견
### Python 통계 관련 함수 정리

#### 1. 대표값 (Central Tendency)

데이터의 대표적인 값을 찾는 방법

| 함수                  | 설명  | 예                 |
| ------------------- | --- | ----------------- |
| `np.mean()`         | 평균  | `np.mean(data)`   |
| `np.median()`       | 중앙값 | `np.median(data)` |
| `statistics.mode()` | 최빈값 | `mode(data)`      |

```python
import numpy as np
from statistics import mode

data = [1,2,2,3,4]

np.mean(data)
np.median(data)
mode(data)
```

---

#### 2. 데이터 퍼짐 정도 (Spread)

데이터가 얼마나 퍼져있는지 측정

| 함수                  | 설명     |
| ------------------- | ------ |
| `np.var()`          | 분산     |
| `np.std()`          | 표준편차   |
| `scipy.stats.iqr()` | 사분위 범위 |

```python
import numpy as np
from scipy.stats import iqr

data = [1,2,3,4,5]

np.var(data)
np.std(data)
iqr(data)
```

---

#### 3. 분위수 (Quantile)

데이터를 비율 기준으로 나누는 값

| 함수                | 설명  |
| ----------------- | --- |
| `np.quantile()`   | 분위수 |
| `np.percentile()` | 백분위 |

```python
np.quantile(data,0.25)
np.quantile(data,0.5)
np.quantile(data,0.75)
```

---

#### 4. 범위 계산

범위 = 최대값 − 최소값

| 함수         | 설명  |
| ---------- | --- |
| `np.max()` | 최대값 |
| `np.min()` | 최소값 |
| `np.ptp()` | 범위  |

```python
np.max(data)
np.min(data)
np.ptp(data)
```

---

#### 5. 공분산과 상관관계

두 변수 간의 관계 분석

| 함수              | 설명   |
| --------------- | ---- |
| `np.cov()`      | 공분산  |
| `np.corrcoef()` | 상관계수 |

```python
np.cov(x,y)
np.corrcoef(x,y)
```

상관계수 해석

| 값  | 의미       |
| -- | -------- |
| 1  | 완전 양의 관계 |
| 0  | 관계 없음    |
| -1 | 완전 음의 관계 |

---

## 핵심 정리

통계 분석 기본 순서

1. 평균(mean)
2. 중앙값(median)
3. 표준편차(std)
4. 분위수(quantile)
5. 이상치 확인

### 이상치 제거 방법 정리

#### 1. 이상치(outlier)란

다른 데이터와 크게 차이가 나는 값

예

```
10, 12, 11, 13, 12, 200
```

200 → 이상치

---

### 2. 이상치 제거 방법

#### 방법 1 : 데이터 삭제

```python
data = data[data < threshold]
```

장점

* 간단

단점

* 데이터 손실

---

#### 방법 2 : NaN 처리

```python
data[data > threshold] = np.nan
```

장점

* 통계 계산 시 자동 제외

---

#### 방법 3 : IQR 방법 (가장 많이 사용)

#### 공식

```
IQR = Q3 - Q1
```

이상치 기준

```
Lower = Q1 - 1.5 × IQR
Upper = Q3 + 1.5 × IQR
```

#### Python 코드

```python
import numpy as np

Q1 = np.quantile(data,0.25)
Q3 = np.quantile(data,0.75)

IQR = Q3 - Q1

lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR

data = data[(data >= lower) & (data <= upper)]
```

---

### 방법 4 : Z-score 방법

공식

```
Z = (x - 평균) / 표준편차
```

조건

```
|Z| > 3 → 이상치
```

---

### ***이상치 처리 방법 비교***

| 방법      | 특징       |
| ------- | -------- |
| 삭제      | 가장 단순    |
| NaN     | 데이터 보존   |
| IQR     | 가장 많이 사용 |
| Z-score | 정규분포 데이터 |

---

# 핵심 정리

이상치 제거 추천 순서

1. IQR
2. Z-score
3. NaN 처리
4. 삭제

---
## Python 그래프 그리는 방법

Python에서 그래프는 보통 **matplotlib** 라이브러리를 사용한다.

```python
import matplotlib.pyplot as plt
```



### 1. 선 그래프 (Line Plot)

데이터 변화 추세 확인

```python
import matplotlib.pyplot as plt

x = [1,2,3,4]
y = [10,20,15,30]

plt.plot(x,y)
plt.show()
```

사용 예

* 시간 변화
* 주가
* 온도 변화

---

### 2. 막대 그래프 (Bar Chart)

카테고리 비교

```python
x = ['A','B','C']
y = [10,20,15]

plt.bar(x,y)
plt.show()
```

사용 예

* 제품 판매량
* 부서별 매출

---

### 3. 히스토그램 (Histogram)

데이터 분포 확인

```python
import numpy as np

data = np.random.randn(100)

plt.hist(data)
plt.show()
```

사용 예

* 데이터 분포 확인
* 정규분포 분석

---

### 4. 산점도 (Scatter Plot)

두 변수 관계 확인

```python
x = [1,2,3,4]
y = [10,15,20,25]

plt.scatter(x,y)
plt.show()
```

사용 예

* 상관관계 분석

---

### 그래프 기본 옵션

```python
plt.title("Graph Title")
plt.xlabel("X axis")
plt.ylabel("Y axis")
plt.grid()
```

예

```python
plt.plot(x,y)
plt.title("Sales Trend")
plt.xlabel("Month")
plt.ylabel("Sales")
plt.grid()
plt.show()
```

---

# 핵심 그래프 4가지

| 그래프          | 용도     |
| ------------ | ------ |
| Line Plot    | 변화 추세  |
| Bar Chart    | 항목 비교  |
| Histogram    | 데이터 분포 |
| Scatter Plot | 상관관계   |

---

# 핵심 정리

데이터 분석 기본 그래프

1. Histogram
2. Box Plot
3. Scatter Plot
4. Line Plot
