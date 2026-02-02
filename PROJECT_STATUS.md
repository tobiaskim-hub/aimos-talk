# 전체 프로젝트 현황 보고서

**업데이트 날짜**: 2026-02-02  
**총 프로젝트 수**: 3개

---

## 📁 프로젝트 개요

```
/home/user/
├── webapp/                          # 프로젝트 1
├── architecture-agent/              # 프로젝트 2
└── local-llm-integration/           # 프로젝트 3 (신규)
```

---

## 🚗 프로젝트 1: Automotive System Modeler

### 기본 정보
- **디렉토리**: `/home/user/webapp`
- **포트**: 3000
- **상태**: ✅ 실행 중 (PM2)
- **목적**: EA-style 다이어그램 편집기

### 주요 기능
- 🎨 SVG 기반 다이어그램 편집
- 🚗 자동차 컴포넌트 (ECU, 센서, 액추에이터, CAN Bus)
- 🔗 드래그&드롭 연결선 그리기
- 💾 JSON 저장/로드
- 📊 속성 편집 패널

### 기술 스택
- **Frontend**: HTML + TailwindCSS + Vanilla JS
- **Backend**: Hono + TypeScript
- **Rendering**: SVG
- **Deployment**: Cloudflare Pages (planned)

### URL
- **공개**: https://3000-ifl0a91rhl2v80gsfr4rn-8f57ffe2.sandbox.novita.ai
- **로컬**: http://localhost:3000

### Git 상태
- ✅ 초기화 완료
- ✅ README.md 작성
- ✅ 최근 커밋: "docs: Add Architecture Agent link to related projects"

### PM2 상태
```
│ 0  │ automotive-modeler │ online │ 12660 │ 40m │ 26.0 MB │
```

---

## 🤖 프로젝트 2: Architecture Agent

### 기본 정보
- **디렉토리**: `/home/user/architecture-agent`
- **포트**: 3001
- **상태**: ✅ 실행 중 (PM2)
- **목적**: 프롬프트 기반 아키텍처 설계

### 주요 기능
- 💬 자연어 프롬프트 입력
- 📚 패턴 라이브러리 (Microservices, Layered, Event-Driven)
- 🎨 SVG 다이어그램 자동 생성
- 🤖 AI 대화형 어시스턴트 (Mock)
- 📋 요구사양 분석 (Mock)

### 기술 스택
- **Frontend**: HTML + TailwindCSS + Vanilla JS
- **Backend**: Hono + TypeScript
- **AI**: Mock (향후 OpenAI/Ollama 통합)
- **Deployment**: Cloudflare Pages (planned)

### URL
- **공개**: https://3001-ifl0a91rhl2v80gsfr4rn-8f57ffe2.sandbox.novita.ai
- **로컬**: http://localhost:3001

### Git 상태
- ✅ 초기화 완료
- ✅ README.md 작성
- ✅ 최근 커밋: "Initial commit: Architecture Agent - Prompt-based architecture design tool"

### PM2 상태
```
│ 1  │ architecture-agent │ online │ 12941 │ 70s │ 63.1 MB │
```

---

## 🧠 프로젝트 3: Local LLM Integration (신규)

### 기본 정보
- **디렉토리**: `/home/user/local-llm-integration`
- **포트**: 미정 (통합 프로젝트)
- **상태**: 🚧 계획 단계
- **목적**: 로컬 LLM을 Architecture Agent에 통합

### 계획된 기능
- 🔧 Ollama 통합 (Qwen2.5, Qwen2.5-Coder)
- 📚 RAG 시스템 (AUTOSAR, ISO26262 지식 베이스)
- 🏠 온디바이스 AI (오프라인 작동)
- 🔒 데이터 보안 (로컬 처리)
- 💰 비용 절감 (OpenAI API 대체)

### 기술 스택
- **LLM Engine**: Ollama
- **모델**: Qwen2.5:14B, Qwen2.5-Coder:7B
- **Vector DB**: Qdrant (로컬)
- **Embedding**: nomic-embed-text
- **하드웨어**: 맥 미니 M2 Pro (32GB)

### 상태
- 🚧 디렉토리 생성 완료
- 📝 전략 문서 작성 완료
- ⏳ 구현 대기

---

## 📊 프로젝트 비교

| 항목 | Automotive Modeler | Architecture Agent | Local LLM Integration |
|------|-------------------|--------------------|----------------------|
| **목적** | 수동 편집 | 자동 생성 | LLM 통합 |
| **입력** | 드래그&드롭 | 자연어 | - |
| **출력** | 다이어그램 | 다이어그램 | AI 응답 |
| **AI** | ❌ 없음 | Mock | ✅ 실제 LLM |
| **포트** | 3000 | 3001 | - |
| **상태** | ✅ 운영 | ✅ 운영 | 🚧 계획 |
| **사용자** | 엔지니어 | 아키텍트 | 개발자 |

---

## 🎯 전체 로드맵

### Phase 1: 현재 (완료)
- [x] Automotive Modeler MVP
- [x] Architecture Agent MVP
- [x] 두 프로젝트 분리
- [x] Git 초기화
- [x] PM2 설정
- [x] README 문서

### Phase 2: 통합 (진행 예정)
- [ ] Local LLM Integration
  - [ ] Ollama 설치 (로컬 맥)
  - [ ] Qwen 모델 테스트
  - [ ] RAG 시스템 구축
  - [ ] Architecture Agent 통합

