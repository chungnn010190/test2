# 🧰 Cài Node.js

## Cài Node.js LTS (khuyến nghị Node 20)

```powershell bash
winget install CoreyButler.NVMforWindows
nvm install 20.12.2
nvm use 20.12.2
node -v
```

## Kích hoạt Yarn (theo repo có yarn.lock)

```powershell bash
npm install -g yarn
yarn -v
```

# 🔐 Cấu hình Git, SSH và GitLab

## Cài Git + OpenSSH

```powershell bash
winget install Git.Git
git --version
```

- Thiết lập Git cơ bản (khuyên dùng cho FE dự án này)

```powershell bash
git config --global user.name "Nguyen Van A"
git config --global user.email "nguyenvana@alta.com.vn"
```

- Sau đó bạn có thể kiểm tra lại bằng:

```powershell bash
git config --list
```

## Kiểm tra OpenSSH

```powershell powershell
# Kiểm tra có ssh chưa
where ssh

# Mở PowerShell với quyền Administrator rồi chạy:
# Nếu chưa có, cài OpenSSH Client (Windows Feature)
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

## Tạo SSH key (khuyến nghị ed25519)

```powershell bash
# Tạo key mới, thay email bằng email GitLab của bạn
# Đường dẫn mặc định: C:\Users\<you>\.ssh\id_ed25519 (private) và id_ed25519.pub (public)
# Gợi ý: đặt passphrase cho key để tăng bảo mật.
ssh-keygen -t ed25519 -C "nguyenvana@alta.com.vn"
```

## Khởi động ssh-agent & nạp key

```powershell bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

## Copy public key và thêm vào GitLab

```powershell bash
# Hiện nội dung public key để copy
cat ~/.ssh/id_ed25519.pub
```

- Vào **GitLab** → **Edit profile** → **SSH Keys**.
- Dán nội dung <font color="#188038">id_ed25519.pub</font> vào, đặt **Title**, chọn **Expiration** (nếu cần), **Add key**.

## Kiểm tra kết nối SSH

```powershell bash
# GitLab nội bộ (ví dụ)
ssh -T https://gitam2.altamedia.vn/
```

## Clone repo bằng SSH (hoặc đổi remote từ HTTPS → SSH)

- Clone mới:

```powershell bash
git clone git@gitam2.altamedia.vn:<group>/<repo>.git
cd <repo>
```

- Đổi origin đang dùng HTTPS sang SSH:

```powershell bash
git remote -v
git remote set-url origin git@gitam2.altamedia.vn:<group>/<repo>.git
git remote -v
```

## Troubleshooting nhanh

- Permission denied (publickey)
  - Chưa add key vào agent → <font color="#188038">ssh-add ~/.ssh/id_ed25519</font>
  - Thêm **nhầm** public key lên GitLab → kiểm tra lại .pub
  - Remote vẫn là **HTTPS** → đổi sang SSH
- Nhiều key xung đột
  - Dùng <font color="#188038">~/.ssh/config</font> với <font color="#188038">Host</font> khớp domain, <font color="#188038">set IdentitiesOnly yes</font> và <font color="#188038">IdentityFile</font> đúng.
- Agent không chạy / không nhận key
  - PowerShell: bật service. Git Bash: <font color="#188038">eval "$(ssh-agent -s)"</font>
  - Xóa nạp lại key: <font color="#188038">ssh-add -D</font> rồi <font color="#188038"><font color="#188038">~/.ssh/config</font></font>
- Self-host GitLab cổng lạ (không phải 22)
  - Khai báo <font color="#188038">Port</font> trong <font color="#188038">~/.ssh/config</font>
- Mạng chặn cổng 22
  - Trao đổi IT (hoặc nếu GitLab tổ chức hỗ trợ **SSH over 443**, dùng host/cấu hình tương ứng của công ty)

# ⚙️ Cài đặt Visual Studio Code và Extensions cần thiết

## Cài đặt Visual Studio Code

```powershell bash
winget install --id Microsoft.VisualStudioCode
```

## Extensions khuyến nghị cho dự án

- Mở VS Code → cài “code” CLI (Command Palette → “Shell Command: Install 'code' command in PATH”), rồi chạy lệnh dưới đây trong PowerShell/CMD
- Bắt buộc / nên có

