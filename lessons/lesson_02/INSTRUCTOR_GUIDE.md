# 2강 강의자 지도안

> 강의자 전용 문서입니다. 학습자에게 수업 전에 정답 부분을 제공하지 않습니다.

## 강의 개요

- 강의명: 생성모형과 우도 - 데이터는 어떻게 만들어졌다고 가정하는가
- 학습자용 노트북: `lesson_02.ipynb`
- 정답 노트북: `lesson_02_answer.ipynb`
- 공통 운영 규칙: `../INSTRUCTOR_PROTOCOL.md`
- 1강 복습 문서: `REVIEW_FROM_LESSON_01.md`
- 대상: 1강을 마치고 Beta-Binomial 갱신을 한 번 구현한 학습자
- 권장 시간:
  - 본강의와 직접 실습: 90~120분
  - 종합평가: 60~90분
- 오프라인 필수 패키지: Python, NumPy, SciPy, pandas, Matplotlib, Jupyter

한 번에 진행하기 벅차면 본강의와 종합평가를 별도 세션으로 나눕니다. 2강은 개념의 방향성이 중요하므로, 코드를 빨리 끝내는 것보다 각 코드가 어떤 확률 질문에 답하는지 확인하는 데 시간을 씁니다.

## 강의 목표

학습자는 강의 후 다음을 할 수 있어야 합니다.

1. 생성모형을 `theta -> Y -> y` 흐름으로 설명한다.
2. $Y\mid\theta\sim Binomial(n,\theta)$에서 각 기호를 노동시장 사례에 대응한다.
3. $P(Y=y\mid\theta)$, $L(\theta;y)$, $P(\theta\mid Y=y)$의 조건 방향을 구분한다.
4. 같은 관측비율에서도 표본 수가 커지면 우도 곡선이 좁아지는 이유를 설명한다.
5. 정규화 상수 $P(Y=y)$를 주변우도/evidence로 해석한다.
6. 격자 근사로 연속형 모수의 사후밀도를 계산한다.
7. Beta-Binomial 갱신식을 짧게 유도하고 코드로 구현한다.
8. 순차 갱신과 한 번에 갱신이 같은 결과를 주는 이유를 설명한다.

## 이 강의에서 반드시 교정할 오개념

1. 우도 $L(\theta;y)$는 $\theta$에 대한 확률분포다.
2. 우도 곡선 아래 면적은 항상 1이어야 한다.
3. $P(Y=y\mid\theta)$와 $P(\theta\mid Y=y)$는 거의 같은 말이다.
4. $n$은 성공 수다.
5. $y$와 $Y$는 표기만 다른 같은 대상이다.
6. 주변우도는 계산 편의를 위한 임의의 나눗셈 숫자일 뿐이다.
7. 같은 관측비율이면 표본 수와 상관없이 같은 정보량을 준다.
8. 순차 갱신은 순서에 따라 결과가 달라지는 일반적 절차다.

## 강의자 말투

- 1강에서 학습자는 계산 감각이 좋았으므로, 맞는 직관을 먼저 확인합니다.
- 단, 조건 방향과 인자 순서 실수는 명확히 교정합니다.
- “데이터가 어떻게 만들어졌다고 가정한 셈인가?”를 자주 묻습니다.
- 수식은 사례 대응 후 제시합니다.
- 대화 체크포인트에서는 한 번에 한 질문만 묻습니다.

---

## 수업 전 점검

1. `REVIEW_FROM_LESSON_01.md`를 읽고 빠른 질문 3개로 시작합니다.
2. `lesson_02.ipynb`의 모든 코드 셀을 위에서부터 실행 가능한지 확인합니다.
3. `scipy.stats`의 `binom`, `beta`가 import되는지 확인합니다.
4. 이전 `RESULT.md`에서 다음 혼동을 복습합니다.
   - `successes`, `failures`, `n`, `y` 구분
   - 우도는 모수에 대한 확률분포가 아니라는 점
   - 연속형 모수의 한 점 확률과 밀도 구분

---

## 수업 순서

## A. 1강 복습: 조건 방향과 기호

노트북의 `## 0. 1강에서 이어지는 질문`을 진행합니다.

### 대화 체크포인트 A

#### 질문 1

> $Y\mid\theta\sim Binomial(n,\theta)$에서 $Y$, $n$, $\theta$는 각각 무엇인가요?

기대 답변:

- $\theta$: 모집단 또는 생성과정의 재취업 성공확률
- $n$: 관측한 구직자 수
- $Y$: 관측 전에 알 수 없는 재취업 성공자 수

교정:

- 학습자가 $Y$를 실제 관측값이라고 말하면 $Y$는 확률변수, $y$는 관측값이라고 구분하게 합니다.
- 학습자가 $n$을 성공 수라고 말하면 전체 수와 성공 수를 분리합니다.

