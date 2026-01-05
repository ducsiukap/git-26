# GIT - a Distributed Version Control System (DVCS).

> Git là một hệ thống quản lý phiên bản phân tán, vận hành dựa trên cơ chế Snapshots, sử dụng thuật toán băm SHA-1 để đảm bảo tính toàn vẹn dữ liệu, và quản lý lịch sử thông qua cấu trúc dữ liệu Directed Acyclic Graph (DAG) (Đồ thị có hướng không chu trình) của các Commit objects.

---

## Working area

- WD (Working Directory): thư mục làm việc hiện tại trên máy local.
  > WD là phiên bản hiện tại của Repo trên máy.
- SA (Staging Area): khu vực lưu trữ tạm thời trên máy local.
  > Stage: là khu vực đói gói => lựa chọn các file để lưu lai, sẵn sàng chờ được `commit`.  
  > Sử dụng `git add` để đưa file vào stage.
- Repository: lưu trữ vĩnh viên các thay đổi dưới dạng các `commit`.
  > Bao gồm: local repository (repo tại máy local) và remote repository (lưu trên github).

---

## Install git

Download git at: https://git-scm.com/install/

> To check the installation:
>
> 1. Open the `git bash`, `cmd` or `powershell`
> 2. Type: `git --version` and run.

---

## Authentication methods

Cần xác thực người dùng khi:

- `push` code
- `clone`/`pull` _*private repo*_

_Mục đích: xác thực người dùng có quyền gì với repo?_

### **Có 03 cách xác thực:**

> Using `SSH Authentication`

Đa số trường hợp sử dụng github đều dùng `SSH auth`  
Cách config ban đầu:

```bash
# Step 1: tạo key ở local
ssh-keygen -t ed25519 -C "your-string"
# ex: ssh-keygen -t ed25519 -C "vduczz@personal-laptop"

# Step 2: copy public key:
cat ~/.ssh/id_e25519.pub

# Step 3: tạo SSH key trên github
# 3.1: github.com -> Account -> Settings -> SSH and GPG keys.
# 3.2: click 'New SSH Key'
# 3.3: đặt Title tùy ý, key=public_key đã copy.
# 3.4: click "Add SSH Key"

# Step 4: test kết nối
ssh -T git@github.com


# ===============================
# *note: quản lí nhiều tài khoản với SSH Config.
# usecase: 2 account => word + personal

# Step 1: create 2 keys for 2 account:
ssh-keygen -t ed25519 -C "vduczz@personal" -f ~/.ssh/id_e25519_personal
ssh-keygen -t ed25519 -C "vduczz@work" -f ~/.ssh/id_e25519_work

# Step 2: chạy và thêm keys vào SSH-agent
# run SSH Agent
eval "$(ssh-agent -s)"
# add keys
ssh-add ~/.ssh/id_e25519_personal
ssh-add ~/.ssh/id_e25519_work

# Step 3: tạo SSH key trên github
# thực hiện lần lượt với account cá nhân và account work
# Step 2 + 3 bên trên,
cat ~/.ssh/id_ed25519_personal.pub # then, add to personal account on github
cat ~/.ssh/id_ed25519_work.pub # -> add to work account

# Step 4: tạo file config
# 4.1: tạo/mở file config
touch ~/.ssh/config
notepad ~/.ssh/config
# 4.2: paste to config file:
# personal accoutn
Host github.com # alias
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal
# work account
Host github-work # alias
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work
```

Định dạng repo: `git@<host>:<owner>/<repo>.git`

- host: `Host` trong file config hoặc mặc định là _`github.com`_
- owner: repo's owner
- repo: repo's name

_\*Note:_

- Mỗi account cần 1 SSH key riêng biệt.
- Chuyển máy => tạo SSH key mới.

> Using `HTTPS Authentication`

Xác thực bằng `username` + `PAT` (personal access token)  
`GCM` (Git Credential Manager) đóng vai trò trung gian, quản lý Token để không cần nhập lại mỗi lần thao tác.  
Trường hợp sử dụng:

- beginers
- bị chặn Port 22.
- truy cập tạm trên máy lạ

Cách config ban đầu:

```bash
# Cách 1: sử dụng OAuth - đăng nhập qua trình duyệt
# khi sử dụng lệnh
git push ... # or git clone ...
# cửa sổ bật lên => "Sign in with your browser"
# => Login => Xong!

# ===========================================
# Cách 2: Sử dụng PAT - Personal Access Token

# Step 1: tạo PAT
# 1.1: github.com > Account > Settings > Developer settings
# 1.2: Personal Access Token > Fine-gained tokens > Generate new token
# 1.3: Điền Token name, chọn Expiration, chọn quyền > Generate token.
# 1.4: copy chuỗi mã PAT (NOTE: CHỈ HIỆN DUY NHÂT 1 LẦN)

# Step2: config
# chạy lệnh
git push ... # or git clone ...
# 2.1: nhập username -> username của account
# 2.2: nhập password -> PAT vừa copy
```

> Using `SSH + gh`

---

## Cấu hình user

_Cấu hình user giúp gắn commit với tài khoản_

### Kiểm tra config

`git config --list`  
hoặc

```bash
git config user.name
git config user.email
```

### Set GLOBAL user => user mặc định trên máy

```bash
git config --global user.name 'vduczz'
git config --global user.email 'your_github_email@gmail.com'
```

### Set LOCAL user => áp dụng cho repo hiện tại được set

```bash
cd your-repo

git config user.name 'vduczz'
git config user.email 'github_email@gmail.com'
```
