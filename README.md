# 护肝宝任务库（纯文本脚本源码仓库）

这里是 [护肝宝（hgb）](https://github.com/lc-cn/hgb) 的官方任务脚本仓库。所有自动化任务与共享片段均以**多行无转义中文任务脚本（`.hgb`）**的形式直接存放在主干分支中，既可以在网页开发工作台可视化编辑、真机试跑并一键发布，也可以直接用 Git、任意编辑器或 AI 工具克隆修改。

## 目录结构

```text
games/
  onmyoji/
    game.json             # 游戏配置、命名区域、取色点组、状态库、页面导航图与资源清单
    tasks/
      *.hgb               # 每个日常/悬赏任务一个纯文本脚本文件（含任务头与 === 步骤 === 分节）
    snippets/
      *.hgb               # 可复用的公共动作片段（纯文本任务脚本）
    templates/
      *.png               # 裁剪后的纯 UI 图标模板（严禁包含任何玩家昵称或隐私信息）
```

> OCR 基础模型文件（`det.onnx`、`rec.onnx`、`keys.txt`）体积较大且长期不变，托管在 `v1.0.0` Release 附件中，由 `game.json` 的 `resources[].release` 按 SHA-256 引用。

## 两种协作与发版工作流

### 方式一：网页开发工作台（推荐用于采样、裁剪与真机单步联调）

1. 打开控制台 `/console/dev`：
   - 点顶栏 **「从仓库同步」** 即可将本仓库 `main` 分支最新的 `.hgb` 纯文本脚本拉入工作台草稿。
2. 在线编辑脚本、实时查看行级校验提示，并在连接的 Android 真机上单步或整任务试跑。
3. 进入 **「校验并发布」** 填写版本号（如 `v1.2.2`）发布：
   - 服务端会自动将最新 `.hgb` 脚本源码树与模板原子提交回本仓库 `main` 分支，并同步发布包含 `task-bundle.json` 的 GitHub Release。

### 方式二：本地 Git / AI 编辑器直接编写

1. 克隆本仓库与主工程仓库 `lc-cn/hgb`（需 JDK 17）：
   ```bash
   git clone https://github.com/lc-cn/hgb-tasks.git
   git clone https://github.com/lc-cn/hgb.git
   ```
2. 直接编辑 `games/onmyoji/tasks/*.hgb` 或 `games/onmyoji/snippets/*.hgb`（语法详见主仓库 `docs/design/task-script.md`）。
3. 本地运行引擎全量编译与静态校验（含脚本语法、页面图连通性、句柄流向及模板 SHA-256 核对）：
   ```bash
   ./hgb/gradlew -p hgb :core:engine:taskRepo --args="check $(pwd)/hgb-tasks"
   ```
4. 推送至 `main` 分支（或发起 Pull Request）将自动触发 GitHub Actions 校验；若需从命令行直接发版，打上 `v*` 标签推送即可自动打包发布 Release：
   ```bash
   git -C hgb-tasks tag -a v1.2.3 -m "优化某某任务流程"
   git -C hgb-tasks push origin main v1.2.3
   ```
