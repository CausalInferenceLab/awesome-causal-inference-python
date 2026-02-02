# CLAUDE.md

이 문서는 본 저장소의 **Jupyter Notebook 작성·수정·리팩토링을 위한 공통 규칙(Rules)**을 정의한다.  
Claude(Code / Chat)을 포함한 모든 자동화 도구 및 기여자는 **반드시 이 문서를 최우선 기준으로 따른다.**

---

## 1. Repository Goal

이 저장소의 Notebook은 **이론 교재가 아닌 실무 레퍼런스**를 목표로 한다.

- 실무자가 노트북에 랜딩했을 때
  - 코드를 **즉시 실행할 수 있고**
  - 별도의 가공 없이 **바로 레퍼런스로 활용**할 수 있어야 한다.
- 각 방법론의 **Python 구현 방식 자체**가 핵심이며,
  이론적 설명은 최소화한다.
- 필요한 이론은 **외부 reference(논문·교과서)**로 대체한다.

---

## 2. Notebook 공통 템플릿 (필수)

모든 방법론 Notebook은 아래 구조를 **반드시** 따른다.

### [1] 제목 + 한 줄 설명 (Markdown)

- 방법론 이름
- 추정 대상 (ATE / CATE / LATE 등)
- Reference 링크 (논문 또는 교과서)

### [2] Setup (Code)

- 모든 `import`는 **첫 번째 코드 셀**에 모은다.
- 데이터 로드는 import 직후 셀에서 수행한다.
- 전처리는 **최소한으로** 제한한다.

### [3] 핵심 구현 (Code × N cells)

- **라이브러리 기반 구현을 우선**한다.
- 근거가 명확하고 짧은 코드가 최우선이다.
- 결과는 아래 형식 중 하나로 출력한다.
  - `.summary()`
  - `point estimate + confidence interval`

### [4] (선택) 변형 / 비교

- 동일 방법론의 **의미 있는 변형이 있을 경우만** 포함한다.
- 동일 기능의 중복 구현은 금지한다.
- 하나의 **canonical 구현만 유지**한다.

---

## 3. 스타일 통일 규칙 (엄격)

### 3.1 언어 규칙

- **마크다운 제목**: 영어
- **설명 문장**: 한국어
  - 단, ATE / CATE / controls / fixed effects 등  
    **핵심 용어만 영어 병기 허용**
- **코드 주석**: 최소화  
  (코드 자체가 설명이 되도록 작성)

#### 예시

- ❌  
  *Estimate the Average Treatment Effect (ATE) of `married` on `lwage` using OLS with controls and panel fixed effects.*

- ✅  
  *`married`가 `lwage`에 미치는 평균 처치 효과(ATE)를 식별하기 위해,  
  controls와 panel fixed effects를 포함한 OLS를 사용한다.*

---

### 3.2 마크다운 계층

Notebook에서는 제목 계층을 다음과 같이 사용한다.

# Notebook Title
## Section
### Subsection


제목에는 숫자를 포함하지 않는다.

계층은 마크다운 헤더(#, ##, ###)로만 구분한다.

❌ 잘못된 예시
## 2. Fixed Effects
### 2.1 Entity Effects


⭕ 올바른 예시
## Fixed Effects
### Entity Effect

---

### 3.3 첫 셀 규칙

모든 Notebook의 첫 셀은 아래 형식을 따른다.

# Method Name
한국어 한 줄 설명

- Reference: (논문 / 교과서 링크)

---

### 3.4 기타 스타일 규칙

- import: 첫 코드 셀에 모두 포함
- 데이터 로드: import 직후 셀
- 시각화:
  - 방법론 이해에 **필수적인 경우만** 허용
  - 예: RDD plot, Synthetic Control pre/post
- Robustness / Sensitivity:
  - Part 1: 포함하지 않음
  - Part 2: 필요 시 포함 가능

---

## 4. 코드 우선순위 (매우 중요)

모든 구현은 아래 우선순위를 **반드시** 따른다.

1. **근거가 명확한 짧은 코드**
   - 검증된 라이브러리
   - 교과서·논문과 직접적으로 연결 가능
2. 근거가 명확한 긴 코드
3. 근거가 불분명한 짧은 코드
   - 사용 사례가 충분히 검증되지 않은 라이브러리
4. 근거가 불분명한 긴 코드

> 동일 기능의 코드가 여러 개 존재할 경우,  
> **가장 높은 우선순위의 코드 하나만 남기고 제거**한다.

---

## 5. Part 1 / Part 2 구분 원칙

### Part 1: ATE Estimation

- 평균 처치 효과(ATE) 식별과 추정이 핵심인 방법론
- 교과서적·표준적 설정
- Robustness / Sensitivity 분석은 포함하지 않는다.

### Part 2: CATE & Advanced Methods

- 처치 효과의 이질성(CATE) 추정
- 머신러닝, 그래프, 시변 처치, 정책 학습 등 확장 주제
- 필요 시 Robustness / Sensitivity 분석 포함 가능

---

## 6. Canonical Style

- `ols.ipynb`의 구성과 스타일을 **canonical 기준**으로 삼는다.
- 새로운 Notebook 또는 리팩토링 시,
  OLS 노트북과 **형식·톤·밀도**를 일관되게 맞춘다.

---

⚠️ **중요**

이 문서의 규칙을 따르지 않는 Notebook, 코드, 설명은  
**리뷰 없이 수정 또는 제거될 수 있다.**