### Phase 3: 고도화 (장기)
- [ ] Automotive Modeler 개선
  - [ ] Orthogonal Routing
  - [ ] 고급 정렬 기능
  - [ ] AUTOSAR 통합
- [ ] Architecture Agent 개선
  - [ ] 실제 LLM 통합 (OpenAI/Ollama)
  - [ ] RAG 고도화
  - [ ] Fine-tuning

### Phase 4: 통합 플랫폼 (미래)
- [ ] AutoBox (온디바이스 박스)
- [ ] Chromium 포크
- [ ] VS Code 포크
- [ ] 자동차 SDV 전용 플랫폼

---

## 💻 개발 환경

### 공통 환경
- **OS**: Linux (Sandbox)
- **Node.js**: v18+
- **Package Manager**: npm
- **Process Manager**: PM2
- **Build Tool**: Vite
- **Framework**: Hono

### 포트 할당
| 포트 | 프로젝트 | 상태 |
|------|---------|------|
| 3000 | Automotive Modeler | ✅ 사용 중 |
| 3001 | Architecture Agent | ✅ 사용 중 |
| 3002 | (예약) | 🔒 미사용 |

### PM2 명령어
```bash
# 전체 목록
pm2 list

# 특정 프로젝트 재시작
pm2 restart automotive-modeler
pm2 restart architecture-agent

# 로그 확인
pm2 logs automotive-modeler --nostream
pm2 logs architecture-agent --nostream

# 전체 중지
pm2 stop all

# 전체 삭제
pm2 delete all
```

---

## 📁 디렉토리 구조

```
/home/user/
├── webapp/                                    # 프로젝트 1
│   ├── src/
│   │   └── index.tsx                         # Hono 메인
│   ├── public/
│   │   └── static/
│   │       ├── app.js                        # 다이어그램 로직
│   │       └── styles.css                    # 스타일
│   ├── dist/                                 # 빌드 결과
│   ├── ecosystem.config.cjs                  # PM2 설정
│   ├── package.json
│   └── README.md
│
├── architecture-agent/                        # 프로젝트 2
│   ├── src/
│   │   └── index.tsx                         # Agent UI 인라인
│   ├── dist/                                 # 빌드 결과
│   ├── ecosystem.config.cjs                  # PM2 설정
│   ├── package.json
│   └── README.md
│
├── local-llm-integration/                     # 프로젝트 3
│   └── (미구현)
│
├── PROJECT_SEPARATION_REPORT.md              # 분리 보고서
├── AUTOMOTIVE_SDV_PLATFORM_STRATEGY.md       # SDV 전략 보고서
└── PROJECT_STATUS.md                         # 이 문서
```

---

## 🚀 빠른 시작 가이드

### 프로젝트 1 작업
```bash
cd /home/user/webapp
npm run build
pm2 restart automotive-modeler
curl http://localhost:3000
```

### 프로젝트 2 작업
```bash
cd /home/user/architecture-agent
npm run build
pm2 restart architecture-agent
curl http://localhost:3001
```

### 프로젝트 3 시작 (예정)
```bash
cd /home/user/local-llm-integration
# TODO: Ollama 통합
```

---

## 📊 메트릭

### 개발 통계
- **총 코드 라인**: ~5,000 LOC
- **Git 커밋**: 10+ commits
- **개발 기간**: 1주
- **빌드 시간**: ~2초 (각 프로젝트)
- **메모리 사용**: ~90MB (두 프로젝트 합계)

### 성능 지표
| 항목 | Automotive Modeler | Architecture Agent |
|------|-------------------|--------------------|
| **로딩 시간** | < 1초 | < 1초 |
| **다이어그램 생성** | 수동 | 3초 (Mock) |
| **메모리 사용** | 26 MB | 63 MB |
| **빌드 크기** | 37.9 KB | 50.3 KB |

---

## 🔗 관련 링크

### 프로젝트 URL
- [Automotive Modeler](https://3000-ifl0a91rhl2v80gsfr4rn-8f57ffe2.sandbox.novita.ai)
- [Architecture Agent](https://3001-ifl0a91rhl2v80gsfr4rn-8f57ffe2.sandbox.novita.ai)

### 문서
- [분리 보고서](./PROJECT_SEPARATION_REPORT.md)
- [SDV 전략 보고서](./AUTOMOTIVE_SDV_PLATFORM_STRATEGY.md)
- [Automotive Modeler README](./webapp/README.md)
- [Architecture Agent README](./architecture-agent/README.md)

---

## ✅ 다음 단계

### 즉시 (이번 주)
1. [ ] 로컬 맥에 Ollama 설치
2. [ ] Qwen 모델 테스트
3. [ ] 성능 평가 (속도, 품질)

### 단기 (1개월)
1. [ ] RAG 시스템 구축
2. [ ] Architecture Agent 통합
3. [ ] 파일럿 테스트

### 중기 (3개월)
1. [ ] 맥 미니 구매
2. [ ] AutoBox 프로토타입
3. [ ] 고객 피드백

### 장기 (1년)
1. [ ] Chromium 포크
2. [ ] VS Code 포크
3. [ ] 통합 플랫폼 출시

---

**현재 총 프로젝트 수: 3개**

**실행 중: 2개** (webapp, architecture-agent)  
**계획 중: 1개** (local-llm-integration)

**어떤 프로젝트를 우선 발전시킬까?** 😊
