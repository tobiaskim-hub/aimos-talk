# 설치 진행 로그

> **사용자**: TaehoKim  
> **시작 시간**: 2026-02-02 19:04 (KST)  
> **PC**: Windows 노트북 (AMD RYZEN AI MAX+ PRO 395, 128GB RAM)

---

## 📅 타임라인

### 2026-02-02 19:00 - GitHub 업로드 완료

#### ✅ 완료된 작업
- [x] GitHub 계정 연결: `@tobiaskim-hub`
- [x] 3개 저장소 생성 및 Push 완료
  - automotive-system-modeler
  - architecture-agent-llm
  - automotive-sdv-platform

#### 📊 업로드 통계
- **총 파일**: 31개
- **총 코드**: ~4,967 줄
- **총 커밋**: 21개

---

### 2026-02-02 19:04 - Windows 설치 시작

#### 📍 현재 위치
```powershell
C:\A-SDV-Platform>
```

#### ❌ 발생한 문제
```
npm error code ENOENT
npm error syscall open
npm error path C:\A-SDV-Platform\package.json
npm error errno -4058
npm error enoent Could not read package.json: Error: ENOENT: no such file or directory
```

#### 🔍 원인 분석
- 프로젝트를 아직 Clone 하지 않음
- `C:\A-SDV-Platform` 폴더가 비어있음
- GitHub에서 다운로드 필요

#### 📋 필요한 작업
1. Git 설치 확인
2. 프로젝트 Clone
3. 의존성 설치 (npm install)
4. 프로젝트 실행

---

### 2026-02-02 19:05 - 설치 가이드 작성

#### ✅ 생성된 문서
- [x] `/home/user/WINDOWS_INSTALLATION_GUIDE.md` (완전한 설치 가이드)
- [x] `/home/user/INSTALLATION_LOG.md` (이 파일)

---

## 🔄 진행 단계

### Phase 1: 사전 준비 ⏳
- [ ] Git 설치 확인: `git --version`
- [ ] Node.js 설치 확인: `node --version`
- [ ] PowerShell 버전 확인: `$PSVersionTable`

### Phase 2: 프로젝트 Clone ⏳
```powershell
cd C:\A-SDV-Platform
git clone https://github.com/tobiaskim-hub/automotive-system-modeler.git
git clone https://github.com/tobiaskim-hub/architecture-agent-llm.git
git clone https://github.com/tobiaskim-hub/automotive-sdv-platform.git
```

### Phase 3: Project 1 실행 ⏳
```powershell
cd automotive-system-modeler
npm install
npm run build
npm run dev
```

### Phase 4: Project 2 실행 ⏳
```powershell
cd ..\architecture-agent-llm
npm install
npm run build
npm run dev
```

### Phase 5: Ollama 설치 ⏳
```powershell
# https://ollama.com/download/windows
ollama pull qwen2.5:7b
ollama serve
```

---

## 📝 명령어 실행 기록

### 2026-02-02 19:04
```powershell
# 시도 1: 프로젝트 폴더로 이동 (실패)
C:\A-SDV-Platform> cd C:\Projects\architecture-agent-llm
# 결과: 지정된 경로를 찾을 수 없습니다.

# 시도 2: npm install (실패)
C:\A-SDV-Platform> npm install
# 결과: ENOENT: no such file or directory, open 'C:\A-SDV-Platform\package.json'

# 시도 3: npm run build (실패)
C:\A-SDV-Platform> npm run build
# 결과: ENOENT: no such file or directory, open 'C:\A-SDV-Platform\package.json'

# 시도 4: npm run dev (실패)
C:\A-SDV-Platform> npm run dev
# 결과: (동일한 에러)
```

---

## 🎯 다음 액션

### 즉시 실행할 명령어

