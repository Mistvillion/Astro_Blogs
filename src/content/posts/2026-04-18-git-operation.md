---
title: Git 与 GitHub 的一些基本操作
published: 2026-04-18
description: 两个GitHub账号之间切换、提交与合并PR的基础操作
tags:
  - GitHub
  - git
category: 学习心得
---
使用 DeepSeek

## 一、切换账号提交

第一次从账号 A 切换到账号 B 进行提交和推送时，需要完成以下四步配置。

### 🔧 首次切换至账号 B 需做的配置

| 步骤 | 操作 | 目的 | 是否可复用 |
| :--- | :--- | :--- | :--- |
| ① | 为账号 B **生成新的 SSH 密钥对** | 创建专属身份凭证 | ❌ 仅首次 |
| ② | 将公钥添加到 GitHub 账号 B 的设置中 | 让 GitHub 信任该密钥 | ❌ 仅首次 |
| ③ | 编辑 `~/.ssh/config`，添加账号 B 的 Host 规则 | 告诉 SSH 连接不同别名时用哪个私钥 | ❌ 仅首次 |
| ④ | 在仓库中**修改远程地址**并**设置本地用户信息** | 确保推送目标正确且提交作者正确 | ✅ 每个仓库需单独设置 |

### 📝 详细操作清单

#### 1. 生成账号 B 的 SSH 密钥

```bash
ssh-keygen -t ed25519 -C "账号B的邮箱" -f ~/.ssh/id_ed25519_github_账号B别名
```

#### 2. 将公钥添加到 GitHub 账号 B

```bash
cat ~/.ssh/id_ed25519_github_账号B别名.pub   # 复制输出内容
```

粘贴到 GitHub 账号 B → Settings → SSH and GPG keys → New SSH key。

#### 3. 配置 `~/.ssh/config`

```bash
nano ~/.ssh/config
```

添加：

```ssh-config
Host github-账号B别名          
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_github_账号B别名
```

#### 4. 针对具体仓库设置（每次新项目需要）

进入项目目录后：

```bash
# 改远程地址为 SSH 别名格式
git remote set-url origin git@github-账号B别名:账号B别名/仓库名.git

# 设置本地提交者信息（仅影响 commit 作者显示）
git config user.name "账号B别名"
git config user.email "账号B的邮箱"
```

### ✅ 切换后的验证命令

```bash
ssh -T git@github-账号B别名        # 应返回 Hi 账号B别名!
git config user.name             # 应返回 账号B别名
git push origin main             # 应成功推送
```

### 📌 后续再切回账号 A 的步骤

因为账号 A 的 SSH 配置和密钥早已存在，只需要：

1. **修改远程地址**：

   ```bash
   git remote set-url origin git@github-账号A别名:账号A别名/仓库名.git
   ```

2. **修改本地用户信息**：

   ```bash
   git config user.name "账号A别名"
   git config user.email "账号A的邮箱"
   ```

3. **清空 SSH Agent 缓存并重新添加账号 A 私钥**

   ```bash
   ssh-add -D
   ssh-add ~/.ssh/id_ed25519_github_账号A别名（注意大小写）
   ```

4. **测试别名（应返回账号 A 别名）**

   ```bash
   ssh -T git@github-账号 A 别名
   ```

> 密钥生成、公钥上传、`config` 文件这三步是**一次性**的，无需重复操作。

## 二、提交 Pull Request

向别人的GitHub仓库提交Pull Request（PR），是参与开源贡献的基本功。核心流程可以概括为：**Fork（复制仓库） → 修改（创建分支） → 提交（推送代码） → 发起PR**。

### 🎯 准备工作

在开始前，有两点准备工作很重要：

* **配置个人访问令牌（Personal Access Token）**：GitHub已不再支持密码认证。建议在Settings → Developer settings → Personal access tokens中创建一个新令牌，**记得勾选 `repo` 权限**。生成后请立即复制保存。
* **仔细阅读项目贡献指南**：务必查看原项目的`CONTRIBUTING.md`或`README.md`文件，遵循其分支命名、代码规范等要求，这能大大提高PR被接受的概率。

### 🚀 标准PR工作流（五步法）

以下是通用的PR提交步骤：

1. **🍴 Fork原仓库**
    进入别人的GitHub仓库页面，点击右上角的“**Fork**”按钮。系统会将此仓库完整地复制到你的GitHub账号下，形成一个属于你的副本。

