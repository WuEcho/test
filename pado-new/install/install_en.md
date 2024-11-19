# Installing the Primus MPC-TLS SDK

Welcome to the first step in integrating Primus MPC-TLS SDK into your project! This guide will walk you through the installation process and help you get started quickly.

## Prerequisites

Before you begin, make sure you have:

- Node.js（version 18 or late）installed on your system
- npm (usually comes with Node.js) or yarn as your package manager

## Installation Steps

### 1.Install the SDK

Open your terminal and navigate to your project directory. Then run one of the following commands:

- Using npm:

```
npm install --save @padolabs/mpctls-js-sdk
```

- Using yarn:

```
yarn add --save @padolabs/mpctls-js-sdk
```

This command will download and install the Primus MPC-TLS SDK and its dependencies into your project.


### 2.Verify Installation

To ensure the SDK was installed correctly, you can check your `package.json` file. You should see `@padolabs/mpctls-js-sdk` listed in the `dependencies` section.

## Importing the SDK

After installation, you can import the SDK in your JavaScript or TypeScript files. Here's how:

```
import MPCTLSJSSDK from "@padolabs/mpctls-js-sdk";
```

## Next Steps

Congratulations! You've successfully installed the Primus MPC-TLS SDK. Here's what you can do next:

1. Check out the Frontend Example (React) guide to create your first proof request.
2. Learn about Advanced Configuration options to customize the SDK for your needs.

If you have further needs for other blockchains, please contact us through our [community](https://discord.gg/AYGSqCkZTz) for support.