```powershell bash
# ESLint
code --install-extension dbaeumer.vscode-eslint

# Prettier – Code formatter
code --install-extension esbenp.prettier-vscode

# GitLens — Git supercharged
code --install-extension eamodio.gitlens

# Code Spell Checker
code --install-extension streetsidesoftware.code-spell-checker

# Tailwind CSS IntelliSense
code --install-extension bradlc.vscode-tailwindcss

# Error Lens
code --install-extension EditorConfig.EditorConfig
```

- Theo nhu cầu dự án

```powershell bash
# Blueprint – New Files and Folders from Templates
code --install-extension teamchilla.blueprint

# Microsoft Edge Tools for VS Code
code --install-extension ms-edgedevtools.vscode-edge-devtools

# Paste JSON as Code
code --install-extension quicktype.quicktype

# Vscode Google Translate
code --install-extension funkyremi.vscode-google-translate

# TypeScript Import Sorter
code --install-extension nsoult.typescript-imports-sort

# Docker (nếu dùng Docker)
code --install-extension ms-azuretools.vscode-docker
```

- Tuỳ chọn (thẩm mỹ)

```powershell bash
# Theme: Dracula Official (optional)
code --install-extension dracula-theme.theme-dracula

# Icon pack: vscode-icons (optional)
code --install-extension vscode-icons-team.vscode-icons

# Icon pack: Material Icon Theme
code --install-extension PKief.material-icon-theme
```

## (Tùy chọn) Docker Desktop để verify image local

```powershell bash
winget install Docker.DockerDesktop
```

# 🌿 Quy trình làm việc với Git & Git Flow

## Kiểm tra & khởi tạo

```powershell bash
# kiểm tra đã cài git-flow chưa
git flow

# khởi tạo lần đầu
# main branch: main
# develop branch: develop
# Các prefix giữ mặc định: feature/, release/, hotfix/, support/
git flow init
```

## Feature flow (hàng ngày)

```Text text
Quy ước tên nhánh: feature/<scope-ngắn> (kebab-case)
```

- Ví dụ: <font color="#188038">feature/dashboard-pagination</font>

```powershell bash
# Tạo nhánh làm việc từ develop
git flow feature start dashboard-pagination

# Đẩy nhánh lên remote để mở MR / làm việc cùng team
# Viết ngắn: git flow publish
git flow feature publish dashboard-pagination

git add -A
git commit -m "feat(Dashboard): build Dashboard component"

# (Trong quá trình dev)
# Khi xong, mở MR từ feature → develop trên GitLab (khuyến nghị)
git push

# merge vào develop & xoá nhánh local
# Viết ngắn: git flow finish
git flow feature finish dashboard-pagination

git push origin develop

# xoá nhánh remote (nếu muốn)
git push origin feature/dashboard-pagination
```

- **Khuyến nghị**: Dùng **publish + MR** thay vì <font color="#188038">finish</font> để đảm bảo review/CI trên GitLab. Pipeline <font color="#188038">.gitlab-ci.yml</font> sẽ chạy khi bạn **push** hoặc **mở MR**.

## Hotfix flow (sửa lỗi khẩn cấp trên production)

```Text text
Quy ước tên nhánh: hotfix/<version> hoặc hotfix/<slug>, ví dụ: hotfix/1.4.1
```

```powershell bash
# tạo nhánh từ main
git flow hotfix start 1.4.1

# commit sửa lỗi
git commit -m "fix(Dashboard): fix pagination"

# có thể publish để chạy CI/QA nhanh
git flow hotfix publish 1.4.1

git flow hotfix finish 1.4.1

# Hoàn tất: merge vào main & develop, tạo tag
git push origin main develop --follow-tags
```

## Quy tắc & lưu ý

- **main** (bảo vệ): luôn là trạng thái phát hành; chỉ nhận merge từ **hotfix**.
- **develop**: tích hợp tính năng; feature luôn merge vào develop.
- **Review & CI**: MR phải xanh (lint/typecheck/test) trước khi merge.
- **Commit message**: theo convention bạn đã chốt, **scope = tên component/module**, ví dụ:
  - <font color="#188038">feat(Dashboard): build Dashboard component</font>
  - <font color="#188038">fix(Dashboard): fix pagination</font>
- **Đồng bộ trước khi start/finish**:

```powershell bash
git checkout develop
git pull

git checkout main
git pull
```

