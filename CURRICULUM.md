# 베이지안 통계 학습 커리큘럼

## 목표

- 베이지안 통계를 대학원 석사 수준으로 설명하고 유도한다.
- Python, PyMC, ArviZ로 재현 가능한 분석을 구현한다.
- 모델을 적합하는 데서 끝나지 않고 예측검사, 계산 진단, 민감도 분석을 수행한다.
- 실업급여 신청자의 재취업 여부와 기간을 계층·생존모형으로 분석한다.
- 불확실성을 정책 또는 업무 의사결정으로 연결한다.
- 방통대 수업자료를 관련 단계에 연결해 복습하고 학습 증거를 남긴다.

## 튜터 호출

| 호출명 | 주 역할 |
|---|---|
| `@베이즈` | 직관, 조건부확률, 사전-우도-사후, 작은 Python 실험 |
| `@라플라스` | 수학적 유도, 해석적 추론, 근사, 의사결정이론 |
| `@겔만` | 생성모형, PyMC, 계층모형, 진단, 현대 베이지안 워크플로 |
| `@모두` | 세 관점을 베이즈 → 라플라스 → 겔만 순서로 종합 |

## 반복 학습 워크플로

Stage 2부터 모든 모델은 다음 순서를 반복합니다.

1. 질문과 추정대상(estimand)을 정의한다.
2. 데이터 생성과정을 확률모형으로 표현한다.
3. 모의 데이터를 만들고 모수 회복 여부를 확인한다.
4. 사전예측분포로 사전분포의 함의를 점검한다.
5. 모델을 적합하고 계산 진단을 확인한다.
6. 사후예측분포로 관측자료의 특징을 재현하는지 확인한다.
7. 사전분포, 모수화, 관측가정을 바꾸어 민감도를 확인한다.
8. 결과를 의사결정과 다음 모델 수정으로 연결한다.

## 숙달 판정

각 개념은 단순 체크가 아니라 다음 네 가지 학습 증거로 판정합니다.

| 영역 | 통과 증거 |
|---|---|
| 개념 | 자신의 말로 설명하고 대표적인 오해 하나를 교정한다. |
| 수식 | 핵심 결과를 유도하거나 작은 예제를 손으로 계산한다. |
| 코드 | 빈 노트북에서 구현하고 테스트 또는 예상값과 대조한다. |
| 적용 | 새 데이터에서 가정을 명시하고 결과와 한계를 해석한다. |

평가 상태는 `미평가 → 연습 중 → 통과 → 재점검 필요`로 관리합니다. 각 Stage를 마칠 때 다음 산출물을 남깁니다.

- 개념 설명 1쪽
- 손계산 또는 유도 문제
- 실행 가능한 Python 노트북
- 진단표와 해석
- 방통대 자료 연결 및 복습 기록

## 방통대 자료 연결 규칙

추가할 수업자료는 `knou_materials/`에 원본 그대로 보관하고, `knou_materials/INDEX.md`에 다음 정보를 기록합니다.

- 과목명, 학기, 강의 또는 장 번호
- 연결되는 Stage와 개념 ID
- 이미 아는 내용과 다시 볼 내용
- 복습 문제와 결과

자료가 추가되면 먼저 Stage 0 진단에 반영하고, 관련 Stage에서 다시 불러옵니다.

---

## Stage 0 - 학습 환경과 수학 예비과정 (3~5주)

> 추천: `@라플라스`, `@겔만`

### 0.1 Python 분석 환경

- [ ] NumPy 배열, 브로드캐스팅, 벡터화
- [ ] SciPy 확률분포와 수치 계산
- [ ] pandas 데이터 정리와 Matplotlib 시각화
- [ ] 가상환경, 의존성 고정, 난수 시드, 재현 가능한 노트북

### 0.2 미적분과 선형대수

