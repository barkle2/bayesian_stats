# 1강 강의자 지도안

> 강의자 전용 문서입니다. 학습자에게 수업 전에 정답 부분을 제공하지 않습니다.

## 강의 개요

- 강의명: 베이지안 통계의 개념 - 증거로 믿음 갱신하기
- 학습자용 노트북: `lesson_01.ipynb`
- 공통 운영 규칙: `../INSTRUCTOR_PROTOCOL.md`
- 대상: 베이지안 통계 입문자, Python 기초 문법을 조금 아는 학습자
- 권장 시간:
  - 본강의와 직접 실습: 90~120분
  - 종합평가: 60~90분
- 오프라인 필수 패키지: Python, NumPy, SciPy, Matplotlib, Jupyter

한 번에 진행하기 벅차면 본강의와 종합평가를 별도 세션으로 나눕니다. 학습자가 피로한 상태에서 평가를 강행하지 않습니다.

## 강의 목표

학습자는 강의 후 다음을 할 수 있어야 합니다.

1. 베이지안 통계가 불확실성을 분포로 표현한다는 점을 설명한다.
2. 모수, 확률변수, 관측값을 구분한다.
3. 사전분포, 우도, 사후분포의 조건 방향을 구분한다.
4. 유한 가설의 베이즈 갱신을 손으로 계산하고 Python으로 구현한다.
5. 사후분포를 다음 갱신의 사전분포로 사용한다.
6. Beta-Binomial 켤레 갱신을 계산한다.
7. 사전분포와 데이터 양이 사후분포의 위치와 폭에 미치는 영향을 설명한다.
8. 같은 구조를 재취업률 문제에 적용한다.

## 이 강의에서 반드시 교정할 오개념

1. 우도는 `P(parameter | data)`이다.
2. 우도값들을 모수 방향으로 더하면 반드시 1이다.
3. 모수 `theta`는 표본 수 또는 성공자 수다.
4. 데이터가 많으면 모형에 없던 참값도 자동으로 발견한다.
5. 성공이 많으면 사후분포는 항상 1로 이동한다.
6. Beta 분포의 `alpha`, `beta`는 갱신 후에도 변하지 않는다.
7. 코드를 실행해 원하는 숫자가 나오면 개념도 이해한 것이다.

## 강의자 말투

페르소나는 선택사항입니다. 어떤 모델이나 사람이 진행해도 다음 태도를 유지합니다.

- 따뜻하지만 정답 판정은 명확하게 한다.
- 학습자의 맞는 직관부터 확인한다.
- 수식보다 실제 사례와 조건 방향을 먼저 설명한다.
- 한 메시지에 질문 하나만 제시한다.
- “틀렸다”로 끝내지 않고 왜 그렇게 생각했는지 확인한다.
- 정답을 말한 뒤에는 숫자나 맥락을 바꾸어 다시 확인한다.

`@베이즈` 페르소나를 사용한다면 약간 격식 있는 말투와 “증거에 따라 믿음을 갱신한다”는 중심 직관을 사용할 수 있습니다. 역사적 인용은 만들지 않습니다.

---

## 수업 순서

## A. 도입: 베이지안 통계란 무엇인가

노트북의 `## 베이지안 통계란 무엇인가`를 함께 읽습니다. 텍스트를 그대로 낭독하지 말고 다음 흐름으로 설명합니다.

```text
관측 가능한 작은 자료
→ 직접 볼 수 없는 모집단의 성질
→ 그 불확실성을 분포로 표현
→ 새 데이터로 분포를 갱신
```

노동시장 예시:

- 관측자료: 조사한 구직자 10명의 재취업 여부
- 모수: 해당 지역 전체 구직자의 3개월 이내 재취업확률
- 미래 예측: 다음 구직자의 3개월 이내 재취업 여부

강조할 점:

- “믿음”은 임의의 기분이 아니라 명시한 가정과 정보다.
- 사전분포는 숨기지 않고 기록하며 민감도 분석의 대상이 된다.
- 빈도주의와 베이지안 통계는 적대적 선택지가 아니다.
- 두 접근 모두 확률모형과 우도를 사용하지만 불확실성을 표현하는 방식이 다르다.

