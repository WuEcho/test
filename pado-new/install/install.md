# 安装Primus MPC-TLS SDK

欢迎将 Primus MPC-TLS SDK 集成到您的 JavaScript 项目中！本指南将引导您完成安装过程并帮助您快速入门。

## 先决条件

开始之前，请确保您已：

- 系统上安装了 Node.js（版本 18 或更高版本）
- npm（通常随 Node.js 提供）或 yarn 作为包管理器



## 安装步骤

### 1. 安装 SDK

打开终端并导航到项目目录。然后运行以下命令之一：

- 使用 npm：

```
npm install --save @padolabs/mpctls-js-sdk
```

- 使用yarn:

```
yarn add --save @padolabs/mpctls-js-sdk
```

此命令将下载并安装 Primus MPC-TLS SDK 及其依赖项到您的项目中。

### 2. 验证安装

为了确保 SDK 已正确安装，您可以检查`package.json`文件。您应该看到`@padolabs/mpctls-js-sdk`本节中列出的内容`dependencies`。

## 导入 SDK

安装后，您可以在 JavaScript 或 TypeScript 文件中导入 SDK。操作方法如下：

```
import MPCTLSJSSDK from "@padolabs/mpctls-js-sdk";
```

## 下一步

恭喜！您已成功安装 Primus MPC-TLS SDK。接下来您可以执行以下操作：

1.查看 前端示例 指南来创建您的第一个证明请求。
2.了解 高级配置 选择项，以便根据您的需要定制 SDK。

如果您在安装过程中遇到任何问题，请联系我们的[社区](https://discord.gg/AYGSqCkZTz)以获得进一步支持。

