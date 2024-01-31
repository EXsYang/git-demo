# 命令总结

当然，根据您提供的命令历史记录，这里是按类别整理且去重的正确命令列表：

1. **克隆仓库（Cloning Repositories）**:
   - `git clone https://github.com.cnpmjs.org/thx/gogocode.git`
   - `git clone git@github.com:YoYoDeveloper/phdbcsv.git`
   - `git clone https://github.com/hwdsl2/setup-ipsec-vpn.git`
   - `git clone https://github.com/Chuyu-Team/Dism-Multi-language.git`
   - `git clone https://github.com/codinglin/StudyNotes.git`
   - `git clone https://github.com/remote-jp/remote-in-japan.git`

2. **配置 Git（Configuring Git）**:
   - `git config --global http.proxy "localhost:443"`
   - `git config --global --unset http.proxy`
   - `git config --global user.name "yangda27"`
   - `git config --global user.email "yangda27@aliyun.com"`
   - `git config -l`
   - `git config --global user.name "EXsYang"`
   - `git config --system --list`

3. **初始化仓库（Initializing Repositories）**:
   - `git init`

4. **检查状态（Checking Status）**:
   - `git status`

5. **添加文件到暂存区（Adding to Staging Area）**:
   - `git add .`

6. **提交更改（Committing Changes）**:
   - `git commit -m 'commit 1'`
   - `git commit -m '第一次提交哦'`
   - `git commit -m 'hot-fix 第一次提交'`
   - `git commit -m 'master 第二次提交'`

7. **分支操作（Branching）**:
   - `git branch`
   - `git branch -v`
   - `git checkout -b hot-fix`
   - `git checkout master`
   - `git checkout -b master`

8. **查看日志（Viewing Logs）**:
   - `git log`
   - `git reflog`

9. **远程仓库操作（Remote Repositories）**:
   - `git remote -v`
   - `git remote add git-demo https://github.com/EXsYang/git-demo.git`

10. **推送到远程仓库（Pushing to Remote Repository）**:
    - `git push git-demo master`

这个整理的列表包含了各种 Git 操作的基本命令，从仓库的克隆、配置、到常见的分支和提交操作，以及与远程仓库的交互。







# 1 git基本命令





~~~bash
#基本配置
$ git config -l

$ git config --global user.name 'CodeYang' 

$ git config --global user.email '10825197+kapaiya@user.noreply.gitee.com'


#初始化本地仓库
$ git init


#克隆
$ git clone url


#文件状态，使用 git add 文件从 Untracked(未跟踪) -> Staged(暂存)
$ git status ABC.txt

$ git add .

$ git commit -m 'commit 1'

$ git status

#查看日志
$ git log

#远程推送
$ git push git-demo master


~~~

![image-20240131184824281](D:\TyporaPhoto\image-20240131184824281.png)





## 版本回退

~~~
yangda@F2 MINGW64 /d/Java_developer_tools/GiteeRepository/gitee_hsp_java/hspgit (master)
$ git-log
* f3d3cc3 (HEAD -> master) 第二次提交
* beeb1a7 第一次提交

yangda@F2 MINGW64 /d/Java_developer_tools/GiteeRepository/gitee_hsp_java/hspgit (master)
$ git reset --hard beeb1a7
HEAD is now at beeb1a7 第一次提交

yangda@F2 MINGW64 /d/Java_developer_tools/GiteeRepository/gitee_hsp_java/hspgit (master)
$ git-log
* beeb1a7 (HEAD -> master) 第一次提交

yangda@F2 MINGW64 /d/Java_developer_tools/GiteeRepository/gitee_hsp_java/hspgit (master)
$

~~~



**注意事项：找回回退之前的commitID的指令 `git reflog`**



![image-20240131173033336](D:\TyporaPhoto\image-20240131173033336.png)



## 分支

**查看当前有哪些分支  `git branch`**

**创建新分支  `git branch dev01`**

**切换分支 `git chechout master`**

**合并分支 `git merge dev01`** ,将分支dev01合并到master上

**删除分支`git branch -d 分支名`**

**强制删除分支`git branch -D 分支名`**

