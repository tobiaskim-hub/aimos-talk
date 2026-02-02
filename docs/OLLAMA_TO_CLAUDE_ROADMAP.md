# 🎯 Ollama AI Agent → GenS/Claude Code Agent 진화 로드맵

> **목표**: Ollama AI Agent를 GenS/Claude Code Agent 수준의 완전한 개발자로 만들기

**작성일**: 2026-02-02  
**버전**: 1.0.0  
**예상 기간**: 3~6개월

---

## 📊 현재 상태 (Phase 0)

### ✅ 완료된 기능 (8개 도구)

#### 파일 관리 (4개)
- `read_file` - 파일 읽기
- `write_file` - 파일 쓰기
- `list_files` - 파일 목록
- `create_directory` - 디렉토리 생성

#### 명령어 실행 (4개)
- `run_command` - 쉘 명령어 실행
- `git_status` - Git 상태 확인
- `git_commit` - Git 커밋
- `git_log` - Git 로그

### 📈 현재 능력치
- 도구 개수: **8개** (목표: 45+개)
- 기능 범위: **20%** (기본 파일/Git만)
- 자동화: **30%** (단순 작업만)
- 지능: **40%** (Qwen 7B 기반)

---

## 🎯 목표 상태 (Phase 4 완료 시)

### GenS/Claude Code Agent 수준
- 도구 개수: **45+개**
- 기능 범위: **100%** (파일/Git/웹/이미지/배포)
- 자동화: **95%** (복잡한 프로젝트 관리)
- 지능: **90%** (멀티 모델 + RAG)

---

## 📅 진화 로드맵 (4단계)

---

## 🚀 Phase 1: 핵심 파일 도구 확장 (2주)

**목표**: 파일 시스템 작업을 GenS 수준으로 강화

### 추가할 도구 (10개)

#### 1. Edit (파일 편집)
```typescript
// src/tools/edit-tools.ts
export const edit_file: Tool = {
  name: 'edit_file',
  description: '파일의 특정 부분을 수정합니다 (정확한 문자열 치환)',
  parameters: {
    file_path: 'string - 파일 경로',
    old_string: 'string - 찾을 문자열',
    new_string: 'string - 바꿀 문자열'
  },
  execute: async (file_path: string, old_string: string, new_string: string): Promise<string> => {
    const content = await fs.readFile(file_path, 'utf-8')
    const count = (content.match(new RegExp(old_string, 'g')) || []).length
    
    if (count === 0) {
      return `❌ "${old_string}"을 찾을 수 없습니다`
    }
    if (count > 1) {
      return `⚠️  "${old_string}"이 ${count}개 발견되었습니다. 더 구체적인 문자열을 사용하세요`
    }
    
    const newContent = content.replace(old_string, new_string)
    await fs.writeFile(file_path, newContent, 'utf-8')
    
    return `✅ 파일 수정 완료: ${file_path}\n변경사항: "${old_string}" → "${new_string}"`
  }
}
```

#### 2. MultiEdit (여러 곳 동시 수정)
```typescript
export const multi_edit: Tool = {
  name: 'multi_edit',
  description: '파일의 여러 부분을 한 번에 수정합니다',
  parameters: {
    file_path: 'string - 파일 경로',
    edits: 'JSON array - [{old_string, new_string}, ...]'
  },
  execute: async (file_path: string, edits_json: string): Promise<string> => {
    const edits = JSON.parse(edits_json)
    let content = await fs.readFile(file_path, 'utf-8')
    
    for (const edit of edits) {
      content = content.replace(edit.old_string, edit.new_string)
    }
    
    await fs.writeFile(file_path, content, 'utf-8')
    return `✅ ${edits.length}개 수정 완료: ${file_path}`
  }
}
```

