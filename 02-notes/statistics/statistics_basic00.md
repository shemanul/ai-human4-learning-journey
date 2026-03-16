## 1. 여러 가지 확률분포
### 1) 이산 확률분포
- 개념: 확률변수가 정수 값만을 가지는 분포
- 베르누이 시행: 성공(1)과 실패(0) 두 가지 결과만 있는 시행
- 이항분포: 여러 번의 베르누이 시행에서 성공 횟수를 나타내는 분포
- 초기하분포: 모집단에서 비복원 추출 시 성공 횟수를 나타내는 분포
- 포아송분포: 단위 시간/공간에서 사건이 발생하는 횟수를 나타내는 분포
```python
import numpy as np
from scipy.stats import bernoulli, binom, hypergeom, poisson

# 베르누이 시행 (성공확률 p=0.3)
print("베르누이:", bernoulli.rvs(p=0.3, size=10))
# p: 성공 확률 (예: 0.3 → 성공할 확률 30%)
# size: 생성할 난수 개수 (예: 10 → 10개의 시행 결과 반환)

# 이항분포 (n=10, p=0.5)
print("이항분포:", binom.rvs(n=10, p=0.5, size=10))
# n: 시행 횟수 (예: 10번 시행)
# p: 각 시행에서 성공할 확률 (예: 0.5 → 50%)
# size: 표본 개수 (예: 10개의 난수 생성)

# 초기하분포 (모집단 크기=20, 성공=7, 표본=5)
print("초기하분포:", hypergeom.rvs(M=20, n=7, N=5, size=10))
# M: 모집단 크기 (예: 전체 20개)
# n: 모집단 내 성공 개수 (예: 성공 항목 7개)
# N: 추출할 표본 크기 (예: 5개 뽑기)
# size: 난수 개수 (예: 10개의 결과 생성)

# 포아송분포 (λ=3)
print("포아송분포:", poisson.rvs(mu=3, size=10))
# mu (λ): 평균 발생 횟수 (예: 평균 3번 발생)
# size: 난수 개수 (예: 10개 생성)
```

### 2) 연속 확률분포
- 개념: 확률변수가 연속적인 실수 값을 가지는 분포
- 균일분포: 일정 구간에서 모든 값이 동일한 확률
- 정규분포: 평균과 분산으로 정의되는 가장 중요한 분포
- 표준정규분포: 평균=0, 분산=1인 정규분포
```python
from scipy.stats import uniform, norm

# 균일분포 (0~1 구간)
print("균일분포:", uniform.rvs(size=10))
# size: 난수 개수
# (기본적으로 0~1 구간에서 균일하게 생성)

# 정규분포 (평균=5, 표준편차=2)
print("정규분포:", norm.rvs(loc=5, scale=2, size=10))
# loc: 평균 (예: 5)
# scale: 표준편차 (예: 2)
# size: 난수 개수 (예: 10개)

# 표준정규분포
print("표준정규분포:", norm.rvs(size=10))
# 평균=0, 표준편차=1인 표준정규분포에서 난수 생성
# size: 난수 개수
```

## 2. 모집단과 표본
- 모집단: 연구 대상이 되는 전체 집합
- 표본: 모집단에서 추출한 일부 집합
- 모수: 모집단의 특성을 나타내는 값 (예: 모집단 평균 μ)
- 통계량: 표본으로부터 계산된 값 (예: 표본 평균 𝑥)

📌 예시:
서울시민 전체(모집단) → 1000명 설문조사(표본) → 평균 소득(통계량)으로 전체 평균 소득(모수)을 추정
```python
# 모집단: 평균 50, 표준편차 10, 모집단크기(10000명)인 정규분포
population = norm.rvs(loc=50, scale=10, size=10000)
# loc: 모집단 평균 (예: 50)
# scale: 모집단 표준편차 (예: 10)
# size: 모집단 크기 (예: 10000명)

# 표본 추출 : 표본크기(100명)
sample = np.random.choice(population, size=100)
# population: 모집단 데이터
# size: 표본 크기 (예: 100명)

print("모집단 평균:", np.mean(population))
print("표본 평균:", np.mean(sample))
```

## 3. 통계적 추론
- 모수추정 (점추정): 표본 통계량으로 모수를 하나의 값으로 추정
- 가설검정 (구간추정): 모수의 범위를 추정
- 모평균의 구간 추정
```python
import scipy.stats as stats

sample_mean = np.mean(sample)
sample_std = np.std(sample, ddof=1)
n = len(sample)

# 95% 신뢰구간
conf_interval = stats.t.interval(0.95, df=n-1, loc=sample_mean, scale=sample_std/np.sqrt(n))
# 0.95: 신뢰수준 (95%)
# df: 자유도 (표본 크기 n-1)
# loc: 표본 평균
# scale: 표본 표준편차 / √n (표본 평균의 표준오차)

print("모평균의 95% 신뢰구간:", conf_interval)

```

## 4. 통계적 가설 검정
- 가설 검정이란: 표본을 통해 모집단에 대한 가설을 검증하는 과정
- 귀무가설(H0): 새로운 약의 치료율이 기존 약보다 높지않다
- 대립가설(H1): 새로운 약의 치료율이 기존 약보다 높다
- 양측/단측 가설: 평균이 특정 값과 다른지(양측), 큰지/작은지(단측)
- 오류의 종류:
  - **제1종 오류:** ***H0가 참인데 기각***
    → 새로운 약의 치료율이 기존 약 보다 높지 않음에도 높다고 잘못 판단
      ***기존 약 대신 새로운 약 생산***
    → **제2종 오류:** ***H0가 거짓(H1이 참) 인데 채택*** 
      새로운 약의 치료율이 기존 약보다 높으나 높지 않다고 잘못 판단
      ***새로운 약 대신 기존 약 생산***
