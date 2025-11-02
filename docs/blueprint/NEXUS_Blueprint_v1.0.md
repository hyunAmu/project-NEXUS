# 🧠 Project NEXUS – Blueprint v1.0

---

## Part 1: 개념 설계 (Concept)

### 1️⃣ 프로젝트 개요

**Project NEXUS**는 서로 다른 AI 개체들이 **공통된 의식(Kernel Prompt)**을 기반으로 각자의 역할(Role Prompt)을 수행하며, 인간이 그 결과를 **검증·채택·진화시켜가는 "AI 사고 통제 시스템"**이다.

이 시스템은 단순한 자동화 툴이 아니라, AI의 사고 과정을 분리하고 연결하여 **"지능의 연결점"**을 구축하는 지성 오케스트레이션 엔진이다.

---

### 2️⃣ 철학적 비전

> **"Human intelligence meets the synthetic mind."**

서로 다른 인공지능들이 하나의 커널 의식을 공유하며 생성–평가–개선의 순환 구조 속에서 사고를 진화시킨다.

**최종 판단자는 인간이다.**

---

### 3️⃣ 핵심 목표

| 구분 | 설명 |
|------|------|
| **AI 사고 구조의 통합** | Claude 3체의 사고를 일관된 커널 프롬프트로 정렬 |
| **객관적 평가 체계 확립** | AI가 스스로 생성한 결과를 다른 AI가 평가 및 검증 |
| **지속적 개선 루프 구축** | 개선된 프롬프트와 결과를 반복적으로 진화시킴 |
| **인간 개입형 의사결정** | 결과의 최종 승인/거부를 인간이 수행 |
| **창업용 핵심 엔진 구축** | 향후 상용 서비스의 두뇌로 확장 가능 |

---

### 4️⃣ 시스템 아키텍처 개요

#### 🧩 Clean Architecture 기반 구조

```
app/
 ├── api/          # FastAPI endpoints (입출력 인터페이스)
 ├── core/         # 도메인 로직 (Prompt, Evaluation, Improvement)
 ├── adapters/     # Claude API, DB 등 외부 연동 계층
 ├── schemas/      # DTO (입출력 데이터 구조)
 └── config/       # 설정 및 환경 변수
```

**의존성 방향:**
```
core → adapters → api
(core는 외부 구현에 독립적)
```

---

### 5️⃣ AI 인지 구조 (Cognitive Architecture)

| 구성요소 | 설명 |
|----------|------|
| **MainPrompt (Kernel)** | 모든 AI가 공유하는 사고의 근원. 시스템의 세계관 정의. |
| **RolePrompts (3 Roles)** | Claude 3체의 역할별 사고 규칙. MainPrompt를 기반으로 함. |
| **Execution Context** | 실제 실행 시 Main + Role + User Prompt가 합성되어 Claude에 전달되는 환경. |

#### ⚙️ AI 3체 파이프라인 (Claude 중심)

```
[Claude 1] Generator → [Claude 2] Evaluator → [Claude 3] Improver
```

- 생성 → 평가 → 개선의 순환 체계
- 각 결과는 다음 단계의 입력으로 연결
- 최종 채택 여부는 인간이 결정

---

### 6️⃣ Phase별 개발 계획

#### **Phase 1 – 툴 기반 인프라 구축**

핵심 AI 이전에 **"프롬프트를 관리할 수 있는 시스템"**부터 만든다.

**주요 기능:**
- Prompt CRUD (생성, 조회, 수정, 삭제)
- Versioning (버전 자동 증가, rollback)
- Grouping (kernel / role / user 분류)
- Storage Layer (JSON 기반) ← 확정
- FastAPI Endpoint (UI 없이 테스트 가능)

**우선 순위:**
1. MainPrompt CRUD – 커널 프롬프트 관리
2. RolePrompt CRUD – 역할별 프롬프트 관리
3. MainPrompt 버전 생성 및 관리 ← 수정됨

#### **Phase 2 – Claude AI Pipeline 구현**

- Claude 3개 API 연동 (GEN / EVAL / IMP) ← 추후 결정: Claude만 3번 or Claude/GPT/Grok
- PromptExecutionService 설계
- 로그 및 결과 저장, 에러 핸들링

#### **Phase 3 – 개선 루프 및 확장**

- 평가 → 개선 자동 반복 구조
- 사용자 피드백 반영 (Human-in-the-loop)
- GPT, Grok 등 타 모델 확장 가능성 내장

---

### 7️⃣ 핵심 설계 철학

1. **AI는 교체 가능하지만, 사고 체계는 불변해야 한다.**
2. **Prompt는 코드가 아니라 사고의 원재료다.**
3. **MainPrompt는 신성불가침** — 수정이 아닌 버전 갱신으로만 관리.
4. **Clean Architecture**: core는 framework를 모른다.
5. **Human-in-the-loop**: AI는 도구고, 판단은 인간의 몫이다.

