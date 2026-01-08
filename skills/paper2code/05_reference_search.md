# Phase 0: 참조 코드 검색 (Reference Search) - 선택적 단계

## 목적
논문 구현 전 **유사 구현체를 찾아** 구현 품질을 향상시킵니다.
참조 코드는 **영감**을 위한 것이며, 논문 원본 사양이 **항상 우선**합니다.

---

## ⚠️ 중요 원칙 (CRITICAL)

```
⚠️ REFERENCE CODE USAGE PRINCIPLES:

1. 참조 코드는 영감(inspiration)을 위한 것
2. 논문 원본 사양이 항상 우선
3. 복사가 아닌 이해와 적용
4. 참조에서 찾은 패턴도 논문 요구사항에 맞게 수정
5. 라이선스 확인 필수

DO:
✓ 구조와 패턴을 참고
✓ 구현 트릭과 최적화 기법 학습
✓ 일반적인 함정(pitfall) 파악
✓ 테스트 방법론 참고

DON'T:
✗ 코드를 그대로 복사
✗ 참조 구현의 버그도 함께 복사
✗ 논문과 다른 참조의 설계를 따르기
✗ 라이선스 위반
```

---

## 검색 프로토콜

### Step 1: 논문 참조문헌 분석

논문의 References 섹션에서 GitHub 저장소가 있을 가능성이 높은 논문 식별:

```
우선순위가 높은 참조:
1. 방법론/구현 섹션에서 인용된 논문
2. "We build upon..." "We extend..." 등으로 언급된 논문
3. Baseline으로 사용된 방법들
4. 동일 저자의 이전 논문

제외:
- 대상 논문의 공식 구현 (있다면 그냥 사용)
- 순수 이론 논문
- 관련 없는 배경 인용
```

### Step 2: 웹 검색으로 저장소 찾기

Claude의 웹 검색 기능을 활용한 검색 쿼리:

```
검색 쿼리 패턴:

1. 직접 검색:
   - "[논문 제목] GitHub"
   - "[논문 제목] code repository"
   - "[저자 이름] [논문 제목] implementation"

2. 알고리즘 기반 검색:
   - "[알고리즘 이름] PyTorch implementation"
   - "[알고리즘 이름] TensorFlow GitHub"
   - "[핵심 방법론] code example"

3. 키워드 조합:
   - "[핵심 용어1] [핵심 용어2] GitHub stars:>100"
   - "[방법 이름] official implementation"
   - "[데이터셋 이름] [방법 이름] benchmark"

검색 팁:
- 논문의 약어(acronym)와 전체 이름 모두 검색
- 저자의 GitHub 프로필 확인
- Papers With Code (paperswithcode.com) 검색
```

### Step 3: 품질 평가 및 순위화

발견된 저장소를 다음 기준으로 평가:

```yaml
evaluation_criteria:
  repository_quality:  # 40% 가중치
    - stars: "[>100: Good, >500: Excellent]"
    - recent_activity: "[6개월 내 커밋: Active]"
    - documentation: "[README, docstrings 품질]"
    - issues_resolved: "[이슈 응답률]"
    - tests: "[테스트 코드 존재 여부]"

  implementation_relevance:  # 30% 가중치
    - algorithm_match: "[구현된 알고리즘이 논문과 일치하는지]"
    - completeness: "[전체 파이프라인 vs 부분 구현]"
    - paper_citation: "[논문을 인용했는지]"

  technical_depth:  # 20% 가중치
    - code_quality: "[가독성, 구조화 정도]"
    - performance: "[벤치마크 결과가 있는지]"
    - flexibility: "[설정 가능성, 확장성]"

  academic_credibility:  # 10% 가중치
    - author_affiliation: "[저자 소속]"
    - official: "[공식 구현인지]"
    - peer_reviewed: "[논문과 함께 peer review됨]"
```

### Step 4: 상위 5개 선정 및 분석

각 저장소에 대해 다음을 기록:

```yaml
selected_references:
  - rank: 1
    title: "[논문/저장소 제목]"
    repository_url: "[GitHub URL]"
    relevance_score: 0.95  # 0-1 스케일

    key_contributions:
      - "[이 저장소에서 배울 수 있는 것 1]"
      - "[이 저장소에서 배울 수 있는 것 2]"

    implementation_value: |
      [구현에 어떻게 도움이 되는지 상세 설명]

    usage_suggestion: |
      [어떤 부분을 참조할지, 어떻게 적용할지]

    caveats:
      - "[주의할 점 - 논문과 다른 부분]"
      - "[라이선스 제한사항]"
```

---

## 출력 형식

```yaml
reference_search_results:
  search_summary:
    total_found: "[발견된 관련 저장소 수]"
    evaluated: "[평가한 저장소 수]"
    selected: 5

  official_implementation:
    exists: true/false
    url: "[있다면 URL]"
    note: "[공식 구현이 있다면 그것을 우선 사용]"

  selected_references:
    - rank: 1
      title: "..."
      repository_url: "..."
      relevance_score: 0.95
      key_contributions: [...]
      implementation_value: "..."
      usage_suggestion: "..."
      caveats: [...]

    - rank: 2
      # ... 동일 구조

    # ... rank 3, 4, 5

  search_queries_used:
    - "[사용한 검색 쿼리 1]"
    - "[사용한 검색 쿼리 2]"

  papers_with_code_link: "[해당 논문의 PWC 페이지 URL]"
```

---

## 활용 가이드

### 언제 이 단계를 수행할지

```
수행 권장:
✓ 복잡한 알고리즘 구현 시
✓ 논문에 구현 세부사항이 부족할 때
✓ 특정 프레임워크(PyTorch, TensorFlow) 구현 패턴이 필요할 때
✓ 성능 최적화 팁이 필요할 때

스킵 가능:
- 매우 간단한 알고리즘
- 논문에 상세한 구현 설명이 있을 때
- 이미 유사 구현 경험이 있을 때
- 시간이 제한적일 때
```

### 참조 활용 방법

```
1. 구조 참고:
   - 파일 구성 방식
   - 클래스/함수 분리 패턴
   - config 관리 방식

2. 구현 트릭 학습:
   - 수치 안정성 처리
   - 메모리 최적화
   - 병렬화 기법

3. 테스트 방법론:
   - 단위 테스트 구조
   - 통합 테스트 시나리오
   - 벤치마크 스크립트

4. 주의사항 파악:
   - 흔한 버그 패턴
   - 성능 병목점
   - 환경 호환성 이슈
```

---

## ⚠️ Self-Check: 참조 검색 완료 확인

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚠️ REFERENCE SEARCH CHECKLIST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

□ 공식 구현 존재 여부 확인함?                    → YES / NO
□ 최소 3개 이상의 검색 쿼리 시도함?              → YES / NO
□ Papers With Code 확인함?                       → YES / NO
□ 발견된 저장소들의 품질 평가함?                 → YES / NO
□ 상위 5개에 대한 상세 분석 완료함?              → YES / NO
□ 각 참조의 라이선스 확인함?                     → YES / NO
□ 논문과 다른 점(caveats) 기록함?                → YES / NO

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 주의사항

```
⚠️ REMEMBER:

참조 코드는 보조 자료일 뿐입니다.

최종 구현은 반드시 논문 사양을 따라야 합니다.
참조 코드에서 논문과 다른 부분을 발견하면,
논문의 명세를 우선으로 따르세요.

참조 코드의 버그나 논문과의 불일치는
우리 구현에 포함되면 안 됩니다.
```