### 도입 대화 체크포인트

#### 질문 1

> 자신의 업무나 관심 분야에서 자주 관측할 수 있는 데이터 하나와 직접 관측할 수 없는 모수 하나를 예로 들어보세요.

좋은 답의 조건:

- 데이터는 실제로 관측 가능한 값이다.
- 모수는 모집단 또는 생성과정의 알 수 없는 성질이다.
- 표본 수를 모수로 잘못 부르지 않는다.

노동시장 예시 답안:

```text
데이터: 조사 대상자 중 3개월 이내 재취업한 사람의 수
모수: 지역 전체 구직자의 3개월 이내 재취업확률
```

#### 질문 2

> 데이터를 보기 전의 지식을 분석에 포함하는 것이 어떤 점에서는 유용하고, 어떤 점에서는 위험할 수 있을까요?

기대 답변:

- 유용성: 과거 연구, 전문가 지식, 현실적인 범위를 활용할 수 있다.
- 위험성: 부적절하거나 지나치게 강한 사전분포가 결과를 왜곡할 수 있다.
- 대응: 사전분포를 명시하고 사전예측검사와 민감도 분석을 한다.

학습자가 “주관적이라 나쁘다”고만 답하면, 측정 방법과 모형 선택에도 판단이 들어간다는 점을 설명하되 사전분포의 위험을 축소하지 않습니다.

---

## B. 사전분포, 우도, 사후분포

노트북의 `## 0. 개념 강의`와 `풀이 예제 0-A`를 사용합니다.

### 풀이 예제의 계산

```text
hypotheses = [0.1, 0.4, 0.7]
prior       = [0.2, 0.5, 0.3]
likelihood  = [0.1, 0.4, 0.7]
weights     = [0.02, 0.20, 0.21]
evidence    = 0.43
posterior   = [0.0465, 0.4651, 0.4884]
```

코드를 실행하기 전에 가장 높은 사후확률을 예측하게 합니다. `p=0.7`이 가장 큰 우도를 갖지만 `p=0.4`의 사전확률이 크기 때문에 두 사후확률이 비슷하다는 점이 핵심입니다.

코드 주석을 한 줄씩 읽게 하지 않습니다. 다음 세 줄은 학습자가 직접 설명하게 합니다.

```python
demo_weights = demo_prior * demo_likelihood
demo_evidence = demo_weights.sum()
demo_posterior = demo_weights / demo_evidence
```

### 대화 체크포인트 1

#### 질문 1

> `demo_likelihood`의 합이 1이 아니어도 되는 이유는 무엇인가요?

정답 핵심:

- 우도는 모수에 대한 확률분포가 아니다.
- 관측 데이터가 고정된 상태에서 후보 모수들을 비교하는 함수다.
- 따라서 후보 방향으로 합이 1일 필요가 없다.

#### 질문 2

> `p=0.7`의 우도와 사후확률은 각각 어떤 질문에 답하나요?

정답:

```text
우도: p=0.7이라고 가정했을 때 성공이 관측될 가능성은?
사후확률: 성공을 관측한 뒤 p=0.7 가설을 믿을 확률은?
```

기호:

\[
L(0.7;y)=P(Y=y\mid p=0.7)
\]

\[
P(p=0.7\mid Y=y)
\]

학습자가 조건 방향을 뒤집으면 숫자 반례를 사용합니다. 우도가 `0.7`이어도 사전확률에 따라 사후확률은 `0.7`이 아닐 수 있음을 보여줍니다.

---

## C. 유한 가설 직접 구현

노트북의 `## 1`과 `## 2`를 진행합니다. 이 부분에서는 핵심 코드를 학습자가 직접 입력해야 합니다.

### 직접 입력 1 기준 답안

```python
import numpy as np

hypotheses = np.array([0.2, 0.5, 0.8])
prior = np.array([1/3, 1/3, 1/3])

print(hypotheses)
print(prior)
```

자동 점검 전에 질문:

- `prior.sum()`은 왜 1이어야 하는가?
- 세 값 외의 가능성은 현재 모형에서 어떻게 처리되는가?

두 번째 질문의 답은 “모형에서 제외되어 있다”입니다. 데이터가 많아져도 제외된 가능성은 복원되지 않습니다.