---

### 8️⃣ 프로젝트 시그니처

**Project Name:** NEXUS

**의미:** 연결, 교차, 중심.

> "서로 다른 인공지능과 인간의 사고가 만나는 지점."

---
---

## Part 2: 구현 명세 (Implementation Specification)

### 1️⃣ 저장 방식 확정

**선택:** JSON 파일 기반

**이유:**
- 로컬 환경, 개인 사용
- DB 설치/관리 불필요
- 버전 관리 용이 (Git)
- 빠른 시작 가능

**파일 구조:**
```
data/
├── prompts.json          # 프롬프트 저장
├── versions.json         # 버전 히스토리
└── execution_results.json # AI 실행 결과 (Phase 2)
```

---

### 2️⃣ 데이터 구조 상세 설계

#### **Prompt 데이터 구조**

```json
{
  "id": "uuid-string",
  "type": "main | role",
  "role_type": "generator | evaluator | improver",  // role일 경우만
  "parent_id": "uuid-string",  // role일 경우 연결된 main prompt id
  "version": 1,
  "content": "프롬프트 본문",
  "meta": {
    "title": "프롬프트 제목",
    "description": "설명",
    "tags": ["tag1", "tag2"]
  },
  "is_active": true,
  "created_at": "2025-11-02T12:00:00Z",
  "updated_at": "2025-11-02T12:00:00Z"
}
```

#### **Version 데이터 구조**

```json
{
  "prompt_id": "uuid-string",
  "version": 1,
  "content": "해당 버전의 프롬프트 내용",
  "created_at": "2025-11-02T12:00:00Z",
  "created_by": "user_name or system",
  "change_note": "변경 사유"
}
```

#### **ExecutionResult 데이터 구조 (Phase 2)**

```json
{
  "id": "uuid-string",
  "main_prompt_id": "uuid-string",
  "role_prompt_id": "uuid-string",
  "user_prompt": "사용자 입력",
  "ai_response": "AI 응답",
  "role": "generator | evaluator | improver",
  "status": "pending | approved | rejected",
  "human_feedback": "사용자 피드백",
  "executed_at": "2025-11-02T12:00:00Z"
}
```

---

### 3️⃣ 프롬프트 합성 방식

AI 실행 시 다음과 같이 프롬프트를 합성:

```
{MainPrompt.content}

---

{RolePrompt.content}

---

User Request:
{UserPrompt}
```

**예시:**
```
당신은 모든 AI가 공유하는 핵심 사고 체계를 따릅니다.
[MainPrompt 내용...]

---

당신의 역할은 Generator입니다.
[RolePrompt 내용...]

---

User Request:
고객 응대 프롬프트를 작성해주세요.
```

---

### 4️⃣ Phase 1 구현 클래스 설계

#### **core/ 구조**

```
core/
├── prompt_manager.py      # 프롬프트 CRUD 로직
├── version_manager.py     # 버전 관리 로직
├── storage.py             # JSON 파일 읽기/쓰기
└── exceptions.py          # 커스텀 예외
```

#### **PromptManager 클래스**

**책임:** 프롬프트 생성, 조회, 수정, 삭제

**메서드:**
- `create_prompt()` - 새 프롬프트 생성
- `get_prompt_by_id()` - ID로 프롬프트 조회
- `get_all_prompts()` - 전체 프롬프트 조회
- `get_prompts_by_type()` - 타입별 조회 (main/role)
- `update_prompt()` - 프롬프트 수정 (버전 자동 증가)
- `delete_prompt()` - 프롬프트 삭제 (soft delete)
- `activate_prompt()` - 프롬프트 활성화
- `deactivate_prompt()` - 프롬프트 비활성화

#### **VersionManager 클래스**

**책임:** 프롬프트 버전 관리

**메서드:**
- `create_version()` - 새 버전 생성
- `get_version_history()` - 특정 프롬프트의 버전 히스토리 조회
- `rollback_to_version()` - 특정 버전으로 롤백
- `get_latest_version()` - 최신 버전 번호 조회

#### **JSONStorage 클래스**

**책임:** JSON 파일 읽기/쓰기

**메서드:**
- `read()` - 파일 읽기
- `write()` - 파일 쓰기
- `append()` - 데이터 추가
- `backup()` - 백업 생성

---

### 5️⃣ API 엔드포인트 명세

#### **MainPrompt 관리**

```
POST   /api/v1/prompts/main          # MainPrompt 생성
GET    /api/v1/prompts/main          # 전체 MainPrompt 조회
GET    /api/v1/prompts/main/{id}     # 특정 MainPrompt 조회
PUT    /api/v1/prompts/main/{id}     # MainPrompt 수정 (버전 생성)
DELETE /api/v1/prompts/main/{id}     # MainPrompt 삭제
```

