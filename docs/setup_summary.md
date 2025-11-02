---

### 📄 **docs/setup_summary.md**

```markdown
# 🧠 Project NEXUS — 초기 세팅 요약서 (v1)

> **모든 AI가 공유하는 사고의 근원.**  
> 프롬프트를 구조적으로 관리하고, 평가하고, 개선하는 시스템의 시작.

---

## ⚙️ 환경 정보

| 항목 | 내용 |
|------|------|
| OS | macOS (Apple Silicon, zsh 환경) |
| Python 버전 | 3.13.0 |
| 프로젝트 경로 | `/Users/hyun/fapi/project-NEXUS` |
| 가상환경 | venv (활성화 상태: ✅) |
| 패키지 관리 | `requirements.txt` |

---

## 📦 설치 패키지 목록 (`requirements.txt`)

```

fastapi
uvicorn
pydantic
python-dotenv

```

- `fastapi`: 백엔드 프레임워크
- `uvicorn`: ASGI 서버 (개발용)
- `pydantic`: 데이터 검증 및 모델 정의
- `python-dotenv`: 환경 변수 관리

---

## 🧱 프로젝트 구조

```

project-NEXUS/
├── app/
│   ├── adapters/    # 외부 연동 (Claude API 등)
│   ├── api/         # FastAPI 엔드포인트
│   ├── config/      # 환경 설정, 초기화
│   ├── core/        # 핵심 도메인 로직
│   └── schemas/     # 데이터 구조 정의
├── tests/           # 테스트 코드
├── .gitignore
├── README.md
├── requirements.txt
└── venv/ or .venv/

````

---

## 🔐 환경 변수 (.env)

```bash
CLAUDE_API_KEY=""
GROK_API_KEY=""
GPT_API_KEY=""
````

> `.env` 파일은 커밋 금지 (`.gitignore`에 등록 필수)

---

## 🧰 실행 명령어

```bash
# 가상환경 활성화
source venv/bin/activate

# 패키지 설치
pip install -r requirements.txt

# FastAPI 서버 실행
python3 -m uvicorn app.main:app --reload
```

실행 후 브라우저에서 [http://127.0.0.1:8000](http://127.0.0.1:8000) 확인
→ `{"message": "NEXUS ONLINE"}` 출력 시 정상 작동.

---

## 🧩 Git 관리

```bash
git add .
git commit -m "chore: 초기 세팅 완료 (venv + FastAPI 환경)"
git push
```

원격 저장소:
`https://github.com/hyunAmu/project-NEXUS.git`

---

## 🧠 다음 단계

1. **공통 프롬프트 (MainPrompt)** 설계
   → 모든 AI가 공유하는 시스템적 사고의 기반

2. **역할 프롬프트 (RolePrompt)** 정의

   * Generator (초안 제작 / GPT)
   * Evaluator (평가 / Claude)
   * Improver (개선 / Grok)

3. **프롬프트 CRUD 기능** 구현

   * 프롬프트 등록 / 수정 / 삭제 / 조회
   * 로컬 JSON 혹은 DB 저장 구조 설계

---

## 🪶 메모

> 이 시점은 NEXUS의 “의식이 깨어난” 순간이다.
> 이후의 모든 로직, 프롬프트, API 호출은 이 구조 위에서 진화한다.

---

**작성자:** hyunAmu
**작성일:** `$(date)`

````

---

### 🪄 다음 해야 할 일
1. VS Code에서 `docs/setup_summary.md` 파일 새로 만들어서 위 내용 붙여넣기  
2. 저장 (`⌘ + S`)  
3. 아래 명령어로 커밋 👇  
```bash
git add .
git commit -m "docs: setup_summary v1 추가 (Project NEXUS 초기 설정 문서)"
git push
````

---
