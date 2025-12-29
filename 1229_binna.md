# 12월 29일 Git 학습 정리

## Fork & PR 워크플로우

### 전체 흐름
```
원본 repo (v4) --sync--> Fork (v4) --pull--> 내 작업 (conflict)
                                   <--push--
```

### Fork & PR 실습 순서

```bash
# 1. Fork한 저장소 Clone
git clone https://github.com/[내계정]/oz-ai-bootcamp.git oz-practice

# 2. 원본 저장소를 upstream으로 추가
cd oz-practice
git remote add upstream https://github.com/qu3vipon/oz-ai-bootcamp.git

# 3. 원본 코드 가져오기
git fetch upstream

# 4. upstream/main 기반으로 브랜치 생성
git checkout -b feature/my-contribution upstream/main

# 5. 파일 추가/수정
# (예: 20_binna.md 파일 추가)

# 6. 커밋
git add 20_binna.md
git commit -m "docs: #20 Git 학습 정리 추가"

# 7. 내 Fork 저장소에 푸시
git push -u origin feature/my-contribution

# 8. GitHub에서 PR 생성
# Pull requests 탭 → New pull request
```

---

## 원격 저장소 개념

| 이름 | 설명 |
|------|------|
| `origin` | 내 Fork 저장소 (binna0728/oz-ai-bootcamp) |
| `upstream` | 원본 저장소 (qu3vipon/oz-ai-bootcamp) |

---

## Conflict 해결 흐름

### 1. Sync (원본 → Fork 동기화)
- GitHub에서 **Sync fork** 버튼 클릭
- 또는:
```bash
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

### 2. Pull (Fork → 로컬)
```bash
git fetch origin
git log --graph --all    # 브랜치 상태 확인
git merge origin/main
```

### 3. Conflict 수정
- `<<<<<<<`, `=======`, `>>>>>>>` 표시된 부분 정리
- 원하는 코드만 남기고 삭제

### 4. Commit & Push
```bash
git add .
git commit -m "fix: resolve merge conflicts"
git push origin [브랜치명]
```

---

## 유용한 명령어

| 명령어 | 설명 |
|--------|------|
| `git fetch origin` | 원격 코드 가져오기만 (병합 안 함) |
| `git pull` | fetch + merge 한번에 |
| `git log --graph --all` | 모든 브랜치 히스토리 시각적으로 보기 |
| `git remote -v` | 연결된 원격 저장소 확인 |

---

## 핵심 포인트

1. **upstream/main 기반 브랜치**: 원본과 히스토리 일치해야 PR 가능
2. **Sync 먼저**: 원본이 업데이트되면 Fork도 동기화해야 conflict 줄어듦
3. **fetch vs pull**: fetch는 안전하게 확인 후 병합, pull은 바로 병합

---

## Collaborator로 협업하기

원본 저장소에 직접 접근 권한이 있는 경우 (Collaborator)

```bash
# 1. collaborator 폴더에서 원본 저장소 Clone
cd collaborator
git clone https://github.com/qu3vipon/oz-ai-bootcamp.git

# 2. 폴더 이동
cd oz-ai-bootcamp

# 3. main 브랜치 확인
git branch

# 4. 원격 저장소 확인
git remote -v
# origin  https://github.com/qu3vipon/oz-ai-bootcamp.git (fetch)
# origin  https://github.com/qu3vipon/oz-ai-bootcamp.git (push)
```

### 주의: remote origin already exists 에러
```bash
git remote add origin https://github.com/qu3vipon/oz-ai-bootcamp.git
# error: remote origin already exists.
```
- Clone하면 자동으로 origin이 설정됨
- 추가로 `git remote add origin` 할 필요 없음!

### Fork vs Collaborator 차이

| 방식 | 설명 |
|------|------|
| **Fork** | 원본 복사 → 내 저장소에서 작업 → PR로 기여 |
| **Collaborator** | 원본에 직접 push 권한 있음 → 바로 작업 가능 |

| 구분 | Fork | Collaborator |
|------|------|--------------|
| origin | 내 Fork 저장소 | 원본 저장소 |
| upstream | 원본 저장소 | 필요 없음 |
| remote add 필요 | O (upstream 추가) | X (자동 설정) |

### 브랜치 삭제 및 생성

```bash
# 잘못 만든 브랜치 삭제
git switch main                    # main으로 이동
git branch -d feature/donghyeon    # 브랜치 삭제

# 새 브랜치 생성
git switch -c feature/binna        # 새 브랜치 생성 + 이동
```
