# Paper2Code Skill for Claude Code

[English](README.md) | **한국어**

> 연구 논문을 구조화된 다단계 파이프라인으로 실행 가능한 코드로 변환합니다.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude-Code-blueviolet)](https://claude.ai/code)
[![Agent Skills](https://img.shields.io/badge/Agent-Skills-green)](https://agentskills.io)

## 왜 만들었나요?

[DeepCode](https://github.com/HKUDS/DeepCode) 같은 훌륭한 논문-코드 변환 도구들이 있지만, 실행할 때마다 별도의 API 비용이 발생합니다. 이미 **Claude Code**를 구독하고 있다면, 왜 추가 비용을 내야 할까요?

이 스킬은 동일한 구조화된 다단계 접근 방식을 Claude Code 안으로 가져옵니다 — **추가 API 비용 없이**, 기존 구독만으로 사용할 수 있습니다.

## 개요

**Paper2Code**는 연구 논문(PDF/arXiv)을 완전히 기능하는 재현 가능한 코드로 체계적으로 변환하는 Claude Code Skill입니다. 단순히 논문을 LLM에 넣는 naive한 접근 방식과 달리, 이 스킬은 **구조화된 중간 표현(YAML)**을 사용하여 정확성과 완전성을 보장합니다.

### 주요 기능

- **4+2 단계 파이프라인**: 알고리즘 추출 → 개념 분석 → 코드 계획 → 구현 (+ 참조 검색 & 메모리 관리)
- **구조화된 YAML 중간 표현**: 단순한 코드 생성이 아닌 체계적인 지식 추출
- **Self-Check 메커니즘**: 각 단계에서 완전성을 보장하는 내장 검증
- **행동 제어**: 일반적인 구현 실수를 방지하는 DO/DON'T 가이드라인
- **참조 기반 생성**: 구현 품질 향상을 위한 선택적 참조 코드 검색

## 빠른 시작

### 설치

**옵션 1: 개인 설치 (권장)**
```bash
# 저장소 클론
git clone https://github.com/issol14/paper2code-skill.git

# Claude skills 디렉토리에 복사
cp -r paper2code-skill/skills/paper2code ~/.claude/skills/
```

**옵션 2: 프로젝트 설치**
```bash
# 프로젝트의 .claude/skills 디렉토리에 추가
mkdir -p .claude/skills
cp -r paper2code-skill/skills/paper2code .claude/skills/
```

**옵션 3: Claude에게 맡기기**

Claude Code에 이것만 붙여넣으세요:
```
Install the paper2code skill from https://github.com/issol14/paper2code-skill
```

### 사용법

설치 후, 논문 구현을 요청하면 Claude Code가 자동으로 스킬을 활성화합니다:

```
# arXiv URL로
"https://arxiv.org/abs/2301.12345 이 논문 구현해줘"

# PDF 파일로
"/path/to/paper.pdf 이 논문의 알고리즘을 구현해줘"

# 특정 섹션만
"이 논문에서 Section 3의 알고리즘만 구현해줘"
```

### 상세 사용 예시

#### 예시 1: 전체 논문 구현
```
User: https://arxiv.org/abs/2312.00752 이 논문을 구현해줘

Claude: 논문을 분석하고 코드로 변환하겠습니다.

[Phase 1: 알고리즘 추출 중...]
→ 01_algorithm_extraction.yaml 저장

[Phase 2: 개념 분석 중...]
→ 02_concept_analysis.yaml 저장

[Phase 3: 구현 계획 수립 중...]
→ 03_implementation_plan.yaml 저장

[Phase 4: 코드 구현 중...]
→ config.py 생성
→ models/network.py 생성
→ ...
→ main.py 생성
→ README.md 생성

구현이 완료되었습니다. `python main.py`로 실행할 수 있습니다.
```

#### 예시 2: 참조 검색과 함께
```
User: 이 논문 구현해줘. 참조할 만한 구현체도 먼저 찾아봐.

Claude: 참조 코드를 먼저 검색한 후 구현하겠습니다.

[Phase 0: 참조 코드 검색 중...]
→ 5개의 관련 구현체 발견
→ reference_search.yaml 저장

[Phase 1-4 진행...]
```

#### 예시 3: 특정 알고리즘만
```
User: 이 논문에서 Algorithm 2의 Self-Attention 부분만 구현해줘

Claude: Algorithm 2의 Self-Attention 구현에 집중하겠습니다.
[해당 알고리즘만 추출하여 구현...]
```

### 출력 구조

구현 후 다음과 같은 결과물을 얻습니다:
```
paper_workspace/
├── 01_algorithm_extraction.yaml   # 추출된 알고리즘 & 수식
├── 02_concept_analysis.yaml       # 논문 구조 분석
├── 03_implementation_plan.yaml    # 상세 구현 계획
└── src/
    ├── config.py                  # 하이퍼파라미터 & 설정
    ├── models/
    │   ├── __init__.py
    │   └── network.py             # 신경망 아키텍처
    ├── algorithms/
    │   └── core.py                # 메인 알고리즘 구현
    ├── training/
    │   ├── losses.py              # 손실 함수
    │   └── trainer.py             # 학습 루프
    ├── evaluation/
    │   └── metrics.py             # 평가 지표
    ├── main.py                    # 진입점
    ├── requirements.txt           # 의존성
    └── README.md                  # 사용 문서
```

## 파이프라인 개요

```
[논문 입력: PDF/arXiv URL]
        │
        ▼
┌─────────────────────────────────────┐
│ Phase 0: 참조 검색 (선택적)          │
│ → 유사 구현체 찾기                   │
└─────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────┐
│ Phase 1: 알고리즘 추출               │
│ → 모든 알고리즘, 수식 추출           │
│ → 출력: YAML 명세                    │
└─────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────┐
│ Phase 2: 개념 분석                   │
│ → 논문 구조 매핑                     │
│ → 컴포넌트 & 실험 식별               │
└─────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────┐
│ Phase 3: 구현 계획                   │
│ → 5개 섹션 상세 계획                 │
│ → 파일 구조 & 의존성                 │
└─────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────┐
│ Phase 4: 코드 구현                   │
│ → 파일별 구현                        │
│ → 완전한 실행 가능 코드베이스        │
└─────────────────────────────────────┘
```

## 스킬 구조

```
paper2code/
├── SKILL.md                      # 메인 스킬 진입점
├── 01_algorithm_extraction.md    # Phase 1: 알고리즘 추출 프로토콜
├── 02_concept_analysis.md        # Phase 2: 논문 구조 분석
├── 03_code_planning.md           # Phase 3: 구현 계획
├── 04_implementation_guide.md    # Phase 4: 코드 생성 가이드
├── 05_reference_search.md        # Phase 0: 참조 코드 검색 (선택적)
└── 06_memory_management.md       # 컨텍스트/메모리 관리 가이드
```

## 차별점

| 측면 | Naive 접근 | Paper2Code Skill |
|--------|---------------|------------------|
| 프로세스 | 논문 → 코드 직접 변환 | 구조화된 다단계 파이프라인 |
| 중간 표현 | 없음 | YAML 지식 표현 |
| 검증 | 수동 | 각 단계 내장 self-check |
| 완전성 | 종종 부분적 | 체크리스트로 체계적 |
| 재현성 | 일관성 없음 | 명시적 성공 기준 |

## 핵심 원칙

### 행동 제어
```
DO:
✓ 논문에 명시된 것을 정확히 구현
✓ 간단하고 직접적인 코드 작성
✓ 각 컴포넌트 즉시 테스트
✓ 허락 없이 다음 파일로 이동

DON'T:
✗ "다음 파일을 구현할까요?" 묻기
✗ 과도한 추상화나 불필요한 추가
✗ 불명확한 부분 건너뛰기 (missing_but_critical에 문서화)
✗ 논문에 없는 파라미터 값 추측
```

### 품질 기준
- **완전성**: 플레이스홀더나 TODO 없음
- **정확성**: 논문의 정확한 수식, 파라미터
- **실행 가능성**: 에러 없이 코드 실행
- **재현 가능성**: 논문 결과 재현 가능

## 요구사항

- **Claude Code** (Claude 구독 필요)
- **pdftotext** (PDF 처리용): `sudo apt install poppler-utils`

## FAQ

<details>
<summary><b>Q: 어떤 종류의 논문에 적합한가요?</b></summary>

주로 **ML/DL 연구 논문**에 최적화되어 있지만, 알고리즘이 명확히 기술된 논문이라면 대부분 사용 가능합니다:
- 딥러닝 모델 (Transformer, CNN, GNN 등)
- 강화학습 알고리즘
- 최적화 알고리즘
- 데이터 처리 파이프라인

</details>

<details>
<summary><b>Q: 구현 결과가 논문과 다르면 어떻게 하나요?</b></summary>

1. 생성된 YAML 파일들을 확인하여 알고리즘 추출이 정확한지 검토
2. `missing_but_critical` 섹션에 누락된 정보가 있는지 확인
3. 논문의 Appendix나 Supplementary Material 추가 제공
4. 특정 부분 재구현 요청: "Algorithm 2의 loss 계산 부분을 다시 구현해줘"

</details>

<details>
<summary><b>Q: 긴 논문도 처리할 수 있나요?</b></summary>

네, `06_memory_management.md`의 가이드라인을 따라 긴 논문도 처리합니다:
- 섹션별 분할 분석
- 중간 결과 YAML 저장으로 컨텍스트 관리
- 필요시 복구 가능한 체크포인트

</details>

<details>
<summary><b>Q: 참조 코드 검색은 언제 사용하나요?</b></summary>

다음 경우에 유용합니다:
- 논문에 구현 세부사항이 부족할 때
- 특정 프레임워크 패턴이 필요할 때
- 복잡한 알고리즘의 구현 트릭을 참고하고 싶을 때

"참조 코드도 찾아봐" 또는 "비슷한 구현체 먼저 검색해줘"라고 요청하면 됩니다.

</details>

<details>
<summary><b>Q: 생성된 코드의 품질은 어떻게 보장되나요?</b></summary>

각 Phase에 **Self-Check 메커니즘**이 내장되어 있습니다:
- Phase 1: 모든 알고리즘/수식 추출 확인
- Phase 2: 컴포넌트 관계 및 실험 요구사항 확인
- Phase 3: 5개 필수 섹션 및 분량 확인
- Phase 4: 최종 완료 체크리스트 (실행 가능성, 재현성 등)

</details>

## 감사의 말

이 스킬은 HKU Data Intelligence Lab의 [DeepCode](https://github.com/HKUDS/DeepCode)에서 영감을 받았습니다. DeepCode는 다중 에이전트 오케스트레이션을 통한 논문-코드 변환의 구조화된 접근 방식을 개척했습니다.

## 라이선스

MIT License - 자세한 내용은 [LICENSE](LICENSE)를 참조하세요.

## 기여

기여를 환영합니다! 이슈나 풀 리퀘스트를 자유롭게 제출해주세요.

---

**참고**: 이 스킬은 Claude Code용으로 설계되었습니다. Agent Skills 표준에 대한 정보는 [agentskills.io](https://agentskills.io)를 참조하세요.