**`master` 分支可能从未被创建**：在某些情况下，如果仓库是新的并且没有任何提交，`master` 分支可能根本就没有被创建。只有在第一次提交后，`master` 分支才会被自动创建。

**初始分支可能有不同的名称**：最近，一些 Git 服务（如 GitHub）开始使用 `main` 而非 `master` 作为新仓库的默认分支名称。如果您的仓库是从这样的服务克隆或创建的，那么默认分支可能是 `main`。

## 检出文件

`git checkout -- filename` 命令实际上是从本地仓库（Git 术语中的“repository”）中取出文件的最后一次提交（commit）的版本，并将其放到工作目录中。这个命令并不涉及暂存区（Git 术语中的“index”或“staging area”）。

这里的流程解释如下：

- **工作目录（Working Directory）**：这是您当前正在进行工作的地方，文件在这里被创建、编辑和删除。

- **暂存区（Staging Area/Index）**：在您提交之前，更改过的文件会放到这里。通过使用 `git add` 命令，文件更改会从工作目录移动到暂存区。

- **本地仓库（Repository）**：当您执行 `git commit` 命令时，暂存区的内容会成为仓库的一部分，即一次提交。

当您执行 `git checkout -- filename` 时，Git 会查找该文件在最后一次提交中的状态（即本地仓库中的状态），并将其复制到工作目录，无论其是否已经被添加到暂存区。这意味着工作目录中的文件将被替换为最后一次提交时的内容，所有未提交的更改都将丢失。



## 解决合并分支冲突问题

![image-20240131181928392](D:\TyporaPhoto\image-20240131181928392.png)



## 合并分支的步骤和注意事项

当然，使用具体的分支名称如 `master` 和 `dev` 会让步骤更加明确。以下是使用 `master` 作为目标分支和 `dev` 作为源分支时合并分支的步骤和注意事项的总结：

1. **检查当前分支**：
   - 使用 `git status` 确认当前所在分支。

2. **切换到 `master` 分支**：
   - 如果您不在 `master` 分支上，使用 `git checkout master` 切换到 `master` 分支。

3. **从 `dev` 分支合并到 `master`**：
   - 在 `master` 分支上执行 `git merge dev` 命令，将 `dev` 分支的更改合并进来。
   - 如果合并成功且没有冲突，Git 会自动创建一个合并提交。

4. **自动合并提交**：
   - Git 在无冲突的合并后会自动生成提交信息，如 "Merge branch 'dev' into master"。

5. **处理合并冲突**：
   - 如果在合并时出现冲突，Git 会暂停并允许您手动解决冲突。
   - 解决冲突后，使用 `git add <解决冲突的文件>` 将这些文件标记为已解决。
   - 然后执行 `git commit`，此时可以编辑自动生成的合并提交信息或直接使用。
   - 注意这里执行提交时 使用 git commit 命令时**不能带文件名**！ 
   
6. **自定义提交信息**：
   - 如果您想为合并提供自定义的提交信息，可以在合并时加上 `--no-commit` 选项，然后使用 `git commit -m "您的自定义合并信息"` 手动提交。

7. **确认合并结果**：
   - 使用 `git log` 检查提交历史，确认合并操作按照预期完成。

通过遵循这些步骤，您可以清晰地将 `dev` 分支的更改合并到 `master` 分支，并确保整个过程顺利进行。













# 2 使用 Git 进行提交时的步骤和注意事项

当然，我将重新整理并包含您提到的关于使用 Vim 编辑器进行提交的详细步骤。以下是使用 Git 进行提交时的步骤和注意事项：

1. **检查当前状态**：
   - 使用 `git status` 来查看哪些文件被修改了。
   - 这有助于确定哪些文件需要添加到暂存区。

2. **添加文件到暂存区**：
   - 使用 `git add <文件名>` 将更改的文件添加到暂存区。
   - 可以使用 `git add .` 添加所有修改过的文件到暂存区，但要小心不要意外添加不需要的文件。

3. **执行提交**：
   - 使用 `git commit`（不带 `-m` 选项）将暂存区的更改提交到仓库。
   - 这会打开 Vim 编辑器以便您输入提交信息。