#### 3. Glob (파일 패턴 검색)
```typescript
import { glob } from 'glob'

export const glob_search: Tool = {
  name: 'glob_search',
  description: '패턴으로 파일을 찾습니다 (예: **/*.ts, src/**/*.json)',
  parameters: {
    pattern: 'string - 글롭 패턴'
  },
  execute: async (pattern: string): Promise<string> => {
    const files = await glob(pattern, { ignore: 'node_modules/**' })
    
    if (files.length === 0) {
      return `❌ "${pattern}" 패턴과 일치하는 파일이 없습니다`
    }
    
    return `✅ ${files.length}개 파일 발견:\n${files.join('\n')}`
  }
}
```

#### 4. Grep (내용 검색)
```typescript
import { execSync } from 'child_process'

export const grep_search: Tool = {
  name: 'grep_search',
  description: '파일 내용에서 텍스트를 검색합니다 (정규표현식 지원)',
  parameters: {
    pattern: 'string - 검색할 패턴',
    path: 'string - 검색 경로 (기본: .)'
  },
  execute: async (pattern: string, path: string = '.'): Promise<string> => {
    try {
      const result = execSync(`grep -r -n "${pattern}" ${path}`, { encoding: 'utf-8' })
      const lines = result.split('\n').filter(l => l.trim())
      
      return `✅ ${lines.length}개 결과:\n${lines.slice(0, 20).join('\n')}`
    } catch {
      return `❌ "${pattern}"을 찾을 수 없습니다`
    }
  }
}
```

#### 5. LS (디렉토리 상세 정보)
```typescript
export const ls_detail: Tool = {
  name: 'ls_detail',
  description: '디렉토리의 상세 파일 목록 (크기, 수정일시)',
  parameters: {
    path: 'string - 디렉토리 경로 (기본: .)'
  },
  execute: async (path: string = '.'): Promise<string> => {
    const files = await fs.readdir(path, { withFileTypes: true })
    
    const details = await Promise.all(files.map(async (file) => {
      const fullPath = `${path}/${file.name}`
      const stats = await fs.stat(fullPath)
      const size = (stats.size / 1024).toFixed(2) + ' KB'
      const modified = stats.mtime.toISOString().split('T')[0]
      const type = file.isDirectory() ? '📁' : '📄'
      
      return `${type} ${file.name.padEnd(30)} ${size.padEnd(12)} ${modified}`
    }))
    
    return `✅ ${path} 디렉토리:\n${details.join('\n')}`
  }
}
```

#### 6~10. 추가 파일 도구
```typescript
// 6. copy_file - 파일 복사
// 7. move_file - 파일 이동
// 8. delete_file - 파일 삭제
// 9. file_info - 파일 메타데이터 (크기, 수정일, 권한)
// 10. create_symlink - 심볼릭 링크 생성
```

### 예상 효과
- 파일 작업 능력 **60% → 90%** 향상
- 복잡한 코드 리팩토링 가능
- 프로젝트 구조 분석 가능

---

## 🌐 Phase 2: 웹 & 외부 연동 (2주)

**목표**: 웹 검색, API 호출, 이미지 이해 능력 추가

### 추가할 도구 (8개)

#### 1. WebSearch (웹 검색)
```typescript
import axios from 'axios'

export const web_search: Tool = {
  name: 'web_search',
  description: '웹에서 정보를 검색합니다 (DuckDuckGo API)',
  parameters: {
    query: 'string - 검색어'
  },
  execute: async (query: string): Promise<string> => {
    // DuckDuckGo API 또는 Brave Search API 사용
    const url = `https://api.duckduckgo.com/?q=${encodeURIComponent(query)}&format=json`
    const response = await axios.get(url)
    
    const results = response.data.RelatedTopics
      .slice(0, 5)
      .map((r: any) => `- ${r.Text}\n  ${r.FirstURL}`)
      .join('\n')
    
    return `✅ 검색 결과 (${query}):\n${results}`
  }
}
```

#### 2. WebFetch (웹 페이지 가져오기)
```typescript
import * as cheerio from 'cheerio'

