<img src="https://raw.githubusercontent.com/recipkg/recipkg/main/assets/recipkg-mark.svg" width="96" alt="Recipkg logo">

# Recipkg · 构方

**把软件的配方交给 Agent。**

从机器码、汇编、源代码，到提示词。我们希望把反复校准的软件需求、接口、依赖和验收条件保存成可以复用、审阅和传递的配方，让不同的 Agent 据此重建软件。

> 别写软件了，写提示词吧，未来的人工智能会完整重建它！

这是愿景。当前的 Recipkg 负责整理和交付重建材料；实际实现由你选择的 Agent 完成，结果需要通过事先定义的验收。

[主仓库](https://github.com/recipkg/recipkg) · [中文文档](https://github.com/recipkg/recipkg/blob/main/README.md) · [English](https://github.com/recipkg/recipkg/blob/main/README.en.md) · [设计理念](https://github.com/recipkg/recipkg/blob/main/docs/PHILOSOPHY.md)

## 一份配方，保存什么

- **行为**：软件为谁解决什么问题，接受什么输入，产生什么结果。
- **依赖**：组件如何配合，以及每一层依赖的完整配方。
- **约束**：目标环境、公开接口、兼容性和外部运行前提。
- **验收**：怎样判断重建结果正确，哪些行为仍未验证。

软件配方包以 `.recipe.tar.gz` 分发，负载全部是 UTF-8 Markdown。`RECIPE.md` 中的 TOML 块声明包信息与依赖；技能保留标准 `SKILL.md` 文件头。归档包含完整递归材料、来源记录和文件校验值。

配方不附带实现源码、二进制或执行钩子。包格式不绑定模型厂商、包工具的实现语言或生成软件的语言。

## 从这里开始

| 入口 | 内容 |
| --- | --- |
| [Recipkg CLI](https://github.com/recipkg/recipkg) | 安装、五个提示词动作、显式包操作与技能同步 |
| [配方协议](https://github.com/recipkg/recipkg/blob/main/docs/SPEC.md) | 清单、依赖来源、归档、锁文件和完整性规则 |
| [三层递归示例](https://github.com/recipkg/recipkg/tree/main/examples) | `hello → greeting → text-rules`，完整材料如何随包交付 |
| [Recipkg 自举配方](https://github.com/recipkg/recipkg/tree/main/recipes/recipkg) | 用公开行为、协议和验收条件描述 Recipkg 自身 |
| [软件质量配方](https://github.com/recipkg/recipkg/tree/main/recipes/software-quality) | 可复用的设计、代码质量与性能要求 |
| [迁移技能](https://github.com/recipkg/migrate-to-recipkg) | 将现有软件整理为 Recipkg 配方的 Agent 工作流程 |
| [重写技能](https://github.com/recipkg/rewrite-to-recipkg) | 观察软件、提取配方、建立规范仓库并重写验收 |
| [测试反馈技能](https://github.com/recipkg/test-feedback) | 实际体验与端到端测试，结合 issue 生成可验收需求 |

11 个项目技能各有独立仓库，通过固定提交的 Git 子模块复用；参见 [技能清单](https://github.com/recipkg/recipkg/blob/main/docs/SKILLS.md)。CLI 源码也独立保存在 [cli-source](https://github.com/recipkg/cli-source)，主仓库以 `generated/cli-source/` 子模块引用。

## 跑通第一个配方

当前参考 CLI 需要 Python 3.11+，运行时仅依赖标准库；获取 Git 配方来源时需要系统 Git。

```sh
git clone --recurse-submodules https://github.com/recipkg/recipkg.git
cd recipkg
python3 -m venv .venv
. .venv/bin/activate
python3 -m pip install ./generated/cli-source
recipkg check examples/hello
recipkg pack examples/hello -o hello-0.1.0.recipe.tar.gz
recipkg build hello-0.1.0.recipe.tar.gz --output hello-build.md
```

把生成的 `hello-build.md` 交给你选择的 Agent。它包含根配方与全部递归依赖，供 Agent 实现软件并执行验收。

安装后也可用 `recipkg search [关键词]` 查找带 `recipkg` topic 的公开仓库候选；标签不代替配方检查。提示词默认英文，用 `--lang zh-CN` 选择中文，配方原文保持不变。

五个主要动作是 `extract`、`build`、`rewrite`、`add` 和 `discover`，均只输出任务提示词。CLI 不调用模型，也不执行生成的软件。结构校验通过说明材料可以被读取和交付；重建成功需要实际运行生成的实现并验证行为。

## 一起完善配方

欢迎提供可复用配方、明确的行为说明、失败案例和验收改进。请先阅读主仓库的 [工作约定](https://github.com/recipkg/recipkg/blob/main/AGENTS.md)，通过 [Issues](https://github.com/recipkg/recipkg/issues) 讨论问题，或提交 [Pull Request](https://github.com/recipkg/recipkg/pulls)。贡献配方时保留材料来源和许可，并写清外部前提、推断与未验证项。

## Software recipes for the agent you choose

Recipkg packages software requirements, interfaces, recursive dependencies, and acceptance criteria as Markdown-only recipes. A complete archive carries the full dependency material and integrity metadata. The format is independent of model providers and implementation languages.

The current CLI renders prompts and manages recipe packages and Agent Skills. Your chosen agent performs the implementation; observable acceptance checks establish what was actually rebuilt.

Read the [English guide](https://github.com/recipkg/recipkg/blob/main/README.en.md), explore the [examples](https://github.com/recipkg/recipkg/tree/main/examples), or help improve the [specification](https://github.com/recipkg/recipkg/blob/main/docs/SPEC.md).