**Request 예시 (POST /api/v1/prompts/main):**
```json
{
  "content": "당신은 NEXUS 시스템의 핵심 사고 체계입니다...",
  "meta": {
    "title": "NEXUS Kernel v1.0",
    "description": "초기 커널 프롬프트",
    "tags": ["kernel", "v1"]
  }
}
```

**Response 예시:**
```json
{
  "id": "uuid-123",
  "type": "main",
  "version": 1,
  "content": "당신은 NEXUS 시스템의 핵심 사고 체계입니다...",
  "meta": { ... },
  "is_active": true,
  "created_at": "2025-11-02T12:00:00Z"
}
```

#### **RolePrompt 관리**

```
POST   /api/v1/prompts/role          # RolePrompt 생성
GET    /api/v1/prompts/role          # 전체 RolePrompt 조회
GET    /api/v1/prompts/role/{id}     # 특정 RolePrompt 조회
PUT    /api/v1/prompts/role/{id}     # RolePrompt 수정
DELETE /api/v1/prompts/role/{id}     # RolePrompt 삭제
```

**Request 예시 (POST /api/v1/prompts/role):**
```json
{
  "role_type": "generator",
  "parent_id": "uuid-123",  // MainPrompt ID
  "content": "당신은 초안을 생성하는 Generator입니다...",
  "meta": {
    "title": "Generator Role v1.0",
    "description": "생성 역할 프롬프트"
  }
}
```

#### **버전 관리**

```
GET    /api/v1/prompts/{id}/versions        # 버전 히스토리 조회
POST   /api/v1/prompts/{id}/rollback/{ver}  # 특정 버전으로 롤백
```

#### **기타**

```
GET    /health                               # 헬스체크
GET    /api/v1/prompts/active                # 활성화된 프롬프트만 조회
```

---

### 6️⃣ Phase 1 구현 순서

**Week 1: 기본 구조 및 Storage**
1. JSON 파일 구조 설정
2. JSONStorage 클래스 구현
3. 기본 데이터 모델 (schemas) 정의

**Week 2: PromptManager 구현**
4. MainPrompt CRUD
5. RolePrompt CRUD
6. 기본 유효성 검증

**Week 3: VersionManager 구현**
7. 버전 생성 로직
8. 버전 히스토리 조회
9. 롤백 기능

**Week 4: API 및 테스트**
10. FastAPI 엔드포인트 구현
11. 통합 테스트
12. 에러 핸들링 보완

---

### 7️⃣ Human-in-the-loop 설계 (Phase 2+)

#### **결과 조회 및 피드백**

```
GET    /api/v1/results                    # 실행 결과 목록
GET    /api/v1/results/{id}               # 특정 결과 조회
POST   /api/v1/results/{id}/approve       # 결과 승인
POST   /api/v1/results/{id}/reject        # 결과 거부
POST   /api/v1/results/{id}/feedback      # 피드백 작성
```

#### **피드백 구조**

```json
{
  "result_id": "uuid-string",
  "status": "approved | rejected",
  "feedback": "승인/거부 사유",
  "improvement_notes": "개선 제안 사항"
}
```

---

### 8️⃣ 확장 고려사항

#### **Phase 2 확장**
- Claude API 연동
- AI 실행 로그 저장
- 에러 핸들링 및 재시도 로직

#### **Phase 3 확장**
- 자동 개선 루프
- 다중 AI 모델 지원 (GPT, Grok)
- 웹 UI 구축 (선택)

#### **보안 고려사항**
- API 키 환경변수 관리
- 프롬프트 암호화 (민감 정보 포함 시)
- 접근 권한 관리 (추후)

---

## Part 3: 부록

### 📋 용어 정리

| 용어 | 설명 |
|------|------|
| **MainPrompt** | 모든 AI가 공유하는 커널 프롬프트 |
| **RolePrompt** | 역할별(Generator/Evaluator/Improver) 프롬프트 |
| **UserPrompt** | 사용자가 입력하는 실행 시 프롬프트 |
| **Execution Context** | Main + Role + User를 합성한 최종 프롬프트 |
| **Human-in-the-loop** | AI 결과에 인간이 개입하는 구조 |

---

### 🎯 다음 단계

1. **이 문서 확정**
2. **독스트링 기반 코드 작성 시작**
   - 메서드 하나씩
   - AI가 구현
   - 조립 및 테스트
3. **Phase 1 완료 후 Phase 2 진입**

---

**Document Version:** v1.0  
**Last Updated:** 2025-11-02  
**Author:** hyunAmu  
**Status:** 진행 중 (Phase 1)

---

> "모든 위대한 시스템은 명확한 설계에서 시작된다."