### 직접 입력 2 기준 답안

```python
likelihood = hypotheses
weights = prior * likelihood
evidence = weights.sum()
posterior = weights / evidence

print(likelihood)
print(weights)
print(evidence)
print(posterior)
```

기준값:

```text
likelihood = [0.2, 0.5, 0.8]
weights = [0.0667, 0.1667, 0.2667]
evidence = 0.5
posterior = [0.1333, 0.3333, 0.5333]
```

### 대화 체크포인트 2

한 항목씩 묻습니다.

```text
p: 압정이 한 번의 시행에서 성공할 확률이라는 모수
Y: 압정 한 번의 성공 여부를 나타내는 확률변수
y=1 또는 성공: 실제 관측값
L(p;y): 관측값 y를 고정하고 후보 p를 바꾸어 비교하는 함수
```

`Y`를 성공 횟수로 정의해도 됩니다. 이 경우 시행 횟수 `n=1`, 관측 성공 횟수 `y=1`임을 명시해야 합니다.

---

## D. 순차 갱신

노트북의 `코드 변형 A`를 진행합니다.

### 기준 답안

```python
failure_likelihood = 1 - hypotheses
second_weights = posterior * failure_likelihood
second_posterior = second_weights / second_weights.sum()

print(failure_likelihood)
print(second_weights)
print(second_posterior)
```

기준값:

```text
failure_likelihood = [0.8, 0.5, 0.2]
second_posterior = [16/57, 25/57, 16/57]
                 ≈ [0.2807, 0.4386, 0.2807]
```

설명:

- 첫 번째 사후분포가 두 번째 갱신의 사전분포가 된다.
- 성공 한 번과 실패 한 번의 우도는 `p(1-p)`다.
- `p=0.2`와 `p=0.8`은 같은 우도 `0.16`을 갖는다.
- `p=0.5`는 우도 `0.25`로 혼합된 결과를 가장 잘 설명한다.

오개념:

- `second_weights`를 실패의 우도라고 부르지 않게 한다.
- 우도와 `prior × likelihood`를 구분하게 한다.

---

## E. 갱신 함수와 사전분포의 영향

노트북의 `## 3`과 `코드 변형 B`를 진행합니다.

### `bayes_update` 기준 답안

```python
def bayes_update(hypotheses, prior, successes, failures):
    likelihood = hypotheses**successes * (1 - hypotheses)**failures
    weights = prior * likelihood
    evidence = weights.sum()
    posterior = weights / evidence
    return likelihood, evidence, posterior
```

함수 작성 후 반드시 다음을 확인합니다.

```python
likelihood, evidence, posterior = bayes_update(
    np.array([0.2, 0.5, 0.8]),
    np.array([1/3, 1/3, 1/3]),
    successes=4,
    failures=1,
)
```

기준값:

```text
likelihood = [0.00128, 0.03125, 0.08192]
posterior ≈ [0.011184, 0.273045, 0.715771]
```

순서를 무시한 성공 횟수를 데이터로 사용할 때 이항계수는 생략되어 있습니다. 모든 후보 모수에 공통으로 곱해져 사후분포 정규화에서 소거되기 때문입니다.

### 강한 사전분포 변형 기준 답안

```python
strong_prior = np.array([0.8, 0.15, 0.05])
_, _, strong_posterior = bayes_update(
    hypotheses,
    strong_prior,
    successes=4,
    failures=1,
)
```

기준값:

```text
strong_posterior ≈ [0.104410, 0.477951, 0.417639]
```

확인 질문:

- 가장 큰 우도를 가진 `p=0.8`이 가장 큰 사후확률을 갖지 못한 이유는?
- 성공 40회, 실패 10회라면 어떤 가설이 지배할 것으로 예상하는가?

기대 답:

- 사후분포는 우도뿐 아니라 사전분포도 반영한다.
- 자료가 50개로 늘면 같은 사전분포에서도 `p=0.8`에 거의 집중한다.

주의:

“데이터가 많으면 사전분포가 항상 무의미하다”고 일반화하지 않습니다. 약한 식별성, 모형 오명세, 고차원 문제에서는 사전분포가 계속 중요할 수 있습니다.

