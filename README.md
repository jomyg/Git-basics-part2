===============================================================================================================================================

[root@ip-172-31-41-83 mydir]# ls -al
total 20
drwxr-xr-x 3 root root 104 Mar 16 10:19 .
dr-xr-x--- 4 root root 132 Mar 16 10:17 ..
drwxr-xr-x 7 root root 119 Mar 16 10:10 .git
-rw-r--r-- 1 root root  29 Mar 16 10:09 .gitignore        >>>>>>>>>>>>>>>>>>>>> This is used to ignore the unwanted files which we dont want to push
-rw-r--r-- 1 root root  90 Mar 16 09:59 hosts
-rw-r--r-- 1 root root 343 Mar 16 10:20 main.yml
-rw-r--r-- 1 root root  25 Mar 16 10:00 server.pem
-rw-r--r-- 1 root root  20 Mar 16 10:20 variables.yml
[root@ip-172-31-41-83 mydir]# cat .gitignore
hosts
server.pem
.gitignore

[root@ip-172-31-41-83 mydir]# git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        main.yml
        variables.yml

nothing added to commit but untracked files present (use "git add" to track)

#cat main.yml

---
- name: " GIT Test"
  hosts: amazon
  become: true
    - atop.yml
  tasks:
    
    - name: "Installing {{task1}} "
      yum:
        name: "httpd"
        state: present
        

    - name: "Restarting/Enabling {{task1}} Webserver."
      service:
        name: httpd
        state: restarted
        enabled: true
# cat variables.yml       
---
task1: "apache"


[root@ip-172-31-41-83 mydir]# git add .
[root@ip-172-31-41-83 mydir]# git ls-files -s
100644 bfba6d595618e2a92a672e323078a047a9ad7ea2 0       main.yml
100644 e4fd69837414c9d1e534e49a55085acdabda8af1 0       variables.yml
[root@ip-172-31-41-83 mydir]# rm -rf .git/hooks/*
[root@ip-172-31-41-83 mydir]# tree .git
.git
├── branches
├── config
├── description
├── HEAD
├── hooks
├── index
├── info
│   └── exclude
├── objects
│   ├── bf
│   │   └── ba6d595618e2a92a672e323078a047a9ad7ea2                >> There you go
│   ├── e4
│   │   └── fd69837414c9d1e534e49a55085acdabda8af1
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags

11 directories, 7 files

[root@ip-172-31-41-83 mydir]# git commit -m " Apache installation task 1"                    >>> Here we commit the files
[master (root-commit) 164b9f8]  Apache installation task 1
 2 files changed, 20 insertions(+)
 create mode 100644 main.yml
 create mode 100644 variables.yml
[root@ip-172-31-41-83 mydir]# git log
commit 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e (HEAD -> master)
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:25:49 2022 +0000

     Apache installation task 1                                       >>>>>>>>>>>>>>>>>>>> Commit msg
    
 ===============================================================================================================================================
   
Listing Branch Default Branch Name Is "master" github "main"

[root@ip-172-31-41-83 mydir]# git branch
* master


Working of git log
==============================
- HEAD   +======> Master +---====-> Recent Commit Object Id from the master +==========> Prints the details from Commit Object
                
The above flow working is like below                 
[root@ip-172-31-41-83 mydir]# cat .git/HEAD
ref: refs/heads/master
[root@ip-172-31-41-83 mydir]# cat .git/refs/heads/master
164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e
[root@ip-172-31-41-83 mydir]# git cat-file -p 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e
tree fd3d784b20ba23b53e9f50ffe21ea9cee66a8655
author jomy <jomy1@gmail.com> 1647426349 +0000
committer jomy <jomy1@gmail.com> 1647426349 +0000

 Apache installation task 1
    
===============================================================================================================================================

### Now we can add some tests to playbook and test it by running. 

[root@ip-172-31-41-83 mydir]# cat atop.yml
---
task2: "atop"

[root@ip-172-31-41-83 mydir]# cat main.yml
---
- name: " GIT Test"
  hosts: amazon
  become: true
  vars_files:
    - variables.yml
    - atop.yml
  tasks:

    - name: "Installing {{task1}} "
      yum:
        name: "httpd"
        state: present

    - name: "Installing {{task2}} "
      yum:
        name: "atop"
        state: present

    - name: "Restarting/Enabling {{task1}} Webserver."
      service:
        name: httpd
        state: restarted
        enabled: true
        
