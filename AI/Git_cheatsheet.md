git config user.email 
Check the email

##### Error: Cannot push
Maybe there are some troubles on the account setting. We may set the email account which is blocked.

#### Cancel commit
```

# check version of each commit
$ git log
 
 
# reset last version
$ git reset HEAD^ 
 
# reset to the state of the previous three commit
$ git reset HEAD~3  
    
# go back to the specific version based on commit_id
$ git reset commit_id    
 

根据–soft –mixed –hard，会对working tree和index和HEAD进行重置:
git reset –mixed：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息
git reset –soft：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可
git reset –hard：彻底回退到某个版本，本地的源码也会变为上一个版本的内容     
```

#### git checkout
It can resolve the issue named "already up-to-date" when you pull the code from the remote.
Ref: 
https://blog.csdn.net/heguixian/article/details/51029026