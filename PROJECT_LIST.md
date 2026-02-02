# 📋 프로젝트 목록 (Project List)

> **업데이트**: 2026-02-02 12:15 KST  
> **총 프로젝트**: 4개 (운영 2개, 문서 1개, 개발 중 1개)  
> **환경**: Sandbox Linux + Windows PC  

---

## 🚀 **운영 중인 프로젝트 (2개)**

### **Project 1: Automotive System Modeler** 🚗

**상태**: ✅ 운영 중 (PM2: automotive-modeler, 포트 3000)  
**경로**: `/home/user/webapp/`  
**GitHub**: https://github.com/tobiaskim-hub/automotive-system-modeler  
**라이브 데모**: https://3000-ifl0a91rhl2v80gsfr4rn-8f57ffe2.sandbox.novita.ai

**설명**: 웹 기반 실시간 UML/SysML 다이어그램 편집기

**주요 기능**:
- 🎨 드래그 앤 드롭 다이어그램 편집
- 🔗 실시간 노드 연결
- 💾 JSON Export/Import
- 📐 그리드 스냅 (10px)
- 🎯 포트 기반 연결

**기술 스택**:
- Frontend: Hono + TypeScript + Vite
- UI: Tailwind CSS + Font Awesome
- Backend: Cloudflare Workers
- 배포: PM2 (Sandbox)

**통계**:
- 파일: 12개
- 코드: ~1,500줄
- 커밋: 17개
- PM2 상태: Online (2h uptime, 재시작 38회)

---

### **Project 2: Architecture Agent + LLM** 🤖

**상태**: ✅ 운영 중 (PM2: architecture-agent, 포트 3001)  
**경로**: `/home/user/architecture-agent/`  
**GitHub**: https://github.com/tobiaskim-hub/architecture-agent-llm  
**라이브 데모**: https://3001-ifl0a91rhl2v80gsfr4rn-8f57ffe2.sandbox.novita.ai

**설명**: AI 기반 자동 아키텍처 다이어그램 생성기 (Ollama + Qwen 2.5 통합)

**주요 기능**:
- 🤖 자연어 프롬프트로 다이어그램 생성
- 🎨 실시간 SVG 렌더링
- 💬 AI 어시스턴트 (우측 패널)
- 🔌 Ollama LLM 연동
- ⚡ 2-3초 응답 속도

**기술 스택**:
- Frontend: Hono + TypeScript + Vite
- AI: Ollama + Qwen 2.5:7B (로컬)
- UI: Tailwind CSS + Font Awesome
- Backend: Hono API (/api/generate, /api/health)
- 배포: PM2 (Sandbox)

**통계**:
- 파일: 14개
- 코드: ~1,200줄
- 커밋: 3개
- PM2 상태: Online (3h uptime, 재시작 1회)

**API 엔드포인트**:
- `GET /` - 프론트엔드 UI
- `POST /api/generate` - LLM 다이어그램 생성
- `GET /api/health` - Ollama 연결 상태 확인

---

## 📚 **문서 프로젝트 (1개)**

### **Project 3: Automotive SDV Platform Documentation** 📖

**상태**: ✅ 완료 (문서만)  
**경로**: `/home/user/confluence-docs/`  
**GitHub**: https://github.com/tobiaskim-hub/automotive-sdv-platform  
**문서 시작**: https://github.com/tobiaskim-hub/automotive-sdv-platform/blob/main/INDEX.md

**설명**: Confluence 스타일 전체 프로젝트 문서 (INDEX + 3개 프로젝트 상세 문서)

**포함 문서**:
- `INDEX.md` - 메인 허브 페이지 (7.5KB)
- `PROJECT-1-Automotive-Modeler.md` - 상세 가이드 (9.4KB)
- `PROJECT-2-Architecture-Agent.md` - LLM 통합 가이드 (14.2KB)
- `PROJECT-3-SDV-Platform-Strategy.md` - 전략 및 로드맵 (10.3KB)
- `README.md` - 사용 가이드 (3.3KB)

**통계**:
- 파일: 5개
- 총 크기: 41.4KB
- 총 줄: ~2,220줄
- 총 표: 34개
- 총 코드 블록: 34개
- 커밋: 1개

---

## 🚧 **개발 중인 프로젝트 (1개)**

### **Project 4: Ollama AI Agent** 🛠️ (NEW!)

**상태**: 🔨 개발 중 (방금 생성!)  
**경로**: `/home/user/ollama-ai-agent/`  
**GitHub**: 아직 업로드 안 됨  
**라이브 데모**: 아직 없음

**설명**: Ollama를 AI 개발자로 만드는 프로젝트 (파일 읽기/쓰기, 명령어 실행, Git 관리)

**목표**:
- ✅ Claude처럼 파일 읽기/쓰기
- ✅ 명령어 자동 실행
- ✅ Git 자동 커밋
- ✅ 대화형 코딩 어시스턴트

**기술 스택**:
- Language: TypeScript
- LLM: Ollama + Qwen 2.5:7B (또는 Llama 3)
- Tools: 파일 시스템, Git, Shell 명령어
- UI: 터미널 기반 (CLI)

**프로젝트 구조**:
```
ollama-ai-agent/
├── src/
│   ├── tools/
│   │   ├── file-tools.ts      # 파일 읽기/쓰기/목록
│   │   ├── command-tools.ts   # 명령어 실행, Git
│   │   └── index.ts           # 도구 통합
│   ├── utils/
│   │   └── ollama-client.ts   # Ollama 통신
│   ├── agent.ts               # Agent 메인 로직
│   └── index.ts               # CLI 진입점
├── package.json
├── tsconfig.json
└── README.md
```

