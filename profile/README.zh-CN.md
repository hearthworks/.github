<div align="center">

[![English](https://img.shields.io/badge/lang-English-lightgrey?style=for-the-badge)](./README.md)
[![中文](https://img.shields.io/badge/lang-中文-blue?style=for-the-badge)](./README.zh-CN.md)

<!-- Banner / logo. 部署后替换为正式资源.
     建议尺寸 1280×320, 文件 ≤ 300 KB. -->
<a href="https://github.com/hearthworks">
  <img src="https://raw.githubusercontent.com/hearthworks/.github/main/profile/banner.png"
       alt="HEARTH —— AI Agent 与边缘硬件之间的能力契约层"
       width="100%" />
</a>

# HEARTH Works

**AI Agent 与边缘硬件之间的能力契约层。**

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![Commercial license available](https://img.shields.io/badge/Commercial-license%20available-success.svg)](mailto:syqust@gmail.com)
[![IETF draft](https://img.shields.io/badge/IETF-draft--sunyi--hacp--protocol--00-orange.svg)](https://datatracker.ietf.org/doc/draft-sunyi-hacp-protocol/)
[![Made with C11](https://img.shields.io/badge/C-11-informational.svg)](https://en.wikipedia.org/wiki/C11_(C_standard_revision))

</div>

---

## 🔥 HEARTH 是什么？

LLM agent 越来越擅长 *推理* 硬件，但它们之间没有一个 *安全* 又 *统一*
的方式去真正 *动* 硬件。今天每个 agent 都在重新发明自己的 GPIO/I²C/UART
胶水代码，靠 shell 调用拼凑、毫无审计能力。

**HEARTH** 用三层结构解决这件事：

1. **HACP** —— *Hearth Agent Capability Protocol*：一个跑在 `AF_UNIX`
   上的极简 JSON-RPC 2.0 协议，让任何 agent 都能问"这台机器能干什么"
   以及"请按顺序执行这 N 步"。
   *已作为 `draft-sunyi-hacp-protocol-00` 提交至 IETF。*
2. **hearthd** —— 单可执行文件的 C11 daemon，它持有硬件、执行 8 层
   纵深安全检查、并写一份 SHA-256 哈希链审计日志。
3. **ai-shell** —— 参考实现的 CLI agent，可以直接驱动 `hearthd`，也可以
   作为 Copilot / Codex / Ollama 的代理。

同一家族的配套规范：

| 规范 | 锁定的内容 |
|------|------------|
| **HACP** | 线协议、6 个 RPC、错误码、状态机 |
| **HCM**  | 单工具能力清单（risk、schema、速率限制） |
| **HBP**  | 单板配置（启用 / 禁用哪些工具） |
| **HAF**  | 防篡改审计日志格式（行式 JSON + 哈希链） |
| **HMB**  | MCP 与 HACP 之间的桥接规则 |

---

## 📦 项目

<table>
  <tr>
    <td width="50%" valign="top">
      <h3>🟦 <a href="https://github.com/hearthworks/hearthd">hearthd</a></h3>
      <p>HACP server 的参考实现 —— 约 6.9 kLOC C11 单 binary，vendored
      依赖，AGPL-3.0。支持 GPIO/I²C/UART、sysfs、子进程沙箱；自动发现
      D-Bus / sysfs / CLI / IPMI 能力。</p>
      <p>
        <a href="https://github.com/hearthworks/hearthd"><img src="https://img.shields.io/github/stars/hearthworks/hearthd?style=flat-square" /></a>
        <a href="https://github.com/hearthworks/hearthd/issues"><img src="https://img.shields.io/github/issues/hearthworks/hearthd?style=flat-square" /></a>
        <img src="https://img.shields.io/github/last-commit/hearthworks/hearthd?style=flat-square" />
      </p>
    </td>
    <td width="50%" valign="top">
      <h3>🟧 <a href="https://github.com/hearthworks/ai-shell">ai-shell</a></h3>
      <p>HACP client 的参考实现 —— C11 CLI，既能把自然语言翻译成安全的
      shell，也能在 agent 模式下驱动 <code>hearthd</code>。支持 GitHub
      Copilot / OpenAI Codex / 本地 Ollama 三种后端，自带 per-model
      工具上限和两阶段 tool router。</p>
      <p>
        <a href="https://github.com/hearthworks/ai-shell"><img src="https://img.shields.io/github/stars/hearthworks/ai-shell?style=flat-square" /></a>
        <a href="https://github.com/hearthworks/ai-shell/issues"><img src="https://img.shields.io/github/issues/hearthworks/ai-shell?style=flat-square" /></a>
        <img src="https://img.shields.io/github/last-commit/hearthworks/ai-shell?style=flat-square" />
      </p>
    </td>
  </tr>
</table>

> 🛤️ **v0.2 路线图**：流式 `task.events`、TCP 上的 mTLS、rollback 语义、
> 更多板 profile（RK3588、BCM2712、LoongArch、ESP32-S3 / FreeRTOS）。
> 我们需要帮助 —— 见下方 *参与方式*。

---

## 🧭 快速开始

```bash
# 1. 启动 daemon
git clone https://github.com/hearthworks/hearthd.git
cd hearthd && cmake -B build && cmake --build build -j
sudo ./build/hearthd

# 2. 用参考 CLI 跟它对话
git clone https://github.com/hearthworks/ai-shell.git
cd ai-shell && make
./ai --agent --backend ollama --model qwen2.5-coder:7b "把状态灯闪一下"
```

完整的快速上手、单板 bring-up 笔记和协议速查表，都在每个 repo 的
`README.md` 与 `docs/` 里。

---

## 👥 维护者与贡献者

<table>
  <tr>
    <td align="center" width="160">
      <a href="https://github.com/ysun">
        <img src="https://github.com/ysun.png" width="80" style="border-radius:50%" /><br/>
        <sub><b>Yi Sun</b></sub>
      </a><br/>
      <sub>创始人 · 维护者<br/>HACP 作者</sub>
    </td>
    <!-- 新增维护者请按 <td> 块追加 -->
  </tr>
</table>

跨整个组织的实时贡献者头像墙：

[![Contributors](https://contrib.rocks/image?repo=hearthworks/hearthd)](https://github.com/hearthworks/hearthd/graphs/contributors)
[![Contributors](https://contrib.rocks/image?repo=hearthworks/ai-shell)](https://github.com/hearthworks/ai-shell/graphs/contributors)

---

## 🤝 参与方式

我们正在三个方向积极寻求帮助：

| 方向 | 为什么 | 怎么参与 |
|------|--------|----------|
| **新板 port** | 已支持 RPi 5 / Jetson / VisionFive 2 / 通用 x86。希望覆盖 RK35xx / 全志 / 龙芯 / 飞腾 / RISC-V SBC / 服务器 BMC。 | 在 `hearthd` 提一个标题为 `board: <SoC>` 的 issue，附上 `dmesg`。 |
| **真实落地场景** | 我们需要 pilot 用户，让 daemon 在工业网关、边缘 AI 盒子、AIoT 中枢等真实负载里被打磨。 | 提 Discussion 或邮件 `syqust@gmail.com`。 |
| **HACP review** | 协议目前是 IETF draft 00。来自 MCP / 机器人 / 设备管理领域的评审极其宝贵。 | 在 [I-D](https://datatracker.ietf.org/doc/draft-sunyi-hacp-protocol/) 留言，或开 issue。 |

新贡献者：每个 repo 的 `CONTRIBUTING.md` 都标注了 `good first issue`
标签，并要求 `git commit -s` 的 DCO 签名。代码是纯 C11 —— `make`、
`cmake`、一份 `gdb` 就够了。

---

## 💖 赞助方与资助渠道

HEARTH 是独立开发，正在寻找机构赞助方来资助新硬件 port、协议一致性
测试套件以及文档翻译。

<table>
  <tr>
    <!-- 第一个赞助 logo 槽位；填入后请删掉这一行 placeholder
         <td align="center"><a href="https://example.org"><img src="https://example.org/logo.svg" height="60" /></a></td>
    -->
    <td align="center" style="opacity:0.5">
      <em>这里可以是你的 logo<br/>—<br/>成为我们的第一个赞助方</em>
    </td>
  </tr>
</table>

### 支持本项目的几种方式

- ⭐ **Star** [hearthd](https://github.com/hearthworks/hearthd) 与
  [ai-shell](https://github.com/hearthworks/ai-shell) —— 真的能帮我们
  吸引到更多板子 porter
- 💵 **GitHub Sponsors**：
  [github.com/sponsors/ysun](https://github.com/sponsors/ysun)
  *（月度档位 $5 / $25 / $100 / 自定义）*
- ☕ **一次性**：[ko-fi.com/yisun](https://ko-fi.com/yisun)
  *（如不使用请删除或替换）*
- 🪙 **加密货币** *（可选）*：
  - BTC：`bc1q…` *（替换）*
  - ETH：`0x…`  *（替换）*
- 🏢 **商业许可 / 咨询**：邮件 `syqust@gmail.com`。
  HEARTH 采用 AGPL-3.0-or-later **+** 商业双授权；商业一侧的收入用于
  维持全职维护。

> 所有赞助收入都会再投入到板卡硬件、CI 资源以及协议一致性测试。每个
> repo 的 `FUNDING.md` 里有当前流水账。

---

## 📜 许可

`hearthworks/` 下所有项目均以 **AGPL-3.0-or-later** 开源；同时提供
**可选的商业许可**，用于无法接受 AGPL 网络使用条款的厂商场景。详见各
repo 的 `LICENSE` 与 `COMMERCIAL-LICENSE.md`。

HACP 协议本身已作为 IETF Internet-Draft 公开，目标是不被任何具体实现
绑定。

---

<div align="center">
<sub>
HEARTH Works · 创立于 2026 ·
<a href="https://github.com/hearthworks">github.com/hearthworks</a> ·
<a href="mailto:syqust@gmail.com">syqust@gmail.com</a>
</sub>
</div>