```powershell
# 1. Git 설치 확인
git --version

# 2. 설치되어 있으면 Clone 시작
cd C:\A-SDV-Platform
git clone https://github.com/tobiaskim-hub/automotive-system-modeler.git
git clone https://github.com/tobiaskim-hub/architecture-agent-llm.git
git clone https://github.com/tobiaskim-hub/automotive-sdv-platform.git

# 3. 폴더 확인
dir

# 4. Project 2로 이동
cd architecture-agent-llm

# 5. 의존성 설치
npm install

# 6. 빌드
npm run build

# 7. 실행
npm run dev
```

---

## 📊 설치 체크리스트

### 필수 소프트웨어
- [ ] Git (v2.40+)
- [ ] Node.js (v18+)
- [ ] npm (v9+)
- [ ] Ollama (Latest)

### 프로젝트
- [ ] automotive-system-modeler (Clone)
- [ ] architecture-agent-llm (Clone)
- [ ] automotive-sdv-platform (Clone)

### 실행 확인
- [ ] Project 1 실행 (http://localhost:5173)
- [ ] Project 2 실행 (http://localhost:5173)
- [ ] Ollama 서버 실행 (localhost:11434)
- [ ] LLM 연동 테스트

---

## 🐛 발생한 에러

### Error 1: 경로를 찾을 수 없음
```
지정된 경로를 찾을 수 없습니다.
cd C:\Projects\architecture-agent-llm
```
**해결**: 프로젝트를 먼저 Clone 해야 함

---

### Error 2: package.json을 찾을 수 없음
```
npm error code ENOENT
npm error syscall open
npm error path C:\A-SDV-Platform\package.json
npm error errno -4058
npm error enoent Could not read package.json
```
**해결**: 프로젝트 폴더로 이동 후 실행

---

### Error 3: PowerShell 주석 실행
```
'#'은(는) 내부 또는 외부 명령, 실행할 수 있는 프로그램, 또는
배치 파일이 아닙니다.
```
**해결**: `#`로 시작하는 주석은 건너뛰고 실제 명령어만 실행

---

## 💡 학습 내용

### PowerShell 사용법
- `#`는 주석이므로 실행하지 말 것
- 명령어만 복사해서 실행
- `cd` 명령어로 폴더 이동

### Git 사용법
- `git clone <URL>`: GitHub에서 프로젝트 다운로드
- `git --version`: Git 설치 확인

### npm 사용법
- `npm install`: 의존성 설치 (package.json 필요)
- `npm run build`: 프로젝트 빌드
- `npm run dev`: 개발 서버 실행

---

## 📂 예상 폴더 구조

Clone 완료 후:
```
C:\A-SDV-Platform\
├── automotive-system-modeler\
│   ├── node_modules\
│   ├── src\
│   ├── public\
│   ├── package.json
│   └── README.md
├── architecture-agent-llm\
│   ├── node_modules\
│   ├── src\
│   ├── public\
│   ├── package.json
│   └── README.md
└── automotive-sdv-platform\
    ├── INDEX.md
    ├── PROJECT-1-*.md
    ├── PROJECT-2-*.md
    └── PROJECT-3-*.md
```

---

## 🔗 참고 링크

- **설치 가이드**: `/home/user/WINDOWS_INSTALLATION_GUIDE.md`
- **GitHub 저장소**:
  - https://github.com/tobiaskim-hub/automotive-system-modeler
  - https://github.com/tobiaskim-hub/architecture-agent-llm
  - https://github.com/tobiaskim-hub/automotive-sdv-platform

---

## 📞 다음 단계

1. **Git 설치 확인**: PowerShell에서 `git --version` 실행
2. **결과 보고**: 설치되어 있는지 확인
3. **Clone 시작**: Git이 있으면 프로젝트 다운로드
4. **실행 테스트**: 프로젝트가 정상 작동하는지 확인

---

**로그 시작**: 2026-02-02 19:04  
**마지막 업데이트**: 2026-02-02 19:10  
**상태**: 🔄 진행 중 (Git 설치 확인 대기)