---

## F. 연속형 모수와 Beta 분포

노트북의 `## 4`, `풀이 예제 4-A`, `직접 풀이 4-B`를 진행합니다.

### 설명 순서

1. 유한한 세 후보는 계산을 위한 단순화였음을 상기시킨다.
2. 연속형 모수에서는 한 점의 확률보다 구간 확률과 밀도를 다룬다.
3. Beta 분포의 지지집합이 `[0,1]`임을 설명한다.
4. 사전분포와 이항우도의 지수항이 더해지는 모습을 보여준다.
5. 같은 분포족으로 돌아오는 성질을 켤레성이라고 부른다.

유도:

\[
p^{\alpha-1}(1-p)^{\beta-1}
\times
p^h(1-p)^t
=
p^{\alpha+h-1}(1-p)^{\beta+t-1}
\]

### 풀이 예제 4-A

```text
prior = Beta(2, 4)
successes = 4
failures = 1
posterior = Beta(6, 5)
prior mean = 2/6 = 0.3333
observed rate = 4/5 = 0.8
posterior mean = 6/11 ≈ 0.5455
```

### 대화 체크포인트 3

> 왜 사후평균은 사전평균과 관측 성공비율 사이에 놓이나요? 데이터가 500개라면 어느 쪽에 더 가까워질까요?

정답 핵심:

- `alpha`, `beta`는 사전 성공·실패 정보의 무게처럼 작용한다.
- 새 성공과 실패 수가 기존 모수에 더해진다.
- 데이터 수가 사전분포의 농도보다 훨씬 크면 관측비율에 가까워진다.

의사표본 해석은 유용한 직관이지만 모든 사전분포와 모형에서 문자 그대로 적용되는 것은 아니라고 알려줍니다.

### 직접 풀이 4-B 기준 답안

```python
from scipy.stats import beta as beta_dist
import matplotlib.pyplot as plt

alpha_prior, beta_prior = 3, 5
successes, failures = 7, 3

alpha_post = alpha_prior + successes
beta_post = beta_prior + failures
posterior_mean = beta_dist(alpha_post, beta_post).mean()

p_grid = np.linspace(0.001, 0.999, 1000)
plt.plot(
    p_grid,
    beta_dist(alpha_prior, beta_prior).pdf(p_grid),
    label="Prior: Beta(3, 5)",
)
plt.plot(
    p_grid,
    beta_dist(alpha_post, beta_post).pdf(p_grid),
    label="Posterior: Beta(10, 8)",
)
plt.xlabel("p")
plt.ylabel("Density")
plt.legend()
plt.show()
```

기준값:

```text
alpha_post = 10
beta_post = 8
posterior_mean = 10/18 ≈ 0.5556
```

---

## G. 노동시장 문제

노트북의 `## 5`를 진행합니다.

모형:

\[
\theta\sim\operatorname{Beta}(4,6)
\]

\[
Y\mid\theta\sim\operatorname{Binomial}(10,\theta)
\]

\[
y=6
\]

기호 대응:

```text
theta: 지역 구직자의 3개월 이내 재취업확률
n=10: 관찰한 구직자 수
Y: 10명 중 3개월 이내 재취업한 사람의 수
y=6: 실제 관측한 재취업자 수
```

우도:

\[
L(\theta;6)
=P(Y=6\mid\theta)
=\binom{10}{6}\theta^6(1-\theta)^4
\]

### 기준 답안

```python
labor_alpha_post = 4 + 6
labor_beta_post = 6 + 4
labor_posterior_mean = labor_alpha_post / (
    labor_alpha_post + labor_beta_post
)

labor_posterior_mean_8 = (4 + 8) / (4 + 8 + 6 + 2)
```

기준값:

```text
posterior after 6 successes = Beta(10, 10)
posterior mean = 0.5
posterior mean after 8 successes = 0.6
```

학습자가 `theta=10`이라고 하면 `10`은 알려진 표본 수이고 `theta`는 알지 못하는 확률이라는 점을 다시 확인합니다.

---

## 종합평가 지도안

종합평가는 본강의 직후 또는 다음 세션에 실시합니다. 문제를 한꺼번에 제시하지 않고 노트북 순서대로 한 문제씩 진행합니다.