4. **在 Vim 中编写提交信息**：
   - 在 Vim 编辑器打开后，在文件的最顶部空白行输入提交信息。
   - 避免在以 `#` 开头的注释行输入文本。

5. **保存提交信息并退出 Vim**：
   - 完成提交信息的编写后，按 `Esc` 退出插入模式。
   - 输入 `:wq` 后按 `Enter` 保存您的提交信息并退出 Vim。

6. **自动完成提交**：
   - 退出 Vim 后，Git 会自动使用您在 Vim 中输入的提交信息完成提交过程。

7. **查看提交历史**：
   - 使用 `git log` 查看您的提交历史，以确认提交成功。

8. **注意事项**：
   - 如果直接执行 `git commit` 而没有先使用 `git add` 添加文件，尚未暂存的更改将不会被包括在提交中。
   - 对于新文件（即 Git 之前未跟踪的文件），必须先使用 `git add` 添加这些文件，然后才能提交。
   - 在 Vim 中编写提交信息时，确保在最上面的空白行输入，避免在注释行（以 `#` 开头）输入。
   - 如果您在 Vim 中未输入任何提交信息就退出（使用 `:q!`），提交将被取消。

这些步骤和注意点将帮助您更准确、有效地使用 Git 进行版本控制和提交管理。



配置的邮箱地址

### yangda27@aliyun.com

pwd:131407da



# 3 将 Git 的默认编辑器恢复为默认设置（通常是 Vim）

**要将 Git 的默认编辑器从 Visual Studio Code 恢复为默认设置（通常是 Vim）**，您可以通过修改 Git 的配置来实现。在 Git Bash 或其他命令行界面中，您可以使用以下命令来更改默认的提交编辑器：

```bash
git config --global core.editor vim
```

这个命令将全局的 Git 配置中的 `core.editor` 设置为 `vim`，这是 Git 默认使用的文本编辑器。

如果您不熟悉 Vim 编辑器，它的操作方式可能与您熟悉的编辑器有所不同。基本的 Vim 命令包括：

- `i` 进入插入模式，可以开始输入文本。
- `Esc` 退出插入模式，回到普通模式。
- 在普通模式下输入 `:wq` 然后按 `Enter` 保存更改并退出。
- 在普通模式下输入 `:q!` 然后按 `Enter` 不保存更改并退出。

这些是使用 Vim 的基础知识。如果您想使用一个更简单的文本编辑器，比如 Nano，您可以将上述命令中的 `vim` 替换为 `nano`。例如：

```bash
git config --global core.editor nano
```

Nano 编辑器的操作会更直观一些，特别是对于那些习惯了图形界面编辑器的用户。



# 4 idea进行push到gitee远程仓库时，会让我输入账号和密码，可是我已经配置了公共ssh公钥了啊？

![image-20240131172033830](D:\TyporaPhoto\image-20240131172033830.png)

![image-20240131171824741](D:\TyporaPhoto\image-20240131171824741.png)

如果 IntelliJ IDEA 在进行 Git 提交操作时要求您输入账号和密码，即使您已经配置了 SSH 公钥，这通常意味着 IDEA 正在尝试通过 HTTPS 而非 SSH 来连接 Git 仓库。要解决这个问题，您可以按照以下步骤操作：

1. **检查仓库的远程 URL**：
   - 首先，您需要确认 IDEA 使用的 Git 仓库远程 URL 是 SSH 格式还是 HTTPS 格式。
   - 您可以在 IDEA 的 Git 仓库设置中查看，或者在命令行使用 `git remote -v` 查看。

2. **切换到 SSH URL**：
   - 如果远程 URL 是 HTTPS 格式的（如 `https://`），您需要将其更改为 SSH 格式（如 `git@github.com:用户名/仓库名.git`）。
   - 在命令行中，您可以使用 `git remote set-url origin 新的SSH_URL` 来更改。

3. **确认 SSH 密钥配置**：
   - 确保您的 SSH 密钥已经添加到 Git 服务器（如 GitHub、GitLab 等）的账户中。
   - 在本地计算机上，SSH 密钥应该存储在 `~/.ssh` 目录下，并且 `ssh-agent` 正在运行并已加载您的密钥。

