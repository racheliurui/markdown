S1, have existing repo and change to use git repo to track the change

with remot repo
$ mkdir /path/to/new_repo
$ cd /path/to/new_repo
$ git --bare init

with existing repo
$ cd /path/to/existing_repo
$ git init
$ vi .gitignroe
$ git add .
$ git commit -m "init"

$ git push --set-upstream /path/to/new_repo mastergit

$git remote add origin /path/tocurrent-existinglib
fatal: remote origin already exists.
$ git remote rm origin
and then add again


Create Developer branch
git branch develop
git checkout develop

follow the flow the develop
https://gist.github.com/yesmeck/4245406

======cancel any change in current branch that not yet being staged (add .) ======
git checkout -- .


S2, create a new bare repo remotely and then start from local

git clone /path/to/remote/bare/repo

====
check remote repository
git ls-remote

print all branchs
git show-branch -a


Common working process
$ git checkout -b feature/xxx develop
# 写代码，提交，写代码，提交。。。
# feature 开发完成，合并回 develop
$ git checkout develop
# 务必加上 --no-ff，以保持分支的合并历史
$ git merge --no-ff feature/xxx
$ git branch -d feature/xxx