export const web_fetch: Tool = {
  name: 'web_fetch',
  description: '웹 페이지의 내용을 가져옵니다 (HTML → 텍스트)',
  parameters: {
    url: 'string - 웹 페이지 URL'
  },
  execute: async (url: string): Promise<string> => {
    const response = await axios.get(url)
    const $ = cheerio.load(response.data)
    
    // HTML에서 텍스트만 추출
    $('script, style').remove()
    const text = $('body').text().replace(/\s+/g, ' ').trim()
    
    return `✅ 웹 페이지 내용 (${url}):\n${text.slice(0, 2000)}...`
  }
}
```

#### 3. HTTP Request (API 호출)
```typescript
export const http_request: Tool = {
  name: 'http_request',
  description: 'HTTP API 요청 (GET/POST/PUT/DELETE)',
  parameters: {
    method: 'string - HTTP 메서드',
    url: 'string - API URL',
    data: 'JSON - 요청 데이터 (선택)'
  },
  execute: async (method: string, url: string, data?: string): Promise<string> => {
    const config = {
      method,
      url,
      data: data ? JSON.parse(data) : undefined
    }
    
    const response = await axios(config)
    return `✅ API 응답 (${method} ${url}):\n${JSON.stringify(response.data, null, 2)}`
  }
}
```

#### 4. Crawler (웹 크롤링)
```typescript
export const web_crawler: Tool = {
  name: 'web_crawler',
  description: '웹사이트를 크롤링하여 링크와 정보 수집',
  parameters: {
    url: 'string - 시작 URL',
    depth: 'number - 크롤링 깊이 (기본: 1)'
  },
  execute: async (url: string, depth: number = 1): Promise<string> => {
    // 간단한 크롤러 구현
    const visited = new Set<string>()
    const links: string[] = []
    
    async function crawl(current_url: string, current_depth: number) {
      if (current_depth > depth || visited.has(current_url)) return
      visited.add(current_url)
      
      const response = await axios.get(current_url)
      const $ = cheerio.load(response.data)
      
      $('a[href]').each((_, el) => {
        const href = $(el).attr('href')
        if (href && href.startsWith('http')) {
          links.push(href)
        }
      })
    }
    
    await crawl(url, 0)
    return `✅ 크롤링 완료:\n${links.slice(0, 10).join('\n')}`
  }
}
```

#### 5~8. 추가 웹 도구
```typescript
// 5. download_file - URL에서 파일 다운로드
// 6. image_understand - 이미지 URL을 받아서 설명 생성 (Ollama Vision 모델 사용)
// 7. screenshot - 웹 페이지 스크린샷 (Puppeteer)
// 8. rss_feed - RSS 피드 읽기
```

### 예상 효과
- 인터넷 연결 **0% → 80%** 향상
- 최신 정보 검색 가능
- API 문서 자동 조회

---

## 🤖 Phase 3: 지능 & 자동화 강화 (3주)

**목표**: RAG, 멀티 모델, 프로젝트 관리 능력 추가

### 추가할 도구 (12개)

#### 1. RAG (문서 기반 답변)
```typescript
import { ChromaClient } from 'chromadb'
import { OllamaClient } from '../utils/ollama-client'

export class RAGTools {
  private chroma: ChromaClient
  private embedModel: OllamaClient
  
  constructor() {
    this.chroma = new ChromaClient()
    this.embedModel = new OllamaClient({ 
      model: 'nomic-embed-text',
      baseUrl: 'http://localhost:11434' 
    })
  }
  
  // 1. rag_index - 문서 인덱싱
  async rag_index(file_path: string): Promise<string> {
    const content = await fs.readFile(file_path, 'utf-8')
    
    // 문서를 청크로 분할 (500자씩)
    const chunks = content.match(/.{1,500}/g) || []
    
    // 임베딩 생성
    const embeddings = await Promise.all(
      chunks.map(chunk => this.embedModel.embed(chunk))
    )
    
    // ChromaDB에 저장
    const collection = await this.chroma.getOrCreateCollection({ name: 'documents' })
    await collection.add({
      ids: chunks.map((_, i) => `${file_path}-${i}`),
      embeddings,
      documents: chunks,
      metadatas: chunks.map(() => ({ source: file_path }))
    })
    
    return `✅ 문서 인덱싱 완료: ${file_path} (${chunks.length}개 청크)`
  }
  