- [ ] 미분, 적분, 다변수 미분
- [ ] Taylor 전개, gradient, Hessian
- [ ] 벡터·행렬 연산과 이차형식
- [ ] 양정치행렬, Cholesky 분해, 고유값과 SVD의 의미

### 0.3 확률수학 준비

- [ ] 합과 적분을 이용한 주변화
- [ ] 변수변환과 Jacobian
- [ ] 로그, 지수, log-sum-exp와 수치 안정성

### 0.4 진단 산출물

- [ ] `stage_00_foundations/01_diagnostic.ipynb`
- [ ] `stage_00_foundations/02_math_for_bayes.ipynb`
- [ ] 방통대 자료 초기 매핑

---

## Stage 1 - 확률변수, 분포, 조건부확률 (4~5주)

> 추천: `@베이즈`, `@라플라스`

### 1.1 확률의 언어

- [ ] 표본공간, 사건, 확률 공리
- [ ] 확률변수와 확률분포
- [ ] PMF, PDF, CDF와 분위수
- [ ] 기댓값, 분산, 공분산, 상관

### 1.2 결합분포

- [ ] 결합·주변·조건부분포
- [ ] 독립성과 조건부 독립성
- [ ] 전확률 법칙과 베이즈 정리
- [ ] 조건부기댓값과 반복기댓값 법칙

### 1.3 주요 분포와 극한

- [ ] Bernoulli, Binomial, Categorical
- [ ] Beta, Dirichlet, Gamma, Poisson
- [ ] Normal과 다변량 Normal
- [ ] 큰수의 법칙과 중심극한정리

### 1.4 실습 산출물

- [x] 1강 대화형 본강의: 증거로 믿음 갱신하기
- [ ] `lessons/lesson_01/lesson_01.ipynb` - 개정 워크북 종합평가
- [ ] `stage_01_probability/01_random_variables.ipynb`
- [ ] `stage_01_probability/02_conditional_probability.ipynb`
- [ ] `stage_01_probability/03_distributions_simulation.ipynb`

---

## Stage 2 - 베이지안 추론과 해석적 모델 (4~6주)

> 추천: `@베이즈`, `@라플라스`

### 2.1 생성모형과 베이즈 갱신

- [ ] 사전분포, 표본모형, 우도, 사후분포
- [ ] 우도는 모수의 확률분포가 아니라는 구분
- [ ] 주변우도와 정규화
- [ ] 순차 갱신과 교환가능성

### 2.2 정확한 해석적 추론

- [ ] Beta-Binomial
- [ ] Gamma-Poisson
- [ ] Normal-Normal
- [ ] Dirichlet-Multinomial
- [ ] 충분통계, 지수족, 켤레성

### 2.3 사전분포와 예측

- [ ] 정보적·약정보적·비정보적 사전분포
- [ ] improper prior와 posterior propriety
- [ ] Jeffreys prior와 Fisher information
- [ ] 사전예측·사후예측분포
- [ ] 사전분포 민감도 분석

### 2.4 사후요약과 결정

- [ ] 사후 평균·중앙값·MAP
- [ ] 크레디블 구간과 신뢰구간
- [ ] 손실함수와 Bayes action

### 2.5 실습 산출물

- [x] 2강 대화형 본강의: 생성모형과 우도
- [ ] `stage_02_inference/01_grid_update.ipynb`
- [ ] `stage_02_inference/02_conjugate_models.ipynb`
- [ ] `stage_02_inference/03_predictive_checks.ipynb`
- [ ] `stage_02_inference/04_prior_sensitivity.ipynb`

---

## Stage 3 - 근사와 Monte Carlo (4~6주)

> 추천: `@라플라스`, `@겔만`

### 3.1 정확 계산에서 근사로

- [ ] 격자 근사와 수치적분
- [ ] MAP 최적화
- [ ] Taylor 전개와 라플라스 근사
- [ ] 정확값·근사값·근사오차 비교

### 3.2 Monte Carlo

