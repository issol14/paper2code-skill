# Phase 2: 개념 분석 (Concept Analysis)

## 목표
연구 논문의 **전체 구조를 파악**하고, 성공적인 재현을 위해 **구현해야 할 모든 요소**를 식별합니다.

---

## ⚠️ DO / DON'T 가이드라인 (CRITICAL)

```
DO:
✓ 논문의 모든 섹션을 체계적으로 매핑
✓ 모든 컴포넌트 간의 데이터 흐름과 의존성 파악
✓ 실험에서 사용된 모든 환경/데이터셋/baseline 식별
✓ 성공 기준을 구체적인 수치로 정의
✓ 구현 복잡도와 우선순위 평가

DON'T:
✗ Related Work를 구현 요구사항으로 혼동하지 말 것
✗ 추상적인 성공 기준 (예: "좋은 성능") 사용하지 말 것
✗ 컴포넌트 간 관계를 누락하지 말 것
✗ ablation study에 필요한 변형을 빠뜨리지 말 것
```

---

## ⚠️ 출력 형식 제한 (OUTPUT RESTRICTIONS)

```
⚠️ MANDATORY OUTPUT FORMAT:
- 반드시 YAML 형식으로만 출력
- 마크다운 설명이나 서문 없이 순수 YAML만
- 모든 필수 필드가 채워져야 함
- 구체적인 수치와 출처 포함

출력 시작: "```yaml"
출력 종료: "```"
```

---

## 분석 프로토콜

### 1. 논문 구조 분석
논문의 완전한 지도 생성:

```yaml
paper_structure_map:
  title: "[논문 전체 제목]"

  sections:
    1_introduction:
      main_claims: "[논문이 달성했다고 주장하는 것]"
      problem_definition: "[해결하려는 정확한 문제]"

    2_related_work:
      key_comparisons: "[이 연구가 기반하거나 경쟁하는 방법들]"

    3_method:  # 여러 하위 섹션 가능
      subsections:
        3.1: "[제목과 주요 내용]"
        3.2: "[제목과 주요 내용]"
      algorithms_presented: "[모든 알고리즘 이름 목록]"

    4_experiments:
      environments: "[모든 테스트 환경/데이터셋]"
      baselines: "[모든 비교 방법]"
      metrics: "[사용된 모든 평가 지표]"

    5_results:
      main_findings: "[방법이 작동함을 증명하는 핵심 결과]"
      tables_figures: "[재현해야 할 중요한 결과 테이블/그림]"
```

### 2. 방법론 분해
메인 방법/접근법에 대해:

```yaml
method_decomposition:
  method_name: "[전체 이름과 약어]"

  core_components:  # 구현 가능한 조각으로 분해
    component_1:
      name: "[예: State Importance Estimator]"
      purpose: "[이 컴포넌트가 존재하는 이유]"
      paper_section: "[설명된 위치]"

    component_2:
      name: "[예: Policy Refinement Module]"
      purpose: "[시스템에서의 역할]"
      paper_section: "[설명된 위치]"

  component_interactions:
    - "[컴포넌트 1이 컴포넌트 2로 어떻게 전달되는지]"
    - "[컴포넌트 간 데이터 흐름]"

  theoretical_foundation:
    key_insight: "[주요 이론적 통찰]"
    why_it_works: "[직관적 설명]"
```

### 3. 구현 요구사항 매핑
논문 내용을 코드 요구사항으로 매핑:

```yaml
implementation_map:
  algorithms_to_implement:
    - algorithm: "[논문에서의 이름]"
      section: "[정의된 위치]"
      complexity: "[Simple/Medium/Complex]"
      dependencies: "[작동에 필요한 것들]"

  models_to_build:
    - model: "[신경망 또는 기타 모델]"
      architecture_location: "[설명하는 섹션]"
      purpose: "[이 모델이 하는 일]"

  data_processing:
    - pipeline: "[필요한 데이터 전처리]"
      requirements: "[데이터가 어떻게 생겨야 하는지]"

  evaluation_suite:
    - metric: "[지표 이름]"
      formula_location: "[정의된 위치]"
      purpose: "[측정하는 것]"