  // 2. rag_search - 문서에서 답변 검색
  async rag_search(query: string, top_k: number = 3): Promise<string> {
    const queryEmbedding = await this.embedModel.embed(query)
    
    const collection = await this.chroma.getCollection({ name: 'documents' })
    const results = await collection.query({
      queryEmbeddings: [queryEmbedding],
      nResults: top_k
    })
    
    const context = results.documents[0].join('\n\n')
    
    return `✅ 관련 문서 검색 완료:\n${context}`
  }
}
```

#### 2. MultiModel (여러 모델 사용)
```typescript
export class MultiModelAgent {
  private models: Map<string, OllamaClient>
  
  constructor() {
    this.models = new Map([
      ['korean', new OllamaClient({ model: 'qwen2.5:7b' })],      // 한국어 우수
      ['code', new OllamaClient({ model: 'codellama:13b' })],     // 코드 생성
      ['fast', new OllamaClient({ model: 'llama3.2:3b' })],       // 빠른 응답
      ['vision', new OllamaClient({ model: 'llava:13b' })]        // 이미지 이해
    ])
  }
  
  // 작업 유형에 따라 최적 모델 자동 선택
  selectModel(task: string): OllamaClient {
    if (task.includes('코드') || task.includes('함수')) {
      return this.models.get('code')!
    }
    if (task.includes('이미지') || task.includes('사진')) {
      return this.models.get('vision')!
    }
    if (task.includes('빠르게')) {
      return this.models.get('fast')!
    }
    return this.models.get('korean')! // 기본값
  }
}
```

#### 3~12. 추가 자동화 도구
```typescript
// 3. todo_write - TODO 목록 관리
// 4. project_backup - 프로젝트 백업 (tar.gz)
// 5. git_push - GitHub에 푸시
// 6. git_clone - 저장소 클론
// 7. npm_install - 의존성 설치
// 8. npm_run - npm 스크립트 실행
// 9. pm2_start - PM2로 서버 시작
// 10. docker_run - Docker 컨테이너 실행
// 11. env_setup - 환경변수 설정
// 12. test_run - 테스트 실행
```

### 예상 효과
- 프로젝트 관리 **0% → 80%** 향상
- 문서 기반 답변 가능
- 멀티 모델 최적화

---

## 🎓 Phase 4: GenS 수준 완성 (3주)

**목표**: 완전한 개발자 수준의 자동화

### 추가할 도구 (15개)

#### 고급 Git 워크플로우
```typescript
// 1. git_branch - 브랜치 관리
// 2. git_merge - 브랜치 병합
// 3. git_diff - 변경사항 비교
// 4. git_stash - 임시 저장
// 5. git_remote - 리모트 관리
```

#### 배포 자동화
```typescript
// 6. cloudflare_deploy - Cloudflare Pages 배포
// 7. docker_build - Docker 이미지 빌드
// 8. k8s_deploy - Kubernetes 배포
```

#### 코드 품질
```typescript
// 9. lint_fix - ESLint 자동 수정
// 10. format_code - Prettier 포맷팅
// 11. type_check - TypeScript 타입 체크
```

#### 문서화
```typescript
// 12. generate_readme - README 자동 생성
// 13. generate_docs - JSDoc → 문서 생성
// 14. diagram_generate - 다이어그램 생성 (Mermaid)
```

#### 디버깅
```typescript
// 15. debug_analyze - 에러 로그 분석 및 해결책 제안
```

### 최종 능력치
- 도구 개수: **45+개**
- 기능 범위: **100%**
- 자동화: **95%**
- 지능: **90%**

---

## 📈 진행 추적

### 완료 체크리스트

#### Phase 0: 기초 (✅ 완료)
- [x] 파일 읽기/쓰기
- [x] 명령어 실행
- [x] Git 기본 (status/commit/log)
- [x] CLI 인터페이스

#### Phase 1: 파일 도구 확장 (⏳ 진행 중)
- [ ] Edit (파일 수정)
- [ ] MultiEdit (다중 수정)
- [ ] Glob (패턴 검색)
- [ ] Grep (내용 검색)
- [ ] LS (상세 목록)
- [ ] 파일 복사/이동/삭제

#### Phase 2: 웹 연동 (⏳ 대기)
- [ ] WebSearch (웹 검색)
- [ ] WebFetch (페이지 가져오기)
- [ ] HTTP Request (API 호출)
- [ ] Crawler (크롤링)
- [ ] 이미지 이해

#### Phase 3: 지능 강화 (⏳ 대기)
- [ ] RAG (문서 기반 답변)
- [ ] MultiModel (여러 모델)
- [ ] TODO 관리
- [ ] 프로젝트 백업
- [ ] Git Push
- [ ] PM2/Docker

#### Phase 4: GenS 수준 (⏳ 대기)
- [ ] 고급 Git 워크플로우
- [ ] 배포 자동화
- [ ] 코드 품질
- [ ] 문서 자동 생성
- [ ] 디버깅

---

## 🎯 마일스톤

| 마일스톤 | 완료 조건 | 예상일 | 상태 |
|---------|---------|--------|------|
| **M1: 파일 마스터** | 10개 파일 도구 완성 | 2주 후 | ⏳ 진행 중 |
| **M2: 웹 연결** | 8개 웹 도구 완성 | 4주 후 | ⏳ 대기 |
| **M3: 지능 업그레이드** | RAG + MultiModel | 7주 후 | ⏳ 대기 |
| **M4: GenS 수준** | 45+ 도구 완성 | 10주 후 | ⏳ 대기 |

---

## 💡 실전 예시

### 현재 (Phase 0)
```bash
You: README.md 파일을 읽어줘
Agent: [파일 내용 출력]