**현재 진행 상황**:
- ✅ 프로젝트 구조 생성
- ✅ package.json 설정
- ✅ tsconfig.json 설정
- ✅ 파일 도구 구현 (file-tools.ts)
- ✅ 명령어 도구 구현 (command-tools.ts)
- ✅ 도구 통합 (index.ts)
- 🔨 Ollama 클라이언트 구현 중...
- ⏳ Agent 로직 구현 예정
- ⏳ CLI 인터페이스 구현 예정

**다음 단계**:
1. Ollama 클라이언트 완성
2. Agent 메인 로직 구현
3. CLI 인터페이스 구현
4. 테스트
5. README 작성
6. GitHub 업로드
7. 실전 사용!

**참고 문서**: `/home/user/OLLAMA_AGENT_GUIDE.md` (완전 가이드)

---

## 📊 **전체 통계**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📦 프로젝트
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
총 프로젝트:           4개
운영 중:               2개
문서:                  1개
개발 중:               1개

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🌐 GitHub 저장소
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
총 저장소:             3개 (Project 4는 미업로드)
총 커밋:               21개
총 파일:               31개
총 코드 줄:            ~4,967줄

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📚 문서
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
총 문서:               46개
Confluence 문서:       5개
메인 가이드:           11개
프로젝트 상세 문서:    30개

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎮 배포
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PM2 서비스:            2개 (모두 Online)
Public URL:            2개
포트:                  3000, 3001

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
💻 기술 스택
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Frontend:              Hono + TypeScript + Vite
UI:                    Tailwind CSS + Font Awesome
Backend:               Hono API
AI/LLM:                Ollama + Qwen 2.5:7B
DevOps:                Git + GitHub + PM2
```

---

## 🗂️ **디렉토리 구조**

```
/home/user/
├── webapp/                       # Project 1: Automotive Modeler
│   ├── src/
│   ├── public/
│   └── package.json
│
├── architecture-agent/           # Project 2: Architecture Agent
│   ├── src/
│   ├── package.json
│   └── .dev.vars
│
├── confluence-docs/              # Project 3: Documentation
│   ├── INDEX.md
│   ├── PROJECT-1-*.md
│   ├── PROJECT-2-*.md
│   └── PROJECT-3-*.md
│
├── ollama-ai-agent/              # Project 4: Ollama AI Agent (NEW!)
│   ├── src/
│   │   ├── tools/
│   │   └── utils/
│   ├── package.json
│   └── tsconfig.json
│
├── local-llm-integration/        # LLM 통합 가이드
│   ├── INSTALLATION_GUIDE.md
│   ├── INTEGRATION_GUIDE.md
│   └── QUICK_START.md
│
├── COMPLETE_PROJECT_DOCUMENTATION.md  # 통합 문서
├── OLLAMA_AGENT_GUIDE.md             # Ollama Agent 가이드
├── LLM_INTEGRATION_COMPLETE.md       # LLM 통합 완료 보고
├── PROJECT_STATUS.md                  # 프로젝트 현황
├── WINDOWS_INSTALLATION_GUIDE.md     # Windows 설치 가이드
└── [기타 문서들...]
```

---

## 🔗 **빠른 링크**

### **GitHub 저장소**
- Project 1: https://github.com/tobiaskim-hub/automotive-system-modeler
- Project 2: https://github.com/tobiaskim-hub/architecture-agent-llm
- Project 3: https://github.com/tobiaskim-hub/automotive-sdv-platform

### **라이브 데모**
- Project 1: https://3000-ifl0a91rhl2v80gsfr4rn-8f57ffe2.sandbox.novita.ai
- Project 2: https://3001-ifl0a91rhl2v80gsfr4rn-8f57ffe2.sandbox.novita.ai

### **주요 문서**
- 통합 문서: `/home/user/COMPLETE_PROJECT_DOCUMENTATION.md`
- Ollama 가이드: `/home/user/OLLAMA_AGENT_GUIDE.md`
- Confluence INDEX: `/home/user/confluence-docs/INDEX.md`

---

## 📅 **개발 타임라인**

```
2026-02-02 (오늘)
├── 09:00 - Project 1 & 2 분리 완료
├── 10:00 - LLM 통합 완료
├── 11:00 - GitHub 업로드 완료
├── 11:30 - Confluence 문서 작성 완료
├── 12:00 - 통합 문서 작성 완료
└── 12:15 - Project 4 시작! ← 지금 여기!
```

---

## 🎯 **다음 할 일**

### **Project 4 완성**
1. ✅ 프로젝트 구조 생성
2. ✅ 도구 구현 (파일, 명령어)
3. 🔨 Ollama 클라이언트 완성
4. ⏳ Agent 로직 구현
5. ⏳ CLI 인터페이스 구현
6. ⏳ 테스트
7. ⏳ README 작성
8. ⏳ GitHub 업로드

### **Windows PC 작업**
- [ ] Ollama 설치 완료 확인
- [ ] Architecture Agent LLM 테스트
- [ ] Project 4 로컬에서 실행

---

**생성일**: 2026-02-02 12:15 KST  
**마지막 업데이트**: 2026-02-02 12:15 KST  
**다음 업데이트**: Project 4 완성 후