- **유의 수준(α):** 제1종 오류를 허용하는 확률

## 5. 검정의 종류와 과정
- 검정 과정:
  - 가설 설정
  - 유의 수준 결정
  - 검정 통계량 계산
  - 기각역 설정
  - 결론 도출

- 검정 통계량: 표본으로부터 계산된 값
- 기각역: 귀무가설을 기각하는 영역
- 유의 확률(p-value): 귀무가설이 참일 때 관측된 결과가 나올 확률

**가설검정 종류**
- 이항검정: 성공/실패 비율 검정
- 모평균 가설검정: 표본 평균으로 모집단 평균 검정
```python
# 모평균 가설검정 (표본 평균이 50과 다른지 검정)
t_stat, p_value = stats.ttest_1samp(sample, popmean=50)
# sample: 표본 데이터
# popmean: 검정할 모집단 평균 (예: 50)
# 출력값:
# t_stat: t-검정 통계량
# p_value: 유의확률 (귀무가설 기각 여부 판단 기준)

print("t-통계량:", t_stat)
print("p-value:", p_value)

if p_value < 0.05:
    print("귀무가설 기각 → 평균은 50과 다르다")
else:
    print("귀무가설 채택 → 평균은 50과 같다")

```

## [정리]
### 📊 확률분포 및 통계 함수 파라미터 정리

#### 1. 이산 확률분포

| 라이브러리 | 함수 | 파라미터 | 의미 | import |
|------------|------|-----------|------|--------|
| SciPy | `bernoulli.rvs(p, size)` | `p` | 성공 확률 (0~1) | `from scipy.stats import bernoulli` |
| | | `size` | 난수 개수 | |
| NumPy | `np.random.binomial(n, p, size)` | `n` | 시행 횟수 | `import numpy as np` |
| | | `p` | 성공 확률 | |
| | | `size` | 난수 개수 | |
| SciPy | `binom.rvs(n, p, size)` | `n` | 시행 횟수 | `from scipy.stats import binom` |
| | | `p` | 성공 확률 | |
| | | `size` | 난수 개수 | |
| SciPy | `hypergeom.rvs(M, n, N, size)` | `M` | 모집단 크기 | `from scipy.stats import hypergeom` |
| | | `n` | 모집단 내 성공 개수 | |
| | | `N` | 추출할 표본 크기 | |
| | | `size` | 난수 개수 | |
| NumPy | `np.random.poisson(lam, size)` | `lam` | 평균 발생 횟수 | `import numpy as np` |
| | | `size` | 난수 개수 | |
| SciPy | `poisson.rvs(mu, size)` | `mu (λ)` | 평균 발생 횟수 | `from scipy.stats import poisson` |
| | | `size` | 난수 개수 | |

---

#### 2. 연속 확률분포

| 라이브러리 | 함수 | 파라미터 | 의미 | import |
|------------|------|-----------|------|--------|
| NumPy | `np.random.uniform(low, high, size)` | `low` | 최소값 | `import numpy as np` |
| | | `high` | 최대값 | |
| | | `size` | 난수 개수 | |
| SciPy | `uniform.rvs(size)` | `size` | 난수 개수 (기본 구간 0~1) | `from scipy.stats import uniform` |
| NumPy | `np.random.normal(loc, scale, size)` | `loc` | 평균 | `import numpy as np` |
| | | `scale` | 표준편차 | |
| | | `size` | 난수 개수 | |
| SciPy | `norm.rvs(loc, scale, size)` | `loc` | 평균 | `from scipy.stats import norm` |
| | | `scale` | 표준편차 | |
| | | `size` | 난수 개수 | |

---

#### 3. 모집단과 표본

| 라이브러리 | 함수 | 파라미터 | 의미 | import |
|------------|------|-----------|------|--------|
| SciPy | `norm.rvs(loc, scale, size)` | `loc` | 모집단 평균 | `from scipy.stats import norm` |
| | | `scale` | 모집단 표준편차 | |
| | | `size` | 모집단 크기 | |
| NumPy | `np.random.choice(population, size)` | `population` | 모집단 데이터 | `import numpy as np` |
| | | `size` | 추출할 표본 크기 | |

---
#### 4. 모평균의 구간 추정

| 라이브러리 | 함수 | 파라미터 | 의미 | import |
|------------|------|-----------|------|--------|
| SciPy | `stats.t.interval(conf, df, loc, scale)` | `conf` | 신뢰수준 (예: 0.95 → 95%) | `import scipy.stats as stats` |
| | | `df` | 자유도 (n-1) | |
| | | `loc` | 표본 평균 | |
| | | `scale` | 표본 평균의 표준오차 (s/√n) | |

---

#### 5. 가설검정

| 라이브러리 | 함수 | 파라미터 | 의미 | import |
|------------|------|-----------|------|--------|
| SciPy | `stats.ttest_1samp(sample, popmean)` | `sample` | 표본 데이터 | `import scipy.stats as stats` |
| | | `popmean` | 검정할 모집단 평균 | |

출력값:
- **t_stat**: t-검정 통계량  
- **p_value**: 유의확률 (귀무가설 기각 여부 판단 기준)