- **Xử lý conflict khi finish**: git-flow sẽ dừng để bạn resolve; sau khi resolve & commit, chạy lại lệnh <font color="#188038">finish</font>.

## Tóm tắt lệnh hay dùng

```powershell bash
# Feature
git flow feature start <name>
git flow feature publish <name>
git flow feature finish <name>

# Hotfix
git flow hotfix start <version|slug>
git flow hotfix publish <version|slug>
git flow hotfix finish <version|slug>
```

# 🚀 Khởi chạy dự án

## Nguyên tắc với Vite

- Biến dùng trong **frontend** phải **bắt đầu bằng** <font color="#188038">VITE\_</font> (ví dụ <font color="#188038">VITE_API_BASE_URL</font>).
- Vite “nhúng” biến vào bundle **tại thời điểm build** → đổi <font color="#188038">.env</font> cần **build lại**.
- Hạn chế tối đa secrets ở FE (token, private key) — **không an toàn**.

## Nguyên tắc với NextJS

- Biến dùng trong **frontend** phải **bắt đầu bằng** <font color="#188038">NEXT*PUBLIC*</font> (ví dụ <font color="#188038">NEXT_PUBLIC_API_BASE_URL</font>).
- Biến env trong Next.js được inject tại **thời điểm build**.
- Với **Next 13+ (App Router)**, có thể load biến tại runtime bằng <font color="#188038">process.env</font>

## Các file <font color="#188038">.env</font> tiêu chuẩn

- Tạo các file sau ở thư mục gốc repo:
  - .env (global mặc định)
  - .env.development (load theo mode (dev))
  - .env.local (override cục bộ (KHÔNG commit))
  - .env.development.local (override cục bộ theo mode (KHÔNG commit))
  - File <font color="#188038">.local</font> **luôn có độ ưu tiên cao hơn** so với file chung
  - Tất cả dự án đều bắt buộc phải có file <font color="#188038">.env.development</font>
  - Khi thêm biến mới trong **CONFIG**, phải thêm vào <font color="#188038">.env.development</font> để toàn team dùng đồng bộ
  - Secrets/token/credentials nhạy cảm → KHÔNG được commit vào repo, chỉ để ở <font color="#188038">.env.local</font> (local dev mỗi người).

## Bộ biến tối thiểu khuyến nghị