[root@ip-172-31-41-83 mydir]# git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   main.yml

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        atop.yml

no changes added to commit (use "git add" and/or "git commit -a")

[root@ip-172-31-41-83 mydir]# git ls-files -s
100644 bfba6d595618e2a92a672e323078a047a9ad7ea2 0       main.yml
100644 e4fd69837414c9d1e534e49a55085acdabda8af1 0       variables.yml
[root@ip-172-31-41-83 mydir]# git add .
[root@ip-172-31-41-83 mydir]# git ls-files -s
100644 09a3c8ea5b8f3ec13bac7b2fe3f8069c82534141 0       atop.yml
100644 2b4bcd83ccd9952efa85b19bd058e631b8babc16 0       main.yml
100644 e4fd69837414c9d1e534e49a55085acdabda8af1 0       variables.yml

[root@ip-172-31-41-83 mydir]# git log
commit 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e (HEAD -> master)
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:25:49 2022 +0000

     Apache installation task 1
[root@ip-172-31-41-83 mydir]# git commit -m "atop installation"                   >>> We have commit the task for atop
[master 770f57c] atop installation
 2 files changed, 9 insertions(+), 1 deletion(-)
 create mode 100644 atop.yml
[root@ip-172-31-41-83 mydir]# git log
commit 770f57c62a755d3a661c288d2582fd2052762b9e (HEAD -> master)
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:42:22 2022 +0000

    atop installation                              >>>> The last commit message

commit 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:25:49 2022 +0000

     Apache installation task 1
    
===============================================================================================================================================

If we mistakenly write the commit message, we can use the command ammend to rewrite the last commit we have done. 
***** NB: This can be only done for last commit.


[root@ip-172-31-41-83 mydir]# git commit --amend -m " atop install"     >>>>  ihave changed the last commit message we have done above. 
[master 170e473]  atop install
 Date: Wed Mar 16 10:42:22 2022 +0000
 2 files changed, 9 insertions(+), 1 deletion(-)
 create mode 100644 atop.yml
[root@ip-172-31-41-83 mydir]# git log
commit 170e473ecc34e93b9d22eab1c681397037c76cf2 (HEAD -> master)
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:42:22 2022 +0000

     atop install                                                >>>>>>>>> After change

commit 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:25:49 2022 +0000

     Apache installation task 1
    
===============================================================================================================================================    

#### Adding New tasks - docker installation
[root@ip-172-31-41-83 mydir]# ls -l
total 24
-rw-r--r-- 1 root root  18 Mar 16 10:35 atop.yml
-rw-r--r-- 1 root root  20 Mar 16 10:52 docker.yml
-rw-r--r-- 1 root root  90 Mar 16 09:59 hosts
-rw-r--r-- 1 root root 555 Mar 16 10:51 main.yml
-rw-r--r-- 1 root root  25 Mar 16 10:00 server.pem
-rw-r--r-- 1 root root  20 Mar 16 10:20 variables.yml

[root@ip-172-31-41-83 mydir]# cat docker.yml
---
task3: "docker"
[root@ip-172-31-41-83 mydir]# cat main.yml
---
- name: " GIT Test"
  hosts: amazon
  become: true
  vars_files:
    - variables.yml
    - atop.yml
    - docker.yml
  tasks:

    - name: "Installing {{task1}} "
      yum:
        name: "httpd"
        state: present

    - name: "Installing {{task2}} "
      yum:
        name: "atop"
        state: present

    - name: "Installing {{task3}} "
      yum:
        name: "docker"
        state: present

    - name: "Restarting/Enabling {{task1}} Webserver."
      service:
        name: httpd
        state: restarted
        enabled: true
        
        
Then we will test the playbook using ansible.
>>> Now we can add and commit the contents

root@ip-172-31-41-83 mydir]# git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   main.yml

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        docker.yml

no changes added to commit (use "git add" and/or "git commit -a")
[root@ip-172-31-41-83 mydir]# git add .
[root@ip-172-31-41-83 mydir]# git commit -m "docker added"
[master cd7a38c] docker added
 2 files changed, 10 insertions(+), 2 deletions(-)
 create mode 100644 docker.yml
[root@ip-172-31-41-83 mydir]# git log
commit cd7a38ccc3e8913e88f5f6ffb7ae5eb8a36336a1 (HEAD -> master)
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:54:15 2022 +0000

    docker added