You: 새 파일 만들어줘
Agent: [파일 생성]
```

### Phase 1 완료 후
```bash
You: src/ 디렉토리에서 모든 TypeScript 파일을 찾아줘
Agent: [glob_search: **/*.ts 실행]
       ✅ 12개 파일 발견

You: src/agent.ts 파일에서 "qwen2.5:7b"를 "llama3"로 바꿔줘
Agent: [edit_file 실행]
       ✅ 파일 수정 완료
```

### Phase 2 완료 후
```bash
You: Ollama 최신 문서를 찾아줘
Agent: [web_search 실행]
       ✅ 검색 결과 5개 발견

You: https://ollama.com/library/qwen2.5 페이지 내용을 가져와줘
Agent: [web_fetch 실행]
       ✅ 페이지 내용 추출 완료
```

### Phase 3 완료 후
```bash
You: AUTOSAR 가이드를 인덱싱해줘
Agent: [rag_index 실행]
       ✅ 문서 인덱싱 완료 (247개 청크)

You: AUTOSAR에서 CAN 통신은 어떻게 설정하나요?
Agent: [rag_search 실행 → LLM 답변 생성]
       ✅ AUTOSAR CAN 통신 설정 방법: ...
```

### Phase 4 완료 후 (GenS 수준!)
```bash
You: 새 프로젝트를 만들고, Git 저장소를 초기화하고, README를 작성하고, GitHub에 올려줘

Agent: 
   🔧 1/5: create_directory("my-project")
   ✅ 디렉토리 생성 완료
   
   🔧 2/5: git_init()
   ✅ Git 저장소 초기화 완료
   
   🔧 3/5: generate_readme()
   ✅ README.md 생성 완료
   
   🔧 4/5: git_commit("Initial commit")
   ✅ 커밋 완료
   
   🔧 5/5: git_push("origin main")
   ✅ GitHub에 푸시 완료
   
   🎉 모든 작업 완료! 프로젝트가 준비되었습니다!
```

---

## 🚀 지금 바로 시작하기

### Step 1: Phase 1 첫 번째 도구 추가 (Edit)

```bash
# 1. 새 파일 생성
cd /home/user/ollama-ai-agent

# 2. Edit 도구 추가
cat > src/tools/edit-tools.ts << 'EOF'
import * as fs from 'fs/promises'

export interface Tool {
  name: string
  description: string
  parameters: Record<string, string>
  execute: (...args: any[]) => Promise<string>
}

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
      const content = await fs.readFile(file_path, 'utf-8')
      const count = (content.match(new RegExp(old_string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'), 'g')) || []).length
      
      if (count === 0) {
        return `❌ "${old_string}"을 찾을 수 없습니다`
      }
      if (count > 1) {
        return `⚠️  "${old_string}"이 ${count}개 발견되었습니다. 더 구체적인 문자열을 사용하세요`
      }
      
      const newContent = content.replace(old_string, new_string)
      await fs.writeFile(file_path, newContent, 'utf-8')
      
      return `✅ 파일 수정 완료: ${file_path}\n변경사항: "${old_string}" → "${new_string}"`
    } catch (error: any) {
      return `❌ 에러: ${error.message}`
    }
  }
}

export const editTools = [edit_file]
EOF

# 3. tools/index.ts에 통합
cat >> src/tools/index.ts << 'EOF'

export * from './edit-tools'

// allTools 업데이트
import { editTools } from './edit-tools'
export const allTools = [
  ...fileTools,
  ...commandTools,
  ...editTools
]
EOF

# 4. 빌드 및 테스트
npm run build
npm run dev
```

### Step 2: 테스트
```bash
You: test.txt 파일을 만들어줘. 내용은 "Hello World"

You: test.txt 파일에서 "World"를 "Ollama"로 바꿔줘
Agent: [edit_file 실행]
       ✅ 파일 수정 완료: test.txt
       변경사항: "World" → "Ollama"

You: test.txt 파일을 읽어줘
Agent: Hello Ollama
```

---

## 📊 예상 타임라인

```
현재 (Week 0)
├── Phase 0 완료: 8개 도구 ✅
│
Week 2
├── Phase 1 완료: 18개 도구 (+10)
│   └── 파일 편집/검색 마스터
│
Week 4
├── Phase 2 완료: 26개 도구 (+8)
│   └── 웹 검색/API 연동
│
Week 7
├── Phase 3 완료: 38개 도구 (+12)
│   └── RAG + 멀티모델 + 프로젝트 관리
│
Week 10
└── Phase 4 완료: 45+ 도구 (+15)
    └── GenS/Claude Code Agent 수준 달성! 🎉
```

---

## 🎯 성공 지표

### 정량적 지표
- 도구 개수: **8 → 45+개** (562% 증가)
- 자동화 가능한 작업: **20 → 95%** (375% 증가)
- 응답 속도: **3초 → 1초** (300% 향상)

### 정성적 지표
- ✅ 복잡한 프로젝트 생성 가능
- ✅ 웹에서 최신 정보 검색
- ✅ 문서 기반 전문가 답변
- ✅ 다중 작업 자동화
- ✅ 배포 자동화

---

## 💰 비용

- 총 개발 비용: **$0** (로컬 LLM 기반)
- 클라우드 API 비용: **$0**
- 예상 시간: **10주** (주 10시간)

---

## 📚 참고 자료

- [Ollama 공식 문서](https://ollama.com/docs)
- [LangChain 가이드](https://js.langchain.com/docs)
- [ChromaDB 문서](https://docs.trychroma.com/)
- [GenS/Claude Code Agent 스타일 가이드](https://docs.anthropic.com/)

---

## 🤝 기여

Phase별로 도구를 추가하면서 GitHub에 커밋하고 문서화하세요!

---

**작성일**: 2026-02-02  
**작성자**: Tobias Kim + Claude Code Agent  
**버전**: 1.0.0  
**상태**: 🚀 Phase 1 진행 예정
