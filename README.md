# 🤖 AI Agent 프로젝트 허브

> **완전한 AI 개발자 생태계**  
> 자동차 소프트웨어부터 AI Agent까지

**작성일**: 2026-02-02  
**버전**: 2.0.0  
**작성자**: Claude Code Agent + Tobias Kim

---

## 🎯 빠른 시작

### 📖 **[AI Agent 완전 가이드 대시보드 열기](./DASHBOARD.html)** ← 클릭!

모든 문서를 한눈에 보고 싶다면 대시보드를 열어보세요!

---

## 📊 프로젝트 현황

### 완료된 프로젝트 (4개)

| # | 프로젝트 | 상태 | 포트 | 데모 URL | GitHub |
|---|---------|------|------|----------|--------|
| 1 | **Automotive System Modeler** | 🟢 운영중 | 3000 | [데모](https://3000-ifl0a91rhl2v80gsfr4rn-8f57ffe2.sandbox.novita.ai) | [GitHub](https://github.com/tobiaskim-hub/automotive-system-modeler) |
| 2 | **Architecture Agent + LLM** | 🟢 운영중 | 3001 | [데모](https://3001-ifl0a91rhl2v80gsfr4rn-8f57ffe2.sandbox.novita.ai) | [GitHub](https://github.com/tobiaskim-hub/architecture-agent-llm) |
| 3 | **SDV Platform Docs** | ✅ 완료 | - | [문서](https://github.com/tobiaskim-hub/automotive-sdv-platform/blob/main/INDEX.md) | [GitHub](https://github.com/tobiaskim-hub/automotive-sdv-platform) |
| 4 | **Ollama AI Agent** | ✅ 완료 | - | 🔄 예정 | [GitHub](https://github.com/tobiaskim-hub/ollama-ai-agent) |

---

## 🎯 Project 4: Ollama AI Agent (최신!)

### 개요
**로컬 LLM(Ollama + Qwen 2.5:7B)을 GenS/Claude 수준으로 만들기**

### 핵심 특징
- ✅ **완전 로컬** - 비용 $0
- ✅ **프라이버시 보장** - 데이터 외부 전송 없음
- ✅ **오프라인 가능** - 인터넷 불필요
- ✅ **Agent 1개 + Tools 45+개** 구조
- ✅ **10주 진화 로드맵**

### 빠른 링크
- 📘 [완전 가이드라인](./AI_AGENT_COMPLETE_GUIDELINE.md) ← **필독!**
- 🗺️ [진화 로드맵](./OLLAMA_TO_CLAUDE_ROADMAP.md) (4단계, 10주)
- 🧠 [Agent vs Tools 설명](./AGENT_VS_TOOLS_DIAGRAM.md)
- 🎓 [훈련 방식 비교](./CLAUDE_VS_QWEN_TRAINING.md)
- 🪟 [Windows 설치 가이드](./WINDOWS_INSTALLATION_GUIDE.md)

### 현재 상태
- **Agent**: 1개 ✅
- **Tools**: 8개 (목표: 45+개)
- **능력**: 기본 파일/Git 작업
- **진행**: Phase 0 완료, Phase 1 준비 중

---

## 📚 문서 구조

```
/home/user/
├── DASHBOARD.html                          # 📊 대시보드 (모든 문서 링크)
├── AI_AGENT_COMPLETE_GUIDELINE.md          # 📘 완전 가이드라인 (필독!)
│
├── docs/                                    # 📁 핵심 문서 모음
│   ├── AGENT_VS_TOOLS_DIAGRAM.md           # Agent vs Tools 설명
│   ├── AGENT_TOOLS_SIMPLE_EXPLANATION.md   # 초간단 설명
│   ├── CLAUDE_VS_QWEN_TRAINING.md          # 훈련 방식 비교
│   ├── OLLAMA_TO_CLAUDE_ROADMAP.md         # 진화 로드맵
│   ├── AI_DEVELOPER_TRAINING_GUIDE.md      # 개발자 육성 가이드
│   ├── OLLAMA_AGENT_GUIDE.md               # Ollama Agent 사용법
│   ├── WINDOWS_INSTALLATION_GUIDE.md       # Windows 설치
│   ├── INSTALLATION_LOG.md                 # 설치 로그
│   └── OLLAMA_TROUBLESHOOTING.md           # 트러블슈팅
│
├── confluence-docs/                         # Confluence 문서
│   ├── INDEX.md                            # Confluence 목차
│   ├── PROJECT-1-Automotive-Modeler.md
│   ├── PROJECT-2-Architecture-Agent.md
│   ├── PROJECT-3-SDV-Platform-Strategy.md
│   └── PROJECT-4-Ollama-AI-Agent.md
│
├── ollama-ai-agent/                         # Project 4 소스 코드
│   ├── src/
│   │   ├── agent.ts                        # Agent (1개)
│   │   ├── tools/                          # Tools (8개 → 45+개)
│   │   └── utils/
│   └── README.md
│
└── (기타 프로젝트 문서들...)
```

---

## 🚀 빠른 시작 (Project 4)

### Step 1: Ollama 설치

```powershell
# 1. https://ollama.com/download 에서 다운로드
# 2. OllamaSetup.exe 실행

# 3. 모델 다운로드
ollama pull qwen2.5:7b

# 4. 확인
ollama list
```

### Step 2: AI Agent 실행

```powershell
# Windows PC에서
cd C:\A-SDV-Platform\ollama-ai-agent
npm install
npm run dev
```

### Step 3: 사용 예시

```bash
💬 You: README.md 파일을 읽어줘
🤖 Agent: [파일 내용 출력]

💬 You: test.txt에 "Hello World" 써줘
🤖 Agent: ✅ 파일 작성 완료!

💬 You: git status 확인해줘
🤖 Agent: [Git 상태 출력]
```

---

## 🎓 핵심 개념

### Agent vs Tools

```
Agent (1개) = 관리자 (말만 함)
  - 도구 선택
  - 결과 요약
  - LLM 사용

Tools (45+개) = 직원들 (실제로 일함)
  - 파일 읽기/쓰기
  - 명령어 실행
  - Git 관리
  - 웹 검색
  - ... (45+개)
```

### Ollama vs Qwen

```
Ollama = 엔진/플랫폼 (자동차)
  - 모델 실행
  - HTTP API 제공
  - ~50MB

Qwen 2.5:7B = AI 모델 (연료)
  - 실제 AI 두뇌
  - 가중치 저장
  - ~4.7GB
```

---

## 📈 진화 로드맵

### Phase 0: 기초 (✅ 완료)
- Agent: 1개
- Tools: 8개 (파일 4개 + 명령어 4개)
- 능력: 기본 작업

### Phase 1: 파일 마스터 (⏳ 2주)
- Tools: +10개 (Edit, Glob, Grep 등)
- 능력: 복잡한 코드 리팩토링

### Phase 2: 웹 연동 (⏳ 2주)
- Tools: +8개 (WebSearch, API 호출 등)
- 능력: 최신 정보 검색

### Phase 3: 지능 강화 (⏳ 3주)
- Tools: +12개 (RAG, MultiModel 등)
- 능력: 문서 기반 전문가 답변

### Phase 4: GenS 수준 (⏳ 3주)
- Tools: +15개 (배포 자동화 등)
- 능력: 완전한 개발자 수준! 🎉

**총 기간**: 10주  
**총 비용**: $0

---

## 🆚 GenS/Claude vs Ollama AI Agent

| 항목 | GenS/Claude | Ollama AI Agent |
|------|-------------|-----------------|
| **비용** | 크레딧 소모 | **$0 (무료)** |
| **프라이버시** | 클라우드 | **로컬 (100%)** |
| **인터넷** | 필수 | **오프라인 가능** |
| **Agent 개수** | 1개 | 1개 (동일!) |
| **Tools 개수** | 45+개 | 8개 → 45+개 (진화 중) |
| **능력** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ (진화 중) |

---

## 📊 전체 통계

### 프로젝트
- 총 프로젝트: **4개**
- 완료: **4개**
- 운영 중: **2개** (포트 3000, 3001)

### 코드
- 총 파일: **41+개**
- 총 코드 줄: **~6,000줄**
- 총 저장소: **4개**

### 문서
- 총 문서: **50+개**
- Confluence: **5개**
- 가이드: **10+개**
- 기술 문서: **35+개**

### 비용
- LLM API: **$0** (로컬 Ollama)
- 크레딧 절감: **100%**

---

## 🔗 빠른 링크

### 대시보드
- 📊 **[DASHBOARD.html](./DASHBOARD.html)** ← 모든 문서 한눈에!

### 핵심 가이드
- 📘 [AI Agent 완전 가이드라인](./AI_AGENT_COMPLETE_GUIDELINE.md)
- 🗺️ [진화 로드맵 (4단계)](./OLLAMA_TO_CLAUDE_ROADMAP.md)
- 🧠 [Agent vs Tools 설명](./AGENT_VS_TOOLS_DIAGRAM.md)
- 💡 [초간단 설명 (비유)](./AGENT_TOOLS_SIMPLE_EXPLANATION.md)
- 🎓 [훈련 방식 비교](./CLAUDE_VS_QWEN_TRAINING.md)

### 설치 & 사용
- 🪟 [Windows 설치 가이드](./WINDOWS_INSTALLATION_GUIDE.md)
- 📘 [Ollama Agent 사용법](./OLLAMA_AGENT_GUIDE.md)
- 🔧 [트러블슈팅](./OLLAMA_TROUBLESHOOTING.md)

### Confluence 문서
- 🗂️ [Confluence INDEX](./confluence-docs/INDEX.md)
- 🚗 [Project 1: Automotive Modeler](./confluence-docs/PROJECT-1-Automotive-Modeler.md)
- 🏗️ [Project 2: Architecture Agent](./confluence-docs/PROJECT-2-Architecture-Agent.md)
- 📈 [Project 3: SDV Platform](./confluence-docs/PROJECT-3-SDV-Platform-Strategy.md)
- 🤖 [Project 4: Ollama AI Agent](./confluence-docs/PROJECT-4-Ollama-AI-Agent.md)

### GitHub 저장소
- 💻 [Ollama AI Agent](https://github.com/tobiaskim-hub/ollama-ai-agent)
- 🚗 [Automotive System Modeler](https://github.com/tobiaskim-hub/automotive-system-modeler)
- 🏗️ [Architecture Agent LLM](https://github.com/tobiaskim-hub/architecture-agent-llm)
- 📈 [Automotive SDV Platform](https://github.com/tobiaskim-hub/automotive-sdv-platform)

### 라이브 데모
- 🚗 [Automotive Modeler (Port 3000)](https://3000-ifl0a91rhl2v80gsfr4rn-8f57ffe2.sandbox.novita.ai)
- 🏗️ [Architecture Agent (Port 3001)](https://3001-ifl0a91rhl2v80gsfr4rn-8f57ffe2.sandbox.novita.ai)

---

## 🤝 기여

이슈나 제안사항이 있으시면 [GitHub Issues](https://github.com/tobiaskim-hub/ollama-ai-agent/issues)에 올려주세요!

---

## 📝 라이선스

MIT License - 자유롭게 사용하세요!

---

## 🙏 감사

- [Ollama](https://ollama.com/) - 로컬 LLM 플랫폼
- [Qwen](https://github.com/QwenLM/Qwen) - 우수한 한국어 지원
- [Anthropic Claude](https://www.anthropic.com/) - AI 개발 파트너
- [TypeScript](https://www.typescriptlang.org/) - 타입 안전성

---

**작성일**: 2026-02-02  
**버전**: 2.0.0  
**작성자**: Claude Code Agent + Tobias Kim

**🎯 목표: Ollama AI Agent를 GenS/Claude 수준으로!**