commit 170e473ecc34e93b9d22eab1c681397037c76cf2
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:42:22 2022 +0000

     atop install

commit 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:25:49 2022 +0000

     Apache installation task 1
    
### We can back track the contents

[root@ip-172-31-41-83 mydir]# cat .git/HEAD
ref: refs/heads/master
[root@ip-172-31-41-83 mydir]# cat .git/refs/heads/master
cd7a38ccc3e8913e88f5f6ffb7ae5eb8a36336a1
[root@ip-172-31-41-83 mydir]# git cat-file -p cd7a38ccc3e8913e88f5f6ffb7ae5eb8a36336a1
tree a2160a7ae82f287c313b95e786671d0bef02e1db
parent 170e473ecc34e93b9d22eab1c681397037c76cf2
author jomy <jomy1@gmail.com> 1647428055 +0000
committer jomy <jomy1@gmail.com> 1647428055 +0000

docker added
[root@ip-172-31-41-83 mydir]# git cat-file -p a2160a7ae82f287c313b95e786671d0bef02e1db
100644 blob 09a3c8ea5b8f3ec13bac7b2fe3f8069c82534141    atop.yml
100644 blob da5970c8a39eabf81cdcbd3034574d6aa5f350ca    docker.yml
100644 blob 07e0362a39f55c3c8cee45b47f4d169704c016e2    main.yml
100644 blob e4fd69837414c9d1e534e49a55085acdabda8af1    variables.yml
[root@ip-172-31-41-83 mydir]#

[root@ip-172-31-41-83 mydir]# git cat-file -p 170e473ecc34e93b9d22eab1c681397037c76cf2
tree a4da14683a58c0e7c30864f7d840367145d7004f
parent 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e
author jomy <jomy1@gmail.com> 1647427342 +0000
committer jomy <jomy1@gmail.com> 1647427568 +0000

 atop install
[root@ip-172-31-41-83 mydir]# git cat-file -p 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e
tree fd3d784b20ba23b53e9f50ffe21ea9cee66a8655
author jomy <jomy1@gmail.com> 1647426349 +0000
committer jomy <jomy1@gmail.com> 1647426349 +0000

 Apache installation task 1
[root@ip-172-31-41-83 mydir]#

Now we can add some task to the play book : Adding git and test the playbook.

[root@ip-172-31-41-83 mydir]# cat git.yml
---
task4: "git"
[root@ip-172-31-41-83 mydir]# cat main.yml
---
- name: " GIT Test"
  hosts: amazon
  become: true
  vars_files:
    - variables.yml
    - atop.yml
    - docker.yml
    - git.yml
  tasks:

    - name: "Installing {{task1}} "
      yum:
        name: "httpd"
        state: present

    - name: "Installing {{task2}} "
      yum:
        name: "atop"
        state: present

    - name: "Installing {{task3}} "
      yum:
        name: "docker"
        state: present

    - name: "Installing {{task4}} "
      yum:
        name: "git"
        state: present

    - name: "Restarting/Enabling {{task1}} Webserver."
      service:
        name: httpd
        state: restarted
        enabled: true
        
        
No we can add and commit the contents

[root@ip-172-31-41-83 mydir]# git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   main.yml

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        git.yml

no changes added to commit (use "git add" and/or "git commit -a")
[root@ip-172-31-41-83 mydir]# git add .
[root@ip-172-31-41-83 mydir]# git commit -m "git added"
[master f565c91] git added
 2 files changed, 9 insertions(+), 1 deletion(-)
 create mode 100644 git.yml
[root@ip-172-31-41-83 mydir]# git log --oneline           >>>>  We can see the details is short format. 
f565c91 (HEAD -> master) git added                          >> The last commit
cd7a38c docker added
170e473  atop install                                     >>> This is commit mssg we have chnaged using ammend
164b9f8  Apache installation task 1


===============================================================================================================================================    
#### Default branch

[root@ip-172-31-41-83 mydir]# git branch -v
* master f565c91 git added                         >>>>>>>>>>>>>>>>>> This will show the last commit ID too
[root@ip-172-31-41-83 mydir]# git branch
* master                                              >> The branch
[root@ip-172-31-41-83 mydir]# 

[root@ip-172-31-41-83 mydir]# git log
commit f565c9169ba64ab31ca5af01163a23921f1811b4 (HEAD -> master)
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 11:02:36 2022 +0000

    git added

