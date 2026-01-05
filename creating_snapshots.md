# Working area

- Working Directory: current version of repo in local machine.
- Staging Area: a container includes the files that is ready to be committed.
- Repo: remote repository on github (public repo).

> _Principle: "Once you publish a commit, it becomes part of history. Don’t change public history."_  
> => Never rewrite public commit history.

---

# Creating snapshots

> ### _init repo_

`git init`

> ### _viewing status_

Full status: `git status`  
Short status: `git status -s`

> ### _removing files_

Remove from WD and SA: `git rm file.js`  
Remove only in SA: `git rm --cached file.js`

> ### _renaming/ moving files_

Rename: `git mv file.js renamedFile.java`  
Remove: `git mv path/to/file.js path/to/newFile.js`

> ### _staging file_

`git add <regex>`, where regex can be:

- single file: `git add file.js`
- multiple files: `git add file1.js file2.java file3`.
- pattern: `git add *.js`
- all files: `<regex> = *` or `<regex> = .`

> ### _committing_

Commit the stagged files: `git commit -m "Message"`  
Add the untracked files and commit: `git commit -am "Message"` = `git add -u` + `git commit -m "Message"`

> ### _Viewing the differences_

Different between **Working Directory** and **Staging**: `git diff`  
Different between **Staging** vs **Repositoy**: `git diff --staged`  
Different between **WD** vs **Repo**: `git diff HEAD`

> ### _Logging the Repo history_

Full history (Hash, Author, Date, Message): `git log`  
Summary log: `git log --oneline`  
Graph log (branching diagram): `git log --graph`  
=> Overview: `git log --oneline --graph --all`

Filtering:

- last N commit: `git log -n N`
- filter by author: `git log --author="vduczz"`
- filter by text included in commit message: `git log --grep="login"`
- filter by file's name: `git log -- file.js`
- filter by file's content: `git log -S "function checkLogin()"`

> ### _Viewing commit_

`git show <commit>` = `git log` (at _commit_)+ `git diff` (between _commit_ and one older commit).  
Where, `<commit>` can be:

- `commit hash`
- `HEAD` or `HEAD~1` for latest commit.
- `HEAD~n`: the n-th latest commit.

View file's content at commit \<hash>: `git show <hash>:filename.ext`

> ### _Restoring files_

Copy `file.js` from _**Staging**_ into _**WD**_: `git restore file.js` // CHANGE THE FILE IN WD BY THE FILE IN STAGING AREA  
Unstagged file: `git restore --staged file.js` // file.js IN WD WILL NOT BE CHANGED, only status of its be changed to _modified_
Copy file.js from older commit into file.js in current WD: `git restore --source=HEAD~2 file.js`
