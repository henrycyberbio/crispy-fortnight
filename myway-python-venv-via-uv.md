# 我的python虚拟环境创建方案

作为一个生信人（Bioinformatcs），常常面对的是各种奇怪的管线和差异很大的依赖，怎么样创建一个稳定的虚拟环境是开发的前提。

需求：
- Git 版本控制：我习惯保留一些本地测试文件在项目目录，方便查找和测试
- Sync：我喜欢到处跑办公，常常用不同电脑，用 Google Drive 或 Onedrive 同步项目目录。但又不想同步虚拟环境，这些依赖太大了占用我的同步空间
- UV管理环境：之前我都用 Conda，一方面 SSL 问题实在太难受，另一方面用过UV就发现好用太多...

## 方案细节

第一步，在同步盘将项目通过 Git 拉下来以同步非环境代码，同时也可以存放一些测试脚本和文件在同一个目录，再通过 `.gitignore` 忽略掉即可。

```
in-sync/ > git clone <url>
```

```
project/
|- src/
|- temp/
|   |- data/
|   |- script/
|- .gitignore  # add temp/
|- pyproject.toml
|- README.md
```

第二步，在非同步盘为 `pyproject.toml` 创建一个快捷方式，即Symbolic Link 符号链接，再通过UV创建虚拟环境

```
out-sync/ > mklink pyproject.toml in-sync/pyproject.toml
symbolic link created for pyproject.toml <<===>> in-sync/pyproject.toml

out-sync/ > uv sync
```

第三步，在VSCode里选择非同步盘的python环境，它会自行运行以下命令激活环境：

```
in-sync/ > out-sync/.venv/Script/activate
```

## 总结

**优点：**
- 同步盘不会同步 `.venv`
- 既可以通过git 进行主要代码的版本控制，又不会影响本地测试的数据和代码

**缺点：**
- 更新环境需要到 `out-sync\`进行，而不是在vscode的终端直接运行 `uv sync`