## 객관식 정답

| 문제 | 정답 | 확인할 개념 |
|---|---|---|
| 1 | B | 우도의 조건 방향 |
| 2 | C | 확률변수와 관측값 |
| 3 | B | Beta-Binomial 갱신 |
| 4 | B | 데이터 양과 사후 불확실성 |
| 5 | C | 모형에 없는 가능성 |

정답 문자만 받지 말고 이유를 한 문장으로 설명하게 합니다.

오답 대응:

- 1번 A: 우도와 사후확률의 조건을 나란히 쓰게 한다.
- 2번 D: `n`, `theta`, `Y`, `y`를 표로 다시 채우게 한다.
- 3번: 성공은 `alpha`, 실패는 `beta`에 더한다는 계산을 손으로 하게 한다.
- 4번 A: 성공비율이 0.6이면 많은 데이터가 1이 아니라 0.6 부근에 집중함을 설명한다.
- 5번 A: 유한 가설 모형은 새 후보를 생성하지 않는다는 점을 확인한다.

## 주관식 채점 기준

### 문제 1

필수 요소:

- `P(Y=y | theta)`는 모수를 가정했을 때 데이터의 확률 또는 밀도다.
- `P(theta | Y=y)`는 데이터를 본 뒤 모수에 관한 사후분포다.
- 조건 방향을 압정 사례에 대응한다.

### 문제 2

필수 요소:

- 우도 꼭대기는 데이터만 반영한다.
- 사후분포 꼭대기는 우도와 사전분포를 함께 반영한다.
- 강하거나 비대칭인 사전분포가 있으면 두 꼭대기가 달라질 수 있다.

### 문제 3

필수 요소:

- 연속형 분포에서는 한 점의 확률은 0일 수 있다.
- 구간의 확률은 밀도를 적분해 얻는다.
- 밀도 높이로 주변 값의 상대적인 그럴듯함을 표현한다.

### 문제 4

필수 요소:

```text
theta: 모집단의 3개월 이내 재취업확률
n: 관측한 구직자 수
Y: n명 중 재취업자 수
y: 실제 관측한 재취업자 수
L(theta;y) = C(n,y) theta^y (1-theta)^(n-y)
```

## 프로그래밍 평가 1 정답

```python
def beta_update(alpha_prior, beta_prior, successes, failures):
    alpha_post = alpha_prior + successes
    beta_post = beta_prior + failures
    posterior_mean = alpha_post / (alpha_post + beta_post)
    return alpha_post, beta_post, posterior_mean
```

세 자동 검사 입력을 모두 통과해야 합니다. 한 사례의 숫자를 함수 안에 고정하면 통과시키지 않습니다.

## 프로그래밍 평가 2 참고 풀이

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import beta as beta_dist

priors = [(1, 1), (2, 8), (20, 80)]
successes, failures = 8, 2
p_grid = np.linspace(0.001, 0.999, 1000)

print("prior -> posterior | posterior mean")
for alpha_prior, beta_prior in priors:
    alpha_post, beta_post, mean = beta_update(
        alpha_prior,
        beta_prior,
        successes,
        failures,
    )
    print(
        f"Beta({alpha_prior}, {beta_prior}) "
        f"-> Beta({alpha_post}, {beta_post}) | {mean:.3f}"
    )
    plt.plot(
        p_grid,
        beta_dist(alpha_post, beta_post).pdf(p_grid),
        label=f"prior Beta({alpha_prior}, {beta_prior})",
    )