- [ ] 직접 샘플링과 Monte Carlo 적분
- [ ] Monte Carlo 표준오차
- [ ] 중요도 샘플링과 가중치 붕괴
- [ ] 선택 주제: 변분추론의 목적과 한계

### 3.3 시뮬레이션 검증

- [ ] 가짜 데이터 생성
- [ ] 모수 회복(parameter recovery)
- [ ] Simulation-based calibration 입문

### 3.4 실습 산출물

- [ ] `stage_03_approximation/01_grid_and_quadrature.ipynb`
- [ ] `stage_03_approximation/02_laplace_approximation.ipynb`
- [ ] `stage_03_approximation/03_monte_carlo.ipynb`
- [ ] `stage_03_approximation/04_parameter_recovery.ipynb`

---

## Stage 4 - MCMC와 PyMC (6~8주)

> 추천: `@겔만`, 필요시 `@라플라스`

### 4.1 MCMC 원리

- [ ] Markov chain, 정상분포, 상세균형
- [ ] 자기상관과 유효표본크기
- [ ] Metropolis-Hastings와 Gibbs sampling
- [ ] HMC의 기하와 NUTS

### 4.2 PyMC 워크플로

- [ ] 모델 정의와 `InferenceData`
- [ ] 사전예측·사후예측 샘플링
- [ ] ArviZ 요약과 시각화
- [ ] 결과 저장과 재현

### 4.3 계산 진단과 수정

- [ ] rank-normalized split R-hat
- [ ] bulk/tail ESS와 MCSE
- [ ] divergence, energy/BFMI, treedepth
- [ ] centered/non-centered 모수화와 스케일 조정
- [ ] 진단 실패 원인과 수정 기록

### 4.4 실습 산출물

- [ ] `stage_04_mcmc/01_metropolis_hastings.ipynb`
- [ ] `stage_04_mcmc/02_hmc_geometry.ipynb`
- [ ] `stage_04_mcmc/03_pymc_workflow.ipynb`
- [ ] `stage_04_mcmc/04_diagnostics_and_repairs.ipynb`

---

## Stage 5 - 베이지안 회귀와 식별가능성 (5~7주)

> 추천: `@겔만`, `@라플라스`

### 5.1 회귀의 공통 구조

- [ ] 생성모형, 선형예측자, link function
- [ ] 예측변수 표준화와 약정보적 사전분포
- [ ] 식별가능성, 공선성, 완전분리
- [ ] 상호작용과 비선형 효과

### 5.2 모델

- [ ] Gaussian 선형회귀
- [ ] Bernoulli 로지스틱회귀
- [ ] Poisson·Negative Binomial 회귀
- [ ] 과산포와 zero inflation

### 5.3 모델별 점검

- [ ] 모의 데이터에서 모수 회복
- [ ] prior predictive check
- [ ] 계산 진단과 posterior predictive check
- [ ] 예측 보정, 잔차적 패턴, 이상치와 영향점

### 5.4 실습 산출물

- [ ] `stage_05_regression/01_linear_regression.ipynb`
- [ ] `stage_05_regression/02_logistic_reemployment.ipynb`
- [ ] `stage_05_regression/03_count_models.ipynb`
- [ ] `stage_05_regression/04_identifiability.ipynb`

---

## Stage 6 - 계층모형 (6~8주)

> 추천: `@겔만`

### 6.1 부분 풀링

- [ ] 완전 풀링·비풀링·부분 풀링
- [ ] 교환가능성과 조건부 교환가능성
- [ ] hyperprior와 shrinkage
- [ ] 기존 그룹과 새로운 그룹의 예측

### 6.2 다층 회귀

- [ ] varying intercepts와 varying slopes
- [ ] 그룹 효과의 상관과 LKJ prior
- [ ] 그룹 수준 예측변수
- [ ] centered/non-centered 선택과 funnel geometry

### 6.3 노동시장 적용

