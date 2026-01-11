# Branching, merging and collaboration.

## Branching & merging

### Why need?

> Branch tồn tại để: **làm việc độc lập, an toàn, song song và không phá code ổn định**

`main` (hoặc `master`): là nhánh chính, chứa code ổn định của project.  
Các branch khác để:

- creates new feature
- bugs fixes
- thử nghiệm mà không ảnh hưởng main.

### To list all branches:

> `git branch`

### To create/switch to another branch:

- create an branch:

  > `git branch new-branch`

  note: "<span style="color: lightgreen">**\***</span>" for current branch

- to switch to another branch:
  > `git checkout other-branch`, or  
  > `git switch other-branch`
- to create & switch:
  > `git checkout -b new-branch`, or  
  > `git switch -C new-branch`
- to delete a branch:
  > `git branch -d other-branch`

### Comparing branches:

- to list all commits in branch A and not in branch main:
  > `git log main..A`
- show the summary of changes:
  > `git diff main..A`

### Stashing

> **Stashing**: create and save a temporary snapshot of current branch. _*<text style="color:red">if current branchs is changed but not be committed or stashed, you can not switch (checkout) to other branch.</text>*_

- to stash all changes:

  > `git stash`, or  
  > `git stash push`,  
  > or naming for stash: `git stash push -m 'stash's message'`

  by default, `git stash` not stashes new file. To stash new file using `-u`:  
  ex: git stash -u

  or stash partial:

  > `git stash -p`

- to list all stash:

  > `git stash list`

  syntax: stash@{\<stash-order>}: on \<branch-name>: \<message>

- show the given stash:

  summary:

  > `git stash show stash@{<stash-order>}`, or  
  > `git stash show <stash-order>`

  add `-p` to show the detail.

  ex: git show stash@{0}

- apply a stash:

  > `git stash apply stash@{<stash-order>}`, or  
  > `git stash apply <stash-order>`

- delete give stash:

  > `git stash drop stash@{<stash-order>}`, or  
  > `git stash drop <stash-order>`

  or deletes all stash:

  > `git stash clear`

- apply + delete:
  > `git stash pop stash@{<stash-order>}`

> in case there is no <text style="color: yellow">stash@{\<stash-order>}</text> is used, <text style="color: yellow">stash@{0}</text> is default for `apply`, `drop`, `pop`, `show`, `branch`,.. commands.

### Merging

- to merge branch _new-feature_ to current branch:

  > `git merge new-feature`

  Các trường hợp khi merge: // giả sử merge `new-feature` vào `main`

  - **Fast-forward merge**:
    - `main` chưa có commit mới
    - `new-feature` được phát triển tiếp từ main, và có commit mới
    ```text
        main
          |
     (new-feature) ──> A ──> B
                             ↑
    ```
    => Git đẩy con trỏ `main` tiến lên commit B. Có thể merge mà không cần merge commit.  
    Để ép bổ sung merge commit kể cả khi có thể FF:
    > `git merge --no-ff new-feature`
  - **Merge commit**:

    - Xảy ra khi cả `main` và `new-feature` đều có các commit mới sau khi tách nhánh (của riêng nhánh đó).

    ```text
        main ── A ── merge-commit
          |     ↑     /
          |          /
    (new-feature)── B
                    ↑
    ```

    => Bắt buộc có merge commit.

- Để gộp toàn bộ commit từ `new-feature` thành 1 commit duy nhất, sau đó merge vào nhánh hiện tại, sử dụng:

  > `git merge --squash new-feature`

  Khi này, chỉ bổ sung merge commit vào nhánh hiện tại thay vì toàn bộ commit của `new-feature` + merge commit.

- **Merge conflict handle**:

  1. Open the file that includes the conflict.
  2. Review & decision which will be retained and delete the marker:

  ```text
  <<<<<<< HEAD
  (code A) // the current file
  =======
  (code B) // the change in new-feature
  >>>>>>> feature-login

  ```

  3. `git add` + `git commit`.

- To cancel an ongoing merge:

  > `git merge --abort`

  Condition:

  - Merging is in progress.
  - Merge not yet complete.  
    (Usually experiencing conflicts)

### View the merged branches:

- to list all branches that have been merged into current branch:
  > `git branch --merged`
- to list all branches that have not been merged into current branch:
  > `git branch --no-merged`

### Rebasing

> Rebase = rewrite the commit history.

Để rebase từ `main` vào `feature`

> ```text
> git checkout feature
> git rebase main
> ```

=> đặt tất cả lịch sử commit (mới) của nhánh hiện tại (feature) lên đầu nhánh khác (main).
Ex: trước rebase:

