<div align="center">

<!-- Banner / logo. Replace with hosted asset once you have one.
     Suggested size 1280×320, keep file under 300 KB. -->
<a href="https://github.com/hearthworks">
  <img src="https://raw.githubusercontent.com/hearthworks/.github/main/profile/banner.png"
       alt="HEARTH — Hardware Execution Layer for AI Agents"
       width="100%" />
</a>

# HEARTH Works

**A capability-contract layer between AI agents and edge hardware.**

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![Commercial license available](https://img.shields.io/badge/Commercial-license%20available-success.svg)](mailto:syqust@gmail.com)
[![IETF draft](https://img.shields.io/badge/IETF-draft--sunyi--hacp--protocol--00-orange.svg)](https://datatracker.ietf.org/doc/draft-sunyi-hacp-protocol/)
[![Made with C11](https://img.shields.io/badge/C-11-informational.svg)](https://en.wikipedia.org/wiki/C11_(C_standard_revision))

</div>

---

## 🔥 What is HEARTH?

LLM agents are getting good at *reasoning* about hardware — but they have no
common, safe way to actually *touch* it.  Today every agent re-invents its
own GPIO/I²C/UART glue, with shell-out hacks and zero audit trail.

**HEARTH** fixes that with three layers:

1. **HACP** — *Hearth Agent Capability Protocol*: a tiny JSON-RPC 2.0 wire
   protocol over `AF_UNIX` so any agent can ask "what can this box do?" and
   "please do these N steps".
   *Submitted to IETF as `draft-sunyi-hacp-protocol-00`.*
2. **hearthd** — a single-binary C11 daemon that owns the hardware, enforces
   8 layers of security, and writes a SHA-256–chained audit log.
3. **ai-shell** — a reference CLI agent that can drive `hearthd` directly
   or proxy to Copilot / Codex / Ollama.

Companion specs in the same family:

| Spec | What it pins down |
|------|-------------------|
| **HACP** | Wire protocol, six RPCs, error codes, state machines |
| **HCM**  | Per-tool capability manifests (risk, schema, rate limits) |
| **HBP**  | Per-board profiles (which tools to enable / disable) |
| **HAF**  | Tamper-evident audit log format (line-JSON + hash chain) |
| **HMB**  | Bridging rules between MCP and HACP |

---

## 📦 Projects

<table>
  <tr>
    <td width="50%" valign="top">
      <h3>🟦 <a href="https://github.com/hearthworks/hearthd">hearthd</a></h3>
      <p>The reference HACP server — a ~6.9 kLOC C11 daemon, single binary,
      vendored deps, AGPL-3.0.  Brings up GPIO/I²C/UART, sysfs, exec
      sandbox; auto-discovers D-Bus / sysfs / CLI / IPMI capabilities.</p>
      <p>
        <a href="https://github.com/hearthworks/hearthd"><img src="https://img.shields.io/github/stars/hearthworks/hearthd?style=flat-square" /></a>
        <a href="https://github.com/hearthworks/hearthd/issues"><img src="https://img.shields.io/github/issues/hearthworks/hearthd?style=flat-square" /></a>
        <img src="https://img.shields.io/github/last-commit/hearthworks/hearthd?style=flat-square" />
      </p>
    </td>
    <td width="50%" valign="top">
      <h3>🟧 <a href="https://github.com/hearthworks/ai-shell">ai-shell</a></h3>
      <p>The reference HACP client — a C11 CLI that translates natural
      language to safe shell or drives <code>hearthd</code> in agent mode.
      Supports GitHub Copilot, OpenAI Codex and local Ollama backends with
      per-model tool caps and two-stage routing.</p>
      <p>
        <a href="https://github.com/hearthworks/ai-shell"><img src="https://img.shields.io/github/stars/hearthworks/ai-shell?style=flat-square" /></a>
        <a href="https://github.com/hearthworks/ai-shell/issues"><img src="https://img.shields.io/github/issues/hearthworks/ai-shell?style=flat-square" /></a>
        <img src="https://img.shields.io/github/last-commit/hearthworks/ai-shell?style=flat-square" />
      </p>
    </td>
  </tr>
</table>

> 🛤️ **On the v0.2 roadmap**: streaming `task.events`, mTLS over TCP,
> rollback semantics, more board profiles (RK3588, BCM2712, LoongArch,
> ESP32-S3 / FreeRTOS).  Help wanted — see *Get involved* below.

---

## 🧭 Getting started

```bash
# 1. Run the daemon
git clone https://github.com/hearthworks/hearthd.git
cd hearthd && cmake -B build && cmake --build build -j
sudo ./build/hearthd

# 2. Talk to it from the reference CLI
git clone https://github.com/hearthworks/ai-shell.git
cd ai-shell && make
./ai --agent --backend ollama --model qwen2.5-coder:7b "blink the status LED"
```

Full quick-start, board bring-up notes and protocol cheat-sheet are in each
repo's `README.md` and `docs/`.

---

## 👥 Maintainers & contributors

<table>
  <tr>
    <td align="center" width="160">
      <a href="https://github.com/ysun">
        <img src="https://github.com/ysun.png" width="80" style="border-radius:50%" /><br/>
        <sub><b>Yi Sun</b></sub>
      </a><br/>
      <sub>Founder · maintainer<br/>HACP author</sub>
    </td>
    <!-- add new maintainers as <td> blocks -->
  </tr>
</table>

A live list of every contributor across the org auto-renders here:

[![Contributors](https://contrib.rocks/image?repo=hearthworks/hearthd)](https://github.com/hearthworks/hearthd/graphs/contributors)
[![Contributors](https://contrib.rocks/image?repo=hearthworks/ai-shell)](https://github.com/hearthworks/ai-shell/graphs/contributors)

---

## 🤝 Get involved

We are actively looking for help in three areas:

| What | Why | How |
|------|-----|-----|
| **Board ports** | We have RPi 5 / Jetson / VisionFive 2 / generic x86. We *want* RK35xx, AllWinner, LoongArch, RISC-V SBCs, and BMC environments. | Open an issue on `hearthd` titled `board: <SoC>` and link your `dmesg`. |
| **Real-world deployments** | We need pilot users to harden the daemon against live workloads (industrial gateways, edge AI boxes, AIoT hubs). | Open a discussion or email `syqust@gmail.com`. |
| **HACP review** | The protocol is at IETF draft 00. Reviews from MCP / robotics / device-management folks are gold. | Comment on the [I-D](https://datatracker.ietf.org/doc/draft-sunyi-hacp-protocol/) or open an issue. |

New contributors: every repo's `CONTRIBUTING.md` lists `good first issue`
labels and a small developer-certificate-of-origin sign-off rule
(`git commit -s`).  The codebase is plain C11 — `make`, `cmake` and a copy
of `gdb` are all you need.

---

## 📜 License

Every project under `hearthworks/` is released under
**AGPL-3.0-or-later** for open-source use, with an **optional commercial
license** for vendors that cannot accept the AGPL's network-use clause.
See each repo's `LICENSE` and `COMMERCIAL-LICENSE.md`.

The HACP protocol itself is published as an IETF Internet-Draft and is
intended to be implementation-unencumbered.

---

<div align="center">
<sub>
HEARTH Works · est. 2026 ·
<a href="https://github.com/hearthworks">github.com/hearthworks</a> ·
<a href="mailto:syqust@gmail.com">syqust@gmail.com</a>
</sub>
</div>
