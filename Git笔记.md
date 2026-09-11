[TOC]

### Git笔记（AI）

ai仿照我笔记风格写的git笔记

### 一，Git是什么

**概念：**

- Git 是一个 ==分布式版本控制工具==，用来记录文件的每一次改动
- 可以理解成给文件夹装了一个“时光机 + 网盘”，随时能回退、能多人协作
- GitHub 是存放 Git 仓库的网站，本地改完后上传（push）到 GitHub

**四个区域（重点理解）：**

| 区域 | 含义 | 对应位置 |
| ---- | ---- | ---- |
| 工作区 | 你实际编辑的文件 | `Desktop\markdown` 里的文件 |
| 暂存区 | “准备提交”的清单 | 用 `git add` 放进去 |
| 本地仓库 | 本机的历史记录 | 隐藏的 `.git` 文件夹 |
| 远程仓库 | GitHub 上的仓库 | `origin` → Study-Notes |

一次提交 = **改文件 → add（挑选）→ commit（存档）→ push（上传）**

---

### 二，初次配置

#### 1. 查看配置

```
git config --global --list
```

#### 2. 配置用户名和邮箱

```
git config --global user.name "xiyueying"
git config --global user.email "3309761252@qq.com"
```

#### 3. 配置代理

GitHub 在国内直连不稳定，Clash 开着时走本地代理：

```
git config --global http.proxy http://127.0.0.1:7897
git config --global https.proxy http://127.0.0.1:7897
```

代理端口要和 Clash 软件里的一致，改端口时记得同步改这里

取消代理：

```
git config --global --unset http.proxy
git config --global --unset https.proxy
```

---

### 三，日常四步走（用得最多）

在 `markdown` 文件夹里打开 PowerShell，依次执行：

```
git status                 查看改了哪些文件
git add -A                 把所有改动加入暂存区
git commit -m "更新笔记"    存一版，引号里写说明
git push                   上传到 GitHub
```

**建议节奏：** 开始工作前先 `git pull`，工作完成后 `push`

`git status` 只查看、无副作用，不确定时随时敲

---

### 四，查看信息

```
git status              当前状态
git diff                还没 add 的具体改动
git diff --staged       已经 add 的改动
git log --oneline       历史提交，一行一条
git log --oneline -10   最近 10 条
git show <提交号>        看某条提交改了什么
```

---

### 五，同步远程

#### 1. 拉取别人的改动

```
git pull
```

= 抓取（fetch）+ 合并（merge）远端最新内容

#### 2. 推送自己的改动

```
git push
```

第一次推送需要指定分支：

```
git push -u origin main
```

`-u` 表示记住这个分支，以后再敲 `git push` 即可

#### 3. 查看远程仓库

```
git remote -v
git remote show origin
```

---

### 六，分支

分支可以理解成“平行世界”，互不影响

```
git branch                 查看本地分支
git branch -a              查看所有分支（含远程）
git branch dev             新建 dev 分支
git switch dev             切换到 dev 分支
git switch -c dev          新建并切换
git merge dev              把 dev 合并到当前分支
git branch -d dev          删除 dev 分支
```

`main` 是主分支，日常笔记只用一个分支就够了

---

### 七，撤销与回退

#### 1. 丢弃工作区改动（谨慎）

```
git restore 文件名
git restore .
```

#### 2. 取消暂存

```
git restore --staged 文件名
```

#### 3. 修改上次提交的说明

```
git commit --amend -m "新说明"
```

#### 4. 回退到某个提交（谨慎，会丢历史）

```
git reset --hard <提交号>
```

`git reset --hard` 会直接丢弃改动，用前先想清楚

---

### 八，.gitignore

用来指定==不提交==的文件，写在仓库根目录的 `.gitignore` 里

```
Thumbs.db
desktop.ini
.DS_Store
.java/AccessKey.csv
```

- 每行一个规则，`#` 开头是注释
- 已被忽略的文件不会出现在 `git status` 里
- 检查某个文件是否被忽略：

```
git check-ignore -v 文件名
```

---

### 九，常见报错与解决

| 报错/现象 | 原因 | 解决 |
| ---- | ---- | ---- |
| `push` 被拒绝，提示先 pull | 远端有本地没有的提交 | 先 `git pull`，再 `git push` |
| `Connection was reset` / 超时 | 代理没开或端口不对 | 打开 Clash，检查代理配置 |
| `Authentication failed` | 登录失效 | 重新登录（会弹浏览器） |
| `detected dubious ownership` | 仓库属主异常 | 按提示 `git config --global --add safe.directory 路径` |
| 提示 `nothing to commit` | 没有改动或忘了保存 | 先保存文件，再 `git status` |
| 忘了命令 | —— | 敲 `git status`，通常会提示下一步 |

---

### 十，本仓库注意事项（红线）

1. ==不要动 `C:\.git`==。C 盘根目录那个仓库是误建的，你的仓库靠 `markdown\.git` 生效；在 C 盘根目录敲 git 命令会波及整个 C 盘
2. `.java\AccessKey.csv` 含密钥，已设为永不提交，不要用 `git add -f` 强加
3. 代理要开着（`127.0.0.1:7897`），Clash 关了会 push/pull 失败
4. 提交说明写清楚，方便以后回查

---

### 十一，命令速查表

| 目的 | 命令 |
| ---- | ---- |
| 看状态 | `git status` |
| 全部加入暂存区 | `git add -A` |
| 提交 | `git commit -m "说明"` |
| 上传 | `git push` |
| 拉取 | `git pull` |
| 历史 | `git log --oneline` |
| 查看改动 | `git diff` |
| 丢弃改动 | `git restore 文件` |
| 换分支 | `git switch 分支名` |

**日常只记这四条：** `git status` → `git add -A` → `git commit -m "说明"` → `git push`
