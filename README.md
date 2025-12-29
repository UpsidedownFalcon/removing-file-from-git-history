# removing-file-from-git-history 

This is a compilation of various online sources to completely remove a file from the commit history on git and GitHub. Any time you see `unwanted-file` in the following commands, **replace it with your own** unwanted file's name **with its extension**. 

## 0) **IDEALLY** have a `.gitignore` and add the `unwanted-file` to it **before the first commit**. But that hasn't happened, sooooo....

## 1) Currently, the `unwanted-file` has been committed and exists (i.e has not been deleted).

So after `git push` you'll see this `unwanted-file` in the commit history of your remote repository. Our goal is to get rid of this `unwanted-file` from the entire commit history. 

## 2) But after stopping git from tracking the file:

```
  git rm --cached unwanted-file
```

You'll see after 

```
  git commit -m messagae
  git push
```

That `unwanted-file` is still found in your remote repository's commit history in all commits before the latest commit with "message" 

## 3) Let's see all occurrences of the file's path in the commit history:

```
  git log --follow -- unwanted-file
```

And we want to eventually run this command and not see the file anywhere in the commit history

## 4) So let's forcefully remove the `unwanted-file` from the commit history

```
  git filter-branch --tree-filter 'rm -f unwanted-file' HEAD
```

It's normal for it to say that a lot of refs were rewritten (this is expected). **NOW WOULD BE A GOOD TIME TO ADD** `unwanted-file` **TO YOUR** `.gitignore` as mentioned in step 0. At this point in time, `unwanted-file` has been removed from all git commit history locally, and no other file changes have been made, so this should be force pushed onto your remote repository: 

```
  git push --force
```

Now, when you check any commits either on git or GitHub, `unwanted-file` won't be there 

## 5) Double check with `git log --follow -- unwanted-file` and it will come back blank

## 6) Remove all refs from the remote repository's commit history with:

```
  git for-each-ref --format="%{refname}" refs/original/ | xargs -n 1 git update-ref -d
```

No need to change anything in the command above ^ 

## 7) And lastly run
   
```
  git filter-branch --index-filter 'git rm -- cached --ignore-unmatch unwanted-file' HEAD
```

In case the first ```git rm``` wasn't run properly 



---



Original credit to [git-filter-repo by newren](https://github.com/newren/git-filter-repo) 
