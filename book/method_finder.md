# Method Finder

어떤 인과추론 방법을 사용해야 할지 모르겠나요?
아래 질문에 답하며 적절한 방법을 찾아보세요.

---

## Decision Tree

```{mermaid}
flowchart TD
    A{RCT 데이터인가?}

    A -->|Yes| B{Encouragement Design인가?}
    B -->|Yes| IV([<a href='part1/iv.ipynb'>IV</a>])
    B -->|No| C{사전 공변량이 있는가?}
    C -->|Yes| OLS([<a href='part1/ols.ipynb'>OLS</a>])
    C -->|No| DIM([Difference in Means])

    A -->|No| D{처치가 이진인가?}

    D -->|Yes| E{시간 정보가 있는가?}
    E -->|Yes| F{Staggered adoption인가?}
    F -->|Yes| SDID([<a href='part1/sdid.ipynb'>SDID</a>])
    F -->|No| DID([<a href='part1/did.ipynb'>DiD</a>])

    E -->|No| G{Running variable이 있는가?}
    G -->|Yes| RDD([<a href='part1/rdd.ipynb'>RDD</a>])
    G -->|No| H{백도어 기준 만족?}

    H -->|Yes| I{공변량 균형?}
    I -->|Yes| IPW([<a href='part1/ipw_psm.ipynb'>IPW</a>])
    I -->|No| PSM([<a href='part1/ipw_psm.ipynb'>PSM</a>])

    H -->|No| J{유효한 IV가 있는가?}
    J -->|Yes| IV
    J -->|No| K{프론트도어 기준 만족?}
    K -->|Yes| FD([<a href='examples/frontdoor_criterion.ipynb'>Frontdoor</a>])
    K -->|No| NA1([식별 불가])

    D -->|No| L{백도어 기준 만족?}
    L -->|Yes| GPS([<a href='part2/dml.ipynb'>DML</a>])
    L -->|No| NA2([식별 불가])
```

---

## Quick Reference

### 추정 대상별

| 추정 대상 | 방법론 | 노트북 |
|----------|--------|--------|
| **ATE** | OLS, IPW, DiD, SC, SDID | [ols](part1/ols.ipynb), [ipw_psm](part1/ipw_psm.ipynb), [did](part1/did.ipynb), [sc](part1/sc.ipynb) |
| **LATE** | IV, RDD | [iv](part1/iv.ipynb), [rdd](part1/rdd.ipynb) |
| **CATE** | Meta-Learners, DML, Neural Net | [meta_learner](part2/meta_learner.ipynb), [dml](part2/dml.ipynb), [cate_with_nn](part2/cate_with_nn.ipynb) |
| **Policy** | Policy Learning | [policy_learning](part2/policy_learning.ipynb) |

### 데이터 구조별

| 데이터 구조 | 방법론 | 노트북 |
|------------|--------|--------|
| **횡단면** | OLS, IPW, PSM, RDD, IV, DML | [ols](part1/ols.ipynb), [ipw_psm](part1/ipw_psm.ipynb), [rdd](part1/rdd.ipynb), [iv](part1/iv.ipynb), [dml](part2/dml.ipynb) |
| **패널** | DiD, SC, SDID, Fixed Effects | [did](part1/did.ipynb), [sc](part1/sc.ipynb), [sdid](part1/sdid.ipynb), [ols](part1/ols.ipynb) |
| **시변 처치** | MSM | [MSM](part2/MSM.ipynb) |

### 식별 전략별

| 식별 전략 | 핵심 가정 | 방법론 |
|----------|----------|--------|
| **Selection on Observables** | 비관측 교란 없음 | OLS, IPW, PSM, DML, Meta-Learners |
| **Parallel Trends** | 평행 추세 | DiD, SC, SDID |
| **Design-based** | 외생적 변이 | RDD, IV |
| **Graphical** | 백도어/프론트도어 | Backdoor, Frontdoor |

---

## 태그 기반 검색

### 모든 태그 목록

#### Estimand (추정 대상)
- `ate` - Average Treatment Effect
- `att` - Average Treatment Effect on the Treated
- `late` - Local Average Treatment Effect
- `cate` - Conditional Average Treatment Effect
- `policy` - Policy Value / Optimal Policy

#### Method (방법론)
- `regression` - OLS, Fixed Effects
- `matching` - Propensity Score Matching
- `weighting` - IPW, AIPW
- `did` - Difference-in-Differences
- `sc` - Synthetic Control
- `rdd` - Regression Discontinuity Design
- `iv` - Instrumental Variables
- `dml` - Double Machine Learning
- `meta-learner` - S/T/X/R-Learner
- `causal-forest` - Causal Forest, GRF
- `neural-net` - DragonNet, CEVAE
- `causal-discovery` - LiNGAM, PC, GES
- `msm` - Marginal Structural Models

#### Data (데이터 구조)
- `cross-sectional` - 단일 시점
- `panel` - 패널 데이터
- `time-series` - 시계열
- `longitudinal` - 종단 추적

#### Treatment (처치 유형)
- `binary` - 이진 처치
- `continuous` - 연속 처치
- `time-varying` - 시변 처치

#### Assumption (핵심 가정)
- `unconfoundedness` - 비관측 교란 없음
- `overlap` - 공통 지지
- `parallel-trends` - 평행 추세
- `exclusion` - 배제 제한
- `continuity` - 연속성

#### Identification (식별 전략)
- `selection-on-observables` - 관측 조건부
- `design-based` - 설계 기반
- `backdoor` - 백도어 기준
- `frontdoor` - 프론트도어 기준
