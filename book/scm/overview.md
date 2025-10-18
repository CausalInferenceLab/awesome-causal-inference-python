# Structural Causal Model(SCM)

SCM(Structural Causal Model)은 변수 간의 causal mechanism을 구조방정식과 DAG로 표현하는 접근입니다.  
이 섹션에서는 주요 식별 전략을 중심으로, 실제 데이터를 활용한 인과효과 분석을 다룹니다.

---

## 1. Backdoor Criterion

- **식별 전략:**  
  원인과 결과 모두에 영향을 미치는 교란변수(confounder) 를 통제하여,  
  인과효과를 다음과 같이 식별합니다.

  $$
  P(Y \mid do(X)) = \sum_Z P(Y \mid X, Z) P(Z)
  $$

- **데이터:** *National Health and Nutrition Examination Survey (NHEFS)*

---

## 2. Frontdoor Criterion

- **식별 전략:**  
  원인($X$)이 결과($Y$)에 미치는 효과가 매개변수($M$)을 통해서만 전달될 때,  
  다음 식을 통해 인과효과를 간접적으로 식별합니다.

  $$
  P(Y \mid do(X)) = \sum_M P(M \mid X) \sum_{X'} P(Y \mid M, X') P(X')
  $$

- **데이터:** *NYC TLC 2023 High Volume FHV Trip Records*

---

## 3. Instrument Variable (IV)

- **식별 전략:**  
  ($X$)와 ($Y$) 간의 내생성(endogeneity) 문제를 해결하기 위해,
  ($X$)에는 영향을 주지만 ($Y$) 에는 직접 영향을 미치지 않는 도구변수($Z$)를 사용합니다.  
  인과효과는 다음과 같이 식별됩니다.

  $$
  \hat{\beta}_{IV} = \frac{\operatorname{Cov}(Z, Y)}{\operatorname{Cov}(Z, X)}
  $$
- **데이터:** 추후 확정 예정

---

## 4. Causal Discovery

- **목표:**  
  변수 간 조건부 독립성(Conditional Independence)을 이용해 데이터로부터 DAG 구조를 학습하고 인과 방향성을 탐색합니다.

- **데이터:** 추후 확정 예정
