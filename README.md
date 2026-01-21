# 🚀 Maxun + CapSolver: 终极无代码网络爬虫集成方案 

[![GitHub stars](https://img.shields.io/github/stars/capsolver/capsolver-maxun?style=for-the-badge)](https://github.com/capsolver/capsolver-maxun/stargazers)
[![License](https://img.shields.io/github/license/capsolver/capsolver-maxun?style=for-the-badge)](LICENSE)
[![CapSolver](https://img.shields.io/badge/Powered%20By-CapSolver-orange?style=for-the-badge)](https://www.capsolver.com/?utm_source=github&utm_medium=repo&utm_campaign=maxun)
[![Maxun](https://img.shields.io/badge/Platform-Maxun-blue?style=for-the-badge)](https://github.com/getmaxun/maxun)

> **让不可能的自动化成为可能。** 使用 CapSolver 的 AI 驱动的验证码解决引擎，在您的 Maxun 无代码爬取工作流中无缝绕过验证码。

---

## 📖 概览

在网络数据提取领域，**[Maxun](https://github.com/getmaxun/maxun)** 是一个颠覆者——一个开源、无代码的平台，允许您以可视化方式训练爬取机器人。然而，最大的障碍仍然是：**验证码 (CAPTCHA)**。

本仓库提供了一个生产就绪的 **Maxun** 与 **[CapSolver](https://www.capsolver.com/?utm_source=github&utm_medium=repo&utm_campaign=maxun)** 的集成方案。通过结合 Maxun 的易用性与 CapSolver 基础设施级别的验证码解决能力，您可以构建可靠、不间断的数据管道。

### ✨ 核心特性

- 🤖 **无代码的简洁性**：利用 Maxun 的可视化构建器完成复杂的爬取任务。
- 🧠 **AI 驱动的解决**：通过 CapSolver 绕过 reCAPTCHA (v2/v3)、Cloudflare Turnstile、AWS WAF 等多种验证码。
- 🛠️ **开发者友好**：包含一个健壮的 TypeScript/Node.js SDK 集成示例。
- ⚡ **高性能**：优化了轮询和并行执行模式。
- 🔒 **会话管理**：处理预认证和基于 Cookie 的绕过。

---

## 🚀 快速开始

### 1. 前置条件

- **Node.js**：v18 或更高版本
- **Maxun**：自托管或 [Maxun 云服务](https://app.maxun.dev/)
- **CapSolver API 密钥**：从 [CapSolver 控制台](https://dashboard.capsolver.com/passport/login?redirect=/dashboard/?utm_source=github&utm_medium=repo&utm_campaign=maxun) 获取

### 2. 安装

```bash
# 克隆集成示例
git clone https://github.com/capsolver/capsolver-maxun.git
cd capsolver-maxun

# 安装依赖
npm install
```

### 3. 环境配置

在项目根目录创建 `.env` 文件：

```env
CAPSOLVER_API_KEY=your_capsolver_api_key # 您的 CapSolver API 密钥
MAXUN_API_KEY=your_maxun_api_key         # 您的 Maxun API 密钥
MAXUN_BASE_URL=https://app.maxun.dev/api/sdk # Maxun API 基础 URL (云服务或本地实例)
```

---

## 🛠️ 使用示例

### CapSolver 服务类

我们提供了一个可复用的 `CapSolverService` 类来处理验证码解决的异步特性。

```typescript
import { CapSolverService } from './src/services/capsolver';

const capSolver = new CapSolverService({ 
  apiKey: process.env.CAPSOLVER_API_KEY! 
});

// 解决 reCAPTCHA v2
const token = await capSolver.solveReCaptchaV2(targetUrl, siteKey);
```

### 集成模式：预认证

由于 Maxun 运行在较高的抽象层级，最佳实践是在运行机器人之前解决验证码并建立会话。

```typescript
// 1. 解决验证码
const token = await capSolver.solveReCaptchaV2(loginUrl, siteKey);

// 2. 获取会话 Cookie (示例)
const response = await axios.post(loginUrl, { 'g-recaptcha-response': token });
const cookies = extractCookies(response);

// 3. 使用 Cookie 运行 Maxun 机器人
const robot = await extractor
  .create('Authenticated Robot')
  .setCookies(cookies) // 传入认证后的 Cookie
  .navigate(targetUrl)
  .run();
```

> [!TIP]
> 请查看 `examples/` 目录，获取 Crawl、Scrape 和 Extract 模式的完整实现。

---

## 📊 支持的验证码类型

| 类型 | CapSolver 产品 | 状态 |
| :--- | :--- | :--- |
| **reCAPTCHA v2/v3** | [查看产品](https://www.capsolver.com/products/recaptchav2?utm_source=github&utm_medium=repo&utm_campaign=maxun) | ✅ 支持 |
| **Cloudflare Turnstile** | [查看产品](https://www.capsolver.com/products/cloudflare?utm_source=github&utm_medium=repo&utm_campaign=maxun) | ✅ 支持 |
| **AWS WAF CAPTCHA** | [查看产品](https://www.capsolver.com/products/awswaf?utm_source=github&utm_medium=repo&utm_campaign=maxun) | ✅ 支持 |
| **GeeTest v3/v4** | [查看产品](https://www.capsolver.com/products/geetest?utm_source=github&utm_medium=repo&utm_campaign=maxun) | ✅ 支持 |

---

## 💡 最佳实践

1. **错误处理**：对于网络相关的故障，始终实现带指数退避的重试机制。
2. **余额监控**：使用 `capSolver.checkBalance()` 确保您的自动化任务不会因余额不足而失败。
3. **Token 缓存**：验证码 Token 通常在 90-120 秒内有效。缓存它们可以避免重复的 API 调用。

---

## 🎁 特别优惠

准备好扩展您的爬取规模了吗？[注册 CapSolver](https://www.capsolver.com/?utm_source=github&utm_medium=repo&utm_campaign=maxun) 并使用优惠码 **MAXUN**，首次充值可额外获得 **6% 奖励**！

---

## 📄 许可证

本项目采用 MIT 许可证分发。详情请参阅 `LICENSE` 文件。

## 🤝 贡献

贡献使开源社区成为一个学习、启发和创造的绝佳场所。我们鼓励并珍视任何形式的贡献。

1. Fork 本项目
2. 创建您的特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交您的更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启一个 Pull Request

---

<p align="center">
  由 <a href="https://www.capsolver.com/">CapSolver 团队</a> 用 ❤️ 构建
</p>
