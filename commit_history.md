# Browsing & rewriting commit history

## Browsing history

### Viewing history #using `git log`

- list all commit history (+ all modified files each commit)
  > `git log --stat`
- show all actual changes:
  > `git log -p`, or  
  > `git log --patch`
- or using:

  > `git log --oneline --graph --all --decorate`

  where:

  - `oneline`: log oneline
  - `graph`: log branch as bgraph
  - `all`: show all branch
  - `decorate`: show branch-name + tag that attached to commit.

### Formatting log output:

> `git log --pretty=format:"pattern"`

where, `pattern` can include:

- `%h`, `%H`: short hash / full hash.
- `%an`, `%ae`, `%ad`: author's name, author's email, commit's date.
- `%s`: commit message.
- `%d`: branch/tag's name.
- `%n`: new line.

### Filtering history

- show last-n commits:
  > `git log -n`
- range of commits:
  > `git log hash1..hash2`
- filter by:
  - author: `git log --author="Vduczz"`
  - date: `git log --before="2026-1-1"`, `git log --after="one week ago"`, ...
  - message text: `git log --grep="docs"` // commits that includes "docs" in its message.
  - file: `git log file.ext`
  - code content: `git log -S "function_name"`

### Creating an alias:

`git config --global alias.lg "log --oneline"`

> then, `git lg` == `git log --oneline`

### Viewing commit

- to show the n-latest commit:

  > `git show HEAD~n`

  \*note:

  - press `q` + `enter` to quit.
  - n starts from 0.

- show file in commit:
  > `git show HEAD~n:file.ext`

### Comparing commits:

- changes between 2 commits:
  > `git diff HEAD~n HEAD`
- change in file only:
  > `git diff HEAD~n HEAD file.ext`

### Checkout a commit:

- checkout a commit likely a branch:
  > `git checkout hash`
  >
  > _note_: để chỉnh sửa và push trên 1 commit đã cũ => tạo nhánh mới để tránh `Detached HEAD`.

### Finding a bad commit:

Sử dụng thuật toán `Binary search` để khoanh vùng commit gây lỗi.

> Condition: have to a good commit in the past.

Quá trình truy vết sử dụng `bisect`:

- Bắt đầu quá trình truy vết:
  > `git bisect start`
- Đánh dấu commit tốt nào đó trong quá khư (càng mới càng tốt, càng nhanh):
  > `git bisect good hash`
- Đánh dấu commit xấu:
  > `git bisect bad <hash>` // hash=HEAD for default.

=> Vòng lặp:

- Checkout vào commit ở giữa `good` và `bad`:
  - Nếu vẫn lỗi: => `git bisect bad`
  - Nếu ok: => `git bisect good`  
    => new good / new bad.

### Finding contributors.

> `git shortlog` // details  
> `git shortlog -s -n` // summary => -s: number of commit, -n: sort by -s (decending)

### Finding the author of lines:

> `git blame file.ext`, or  
> `git blame <commit> file.ext`

### Tagging

- Tags a commit:
  > `git tag <tag-name> <commit>` // default `commit` is HEAD.
- Lists all tags:
  > `git tag`
- Deletes a tag:
  > `git tag -d <tag-name>`

---

# Rewriting history

### <text style="color:red">Golden principle: Only edit the commit history on local branch (not push to remote ever)</text>

To push code to remote :)

> `git push --force`  
> or more safety:  
> `git push --force-with-release` // check if there is no new commit on branch

### Edit the latest commit:

1. Add an uncommitted file:
   > `git add forgotten_file.ext`
2. Open editor to edit commit:
   > `git commit --amend`

or, only change the commit message:

> `git commit --amend -m "new-message"`

### Interactive rebasing

> Combine last-n commit into only one commit before merging it into `main`.
>
> `git rebase -i HEAD~n`
>
> then:
>
> - `pick`: keep last commit.
> - `rework`: enable edit last commit.
> - `squash` (or `s`): combine to above commit (behind it)
> - `drop`: delete commit and code :>

### Undoing commits - `reset`:

- Back to the past, keep all changes in `working directory + staging`:
  > `git reset --soft HEAD~n`
- Back to the past, keep all changes in `working directory`: // default of git reset command.
  > `git reset --mixed HEAD~n`
- Remove all change in `wd + staging`, move HEAD to the specified commit.
  > `git reset --hard HEAD~n`

### Reverting commits:

> Using when edit the history of branch (commit) that pushed to remote.

To revert a earlier:

> `git revert <hash>`  
> Tạo commit mới, hủy bỏ toàn bộ thay đổi ở commit được chỉ đinh
>
> ```text
> before revert:
> A -- B -- C -- D (HEAD)
>
> reverting:
> git revert B
>
> after revert
> A -- B -- C -- D -- B' (HEAD)
> ```
>
> now, B' remove all change in commit B, but keep all change in A, C, D. So, B' can be considered as B' = (A + C + D)

Revert multi-commits:

> `git revert <hash1>..<hash2>` // hash2 is HEAD by default.

Revert but not create revert-commit immediately:

> `git revert --no-commit <hash1>[..<hash2>]`  
> <text style="color:lightgreen">**Recommended!!!**</text>

### Recovering lost commits:

> `reflog` - reference logs - record every change of HEAD pointer in repo: checkout, reset, rebase, commit, ... everything.  
> `reflog` chỉ lưu tại local, có thời hạn sống tối đa 90 ngày (hoặc 30 ngày với những commit không thể truy cập).

- To show the history of HEAD:

  > `git reflog`  
  >  Example result:  
  > ea32b10 (HEAD -> master) HEAD@{0}: reset: moving to HEAD~1  
  > 4a1f390 HEAD@{1}: commit: Them tinh nang dang nhap  
  > c92a110 HEAD@{2}: checkout: moving from develop to master

  Then, analysis the reflog. To restore the HEAD pointer to an spot in the past:

  > `git reset <reflog-hash>`, or  
  > `git reset HEAD@{n}`

- Show the history of a branch pointer:
  > `git reflog show branch`
