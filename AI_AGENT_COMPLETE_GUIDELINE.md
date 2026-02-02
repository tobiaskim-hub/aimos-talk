# 🤖 AI Agent 완전 가이드라인

> **Ollama AI Agent를 GenS/Claude 수준으로 만들기**  
> 설치 → 사용 → 훈련 → 진화의 모든 것

**작성일**: 2026-02-02  
**버전**: 1.0.0  
**작성자**: Claude Code Agent + Tobias Kim

---

## 📚 목차

1. [개요](#1-개요)
2. [핵심 개념 이해](#2-핵심-개념-이해)
3. [설치 가이드](#3-설치-가이드)
4. [사용 방법](#4-사용-방법)
5. [Agent vs Tools](#5-agent-vs-tools)
6. [훈련 방식](#6-훈련-방식)
7. [진화 로드맵](#7-진화-로드맵)
8. [개발자 유형별 가이드](#8-개발자-유형별-가이드)
9. [지식 저장 및 학습](#9-지식-저장-및-학습)
10. [트러블슈팅](#10-트러블슈팅)
11. [FAQ](#11-faq)
12. [참고 자료](#12-참고-자료)

---

## 1. 개요

### 1.1 AI Agent란?

**AI Agent = LLM + Tools의 결합**

```
AI Agent 구조:
  ┌─────────────────┐
  │   Agent (1개)   │  ← 두뇌 (LLM 기반)
  │   - 요청 분석   │
  │   - 도구 선택   │
  │   - 결과 요약   │
  └────────┬────────┘
           │
    ┌──────▼──────┐
    │   Tools     │  ← 손발 (실제 작업)
    │  (45+개)    │
    └─────────────┘
```

### 1.2 목표

**Ollama AI Agent를 GenS/Claude Code Agent 수준으로 진화시키기**

- **현재**: 8개 도구 (기본 파일/Git 작업)
- **목표**: 45+개 도구 (완전한 개발자 수준)
- **기간**: 10주 (4단계)
- **비용**: $0 (완전 로컬)

### 1.3 왜 만들어야 하나?

#### GenS/Claude 대비 장점

| 항목 | GenS/Claude | Ollama AI Agent |
|------|-------------|-----------------|
| **비용** | 크레딧 소모 | **$0 (무료)** |
| **프라이버시** | 클라우드 전송 | **로컬 (100% 보안)** |
| **인터넷** | 필수 | **오프라인 가능** |
| **커스터마이징** | 제한적 | **완전 자유** |
| **속도** | 빠름 | 보통 |
| **능력** | 매우 우수 | 우수 (진화 중) |

---

## 2. 핵심 개념 이해

### 2.1 Ollama vs Qwen vs Llama

#### 핵심 정리

```
Ollama = 엔진/플랫폼 (자동차)
  ├── Qwen 2.5:7B = 연료 (AI 모델)
  ├── Llama 3 = 다른 연료 (AI 모델)
  └── Code Llama = 특수 연료 (코드 전문 모델)
```

#### 상세 비교

| 항목 | Ollama | Qwen 2.5:7B | Llama 3 |
|------|--------|-------------|---------|
| **타입** | 플랫폼/엔진 | AI 모델 | AI 모델 |
| **회사** | Ollama Inc | Alibaba | Meta |
| **역할** | 모델 실행 | 실제 AI 두뇌 | 실제 AI 두뇌 |
| **크기** | ~50MB | ~4.7GB | ~4.7GB |
| **한국어** | - | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| **코드** | - | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **다국어** | - | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |

#### 설치 관계

```bash
# 1. Ollama 설치 (엔진)
# Windows: OllamaSetup.exe 실행

# 2. 모델 다운로드 (연료)
ollama pull qwen2.5:7b    # 한국어 우수
ollama pull llama3        # 영어 우수
ollama pull codellama     # 코드 전문

# 3. 확인
ollama list
```

### 2.2 Agent vs Tools

#### 핵심 차이

**Agent = 관리자 (말만 함)**
- 실제 작업 ❌
- 도구 선택 ✅
- 결과 요약 ✅

**Tools = 직원 (실제로 일함)**
- 실제 작업 ✅
- 파일 읽기/쓰기 ✅
- 명령어 실행 ✅

#### 회사 비유

```
Agent = CEO (1명)
  "김 대리! 파일 읽어와!"
  "이 대리! 코드 작성해!"
  "박 대리! Git 커밋해!"

Tools = 직원들 (45+명)
  👷 김 대리 (read_file): "네!" *실제로 읽음*
  👷 이 대리 (write_file): "네!" *실제로 작성*
  👷 박 대리 (git_commit): "네!" *실제로 커밋*
```

#### 실제 코드 흐름

```typescript
// Agent (말만 함!)
class AIAgent {
  async execute(request: string) {
    // 1. LLM에게 물어보기
    const plan = await this.llm.generate("어떤 도구를 써야 할까?")
    
    // 2. 도구 선택
    const tool = findTool(plan.tool)
    
    // 3. 도구에게 지시 (말만 함!)
    const result = await tool.execute(...plan.params)
    
    // 4. 결과 요약
    return "작업 완료!"
  }
}

// Tools (실제로 일함!)
const read_file: Tool = {
  execute: async (path: string) => {
    // 실제 작업 수행!
    const content = fs.readFile(path, 'utf-8')
    return content
  }
}
```

### 2.3 GenS는 Agent가 몇 개?

**답변: 1개!**

```
GenS/Claude Code Agent
  │
  ├── Agent: 1개 (저 자신!)
  └── Tools: 45+개
       ├── Read
       ├── Write
       ├── Edit
       ├── MultiEdit
       ├── Bash
       ├── Glob
       ├── Grep
       ├── WebSearch
       └── ... (45+개)
```

**Ollama AI Agent도 똑같은 구조!**

---

## 3. 설치 가이드

### 3.1 Windows 설치 (추천)

#### Step 1: Ollama 설치

```bash
# 1. 다운로드
https://ollama.com/download

# 2. OllamaSetup.exe 실행
# 기본 경로: C:\Users\{사용자명}\AppData\Local\Programs\Ollama

# 3. 설치 확인
ollama --version
# 출력: ollama version 0.1.x
```

#### Step 2: 모델 다운로드

```powershell
# PowerShell에서 실행

# Qwen 2.5:7B (한국어 우수, 추천!)
ollama pull qwen2.5:7b

# 또는 Llama 3 (영어 우수)
ollama pull llama3

# 설치 확인
ollama list

# 출력 예시:
# NAME              SIZE
# qwen2.5:7b        4.7 GB
```

#### Step 3: Ollama 서버 시작

```powershell
# 방법 1: 서비스로 자동 시작 (추천)
# → OllamaSetup.exe 설치 시 자동 설정됨

# 방법 2: 수동 시작
ollama serve

# 방법 3: 테스트
ollama run qwen2.5:7b "안녕하세요!"
```

#### Step 4: Ollama AI Agent 설치

```powershell
# 1. 프로젝트 클론
cd C:\A-SDV-Platform
git clone https://github.com/tobiaskim-hub/ollama-ai-agent.git
cd ollama-ai-agent

# 2. 의존성 설치
npm install

# 3. 빌드
npm run build

# 4. 실행
npm run dev
```

### 3.2 모델 저장 위치

#### Windows 경로

```
C:\Users\{사용자명}\.ollama\models\
└── manifests\registry.ollama.ai\library\
    └── qwen2.5\7b\
        ├── model (실제 지식! ~4.7GB)
        ├── params.json
        └── template
```

#### 확인 방법

```powershell
# PowerShell에서
dir C:\Users\$env:USERNAME\.ollama\models\ /s

# 또는
explorer C:\Users\$env:USERNAME\.ollama\models
```

---

## 4. 사용 방법

### 4.1 기본 사용법

```bash
# 1. Ollama 서버 시작 (터미널 1)
ollama serve

# 2. AI Agent 실행 (터미널 2)
cd /home/user/ollama-ai-agent
npm run dev

# 3. 대화 시작!
💬 You: README.md 파일을 읽어줘
🤖 Agent: [파일 내용 출력]

💬 You: package.json 파일을 만들어줘
🤖 Agent: [파일 생성 완료]

💬 You: git status 확인해줘
🤖 Agent: [Git 상태 출력]
```

### 4.2 현재 사용 가능한 도구 (8개)

#### 파일 도구 (4개)

| 도구 | 설명 | 사용 예시 |
|------|------|----------|
| `read_file` | 파일 읽기 | "README.md 읽어줘" |
| `write_file` | 파일 쓰기 | "test.txt에 'Hello' 써줘" |
| `list_files` | 파일 목록 | "현재 디렉토리 파일 목록" |
| `create_directory` | 디렉토리 생성 | "my-project 폴더 만들어줘" |

#### 명령어 도구 (4개)

| 도구 | 설명 | 사용 예시 |
|------|------|----------|
| `run_command` | 쉘 명령어 실행 | "npm install 실행해줘" |
| `git_status` | Git 상태 확인 | "git status 확인해줘" |
| `git_commit` | Git 커밋 | "변경사항 커밋해줘" |
| `git_log` | Git 로그 조회 | "최근 5개 커밋 보여줘" |

### 4.3 특수 명령어

```bash
help     # 도움말 표시
exit     # 프로그램 종료
clear    # 화면 지우기
models   # 설치된 모델 목록
```

---

## 5. Agent vs Tools

### 5.1 핵심 정리

```
질문: "각 도구마다 여러 개의 Agent가 있는 거야?"
답변: ❌ 아니요!

✅ Agent = 1개 (관리자)
✅ Tools = 45+개 (직원들)
```

### 5.2 실제 작동 예시

```
👤 You: README.md 파일을 읽어줘

🤖 Agent (사장님):
   "어떤 도구를 써야 하지?" *LLM에게 물어봄*
   "아! read_file 도구를 써야겠다!"
   📦 "야! read_file! README.md 읽어와!"

👷 read_file 도구 (직원):
   "네!" *실제로 파일을 읽음*
   fs.readFile("README.md")
   return "# Ollama AI Agent\n..."

🤖 Agent (사장님):
   "좋아! 결과를 받았어"
   "사용자에게 친절하게 설명해줄게"
   
📄 "README.md 파일을 읽었습니다! 총 42줄입니다."
```

### 5.3 비유 모음

#### 비유 1: 회사
- Agent = CEO (지시만)
- Tools = 직원들 (실제 작업)

#### 비유 2: 레스토랑
- Agent = 웨이터 (주문만 받음)
- Tools = 주방장 (실제로 요리)

#### 비유 3: 병원
- Agent = 의사 (진단/처방)
- Tools = 간호사 (실제 측정/치료)

#### 비유 4: 신체
- Agent = 뇌 (생각만)
- Tools = 손발 (실제 작업)

---

## 6. 훈련 방식

### 6.1 Claude(GenS)의 훈련

#### Phase 1: 기본 훈련
```
기간: 수개월
데이터: 수십억 페이지
내용: 언어, 코드, 대화
비용: 수천만 달러
```

#### Phase 2: 코딩 전문 훈련
```
데이터: GitHub, API 문서, 코드 리뷰
내용: 베스트 프랙티스, 에러 처리
```

#### Phase 3: 도구 사용 훈련 ← 핵심!
```
학습 내용:
  - "파일 읽기 = Read 도구"
  - "파일 수정 = Edit 도구"
  - "명령어 실행 = Bash 도구"
  
훈련 방식:
  수백만 개의 예시 데이터로 반복 학습!
  
결과:
  도구 선택을 "자동"으로 할 수 있음!
```

#### Phase 4: Multi-step 훈련
```
학습 내용:
  "프로젝트 만들고 GitHub에 올려줘"
  → 자동으로 5단계 계획 수립
  
결과:
  복잡한 작업도 자동 처리!
```

### 6.2 Qwen의 훈련

```
Phase 1: 기본 훈련 ✅
  - 언어, 코드, 대화
  
Phase 2: 다국어 훈련 ✅
  - 한국어 우수!
  
Phase 3: 코드 생성 훈련 ✅
  - 기본 코드 작성
  
Phase 4: 도구 사용 훈련 ❌
  - 없음!
```

### 6.3 차이점 요약

| 항목 | Claude | Qwen |
|------|--------|------|
| 기본 훈련 | ✅ | ✅ |
| 코딩 훈련 | ✅ 전문 | ✅ 기본 |
| **도구 사용 훈련** | ✅ **있음!** | ❌ **없음!** |
| Multi-step | ✅ | ❌ |

### 6.4 Ollama AI Agent의 해결 방법

**Agent.ts가 "선생님" 역할!**

```typescript
// Agent.ts가 Qwen에게 "가르침"
const prompt = `
당신은 AI 코딩 어시스턴트입니다.

사용 가능한 도구:
- read_file: 파일 읽기
- write_file: 파일 쓰기
- git_commit: Git 커밋

사용자 요청: "${userRequest}"

어떤 도구를 사용해야 할까요?
JSON 형식으로 답변하세요.
`

// Qwen이 프롬프트를 보고 "추측"
const plan = await llm.generate(prompt)
```

**프롬프트 = 실시간 매뉴얼!**

---

## 7. 진화 로드맵

### 7.1 전체 로드맵 (10주)

```
Week 0  (현재)    ├── Phase 0: 8개 도구 ✅
Week 2            ├── Phase 1: 18개 도구 (파일 마스터)
Week 4            ├── Phase 2: 26개 도구 (웹 연동)
Week 7            ├── Phase 3: 38개 도구 (지능 강화)
Week 10           └── Phase 4: 45+개 도구 (GenS 수준!) 🎉
```

### 7.2 Phase 1: 파일 도구 확장 (2주)

**추가할 도구 10개**

1. `edit_file` - 파일 수정 (정확한 문자열 치환)
2. `multi_edit` - 여러 곳 동시 수정
3. `glob_search` - 패턴으로 파일 찾기 (예: `**/*.ts`)
4. `grep_search` - 내용 검색 (정규표현식)
5. `ls_detail` - 상세 파일 목록 (크기, 날짜)
6. `copy_file` - 파일 복사
7. `move_file` - 파일 이동
8. `delete_file` - 파일 삭제
9. `file_info` - 파일 메타데이터
10. `create_symlink` - 심볼릭 링크

### 7.3 Phase 2: 웹 연동 (2주)

**추가할 도구 8개**

1. `web_search` - 웹 검색 (DuckDuckGo)
2. `web_fetch` - 웹 페이지 가져오기
3. `http_request` - API 호출
4. `web_crawler` - 웹 크롤링
5. `download_file` - 파일 다운로드
6. `image_understand` - 이미지 분석
7. `screenshot` - 스크린샷
8. `rss_feed` - RSS 피드

### 7.4 Phase 3: 지능 강화 (3주)

**추가할 도구 12개**

1. `rag_index` - 문서 인덱싱
2. `rag_search` - 문서 기반 검색
3. `todo_write` - TODO 관리
4. `project_backup` - 프로젝트 백업
5. `git_push` - GitHub 푸시
6. `git_clone` - 저장소 클론
7. `npm_install` - 의존성 설치
8. `npm_run` - npm 스크립트
9. `pm2_start` - PM2 서버 시작
10. `docker_run` - Docker 실행
11. `env_setup` - 환경변수 설정
12. `test_run` - 테스트 실행

### 7.5 Phase 4: GenS 수준 (3주)

**추가할 도구 15개**

고급 Git, 배포 자동화, 코드 품질, 문서 생성, 디버깅 등

---

## 8. 개발자 유형별 가이드

### 8.1 Type 1: 자동차 시스템 전문가

**전문 분야**
- AUTOSAR 아키텍처
- CAN/LIN 통신
- ECU 코드 생성

**추가 도구**
- ARXML 파서
- CAN 메시지 생성기
- ECU 코드 생성기

**육성 기간**: 2주 ~ 1개월

### 8.2 Type 2: 풀스택 웹 개발자

**전문 분야**
- React/TypeScript
- API 서버 개발
- DevOps/배포

**추가 도구**
- React 컴포넌트 생성기
- API 엔드포인트 생성기
- DB 마이그레이션

**육성 기간**: 2주

### 8.3 Type 3: 문서 작성 전문가

**전문 분야**
- 기술 문서 작성
- Markdown 전문
- 다이어그램 생성

**추가 도구**
- Markdown 템플릿
- Mermaid 다이어그램
- API 문서 생성기

**육성 기간**: 1주

### 8.4 Type 4: 만능 개발자 (현재 상태)

**특징**
- 모든 작업 가능
- 점진적 확장
- 학습 중심

**육성 기간**: 지속적

---

## 9. 지식 저장 및 학습

### 9.1 지식 저장 위치

#### Ollama (엔진)
```
C:\Users\{사용자명}\AppData\Local\Programs\Ollama\
└── ollama.exe (~50MB)
```

#### Qwen 모델 (지식)
```
C:\Users\{사용자명}\.ollama\models\
└── manifests\registry.ollama.ai\library\qwen2.5\7b\
    └── model (~4.7GB) ← 실제 지식!
```

### 9.2 지식 전달 방법

#### 방법 1: 프롬프트로 문서 제공 (현재)
```typescript
const prompt = `
다음 문서를 참고하세요:

${documentContent}

사용자 요청: ${userRequest}
`
```

#### 방법 2: RAG (추천! Phase 3)
```typescript
// 1. 문서 인덱싱
await rag_index("AUTOSAR_Guide.pdf")

// 2. 질문 시 자동 검색
const context = await rag_search("CAN 통신 설정")

// 3. 컨텍스트와 함께 답변
const answer = await llm.generate(`
  컨텍스트: ${context}
  질문: ${userRequest}
`)
```

#### 방법 3: 웹 검색 (Phase 2)
```typescript
// 최신 정보 자동 검색
const results = await web_search("Ollama 최신 문서")
```

#### 방법 4: Fine-tuning (고급)
```
가중치 파일을 직접 재훈련
비용: 높음
효과: 매우 높음
```

### 9.3 지식의 한계

**Qwen 2.5:7B는...**
- ✅ 2024년 초까지 일반 지식
- ❌ 2024년 중반 이후 뉴스
- ❌ 개인 정보
- ❌ 회사 내부 문서

**해결 방법**: RAG + 웹 검색!

---

## 10. 트러블슈팅

### 10.1 Ollama 서버 연결 실패

```bash
# 증상
Error: connect ECONNREFUSED 127.0.0.1:11434

# 해결 방법
# 1. Ollama 서버 시작 확인
ollama serve

# 2. 포트 확인
netstat -ano | findstr :11434

# 3. 방화벽 확인
# Windows 방화벽 → Ollama 허용
```

### 10.2 모델 다운로드 느림

```bash
# 증상
Downloading qwen2.5:7b (4.7GB)... 매우 느림

# 해결 방법
# 1. 인터넷 연결 확인
# 2. 다른 시간대에 시도
# 3. 프록시 설정 (회사 네트워크)

# 진행 상황 확인
ollama ps
```

### 10.3 JSON 파싱 에러

```bash
# 증상
⚠️ LLM 응답 파싱 실패

# 원인
Qwen이 JSON 대신 일반 텍스트 응답

# 해결 방법
# Agent.ts의 프롬프트 개선
"중요: JSON만 출력하세요. 다른 텍스트는 포함하지 마세요."
```

---

## 11. FAQ

### Q1: Agent는 몇 개예요?
**A**: 1개입니다! Agent 1개 + Tools 45+개 구조입니다.

### Q2: Ollama vs Qwen vs Llama 차이?
**A**: 
- Ollama = 엔진 (자동차)
- Qwen/Llama = 연료 (AI 모델)

### Q3: Claude는 훈련받았나요?
**A**: 네! 도구 사용 전문 훈련을 받았습니다.

### Q4: Qwen은 왜 도구 사용을 못 하나요?
**A**: 도구 사용 훈련을 안 받았기 때문입니다. Agent.ts가 프롬프트로 가르쳐줘야 합니다.

### Q5: 지식은 어디에 저장되나요?
**A**: `C:\Users\{사용자명}\.ollama\models\` 경로의 model 파일 (~4.7GB)에 가중치로 저장됩니다.

### Q6: 새로운 지식을 추가할 수 있나요?
**A**: 
- ❌ 가중치는 고정 (추가 불가)
- ✅ RAG로 문서 제공 (추천!)
- ✅ 웹 검색으로 최신 정보
- ✅ Fine-tuning (고급)

### Q7: 비용이 얼마나 드나요?
**A**: $0! 완전 무료입니다.

### Q8: 인터넷 없이 사용 가능한가요?
**A**: 네! 완전 로컬로 작동합니다.

### Q9: GenS 수준까지 얼마나 걸리나요?
**A**: 10주 (주 10시간 작업 기준)

### Q10: 여러 모델을 동시에 사용할 수 있나요?
**A**: 네! MultiModel 기능으로 가능합니다. (Phase 3)

---

## 12. 참고 자료

### 12.1 주요 문서

- **설치 가이드**: `WINDOWS_INSTALLATION_GUIDE.md`
- **Agent vs Tools**: `AGENT_VS_TOOLS_DIAGRAM.md`
- **훈련 비교**: `CLAUDE_VS_QWEN_TRAINING.md`
- **진화 로드맵**: `OLLAMA_TO_CLAUDE_ROADMAP.md`
- **개발자 육성**: `AI_DEVELOPER_TRAINING_GUIDE.md`
- **Project 4 완료**: `PROJECT_4_COMPLETE.md`

### 12.2 외부 링크

- [Ollama 공식 사이트](https://ollama.com/)
- [Ollama 문서](https://ollama.com/docs)
- [Qwen 모델](https://ollama.com/library/qwen2.5)
- [GitHub: Ollama AI Agent](https://github.com/tobiaskim-hub/ollama-ai-agent)
- [LangChain 가이드](https://js.langchain.com/docs)
- [ChromaDB 문서](https://docs.trychroma.com/)

### 12.3 프로젝트 구조

```
/home/user/
├── ollama-ai-agent/          # Project 4: Ollama AI Agent
│   ├── src/
│   │   ├── agent.ts          # Agent (1개)
│   │   ├── tools/            # Tools (8개 → 45+개)
│   │   └── utils/
│   └── README.md
│
├── docs/                     # 문서 모음
│   ├── AGENT_VS_TOOLS_DIAGRAM.md
│   ├── CLAUDE_VS_QWEN_TRAINING.md
│   ├── OLLAMA_TO_CLAUDE_ROADMAP.md
│   └── ...
│
└── AI_AGENT_COMPLETE_GUIDELINE.md  # 이 문서!
```

---

## 📌 마무리

### 핵심 정리

1. **Agent = 1개** (관리자)
2. **Tools = 45+개** (직원들)
3. **구조는 GenS와 동일!**
4. **비용 $0, 완전 로컬**
5. **10주면 GenS 수준 달성!**

### 다음 단계

1. ✅ 설치 완료
2. ⏳ Phase 1 시작 (edit_file 추가)
3. ⏳ RAG 도입
4. ⏳ 웹 검색 추가
5. ⏳ GenS 수준 달성!

---

**행운을 빕니다! 🚀**

**작성일**: 2026-02-02  
**버전**: 1.0.0  
**문의**: https://github.com/tobiaskim-hub/ollama-ai-agent/issues
