### 상황 1. fast-foward => 프리라이딩

> fast-foward는 feature 브랜치 생성된 이후 master 브랜치에 변경 사항이 없는 상황

1. feature/test branch 생성 및 이동

   ```bash
   $ git branch feature/main
   (master) $ git checkout feature/main
   Switched to branch 'feature/main'
   (feature/main) $
   ```

2. 작업 완료 후 commit

   ```bash
   $ touch main.html
   $ git add .
   $ git commit -m 'Complete main'
   [feature/main c3aa742] Complete main
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 main.html
   ```


3. master 이동

   ```bash
   $ git checkout master
   Switched to branch 'master'
   $ git log --oneline
   6a84975 (HEAD -> master, feature/main) README.md 생성
   91495fa 1.txt 생성
   ```


4. master에 병합

   ```bash
   (master) $ git merge feature/main
   Updating 6a84975..c3aa742
   Fast-forward
    main.html | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 main.html
   ```


5. 결과 -> fast-foward (단순히 HEAD를 이동)

   ```bash
   $ git log --oneline
   c3aa742 (HEAD -> master, feature/main) Complete main
   6a84975 README.md 생성
   91495fa 1.txt 생성
   ```

6. branch 삭제

   ```bash
   $ git branch -d feature/main
   Deleted branch feature/main (was c3aa742).
   ```
   
   ![ff](../TIL/md-img/ff.jpg)
   
   

---

### 상황 2. merge commit (보고서/PPT)

> 서로 다른 이력(commit)을 병합(merge)하는 과정에서 다른 파일이 수정되어 있는 상황
>
> git이 auto merging을 진행하고, commit이 발생된다.

1. feature/signout branch 생성 및 이동

   ```bash
   $ git checkout -b feature/signout
   Switched to a new branch 'feature/signout'
   (feature/signout) $
   ```

2. 작업 완료 후 commit

   ```bash
   $ touch signout.html
   $ git add .
   $ git commit -m 'Complete signout'
   [feature/signout e018c79] Complete signout
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 signout.html
    $ git log --oneline
   e018c79 (HEAD -> feature/signout) Complete signout
   c3aa742 (master) Complete main
   6a84975 README.md 생성
   91495fa 1.txt 생성
   ```

3. master 이동

   ```bash
   $ git checkout master
   Switched to branch 'master'
   ```

4. *master에 추가 commit 이 발생시키기!!*

   * **다른 파일을 수정 혹은 생성하세요!**

   ```bash
   $ touch hotfix.html
   $ git add .
   $ git commit -m 'hotfix'
   [master c39a6b1] hotfix
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 hotfix.html
   $ git log --oneline
   c39a6b1 (HEAD -> master) hotfix
   c3aa742 Complete main
   6a84975 README.md 생성
   91495fa 1.txt 생성
   ```

5. master에 병합

   ```bash
   $ git merge feature/signout
   Merge made by the 'recursive' strategy.
    signout.html | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 signout.html
   ```

6. 결과 -> 자동으로 *merge commit 발생*

   ![image-20210205142527223](../TIL/md-img/image-20210205142527223.png)

   * vim 편집기 화면이 나타납니다.

   * 자동으로 작성된 커밋 메시지를 확인하고, `esc`를 누른 후 `:wq`를 입력하여 저장 및 종료를 합니다.
      * `w` : write
      * `q` : quit
      
   * 커밋이  확인 해봅시다.

7. 그래프 확인하기

   ```bash
   $ git log --oneline --graph
   *   dd016ae (HEAD -> master) Merge branch 'feature/signout' into master
   |\
   | * e018c79 (feature/signout) Complete signout
   * | c39a6b1 hotfix
   |/
   * c3aa742 Complete main
   * 6a84975 README.md 생성
   * 91495fa 1.txt 생성
   ```