- [ ] 지역·산업별 재취업률
- [ ] 개인 특성과 지역 효과 분리
- [ ] 그룹 수와 표본 크기에 따른 민감도

### 6.4 실습 산출물

- [ ] `stage_06_hierarchical/01_partial_pooling.ipynb`
- [ ] `stage_06_hierarchical/02_varying_effects.ipynb`
- [ ] `stage_06_hierarchical/03_labor_market_multilevel.ipynb`

---

## Stage 7 - 베이지안 생존분석 (6~8주)

> 추천: `@겔만`, `@라플라스`

### 7.1 관측과정과 우도

- [ ] 생존함수, 위험함수, 누적위험함수
- [ ] 중도절단 우도 유도
- [ ] 독립적·정보적 중도절단
- [ ] truncation과 competing risks

### 7.2 생존모형

- [ ] Exponential·Weibull 모형
- [ ] piecewise exponential 모형
- [ ] 비례위험 가정과 Bayesian Cox 모형
- [ ] 시간가변 공변량과 계층 frailty

### 7.3 실업기간 적용

- [ ] 행정적 관측종료와 재취업 정의
- [ ] 재취업과 실업급여 종료의 경쟁사건
- [ ] 생존모형과 계층모형 결합
- [ ] censoring과 tail에 대한 예측검사

### 7.4 실습 산출물

- [ ] `stage_07_survival/01_censored_likelihood.ipynb`
- [ ] `stage_07_survival/02_parametric_survival.ipynb`
- [ ] `stage_07_survival/03_unemployment_duration.ipynb`

---

## Stage 8 - 모델 평가, 의사결정, 캡스톤 (5~7주)

> 추천: `@모두`

### 8.1 예측 평가와 모델 비교

- [ ] log predictive density와 ELPD
- [ ] PSIS-LOO와 Pareto-k 진단
- [ ] reloo, K-fold, grouped/time-based validation
- [ ] WAIC의 역할과 한계
- [ ] stacking과 model averaging
- [ ] Bayes factor의 목적, 사전민감도, 사용 조건

### 8.2 베이지안 의사결정

- [ ] 행동공간, 손실함수, 효용함수
- [ ] 사후기대손실과 최적 행동
- [ ] 오판 비용과 분류 임계값
- [ ] 정보가치와 추가 데이터 수집

### 8.3 노동시장 캡스톤

- [ ] 문제와 estimand 정의
- [ ] 데이터 사전, 결측, 선택편향, 시간누수 점검
- [ ] 기준모형과 계층·생존모형 비교
- [ ] 계산 진단, 예측검사, 민감도 분석
- [ ] 공정성과 정책적 한계 검토
- [ ] 재현 가능한 보고서, 코드 리뷰, 발표

### 8.4 최종 산출물

- [ ] `stage_08_capstone/01_model_comparison.ipynb`
- [ ] `stage_08_capstone/02_decision_analysis.ipynb`
- [ ] `stage_08_capstone/final_report.md`

---

## 전체 진도

| 단계 | 주제 | 예상 기간 | 상태 |
|---|---|---:|---|
| Stage 0 | 환경·수학 예비과정 | 3~5주 | 시작 전 |
| Stage 1 | 확률변수와 분포 | 4~5주 | 시작 전 |
| Stage 2 | 베이지안 추론 | 4~6주 | 시작 전 |
| Stage 3 | 근사와 Monte Carlo | 4~6주 | 시작 전 |
| Stage 4 | MCMC와 PyMC | 6~8주 | 시작 전 |
| Stage 5 | 베이지안 회귀 | 5~7주 | 시작 전 |
| Stage 6 | 계층모형 | 6~8주 | 시작 전 |
| Stage 7 | 생존분석 | 6~8주 | 시작 전 |
| Stage 8 | 평가·의사결정·캡스톤 | 5~7주 | 시작 전 |

전체 기간은 약 43~60주입니다. Stage 0 진단 결과와 방통대 복습 범위에 따라 단축할 수 있습니다.
