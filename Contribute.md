## 附录 A

[TOC]

软件工程小组 Cocoon 的 Git 规范。

> 这份规范由 [roife](https://github.com/roife) 在 2022 级北航软件工程的团队开发中编写。
> **如有具体建议，请联系 volcaxiao**

### ⚙ 提交规范

#### Commit Message

```jsx
<type>(<scope>): <subject>

<body>
```

- type 有下面几类
  - `feat` 新功能
  - `fix` 修补bug（在 `<body>` 里面加对应的 Issue ID）
  - `test` 测试相关
  - `style` 代码风格变化（不影响运行）
  - `refactor` 重构（没有新增功能或修复 BUG）
  - `perf` 性能优化
  - `chore` 构建过程变动（包括构建工具/CI等）
- scope（可选）：影响的模块
- subject：主题（一句话简要描述）
- body（可选）：详细描述，包括相关的 issue、bug 以及具体变动等，可以有多行

#### 例子

```bash
[feat](账号模块): 增加微信登录验证
```
```bash
[fix](管理员 UI): Safari 下界面适配

1. xxx 元素 yyyy
2. aaa 页面 bbbb

Issue: #3
```
```bash
[refactor](招聘信息接口): xxx 接口更新
```
```bash
[style]: 格式规范更改，重新格式化
```

**请不要用 `fix #3` 之类的 message 关闭 Issue！请按照 BUG 提出与修复的描述用 Pull Request！**

### 🎋 分支规范

代码仓库分为以下六类分支：

1. main 分支 `线上在跑的版本`
    1. 提供给用户使用的**正式版本**和**稳定版本**；
    2. 🏷️ 所有**版本发布**和 **Tag** 操作都在这里进行；
    3. ❌ **不允许**开发者日常 push，只允许从 release 合并。
2. release 分支 `将要上线的版本`
    1. 从 develop 分支检出，只用于发布前的确认；
    2. 允许从中分出 fix 分支，修复的 commit 需要 push 回 dev；
    3. ❌ **不允许**开发者日常 push，只允许从 dev 合并。
3. dev 分支 `日常开发汇总`
    1. 开发者可以检出 feature 和 fix 分支，开发完成后 push 回 dev；
    2. 保证领先于 main；
    3. ❌ **不允许**开发者日常 push，只允许完成功能开发或 bug 修复后通过 pull request 进行合并。
4. feature 分支
    1. 从 dev 分支检出，用于新功能开发；
    2. 命名为 `feature/name`，如 `feature/resume_generation`；
    3. 开发完毕，经过测试后合并到 dev 分支；
    4. ✅ 允许开发者日常 push.
5. fix 分支
    1. 从 dev 或 release 分支检出，用于 bug 修复（feature 过程中的 bug 直接就地解决）；
    2. **特殊情况下**允许直接从 main 直接开 fix 分支进行修复；
    3. 命名为 `fix/issue_id`，如 `fix/2` ;
    4. 修复完毕，经过测试后合并到原来的分支（dev/release/main），**并且保证同时合并到 dev**;
    5. ✅ 允许开发者日常 push.
6. chore 分支
    1. 从 dev 分支检出，用于各项修正，如重构、风格优化等；
    2. 命名为 `chore/name`，如 `chore/resume_generation`；
    3. 开发完毕，经过测试后合并到 dev 分支；
    4. ✅ 允许开发者日常 push.


### 🧬 分支合并

<u>**不许在别人的分支上开新分支，只能在主分支开分支。分支粒度可以尽可能小。**</u>

#### 合并到 `dev` 分支

我在 `my_feature` 上开发了一段时间了，想要合并到 `dev`，可以这么做：

1. 拉取最新的 `dev` ；
2. `rebase` 当前分支到 `dev`，并且解决 `rebase` 带来的问题；
3. `rebase` 完成后，将当前分支推到远端（**TA注：请考虑一下这个推送的操作要在什么条件下进行**），在 GitHub 上发起一个到 dev 的 Pull Request，**及时合并**，防止更多冲突。

#### 想解决 `dev` 上和我有冲突的新提交，但是不想合并到 `dev`

假设我在 `my_feature` 上开发，其他人开发的功能上线到 `dev` 分支了。

**那么我可以在当前的分支下 `rebase` 一下 `dev` 分支**，这样我这个分支的几个 commits 相对于 `main` 还是处于最顶端的，也就是说 `rebase` 主要用来跟上游同步，同时把自己的修改顶到最上面。

开发的时候，如果你的功能很复杂，**尽量及时 `rebase` 上游分支**，有冲突提前就 fix 掉。

> 这样即使我们自己的分支开发了很久，也不会积累太多的 conflict，最后合并进主分支的时候特别轻松。
>
> 反对从主分支 checkout 出新分支，自己闷头开发，结果最后合并进主分支的时候，产生有一大堆冲突。

最后，<u>**不要在公共分支（例如 dev/main）上瞎 rebase！**</u>

### 🪳 BUG 提出与修复

1. 提出一个 issue，按照 template **简要记录** bug 相关的信息，事后能看懂即可，不用太详细；
2. 开一个 `fix` 分支，对 bug 进行修复。fix 修复完成后需要**进行测试，确认已经修复**；
3. 开一个 pull request，并将 pull request 和 issue 关联起来；
4. 合并 PR。

❌ **不要在 commit message 里面用 `fix #3` 等直接 close issue，应该在 pull request 关联 issue**.

如果是在 feature 分支**开发过程**中遇到的小 bug，可以直接顺手本地修了，不用按照这个流程来。