```text
main ── A ── B
         \
          C ── D   (feature)
```

rebasing:

```bash
git checkout feature
git rebase main
```

sau rebase:

```text
main ── A ── B ── C' ── D' (feature)
```

Lúc này, feature được viết lại lịch sử commit của nó, không ảnh hưởng tới main.  
 Khi có conflict lúc rebase:

- tiếp tục rebase:
  > `git rebase --continue` // \_sau khi đã handle conflict
- bỏ qua rebase:
  > `git rebase --abort` //_quay về trước khi bắt đầu rebase_

### Cherry-picking

> Cherry-pick = rebase có chọn lọc.  
> _Lấy 1 commit từ nhánh khác (bất kì nhánh nào) viết lên đầu branch hiện tại (tạo commit mới)_

- to using cherry pick:

  > `git cherry-pick <commit-hash>`
  > //_commit-hash của bất kỳ commit nào trong repo(ở bất kỳ thời điểm nào, của bất kỳ branch nào)_

Ex:

- trước cherry-pick:

  ```text
  main ── A ── B
  feature ── C
  ```

- cherry-picking

  ```text
  git checkout feature
  git cherry-pick B
  ```

  // B là hash của commit bất kì ở bất kì thời điểm nào của bất kì nhánh nào.

- sau cherry-pick

  ```text
  feature ── C ── B'
  ```

---

## Collaboration

### Clone a repository:

To clone a repo on Github:

> `git clone url`

note: \<url> for SSH Authentication is different from \<url> for HTTPS Authentication.

### Syncing with remotes:

#### 1. Fetching

> fetching: update `origin/*` or `origin/branch` on local machine to match the latest version on remote (Github)
>
> \*note: `fetch` only fetch the updates to `origin`, no-change in working directory are occur.

To fetch all branch or specified branch from remote:

> `git fetch origin`, or  
> `git fetch origin branch`

After fetching:

- view the commit history on remote:
  > `git log origin/main`, or  
  > `git log main..origin/main`
- show the differences:
  > `git diff main origin/main`
- merge code:
  - create merge commit:
    > `git merge origin/main`
  - or, rebase: (no merge commit is created)
    > `git rebase origin/main`  
    > _**note**_: using `rebase` only if never ever push code to remote.  
    > <text style="color:red">Vì khi đã push => commit (A) thành lịch sử chung => rebase thay thế commit đó bằng commit khác (A') => người khác cùng phát triển nhưng lại dùng trên commit cũ (A) => họ không thể push code mới code parent commit=A.</text>  
    > <text style="color:green">Ngược lại, khi chưa push, commit vẫn nằm ở local => rebase + push chỉ push lên commit sau cùng.</text>  
    > <text style="color:yellow">Rebase chỉ nguy hiểm khi có người khác đã dựa vào commit đó.</text>

#### 2. Pulling

> `pull` calls `fetch` to fetch the updates. Then, executes `merge` to merge new code to working directory.  
> `pull` = `fetch` + `merge`

To using `git pull`:

> `git pull` = `git pull origin`, or  
> `git pull origin feature`

To fetch + rebase:

> `git pull --rebase`

### Sharing tags, branches

#### Sharing tags

> Tag is a static pointer that used for relase, version or important milestone,..  
> Tag can be used as a branch.  
> Tag is not push automatically.

- To push `tags`:

  > `git push origin <tag-name>`, or  
  > `git push origin --tags`// push all tags

- To fetch `tags` from remote to local:
  > `git fetch --tags`

#### Sharing branches:

- show remote tracking branches:
  > `git branch -r`
- show local + remote tracking branches:
  > `git branch -vv`
- push branch to remote:
  > `git push -u origin branch-name`

#### Delete tag or branch

to delete a reference (tag, branch) that's name is "label":

> `git push origin --delete label`, or  
> `git push -d origin label`

### Managing remote

- To list all remote repos:

  > `git remote` // for summary  
  > `git remote -v` // for more details

- To view a specified remote repo:

  > `git remote show <alias>`

- Add new remote:

  > `git remote add <alias> <url>`  
  > **note**:
  >
  > - `origin`: thường là repo chính (update thường xuyên), fork (nếu fork).
  > - `upstream`: thường là repo gốc (nếu fork).
  > - `backup`/`mirror`: remote để backup code.
  > - `personal`/`team`/`company`

- Delete a remote:

  > `git remote rm <alias>`

- Renaming:

  > `git remote rename <old-alias> <new-alias>`

- Update URL:
  > `git remote set-url <alias> <url>`
