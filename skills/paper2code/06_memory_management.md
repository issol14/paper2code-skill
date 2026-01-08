# 메모리 및 컨텍스트 관리 가이드

## 목적
긴 논문 처리 시 **컨텍스트 오버플로우를 방지**하고 **효율적인 작업 진행**을 보장합니다.

---

## 핵심 전략

### 1. 단계별 출력 저장

각 Phase 완료 시 결과를 파일로 저장하여 컨텍스트 부담을 줄입니다:

```
paper_workspace/
├── paper.txt                      # 원본 논문 텍스트
├── 01_algorithm_extraction.yaml   # Phase 1 결과
├── 02_concept_analysis.yaml       # Phase 2 결과
├── 03_implementation_plan.yaml    # Phase 3 결과
└── src/                           # Phase 4 생성 코드
    ├── config.py
    ├── models/
    ├── algorithms/
    └── ...
```

**저장 명령 예시:**
```bash
# Phase 1 결과 저장
cat > paper_workspace/01_algorithm_extraction.yaml << 'EOF'
[Phase 1 YAML 출력]
EOF

# Phase 2 결과 저장
cat > paper_workspace/02_concept_analysis.yaml << 'EOF'
[Phase 2 YAML 출력]
EOF
```

### 2. Phase 간 컨텍스트 전달

다음 Phase로 넘어갈 때 **전체 출력 대신 핵심 요약만 전달**:

```yaml
# Phase 1 → Phase 2 전달 요약
phase1_summary:
  algorithms_found: 3
  key_algorithms:
    - "Algorithm 1: [이름] - [핵심 내용 1줄]"
    - "Algorithm 2: [이름] - [핵심 내용 1줄]"
  hyperparameters_count: 15
  critical_equations: [3, 5, 7, 12]

# Phase 2 → Phase 3 전달 요약
phase2_summary:
  components_count: 5
  implementation_complexity: "Medium"
  key_dependencies:
    - "Component A → Component B"
    - "Component B → Component C"
  experiments_count: 4
```

### 3. 구현 시 메모리 최적화

파일별 구현 사이클에서 컨텍스트 관리:

```
파일 구현 사이클:
┌─────────────────────────────────────────────────────┐
│ 1. 현재 파일 구현에 필요한 정보만 로드               │
│    - implementation_plan.yaml에서 해당 파일 섹션    │
│    - 의존하는 파일의 인터페이스 (전체 코드 X)        │
├─────────────────────────────────────────────────────┤
│ 2. 파일 구현                                        │
├─────────────────────────────────────────────────────┤
│ 3. 구현 완료 후 다음 파일로 이동                    │
│    - 이전 파일 내용은 필요시에만 참조               │
│    - 전체 코드를 메모리에 유지하지 않음             │
└─────────────────────────────────────────────────────┘
```

---

## 긴 논문 처리 팁

### 논문 분할 읽기

논문이 매우 길 경우 섹션별로 분석:

```
읽기 순서 (우선순위):
1. Abstract + Introduction (핵심 기여 파악)
2. Method 섹션 전체 (알고리즘 추출)
3. Experiments 섹션 (환경, baseline, 지표)
4. Appendix (세부 하이퍼파라미터)
5. Related Work (필요시에만)

스킵 가능:
- Related Work 상세 내용 (구현에 불필요)
- 긴 Discussion/Conclusion (요약만)
- Acknowledgments
```

### 큰 알고리즘 분할

복잡한 알고리즘은 하위 컴포넌트로 분할:

```yaml
# 전체를 한 번에 처리하지 않고 분할
large_algorithm:
  component_1:
    extracted: true
    summary: "[요약]"
  component_2:
    extracted: true
    summary: "[요약]"
  component_3:
    extracted: false  # 아직 미처리
```

---

## Self-Monitoring 체크포인트

구현 중 다음 상황에서 **중간 저장** 권장:

```
중간 저장 트리거:
□ 5개 파일 구현 완료마다
□ 복잡한 알고리즘 (50줄 이상) 구현 완료 시
□ 에러 발생 시 현재 상태 저장
□ 새로운 Phase 시작 전
□ 긴 작업 (30분 이상 예상) 시작 전
```

### 저장 체크리스트

```yaml
checkpoint_save:
  current_phase: "[현재 Phase 번호]"
  completed_files:
    - "config.py"
    - "models/network.py"
  current_file: "algorithms/core.py"
  current_progress: "50%"  # 현재 파일 진행률
  next_steps:
    - "[다음 할 일 1]"
    - "[다음 할 일 2]"
  blockers:
    - "[있다면 막힌 부분]"
```

---

## 컨텍스트 복구 프로토콜

대화가 중단되었거나 컨텍스트가 손실된 경우:

```
복구 단계:
1. paper_workspace/ 디렉토리 확인
2. 가장 최근 완료된 Phase 결과 파일 읽기
3. 생성된 코드 파일 목록 확인
4. 마지막 작업 지점 파악
5. 해당 지점부터 재개
```

**복구 명령 예시:**
```bash
# 현재 상태 확인
ls -la paper_workspace/
ls -la paper_workspace/src/

# 마지막 Phase 결과 확인
cat paper_workspace/03_implementation_plan.yaml

# 생성된 파일 확인
find paper_workspace/src -name "*.py" -type f
```

---

## 효율적인 참조 패턴

### 인터페이스만 참조

다른 파일을 참조할 때 **전체 구현이 아닌 인터페이스만** 필요:

```python
# 전체 코드 대신 시그니처만 참조
# models/network.py의 인터페이스:
class NetworkModel:
    def __init__(self, config: Config): ...
    def forward(self, x: Tensor) -> Tensor: ...
    def get_features(self, x: Tensor) -> Tensor: ...
```

### 의존성 그래프 활용

구현 순서를 결정할 때 의존성 그래프 참조:

```
config.py (의존성 없음)
    ↓
utils/helpers.py (config만 의존)
    ↓
models/components.py (config, utils 의존)
    ↓
models/network.py (components 의존)
    ↓
algorithms/core.py (network 의존)
    ↓
training/trainer.py (모두 의존)
```

---

## ⚠️ 주의사항

```
⚠️ MEMORY MANAGEMENT RULES:

1. 전체 논문을 한 번에 처리하지 말 것
   → 섹션별로 나누어 처리

2. 이전 Phase 전체 출력을 다음 Phase에 포함하지 말 것
   → 핵심 요약만 전달

3. 모든 생성 코드를 메모리에 유지하지 말 것
   → 파일로 저장하고 필요시 참조

4. 긴 작업 시 주기적으로 진행 상태 저장
   → 중단 시 복구 가능하도록

5. 불필요한 반복 읽기 피하기
   → 한 번 읽은 정보는 요약하여 보관
```

---

## 권장 워크플로우

```
[논문 입력]
    │
    ▼
[Phase 1: 알고리즘 추출]
    │ → 01_algorithm_extraction.yaml 저장
    │ → 핵심 요약 생성
    ▼
[Phase 2: 개념 분석]
    │ → 02_concept_analysis.yaml 저장
    │ → Phase 1 요약 + Phase 2 요약 유지
    ▼
[Phase 3: 구현 계획]
    │ → 03_implementation_plan.yaml 저장
    │ → 구현에 필요한 핵심 정보만 유지
    ▼
[Phase 4: 코드 구현]
    │ → 파일별로 구현하며 저장
    │ → 5개 파일마다 체크포인트
    ▼
[완료]
```