- Tuỳ backend của team, hãy chuẩn hoá tên biến và **giữ nguyên prefix** <font color="#188038">VITE\_</font>:
- dotenv
  - Ví dụ: .env.development
  - VITE_APP_NAME=Alta-CMS
  - VITE_API_BASE_URL=[https://api-dev.example.com](https://api-dev.example.com)

## Truy cập biến & cấu hình Axios (ưu tiên <font color="#188038">process.env</font>, fallback <font color="#188038">import.meta.env</font>

```typescript ts
const SIMPLE_CONFIG: SimpleConfig = {
  API_BASE_URL: process.env.VITE_APP_API_BASE_URL || "https://example.com",
  APP_NAME: process.env.VITE_APP_APP_NAME || "CMS-ANIMAL",
  LOGIN_PAGE: process.env.VITE_APP_SSO_PAGE || "/",
  CLIENT_ID: process.env.VITE_APP_CLIENT_ID || "123456",
  API_KEY_GG_MAP: process.env.API_KEY_GG_MAP || "321564",
  NUMBER_MULTIPLY_DURATION: 3,
  MOBILE_WIDTH: Number(process.env.MOBILE_WIDTH) || 600,
  TABLET_WIDTH: Number(process.env.TABLET_WIDTH) || 1024,
  ROUTER_HISTORY: process.env.ROUTER_HISTORY || "s",
  DEFAULT_LANG: process.env.DEFAULT_LANG || "vi",
};

export default SIMPLE_CONFIG;
```

```typescript ts
...
  constructor(baseURL?: string, refreshUrl?: string) {
    this.service = axios.create({
      baseURL: baseURL ?? CONFIG.API_BASE_URL,
      withCredentials: false,
    });

    if (refreshUrl != null) {
      this.refreshUrl = refreshUrl;
    }
  }
...
```

## Cài đặt

- Local dev:

```powershell bash
yarn
```

## Khi đổi phiên bản Node/Yarn

- Xóa cache & <font color="#188038">node_modules</font> rồi cài lại (khi gặp lỗi lạ do phiên bản):

```powershell bash
yarn cache clean
rm -rf node_modules
yarn install
```

## Chạy dự án ở local

```powershell bash
# mặc định port 5050 theo scripts
yarn dev

# hoặc đổi cổng:
yarn dev --port 5051

# mở cho thiết bị cùng mạng test UI:
yarn dev --host

```

- Truy cập: [http://localhost:5050](http://localhost:5050)

## Kiểm tra nhanh khi app lên

- Console DevTools:

```typescript ts
console.log(
  "API:",
  import.meta.env.VITE_APP_API_BASE_URL ||
    (process as any).env?.VITE_APP_API_BASE_URL,
);
```

## Lint/format nhanh (trước khi code)

```powershell bash
yarn lint
yarn prettier
```

## Troubleshooting nhanh

- **Cổng bận **→ <font color="#188038">yarn dev --port 5051</font> hoặc bật <font color="#188038">strictPort</font>.
- <font color="#188038">process is not defined</font> → đảm bảo đã cấu hình <font color="#188038">vite-plugin-environment</font> hoặc <font color="#188038">define</font> trong <font color="#188038">vite.config.ts</font>.
- **ENV không ăn** → biến phải có prefix <font color="#188038">VITE\_</font> và dùng đúng **mode** (<font color="#188038">yarn dev</font> → <font color="#188038">.env.development</font>), restart dev server sau khi sửa <font color="#188038">.env</font>.

# 📏 Quy ước code & kiểm tra trước commit

## Triết lý tổ chức

- **Feature-first**: nhóm code theo **tính năng** thay vì loại file.
- **Shared rõ ràng**: mọi thứ dùng lại nhiều nơi đặt ở <font color="#188038">src/shared</font>.
- **Rõ tầng**: UI (component) tách **logic** (hook), tách **API/service**.

## Quy ước đặt tên (file, symbol)

- **Component React**: <font color="#188038">PascalCase</font> file + export cùng tên.
  - <font color="#188038">UserTable.tsx</font>:

```typescript ts
export function UserTable() { // dosomethings }
// (shared dùng named export; page có thể default export).
```

- **Custom hook**: <font color="#188038">useXxx</font> (camelCase), file <font color="#188038">useXxx.ts</font>.
  - Ví dụ: <font color="#188038">useUserList.ts</font>, <font color="#188038">useAuth.ts</font>.
- **Slice/selector/thunk (RTK):**
  - <font color="#188038">userSlice.ts</font>, <font color="#188038">userSelectors.ts</font>, <font color="#188038">\userThunks.ts</font>.
  - Tên slice: <font color="#188038">users</font> (số nhiều cho collection).
  - Action/thunk: <font color="#188038">fetchUsers</font>, <font color="#188038">createUser</font>.
- **Types**: <font color="#188038">\*.types.ts</font>; **type** cho shape, **enum** PascalCase, member SCREAMING_SNAKE_CASE.

```typescript ts
export type UserDto = { // dosomethings }
```

- **API service**: <font color="#188038">xxxApi.ts</font> với **hàm thuần** trả về dữ liệu đã parse.
  - Ví dụ:

```typescript ts
getUsers(params): Promise<UserDto[]>
```

- **Utils**: <font color="#188038">\*Utils.ts</font> (camelCase function).
  - Nếu function chỉ phục vụ 1 module → đặt trong <font color="#188038">\*Utils.ts</font> của module đó.
  - Nếu function có thể tái sử dụng nhiều nơi → chuyển sang <font color="#188038">shared/helper/</font>.
- **Routes**: path **kebab-case** (<font color="#188038">/user-detail</font>), file routes.ts

## Commit convention (đã chuẩn hoá)

- Cú pháp:

```Text text
<type>(<Scope>): <subject>
```

- **type**: <font color="#188038">feat | fix | docs | style | refactor | perf | test | chore</font>
- **Scope: tên component/module đúng như code** (PascalCase hay lowercase đều được)
- **subject**: câu ngắn, thì mệnh lệnh, không dấu chấm cuối
- Ví dụ đúng

```powershell bash
feat(Dashboard): build Dashboard component

fix(Dashboard): fix pagination

refactor(UserForm): split form logic into hooks

style(Table): tidy column spacing

docs(Dashboard): add usage guide

perf(Dashboard): virtualize rows

test(Dashboard): add tests for pagination

chore(Dashboard): bump antd to 5.26.0
```

<br />