commit cd7a38ccc3e8913e88f5f6ffb7ae5eb8a36336a1
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:54:15 2022 +0000

    docker added

commit 170e473ecc34e93b9d22eab1c681397037c76cf2
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:42:22 2022 +0000

     atop install

commit 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:25:49 2022 +0000

     Apache installation task 1
    
===============================================================================================================================================    
#### Creating Branch called Mysql

[root@ip-172-31-41-83 mydir]# git branch mysql        >>> Created a new branch called mysql
[root@ip-172-31-41-83 mydir]# git branch          >>> Listed the branches
* master
  mysql
[root@ip-172-31-41-83 mydir]# git branch -v        
* master f565c91 git added
  mysql  f565c91 git added
    
##### How to Changing the default Branch

[root@ip-172-31-41-83 mydir]# git branch mysql
[root@ip-172-31-41-83 mydir]# git branch
* master
  mysql
[root@ip-172-31-41-83 mydir]# git branch -v
* master f565c91 git added
  mysql  f565c91 git added
[root@ip-172-31-41-83 mydir]# git switch mysql       >>. From master to mysql now using "switch"
Switched to branch 'mysql'
[root@ip-172-31-41-83 mydir]# git branch
  master
* mysql
[root@ip-172-31-41-83 mydir]# git branch -v
  master f565c91 git added
* mysql  f565c91 git added                   >>>

[root@ip-172-31-41-83 mydir]# cat .git/HEAD          >>> After the change the head point to mysql branch now
ref: refs/heads/mysql    
===============================================================================================================================================
#### Adding New tasks To install mysql client

cat mysql.yml
---
task5: "mysql"
[root@ip-172-31-41-83 mydir]# cat main.yml
---
- name: " GIT Test"
  hosts: amazon
  become: true
  vars_files:
    - variables.yml
    - atop.yml
    - docker.yml
    - git.yml
    - mysql.yml
  tasks:

    - name: "Installing {{task1}} "
      yum:
        name: "httpd"
        state: present

    - name: "Installing {{task2}} "
      yum:
        name: "atop"
        state: present

    - name: "Installing {{task3}} "
      yum:
        name: "docker"
        state: present

    - name: "Installing {{task4}} "
      yum:
        name: "git"
        state: present

    - name: "Installing {{task5}} "
      yum:
        name: "mysql"
        state: present

[root@ip-172-31-41-83 mydir]# git status
On branch mysql
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   main.yml

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        mysql.yml

no changes added to commit (use "git add" and/or "git commit -a")

[root@ip-172-31-41-83 mydir]# git add .
[root@ip-172-31-41-83 mydir]# git commit -m "Mysql add"
[mysql e3a71bb] Mysql add
 2 files changed, 8 insertions(+), 5 deletions(-)
 create mode 100644 mysql.yml
[root@ip-172-31-41-83 mydir]# git log
commit e3a71bb43f89091b344ddd16de340766ac7f80b0 (HEAD -> mysql)            >>>>  As the brach is mysql. It saved to mysql.
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 11:21:22 2022 +0000

    Mysql add

commit f565c9169ba64ab31ca5af01163a23921f1811b4 (master)
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 11:02:36 2022 +0000

    git added

commit cd7a38ccc3e8913e88f5f6ffb7ae5eb8a36336a1
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:54:15 2022 +0000

    docker added

commit 170e473ecc34e93b9d22eab1c681397037c76cf2
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:42:22 2022 +0000

     atop install

commit 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:25:49 2022 +0000

     Apache installation task 1
    
    
From above git log result. The head is pointig to mysql branch. 
===============================================================================================================================================
#### Now we add some extra task other than from the above and at last we can merge it all

[root@ip-172-31-41-83 mydir]# cat main.yml
---
- name: " GIT Test"
  hosts: amazon
  become: true
  vars_files:
    - variables.yml
    - atop.yml
    - docker.yml
    - git.yml
    - mysql.yml
    -
  tasks:

    - name: "Installing {{task1}} "
      yum:
        name: "httpd"
        state: present

    - name: "Installing {{task2}} "
      yum:
        name: "atop"
        state: present

    - name: "Installing {{task3}} "
      yum:
        name: "docker"
        state: present

    - name: "Installing {{task4}} "
      yum:
        name: "git"
        state: present

    - name: "Installing {{task5}} "
      yum:
        name: "mysql"
        state: present

    - name: "Installing {{task6}} "
      yum:
        name: "php-mysql"
        state: present