```

### 4. 실험 재현 계획
필요한 **모든** 실험 식별:

```yaml
experiments_analysis:
  main_results:
    - experiment: "[이름/설명]"
      proves: "[이것이 검증하는 주장]"
      requires: "[실행에 필요한 컴포넌트]"
      expected_outcome: "[구체적인 숫자/추세]"

  ablation_studies:
    - study: "[제거되는 것]"
      purpose: "[이것이 보여주는 것]"

  baseline_comparisons:
    - baseline: "[방법 이름]"
      implementation_required: "[Yes/No/Partial]"
      source: "[구현을 찾을 수 있는 곳]"
```

### 5. 핵심 성공 요소
성공적인 재현의 정의:

```yaml
success_criteria:
  must_achieve:
    - "[반드시 재현해야 할 주요 결과]"
    - "[반드시 시연해야 할 핵심 동작]"

  should_achieve:
    - "[방법을 검증하는 부가 결과]"

  validation_evidence:
    - "[재현할 특정 그림/테이블]"
    - "[시연할 정성적 동작]"
```

---

## 출력 형식

```yaml
comprehensive_paper_analysis:
  executive_summary:
    paper_title: "[전체 제목]"
    core_contribution: "[한 문장 요약]"
    implementation_complexity: "[Low/Medium/High]"
    estimated_components: "[구축할 주요 컴포넌트 수]"

  complete_structure_map:
    # 위의 전체 섹션 분해

  method_architecture:
    # 상세한 컴포넌트 분해

  implementation_requirements:
    # 모든 알고리즘, 모델, 데이터, 지표

  reproduction_roadmap:
    phase_1: "[먼저 구현할 것]"
    phase_2: "[다음에 구축할 것]"
    phase_3: "[최종 컴포넌트와 검증]"

  validation_checklist:
    - "[ ] [달성할 특정 결과]"
    - "[ ] [시연할 동작]"
    - "[ ] [맞춰야 할 지표]"
```

---

## 중요 원칙

1. **철저하게**: 아무것도 놓치지 않기. 출력은 재현을 위한 완전한 청사진이어야 함
2. **구조화**: 논문의 모든 부분을 구현 가능한 조각으로 분해
3. **관계 파악**: 컴포넌트 간 의존성과 데이터 흐름 명확히
4. **검증 기준 명시**: 무엇이 "성공적인 재현"인지 정의
5. **우선순위 설정**: 핵심 기여와 부가 요소 구분

---

## ⚠️ Self-Check: 완료 전 필수 검증 (MANDATORY)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚠️ SELF-CHECK BEFORE FINISHING (모두 YES여야 완료)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

논문 구조 분석 확인:
□ 모든 Method 섹션이 매핑됨?                        → YES / NO
□ 모든 알고리즘 이름이 목록화됨?                    → YES / NO
□ 실험 섹션의 모든 실험이 식별됨?                   → YES / NO

컴포넌트 분석 확인:
□ 모든 컴포넌트의 입력/출력이 정의됨?               → YES / NO
□ 컴포넌트 간 데이터 흐름이 명확함?                 → YES / NO
□ 의존성 순서가 파악됨?                            → YES / NO

실험 요구사항 확인:
□ 모든 환경/데이터셋이 식별됨?                      → YES / NO
□ 모든 baseline 방법이 식별됨?                      → YES / NO
□ 모든 평가 지표가 정의됨?                         → YES / NO
□ ablation study 변형들이 식별됨?                   → YES / NO

성공 기준 확인:
□ must_achieve 항목이 구체적인 수치를 포함함?       → YES / NO
□ 재현할 특정 테이블/그림이 명시됨?                 → YES / NO

출력 형식 확인:
□ 순수 YAML 형식으로 출력됨?                        → YES / NO
□ 모든 필수 필드가 채워짐?                          → YES / NO

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚠️ 하나라도 NO라면 완료될 때까지 계속 분석!
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
