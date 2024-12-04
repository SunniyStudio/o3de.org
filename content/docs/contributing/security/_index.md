---
linktitle: 安全
title: "为 O3DE 安全做出贡献"
description: 了解如何确保 Open 3D Engine （O3DE） 的安全。
toc: true
weight: 300
---

## Open 3D Engine: 安全
本页介绍了 Open 3D Engine （O3DE） 的安全和披露政策。

O3DE 重视所有有助于确保 O3DE 安全的贡献。[SIG-安全](https://github.com/o3de/sig-security) 志愿者将彻底研究和调查所有报告的漏洞。

# 漏洞报告方法
我们鼓励通过 [security@o3de.org](mailto:security@o3de.org)  邮件列表报告所有漏洞。漏洞报告应遵循与 [标准 O3DE 问题](https://github.com/o3de/o3de/blob/development/.github/ISSUE_TEMPLATE/bug_template.md) 相同的信息报告格式，以及任何特定于安全的详细信息。

## 我应该何时报告漏洞？
* 您发现了与 O3DE 相关的潜在安全问题。
* 您已发现或意识到与 O3DE 依赖项相关的安全问题，该问题可能会影响 O3DE。
     * 如果问题存在于 O3DE 所依赖的产品中，我们强烈建议您也参与该项目的漏洞报告机制。

# 什么时候不应该报告漏洞？
* 您需要帮助设置安全依赖项（如 OpenSSL 或 TLS/DTLS）或遇到与使用相关的问题。
* 您需要帮助将安全相关修复程序应用于 O3DE 应用程序。
* 此问题与安全性无关。请使用正常的 [O3DE 问题报告流程报告问题](https://github.com/o3de/o3de/issues).

## 我可以通过 GitHub Issues 报告漏洞吗？
如果有公开披露的安全信息，例如 [国家漏洞数据库](https://nvd.nist.gov/)中的报告，则可以使用 GitHub issues 来传达影响 O3DE 的安全漏洞。

# 安全漏洞响应
当通过 [安全邮件清单](mailto:security@o3de.org)报告安全漏洞时，SIG-Security 响应团队的成员应在 3 个工作日内做出回应。一旦确认问题，SIG-Security 响应团队将遵循其 [记录的响应流程](https://github.com/o3de/sig-security/issues/14)，以确保报告者了解最新情况并告知当前操作和后续步骤。

任何漏洞信息都将被视为机密信息，不会在安全响应流程之外共享。未经报告者同意，不会公开共享或披露漏洞信息。

# 公开披露政策
任何安全漏洞的公开披露日期将由 SIG-Security 和漏洞报告者协商。SIG 旨在尽快公开披露问题，最好是在确定缓解措施后。披露时间可以从几乎立即披露（对于公开已知的问题）到几周不等，具体取决于问题的复杂程度和所需的协调。