#### 질문 2

> 관측값이 $y=12$라면 우도 $L(\theta;y)$는 무엇을 고정하고 무엇을 움직이나요?

기대 답변:

- 관측값 $y=12$와 $n$은 고정한다.
- $\theta$를 움직이며 각 $\theta$가 그 데이터를 얼마나 잘 설명하는지 본다.

#### 질문 3

> `n=20`, `y=12`일 때 성공 수와 실패 수는 각각 몇 명인가요?

정답:

```text
successes = 12
failures = 8
```

이 질문은 1강의 마지막 실수를 회수하는 질문입니다. 통과하지 못하면 본강의로 넘어가지 않고 `n`, `y`, `n-y`를 다른 숫자로 한 번 더 확인합니다.

---

## B. 생성모형: 데이터가 만들어지는 이야기

노트북의 `## 1. 생성모형`을 진행합니다.

중심 설명:

```text
theta에 대한 사전 믿음이 있다.
theta가 정해지면 n명 중 몇 명이 성공할지 Y가 확률적으로 생성된다.
우리는 그중 실제 관측된 값 y를 본다.
```

수식:

$$
\theta \sim Beta(\alpha,\beta)
$$

$$
Y\mid\theta\sim Binomial(n,\theta)
$$

$$
y\ \text{observed}
$$

강조:

- 생성모형은 “세상이 실제로 반드시 이렇게 생겼다”가 아니라 “분석을 위해 이렇게 가정한다”입니다.
- 우도는 이 표본모형에서 나옵니다.

### 대화 체크포인트 B

> 노동시장 예제에서 $\theta$, $Y$, $y$, $n$을 각각 한 문장으로 정의해보세요.

기대 답변:

- $\theta$: 지역 전체 구직자가 일정 기간 안에 재취업할 확률
- $n$: 관측한 구직자 수
- $Y$: 관측 전에는 알 수 없는 재취업 성공자 수
- $y$: 실제 관측된 재취업 성공자 수

통과 기준:

- 확률변수와 관측값을 구분한다.
- 모수를 표본 수나 성공자 수로 설명하지 않는다.

---

## C. 우도 곡선: 같은 비율, 다른 표본 수

노트북의 `## 2. 우도 곡선`을 진행합니다.

예제:

```text
A: n=20, y=12
B: n=200, y=120
```

둘 다 관측 성공률은 0.6입니다.

코드 핵심:

```python
theta_grid = np.linspace(0.001, 0.999, 500)
lik_A = binom.pmf(12, 20, theta_grid)
lik_B = binom.pmf(120, 200, theta_grid)
rel_lik_A = lik_A / lik_A.max()
rel_lik_B = lik_B / lik_B.max()
```

실행 전 예측 질문:

> 두 우도 곡선의 꼭대기는 어디쯤일까요? 어느 곡선이 더 좁을까요?

기대 답변:

- 꼭대기는 둘 다 0.6 근처다.
- B가 더 좁다.
- B는 표본 수가 더 크기 때문에 같은 비율이라도 더 많은 정보를 준다.

오개념 대응:

- “둘 다 0.6이니까 같은 곡선”이라고 답하면, 12/20과 120/200 중 어느 쪽이 더 우연히 흔들리기 쉬운지 묻습니다.
- “B의 우도값 합이 1이어야 한다”고 답하면, 우도는 $\theta$ 방향의 확률분포가 아니라고 다시 설명합니다.

자동 점검 기준:

- `theta_grid` 길이가 충분하다.
- `lik_A`, `lik_B`가 같은 shape이다.
- 두 우도의 최댓값 위치가 대략 0.6 근처다.
- 상대우도로 비교했을 때 B가 0.6 주변에서 더 빠르게 감소한다.

---

## D. 정규화와 주변우도

노트북의 `## 3. 정규화 상수와 주변우도`를 진행합니다.

유한 가설 예제:

```python
theta_candidates = np.array([0.2, 0.5, 0.8])
prior = np.array([0.2, 0.5, 0.3])
n = 5
y = 4
likelihood = binom.pmf(y, n, theta_candidates)
unnormalized = prior * likelihood
evidence = unnormalized.sum()
posterior = unnormalized / evidence
```

핵심 질문:

> `evidence`는 왜 그냥 나누는 숫자가 아니라 $P(Y=y)$인가요?

기대 답변:

- 각 가설 아래에서 데이터가 나올 확률을 사전확률로 가중평균한 값이다.
- 즉 데이터를 관측하기 전, 현재 모형 전체가 그 데이터를 예측하는 확률이다.
- 사후확률의 합이 1이 되게 하는 정규화 상수다.

수식:

$$
P(Y=y)=\sum_\theta P(Y=y\mid\theta)P(\theta)
$$

연속형:

