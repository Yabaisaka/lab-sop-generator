# lab-sop-generator

把设备信息、原装说明书及负责人确认的操作资料整理为 **DigitalLab 可导入的结构化 SOP JSON**。一份文件支持多种使用方式，同时填写设备通用使用前提示及标签短提示。

## 安装

配套网页工程：[DigitalLab · 实验室设备档案](https://github.com/Yabaisaka/DigitalLab)。生成的 JSON 可在设备编辑页预览并导入，统一填写多套 SOP、使用前提示和标签短提示。

使用支持 skills 的 agent，可通过 Skills CLI 从本仓库安装：

```bash
npx skills add Yabaisaka/lab-sop-generator --skill lab-sop-generator
```

也可以下载仓库，将其作为 `lab-sop-generator` 文件夹放入你的 agent 的 skills 目录。Codex 用户可使用 `~/.codex/skills/lab-sop-generator`；其他 agent 使用对应的 skills 目录。

## 使用

向 agent 提供设备名称、型号、配套附件、设备编号（如有）、使用场景及实际说明书，然后调用：

```text
使用 $lab-sop-generator，根据这些设备资料和说明书生成 DigitalLab SOP JSON。
为说明书支持的每种使用方式建立独立 SOP，未知参数标为待补充。
```

输出是 UTF-8 `.sop.json` 文件。使用配套校验器检查：

```bash
python3 scripts/validate_sop.py /path/to/equipment.sop.json
```

在 DigitalLab 的设备编辑页打开“操作 SOP”，上传 JSON → 校验并预览 → 核对合并或替换范围 → 确认导入到表单 → 保存档案。

## 包含内容

- [SKILL.md](SKILL.md)：agent 的生成流程及事实核对要求。
- [格式说明](references/format.md)及 [JSON Schema](references/schema.json)：`digitallab.sop` 协议版本1。
- [示例 JSON](assets/example.json)：两种使用方式的结构示例，内容标为待补充，不是真实仪器规程。
- [校验器](scripts/validate_sop.py)：仅需要 Python 3 标准库，检查结构、长度、重复标识等。

每台设备最多20套 SOP，每套最多100步。正文支持 Markdown；标签最多2条单行纯文本，每条最多32个普通中文字。来源定位信息与待核对事项会随文件保留。

## 资料与核对

只使用用户提供的资料、原装说明书和实际读取的厂家官方说明。不得猜测温度、压力、电压、时间等参数，或把相近机型的流程当作本设备流程。未知内容标记“待补充”，具体疑问列在 `reviewNotes`，由负责人核实后再用于实际操作。

生成 JSON 不会自动登录、上传或发布设备资料。结构校验通过也不代表操作规程已获批准。

## 维护

提交修改前，运行校验器检查示例文件。修改格式时同步更新规范、Schema 和校验器，保持已发布文件的兼容性；协议变更需使用新版本号。
