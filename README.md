## Git in Action

This repository is used for practice on Git, 
include basic commands and basic knowledge.

#### Basic Concept
1. What is Git?  
![logo](https://git-scm.com/images/logos/1color-orange-lightbg@2x.png) 
> Git is a free and open source **distributed version control system** 
> designed to handle everything from small to very large projects
> with **speed and efficiency**.  

- What is version control system (VCS/SCM)?
> the management of changes to documents, computer programs, large websites,
> and other collection of information

- Why distributed?  
Compared with traditional centralize version control system(CVCS) like Subversion(SVN),
it avoids the single point failure, which means if the server down or lose data, no one 
can connect to the server to retrieve the code and even lose the change history.

- Why speed and efficiency?  
Git records the snapshots instead of differences   
other tools :  record the difference between each version, 
it takes time to switch to another version or branch  
![delta](https://git-scm.com/book/en/v2/images/deltas.png)  
Git : records the snapshots, use SHA-1 hash, 40-character string, 
can use shorter string as long as it's unique  
![snapshots](https://git-scm.com/book/en/v2/images/snapshots.png)  

2. Working directory / staging area / local repository / remote repository  
![areas](https://git-scm.com/book/en/v2/images/areas.png)  

3. origin / master / HEAD  
- origin : default name of remote repository  
    ``git remote add orgin git@github.com:allenwhm/git-in-action.git``  
- master : default name of first branch  
- HEAD : name of current branch, it's a pointer  
![head-to-master](https://git-scm.com/book/en/v2/images/head-to-master.png)  

#### Basic Action  
1. config username / email
- config scope  
    current repository : .git/config  
    global, for all repositories in current user --global : ~/.gitconfig
    system, for all uses in current system --system : /etc/gitconfig  
- list config  
    ``git config --list [--global/system]``  
- config username / email  
    ```
    git config --global user.name "allen"   
    git confif --global user.email "allen@wang.com"
    ```
- command alias  
    ```
    git config --global alias.co checkout
    git config --global alias.br branch
    git config --global alias.ci commit
    git config --global alias.st status
    git config --global alias.unstage 'reset HEAD --'
    git config --global alias.last 'log -1 HDEA'
    ```
- cancel alias
    ``git config --global --unset alias.st``
    
2. clone a repo from remote server  
- clone : git clone repo_url [new_repo_name]
    ``git clone git@github.com:allenwhm/git-in-action.git``
- git protocol
    - local protocol : based on file systems
    - git : no authentication, fastest
    - SSH : need add SSH ket into authorized_keys
    - HTTP/HTTPS : most convenient, with authentication 
    
3. init a repo with current project and publish it to git server(GitHub/Bitbucket)
- init repo : change directory to project folder  
    ```
    cd project_directory
    git init
    ```
- create .gitignore file to ignore files not required to be tracked
    ``echo target > .gitignore``    
- add files into staging area (working directory -> staging area)  
add all files  
    ``git add --all / * / .``  
add one file  
    ``git add file_name``
- commit changes to local repo (staging area -> local repo)  
    commit and write message in editor
    ``git commit``  
    commit and write message at the same time  
    ``git commit -m "commit message" ``  
    add and commit in one step  
    ```
    git commit -am "add and commit in one step"
    git commit -a -m "add and commit in one step"
    ```  
    commit --amend : commit again, and replace the last commit
    ```
    git commit -am "some commit here"
    some changes
    git commit --amend "latest commit"
    ```
- add remote repo for current repo  
    list remote repo  
    ``git remote``  
    list remote repo with verbose information  
    ``git remote -v``  
    list remote repo details  
    ``git remote show origin``  
    add remote repo : git remote add remote_name remote_url  
    ``git remote add origin git@github.com:allenwhm/git-in-action.git``  
    rename remote repo    
    ``git remote rename origin aw``  
    remove remote repo  
    ``git remote rm aw``  
- push code to remote repo : git push [remote_name] [remote_branch]  
    ``git push origin master``  
    ``git push``  
    push at the first time, add -u  
    ``git push -u origin master``  
- if your code is already tracked by Git, set origin to push the code  
    ```
    cd project_diretory
    git remote set-url origin git@github.com:allenwhm/git-in-action.git
    git push -u origin --all
    git push origin --tags
    ```  
- change remote repo url from https to git  
    ``git remote set-url origin git@github.com:allenwhm/git-in-action.git``  
- fetch or pull ?
    - pull = fetch + merge  
    fetch : fetch the latest remote repo into local repo
    merge : merge the changes into current branch  
    
4. git status / diff / log  
    - git status, check the status anytime  
    ``git status``  
    - check status with short message : git status [-s/--short]  
    ``git status -s``  
    result sample :
        ```
        M README.md
        MM Rakefile
        A lib/git.rb
        ?? LICENSE.txt
      
        ?? : not tracked  
        A : new files added to staging area  
        M on left : modified and saved in staging area
        M on the right : modified and not saved in staging area
        ```  
    - git diff [--staged(new version) / --cached]  
        - git diff without parameters, compare the difference of unstaged files 
        between working directory and staging area  
        ``git diff``  
        - git diff --staged : check difference of staged area and local repo  
        ``git diff --stage``  
    - git log : list all commits, order by commit time desc  
        - without parameters  
        ``git log``  
        -n : limit the number  
        ``git log -2``  
        - p : show the difference of each commit  
        ``git log -p -2``  
        - --stat : show less  
        ``git log --stat``  
        - --pretty : format the message  
            - oneline : one commit a line  
            - short  
            - full  
            - fuller  
            - format : eg. format:"%h - %an, %ar : %s"  
        - --graph : show the tree of historical commits  
            ``git log --pretty=format:"%h - %an, %ar : %s" --graph``  
5. cancel the changes staged or in working directory  
    - cancel the changes staged, change to unstaged status : git reset HEAD file_name  
    ``git reset HEAD .gitignore``  
    - cancel the changes in working directory, change to status in last commit : 
    git checkout -- file_name  
    ``git checkout -- .gitignore``  
6. remove / move files  
    - remove with Git : git rm file_name  
    - remove with force mode -f : if file modified and saved in staging area  
    ``git rm -f file_changed_and_staged``  
    - remove files in remote but keep in local (push to remote before adding to .gitignore)  
        ```
        git rm --cached -r directory_name  
        git rm --cached file_name
        ```  
    - delete manually and git rm file_name  
    - move file : git mv file_from file_to  
7. branch  
    - branch, how Git distinguish itself from other VCS  
    it will take minutes to create and switch branch in other VCS but almost in no time for Git,
    as it only generate or move the index   
    - list branch, star means current branch  
    ``git branch --list``
    - list branch with last commit  
    ``git branch -v``  
    - list branch with more information  
    ``git branch -vv``  
    - create branch  
    ``git branch testing``  
    ![create-branch](https://git-scm.com/book/en/v2/images/head-to-master.png)
    - switch branch  
    ``git switch testing``
    ![switch-branch](https://git-scm.com/book/en/v2/images/head-to-testing.png)  
    - create and switch branch    
    ``git checkout -b testing``  
    - merge branch : git merge  branch_to_be_merged [--no-ff] [-m "message"]    
    ![branch-merge](https://git-scm.com/book/en/v2/images/basic-branching-4.png)
    ``git merge hotfix``
        - fast-forward : move the pointer to the target branch pointer directly, will hide
        the merge action in commit history  
        ![merge-ff](https://git-scm.com/book/en/v2/images/basic-branching-5.png)
        - --no-ff : git merge --no-ff, will create a new commit  
    - delete branch : git branch -d branch_name  
    ``git branch -d hotfix``  
    - branch management 
        - git branch --merged : check branches already merged into current branch  
        - git branch --no-merged  
8. rebase  
    - merge changes from two branch  
        - merge  
        - rebase  
        - difference and choice 
            - rebase makes the commit history simple
            - follow the principles  
                > Do not rebase commits that exists outside your repository and people
                > may have based work on them.
    - example  
        - current status  
        ![rebase-1](https://git-scm.com/book/en/v2/images/basic-rebase-1.png)  
        - by merge  
        ``git merge experiment``  
        ![rebase-2](https://git-scm.com/book/en/v2/images/basic-rebase-2.png)  
        - by rebase : git rebase base_branch [source_branch]  
            ```
            git checkout experiment
            git rebase master
            ```  
            ![rebase-3](https://git-scm.com/book/en/v2/images/basic-rebase-3.png)
9. tags  
    - type  
        - lightwight : reference of a commit
        - annotated : covers more information, recommend
    - list tags : result shows in alphabetical order  
        ``git tag``  
    - filter tags  
        ``git tag -l 'v1.8.*' ``  
    - create annotated tag  
        ``git tag -a v1.4 -m 'version 1.4' ``  
    - create lightweight tag  
        ``git tag v1.4-lw``  
    - show the tag message  
        ``git show v1.4``  
    - amend tag with commit id  
        ``git tag -a v1.2 9fceb02``  
    - push tags to remote  
        ``git push origin --tags``    
    - push one tag to remote  
        ``git push origin v1.4``  
    - create branch based on tag : git checkout -b [branch_name] [tag_name]  
        ``git checkout -b version2 v1.2``  
    - create a branch which name different from remote  
        ``git checkout -b sf origin/serverfix``  
    - set local branch to track a remote branch with different name  
        ``git checkout --track origin/serverfix``  
        or ``git branch -u origin/serverfix``  
10. Reference  
    - learn Git in 30 minutes : Liao Xuefeng
    - Pro Git 

    
