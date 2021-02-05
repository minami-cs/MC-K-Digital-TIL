# git status

```bash
$ git status
on branch master
# 커밋될 변경사항
# staging area
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
  		# 새파일
        new file:   a.txt

# staged(커밋을 위해서)되지 않은 변경사항
# working directory
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        # 삭제됨
        deleted:    a.txt

# 트래킹이 되지 않는 파일
# 일반적으로 새로 만든 파일
# working directory
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        "02_git/00_tip1.git \355\216\270\354\247\221\352\270\260\353\245\274 vs code\353\241\234 \353\260\224\352\276\270\352\270\260.md"
        02_git/04_status.md
        02_git/06_git remote error.md
        md-img/image-20210205112342883.png

```