2. **📥 克隆与分支**
    这是进行修改的前提。首先，用`git clone`命令将Fork到你账号下的仓库下载到本地。然后，**务必创建一个新的本地分支**来进行修改，请勿直接在`main`或`master`分支上操作。建议为分支起一个清晰的名字，如`feature/add-login`或`bugfix/fix-typo`。

    ```bash
    # 克隆你Fork的仓库
    git clone https://github.com/你的用户名/仓库名.git
    cd 仓库名

    # 创建并切换到新分支
    git checkout -b 你的新分支名
    ```

    *💡 小提示：为了方便同步原仓库的更新，建议将原作者的仓库添加为上游（upstream）*。

    ```bash
    git remote add upstream https://github.com/原作者用户名/原仓库名.git
    ```

3. **✍️ 修改与推送**
    在本地进行代码修改并充分测试后，提交修改并推送到你Fork的仓库。

    ```bash
    # 添加修改
    git add .
    # 提交修改，写好清晰的说明
    git commit -m "feat: 添加了xxx功能"

    # 将新分支推送到你Fork的远程仓库
    git push origin 你的新分支名
    ```

4. **🤝 创建Pull Request**
    推送成功后，进入你在GitHub上**Fork后的仓库页面**。通常页面顶部会出现黄色的“**Compare & pull request**”按钮，点击它。
    如果没有，可手动点击“Pull requests”标签页，再点击绿色的“**New pull request**”按钮。
    在创建页面上，你需要：
    * 选择正确的合并方向：`base repository`为**原作者的仓库**，`head repository`为你**Fork的仓库**，并选择你刚刚推送的分支。
    * 在标题和描述框中**清晰地说明你的修改内容和目的**。
    最后，点击“**Create pull request**”提交。

5. **✅ 等待审核**
    提交后，原仓库的维护者会收到通知。请保持耐心，他们可能会提出修改建议，你只需在本地的分支上继续修改并再次推送即可，PR会自动更新。一旦通过审核，你的代码就会被合并。

### 🛠️ 补充说明：同步与清理

* **如何保持同步**：在贡献前，建议先将你的主分支与原仓库同步。

    ```bash
    git checkout main
    git pull upstream main
    git push origin main
    ```

* **如何清理分支**：PR被合并后，你可以在GitHub的PR页面点击“Delete branch”删除远程分支，再在本地执行 `git branch -d 你的新分支名` 删除本地分支。

简单来说，就是“Fork过来，分支修改，推送上去，发起PR”。

## 三、合并 Pull Request

当你提交PR后，仓库的维护者（或拥有合并权限的人）会按照一定流程来处理它。以下是他们**通过（合并）你的PR**时通常会做的操作：

### 🔍 查看PR内容

维护者登录GitHub后，进入仓库的 **Pull requests** 标签页，点击你的PR。他们会看到：

* **Conversation** 选项卡：你的描述、讨论历史。
* **Commits** 选项卡：你提交的所有commit记录。
* **Files changed** 选项卡：所有代码变动的详细对比（diff）。

### ✅ 审查代码（Code Review）

维护者会逐行查看代码改动，重点关注：

* 逻辑是否正确，有无潜在bug。
* 代码风格是否符合项目规范。
* 是否遵循了提交规范（如commit message格式）。
* 是否需要补充测试或文档。

如果需要修改，他们会在对应代码行**添加评论（Review comments）**，并选择“**Request changes**”（请求修改）。你会收到通知，按要求更新代码后重新推送即可。

### ⚙️ 检查自动化验证

PR页面底部通常会显示一系列**状态检查（Checks）**，如：

* 持续集成（CI）是否通过（测试、构建）。
* 代码覆盖率是否达标。
* 是否有未解决的冲突（Merge conflicts）。

如果出现红色❌，维护者通常会等你修复后才会合并。

### 🤝 合并PR

当所有审查通过、检查变绿、且维护者满意后，他们会在PR页面底部点击 **“Merge pull request”** 按钮。GitHub提供了**三种合并方式**（由仓库设置决定可用选项）：

| 合并方式 | 效果 | 适用场景 |
| :--- | :--- | :--- |
| **Create a merge commit** | 保留所有commit记录，并额外生成一个合并commit。 | 保留完整历史，适合大型协作。 |
| **Squash and merge** | 将你的所有commit**压缩成一个commit**再合并到目标分支。 | 保持主干历史简洁，适合小型功能PR。 |
| **Rebase and merge** | 将你的commit逐个**变基**到目标分支顶端，不产生合并commit。 | 保持线性历史，但操作要求较高。 |

维护者选择一种方式点击确认后，你的代码就会被合并进目标分支（通常是`main`）。合并成功后，你的PR页面会显示 **“Merged”** 状态。

### 💬 后续清理

合并完成后，维护者可能会：

* **删除你的远程分支**（GitHub界面提供一键删除按钮）。
* 在PR对话中留下感谢或补充说明。

> **简单来说**：维护者会先**审阅代码**，确保一切OK后，点击绿色的 **“Merge pull request”** 按钮，你的贡献就被正式收入项目了。如果中途需要修改，他们会通过评论告诉你下一步怎么做。