4. **在 IDEA 中配置 SSH**：
   - 在 IDEA 中，确保您的 SSH 配置是正确的。
   - 您可以在 IDEA 的设置中找到 SSH 配置，确保它指向了正确的 SSH 密钥文件。

完成这些步骤后，您应该能够通过 SSH 使用 IDEA 进行 Git 操作，而不需要再输入账号和密码。如果问题仍然存在，可能需要检查防火墙设置或网络连接，以确保您的计算机能够通过 SSH 访问远程 Git 仓库。



# 5 如果远程 URL 是 HTTPS 格式的是不可以进行免密登录吗？

实际上，HTTPS URL 也可以用于免密登录，但它通常需要不同的方法来实现，如使用凭据存储或访问令牌。与 SSH 公钥/私钥认证不同，HTTPS 连接通常会要求用户名和密码，但这些可以通过以下方式自动提供：

1. **凭据缓存**：
   - Git 提供了凭据存储的功能，可以在一段时间内记住您的用户名和密码。您可以通过 Git 命令启用这个特性：
     ```bash
     git config --global credential.helper cache
     ```
   - 这会使 Git 在一定时间内记住您的凭据。

2. **凭据管理器**：
   - 在 Windows 上，Git 可以使用 Windows 凭据管理器来存储凭据。设置方法为：
     ```bash
     git config --global credential.helper manager
     ```
   - 在 macOS 上，Git 可以使用 macOS 密钥链存储访问凭据。

3. **访问令牌**：
   - 对于一些服务（如 GitHub），您可以生成一个访问令牌并使用它作为密码。这在多因素认证（MFA）启用的情况下特别有用。

4. **使用 `.netrc` 文件**：
   - 您可以在主目录下创建一个 `.netrc`（在 Windows 下是 `_netrc`）文件，其中存储您的登录凭据。Git 会从这个文件读取凭据以进行自动登录。

即使您配置了 SSH 公钥，如果您的远程仓库 URL 是 HTTPS 格式的，Git 默认会使用 HTTPS 连接，此时您需要按照上述方法之一来配置 HTTPS 的免密登录。不过，出于安全和便利性考虑，很多人更倾向于使用 SSH 连接，因为它在配置后可以非常方便地进行免密操作。







# 6 远程推送到GitHub是否成功的案例

![image-20240201005959906](D:\TyporaPhoto\image-20240201005959906.png)







#  小技巧：

## 1  gitbash命令窗口复制粘贴的说明

**在gitBash命令窗口中，选中文本就是已经复制了，不需要再右键选择复制,默认按下鼠标滚轮/中键就是粘贴paste**

## 2 SSH 密钥注意事项说明

当然，以下是关于同一个 SSH 密钥可用于多个平台，以及本地可以生成多份 SSH 密钥的简要总结：

1. **同一个 SSH 密钥可用于多个平台**：
   - 一个生成的 SSH 密钥对（包括公钥和私钥）可以在多个平台或服务中使用。
   - 公钥可以被安全地添加到多个远程服务（如 GitHub, GitLab, Bitbucket 等）的账户设置中。
   - 私钥保留在本地，用于身份验证。它应保持安全，不被公开或在不同设备间共享。

2. **本地可以生成多份 SSH 密钥**：
   - 可以在本地生成多个 SSH 密钥对，用于不同的目的或服务，这有助于管理和安全性。
   - 每个密钥对都可以有不同的文件名和注释，以便于区分。
   - 例如，您的命令 `ssh-keygen -t rsa -C 'Github_Email_yangda27@aliyun.com' -f ~/.ssh/github01_id_rsa` 创建了一个专门用于 GitHub 的密钥对，文件名为 `github01_id_rsa`，注释为您的 GitHub 邮箱地址。
   - 为不同服务或目的创建单独的密钥对可以增加安全性，因为每个密钥的使用范围和访问权限被限制在特定的服务或场景中。

简而言之，单个 SSH 密钥对可跨多个平台使用，但为了更好的安全性和管理，建议为不同的服务或目的生成不同的密钥对，并且可以通过命令行选项指定每个密钥对的文件名和注释。