[root@ip-172-31-41-83 mydir]# cat phpmysql.yml
---
task6: "php-mysql"
===============================================================================================================================================
### Now we can add the contents to mysql branch

[root@ip-172-31-41-83 mydir]# git status
On branch mysql
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   main.yml

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        phpmysql.yml

no changes added to commit (use "git add" and/or "git commit -a")
[root@ip-172-31-41-83 mydir]# git add .
[root@ip-172-31-41-83 mydir]# git commit -m "phpmysql added"
[mysql dbc00d1] phpmysql added
 2 files changed, 9 insertions(+), 2 deletions(-)
 create mode 100644 phpmysql.yml
[root@ip-172-31-41-83 mydir]# git log
commit dbc00d1b27f0573dc02153d2c82b9706ee7c8950 (HEAD -> mysql) >>>>>>>>>>. Now the last commit is point to head on mysql branch
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 11:40:44 2022 +0000

    phpmysql added

commit e3a71bb43f89091b344ddd16de340766ac7f80b0
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 11:21:22 2022 +0000

    Mysql add

commit f565c9169ba64ab31ca5af01163a23921f1811b4 (master)
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 11:02:36 2022 +0000

    git added

commit cd7a38ccc3e8913e88f5f6ffb7ae5eb8a36336a1
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:54:15 2022 +0000

    docker added

commit 170e473ecc34e93b9d22eab1c681397037c76cf2
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:42:22 2022 +0000

     atop install

commit 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:25:49 2022 +0000

     Apache installation task 1
[root@ip-172-31-41-83 mydir]#


[root@ip-172-31-41-83 mydir]# git log --oneline
dbc00d1 (HEAD -> mysql) phpmysql added
e3a71bb Mysql add
f565c91 (master) git added
cd7a38c docker added
170e473  atop install
164b9f8  Apache installation task 1
[root@ip-172-31-41-83 mydir]#

#### Now we can change the mysql branch to master branch

[root@ip-172-31-41-83 mydir]# git log --oneline
dbc00d1 (HEAD -> mysql) phpmysql added
e3a71bb Mysql add
f565c91 (master) git added
cd7a38c docker added
170e473  atop install
164b9f8  Apache installation task 1
[root@ip-172-31-41-83 mydir]#

[root@ip-172-31-41-83 mydir]# git cat-file -p f565c91           >>> Here is the master branch files saved
tree 72d5876b240f8b69cd4a0f47a7bb19c7abc87383
parent cd7a38ccc3e8913e88f5f6ffb7ae5eb8a36336a1
author jomy <jomy1@gmail.com> 1647428556 +0000
committer jomy <jomy1@gmail.com> 1647428556 +0000

git added
[root@ip-172-31-41-83 mydir]# git cat-file -p 72d5876b240f8b69cd4a0f47a7bb19c7abc87383
100644 blob 09a3c8ea5b8f3ec13bac7b2fe3f8069c82534141    atop.yml
100644 blob da5970c8a39eabf81cdcbd3034574d6aa5f350ca    docker.yml
100644 blob 67f19c472fecefb79354e85297a14c0921bb6b1e    git.yml
100644 blob 3f14da129076d947fbca5f945aaeedac1af44544    main.yml
100644 blob e4fd69837414c9d1e534e49a55085acdabda8af1    variables.yml

===============================================================================================================================================
### Now we can switch the mysql branch to master:

[root@ip-172-31-41-83 mydir]# git switch master
Switched to branch 'master'
[root@ip-172-31-41-83 mydir]# git branch
* master
  mysql
[root@ip-172-31-41-83 mydir]# git branch -v
* master f565c91 git added
  mysql  dbc00d1 phpmysql added
    
    
[root@ip-172-31-41-83 mydir]# cat .git/HEAD
ref: refs/heads/master
[root@ip-172-31-41-83 mydir]# ls
atop.yml  docker.yml  git.yml  hosts  main.yml  server.pem  variables.yml
[root@ip-172-31-41-83 mydir]# ll
total 28
-rw-r--r-- 1 root root  18 Mar 16 10:35 atop.yml
-rw-r--r-- 1 root root  20 Mar 16 10:52 docker.yml
-rw-r--r-- 1 root root  17 Mar 16 11:01 git.yml
-rw-r--r-- 1 root root  90 Mar 16 09:59 hosts
-rw-r--r-- 1 root root 652 Mar 16 11:50 main.yml
-rw-r--r-- 1 root root  25 Mar 16 10:00 server.pem
-rw-r--r-- 1 root root  20 Mar 16 10:20 variables.yml

