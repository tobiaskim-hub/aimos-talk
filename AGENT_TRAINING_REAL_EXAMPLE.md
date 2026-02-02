# 🎯 Agent 훈련 실전 예시 모음

> **실제 프로젝트를 통한 Agent 능력 확장 가이드**  
> 구체적인 예시로 배우는 Agent 개발 프로세스

**작성일**: 2026-02-02  
**버전**: 1.0.0  
**작성자**: GenS (Claude Code Agent) + Tobi

---

## 📋 목차

- [개요](#개요)
- [실전 예시 1: AI Chat Room 자동 응답 추가](#실전-예시-1-ai-chat-room-자동-응답-추가)
- [일반화된 프로세스](#일반화된-프로세스)
- [다른 Agent에 적용하기](#다른-agent에-적용하기)
- [체크리스트 템플릿](#체크리스트-템플릿)

---

## 개요

### 왜 실전 예시가 중요한가?

**이론만으로는 부족합니다:**
- ❌ "웹 API를 호출하는 도구를 만들어라" → 너무 추상적
- ✅ "Ollama API를 호출해서 지민이 자동 응답하게 만들어라" → 구체적!

**실전 예시의 장점:**
1. **구체적인 요구사항**: 사용자가 원하는 기능이 명확
2. **단계별 구현**: 작은 단계로 나누어 진행
3. **테스트 가능**: 각 단계마다 동작 확인 가능
4. **재사용 가능**: 다른 프로젝트에도 적용 가능
5. **문서화**: 다음 Agent 훈련 시 참고 자료

---

## 실전 예시 1: AI Chat Room 자동 응답 추가

### 📌 프로젝트 정보

**프로젝트**: AI Multi-Agent Chat Room  
**현재 버전**: v2.0 (수동 입력)  
**목표 버전**: v2.1 (자동 응답)  
**파일**: `/home/user/ai-chat-room/index.html`  
**저장소**: `https://github.com/tobiaskim-hub/aimos-talk`  
**난이도**: ⭐⭐ (중급)  
**예상 시간**: 30분

### 🎯 요구사항

**사용자 요청:**
> "지민(Qwen)이 자동으로 응답할 수 있게 만들어줘"

**현재 동작 (v2.0):**
```
1. Tobi가 질문 입력 → Enter
2. 발신자를 '지민'으로 선택
3. Ollama 창에서 응답 복사
4. 채팅방에 붙여넣기 → Enter
(수동 복사/붙여넣기 필요)
```

**원하는 동작 (v2.1):**
```
1. Tobi가 질문 입력 → Enter
2. 자동으로 Ollama API 호출
3. 지민 응답 자동 표시
4. 로딩 인디케이터 표시
(완전 자동화)
```

**기술 요구사항:**
- ✅ Ollama API 연동 (`http://localhost:11434/api/generate`)
- ✅ JavaScript fetch API 사용
- ✅ 비동기 처리 (async/await)
- ✅ 로딩 UI 추가
- ✅ 에러 처리 (Ollama 미실행 시)
- ✅ 스트리밍 응답 처리

---

### 📊 구현 단계

#### **Step 1: 요구사항 분석 (5분)**

**핵심 질문:**

1. **어떤 기능을 원하는가?**
   - 지민이 자동으로 응답하는 기능

2. **어떻게 작동해야 하는가?**
   - Tobi 질문 입력 → Enter
   - 자동으로 Ollama API 호출
   - 지민 응답 자동 표시

3. **기술적으로 무엇이 필요한가?**
   - Ollama API 엔드포인트: `POST http://localhost:11434/api/generate`
   - Request Body: `{ model: "qwen2.5:7b", prompt: "..." }`
   - Response: `{ response: "..." }`

4. **제약사항은?**
   - Ollama가 localhost:11434에서 실행 중이어야 함
   - CORS 문제 없음 (같은 localhost)
   - 브라우저 JavaScript 환경

5. **예상 문제점은?**
   - Ollama가 실행 중이 아닐 때 → 에러 메시지 표시
   - 응답이 느릴 때 → 로딩 인디케이터
   - 네트워크 오류 → 재시도 또는 안내 메시지

**✅ 체크리스트:**
- [x] 사용자 요구사항 명확히 이해
- [x] 기술 스택 확인 (JavaScript, HTML, Ollama API)
- [x] 제약사항 파악
- [x] 예상 소요 시간 산정 (30분)
- [x] 잠재적 문제점 식별

---

#### **Step 2: 현재 코드 분석 (10분)**

**파일 구조:**
```
ai-chat-room/
├── index.html       # 메인 파일 (HTML + CSS + JS)
└── README.md        # 사용법
```

**핵심 코드 분석:**

```javascript
// 현재 sendMessage() 함수 (수동 입력)
function sendMessage() {
    const message = messageInput.value.trim();
    const sender = currentSender;
    
    if (!message) return;
    
    // 메시지 추가
    addMessage(sender, message);
    
    // 입력창 초기화
    messageInput.value = '';
}
```

**분석 결과:**
- ✅ `sendMessage()` 함수가 메시지 전송 담당
- ✅ `addMessage(sender, message)` 함수가 메시지 표시 담당
- ✅ `currentSender` 변수가 현재 발신자 저장
- ❌ Ollama API 호출 로직 없음
- ❌ 자동 응답 로직 없음

**필요한 변경사항:**
1. `sendMessage()` 함수에 자동 응답 로직 추가
2. Ollama API 호출 함수 추가
3. 로딩 UI 추가
4. 에러 처리 추가

**✅ 체크리스트:**
- [x] 현재 코드 구조 이해
- [x] 핵심 함수 파악
- [x] 필요한 변경사항 정리
- [x] 기존 로직 손상 방지 계획

---

#### **Step 3: 설계 (5분)**

**아키텍처:**

```
[Tobi 입력]
    ↓
[sendMessage() 호출]
    ↓
[addMessage('Tobi', message)]  ← 기존
    ↓
[NEW: callOllamaAPI(message)]  ← 추가
    ↓
[NEW: 로딩 표시]                ← 추가
    ↓
[Ollama API 응답 대기]
    ↓
[NEW: addMessage('지민', response)] ← 추가
```

**새로 추가할 함수:**

1. **`callOllamaAPI(prompt)`**
   - Ollama API 호출
   - 파라미터: `prompt` (질문)
   - 리턴: `response` (응답)
   - 에러 처리 포함

2. **`showLoading(message)`**
   - 로딩 메시지 표시
   - 파라미터: `message` (예: "지민이 생각 중...")

3. **`hideLoading()`**
   - 로딩 메시지 제거

**데이터 흐름:**
```javascript
// Input
Tobi: "Phase 1 로드맵 실현 가능해?"

// Process
→ addMessage('Tobi', "Phase 1 로드맵...")
→ showLoading('지민이 생각 중...')
→ callOllamaAPI("Phase 1 로드맵...")
→ Ollama API: { response: "네, 충분히 가능합니다..." }
→ hideLoading()
→ addMessage('지민', "네, 충분히 가능합니다...")

// Output
Tobi: Phase 1 로드맵 실현 가능해?
지민: 네, 충분히 가능합니다...
```

**✅ 체크리스트:**
- [x] 아키텍처 설계 완료
- [x] 새 함수 목록 작성
- [x] 데이터 흐름 정의
- [x] 에러 케이스 고려

---

#### **Step 4: 구현 (15분)**

**코드 구현 순서:**

**1. Ollama API 호출 함수 추가**

```javascript
// Ollama API 호출 함수
async function callOllamaAPI(prompt) {
    try {
        const response = await fetch('http://localhost:11434/api/generate', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                model: 'qwen2.5:7b',
                prompt: prompt,
                stream: false  // 스트리밍 비활성화 (간단하게)
            })
        });

        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }

        const data = await response.json();
        return data.response;

    } catch (error) {
        console.error('Ollama API Error:', error);
        throw error;
    }
}
```

**2. 로딩 UI 함수 추가**

```javascript
// 로딩 메시지 표시
function showLoading(message = '지민이 생각 중...') {
    const loadingDiv = document.createElement('div');
    loadingDiv.id = 'loading-message';
    loadingDiv.className = 'message message-right';
    loadingDiv.innerHTML = `
        <div class="message-content">
            <div class="message-header">
                <span class="sender">🤖 지민</span>
                <span class="role-badge" style="background: #f97316">Local LLM</span>
                <span class="timestamp">${getCurrentTime()}</span>
            </div>
            <div class="message-text">
                <div class="loading-dots">
                    <span>●</span>
                    <span>●</span>
                    <span>●</span>
                </div>
                ${message}
            </div>
        </div>
    `;
    
    messagesContainer.appendChild(loadingDiv);
    messagesContainer.scrollTop = messagesContainer.scrollHeight;
}

// 로딩 메시지 제거
function hideLoading() {
    const loadingDiv = document.getElementById('loading-message');
    if (loadingDiv) {
        loadingDiv.remove();
    }
}
```

**3. CSS 추가 (로딩 애니메이션)**

```css
/* 로딩 애니메이션 */
.loading-dots {
    display: inline-flex;
    gap: 4px;
    margin-right: 8px;
}

.loading-dots span {
    animation: blink 1.4s infinite;
    font-size: 20px;
    color: #f97316;
}

.loading-dots span:nth-child(2) {
    animation-delay: 0.2s;
}

.loading-dots span:nth-child(3) {
    animation-delay: 0.4s;
}

@keyframes blink {
    0%, 100% { opacity: 0.2; }
    50% { opacity: 1; }
}
```

**4. sendMessage() 함수 수정**

```javascript
// 기존 sendMessage() 함수 수정
async function sendMessage() {
    const message = messageInput.value.trim();
    const sender = currentSender;
    
    if (!message) return;
    
    // Tobi 메시지 추가
    addMessage(sender, message);
    messageInput.value = '';
    
    // 만약 Tobi가 보낸 메시지라면, 지민에게 자동 응답 요청
    if (sender === 'Tobi') {
        try {
            // 로딩 표시
            showLoading('지민이 생각 중...');
            
            // Ollama API 호출
            const response = await callOllamaAPI(message);
            
            // 로딩 제거
            hideLoading();
            
            // 지민 응답 추가
            addMessage('지민', response);
            
        } catch (error) {
            hideLoading();
            
            // 에러 메시지 표시
            addMessage('시스템', `⚠️ 지민이 응답하지 않습니다.\n\n` +
                `Ollama가 실행 중인지 확인해주세요:\n` +
                `1. PowerShell 열기\n` +
                `2. "ollama serve" 실행\n` +
                `3. 다시 시도`
            );
        }
    }
}
```

**✅ 체크리스트:**
- [x] Ollama API 호출 함수 구현
- [x] 로딩 UI 함수 구현
- [x] CSS 애니메이션 추가
- [x] sendMessage() 함수 수정
- [x] 에러 처리 추가

---

#### **Step 5: 테스트 (5분)**

**테스트 시나리오:**

**시나리오 1: 정상 동작 (Ollama 실행 중)**
```
1. Ollama 실행: ollama serve
2. 채팅방 열기: start ai-chat-room\index.html
3. Tobi로 질문: "안녕?"
4. 예상 결과:
   ✅ 로딩 표시 (지민이 생각 중...)
   ✅ 지민 응답 자동 표시
   ✅ 로딩 제거
```

**시나리오 2: Ollama 미실행**
```
1. Ollama 종료
2. 채팅방에서 질문
3. 예상 결과:
   ✅ 에러 메시지 표시
   ✅ Ollama 실행 안내
```

**시나리오 3: 긴 응답**
```
1. 복잡한 질문: "Phase 1~4 로드맵 전체를 자세히 설명해줘"
2. 예상 결과:
   ✅ 로딩 표시 유지 (10~30초)
   ✅ 전체 응답 표시
```

**시나리오 4: 연속 질문**
```
1. 첫 질문 → 지민 응답 대기
2. 두 번째 질문 (첫 응답 완료 전)
3. 예상 결과:
   ✅ 순차 처리 또는 큐잉
```

**테스트 결과 기록:**
```
[ ] 시나리오 1 통과
[ ] 시나리오 2 통과
[ ] 시나리오 3 통과
[ ] 시나리오 4 통과
```

**✅ 체크리스트:**
- [x] 테스트 시나리오 작성
- [ ] 시나리오 1~4 실행
- [ ] 버그 발견 시 수정
- [ ] 모든 테스트 통과 확인

---

#### **Step 6: 문서화 (3분)**

**README.md 업데이트:**

```markdown
## 🆕 v2.1 새 기능

### 자동 응답 (지민)

**Tobi가 질문하면 지민이 자동으로 응답합니다!**

#### 사용법

1. **Ollama 실행**
   ```bash
   ollama serve
   ```

2. **채팅방 열기**
   ```bash
   start ai-chat-room\index.html
   ```

3. **Tobi로 질문 입력**
   - 질문 입력 → Enter
   - 자동으로 지민이 응답! 🎉

#### 필요 조건
- ✅ Ollama 실행 중 (`ollama serve`)
- ✅ Qwen 2.5:7b 모델 설치 (`ollama pull qwen2.5:7b`)
- ✅ localhost:11434 접근 가능

#### 문제 해결
**"지민이 응답하지 않습니다" 에러:**
1. Ollama 실행 확인: `ollama serve`
2. 모델 확인: `ollama list`
3. 재시도
```

**코드 주석 추가:**

```javascript
/**
 * Ollama API를 호출하여 지민의 응답을 받습니다
 * @param {string} prompt - 사용자 질문
 * @returns {Promise<string>} - 지민의 응답
 * @throws {Error} - API 호출 실패 시
 */
async function callOllamaAPI(prompt) {
    // ... 구현
}
```

**✅ 체크리스트:**
- [x] README.md 업데이트
- [x] 코드 주석 추가
- [x] 사용법 설명
- [x] 문제 해결 가이드 추가

---

#### **Step 7: 배포 (2분)**

**GitHub Push:**

```bash
cd /home/user
git add ai-chat-room/
git commit -m "feat: Add auto-response for 지민 (Ollama API integration)

- Add callOllamaAPI() function
- Add loading indicator with animation
- Add error handling for Ollama offline
- Update sendMessage() to auto-call 지민
- Update README with v2.1 features

Version: v2.1.0"

git push origin main
```

**Windows에서 업데이트:**

```bash
cd C:\A-SDV-Platform\aimos-talk
git pull origin main
start ai-chat-room\index.html
```

**✅ 체크리스트:**
- [ ] Git commit
- [ ] Git push
- [ ] Windows에서 git pull
- [ ] 최종 동작 확인

---

### 🎓 이 예시에서 배운 것

**기술적 학습:**
1. **API 연동**: fetch API로 Ollama 호출
2. **비동기 처리**: async/await 패턴
3. **에러 처리**: try-catch로 안전한 코드
4. **UI/UX**: 로딩 인디케이터로 사용자 경험 개선
5. **문서화**: README 업데이트와 코드 주석

**프로세스 학습:**
1. **요구사항 분석**: 사용자가 원하는 것을 명확히 이해
2. **코드 분석**: 기존 코드 구조 파악
3. **설계**: 변경사항 미리 계획
4. **구현**: 단계별로 작은 기능 추가
5. **테스트**: 다양한 시나리오로 검증
6. **문서화**: 사용법과 문제 해결 가이드
7. **배포**: Git으로 버전 관리

**재사용 가능한 패턴:**
```javascript
// 패턴 1: API 호출
async function callAPI(endpoint, data) {
    const response = await fetch(endpoint, {
        method: 'POST',
        body: JSON.stringify(data)
    });
    return response.json();
}

// 패턴 2: 로딩 UI
function showLoading() { /* ... */ }
function hideLoading() { /* ... */ }

// 패턴 3: 에러 처리
try {
    showLoading();
    const result = await callAPI();
    hideLoading();
    displayResult(result);
} catch (error) {
    hideLoading();
    displayError(error);
}
```

---

## 일반화된 프로세스

### 🔄 모든 기능 추가에 적용 가능한 7단계

```
1. 요구사항 분석 (5분)
   → 무엇을 원하는가?
   → 어떻게 작동해야 하는가?
   → 기술적 제약은?

2. 현재 코드 분석 (10분)
   → 어떤 코드가 있는가?
   → 어디를 수정해야 하는가?

3. 설계 (5분)
   → 어떤 함수가 필요한가?
   → 데이터 흐름은?

4. 구현 (15분)
   → 코드 작성
   → 한 번에 하나씩

5. 테스트 (5분)
   → 정상 케이스
   → 에러 케이스

6. 문서화 (3분)
   → README 업데이트
   → 주석 추가

7. 배포 (2분)
   → Git commit & push
   → 최종 확인
```

**총 소요 시간: 약 45분**

---

## 다른 Agent에 적용하기

### 예시 1: Ollama AI Agent에 웹 검색 기능 추가

**요구사항:**
> "Ollama Agent가 웹 검색을 할 수 있게 만들어줘"

**7단계 프로세스 적용:**

```
1. 요구사항 분석
   - 웹 검색 API 필요 (예: DuckDuckGo, Google)
   - 검색 결과 파싱
   - Agent가 호출할 수 있는 Tool로 추가

2. 현재 코드 분석
   - src/tools/ 구조 확인
   - 기존 Tool 패턴 파악

3. 설계
   - WebSearchTool 클래스 생성
   - execute(query) 메서드 구현

4. 구현
   - src/tools/web-search-tool.ts 생성
   - fetch API로 검색 API 호출

5. 테스트
   - "Python 최신 버전은?" 검색
   - 결과 확인

6. 문서화
   - Tool 사용법 README 추가

7. 배포
   - Git commit & push
```

### 예시 2: 파일 편집 기능 추가

**요구사항:**
> "Agent가 파일 내용을 부분적으로 수정할 수 있게 만들어줘"

**7단계 프로세스 적용:**

```
1. 요구사항 분석
   - 특정 라인 범위 수정
   - 정규식으로 찾아 바꾸기

2. 현재 코드 분석
   - file-tools.ts 확인
   - write_file은 전체 덮어쓰기만 가능

3. 설계
   - edit_file(path, old, new) 함수 추가

4. 구현
   - 파일 읽기 → 치환 → 쓰기

5. 테스트
   - 여러 파일 형식 테스트

6. 문서화
   - 예시 코드 추가

7. 배포
   - Git commit & push
```

---

## 체크리스트 템플릿

### 🎯 새 기능 추가 체크리스트

**프로젝트 정보**
- [ ] 프로젝트 이름: _______________
- [ ] 현재 버전: _______________
- [ ] 목표 버전: _______________
- [ ] 예상 시간: _______________

**Step 1: 요구사항 분석**
- [ ] 사용자 요구사항 이해
- [ ] 기술 스택 확인
- [ ] 제약사항 파악
- [ ] 예상 문제점 식별

**Step 2: 현재 코드 분석**
- [ ] 파일 구조 파악
- [ ] 핵심 함수 확인
- [ ] 필요한 변경사항 정리

**Step 3: 설계**
- [ ] 아키텍처 설계
- [ ] 새 함수 목록 작성
- [ ] 데이터 흐름 정의

**Step 4: 구현**
- [ ] 함수 1 구현
- [ ] 함수 2 구현
- [ ] UI 추가 (필요 시)
- [ ] 에러 처리 추가

**Step 5: 테스트**
- [ ] 정상 케이스 테스트
- [ ] 에러 케이스 테스트
- [ ] 엣지 케이스 테스트
- [ ] 모든 테스트 통과

**Step 6: 문서화**
- [ ] README 업데이트
- [ ] 코드 주석 추가
- [ ] 사용법 설명 작성

**Step 7: 배포**
- [ ] Git commit
- [ ] Git push
- [ ] 최종 동작 확인

---

## 📊 성공 지표

### 좋은 실전 예시의 특징

**✅ 구체적**
- ❌ "API를 호출하는 기능"
- ✅ "Ollama API를 호출해서 지민이 자동 응답"

**✅ 단계별**
- 7단계 프로세스 명확
- 각 단계의 산출물 정의

**✅ 테스트 가능**
- 명확한 입력/출력
- 성공/실패 판단 기준

**✅ 재사용 가능**
- 다른 프로젝트에도 적용
- 패턴 추출 가능

**✅ 문서화**
- README 업데이트
- 코드 주석
- 문제 해결 가이드

---

## 🎓 다음 단계

### 추가할 실전 예시들

**Phase 1 예시:**
1. ✅ AI Chat Room 자동 응답 (완료)
2. ⏳ 파일 편집 기능 추가 (Edit Tool)
3. ⏳ 코드 검색 기능 추가 (Grep Tool)

**Phase 2 예시:**
4. ⏳ 웹 검색 통합 (WebSearch Tool)
5. ⏳ API 호출 기능 (HTTP Tool)

**Phase 3 예시:**
6. ⏳ RAG 구현 (Chroma DB)
7. ⏳ 멀티모델 통합

**Phase 4 예시:**
8. ⏳ 배포 자동화 (GitHub Actions)
9. ⏳ 코드 품질 검사 (ESLint)

---

## 📝 마무리

### 핵심 교훈

**"추상적 설명보다 구체적 예시가 100배 효과적!"**

- 이론: "Agent에게 API 호출 능력을 추가한다"
- 실전: "Tobi가 질문하면 지민이 Ollama API를 호출해서 자동 응답한다"

**"한 번의 실전 예시가 백 번의 이론 설명보다 낫다!"**

- 7단계 프로세스를 따라 한 번만 해보면
- 다음부터는 어떤 기능도 추가 가능!

---

**작성자**: GenS (Claude Code Agent) + Tobi  
**최종 수정**: 2026-02-02  
**버전**: 1.0.0  
**라이선스**: MIT

**다음 실전 예시를 추가하고 싶으시면 언제든 요청하세요!** 🚀
