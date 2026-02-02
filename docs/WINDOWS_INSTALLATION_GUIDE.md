# Windows PC 설치 가이드

> **대상**: Windows 노트북 사용자  
> **사용자**: TaehoKim  
> **PC 사양**: AMD RYZEN AI MAX+ PRO 395, 128GB RAM  
> **작성일**: 2026-02-02  
> **상태**: 🔄 진행 중

---

## 📋 Table of Contents

- [개요](#개요)
- [시스템 요구사항](#시스템-요구사항)
- [설치 순서](#설치-순서)
- [Step 1: Git 설치](#step-1-git-설치)
- [Step 2: Node.js 설치](#step-2-nodejs-설치)
- [Step 3: 프로젝트 Clone](#step-3-프로젝트-clone)
- [Step 4: 프로젝트 실행](#step-4-프로젝트-실행)
- [Step 5: Ollama 설치](#step-5-ollama-설치)
- [트러블슈팅](#트러블슈팅)
- [진행 상황](#진행-상황)

---

## 개요

이 문서는 Windows PC에 Automotive SDV 프로젝트를 설치하고 실행하는 전체 과정을 기록합니다.

### 목표
- ✅ GitHub에서 3개 프로젝트 다운로드
- ✅ 로컬 개발 환경 구축
- ✅ Ollama LLM 설치 및 연동
- ✅ 프로젝트 실행 및 테스트

---

## 시스템 요구사항

### 하드웨어
- **CPU**: AMD RYZEN AI MAX+ PRO 395 (✅ 충분)
- **RAM**: 128GB (✅ 완벽)
- **저장공간**: 최소 20GB 여유 공간

### 소프트웨어
- **OS**: Windows 10/11
- **Git**: v2.40+
- **Node.js**: v18+ (LTS 권장)
- **npm**: v9+
- **Ollama**: Latest

---

## 설치 순서

```
1. Git 설치 (버전 관리)
   ↓
2. Node.js 설치 (JavaScript 런타임)
   ↓
3. 프로젝트 Clone (GitHub → 로컬)
   ↓
4. 프로젝트 실행 (개발 서버)
   ↓
5. Ollama 설치 (LLM 연동)
```

---

## Step 1: Git 설치

### 확인
```powershell
git --version
```

**예상 출력**: `git version 2.43.0.windows.1` (또는 유사)

---

### 설치 방법

#### Option A: Winget (Windows 11 권장)
```powershell
winget install Git.Git
```

#### Option B: 직접 다운로드
1. https://git-scm.com/download/win
2. 다운로드 및 실행
3. 기본 설정으로 설치 (다음, 다음, 다음...)

---

### 설치 확인
```powershell
# PowerShell 재시작 후
git --version
```

---

## Step 2: Node.js 설치

### 확인
```powershell
node --version
npm --version
```

**예상 출력**:
```
v20.11.0
10.2.4
```

---

### 설치 방법

#### Option A: Winget (권장)
```powershell
winget install OpenJS.NodeJS.LTS
```

#### Option B: 직접 다운로드
1. https://nodejs.org/
2. **LTS 버전** 다운로드 (왼쪽 버튼)
3. 설치 실행 (기본 설정)

---

### 설치 확인
```powershell
# PowerShell 재시작 후
node --version
npm --version
```

---

## Step 3: 프로젝트 Clone

### 작업 디렉토리 생성 및 이동
```powershell
# 이미 존재하는 경우
cd C:\A-SDV-Platform
```

---

### GitHub에서 프로젝트 Clone

#### 3개 프로젝트 모두 다운로드
```powershell
cd C:\A-SDV-Platform

# Project 1: Automotive System Modeler
git clone https://github.com/tobiaskim-hub/automotive-system-modeler.git

# Project 2: Architecture Agent (LLM)
git clone https://github.com/tobiaskim-hub/architecture-agent-llm.git

# Project 3: 전체 문서
git clone https://github.com/tobiaskim-hub/automotive-sdv-platform.git
```

---

### Clone 확인
```powershell
dir
```

**예상 출력**:
```
디렉터리: C:\A-SDV-Platform

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2026-02-02   오전 10:00              automotive-system-modeler
d-----         2026-02-02   오전 10:01              architecture-agent-llm
d-----         2026-02-02   오전 10:02              automotive-sdv-platform
```

---

## Step 4: 프로젝트 실행

### Project 1: Automotive System Modeler

```powershell
# 폴더 이동
cd C:\A-SDV-Platform\automotive-system-modeler

# 의존성 설치 (처음 1회만)
npm install

# 빌드
npm run build

# 개발 서버 실행
npm run dev
```

**접속**: http://localhost:5173 (Vite 기본 포트)

---

### Project 2: Architecture Agent (LLM)

```powershell
# 폴더 이동
cd C:\A-SDV-Platform\architecture-agent-llm

# 의존성 설치 (처음 1회만)
npm install

# 빌드
npm run build

# 개발 서버 실행
npm run dev
```

**접속**: http://localhost:5173

---

### 환경변수 설정 (.dev.vars)

Architecture Agent는 Ollama 연결이 필요해:

```powershell
cd C:\A-SDV-Platform\architecture-agent-llm

# .dev.vars 파일 생성
echo OLLAMA_HOST=http://localhost:11434 > .dev.vars
```

---

## Step 5: Ollama 설치

### 다운로드 및 설치

1. **다운로드**: https://ollama.com/download/windows
2. **설치 실행** (기본 설정)
3. **PowerShell 재시작**

---

### 모델 다운로드

```powershell
# Qwen 2.5:7B 다운로드 (약 5GB, 5-10분 소요)
ollama pull qwen2.5:7b
```

**진행 상황**:
```
pulling manifest
pulling 8934d96d3f08... 100% ▕████████████████▏ 4.7 GB
pulling 8c17c2ebb0ea... 100% ▕████████████████▏ 7.0 KB
pulling 7c23fb36d801... 100% ▕████████████████▏ 4.8 KB
pulling 2e0493f67d0c... 100% ▕████████████████▏   59 B
pulling fa8235e5b48f... 100% ▕████████████████▏  485 B
verifying sha256 digest
writing manifest
success
```

---

### Ollama 서버 시작

```powershell
# 네트워크 노출 설정
setx OLLAMA_HOST "0.0.0.0:11434"

# PowerShell 재시작 후
ollama serve
```

**예상 출력**:
```
Listening on 0.0.0.0:11434
```

---

### 테스트

```powershell
# 새 PowerShell 창에서
ollama run qwen2.5:7b "안녕하세요! 마이크로서비스 아키텍처를 설명해주세요"
```

---

## 트러블슈팅

### 문제 1: `git: command not found`

**원인**: Git이 설치되지 않았거나 PATH에 없음

**해결**:
```powershell
# Git 설치
winget install Git.Git

# PowerShell 재시작
exit
```

---

### 문제 2: `npm: command not found`

**원인**: Node.js가 설치되지 않았거나 PATH에 없음

**해결**:
```powershell
# Node.js 설치
winget install OpenJS.NodeJS.LTS

# PowerShell 재시작
exit
```

---

### 문제 3: `package.json not found`

**원인**: 프로젝트 폴더로 이동하지 않음

**해결**:
```powershell
# 올바른 프로젝트 폴더로 이동
cd C:\A-SDV-Platform\architecture-agent-llm

# 현재 위치 확인
pwd

# package.json 확인
dir package.json
```

---

### 문제 4: `npm install` 실패

**원인**: 네트워크 문제 또는 권한 문제

**해결**:
```powershell
# npm 캐시 정리
npm cache clean --force

# 재시도
npm install
```

---

### 문제 5: `npm run dev` 포트 충돌

**원인**: 포트 5173이 이미 사용 중

**해결**:
```powershell
# 사용 중인 프로세스 확인
netstat -ano | findstr :5173

# 프로세스 종료 (PID 확인 후)
taskkill /PID <PID번호> /F

# 또는 다른 포트 사용
# package.json에서 포트 변경
```

---

### 문제 6: Ollama 연결 실패

**원인**: Ollama 서버가 실행되지 않음

**해결**:
```powershell
# Ollama 서버 확인
curl http://localhost:11434/api/health

# 서버가 없으면 시작
ollama serve
```

---

## 진행 상황

### 2026-02-02

#### ✅ 완료
- [x] GitHub 저장소 3개 생성
- [x] 샌드박스에서 GitHub로 Push
- [x] GitHub URL 확인

#### 🔄 진행 중
- [ ] Git 설치 확인
- [ ] Node.js 설치 확인
- [ ] 프로젝트 Clone
- [ ] 프로젝트 실행

#### ⏳ 대기 중
- [ ] Ollama 설치
- [ ] LLM 연동 테스트
- [ ] 전체 시스템 테스트

---

## 빠른 참조

### GitHub 저장소

| 프로젝트 | URL |
|---------|-----|
| **Automotive Modeler** | https://github.com/tobiaskim-hub/automotive-system-modeler |
| **Architecture Agent** | https://github.com/tobiaskim-hub/architecture-agent-llm |
| **문서 및 전략** | https://github.com/tobiaskim-hub/automotive-sdv-platform |

---

### 로컬 경로

```
C:\A-SDV-Platform\
├── automotive-system-modeler\    # Project 1
├── architecture-agent-llm\       # Project 2
└── automotive-sdv-platform\      # 문서
```

---

### 주요 명령어

```powershell
# 프로젝트 Clone
git clone <URL>

# 의존성 설치
npm install

# 빌드
npm run build

# 개발 서버
npm run dev

# Ollama 모델 다운로드
ollama pull qwen2.5:7b

# Ollama 서버
ollama serve
```

---

## 다음 단계

1. **Git 설치 확인**: `git --version`
2. **Node.js 설치 확인**: `node --version`
3. **프로젝트 Clone**: 위의 명령어 실행
4. **프로젝트 실행**: `npm install` → `npm run dev`
5. **Ollama 설치**: https://ollama.com/download/windows

---

**작성자**: AI Assistant  
**최종 업데이트**: 2026-02-02  
**다음 업데이트**: 설치 완료 후