$$
P(Y=y)=\int P(Y=y\mid\theta)p(\theta)d\theta
$$

---

## E. 격자 근사: 연속형 모수를 계산하기

노트북의 `## 4. 격자 근사`를 진행합니다.

목표:

- 연속형 $\theta\in[0,1]$를 촘촘한 격자로 근사합니다.
- 사전밀도와 우도를 곱해 정규화되지 않은 사후밀도를 만듭니다.
- 적분값이 1이 되도록 정규화합니다.

코드 핵심:

```python
theta_grid = np.linspace(0.001, 0.999, 1000)
prior_density = beta_dist(2, 2).pdf(theta_grid)
likelihood = binom.pmf(y, n, theta_grid)
unnormalized = prior_density * likelihood
evidence_grid = np.trapezoid(unnormalized, theta_grid)
posterior_density = unnormalized / evidence_grid
```

주의:

- `posterior_density.sum()`이 1일 필요는 없습니다.
- 밀도이므로 적분값이 1이어야 합니다.

자동 점검:

```python
posterior_area = np.trapezoid(posterior_density, theta_grid)
np.isclose(posterior_area, 1, atol=1e-3)
```

---

## F. Beta-Binomial 해석적 갱신

노트북의 `## 5. Beta-Binomial 갱신`을 진행합니다.

유도:

$$
p(\theta\mid y)\propto p(\theta)p(y\mid\theta)
$$

$$
\propto \theta^{\alpha-1}(1-\theta)^{\beta-1}
\theta^y(1-\theta)^{n-y}
$$

$$
\propto \theta^{\alpha+y-1}(1-\theta)^{\beta+n-y-1}
$$

따라서:

$$
\theta\mid y\sim Beta(\alpha+y,\beta+n-y)
$$

직접 구현 함수:

```python
def beta_binomial_update(alpha_prior, beta_prior, n, y):
    successes = y
    failures = n - y
    alpha_post = alpha_prior + successes
    beta_post = beta_prior + failures
    posterior_mean = alpha_post / (alpha_post + beta_post)
    ci_low, ci_high = beta_dist(alpha_post, beta_post).ppf([0.025, 0.975])
    return alpha_post, beta_post, posterior_mean, ci_low, ci_high
```

이 함수는 일부러 `n, y`를 받습니다. 1강의 `successes, failures` 입력 함수와 구분하게 합니다.

자동 점검 예:

```text
beta_binomial_update(3, 7, 20, 12)
-> alpha_post=15, beta_post=15, mean=0.5

beta_binomial_update(3, 7, 200, 120)
-> alpha_post=123, beta_post=87, mean≈0.585714
```

---

## G. 순차 갱신과 한 번에 갱신

노트북의 `## 6. 순차 갱신`을 진행합니다.

예제:

```text
prior: Beta(2, 2)
data 1: n=5,  y=3
data 2: n=10, y=5
```

한 번에 갱신:

```text
successes = 8
failures = 7
posterior = Beta(10, 9)
```

순차 갱신:

```text
Beta(2, 2) -> Beta(5, 4) -> Beta(10, 9)
```

해석:

- 이항-Beta 모형에서는 성공 총수와 실패 총수가 핵심입니다.
- 같은 자료를 두 묶음으로 나누어 보아도 전체 성공/실패 수가 같으면 같은 사후분포가 됩니다.
- 이를 너무 일반화하지 말고, 현재 모형의 가정 아래에서 그렇다고 설명합니다.

---

## 종합평가

종합평가는 노트북 마지막 절을 사용하되, 객관식과 주관식은 대화창에서 한 문항씩 묻습니다. 프로그래밍 문제는 학습자가 노트북에서 작성한 코드와 실행 결과를 대화창에 붙여넣게 합니다.

### 객관식 평가

#### 객관식 1

다음 중 우도 $L(\theta;y)$에 대한 설명으로 가장 적절한 것은?

A. 데이터를 본 뒤 $\theta$가 참일 확률분포다.  
B. $\theta$가 주어졌을 때 관측값 $y$가 나올 확률을, $y$를 고정하고 $\theta$의 함수로 본 것이다.  
C. 모든 $\theta$에 대해 합하면 반드시 1이 되는 값이다.  
D. 사전분포와 항상 같은 모양을 갖는다.

정답: B

#### 객관식 2

주변우도 $P(Y=y)$의 역할로 가장 적절한 것은?

A. 사전분포의 평균이다.  
B. 우도 곡선의 최댓값이다.  
C. 정규화되지 않은 사후값들을 확률분포로 만들기 위한 정규화 상수다.  
D. 관측 성공률이다.

정답: C

#### 객관식 3

$\theta\sim Beta(2,3)$이고 $n=5, y=4$를 관측했다. 사후분포는?

A. $Beta(6,4)$  
B. $Beta(4,6)$  
C. $Beta(7,8)$  
D. $Beta(2,3)$

