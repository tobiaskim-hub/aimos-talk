# 🤖 Ollama를 AI 개발자로 만들기 (완전 가이드)

> **목표**: Ollama + Qwen을 Claude처럼 파일 읽고, 코드 작성하고, Git 커밋하는 AI 개발자로 만들기

---

## 📋 목차

1. [개념 이해](#개념-이해)
2. [방법 비교](#방법-비교)
3. [방법 1: 직접 구현](#방법-1-직접-구현)
4. [방법 2: LangChain 사용](#방법-2-langchain-사용)
5. [방법 3: Open WebUI 사용](#방법-3-open-webui-사용)
6. [방법 4: Continue.dev 사용](#방법-4-continuedev-사용)
7. [방법 5: Aider 사용](#방법-5-aider-사용)
8. [실전 프로젝트](#실전-프로젝트)

---

## 🧠 개념 이해

### Claude (GenS) vs Ollama 차이

```
Claude (GenS):
┌─────────────────┐
│  Claude LLM     │  "README.md를 읽어줘"
└────────┬────────┘
         │
         ↓
┌─────────────────┐
│  도구 시스템    │
│  - Read         │  파일 읽기 → 결과 반환
│  - Write        │
│  - Bash         │
└─────────────────┘

Ollama만:
┌─────────────────┐
│  Ollama LLM     │  "파일을 읽으려면 cat README.md 명령어를 사용하세요"
└─────────────────┘
         ❌ 직접 실행 안 함!

Ollama + 도구:
┌─────────────────┐
│  Ollama LLM     │  "README.md를 읽어줘"
└────────┬────────┘
         │
         ↓
┌─────────────────┐
│  직접 만든 도구 │
│  - readFile()   │  파일 읽기 → 결과 반환 ✅
│  - writeFile()  │
│  - runCommand() │
└─────────────────┘
```

---

## ⚖️ 방법 비교

| 방법 | 난이도 | 유연성 | 기능 | 추천도 |
|------|--------|--------|------|--------|
| **1. 직접 구현** | ⭐⭐⭐⭐ 어려움 | ⭐⭐⭐⭐⭐ 최고 | 모든 기능 | ⭐⭐⭐ |
| **2. LangChain** | ⭐⭐⭐ 보통 | ⭐⭐⭐⭐ 높음 | 대부분 | ⭐⭐⭐⭐⭐ |
| **3. Open WebUI** | ⭐ 쉬움 | ⭐⭐ 낮음 | 기본적 | ⭐⭐⭐⭐ |
| **4. Continue.dev** | ⭐ 쉬움 | ⭐⭐⭐ 보통 | 코드 중심 | ⭐⭐⭐⭐ |
| **5. Aider** | ⭐⭐ 쉬움 | ⭐⭐⭐ 보통 | Git 통합 | ⭐⭐⭐⭐⭐ |

---

## 🛠️ 방법 1: 직접 구현

### 개요

완전히 처음부터 만들어서 최대 제어권 확보!

### 프로젝트 생성

```bash
mkdir ollama-agent
cd ollama-agent
npm init -y
npm install typescript ts-node @types/node
npm install axios
npx tsc --init
```

### 핵심 코드

#### `src/tools.ts` - 도구 정의

```typescript
import { readFileSync, writeFileSync, existsSync, mkdirSync } from 'fs'
import { exec } from 'child_process'
import { promisify } from 'util'

const execAsync = promisify(exec)

export interface Tool {
  name: string
  description: string
  parameters: any
  execute: (...args: any[]) => Promise<string>
}

export const tools: Tool[] = [
  {
    name: 'read_file',
    description: '파일의 내용을 읽습니다',
    parameters: {
      path: 'string - 파일 경로'
    },
    execute: async (path: string) => {
      try {
        const content = readFileSync(path, 'utf-8')
        return `파일 내용:\n${content}`
      } catch (error) {
        return `에러: 파일을 읽을 수 없습니다 - ${error.message}`
      }
    }
  },
  
  {
    name: 'write_file',
    description: '파일에 내용을 씁니다',
    parameters: {
      path: 'string - 파일 경로',
      content: 'string - 파일 내용'
    },
    execute: async (path: string, content: string) => {
      try {
        writeFileSync(path, content, 'utf-8')
        return `파일 작성 완료: ${path}`
      } catch (error) {
        return `에러: 파일을 쓸 수 없습니다 - ${error.message}`
      }
    }
  },
  
  {
    name: 'run_command',
    description: '쉘 명령어를 실행합니다',
    parameters: {
      command: 'string - 실행할 명령어'
    },
    execute: async (command: string) => {
      try {
        const { stdout, stderr } = await execAsync(command)
        return stdout || stderr || '명령어 실행 완료'
      } catch (error) {
        return `에러: ${error.message}`
      }
    }
  },
  
  {
    name: 'list_files',
    description: '디렉토리의 파일 목록을 출력합니다',
    parameters: {
      path: 'string - 디렉토리 경로 (기본: .)'
    },
    execute: async (path: string = '.') => {
      try {
        const { stdout } = await execAsync(`ls -la ${path}`)
        return stdout
      } catch (error) {
        return `에러: ${error.message}`
      }
    }
  },
  
  {
    name: 'git_status',
    description: 'Git 상태를 확인합니다',
    parameters: {},
    execute: async () => {
      try {
        const { stdout } = await execAsync('git status')
        return stdout
      } catch (error) {
        return `에러: ${error.message}`
      }
    }
  },
  
  {
    name: 'git_commit',
    description: 'Git 커밋을 수행합니다',
    parameters: {
      message: 'string - 커밋 메시지'
    },
    execute: async (message: string) => {
      try {
        await execAsync('git add .')
        const { stdout } = await execAsync(`git commit -m "${message}"`)
        return stdout
      } catch (error) {
        return `에러: ${error.message}`
      }
    }
  }
]
```

#### `src/llm.ts` - Ollama 통신

```typescript
import axios from 'axios'

export interface OllamaOptions {
  model: string
  baseUrl: string
}

export class OllamaClient {
  private model: string
  private baseUrl: string
  
  constructor(options: OllamaOptions) {
    this.model = options.model
    this.baseUrl = options.baseUrl
  }
  
  async generate(prompt: string): Promise<string> {
    try {
      const response = await axios.post(`${this.baseUrl}/api/generate`, {
        model: this.model,
        prompt: prompt,
        stream: false
      })
      
      return response.data.response
    } catch (error) {
      throw new Error(`Ollama 에러: ${error.message}`)
    }
  }
  
  async chat(messages: Array<{role: string, content: string}>): Promise<string> {
    try {
      const response = await axios.post(`${this.baseUrl}/api/chat`, {
        model: this.model,
        messages: messages,
        stream: false
      })
      
      return response.data.message.content
    } catch (error) {
      throw new Error(`Ollama 에러: ${error.message}`)
    }
  }
}
```

#### `src/agent.ts` - Agent 메인 로직

```typescript
import { OllamaClient } from './llm'
import { tools, Tool } from './tools'

export class OllamaAgent {
  private llm: OllamaClient
  private conversationHistory: Array<{role: string, content: string}> = []
  
  constructor(model: string = 'qwen2.5:7b', baseUrl: string = 'http://localhost:11434') {
    this.llm = new OllamaClient({ model, baseUrl })
  }
  
  private getToolsDescription(): string {
    return tools.map(tool => {
      const params = Object.entries(tool.parameters)
        .map(([key, value]) => `  - ${key}: ${value}`)
        .join('\n')
      
      return `${tool.name}:\n  설명: ${tool.description}\n  파라미터:\n${params}`
    }).join('\n\n')
  }
  
  async execute(userRequest: string): Promise<string> {
    console.log(`\n🤖 사용자 요청: ${userRequest}\n`)
    
    // Step 1: 도구 선택
    const planPrompt = `
당신은 AI 개발 어시스턴트입니다.

사용자 요청: "${userRequest}"

사용 가능한 도구들:
${this.getToolsDescription()}

위 요청을 수행하기 위해 어떤 도구를 사용해야 할까요?
다음 JSON 형식으로만 응답하세요 (다른 텍스트 없이):
{
  "tool": "도구명",
  "params": ["param1", "param2", ...]
}

만약 도구가 필요 없다면:
{
  "tool": "none",
  "response": "직접 답변"
}
`
    
    console.log('🔍 도구 선택 중...')
    const planResponse = await this.llm.generate(planPrompt)
    console.log('📋 계획:', planResponse)
    
    try {
      // JSON 추출 (```json ... ``` 제거)
      const jsonMatch = planResponse.match(/\{[\s\S]*\}/)
      if (!jsonMatch) {
        throw new Error('JSON 형식을 찾을 수 없습니다')
      }
      
      const plan = JSON.parse(jsonMatch[0])
      
      // Step 2: 도구 실행
      if (plan.tool === 'none') {
        return plan.response
      }
      
      const tool = tools.find(t => t.name === plan.tool)
      if (!tool) {
        return `에러: 도구 '${plan.tool}'을 찾을 수 없습니다`
      }
      
      console.log(`⚡ 도구 실행: ${tool.name}(${plan.params.join(', ')})`)
      const result = await tool.execute(...plan.params)
      console.log('✅ 실행 결과:', result.substring(0, 200) + '...')
      
      // Step 3: 결과 정리
      const summaryPrompt = `
사용자 요청: "${userRequest}"
도구 실행 결과:
${result}

위 결과를 사용자에게 친절하게 설명해주세요.
`
      
      const summary = await this.llm.generate(summaryPrompt)
      return summary
      
    } catch (error) {
      console.error('❌ 에러:', error.message)
      return `에러: ${error.message}`
    }
  }
  
  async chat(message: string): Promise<string> {
    this.conversationHistory.push({
      role: 'user',
      content: message
    })
    
    const response = await this.execute(message)
    
    this.conversationHistory.push({
      role: 'assistant',
      content: response
    })
    
    return response
  }
}
```

#### `src/index.ts` - 메인

```typescript
import { OllamaAgent } from './agent'
import * as readline from 'readline'

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
})

const agent = new OllamaAgent('qwen2.5:7b', 'http://localhost:11434')

console.log('🤖 Ollama Agent 시작!')
console.log('   Ollama가 http://localhost:11434 에서 실행 중이어야 합니다.')
console.log('   종료하려면 "exit" 입력\n')

function ask() {
  rl.question('👤 You: ', async (input) => {
    if (input.toLowerCase() === 'exit') {
      console.log('👋 종료합니다!')
      rl.close()
      return
    }
    
    try {
      const response = await agent.chat(input)
      console.log(`\n🤖 Agent: ${response}\n`)
    } catch (error) {
      console.error(`❌ 에러: ${error.message}\n`)
    }
    
    ask()
  })
}

ask()
```

### 사용 방법

```bash
# 1. Ollama 서버 시작 (PowerShell 1)
ollama serve

# 2. Agent 실행 (PowerShell 2)
cd ollama-agent
npx ts-node src/index.ts

# 3. 사용!
👤 You: README.md 파일을 읽어줘
🤖 Agent: [파일 내용 출력]

👤 You: package.json 파일을 만들어줘
🤖 Agent: [파일 생성 완료]

👤 You: git status 확인해줘
🤖 Agent: [Git 상태 출력]
```

---

## 🦜 방법 2: LangChain 사용 (추천!)

### 설치

```bash
npm install langchain @langchain/community @langchain/core
```

### 코드

```typescript
import { ChatOllama } from '@langchain/community/chat_models/ollama'
import { DynamicTool } from '@langchain/core/tools'
import { AgentExecutor, createReactAgent } from 'langchain/agents'
import { ChatPromptTemplate } from '@langchain/core/prompts'
import { readFileSync, writeFileSync } from 'fs'
import { exec } from 'child_process'
import { promisify } from 'util'

const execAsync = promisify(exec)

// LLM 설정
const llm = new ChatOllama({
  baseUrl: 'http://localhost:11434',
  model: 'qwen2.5:7b'
})

// 도구 정의
const tools = [
  new DynamicTool({
    name: 'read_file',
    description: '파일을 읽습니다. 입력: 파일 경로',
    func: async (path: string) => {
      return readFileSync(path, 'utf-8')
    }
  }),
  
  new DynamicTool({
    name: 'write_file',
    description: '파일을 작성합니다. 입력: JSON {"path": "경로", "content": "내용"}',
    func: async (input: string) => {
      const { path, content } = JSON.parse(input)
      writeFileSync(path, content, 'utf-8')
      return `파일 작성 완료: ${path}`
    }
  }),
  
  new DynamicTool({
    name: 'run_command',
    description: '쉘 명령어를 실행합니다. 입력: 명령어',
    func: async (command: string) => {
      const { stdout, stderr } = await execAsync(command)
      return stdout || stderr
    }
  })
]

// Prompt 템플릿
const prompt = ChatPromptTemplate.fromMessages([
  ['system', `당신은 AI 개발 어시스턴트입니다.
사용 가능한 도구: {tools}
도구 이름: {tool_names}

작업을 수행하기 위해 도구를 사용하세요.`],
  ['human', '{input}'],
  ['assistant', '{agent_scratchpad}']
])

// Agent 생성
const agent = await createReactAgent({
  llm,
  tools,
  prompt
})

const agentExecutor = new AgentExecutor({
  agent,
  tools,
  verbose: true
})

// 실행
const result = await agentExecutor.invoke({
  input: 'README.md 파일을 읽어줘'
})

console.log(result.output)
```

---

## 🌐 방법 3: Open WebUI 사용

### Docker 설치

```bash
docker run -d -p 3000:8080 \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  -v open-webui:/app/backend/data \
  --name open-webui \
  ghcr.io/open-webui/open-webui:main
```

### 사용

1. 브라우저: http://localhost:3000
2. 회원가입/로그인
3. 설정 → 모델 → Ollama 모델 선택 (qwen2.5:7b)
4. ChatGPT처럼 사용!

### 기능

- ✅ 파일 업로드
- ✅ 코드 실행 플러그인
- ✅ 웹 검색
- ✅ 대화 저장/불러오기

---

## 💻 방법 4: Continue.dev 사용

### 설치

1. VS Code 열기
2. Extensions → "Continue" 검색 → 설치
3. Continue 설정 열기 (Ctrl+Shift+P → "Continue: Open Config")

### 설정

```json
{
  "models": [
    {
      "title": "Qwen 2.5:7B",
      "provider": "ollama",
      "model": "qwen2.5:7b"
    }
  ],
  "tabAutocompleteModel": {
    "title": "Qwen",
    "provider": "ollama",
    "model": "qwen2.5:7b"
  }
}
```

### 사용

```typescript
// 1. 코드 작성 중
function calculateSum(a, b) {
  // 여기서 Ctrl+I
  // → "타입스크립트로 변환해줘"
  // → Continue가 자동으로 변환!
}

// 2. 파일 선택
// Ctrl+L → 파일 추가
// "이 파일을 리팩토링해줘"
// → 자동 수정!
```

---

## 🔧 방법 5: Aider 사용 (최고 추천!)

### 설치

```bash
# Python 필요
pip install aider-chat
```

### 사용

```bash
$ aider --model ollama/qwen2.5:7b

Aider v0.52.0
Main model: ollama/qwen2.5:7b with whole edit format
Git repo: .git
Repo-map: disabled
Added README.md to the chat

> README.md에 설치 가이드를 추가해줘

[Aider가 자동으로 파일 수정]
[Git 커밋 자동 생성]

> 이제 src/index.ts 파일을 만들어줘

[파일 생성 및 커밋]

> 방금 만든 파일에 Express 서버 코드를 작성해줘

[코드 작성 및 커밋]
```

### 특징

- ✅ Git 자동 통합
- ✅ 여러 파일 동시 수정
- ✅ 자동 커밋
- ✅ 터미널에서 바로 사용
- ✅ 매우 빠름

---

## 🚀 실전 프로젝트: 나만의 AI 코딩 어시스턴트

### 프로젝트 목표

Claude처럼 대화하면서 코드를 작성하는 터미널 앱 만들기!

### 최종 코드 (위 방법 1 참조)

### 실행

```bash
# Terminal 1: Ollama 서버
ollama serve

# Terminal 2: Agent 실행
cd ollama-agent
npx ts-node src/index.ts
```

### 데모

```bash
🤖 Ollama Agent 시작!

👤 You: 새 프로젝트를 만들어줘. express-app이라는 이름으로

🤖 Agent: 
   ✅ 디렉토리 생성: express-app
   ✅ package.json 생성 완료
   ✅ src/index.ts 생성 완료
   ✅ Git 초기화 완료
   
   프로젝트가 준비되었습니다!

👤 You: src/index.ts에 Express 서버 코드를 작성해줘

🤖 Agent:
   ✅ Express 서버 코드 작성 완료
   ✅ Git 커밋: "feat: Add Express server"
   
   서버가 포트 3000에서 실행되도록 설정했습니다!

👤 You: npm install을 실행해줘

🤖 Agent:
   ⚡ 명령어 실행 중...
   ✅ Express, TypeScript 설치 완료!
```

---

## 📊 성능 비교

| 방법 | 설정 시간 | 응답 속도 | 기능 | 추천도 |
|------|-----------|-----------|------|--------|
| **직접 구현** | 2-3시간 | 3-5초 | 커스텀 | ⭐⭐⭐ |
| **LangChain** | 30분 | 3-5초 | 풍부 | ⭐⭐⭐⭐⭐ |
| **Open WebUI** | 5분 | 2-3초 | 기본 | ⭐⭐⭐⭐ |
| **Continue.dev** | 5분 | 1-2초 | VS Code | ⭐⭐⭐⭐ |
| **Aider** | 2분 | 2-3초 | Git 최고 | ⭐⭐⭐⭐⭐ |

---

## 💡 최종 추천

### 초보자
→ **Open WebUI** 또는 **Continue.dev**

### 개발자
→ **Aider** (Git 통합 최고!)

### 고급 사용자
→ **LangChain** (완전 커스터마이징)

---

## 🎯 요약

```
질문: "Ollama를 AI 개발자로 만들려면?"

답변:
1. 도구 시스템을 추가하면 됩니다!
2. 가장 쉬운 방법: Aider 사용
3. 가장 강력한 방법: LangChain 사용
4. 가장 UI 좋은 방법: Open WebUI 사용

추천:
→ 지금 바로: Aider 설치 후 사용!
→ 나중에: LangChain으로 커스터마이징!
```

---

**생성일**: 2026-02-02  
**버전**: 1.0.0