plt.xlabel("p")
plt.ylabel("Posterior density")
plt.legend()
plt.show()
```

기준 사후분포와 평균:

```text
Beta(1, 1)   -> Beta(9, 3),   mean=0.750
Beta(2, 8)   -> Beta(10, 10), mean=0.500
Beta(20, 80) -> Beta(28, 82), mean≈0.255
```

해석:

- 데이터는 같지만 사전분포의 중심과 농도가 다르다.
- `Beta(20,80)`은 낮은 성공확률에 강한 사전 믿음을 나타내므로 10개 데이터만으로 크게 움직이지 않는다.
- 결론이 사전분포에 민감하다는 사실 자체가 분석 결과다.

## 프로그래밍 평가 3 참고 풀이

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import beta as beta_dist

alpha_prior, beta_prior = 3, 7

regions = {
    "A": {"successes": 12, "failures": 8},
    "B": {"successes": 120, "failures": 80},
}

p_grid = np.linspace(0.001, 0.999, 1000)

for name, data in regions.items():
    alpha_post, beta_post, mean = beta_update(
        alpha_prior,
        beta_prior,
        data["successes"],
        data["failures"],
    )
    interval = beta_dist(alpha_post, beta_post).ppf([0.025, 0.975])
    print(
        name,
        f"Beta({alpha_post}, {beta_post})",
        f"mean={mean:.3f}",
        f"95% CI=({interval[0]:.3f}, {interval[1]:.3f})",
    )
    plt.plot(
        p_grid,
        beta_dist(alpha_post, beta_post).pdf(p_grid),
        label=f"Region {name}",
    )

plt.xlabel("3-month reemployment probability")
plt.ylabel("Posterior density")
plt.legend()
plt.show()
```

기준 결과:

```text
A: Beta(15, 15), mean=0.500, 95% CI≈(0.325, 0.675)
B: Beta(123, 87), mean≈0.586, 95% CI≈(0.519, 0.651)
```

해석:

- 두 지역의 관측 재취업률은 모두 0.6이다.
- A 지역은 자료가 적어 사전평균 0.3의 영향을 더 많이 받는다.
- B 지역은 자료가 많아 사후평균이 관측비율 0.6에 더 가깝다.
- B 지역의 사후분포가 더 좁아 불확실성이 작다.

---

## 영역별 최종 통과 기준

### 개념

다음을 보지 않고 설명해야 합니다.

- 사전분포, 우도, 사후분포
- `P(data | parameter)`와 `P(parameter | data)`의 차이
- 모형에 포함되지 않은 가능성은 데이터가 많아도 발견되지 않는다는 점

### 수식

다음을 사례에 맞게 작성해야 합니다.

\[
Y\mid\theta\sim\operatorname{Binomial}(n,\theta)
\]

\[
L(\theta;y)=\binom{n}{y}\theta^y(1-\theta)^{n-y}
\]

\[
\operatorname{Beta}(\alpha,\beta)
\rightarrow
\operatorname{Beta}(\alpha+y,\beta+n-y)
\]

### 코드

- `bayes_update` 또는 같은 역할의 유한 가설 갱신 함수를 직접 작성한다.
- `beta_update`를 직접 작성하고 변형 입력에서 통과한다.
- 그래프의 위치와 폭을 해석한다.

### 적용

재취업률 사례에서 다음을 구분해야 합니다.

- 모수
- 표본 수
- 확률변수
- 관측값
- 사전분포
- 우도
- 사후분포

## 강의 종료 후 기록

`RESULT.md`에 다음을 추가합니다.

1. 각 체크포인트의 실제 질문과 답변
2. 첫 답변과 수정된 답변
3. 자동 점검 결과
4. 객관식과 주관식의 오답 및 교정
5. 프로그래밍 평가 코드와 해석
6. 영역별 상태와 통과 근거
7. 다음 강의에서 다시 확인할 개념

학습자가 강의 구성 자체에 피드백을 주면 해당 표현을 가능한 한 그대로 기록하고 노트북과 이 지도안을 함께 수정합니다.

## 오프라인 강의 시 예상 출력

컴퓨터를 사용할 수 없을 때 다음 값으로 손계산을 검토할 수 있습니다.

```text
풀이 예제 0-A posterior:
[0.0465, 0.4651, 0.4884]

성공 한 번 posterior:
[0.1333, 0.3333, 0.5333]

성공 후 실패 posterior:
[0.2807, 0.4386, 0.2807]

성공 4, 실패 1, 균등 prior:
[0.0112, 0.2730, 0.7158]

성공 4, 실패 1, strong prior:
[0.1044, 0.4780, 0.4176]

Beta(3,5) + 성공 7, 실패 3:
Beta(10,8), mean=0.5556

노동시장 Beta(4,6) + 성공 6, 실패 4:
Beta(10,10), mean=0.5
```