정답: A

#### 객관식 4

같은 관측비율 $y/n=0.6$에서 $n=20$보다 $n=200$의 우도 곡선이 더 좁은 이유는?

A. 관측비율이 더 크기 때문이다.  
B. 사전분포가 더 강하기 때문이다.  
C. 더 많은 관측값이 같은 비율을 지지하기 때문이다.  
D. 우도는 표본 수와 무관하기 때문이다.

정답: C

#### 객관식 5

연속형 모수 $\theta$에서 $P(\theta=0.5\mid y)$에 대해 주의할 점은?

A. 항상 1이다.  
B. 보통 한 점의 확률은 0이므로 밀도나 구간확률을 보아야 한다.  
C. 우도와 항상 같다.  
D. 사전분포와 항상 같다.

정답: B

### 주관식 평가

#### 주관식 1

노동시장 예제에서 생성모형을 $\theta\to Y\to y$ 흐름으로 설명하세요.

통과 기준:

- $\theta$를 재취업 확률로 정의한다.
- $\theta$가 주어졌을 때 $Y$가 이항분포를 따른다고 설명한다.
- $y$는 실제 관측된 성공자 수라고 구분한다.

#### 주관식 2

우도와 사후분포의 차이를 $P(Y=y\mid\theta)$, $P(\theta\mid Y=y)$를 사용해 설명하세요.

통과 기준:

- 조건 방향을 정확히 말한다.
- 우도는 모수에 대한 확률분포가 아니며, 사후분포는 데이터 관측 후 모수에 대한 분포라고 설명한다.

#### 주관식 3

주변우도/evidence가 “데이터의 전체 확률”이라는 말을 유한 가설 예제로 설명하세요.

통과 기준:

- 각 가설별 데이터 확률을 사전확률로 가중합한다고 설명한다.
- 사후분포를 정규화하는 역할을 말한다.

#### 주관식 4

같은 관측비율이라도 표본 수가 커지면 우도와 사후분포가 좁아지는 이유를 설명하세요.

통과 기준:

- 표본 수가 많을수록 우연한 변동 가능성이 줄어든다는 직관을 말한다.
- 더 많은 데이터가 같은 $\theta$ 근처를 지지한다고 설명한다.

### 프로그래밍 평가

#### 프로그래밍 1: 격자 사후분포 함수

요구사항:

```python
def grid_posterior(theta_grid, prior_density, n, y):
    ...
    return likelihood, posterior_density
```

통과 기준:

- `binom.pmf(y, n, theta_grid)`로 우도를 계산한다.
- `prior_density * likelihood`를 계산한다.
- `np.trapezoid(..., theta_grid)`로 정규화한다.
- 반환된 `posterior_density`의 적분값이 1에 가깝다.
- 다른 `n, y`에도 동작한다.

#### 프로그래밍 2: Beta-Binomial 함수

요구사항:

```python
def beta_binomial_update(alpha_prior, beta_prior, n, y):
    ...
    return alpha_post, beta_post, posterior_mean, ci_low, ci_high
```

통과 기준:

- `successes = y`, `failures = n - y`를 명시한다.
- 사후 모수와 평균이 맞다.
- 95% credible interval을 계산한다.
- 입력을 바꿔도 동작한다.

#### 프로그래밍 3: 노동시장 비교

공통 사전분포:

```text
theta ~ Beta(3, 7)
```

지역:

```text
A: n=20, y=12
B: n=200, y=120
```

요구사항:

1. 코드 실행 전 어느 지역의 사후평균이 0.6에 더 가까운지, 어느 사후분포가 더 좁은지 예측한다.
2. `beta_binomial_update`를 사용해 두 지역의 사후분포를 계산한다.
3. 사후평균과 95% credible interval을 표로 출력한다.
4. 두 사후밀도를 하나의 그래프에 그린다.
5. 결과를 정책 담당자에게 설명하듯 3~5문장으로 해석한다.

기준 결과:

```text
A: Beta(15, 15),  mean = 0.500000, CI ≈ [0.325, 0.675]
B: Beta(123, 87), mean = 0.585714, CI ≈ [0.519, 0.651]
```

판정:

- 예측, 코드, 결과, 해석이 모두 맞으면 통과
- `n`과 `y`를 혼동하면 교정 후 재제출
- 그래프만 있고 해석이 없으면 연습 중

---

## RESULT.md 기록 지침

각 체크포인트마다 다음을 기록합니다.

1. 질문
2. 학습자의 실제 답변
3. 맞는 직관
4. 발견된 오개념
5. 교정 설명
6. 수정된 답변
7. 통과 여부

프로그래밍 평가는 코드 전체를 모두 붙이지 않아도 되지만, 함수 정의와 핵심 출력값은 기록합니다.