After the swicth to master the contents we have added on mysql branch will not avaiable. Eg:phpmysql/yml and its task is not there.
[root@ip-172-31-41-83 mydir]# cat main.yml
---
- name: " GIT Test"
  hosts: amazon
  become: true
  vars_files:
    - variables.yml
    - atop.yml
    - docker.yml
    - git.yml
  tasks:

    - name: "Installing {{task1}} "
      yum:
        name: "httpd"
        state: present

    - name: "Installing {{task2}} "
      yum:
        name: "atop"
        state: present

    - name: "Installing {{task3}} "
      yum:
        name: "docker"
        state: present

    - name: "Installing {{task4}} "
      yum:
        name: "git"
        state: present

    - name: "Restarting/Enabling {{task1}} Webserver."
      service:
        name: httpd
        state: restarted
        enabled: true
[root@ip-172-31-41-83 mydir]#
From the git log, there is no relates of mysql and its branch
total 28
-rw-r--r-- 1 root root  18 Mar 16 10:35 atop.yml
-rw-r--r-- 1 root root  20 Mar 16 10:52 docker.yml
-rw-r--r-- 1 root root  17 Mar 16 11:01 git.yml
-rw-r--r-- 1 root root  90 Mar 16 09:59 hosts
-rw-r--r-- 1 root root 652 Mar 16 11:50 main.yml
-rw-r--r-- 1 root root  25 Mar 16 10:00 server.pem
-rw-r--r-- 1 root root  20 Mar 16 10:20 variables.yml

[root@ip-172-31-41-83 mydir]# git log
commit f565c9169ba64ab31ca5af01163a23921f1811b4 (HEAD -> master)
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 11:02:36 2022 +0000

    git added

commit cd7a38ccc3e8913e88f5f6ffb7ae5eb8a36336a1
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:54:15 2022 +0000

    docker added

commit 170e473ecc34e93b9d22eab1c681397037c76cf2
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:42:22 2022 +0000

     atop install

commit 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:25:49 2022 +0000

     Apache installation task 1
    
### If we switch to mysql brach we can see all the details as we already commited before.
===============================================================================================================================================
===============================================================================================================================================

## Merge braches ### ******************************************

- Fast Forward Merge
A fast-forward merge can occur when there is a linear path from the current branch tip to the target branch. Instead of “actually” merging the branches,
all Git has to do to integrate the histories is move (i.e., “fast forward”) the current branch tip up to the target branch tip.

- 3 way Merge , Recursive merge
Recursive is the default merge strategy when pulling or merging one branch. Additionally this can detect and handle merges involving renames, 
but currently cannot make use of detected copies. This is the default merge strategy when pulling or merging one branch.
A three-way merge is where two changesets to one base file are merged as they are applied, as opposed to applying one, then merging the result with the other. 
For example, having two changes where a line is added in the same place could be interpreted as two additions, not a change of one line.

Git merge will combine multiple sequences of commits into one unified history. In the most frequent use cases, git merge is used to combine two branches.
When merging one branch into another git merge will apply all the commits from the branch being merged from to the branch being merged into since the two diverged. 
You can consider it as forming a new head that contains the latest state from both of the branches put together.

[root@ip-172-31-41-83 mydir]# git branch
* master
  mysql
[root@ip-172-31-41-83 mydir]# git merge mysql    >>> This way we can merge the mysql branch to master branch
Updating f565c91..dbc00d1
Fast-forward
 main.yml     | 16 +++++++++++-----
 mysql.yml    |  2 ++
 phpmysql.yml |  2 ++
 3 files changed, 15 insertions(+), 5 deletions(-)
 create mode 100644 mysql.yml
 create mode 100644 phpmysql.yml
