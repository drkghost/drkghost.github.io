---
title: Git notes: log vs reflog
---

Let's compare base git functions - log and reflog.

First, we need to create a repository in github and clone it locally.
```
PS D:\dev\repos> git clone https://github.com/drkghost/new-git-demo-repo.git
Cloning into 'new-git-demo-repo'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
PS D:\dev\repos>
```

Let's commit some changes
```
PS D:\dev\repos\new-git-demo-repo> git add .
PS D:\dev\repos\new-git-demo-repo> git commit -m "add new file"
[main 8657ba6] add new file
 1 file changed, 1 insertion(+)
 create mode 100644 new-file.txt
PS D:\dev\repos\new-git-demo-repo> git push
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 12 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 297 bytes | 297.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/drkghost/new-git-demo-repo.git
   784342e..8657ba6  main -> main
PS D:\dev\repos\new-git-demo-repo>
```

And create a new branch
```
PS D:\dev\repos\new-git-demo-repo> git checkout -b "dev-branch-todo-smth"
Switched to a new branch 'dev-branch-todo-smth'
PS D:\dev\repos\new-git-demo-repo> git branch -a
* dev-branch-todo-smth
  main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
PS D:\dev\repos\new-git-demo-repo>
```

Check our current state with log and reflog commands:
```
PS D:\dev\repos\new-git-demo-repo> git log --pretty=oneline
8657ba6f0dcd22f99cd4784c2cec4629dd8e7503 (HEAD -> dev-branch-todo-smth, origin/main, origin/HEAD, main) add new file
784342e659bce3ec5d8fce5ac970b404590b761e Initial commit
PS D:\dev\repos\new-git-demo-repo> git reflog
8657ba6 (HEAD -> dev-branch-todo-smth, origin/main, origin/HEAD, main) HEAD@{0}: checkout: moving from main to dev-branch-todo-smth
8657ba6 (HEAD -> dev-branch-todo-smth, origin/main, origin/HEAD, main) HEAD@{1}: commit: add new file
784342e HEAD@{2}: clone: from https://github.com/drkghost/new-git-demo-repo.git
```

Let's checkout to main branch, go back to dev branch, and repeath the same commands:
```
PS D:\dev\repos\new-git-demo-repo> git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
PS D:\dev\repos\new-git-demo-repo> git log --pretty=oneline
8657ba6f0dcd22f99cd4784c2cec4629dd8e7503 (HEAD -> main, origin/main, origin/HEAD, dev-branch-todo-smth) add new file
784342e659bce3ec5d8fce5ac970b404590b761e Initial commit
PS D:\dev\repos\new-git-demo-repo> git reflog
8657ba6 (HEAD -> main, origin/main, origin/HEAD, dev-branch-todo-smth) HEAD@{0}: checkout: moving from dev-branch-todo-smth to main
8657ba6 (HEAD -> main, origin/main, origin/HEAD, dev-branch-todo-smth) HEAD@{1}: checkout: moving from main to dev-branch-todo-smth
8657ba6 (HEAD -> main, origin/main, origin/HEAD, dev-branch-todo-smth) HEAD@{2}: commit: add new file
784342e HEAD@{3}: clone: from https://github.com/drkghost/new-git-demo-repo.git
PS D:\dev\repos\new-git-demo-repo>
```

As you can see, there's the same output after `git log` but `git reflog` became different.

That's the main difference between commands: `git log` shows history of commites in the current branch, but `git reflog` reflects all changes in the repository.
Git keeps history of changes 90 days by default. This oprion can be overwritten in config, gc.reflogexpire, or by using `git reflog expire --expire=time` command.

It's no only about writing history of changes. Reflog is powerfull tool for reverting changes, restoring removed files etc.

Going back to out demo project, dev branch. Let's commit a new file we want to keep forever
![new commit with important data](/assets/2021-07-11-git-demo-1.png)


More about reflog:
<https://www.w3docs.com/learn-git/git-reflog.html#>
<https://git-scm.com/docs/git-reflog>


