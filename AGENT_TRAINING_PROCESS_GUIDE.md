# 🎓 AI Agent 단계별 훈련 프로세스 가이드

> **Ollama AI Agent를 GenS/Claude 수준으로 훈련시키는 완전한 실행 가이드**  
> 각 단계별 프로세스, 절차, 체크리스트

**작성일**: 2026-02-02  
**버전**: 1.0.0  
**작성자**: Claude Code Agent + Tobias Kim

---

## 📋 목차

- [전체 개요](#전체-개요)
- [📚 실전 예시 모음 (NEW!)](#실전-예시-모음)
- [Phase 0: 기초 구축 (완료)](#phase-0-기초-구축-완료)
- [Phase 1: 파일 도구 확장 (2주)](#phase-1-파일-도구-확장-2주)
- [Phase 2: 웹 연동 (2주)](#phase-2-웹-연동-2주)
- [Phase 3: 지능 강화 (3주)](#phase-3-지능-강화-3주)
- [Phase 4: GenS 수준 완성 (3주)](#phase-4-gens-수준-완성-3주)
- [매 단계 공통 프로세스](#매-단계-공통-프로세스)
- [트러블슈팅 가이드](#트러블슈팅-가이드)

---

## 📚 실전 예시 모음

### 🎯 왜 실전 예시가 중요한가?

**"이론보다 실전!"** 

추상적인 설명보다 구체적인 예시가 100배 효과적입니다:
- ❌ "Agent에게 API 호출 능력을 추가한다"
- ✅ "Tobi가 질문하면 지민이 Ollama API를 호출해서 자동 응답한다"

### 📖 실전 예시 문서

**[AGENT_TRAINING_REAL_EXAMPLE.md](./AGENT_TRAINING_REAL_EXAMPLE.md)** 문서에서 다음 내용을 확인하세요:

#### 🎓 실전 예시 1: AI Chat Room 자동 응답 추가

**7단계 프로세스로 구현:**
1. 요구사항 분석 (5분)
2. 현재 코드 분석 (10분)
3. 설계 (5분)
4. 구현 (15분)
5. 테스트 (5분)
6. 문서화 (3분)
7. 배포 (2분)

**배울 수 있는 것:**
- Ollama API 연동 방법
- 비동기 처리 (async/await)
- 로딩 UI 구현
- 에러 처리 패턴
- 재사용 가능한 코드 패턴

#### 🔄 일반화된 프로세스

모든 기능 추가에 적용 가능한 템플릿과 체크리스트:
- ✅ 요구사항 분석 체크리스트
- ✅ 설계 템플릿
- ✅ 테스트 시나리오 작성법
- ✅ 문서화 가이드

#### 📋 다른 Agent에 적용하기

같은 프로세스를 다른 프로젝트에도 적용:
- Ollama AI Agent에 웹 검색 기능 추가
- 파일 편집 기능 추가
- RAG 구현

**👉 자세한 내용은 [AGENT_TRAINING_REAL_EXAMPLE.md](./AGENT_TRAINING_REAL_EXAMPLE.md)를 참고하세요!**

---

## 전체 개요

### 훈련 로드맵

```
Week 0  (현재)    Phase 0: 기초 구축 ✅
                  ├── Agent: 1개
                  ├── Tools: 8개
                  └── 능력: 기본 파일/Git

Week 2            Phase 1: 파일 도구 확장 ⏳
                  ├── 추가 Tools: +10개
                  ├── 총 Tools: 18개
                  └── 능력: 복잡한 코드 리팩토링

Week 4            Phase 2: 웹 연동 ⏳
                  ├── 추가 Tools: +8개
                  ├── 총 Tools: 26개
                  └── 능력: 웹 검색, API 호출

Week 7            Phase 3: 지능 강화 ⏳
                  ├── 추가 Tools: +12개
                  ├── 총 Tools: 38개
                  └── 능력: RAG, 멀티모델

Week 10           Phase 4: GenS 수준 완성 🎉
                  ├── 추가 Tools: +15개
                  ├── 총 Tools: 45+개
                  └── 능력: 완전한 개발자!
```

### 각 Phase의 목표

| Phase | 기간 | 추가 도구 | 핵심 능력 | 난이도 |
|-------|------|----------|----------|--------|
| Phase 0 | 완료 | 8개 | 기본 파일/Git | ⭐ |
| Phase 1 | 2주 | +10개 | 파일 편집/검색 | ⭐⭐ |
| Phase 2 | 2주 | +8개 | 웹 검색/API | ⭐⭐⭐ |
| Phase 3 | 3주 | +12개 | RAG/멀티모델 | ⭐⭐⭐⭐ |
| Phase 4 | 3주 | +15개 | 배포 자동화 | ⭐⭐⭐⭐⭐ |

---

## Phase 0: 기초 구축 (완료)

### ✅ 완료 상태

#### 현재 구조
```
ollama-ai-agent/
├── src/
│   ├── agent.ts              # Agent (1개) ✅
│   ├── tools/
│   │   ├── file-tools.ts     # 파일 도구 4개 ✅
│   │   ├── command-tools.ts  # 명령어 도구 4개 ✅
│   │   └── index.ts          # 도구 통합 ✅
│   └── utils/
│       └── ollama-client.ts  # Ollama 클라이언트 ✅
├── package.json              ✅
├── tsconfig.json             ✅
└── README.md                 ✅
```

#### 현재 도구 (8개)

**파일 도구 (4개)**
- ✅ `read_file` - 파일 읽기
- ✅ `write_file` - 파일 쓰기
- ✅ `list_files` - 파일 목록
- ✅ `create_directory` - 디렉토리 생성

**명령어 도구 (4개)**
- ✅ `run_command` - 쉘 명령어 실행
- ✅ `git_status` - Git 상태 확인
- ✅ `git_commit` - Git 커밋
- ✅ `git_log` - Git 로그

---

## Phase 1: 파일 도구 확장 (2주)

### 🎯 목표
**파일 시스템 작업을 GenS 수준으로 강화**
- 파일 편집 (정확한 문자열 치환)
- 파일 검색 (Glob, Grep)
- 파일 관리 (복사, 이동, 삭제)

### 📅 일정

```
Week 1 (5일)
├── Day 1-2: Edit, MultiEdit 도구 개발
├── Day 3-4: Glob, Grep 도구 개발
└── Day 5: 테스트 및 디버깅

Week 2 (5일)
├── Day 1-2: LS, Copy, Move 도구 개발
├── Day 3-4: Delete, FileInfo, Symlink 도구 개발
└── Day 5: 통합 테스트 및 문서화
```

---

### 🔧 Step 1.1: Edit 도구 개발

#### 작업 프로세스

**1단계: 도구 파일 생성**

```bash
# Windows PowerShell
cd C:\A-SDV-Platform\ollama-ai-agent
New-Item -ItemType File -Path "src\tools\edit-tools.ts"
```

**2단계: 코드 작성**

```typescript
// src/tools/edit-tools.ts
import * as fs from 'fs/promises'

export interface Tool {
  name: string
  description: string
  parameters: Record<string, string>
  execute: (...args: any[]) => Promise<string>
}

/**
 * 파일 편집 도구 - 정확한 문자열 치환
 */
export const edit_file: Tool = {
  name: 'edit_file',
  description: '파일의 특정 부분을 수정합니다 (정확한 문자열 치환)',
  parameters: {
    file_path: 'string - 파일 경로',
    old_string: 'string - 찾을 문자열',
    new_string: 'string - 바꿀 문자열'
  },
  execute: async (file_path: string, old_string: string, new_string: string): Promise<string> => {
    try {
      // 1. 파일 읽기
      const content = await fs.readFile(file_path, 'utf-8')
      
      // 2. 문자열 개수 확인
      const regex = new RegExp(old_string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'), 'g')
      const count = (content.match(regex) || []).length
      
      if (count === 0) {
        return `❌ "${old_string}"을 찾을 수 없습니다`
      }
      if (count > 1) {
        return `⚠️  "${old_string}"이 ${count}개 발견되었습니다. 더 구체적인 문자열을 사용하세요`
      }
      
      // 3. 문자열 치환
      const newContent = content.replace(old_string, new_string)
      
      // 4. 파일 쓰기
      await fs.writeFile(file_path, newContent, 'utf-8')
      
      return `✅ 파일 수정 완료: ${file_path}\n변경사항: "${old_string}" → "${new_string}"`
    } catch (error: any) {
      return `❌ 에러: ${error.message}`
    }
  }
}

export const editTools = [edit_file]
```

**3단계: 도구 통합**

```typescript
// src/tools/index.ts
import { fileTools } from './file-tools'
import { commandTools } from './command-tools'
import { editTools } from './edit-tools'  // ← 추가

export * from './file-tools'
export * from './command-tools'
export * from './edit-tools'  // ← 추가

/**
 * 모든 도구를 하나의 배열로 통합
 */
export const allTools = [
  ...fileTools,
  ...commandTools,
  ...editTools  // ← 추가
]
```

**4단계: 빌드**

```bash
npm run build
```

**5단계: 테스트**

```bash
npm run dev

# 테스트 대화
💬 You: test.txt 파일을 만들어줘. 내용은 "Hello World"

💬 You: test.txt 파일에서 "World"를 "Ollama"로 바꿸줘
🤖 Agent: ✅ 파일 수정 완료: test.txt
          변경사항: "World" → "Ollama"

💬 You: test.txt 파일을 읽어줘
🤖 Agent: Hello Ollama
```

#### ✅ 체크리스트

- [ ] **파일 생성**: `src/tools/edit-tools.ts` 생성
- [ ] **코드 작성**: `edit_file` 도구 구현
- [ ] **도구 통합**: `src/tools/index.ts`에 추가
- [ ] **빌드 성공**: `npm run build` 오류 없음
- [ ] **테스트 통과**: 파일 수정 동작 확인
- [ ] **에러 처리**: 파일 없음, 문자열 없음, 중복 문자열 처리
- [ ] **Git 커밋**: 변경사항 커밋
- [ ] **문서 업데이트**: README.md에 도구 추가

---

### 🔧 Step 1.2: MultiEdit 도구 개발

#### 작업 프로세스

**1단계: 코드 추가**

```typescript
// src/tools/edit-tools.ts에 추가

/**
 * 다중 편집 도구 - 여러 곳 동시 수정
 */
export const multi_edit: Tool = {
  name: 'multi_edit',
  description: '파일의 여러 부분을 한 번에 수정합니다',
  parameters: {
    file_path: 'string - 파일 경로',
    edits: 'JSON array - [{old_string, new_string}, ...]'
  },
  execute: async (file_path: string, edits_json: string): Promise<string> => {
    try {
      // 1. JSON 파싱
      const edits = JSON.parse(edits_json)
      
      // 2. 파일 읽기
      let content = await fs.readFile(file_path, 'utf-8')
      
      // 3. 순차적으로 치환
      let changes = 0
      for (const edit of edits) {
        if (content.includes(edit.old_string)) {
          content = content.replace(edit.old_string, edit.new_string)
          changes++
        }
      }
      
      // 4. 파일 쓰기
      await fs.writeFile(file_path, content, 'utf-8')
      
      return `✅ ${changes}개 수정 완료: ${file_path}`
    } catch (error: any) {
      return `❌ 에러: ${error.message}`
    }
  }
}

export const editTools = [edit_file, multi_edit]  // ← 추가
```

**2단계: 테스트**

```bash
npm run build
npm run dev

# 테스트 대화
💬 You: src/agent.ts에서 여러 곳을 수정해줘:
       "qwen2.5:7b"를 "llama3"로,
       "temperature: 0.3"을 "temperature: 0.5"로

🤖 Agent: ✅ 2개 수정 완료: src/agent.ts
```

#### ✅ 체크리스트

- [ ] **코드 추가**: `multi_edit` 도구 구현
- [ ] **JSON 파싱**: 편집 배열 파싱 성공
- [ ] **순차 치환**: 여러 문자열 치환 동작
- [ ] **빌드 성공**: `npm run build` 오류 없음
- [ ] **테스트 통과**: 다중 수정 동작 확인
- [ ] **Git 커밋**: 변경사항 커밋

---

### 🔧 Step 1.3: Glob 도구 개발

#### 작업 프로세스

**1단계: 패키지 설치**

```bash
npm install glob
npm install --save-dev @types/glob
```

**2단계: 도구 파일 생성**

```typescript
// src/tools/search-tools.ts
import { glob } from 'glob'
import { execSync } from 'child_process'

export interface Tool {
  name: string
  description: string
  parameters: Record<string, string>
  execute: (...args: any[]) => Promise<string>
}

/**
 * Glob 검색 도구 - 패턴으로 파일 찾기
 */
export const glob_search: Tool = {
  name: 'glob_search',
  description: '패턴으로 파일을 찾습니다 (예: **/*.ts, src/**/*.json)',
  parameters: {
    pattern: 'string - 글롭 패턴'
  },
  execute: async (pattern: string): Promise<string> => {
    try {
      const files = await glob(pattern, { ignore: 'node_modules/**' })
      
      if (files.length === 0) {
        return `❌ "${pattern}" 패턴과 일치하는 파일이 없습니다`
      }
      
      return `✅ ${files.length}개 파일 발견:\n${files.join('\n')}`
    } catch (error: any) {
      return `❌ 에러: ${error.message}`
    }
  }
}

export const searchTools = [glob_search]
```

**3단계: 도구 통합 및 테스트**

```typescript
// src/tools/index.ts에 추가
import { searchTools } from './search-tools'

export const allTools = [
  ...fileTools,
  ...commandTools,
  ...editTools,
  ...searchTools  // ← 추가
]
```

**4단계: 테스트**

```bash
npm run build
npm run dev

💬 You: src 디렉토리에서 모든 TypeScript 파일을 찾아줘

🤖 Agent: ✅ 6개 파일 발견:
          src/agent.ts
          src/index.ts
          src/tools/file-tools.ts
          src/tools/command-tools.ts
          src/tools/edit-tools.ts
          src/tools/search-tools.ts
```

#### ✅ 체크리스트

- [ ] **패키지 설치**: `glob` 설치 완료
- [ ] **파일 생성**: `src/tools/search-tools.ts` 생성
- [ ] **코드 작성**: `glob_search` 도구 구현
- [ ] **도구 통합**: `src/tools/index.ts`에 추가
- [ ] **빌드 성공**: `npm run build` 오류 없음
- [ ] **테스트 통과**: 패턴 검색 동작 확인
- [ ] **Git 커밋**: 변경사항 커밋

---

### 🔧 Step 1.4: Grep 도구 개발

#### 작업 프로세스

**1단계: 코드 추가**

```typescript
// src/tools/search-tools.ts에 추가

/**
 * Grep 검색 도구 - 파일 내용 검색
 */
export const grep_search: Tool = {
  name: 'grep_search',
  description: '파일 내용에서 텍스트를 검색합니다 (정규표현식 지원)',
  parameters: {
    pattern: 'string - 검색할 패턴',
    path: 'string - 검색 경로 (기본: .)'
  },
  execute: async (pattern: string, path: string = '.'): Promise<string> => {
    try {
      const result = execSync(`grep -r -n "${pattern}" ${path} 2>/dev/null || true`, { 
        encoding: 'utf-8',
        maxBuffer: 10 * 1024 * 1024  // 10MB
      })
      
      if (!result.trim()) {
        return `❌ "${pattern}"을 찾을 수 없습니다`
      }
      
      const lines = result.split('\n').filter(l => l.trim())
      const preview = lines.slice(0, 20).join('\n')
      
      if (lines.length > 20) {
        return `✅ ${lines.length}개 결과 (처음 20개만 표시):\n${preview}\n...`
      }
      
      return `✅ ${lines.length}개 결과:\n${preview}`
    } catch (error: any) {
      return `❌ 에러: ${error.message}`
    }
  }
}

export const searchTools = [glob_search, grep_search]  // ← 추가
```

**2단계: 테스트**

```bash
npm run build
npm run dev

💬 You: src 디렉토리에서 "Agent" 텍스트를 검색해줘

🤖 Agent: ✅ 15개 결과:
          src/agent.ts:5:export class AIAgent {
          src/agent.ts:10:  constructor() {
          ...
```

#### ✅ 체크리스트

- [ ] **코드 추가**: `grep_search` 도구 구현
- [ ] **정규표현식**: 패턴 검색 동작
- [ ] **결과 제한**: 20개 미리보기 동작
- [ ] **빌드 성공**: `npm run build` 오류 없음
- [ ] **테스트 통과**: 내용 검색 동작 확인
- [ ] **Git 커밋**: 변경사항 커밋

---

### 🔧 Step 1.5~1.10: 나머지 도구 개발

#### 추가할 도구 목록

**5. LS Detail (상세 파일 목록)**
```typescript
// 파일 크기, 수정일, 권한 표시
```

**6. Copy File (파일 복사)**
```typescript
// fs.copyFile 사용
```

**7. Move File (파일 이동)**
```typescript
// fs.rename 사용
```

**8. Delete File (파일 삭제)**
```typescript
// fs.unlink 사용 (확인 필요)
```

**9. File Info (파일 메타데이터)**
```typescript
// fs.stat으로 크기, 날짜, 권한 조회
```

**10. Create Symlink (심볼릭 링크)**
```typescript
// fs.symlink 사용
```

#### 공통 프로세스

각 도구마다:
1. 코드 작성
2. 도구 통합
3. 빌드
4. 테스트
5. Git 커밋

---

### 📊 Phase 1 완료 체크리스트

#### 개발 완료
- [ ] **1. edit_file** - 파일 수정
- [ ] **2. multi_edit** - 다중 수정
- [ ] **3. glob_search** - 패턴 검색
- [ ] **4. grep_search** - 내용 검색
- [ ] **5. ls_detail** - 상세 목록
- [ ] **6. copy_file** - 파일 복사
- [ ] **7. move_file** - 파일 이동
- [ ] **8. delete_file** - 파일 삭제
- [ ] **9. file_info** - 파일 정보
- [ ] **10. create_symlink** - 심볼릭 링크

#### 테스트 완료
- [ ] **단위 테스트**: 각 도구 개별 동작 확인
- [ ] **통합 테스트**: 여러 도구 연속 사용
- [ ] **에러 처리**: 잘못된 입력 처리 확인
- [ ] **Agent 통합**: Agent가 도구 선택 확인

#### 문서화 완료
- [ ] **README 업데이트**: 새 도구 목록 추가
- [ ] **도구 설명**: 각 도구 사용법 문서화
- [ ] **예시 추가**: 실제 사용 예시 작성

#### Git 관리
- [ ] **10개 커밋**: 각 도구마다 커밋
- [ ] **태그 생성**: `v0.2.0-phase1-complete`
- [ ] **GitHub Push**: 원격 저장소 동기화

#### 성능 검증
- [ ] **도구 개수**: 18개 (8 + 10)
- [ ] **응답 시간**: 3초 이내
- [ ] **에러율**: 5% 이하

---

## Phase 2: 웹 연동 (2주)

### 🎯 목표
**웹 검색, API 호출, 이미지 이해 능력 추가**
- 웹 검색 (DuckDuckGo)
- 웹 페이지 가져오기
- HTTP API 호출
- 파일 다운로드

### 📅 일정

```
Week 3 (5일)
├── Day 1-2: WebSearch, WebFetch 도구 개발
├── Day 3-4: HTTP Request 도구 개발
└── Day 5: 테스트 및 디버깅

Week 4 (5일)
├── Day 1-2: Crawler, Download 도구 개발
├── Day 3-4: 나머지 웹 도구 개발
└── Day 5: 통합 테스트 및 문서화
```

---

### 🔧 Step 2.1: WebSearch 도구 개발

#### 작업 프로세스

**1단계: 패키지 설치**

```bash
npm install axios cheerio
npm install --save-dev @types/cheerio
```

**2단계: 도구 파일 생성**

```typescript
// src/tools/web-tools.ts
import axios from 'axios'
import * as cheerio from 'cheerio'

export interface Tool {
  name: string
  description: string
  parameters: Record<string, string>
  execute: (...args: any[]) => Promise<string>
}

/**
 * 웹 검색 도구 - DuckDuckGo API 사용
 */
export const web_search: Tool = {
  name: 'web_search',
  description: '웹에서 정보를 검색합니다 (DuckDuckGo API)',
  parameters: {
    query: 'string - 검색어'
  },
  execute: async (query: string): Promise<string> => {
    try {
      const url = `https://api.duckduckgo.com/?q=${encodeURIComponent(query)}&format=json`
      const response = await axios.get(url, { timeout: 10000 })
      
      const results = response.data.RelatedTopics
        .slice(0, 5)
        .map((r: any) => `- ${r.Text}\n  ${r.FirstURL}`)
        .join('\n\n')
      
      if (!results) {
        return `❌ "${query}"에 대한 검색 결과가 없습니다`
      }
      
      return `✅ 검색 결과 (${query}):\n\n${results}`
    } catch (error: any) {
      return `❌ 에러: ${error.message}`
    }
  }
}

export const webTools = [web_search]
```

**3단계: 도구 통합**

```typescript
// src/tools/index.ts에 추가
import { webTools } from './web-tools'

export const allTools = [
  ...fileTools,
  ...commandTools,
  ...editTools,
  ...searchTools,
  ...webTools  // ← 추가
]
```

**4단계: 테스트**

```bash
npm run build
npm run dev

💬 You: Ollama 최신 문서를 검색해줘

🤖 Agent: ✅ 검색 결과 (Ollama 최신 문서):

          - Ollama is a tool for running large language models locally...
            https://ollama.com/
          
          - Download Ollama for Windows, macOS, and Linux...
            https://ollama.com/download
          
          ...
```

#### ✅ 체크리스트

- [ ] **패키지 설치**: `axios`, `cheerio` 설치
- [ ] **파일 생성**: `src/tools/web-tools.ts` 생성
- [ ] **코드 작성**: `web_search` 도구 구현
- [ ] **API 테스트**: DuckDuckGo API 동작 확인
- [ ] **도구 통합**: `src/tools/index.ts`에 추가
- [ ] **빌드 성공**: `npm run build` 오류 없음
- [ ] **테스트 통과**: 웹 검색 동작 확인
- [ ] **타임아웃 처리**: 10초 타임아웃 동작
- [ ] **Git 커밋**: 변경사항 커밋

---

### 🔧 Step 2.2~2.8: 나머지 웹 도구 개발

#### 추가할 도구 목록

**2. WebFetch (웹 페이지 가져오기)**
```typescript
// axios + cheerio로 HTML → 텍스트 추출
```

**3. HTTP Request (API 호출)**
```typescript
// GET, POST, PUT, DELETE 지원
```

**4. Web Crawler (크롤링)**
```typescript
// 링크 추출 및 재귀 크롤링
```

**5. Download File (파일 다운로드)**
```typescript
// URL → 로컬 파일 저장
```

**6-8. 기타 웹 도구**
```typescript
// Screenshot, RSS Feed 등
```

---

### 📊 Phase 2 완료 체크리스트

#### 개발 완료
- [ ] **1. web_search** - 웹 검색
- [ ] **2. web_fetch** - 페이지 가져오기
- [ ] **3. http_request** - API 호출
- [ ] **4. web_crawler** - 크롤링
- [ ] **5. download_file** - 파일 다운로드
- [ ] **6-8. 기타 도구** - 추가 웹 도구

#### 테스트 완료
- [ ] **웹 검색**: DuckDuckGo API 동작
- [ ] **페이지 가져오기**: HTML 파싱 동작
- [ ] **API 호출**: HTTP 메서드 동작
- [ ] **네트워크 에러**: 타임아웃, 연결 실패 처리

#### 문서화 완료
- [ ] **README 업데이트**: 웹 도구 추가
- [ ] **API 문서**: 각 도구 사용법

#### Git 관리
- [ ] **8개 커밋**: 각 도구마다 커밋
- [ ] **태그 생성**: `v0.3.0-phase2-complete`

#### 성능 검증
- [ ] **도구 개수**: 26개 (18 + 8)
- [ ] **웹 요청**: 10초 타임아웃 동작
- [ ] **에러 처리**: 네트워크 에러 처리

---

## Phase 3: 지능 강화 (3주)

### 🎯 목표
**RAG, 멀티모델, 프로젝트 관리 능력 추가**
- RAG (문서 기반 답변)
- MultiModel (여러 모델 사용)
- 프로젝트 자동화

### 📅 일정

```
Week 5-6 (10일)
├── Day 1-3: RAG 도구 개발 (ChromaDB)
├── Day 4-6: MultiModel 도구 개발
├── Day 7-9: 프로젝트 관리 도구 개발
└── Day 10: 테스트 및 디버깅

Week 7 (5일)
├── Day 1-3: 배포 도구 개발 (PM2, Docker)
├── Day 4: 통합 테스트
└── Day 5: 문서화
```

---

### 🔧 Step 3.1: RAG 도구 개발

#### 작업 프로세스

**1단계: 패키지 설치**

```bash
npm install chromadb
npm install langchain @langchain/community
```

**2단계: Ollama 임베딩 모델 설치**

```bash
ollama pull nomic-embed-text
```

**3단계: 도구 파일 생성**

```typescript
// src/tools/rag-tools.ts
import { ChromaClient } from 'chromadb'
import { OllamaClient } from '../utils/ollama-client'
import * as fs from 'fs/promises'

export interface Tool {
  name: string
  description: string
  parameters: Record<string, string>
  execute: (...args: any[]) => Promise<string>
}

/**
 * RAG 인덱싱 도구 - 문서를 ChromaDB에 저장
 */
export const rag_index: Tool = {
  name: 'rag_index',
  description: '문서를 인덱싱하여 검색 가능하게 만듭니다',
  parameters: {
    file_path: 'string - 파일 경로'
  },
  execute: async (file_path: string): Promise<string> => {
    try {
      // 1. 파일 읽기
      const content = await fs.readFile(file_path, 'utf-8')
      
      // 2. 청크로 분할 (500자씩)
      const chunkSize = 500
      const chunks: string[] = []
      for (let i = 0; i < content.length; i += chunkSize) {
        chunks.push(content.substring(i, i + chunkSize))
      }
      
      // 3. ChromaDB 클라이언트 생성
      const client = new ChromaClient()
      const collection = await client.getOrCreateCollection({ 
        name: 'documents' 
      })
      
      // 4. Ollama 임베딩 모델 사용
      const ollamaClient = new OllamaClient({ 
        model: 'nomic-embed-text',
        baseUrl: 'http://localhost:11434' 
      })
      
      // 5. 임베딩 생성 및 저장
      for (let i = 0; i < chunks.length; i++) {
        const embedding = await ollamaClient.embed(chunks[i])
        
        await collection.add({
          ids: [`${file_path}-${i}`],
          embeddings: [embedding],
          documents: [chunks[i]],
          metadatas: [{ source: file_path, chunk: i }]
        })
      }
      
      return `✅ 문서 인덱싱 완료: ${file_path} (${chunks.length}개 청크)`
    } catch (error: any) {
      return `❌ 에러: ${error.message}`
    }
  }
}

/**
 * RAG 검색 도구 - 문서에서 관련 내용 검색
 */
export const rag_search: Tool = {
  name: 'rag_search',
  description: '인덱싱된 문서에서 관련 내용을 검색합니다',
  parameters: {
    query: 'string - 검색 질의',
    top_k: 'number - 결과 개수 (기본: 3)'
  },
  execute: async (query: string, top_k: number = 3): Promise<string> => {
    try {
      // 1. Ollama 임베딩
      const ollamaClient = new OllamaClient({ 
        model: 'nomic-embed-text',
        baseUrl: 'http://localhost:11434' 
      })
      const queryEmbedding = await ollamaClient.embed(query)
      
      // 2. ChromaDB 검색
      const client = new ChromaClient()
      const collection = await client.getCollection({ name: 'documents' })
      
      const results = await collection.query({
        queryEmbeddings: [queryEmbedding],
        nResults: top_k
      })
      
      // 3. 결과 포맷팅
      const context = results.documents[0].join('\n\n---\n\n')
      
      return `✅ 관련 문서 검색 완료 (${top_k}개):\n\n${context}`
    } catch (error: any) {
      return `❌ 에러: ${error.message}`
    }
  }
}

export const ragTools = [rag_index, rag_search]
```

**4단계: OllamaClient에 embed 메서드 추가**

```typescript
// src/utils/ollama-client.ts에 추가

export class OllamaClient {
  // ... 기존 코드 ...
  
  /**
   * 텍스트 임베딩 생성
   */
  async embed(text: string): Promise<number[]> {
    const response = await axios.post(`${this.baseUrl}/api/embeddings`, {
      model: this.model,
      prompt: text
    })
    
    return response.data.embedding
  }
}
```

**5단계: 테스트**

```bash
npm run build
npm run dev

💬 You: AUTOSAR_Guide.pdf 파일을 인덱싱해줘

🤖 Agent: ✅ 문서 인덱싱 완료: AUTOSAR_Guide.pdf (127개 청크)

💬 You: AUTOSAR에서 CAN 통신 설정 방법을 찾아줘

🤖 Agent: ✅ 관련 문서 검색 완료 (3개):

          [CAN 통신 설정 방법 내용...]
          
          ---
          
          [관련 내용 2...]
```

#### ✅ 체크리스트

- [ ] **패키지 설치**: `chromadb`, `langchain` 설치
- [ ] **모델 설치**: `nomic-embed-text` 다운로드
- [ ] **파일 생성**: `src/tools/rag-tools.ts` 생성
- [ ] **RAG 인덱싱**: 문서 청크화 및 저장 동작
- [ ] **RAG 검색**: 유사도 검색 동작
- [ ] **임베딩 생성**: Ollama 임베딩 동작
- [ ] **빌드 성공**: `npm run build` 오류 없음
- [ ] **테스트 통과**: RAG 전체 흐름 동작
- [ ] **Git 커밋**: 변경사항 커밋

---

### 🔧 Step 3.2~3.12: 나머지 도구 개발

#### 추가할 도구 목록

**MultiModel 도구**
```typescript
// 작업 유형에 따라 최적 모델 선택
// - 한국어: qwen2.5:7b
// - 코드: codellama
// - 빠른 응답: llama3.2:3b
```

**프로젝트 관리 도구**
```typescript
// - todo_write: TODO 관리
// - project_backup: 프로젝트 백업
// - git_push: GitHub 푸시
// - npm_install: 의존성 설치
// - pm2_start: PM2 서버 시작
// - docker_run: Docker 실행
```

---

### 📊 Phase 3 완료 체크리스트

#### 개발 완료
- [ ] **1-2. RAG 도구** - 인덱싱, 검색
- [ ] **3. MultiModel** - 다중 모델 관리
- [ ] **4-12. 프로젝트 도구** - 자동화 도구

#### 테스트 완료
- [ ] **RAG 동작**: 문서 인덱싱 및 검색
- [ ] **임베딩**: nomic-embed-text 동작
- [ ] **멀티모델**: 모델 자동 선택 동작
- [ ] **프로젝트 관리**: PM2, Docker 동작

#### 문서화 완료
- [ ] **README 업데이트**: RAG 사용법
- [ ] **가이드 작성**: RAG 설정 가이드

#### Git 관리
- [ ] **12개 커밋**: 각 도구마다 커밋
- [ ] **태그 생성**: `v0.4.0-phase3-complete`

#### 성능 검증
- [ ] **도구 개수**: 38개 (26 + 12)
- [ ] **RAG 속도**: 검색 3초 이내
- [ ] **임베딩 품질**: 관련 문서 검색 정확도 80%+

---

## Phase 4: GenS 수준 완성 (3주)

### 🎯 목표
**배포 자동화, 코드 품질, 문서 생성 능력 추가**
- 고급 Git 워크플로우
- Cloudflare 배포
- 코드 품질 도구
- 문서 자동 생성

### 📅 일정

```
Week 8-9 (10일)
├── Day 1-3: 고급 Git 도구 개발
├── Day 4-6: 배포 도구 개발
├── Day 7-9: 코드 품질 도구 개발
└── Day 10: 테스트

Week 10 (5일)
├── Day 1-3: 문서 생성 도구 개발
├── Day 4: 통합 테스트
└── Day 5: 최종 문서화 및 축하 🎉
```

---

### 🔧 Step 4.1~4.15: 도구 개발

#### 추가할 도구 목록

**고급 Git (5개)**
- git_branch, git_merge, git_diff, git_stash, git_remote

**배포 자동화 (3개)**
- cloudflare_deploy, docker_build, k8s_deploy

**코드 품질 (3개)**
- lint_fix, format_code, type_check

**문서 생성 (3개)**
- generate_readme, generate_docs, diagram_generate

**디버깅 (1개)**
- debug_analyze

---

### 📊 Phase 4 완료 체크리스트

#### 개발 완료
- [ ] **1-5. 고급 Git** - Branch, Merge, Diff, Stash, Remote
- [ ] **6-8. 배포** - Cloudflare, Docker, K8s
- [ ] **9-11. 품질** - Lint, Format, Type
- [ ] **12-14. 문서** - README, Docs, Diagram
- [ ] **15. 디버깅** - Debug Analyze

#### 테스트 완료
- [ ] **Git 워크플로우**: 복잡한 Git 작업 동작
- [ ] **배포 자동화**: Cloudflare 배포 성공
- [ ] **코드 품질**: Lint, Format 동작
- [ ] **문서 생성**: README 자동 생성

#### 문서화 완료
- [ ] **완전 가이드 업데이트**: 모든 도구 문서화
- [ ] **사용 예시**: 실전 시나리오 작성
- [ ] **베스트 프랙티스**: 권장 사용법

#### Git 관리
- [ ] **15개 커밋**: 각 도구마다 커밋
- [ ] **태그 생성**: `v1.0.0-phase4-complete`
- [ ] **릴리즈 노트**: GitHub Release 작성

#### 성능 검증
- [ ] **도구 개수**: 45+개 ✅
- [ ] **응답 시간**: 2초 이내
- [ ] **에러율**: 3% 이하
- [ ] **GenS 수준**: 90% 달성 🎉

---

## 매 단계 공통 프로세스

### 📋 도구 개발 표준 프로세스

#### 1. 계획 단계
```
[ ] 도구 이름 정의
[ ] 기능 명세 작성
[ ] 파라미터 정의
[ ] 예상 출력 정의
[ ] 에러 케이스 정의
```

#### 2. 개발 단계
```
[ ] 파일 생성
[ ] 인터페이스 정의
[ ] 핵심 로직 구현
[ ] 에러 처리 추가
[ ] 로깅 추가
```

#### 3. 통합 단계
```
[ ] tools/index.ts에 추가
[ ] allTools 배열에 추가
[ ] export 문 추가
[ ] 타입 체크
```

#### 4. 테스트 단계
```
[ ] 빌드 테스트 (npm run build)
[ ] 단위 테스트 (개별 도구)
[ ] 통합 테스트 (Agent와 함께)
[ ] 에러 처리 테스트
[ ] 성능 테스트
```

#### 5. 문서화 단계
```
[ ] 도구 설명 작성
[ ] 파라미터 문서화
[ ] 사용 예시 작성
[ ] README 업데이트
```

#### 6. Git 관리
```
[ ] git add
[ ] git commit (의미있는 메시지)
[ ] git push
[ ] 태그 생성 (필요시)
```

---

### 🧪 테스트 체크리스트

#### Agent 통합 테스트
```
[ ] Agent가 도구를 올바르게 선택하는가?
[ ] LLM이 파라미터를 정확하게 추출하는가?
[ ] 도구 실행 결과가 올바른가?
[ ] 결과 요약이 사용자 친화적인가?
```

#### 에러 처리 테스트
```
[ ] 파일 없음
[ ] 권한 부족
[ ] 네트워크 에러
[ ] 타임아웃
[ ] 잘못된 파라미터
[ ] 예외 상황
```

#### 성능 테스트
```
[ ] 응답 시간 3초 이내
[ ] 메모리 사용량 정상 범위
[ ] 동시 요청 처리
[ ] 큰 파일 처리
```

---

### 📊 진행 상황 추적

#### 주간 리포트 템플릿

```markdown
## Week X 진행 리포트

### 완료 항목
- [ ] 도구 1: xxx (100%)
- [ ] 도구 2: xxx (100%)
- [ ] 도구 3: xxx (50%)

### 이슈
- [ ] 문제 1: 해결 방법
- [ ] 문제 2: 진행 중

### 다음 주 계획
- [ ] 도구 4 개발
- [ ] 도구 5 개발

### 통계
- 총 도구 개수: XX개
- 코드 줄 수: XXX줄
- 테스트 커버리지: XX%
```

---

## 트러블슈팅 가이드

### 🚨 자주 발생하는 문제

#### 1. JSON 파싱 에러

**증상**:
```
⚠️ LLM 응답 파싱 실패
```

**원인**: LLM이 JSON 대신 일반 텍스트 응답

**해결**:
```typescript
// agent.ts의 systemPrompt 강화
"중요: JSON만 출력하세요. 다른 텍스트는 포함하지 마세요.
예시: {\"tool\": \"read_file\", \"params\": [\"test.txt\"]}"
```

#### 2. 도구 선택 실패

**증상**:
```
❌ 도구를 선택하지 못했습니다
```

**원인**: 도구 설명이 불명확

**해결**:
```typescript
// 도구 설명을 더 구체적으로
description: '파일을 읽습니다' // ❌
description: '지정한 경로의 파일을 읽어서 내용을 반환합니다' // ✅
```

#### 3. 타임아웃

**증상**:
```
❌ 에러: timeout of 10000ms exceeded
```

**해결**:
```typescript
// axios 설정에 타임아웃 증가
const response = await axios.get(url, { timeout: 30000 })
```

#### 4. 메모리 부족

**증상**:
```
❌ 에러: JavaScript heap out of memory
```

**해결**:
```bash
# Node.js 메모리 증가
node --max-old-space-size=4096 dist/index.js
```

---

## 최종 검증 체크리스트

### 🎯 GenS 수준 달성 확인

#### 기능 검증
- [ ] **파일 작업**: 읽기/쓰기/편집/검색 완벽
- [ ] **Git 관리**: 모든 Git 작업 가능
- [ ] **웹 연동**: 검색/API/크롤링 동작
- [ ] **지능**: RAG/멀티모델 동작
- [ ] **배포**: Cloudflare/Docker 배포 가능
- [ ] **문서**: 자동 생성 가능

#### 성능 검증
- [ ] **도구 개수**: 45+개 ✅
- [ ] **응답 시간**: 평균 2초 이내
- [ ] **정확도**: 90% 이상
- [ ] **안정성**: 에러율 3% 이하

#### 사용성 검증
- [ ] **자연어 이해**: 복잡한 요청 이해
- [ ] **도구 선택**: 정확한 도구 선택
- [ ] **결과 요약**: 사용자 친화적 설명
- [ ] **에러 처리**: 명확한 에러 메시지

---

## 📝 최종 요약

### Phase별 요약

| Phase | 기간 | 도구 | 핵심 능력 | 체크리스트 |
|-------|------|------|----------|-----------|
| **Phase 0** | 완료 | 8개 | 기본 파일/Git | ✅ 완료 |
| **Phase 1** | 2주 | +10개 | 파일 편집/검색 | 📋 10개 항목 |
| **Phase 2** | 2주 | +8개 | 웹 검색/API | 📋 8개 항목 |
| **Phase 3** | 3주 | +12개 | RAG/멀티모델 | 📋 12개 항목 |
| **Phase 4** | 3주 | +15개 | 배포 자동화 | 📋 15개 항목 |

### 총 체크리스트

- **총 도구 개발**: 45개
- **총 테스트 항목**: 135개 (각 도구당 3개)
- **총 문서 항목**: 45개
- **총 Git 커밋**: 45개 (최소)

---

**작성일**: 2026-02-02  
**버전**: 1.0.0  
**작성자**: Claude Code Agent + Tobias Kim

**🎯 목표: 10주 안에 GenS/Claude 수준 달성!**