[root@ip-172-31-41-83 mydir]# ls -l           >>> All contents is now merged
total 36
-rw-r--r-- 1 root root  18 Mar 16 10:35 atop.yml
-rw-r--r-- 1 root root  20 Mar 16 10:52 docker.yml
-rw-r--r-- 1 root root  17 Mar 16 11:01 git.yml
-rw-r--r-- 1 root root  90 Mar 16 09:59 hosts
-rw-r--r-- 1 root root 735 Mar 16 12:04 main.yml
-rw-r--r-- 1 root root  19 Mar 16 12:04 mysql.yml
-rw-r--r-- 1 root root  23 Mar 16 12:04 phpmysql.yml
-rw-r--r-- 1 root root  25 Mar 16 10:00 server.pem
-rw-r--r-- 1 root root  20 Mar 16 10:20 variables.yml
[root@ip-172-31-41-83 mydir]# cat main.yml
---
- name: " GIT Test"
  hosts: amazon
  become: true
  vars_files:
    - variables.yml
    - atop.yml
    - docker.yml
    - git.yml
    - mysql.yml
    -
  tasks:

    - name: "Installing {{task1}} "
      yum:
        name: "httpd"
        state: present

    - name: "Installing {{task2}} "
      yum:
        name: "atop"
        state: present

    - name: "Installing {{task3}} "
      yum:
        name: "docker"
        state: present

    - name: "Installing {{task4}} "
      yum:
        name: "git"
        state: present

    - name: "Installing {{task5}} "
      yum:
        name: "mysql"
        state: present

    - name: "Installing {{task6}} "
      yum:
        name: "php-mysql"
        state: present
[root@ip-172-31-41-83 mydir]# git log
commit dbc00d1b27f0573dc02153d2c82b9706ee7c8950 (HEAD -> master, mysql)
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 11:40:44 2022 +0000

    phpmysql added

commit e3a71bb43f89091b344ddd16de340766ac7f80b0
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 11:21:22 2022 +0000

    Mysql add

commit f565c9169ba64ab31ca5af01163a23921f1811b4
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 11:02:36 2022 +0000

    git added

commit cd7a38ccc3e8913e88f5f6ffb7ae5eb8a36336a1
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:54:15 2022 +0000

    docker added

commit 170e473ecc34e93b9d22eab1c681397037c76cf2
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:42:22 2022 +0000

     atop install

commit 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:25:49 2022 +0000

     Apache installation task 1
[root@ip-172-31-41-83 mydir]# git branch -D mysql
Deleted branch mysql (was dbc00d1).
[root@ip-172-31-41-83 mydir]# git branch
* master
[root@ip-172-31-41-83 mydir]# git log
commit dbc00d1b27f0573dc02153d2c82b9706ee7c8950 (HEAD -> master)
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 11:40:44 2022 +0000

    phpmysql added

commit e3a71bb43f89091b344ddd16de340766ac7f80b0
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 11:21:22 2022 +0000

    Mysql add

commit f565c9169ba64ab31ca5af01163a23921f1811b4
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 11:02:36 2022 +0000

    git added

commit cd7a38ccc3e8913e88f5f6ffb7ae5eb8a36336a1
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:54:15 2022 +0000

    docker added

commit 170e473ecc34e93b9d22eab1c681397037c76cf2
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:42:22 2022 +0000

     atop install

commit 164b9f8e6b3ce92d4748abceade4ecbf8ee5cc2e
Author: jomy <jomy1@gmail.com>
Date:   Wed Mar 16 10:25:49 2022 +0000

     Apache installation task 1
[root@ip-172-31-41-83 mydir]#
  
  [root@ip-172-31-41-83 mydir]# ls -al
total 40
drwxr-xr-x 3 root root 190 Mar 16 12:04 .
dr-xr-x--- 4 root root 132 Mar 16 10:49 ..
-rw-r--r-- 1 root root  18 Mar 16 10:35 atop.yml
-rw-r--r-- 1 root root  20 Mar 16 10:52 docker.yml
drwxr-xr-x 8 root root 202 Mar 16 12:07 .git
-rw-r--r-- 1 root root  29 Mar 16 10:09 .gitignore
-rw-r--r-- 1 root root  17 Mar 16 11:01 git.yml
-rw-r--r-- 1 root root  90 Mar 16 09:59 hosts
-rw-r--r-- 1 root root 735 Mar 16 12:04 main.yml
-rw-r--r-- 1 root root  19 Mar 16 12:04 mysql.yml
-rw-r--r-- 1 root root  23 Mar 16 12:04 phpmysql.yml
-rw-r--r-- 1 root root  25 Mar 16 10:00 server.pem
-rw-r--r-- 1 root root  20 Mar 16 10:20 variables.yml
