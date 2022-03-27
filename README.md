# git-learning #
----


docker build -t training/git .
mkdir -p /Users/eruizalo/git_docker2/
docker run -it -d -v /Users/eruizalo/git_docker2/:/root/projects/ --name learning-git training/git:latest
docker exec -it git bash
cd projects

docker build -t training/git .
mkdir -p /Users/eruizalo/git_docker2/
docker run -it -d -v /Users/eruizalo/git_docker2/:/root/projects/ --name learning-git training/git:latest
docker exec -it git bash
cd projects

git config --global init.defaultBranch main
git config --global user.email eduardo.ruiz@hablapps.com
git config --global user.name "Eduardo Ruiz Alonso"

git config --list

git init
echo -e "# This is my git project\n" > README.md
echo -e "Cheatsheet: https://devhints.io/git-log-format\n\n" >> README.md
git add .
git commit -m "Initial commit"

git config user.email eduardo.ruiz@hablapps.com
git config user.name "Eduardo Ruiz"

git config --list --show-origin


echo "this is a test" > test.txt

#####################################
#                                   #
#      tracked and untracked        #
#                                   #
#####################################
git status
echo "-----" >> README.md
git add test.txt
git status

git commit -a -m "commit 2"   ## -a ==> ONLY TRACKED FILES
git status

touch .gitignore
touch .testing
git status
git add .gitignore
echo ".*" >> .gitignore
git status
mkdir secrets
touch secrets/secret1.txt
touch secrets/secret2.txt
git status
git status -u
git status -u -s
echo "secrets/*" >> .gitignore
git status
git commit -am "gitignore"


#####################################
#                                   #
#            log & alias            #
#                                   #
#####################################
echo "Something" >> README.md
git add .
git config --global alias.unstage 'reset --'
git config --global alias.discard 'checkout --'
git config --global alias.stats 'status -s'

git stats
git unstage README.md
git stats
git discard README.md
git stats


git log
git log -n 1
git config --global alias.logtree "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)(%ar|%cr)%C(reset) %C(bold yellow)%d%C(reset) %C(white)%s%C(reset) %C(dim green)- %an%C(reset)' --all"
git logtree

#####################################
#                                   #
# Qué es HEAD
# referencias relativas (^/~)
# creando y reposicionando ramas
#                                   #
#####################################


git branch branch1
git branch
git checkout branch1
git branch
git checkout -b branch2
git branch
git logtree

echo "commit 3" >> README.md
git add .
git commit -am "commit 3"
git checkout branch1

echo "commit 4" >> tests.txt
git add .
git commit -am "commit 4"
git merge branch2
git logtree

git branch ref
git branch -f main ref
git logtree
git checkout main
git branch -f ref HEAD~1
git branch -f refParen1 main^1
git branch -f refParen2 main^2
git logtree

git branch newBranch commit_hash
git logtree


git tag v0.0.2
git tag v0.0.1 main~3
git tag
git branch release/v0.0.1 v0.0.1
git logtree


#####################################
#                                   #
#        Stash (apply vs pop)       #
#                                   #
#####################################
git checkout -b feature/myFeature
echo "some changes" >> README.md
git checkout main
git commit -am "main changes"
git checkout feature/myFeature
echo "other changes" >> README.md
git checkout main           <----will fail
git stats
git stash
git stash list
git stats
git checkout main

git checkout feature/myFeature
git stash pop stash@{0}
git stash list
git stash
git stash apply stash@{0}
git stash list
git stash -m "WIP feature my stash"
git stash list
git stash show stash@{0}
git stash show -p


#####################################
#                                   #
# Revertir cambios (revert / reset) #
#                                   #
#####################################
echo -e "\n\nthis will not fail\n" >> README.md
git commit -am "this will not fail"
echo -e "\n\nbut this one will\n" >> README.md
git commit -am "but this one will"
git logtree
git revert HEAD
git revert <COMMIT_ID HEAD - 2>
git logtree
git checkout main
git revert <COMMIT_ID HEAD - 2>
#### git revert (--continue | --skip | --abort | --quit)
git checkout feature/myFeature
git logtree
git reset --soft HEAD~3
git logtree
git status -vv
git commit -am "this too 2"
git reset --mixed HEAD~1
git logtree
git status -vv
git commit -am "this too 3"
git reset --hard HEAD~1
git logtree
git status -vv
git reset --hard main
git logtree
git reset --hard <COMMIT ID DELETED BRANCH>
git logtree


#####################################
#                                   #
#            Commit amend           #
#                                   #
#####################################
echo -e "\nDis is som text\n" >> README.md
git commit -am "Fixing readme"
git logtree
head -n -3 README.md > README.md
echo -e "\nThis is some text\n" >> README.md
git logtree
git commit -am "Fixing readme" --amend
git logtree

git config user.name "Mr. X"
git commit -am "Fixing readme 2" --amend
git log --pretty=fuller -n 2


#####################################
#                                   #
#           Cherrypicking           #
#                                   #
#####################################
git checkout -b feature/myOtherFeature main
git logtree
echo -e "MyClass()\n" > MyClass.scala
git add .
git commit -m "Add class"
echo "Utilities()" > utils.scala
git add utils.scala
git commit -m "add utils"

git checkout -b feature/myOtherFeature2 main
git logtree
echo -e "MyClass2()\n" > MyClass2.scala
git add MyClass2.scala
git commit -m "[+] MyClass2"
git logtree
	##### echo -e "MyClass2() extends Utilities\n" > MyClass2.scala

git cherry-pick <COMMIT ID OTHER BRANCH>
git logtree
git log --pretty=fuller -n 2
git config user.name "Eduardo Ruiz"
echo -e "MyClass2() extends Utilities\n" > MyClass2.scala
git commit -am "extends"


#####################################
#                                   #
#          MERGE VS REBASE          #
#                                   #
#####################################
git checkout feature/myOtherFeature
echo -e "some code here...\n" >> MyClass.scala
git commit -am "some code"
git branch backup/feature/myOtherFeature feature/myOtherFeature
git checkout main
ls -l
git merge --no-ff feature/myOtherFeature
git logtree
ls -l
git reset --hard HEAD~1
git logtree

git merge --squash feature/myOtherFeature
git commit -m "merge feature/myOtherFeature -> main"
git logtree
ls -l

git reset --hard HEAD~1
git checkout feature/myOtherFeature2
git rebase main
git logtree				### OJO deleted commit
git rebase -i main		### PICK + SQUASH
git logtree




##########################################################################
#####			                          					         #####
#####			            USING REMOTES 					         #####
#####			                          					         #####
##########################################################################