8. branch 삭제

   ```bash
   $ git branch -d feature/signout
   Deleted branch feature/signout (was e018c79).
   ```

   ![mc](../TIL/md-img/mc.jpg)

---

### 상황 3. merge commit 충돌 (같은 파일 수정)

> 서로 다른 이력(commit)을 병합(merge)하는 과정에서 동일 파일이 수정되어 있는 상황
>
> git이 auto merging을 하지 못하고, 해당 파일의 위치에 라벨링을 해준다.
>
> 원하는 형태의 코드로 직접 수정을 하고 merge commit을 발생 시켜야 한다.

1. feature/board branch 생성 및 이동

   ```bash
   $ git checkout -b feature/board
   Switched to a new branch 'feature/board'
   ```

2. 작업 완료 후 commit

   ```bash
   $ touch README.md
   $ touch board.html
   $ git commit -m 'Comeplete README and board.html'
   [feature/board 736d589] Comeplete README and board.html
    2 files changed, 1 insertion(+)
    create mode 100644 board.html 
   ```
   
   ```bash
   $ git log --oneline
   736d589 (HEAD -> feature/board) Comeplete README and board.html
   dd016ae (master) Merge branch 'feature/signout' into master
   c39a6b1 hotfix
   e018c79 Complete signout
   c3aa742 Complete main
   6a84975 README.md 생성
   91495fa 1.txt 생성
   ```

3. master 이동

    ```bash
    $ git checkout master
    Switched to branch 'master'
    ```


4. *master에 추가 commit 이 발생시키기!!*

   * **동일 파일을 수정 혹은 생성하세요!**

   ```bash
   $ git add .
   $ git commit -m 'Update README.md'
   [master 00cc75b] Update README.md
    1 file changed, 3 insertions(+)
   ```

5. master에 병합

   ```bash
   $ git merge feature/board
   # README.md를 자동으로 병합중
   Auto-merging README.md
   ```


6. 결과 -> *merge conflict발생*

   ```bash
   # 충돌!
   # README.md merge conflict!
   CONFLICT (content): Merge conflict in README.md
   # conflict를 고치고 결과 커밋하기
   Automatic merge failed; fix conflicts and then commit the result.
   ```


7. 충돌 확인 및 해결

   ```bash
   <<<<<<< HEAD
   # README 수정
   
   * 수정했슴다
   =======
   # branch 실습 중!
   >>>>>>> feature/board
   ```
   
   ```bash
   $ git status
   On branch master
   You have unmerged paths.
     (fix conflicts and run "git commit")
     (use "git merge --abort" to abort the merge)
   
   Changes to be committed:
           new file:   board.html
   
   Unmerged paths:
     (use "git add <file>..." to mark resolution)
           both modified:   README.md
   ```


8. merge commit 진행

    ```bash
    $ git add .
    $ git commit -m 'Fix merge conflict'
[master cfbbd0f] Fix merge conflict
   ```
   
   * vim 편집기 화면이 나타납니다.
   
   * 자동으로 작성된 커밋 메시지를 확인하고, `esc`를 누른 후 `:wq`를 입력하여 저장 및 종료를 합니다.
      * `w` : write
      * `q` : quit
      
   * 커밋이  확인 해봅시다.
   
9. 그래프 확인하기

    ```bash
   $ git log --oneline --graph
   *   cfbbd0f (HEAD -> master) Fix merge conflict
   |\
   | * 736d589 (feature/board) Comeplete README and board.html
   * | 00cc75b Update README.md
   |/
   *   dd016ae Merge branch 'feature/signout' into master
   |\
   | * e018c79 Complete signout
   * | c39a6b1 hotfix
   |/
   * c3aa742 Complete main
   * 6a84975 README.md 생성
   * 91495fa 1.txt 생성
   ```


10. branch 삭제

    ```bash
    $ git branch -d feature/board
    Deleted branch feature/board (was 736d589).
    ```
    
    ![mccc](../TIL/md-img/mccc.jpg)

