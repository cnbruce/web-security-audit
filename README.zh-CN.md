# Web Security Audit Skill（web-security-audit）

通用 Web 应用安全审计技能：**白盒（源码）找因 + 黑盒（线上行为）验果 + 负面行为测试三件套**，输出分级风险报告与可直接粘贴的修复代码。

> 本技能遵循 [Agent Skills 开放标准](https://agentskills.io/specification)（Anthropic 发起），兼容 Claude Code、WorkBuddy、Codex CLI、Gemini CLI、Cursor、VS Code、GitHub Copilot 等平台。

## 特性

- **白盒 + 黑盒结合**：静态核查漏洞成因（SQLi / XSS / 认证 / 越权 / CSRF / 上传 / 路径遍历 / SSRF / 信息泄露 / 依赖），黑盒验证攻击者可观察的行为（状态码 / 响应体 / 兜底 / 敏感文件可达性），两者交叉印证。
- **负面行为测试三件套**（核心，纯静态审计最易漏的部分）：
  1. 公开接口畸形输入 → 断言返回 400/422 而非 500；
  2. 敏感路径清单 + `content-type` 校验 → 区分 SPA 兜底假象与真实文件泄露；
  3. 未知路径 404 语义 → SPA 兜底只应命中无扩展名前端路由。
- **可执行探测脚本**：`scripts/probe.sh <url>` 一键完成三件套。
- **安全边界内置**：仅限授权目标、全程非破坏性（无真实注入、无爆破、不触碰数据）。

## 安装

| 平台 | 方式 |
|---|---|
| Claude Code | `mkdir -p ~/.claude/skills && unzip web-security-audit-claude.zip -d ~/.claude/skills/` |
| WorkBuddy | 放入 `~/.workbuddy/skills/` |
| 其他兼容平台 | 解压 zip 到对应 skills 目录 |
| 本地开发 | 本项目目录即技能本体，`scripts/probe.sh` 可独立运行 |

## 使用

```bash
# 1) 负面行为测试（畸形输入 / 敏感路径 / 未知路径）
./scripts/probe.sh https://target.example.com

# 2) 完整审计流程（由 AI 执行）
#    摸底 → 白盒逐类核查 → 黑盒验果 → 负面测试三件套 → 交叉验证与报告
```

审计输出：风险分级表（Critical / High / Medium / Low / Info），每项附「现象 / 影响 / 修复代码 / 验证方法」，并直接回答"有没有后门""会不会被爆库"等核心问题。

## 目录结构

```
web-security-audit/
├── SKILL.md                    # 技能定义：方法论 + 安全边界 + 5 步审计流程
├── references/checklist.md     # 10 类白盒核查项 + 三件套命令与判定规则 + 报告模板 + 常见陷阱
├── scripts/probe.sh            # 负面行为测试探测脚本（非破坏性）
├── README.md                   # 英文说明
└── README.zh-CN.md             # 本文档
```

## 安全声明

本技能仅用于**授权**的安全评估（自有项目或已获书面授权）。所有内置探测均为无害验证性请求；禁止用于未授权目标。

## License

MIT
