# Phase 1: 알고리즘 추출 (Algorithm Extraction)

## 목표
연구 논문에서 구현에 필요한 **모든 기술적 세부사항**을 추출합니다.
개발자가 이 추출 결과만으로 논문 전체를 구현할 수 있어야 합니다.

---

## ⚠️ DO / DON'T 가이드라인 (CRITICAL)

```
DO:
✓ 의사코드를 논문에서 정확히 복사 (한 글자도 바꾸지 말 것)
✓ 수식 번호(Eq. X)와 함께 수식을 정확히 기록
✓ 텍스트, 테이블, 캡션, 부록 모든 곳에서 하이퍼파라미터 검색
✓ 누락되었지만 구현에 필수적인 항목 식별 및 기록
✓ 모든 정보에 출처(Section X, Table Y, Page Z) 명시
✓ 변수명, 기호, 첨자를 논문 그대로 유지

DON'T:
✗ 수식이나 의사코드를 "이해하기 쉽게" 수정하지 말 것
✗ 논문에 없는 파라미터 값을 추측하지 말 것
✗ 출처 없이 정보를 기록하지 말 것
✗ "일반적으로 사용되는" 값으로 대체하지 말 것
✗ 불명확한 부분을 건너뛰지 말 것 (missing_but_critical에 기록)
```

---

## ⚠️ 출력 형식 제한 (OUTPUT RESTRICTIONS)

```
⚠️ MANDATORY OUTPUT FORMAT:
- 반드시 YAML 형식으로만 출력
- 마크다운 설명이나 서문 없이 순수 YAML만
- 모든 필수 필드가 채워져야 함
- 정보가 없는 필드는 "Not specified in paper" 기록
- 추측한 값은 "[INFERRED]" 태그 추가

출력 시작: "```yaml"
출력 종료: "```"
```

---

## 추출 프로토콜

### 1. 알고리즘 스캔
논문에서 다음을 찾아 모두 추출합니다:
- Method/Algorithm 섹션의 모든 내용
- Algorithm 박스 (Algorithm 1, 2, 3...)
- 수식과 공식 (모든 Equation)
- 의사코드 (Pseudocode)
- 구현 세부사항 (Implementation Details)

### 2. 알고리즘 심층 추출
발견된 **모든** 알고리즘/방법/절차에 대해:

```yaml
algorithm_name: "[논문에서의 정확한 이름]"
section: "[예: Section 3.2]"
algorithm_box: "[예: Algorithm 1 on page 4]"

pseudocode: |
  [논문의 의사코드를 정확히 복사]
  Input: ...
  Output: ...
  1. Initialize ...
  2. For each ...
     2.1 Calculate ...
  [정확한 포맷과 번호 유지]

mathematical_formulation:
  - equation: "[수식을 정확히 복사, 예: L = L_task + λ*L_explain]"
    equation_number: "[예: Eq. 3]"
    where:
      L_task: "task loss"
      L_explain: "explanation loss"
      λ: "weighting parameter (default: 0.5)"

step_by_step_breakdown:
  1. "[Step 1이 하는 일 상세 설명]"
  2. "[Step 2가 계산하는 것과 이유]"

implementation_details:
  - "Uses softmax temperature τ = 0.1"
  - "Gradient clipping at norm 1.0"
  - "Initialize weights with Xavier uniform"
```

### 3. 컴포넌트 추출
언급된 **모든** 컴포넌트/모듈에 대해:

```yaml
component_name: "[예: Mask Network, Critic Network]"
purpose: "[시스템에서 이 컴포넌트의 역할]"
architecture:
  input: "[shape과 의미]"
  layers:
    - "[Conv2d(3, 64, kernel=3, stride=1)]"
    - "[ReLU activation]"
    - "[BatchNorm2d(64)]"
  output: "[shape과 의미]"

special_features:
  - "[고유한 특징]"
  - "[특별한 초기화 방법]"
```

### 4. 학습 절차 추출
**완전한** 학습 과정 추출:

```yaml
training_loop:
  outer_iterations: "[횟수 또는 조건]"
  inner_iterations: "[횟수 또는 조건]"

  steps:
    1. "Sample batch of size B from buffer"
    2. "Compute importance weights using..."
    3. "Update policy with loss..."

  loss_functions:
    - name: "policy_loss"
      formula: "[정확한 수식]"
      components: "[각 항의 의미]"

  optimization:
    optimizer: "Adam"
    learning_rate: "3e-4"
    lr_schedule: "linear decay to 0"
    gradient_norm: "clip at 0.5"
```

### 5. 하이퍼파라미터 수집
텍스트, 테이블, 캡션 **모든 곳**에서 찾기:

```yaml
hyperparameters:
  # Training
  batch_size: 64
  buffer_size: 1e6
  discount_gamma: 0.99

  # Architecture
  hidden_units: [256, 256]
  activation: "ReLU"

  # Algorithm-specific
  explanation_weight: 0.5
  exploration_bonus_scale: 0.1
  reset_probability: 0.3

  # 출처
  location_references:
    - "batch_size: Table 1"
    - "hidden_units: Section 4.1"
```

---

## 출력 형식

```yaml
complete_algorithm_extraction:
  paper_structure:
    method_sections: "[3, 3.1, 3.2, 3.3, 4]"
    algorithm_count: "[발견된 알고리즘 총 개수]"

  main_algorithm:
    # 위의 형식으로 상세 작성

  supporting_algorithms:
    - # 각 보조 알고리즘의 상세 정보

  components:
    - # 모든 컴포넌트와 아키텍처

  training_details:
    # 완전한 학습 절차

  all_hyperparameters:
    # 모든 파라미터와 값, 출처

  implementation_notes:
    - "[논문에서 언급된 구현 힌트]"
    - "[텍스트에 언급된 트릭]"

  missing_but_critical:
    - "[명시되지 않았지만 필수적인 것]"
    - "[제안하는 기본값과 함께]"
```

---

## 중요 원칙

1. **철저하게**: 개발자가 이 추출 결과**만으로** 전체 논문을 구현할 수 있어야 함
2. **정확하게**: 수식, 변수명, 값을 **정확히** 복사
3. **빠짐없이**: 모든 알고리즘, 모든 수식, 모든 파라미터
4. **출처 명시**: 각 정보가 논문의 어디에서 왔는지 기록
5. **누락 식별**: 논문에 없지만 구현에 필요한 것 식별

---

## ⚠️ Self-Check: 완료 전 필수 검증 (MANDATORY)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚠️ SELF-CHECK BEFORE FINISHING (모두 YES여야 완료)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

알고리즘 추출 확인:
□ 모든 Algorithm 박스 (Algorithm 1, 2, ...)가 추출됨?  → YES / NO
□ Method 섹션의 모든 절차가 포함됨?                   → YES / NO
□ 모든 수식에 Equation 번호가 있음?                   → YES / NO

하이퍼파라미터 확인:
□ 본문에서 언급된 모든 파라미터 수집됨?               → YES / NO
□ 테이블에서 언급된 모든 파라미터 수집됨?             → YES / NO
□ 캡션/부록에서 언급된 파라미터도 확인함?             → YES / NO

완전성 확인:
□ 학습 절차가 완전히 기술됨?                         → YES / NO
□ 손실 함수의 모든 항이 정의됨?                      → YES / NO
□ 누락된 필수 정보가 missing_but_critical에 기록됨?  → YES / NO

출력 형식 확인:
□ 순수 YAML 형식으로 출력됨?                         → YES / NO
□ 모든 필수 필드가 채워짐?                           → YES / NO

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚠️ 하나라도 NO라면 완료될 때까지 계속 추출!
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
