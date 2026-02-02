# Ollama 포트 충돌 해결 가이드

## 문제 상황
```
Error: listen tcp 127.0.0.1:11434: bind: Only one usage of each socket address
```

## 원인
Ollama가 **이미 실행 중**이어서 같은 포트(11434)를 두 번 열려고 시도함.

## 해결 방법

### Option 1: 기존 Ollama 프로세스 확인 (권장)

**PowerShell에서 실행:**
```powershell
# Ollama 프로세스 확인
tasklist | findstr ollama

# 기대 결과: ollama.exe가 보이면 이미 실행 중
# 예: ollama.exe    12345 Console    1     8,234,456 K
```

### Option 2: 기존 프로세스 종료 후 재시작

**PowerShell 관리자 권한으로 실행:**
```powershell
# 1. 모든 Ollama 프로세스 종료
taskkill /F /IM ollama.exe

# 2. 포트 확인 (비어있어야 함)
netstat -ano | findstr :11434

# 3. Ollama 재시작
ollama serve
```

### Option 3: 다른 포트로 실행 (비추천)

**환경변수 변경:**
```powershell
# PowerShell (현재 세션만)
$env:OLLAMA_HOST = "0.0.0.0:11435"
ollama serve

# 영구 설정
setx OLLAMA_HOST "0.0.0.0:11435"
```

**주의:** Architecture Agent의 `.dev.vars`도 같이 변경해야 함:
```bash
OLLAMA_HOST=http://localhost:11435
```

## 현재 상황 진단

**이미 실행 중인지 확인:**
```powershell
# PowerShell에서 실행
curl http://localhost:11434/api/tags
```

**성공 응답 예시:**
```json
{
  "models": [
    {
      "name": "qwen2.5:7b",
      "size": 4700000000
    }
  ]
}
```

✅ **응답이 온다면:** Ollama가 정상 실행 중! `ollama serve` 다시 실행할 필요 없음.  
❌ **에러가 난다면:** 프로세스 종료 후 재시작 필요.

## Architecture Agent 연결 테스트

**1. .dev.vars 확인:**
```bash
cat C:\A-SDV-Platform\architecture-agent-llm\.dev.vars
```

**내용:**
```bash
OLLAMA_HOST=http://localhost:11434
```

**2. Architecture Agent 재시작:**
```powershell
cd C:\A-SDV-Platform\architecture-agent-llm
npm run dev
```

**3. 브라우저 테스트:**
- URL: http://localhost:5173
- 왼쪽 하단 "LLM 연결 상태" 확인
- ✅ 초록색 "연결됨" → 성공!
- ❌ 빨간색 "연결 실패" → Ollama 상태 재확인

## 빠른 체크리스트

**지금 바로 확인:**
```powershell
# 1. Ollama 프로세스 확인
tasklist | findstr ollama

# 2. 포트 확인
netstat -ano | findstr :11434

# 3. API 테스트
curl http://localhost:11434/api/tags
```

**결과 분석:**
1. `tasklist`에서 ollama.exe 보임 → ✅ 실행 중
2. `netstat`에서 11434 포트 보임 → ✅ 리스닝 중
3. `curl` 응답 성공 → ✅ API 작동 중

→ **모두 ✅라면 추가 작업 필요 없음!**

## 다음 단계

**Ollama가 정상 작동 중이라면:**
1. Architecture Agent만 실행: `npm run dev`
2. 브라우저에서 http://localhost:5173 접속
3. 프롬프트 테스트: "마이크로서비스 아키텍처 만들어줘"

**문제가 계속되면:**
1. 위 명령어 실행 결과 공유
2. 에러 메시지 전체 복사
3. 추가 진단 진행

---

**작성일:** 2026-02-02  
**버전:** 1.0  
