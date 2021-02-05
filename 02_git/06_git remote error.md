# git remote error

* 문제
  * github 원격 저장소 상에서 README.md를 생성하고
  * 로컬 컴퓨터에서 해당 폴더에 다른 작업을 한 다음에 push를 시도했을 때

```bash
$ git push origin master
To https://github.com/minami-cs/first.git
# 거절됨
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/minami-cs/first.git'
# 왜냐하면.. 원격저장소가 가진 작업(커밋)이
hint: Updates were rejected because the remote contains work that you do
# 로컬에(locally) 없기 때문에
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

* 해결
  * 이렇게 될 경우에는 `hint`에 나온 대로 `git pull` 명령어를 입력해보자.
  * 그러면 github 원격저장소에만 저장되어 있던 작업내역이 로컬로 받아진다.

```bash
$ git pull origin master
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 756 bytes | 42.00 KiB/s, done.
From https://github.com/minami-cs/first
 * branch            master     -> FETCH_HEAD
   d3f5dbc..4a95cae  master     -> origin/master
Updating d3f5dbc..4a95cae
Fast-forward
 README.md | 3 +++
 1 file changed, 3 insertions(+)
 create mode 100644 README.md
```

* 그리고 로컬에서 작업 후 git add,  git commit, git push를 하고 나서 github의 커밋 내역을 보면 자동으로 merge가 발생한다!
   * 예시
    ![image-20210205112342883](C:%5CUsers%5CUser%5CDesktop%5CTIL%5Cmd-img%5Cimage-20210205112342883.png)

