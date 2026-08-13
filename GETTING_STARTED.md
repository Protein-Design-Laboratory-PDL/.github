# Git / GitHub 사용 가이드 (실전 명령어)

이 문서는 처음 Git을 쓰거나 오랜만에 쓰는 랩원을 위한 **그대로 따라 치면 되는** 명령어 모음입니다.
규칙/정책은 Organization 메인 페이지의 README와 CONTRIBUTING.md를 참고하시고, 이 문서는 "실제로 뭘 타이핑해야 하는지"에 집중합니다.

---

## 0. 처음 한 번만 하면 되는 설정

### Git 설치 확인
```bash
git --version
```
설치 안 되어 있으면: Mac은 `brew install git`, Windows는 [git-scm.com](https://git-scm.com)에서 다운로드.

### 이름/이메일 설정 (커밋에 표시될 정보)
```bash
git config --global user.name "본인이름"
git config --global user.email "본인깃허브에연결된이메일"
```

### 인증 방법 (SSH 권장)
비밀번호 입력 없이 push하려면 SSH 키를 등록하는 걸 추천합니다.

```bash
# SSH 키가 이미 있는지 확인
ls -al ~/.ssh

# 없으면 새로 생성 (엔터 계속 눌러서 기본값으로 진행해도 됨)
ssh-keygen -t ed25519 -C "본인이메일"

# 공개키 출력 → 복사
cat ~/.ssh/id_ed25519.pub
```
복사한 내용을 GitHub → 우측 상단 프로필 → **Settings → SSH and GPG keys → New SSH key**에 붙여넣기.

---

## 1. 레포 처음 받아오기 (Clone)

레포 페이지에서 초록색 **Code** 버튼 → SSH 탭 → 주소 복사 후:

```bash
git clone git@github.com:Protein-Design-Laboratory-PDL/레포이름.git
cd 레포이름
```

---

## 2. 작업 시작하기

### main을 최신 상태로
```bash
git checkout main
git pull origin main
```

### 새 브랜치 만들기
```bash
git checkout -b feature/작업내용
```
예: `git checkout -b feature/vdm-clash-filter`

---

## 3. 작업하고 커밋하기

### 변경 사항 확인
```bash
git status
git diff
```

### 스테이징 + 커밋
```bash
git add 파일명          # 특정 파일만
git add .               # 변경된 파일 전체

git commit -m "feat: vdM clash 검사 로직 추가"
```

### 원격 저장소로 올리기
```bash
git push origin feature/작업내용
```
(맨 처음 push할 때는 `git push -u origin feature/작업내용`으로 하면 다음부터 `git push`만 쳐도 됩니다.)

---

## 4. Pull Request 만들기

push하고 나면 터미널에 링크가 뜨는데, 그걸 클릭하거나 아래 방법 사용:

1. GitHub 레포 페이지 접속
2. 노란 배너에 **Compare & pull request** 버튼이 보이면 클릭
3. 제목/설명 작성 → **Create pull request**

리뷰어 지정: PR 화면 우측 **Reviewers**에서 팀원 선택

---

## 5. 리뷰 반영하기

리뷰에서 수정 요청이 오면, 같은 브랜치에서 계속 작업하면 됩니다.

```bash
# (코드 수정 후)
git add .
git commit -m "fix: 리뷰 반영 - 예외 처리 추가"
git push origin feature/작업내용
```
같은 브랜치로 push하면 열려 있는 PR에 자동으로 반영됩니다.

---

## 6. Merge 후 정리

PR이 GitHub 웹에서 merge되고 나면, 로컬을 정리합니다.

```bash
git checkout main
git pull origin main

# 다 쓴 로컬 브랜치 삭제
git branch -d feature/작업내용

# 원격 브랜치도 삭제 (GitHub에서 자동 삭제 버튼 눌러도 됨)
git push origin --delete feature/작업내용
```

---

## 7. 최신 main 내용을 작업 중인 브랜치에 반영하기

다른 사람 작업이 먼저 merge돼서 내 브랜치가 오래된 경우:

```bash
git checkout feature/작업내용
git fetch origin
git merge origin/main
```
충돌(conflict)이 뜨면 해당 파일 열어서 `<<<<<<<`, `=======`, `>>>>>>>` 표시된 부분을 직접 수정한 뒤:
```bash
git add 충돌났던파일
git commit
git push origin feature/작업내용
```

---

## 자주 겪는 문제

**Q. push할 때 `Permission denied` 에러가 나요**
→ SSH 키 등록이 안 됐거나, 레포 접근 권한이 없는 경우입니다. Organization 관리자에게 초대/권한 확인 요청하세요.

**Q. `git pull` 했더니 충돌(conflict)이 났어요**
→ 당황하지 말고 충돌 표시된 파일을 열어 직접 정리 후 `git add` → `git commit`. 잘 모르겠으면 브랜치 그대로 두고 랩원에게 물어보는 게 안전합니다 (섣불리 강제 push 하지 마세요).

**Q. 실수로 main에 직접 커밋해버렸어요**
→ 아직 push 전이면:
```bash
git branch feature/살릴브랜치명   # 현재 커밋을 새 브랜치로 옮겨두기
git reset --hard origin/main      # main을 원격 상태로 되돌리기
```
이미 push했다면 바로 되돌리지 말고 랩 GitHub 관리자에게 알려주세요.

---

궁금한 점은 언제든 랩 GitHub Organization 관리자에게 문의해주세요.
