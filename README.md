
# 🚀 GitHub Actions 정리 문서

## 📌 GitHub Actions란?
GitHub Actions는 **GitHub에서 제공하는 CI/CD(지속적 통합/지속적 배포) 자동화 도구**입니다.  
레포지토리에 `.github/workflows/` 폴더를 만들고 `yaml` 파일을 정의하면,  
코드 푸시(push), PR 생성, 이슈 등록, 스케줄 실행 등 특정 이벤트가 발생할 때  
지정한 작업(빌드, 테스트, 배포 등)이 자동으로 실행됩니다.

---

## ✅ GitHub Actions 핵심 개념
- **Workflow**: 자동화 전체 프로세스를 정의한 단위 (`.yml` 파일)
- **Job**: Workflow 안에서 실행되는 작업 단위 (병렬/순차 실행 가능)
- **Step**: Job을 구성하는 세부 단계 (셸 명령어, Action 실행)
- **Action**: 재사용 가능한 작업 모듈 (Marketplace에서 사용 가능)

---

## 📂 예시: Node.js 프로젝트 CI
```yaml
name: Node.js CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```
👉 `main` 브랜치에 push 또는 PR이 생성되면 자동으로 **설치 → 빌드 → 테스트**가 실행됩니다.

---

## 🚀 GitHub Actions을 사용하는 이유
- 반복적인 코드를 자동으로 관리하기 위해  
- 사람이 직접 할 필요 없는 작업을 자동화:
  1. **자동 포맷팅 + 커밋**
  2. **자동 코드 스타일 검사**
  3. **자동 의존성 업데이트**
  4. **자동 테스트 및 빌드**
  5. **자동 배포**

### 📌 예시: 코드 자동 포맷팅 & 커밋
```yaml
- name: Format code
  run: npx prettier --write .

- name: Commit changes
  uses: stefanzweifel/git-auto-commit-action@v5
  with:
    commit_message: "chore: auto format code"
```

---

## ⚙️ 다양한 스택별 예시
각 언어/프레임워크마다 설정은 다르지만, **공통 흐름**은 같다.

### ☕ Java (Maven)
```yaml
- uses: actions/setup-java@v4
  with:
    distribution: temurin
    java-version: 17
    cache: maven
- run: mvn -B clean verify
```

### 🌱 Spring Boot (Gradle)
```yaml
- uses: actions/setup-java@v4
  with:
    distribution: temurin
    java-version: 17
    cache: gradle
- run: ./gradlew clean build
```

### ⚛️ React
```yaml
- uses: actions/setup-node@v4
  with:
    node-version: 18
    cache: npm
- run: npm ci
- run: npm test
- run: npm run build
```

### 🟩 Node.js (Express)
```yaml
- uses: actions/setup-node@v4
  with:
    node-version: 18
- run: npm ci
- run: npm test
```

### ⚡ Vite
```yaml
- uses: actions/setup-node@v4
  with:
    node-version: 18
- run: npm ci
- run: npm run build
```

---

## 📝 YAML 개념 정리

### 1. YAML 이란?
- **YAML = “YAML Ain’t Markup Language”**
- 주로 **설정(configuration) 파일**을 작성할 때 쓰이는 **데이터 직렬화 포맷**
- 사람이 읽기 쉽고, JSON보다 간단하게 표현 가능

#### 📌 JSON vs YAML
```json
{
  "name": "이름",
  "age": 30,
  "skills": ["Java", "Spring", "SQL"]
}
```

```yaml
name: 이름
age: 30
skills:
  - Java
  - Spring
  - SQL
```

### 2. GitHub Actions에서 YAML
- `on:` → 실행 조건  
- `jobs:` → 여러 개 작업(Job) 정의  
- `steps:` → 각 Job 안의 세부 단계  

### 3. YAML의 장점
- 읽기 쉽고 간결함  
- 들여쓰기로 계층 구조 표현  
- GitHub Actions, Docker Compose, Kubernetes 등에서 표준처럼 사용  

---

## 🎯 최종 정리
- YAML = 사람이 읽기 좋은 설정 파일 포맷  
- GitHub Actions = 이 YAML로 자동화 시나리오를 정의하는 규칙서  
- 내가 사용하는 이유: **자동 포맷팅, 자동 검사, 자동 배포 → 코드 관리 자동화**
