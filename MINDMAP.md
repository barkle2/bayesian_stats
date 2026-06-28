# 베이지안 통계 마인드맵

> `CURRICULUM.md`와 동일한 Stage 구조를 사용합니다.
> 실제 진도는 `mindmap.html`의 브라우저 저장소에 기록되며 Markdown 체크리스트와 자동 동기화되지는 않습니다.

## 전체 구조

```text
베이지안 통계학
│
├── Stage 0. 학습 환경과 수학 예비과정
│   ├── Python 재현성
│   │   ├── NumPy · SciPy · pandas · Matplotlib
│   │   ├── 가상환경과 의존성 고정
│   │   └── 난수 시드와 실행 기록
│   ├── 미적분
│   │   ├── 적분과 주변화
│   │   ├── gradient · Hessian
│   │   └── Taylor 전개
│   ├── 선형대수
│   │   ├── 벡터 · 행렬 · 이차형식
│   │   └── 양정치행렬 · Cholesky · SVD
│   └── 수치 안정성
│       ├── 변수변환과 Jacobian
│       └── log-sum-exp
│
├── Stage 1. 확률변수와 분포
│   ├── 확률변수
│   │   ├── PMF · PDF · CDF
│   │   └── 기댓값 · 분산 · 공분산
│   ├── 결합분포
│   │   ├── 결합 · 주변 · 조건부분포
│   │   ├── 독립성과 조건부 독립성
│   │   └── 조건부기댓값
│   ├── 베이즈 정리와 전확률 법칙
│   ├── 주요 분포
│   │   ├── Bernoulli · Binomial · Categorical
│   │   ├── Beta · Dirichlet
│   │   ├── Gamma · Poisson
│   │   └── Normal · Multivariate Normal
│   └── 큰수의 법칙 · 중심극한정리
│
├── Stage 2. 베이지안 추론
│   ├── 생성모형
│   │   ├── 사전분포 p(θ)
│   │   ├── 표본모형 p(y|θ)
│   │   ├── 우도 L(θ;y)
│   │   └── 사후분포 p(θ|y)
│   ├── 주변우도와 순차 갱신
│   ├── 해석적 추론
│   │   ├── Beta-Binomial
│   │   ├── Gamma-Poisson
│   │   ├── Normal-Normal
│   │   └── Dirichlet-Multinomial
│   ├── 충분통계 · 지수족 · 켤레성
│   ├── 사전분포
│   │   ├── 정보적 · 약정보적 · 비정보적
│   │   ├── Jeffreys prior · posterior propriety
│   │   └── 사전민감도
│   ├── 사전예측 · 사후예측
│   └── 손실함수 · Bayes action
│
├── Stage 3. 근사와 Monte Carlo
│   ├── 격자 근사 · 수치적분
│   ├── MAP 최적화
│   ├── 라플라스 근사
│   │   └── Taylor 전개 + Hessian
│   ├── Monte Carlo 적분과 MCSE
│   ├── 중요도 샘플링
│   └── 시뮬레이션 검증
│       ├── 가짜 데이터
│       ├── 모수 회복
│       └── Simulation-based calibration
│
├── Stage 4. MCMC와 PyMC
│   ├── Markov chain · 정상분포 · 상세균형
│   ├── Metropolis-Hastings · Gibbs
│   ├── HMC · NUTS
│   ├── PyMC · ArviZ · InferenceData
│   └── 계산 진단
│       ├── rank-normalized split R-hat
│       ├── bulk/tail ESS · MCSE
│       ├── divergence · energy/BFMI · treedepth
│       └── 재매개화와 스케일 조정
│
├── Stage 5. 베이지안 회귀
│   ├── 생성모형 · 선형예측자 · link function
│   ├── 식별가능성
│   │   ├── 공선성
│   │   └── 로지스틱 완전분리
│   ├── Gaussian 선형회귀
│   ├── Bernoulli 로지스틱회귀
│   ├── Poisson · Negative Binomial 회귀
│   ├── 상호작용 · 비선형 효과
│   └── 예측검사 · 보정 · 영향점
│
├── Stage 6. 계층모형
│   ├── 완전 풀링 · 비풀링 · 부분 풀링
│   ├── 교환가능성 · hyperprior · shrinkage
│   ├── varying intercepts · varying slopes
│   ├── 그룹 효과 상관 · LKJ prior
│   ├── centered · non-centered · funnel geometry
│   └── 기존 그룹과 새로운 그룹 예측
│
├── Stage 7. 베이지안 생존분석
│   ├── 생존함수 · 위험함수 · 누적위험함수
│   ├── censored likelihood
│   ├── 독립적 · 정보적 중도절단
│   ├── truncation · competing risks
│   ├── Exponential · Weibull
│   ├── piecewise exponential · Bayesian Cox
│   └── 시간가변 공변량 · 계층 frailty
│
└── Stage 8. 평가 · 의사결정 · 캡스톤
    ├── 예측 평가
    │   ├── log predictive density · ELPD
    │   ├── PSIS-LOO · Pareto-k
    │   ├── K-fold · grouped/time-based validation
    │   └── stacking · model averaging
    ├── Bayes factor의 목적과 한계
    ├── 의사결정
    │   ├── 행동 · 손실 · 효용
    │   ├── 사후기대손실
    │   └── 정보가치
    └── 노동시장 캡스톤
        ├── estimand · 데이터 사전 · 결측
        ├── 선택편향 · 시간누수 · 공정성
        ├── 계층모형 + 생존모형
        └── 재현 보고서 · 코드 리뷰 · 발표
```

## 핵심 의존관계

트리의 가지뿐 아니라 다음 연결을 함께 기억합니다.

```text
조건부확률 ───────────────→ 베이즈 정리
확률변수와 분포 ──────────→ 생성모형과 우도
적분과 주변화 ────────────→ 사후예측 · 주변우도
Taylor 전개 + Hessian ────→ 라플라스 근사
gradient + 확률기하 ──────→ HMC
큰수의 법칙 + 자기상관 ───→ Monte Carlo 오차 · ESS
사전분포 ─────────────────→ 사전예측검사
식별가능성 ───────────────→ 사전분포 · 모수화 · 진단
계산 진단 ────────────────→ 재매개화 · 모델 수정
사후분포 + 손실함수 ──────→ 베이지안 의사결정
계층모형 + 중도절단 우도 ─→ 계층 생존모형
```

## 반복 워크플로

```text
질문·estimand
      ↓
생성모형과 가짜 데이터
      ↓
사전예측검사
      ↓
적합과 계산 진단
      ↓
사후예측검사
      ↓
민감도·모델 비교
      ↓
의사결정과 모델 수정 ─────→ 처음으로 반복
```

## 노동시장 분석 연결

```text
실업급여 신청자 데이터
│
├── 재취업 여부 ─────────────→ 로지스틱회귀
├── 재취업까지 걸린 기간 ────→ 생존분석
├── 지역·산업별 차이 ────────→ 계층모형
├── 실업급여 종료·이탈 ──────→ 경쟁사건
├── 지원 대상 선정 ──────────→ 손실함수와 의사결정
└── 정책 효과 해석 ──────────→ 예측과 인과의 구분
```
